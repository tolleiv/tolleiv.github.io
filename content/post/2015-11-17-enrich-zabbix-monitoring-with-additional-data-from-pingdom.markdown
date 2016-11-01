---
categories: null
comments: true
date: 2015-11-17T08:43:33Z
title: Enrich Zabbix monitoring with additional data from Pingdom
---

If you’re running your own monitoring system which has to observe services for global customers, you might have considered to spin up some monitoring slaves around the world to gather accurate availability data. Unfortunately bringing up these slaves is time consuming and expensive to some extend. On the other hand services like Pingdom provide global monitoring for low budget. But using these splits your monitoring infrastructure into two parts and brings drawbacks when it comes to reporting and alerting. This blog post explains how to combine both, an internal monitoring system like Zabbix and external monitoring data from Pingdom in one place, to gather global availability data and keep alerting and event handling centralised.

## Prerequisites:

Before data can be pulled from one service to the other both have to be working on it’s own. Therefore I assume that your webservice/website is running on one or more Linux* hosts which are already monitored with Zabbix [[2]](#zabbix). You should have access to the Zabbix server and the hosts. Along with that I also assume that you’ve access to Pingdom and it’s API [[3]](#pingdom), you should have a couple of checks configured already [[4]](#pingdomcfg). Furthermore I assume that you’re able to work on the Linux command line.
In addition to all of that, I suggest that you open up a second browser tab with the Gist [[1]](#gist) which holds all prepared scripts and configuration.

## Understanding the data flow:
The first thing to dive into is the high level data flow. As described initially, data should be pulled from Pingdom through the API and pushed into Zabbix. Pulling the data from Pingdom is easy with a simple curl request against `https://api.pingdom.com/api/2.0/checks` which would then return the most recent monitoring data as JSON. When the data is available, the list has to be limited to the relevant information and mapped into a format which can be transferred to Zabbix. The `zabbix_sender` utility will then be used to actually push the data to Zabbix. Once this is done, Zabbix has to be prepared to receive the data within related monitoring items and triggers which need to be configured for alerting. But first things first ...

## Mapping and filtering data for Zabbix:
Pulling data from Pingdom with the `/checks` endpoint (see [[5]](#pingdomapi) for full reference) might give you much more than you need, therefore I’d suggest that you add some tags to the Pingdom checks which you want to pull into Zabbix and then just fetch these with `/checks?tag=zabbix`. If you’re using Zabbix proxy systems along with the Zabbix server, you should then use one tag per proxy, so that each proxy is able to pull just their related checks**.

You will get a response which looks like this:

```
{ "checks": [
    {
      "hostname": "example.com",
      "id": 85975,
      "lasterrortime": 1297446423,
      "lastresponsetime": 355,
      "lasttesttime": 1300977363,
      "name": "example.com homepage",
      "resolution": 1,
      "status": "up",
      "type": "http",
      "tags": [{
        "name": "zabbix",
        "type": "au",
        "count": 2
      }]
     }
]}
```

Now that you’re able to retrieve data, it has to be filtered and mapped to a format which zabbix_sender understands. This is done with the following steps:

- Splitting up the long checks array into multiple separated objects
- Filtering out the relevant fields from each object
- Transforming the timestamps from integer to string representations
- Mapping the status field into an integer - this will make it easier to define triggers in Zabbix later on
- Combining all fields of the object into an array with two event data strings
- Splitting up the array to have one zabbix_sender event per line
- Adding in some text block which relates to the Zabbix item key to have a fully defined zabbix_sender event

All this is done with this small snippet***:

```
echo "$json" \
  | jq -r '.checks[]' \
  | jq -r '{time:.lasttesttime, key:.name ,status:.status, responsetime:.lastresponsetime}' \
  | jq -r '.time=(.time | tostring)' \
  | jq -r '.responsetime=(.responsetime | tostring)' \
  | jq -r 'def mapping: {"up":"2","paused":"1","down":"0"};.status=mapping[.status]' \
  | jq -r '[.key + ",status] " + .time + " " + .status, .key + ",time] " + .time + " " + .responsetime]' \
  | jq -r '.[]' \
  | sed -e 's~ ~ pingdom.response[~1'
```

As you might have guessed, the tool which is used mainly ‘jq’ is a powerful json processing tool which by itself is worth a long article. You’ll find the manual here [[6]](#jq) and a great introduction within the well known '7 command-line tools for data science’ article [[7]](#dscl).

After the filtering is done, the result looks like this:

```
example.com pingdom.response[homepage,status] 1300977363 2
example.com pingdom.response[homepage,time] 1300977363 355
```

You see that this will report data for two Zabbix items with the keys `pingdom.response[homepage,status]` and `pingdom.response[homepage,time]`. If you’re just interested in the state but not the timings you could simplify the filtering command by 2 or 3 lines and have a much more straight forward flow.

## Zabbix check discovery:
Once the script is ready and able to send valid data to Zabbix, you've to make sure that Zabbix also associates it to a host and the related items. So you've to bring up a template and configure the trapper items [8]. But in case you want to have more than one Pingdom check per host you run into issues with that straight forward approach. You’d either have to configure a new item by hand for every new Pingdom check or let the low level discovery (LLD) of Zabbix do that for you. With the LLD approach in place, Zabbix will be able to detect new checks on his own and create the related items or triggers automatically.

The Zabbix low level discovery needs some input to know which checks exists. This is done with a JSON file on the related host:

```
{
  "data":[
    { "{#NAME}":"homepage", "{#URL}":"http://example.org/" },
    { "{#NAME}":"newsletterform", "{#URL}":"http://example.org/newsletter.html" }
  ]
}
```

Generating this JSON with a similar processing chain like above isn’t really hard and it helps Zabbix to pick up the data needed for low level discovery. Once the JSON file is in place it can be picked up with the Zabbix agent and the `vfs.file.contents[/etc/zabbix/pingdom_lld.json,UTF8]` discovery configuration. In order to retrieve data from the discovered checks, you’ll have to bring up some discovery items. As described before these should be Zabbix trapper items with the keys `pingdom.response[{#NAME},status]` and `pingdom.response[{#NAME},time]`.

You’ll find a prepared Zabbix Template XML within my Gist [[9]](#gistfile).

## Distribution when working with Zabbix-Proxy setups:
The steps described so far will give you nice results if you’re working with a single central Zabbix server. They might not work if you run your Zabbix installation with multiple Zabbix proxies instead. The reason for this is that the zabbix_sender is also supposed to send the data to the Zabbix proxy which was configured for the monitored host. Therefore the script which pulls the data from Pingdom should run on each Zabbix proxy and just pull the data for the relevant hosts. To avoid that each proxy pulls all the data and then fails to send it to Zabbix the call to the Pingdom API should be limited. This can be done with a slight modification to the API call. Instead of having a single tag for all checks which relate to Zabbix, checks should be tagged related to their proxy e.g. with `/checks?tag=zabbix_dc1` vs. `/checks?tag=zabbix_dc2`.

## Putting it all together:
Once all your script and configuration is in place you’ll receive Pingdom monitoring data within Zabbix and you’ll be able to configure triggers and relate other monitoring data to it.

In case you use some sort of provisioning tool (Puppet, Check, Cfengine, Ansible, Salt, …) you should also consider to move the definition of the Pingdom checks into that tool and roll out all the configuration from a central spot.

Last but not least, it would be great if you’d leave a comment and whether you think this approach is of any use for you or not. In case you use a similar approach with other vendors like Sentry [[10]](#sentry) or Port-Monitor [[11]](#portmonitor) I’d love to see how you pull the data in. Also feel free to leave questions in the comments below, share the link on Twitter or Facebook or Flattr it.

----
### Remarks & references:
\* the introduced setup is not limited to Linux hosts, but so far the related Zabbix Template is not ready to be used with Windows - but that should not be a big problem as long as you have your Zabbix Proxy/Server running on Linux.

** this comes from a limitation in the zabbix_sender with binds the sender to the Zabbix proxy

*** readable simplifications welcome

- <a name="gist">[ 1] - [gist.github.com/tolleiv/2f49a7711e3040f2b25b](https://gist.github.com/tolleiv/2f49a7711e3040f2b25b)</a>
- <a name="zabbix">[ 2] - [www.zabbix.com/](http://www.zabbix.com/)</a>
- <a name="pingdom">[ 3] - [pingdom.com/](http://pingdom.com/)</a>
- <a name="pingdomcfg">[ 4] - [www.pingdom.com/resources/tutorials/how-to-add-check](https://www.pingdom.com/resources/tutorials/how-to-add-check)</a>
- <a name="pingdomapi">[ 5] - [www.pingdom.com/resources/api#MethodGet+Check+List](https://www.pingdom.com/resources/api#MethodGet+Check+List)</a>
- <a name="jq">[ 6] - [stedolan.github.io/jq/manual/](https://stedolan.github.io/jq/manual/)</a>
- <a name="dscl">[ 7] - [jeroenjanssens.com/2013/09/19/seven-command-line-tools-for-data-science.html](http://jeroenjanssens.com/2013/09/19/seven-command-line-tools-for-data-science.html)</a>
- <a name="zabbixtrapper">[ 8] - [www.zabbix.com/documentation/2.4/manual/config/items/itemtypes/trapper](https://www.zabbix.com/documentation/2.4/manual/config/items/itemtypes/trapper)</a>
- <a name="gistfile">[ 9] - [gist.github.com/tolleiv/2f49a7711e3040f2b25b#file-zbx_template-xml](https://gist.github.com/tolleiv/2f49a7711e3040f2b25b#file-zbx_template-xml)</a>
- <a name="sentry">[10] - [getsentry.com](https://getsentry.com)</a>
- <a name="portmonitor">[11] - [www.port-monitor.com/](https://www.port-monitor.com/)</a>
