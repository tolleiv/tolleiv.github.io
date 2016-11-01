---
categories:
- devops
- chef
comments: true
date: 2014-07-06T15:32:04Z
title: Chef cookbook dependecy visualization
---

<div>
<img src="/uploads/2014/07/smallgraph.png" align="right" markdown="1" />
</div>

In my [previous post](http://blog.tolleiv.de/2014/05/chef-qa-toolchain/) I've pointed out some tools which make your live with [Chef](http://www.getchef.com/chef/) much easier. Among these, [Berkshelf](http://berkshelf.com/) helps with managing cookbooks and their dependencies. Depending on your workflows these  can be very straight forward or grow very complex. 

I had some free time and thought that visualizing these dependencies as graphs could be a fun weekend project. You can test out the results on [berksgraph.tolleiv.de](http://berksgraph.tolleiv.de/) yourself. Just upload your Berksfile.lock and you'll see the result.

### Building blocks

Esentially I've used [Node.Js transform streams](http://nodejs.org/api/stream.html) [[1]](http://nicolashery.com/parse-data-files-using-nodejs-streams/) [[2]](http://strongloop.com/strongblog/practical-examples-of-the-new-node-js-streams-api/) to parse the Berksfile.lock and generate [D3](http://d3js.org/) graph data. The graph data is transformed into a nice graph with [cola.js](http://marvl.infotech.monash.edu/webcola/). In order to provide a nice interface for the application, everything was wrapped into an [Express](http://expressjs.com/) application which uploads the graph data to [Amazon S3](https://github.com/tolleiv/berksfile-graphs/blob/master/lib/S3UploadStream.js). The actual hosting is done on one [Heroku](https://devcenter.heroku.com/articles/getting-started-with-nodejs) dyno.

### Examples

* [Very simple graph](http://berksgraph.tolleiv.de/grph?1404657586462#2014-07-06-fe9d7192c4c36da5279226d5d82ef3f93128a48e.js)
* [Graph with moderate complexity](http://berksgraph.tolleiv.de/grph?1404656042676#2014-07-06-04981e1185370fecf939ada2ef87ffda374c5e17.js)
* [Complex graph](http://berksgraph.tolleiv.de/grph?1404656118013#2014-07-06-51192e0f4bb8bfd2aab15551d4b31241eb295d18.js)





