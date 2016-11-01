---
categories:
- devops
- chef
comments: true
date: 2014-05-24T00:35:09Z
title: Chef QA toolchain
---

In the past years I've jumped into the role of the DevOps evangelist at AOE. As it seems this was quite successful and people joined in quickly. But due to the fact that DevOps for us also means that less trained colleges should participate in most parts of the codified infrastructure, it requires some quality assurance efforts in the background. An overview of the tools which help us to keep things clean, is what I wanted to share in this post.

### Tl;dr

* [Chef](http://www.getchef.com/chef/) - Great provisioning tool
* [Berkshelf](http://berkshelf.com/) - Easy dependency management
* [Foodcritic](http://acrmp.github.io/foodcritic/) - Chef specific lint
* [Rubocop](http://batsov.com/rubocop/) - Generic Ruby lint with autocorrect
* [Test-Kitchen](https://github.com/test-kitchen) - Real world test automation 
* [Serverspec](http://serverspec.org/) - Intuitive good documented functional test library


### Provisioning tools

The first part in the toolchain is [Chef](http://www.getchef.com/chef/) itself. We choose it over [Puppet](http://puppetlabs.com/) because it seemed to have a lower entry barrier and involved developers much better than the strict DSL of Puppet. We had some knowledge in our team for both tools, but in the end people sticked to Chef even on their private servers and that made the choice easy.

### Dependency management

Nowadays we're using [Berkshelf](http://berkshelf.com/) to maintain dependencies within our cookbooks and for our cookbook packages. The main reason to choose it over [Librarian](https://github.com/applicationsonline/librarian-chef), which we used before, was that it is failing with more meaningful errors. Besides that it turned out to be more robust when it comes to circular dependencies between cookbooks. Both features are essential when working with beginners.

### Lint tools

Once our team started to write cookbooks on their own, it turned out quickly that the understanding of Chef, the awareness of best practices and a unique coding style is hard to achieve without tools. And that's where the following Ruby tools can help out a lot.

The first tool which should be used by everyone who typed more than two lines of Chef code is [Foodcritic](http://acrmp.github.io/foodcritic/). It is analizing the cookbooks and responds with very cool and well documented error messages and code improvement suggestions. Along with them it guides every user to stick to Chef best practices.

The second tool is [Rubocop](http://batsov.com/rubocop/), a well known Ruby static code analizer which enforces the [Ruby Style Guide](https://github.com/bbatsov/ruby-style-guide). One of the most powerful features is the autocorrect option which helps to get rid of trivial problems. That's especially important for developers who don't use Ruby all day and who hate to think about *single vs. double quotes* problems. Rubocop handles such problems automatically and keeps the code base in a consistent state.

An alternative to Rubocop would be [Cane](https://github.com/square/cane) - but that comes without the autocorrect feature. Further Ruby QA tools like [flay](https://github.com/seattlerb/flay), [reek](https://github.com/troessner/reek) or [flog](https://github.com/seattlerb/flay) didn't really apply to our Chef recipes or brought up too much false positives. This just created too much confusion and brought in too little value.

### Testing

There's a large amount of tools which you could use to test recipes. [ChefSpec](https://github.com/sethvargo/chefspec) is the way to go when it comes to plain unit tests. It runs everything in Chef solo without actually converging anything on the system. Its speed is the biggest benefit and it is also the only way to properly test exception handling and broken paths in your recipes. Nowadays there's no way around ChefSpec when you look into the official OpsCode cookbooks. It is covered in lot's of blog posts as the entry point to cookbook testing ([Unit testing Chef cookbooks](https://sethvargo.com/unit-testing-chef-cookbooks/), [Starting ChefSpec Example](http://jtimberman.housepub.org/blog/2013/05/09/starting-chefspec-example/)).

If you're interested to run functional or integration tests, you could use [Bats](https://github.com/sstephenson/bats), [Serverspec](http://serverspec.org/), [Minitest](http://docs.seattlerb.org/minitest/), [Rspec](https://github.com/calavera/rspec-chef), [Cucumber](https://github.com/Atalanta/cucumber-chef), [shUnit2](https://code.google.com/p/shunit2/) and even plain [Bash](https://github.com/test-kitchen/busser-bash) scripts. All of the tools depend on a preceding converge step before the actual tests could check the results. That's why you usually want to run the tests in a fixed virtualized environment. That's provided with [Test-Kitchen](https://github.com/test-kitchen). Every Test-Kitchen suite is pulled up as seperate environment with it's own runlist and can then be tested fully isolated.

For our recipe QA my favorite functional testing tool is Serverspec. It provides an intuitive test structure (opposed to MiniTest) and comes with a long list of well documented [predefined resources](http://serverspec.org/resource_types.html). But to encourage everyone to test as much as possible, I wouldn't really limit the toolchain to a single tool, instead I brought up example implementations for most of them into our internal cookbooks. This way everyone could pick the tool they liked.

Good public examples for these testing libraries can be found on Github in various public cookbooks:

* ChefSpec: [1](https://github.com/tas50/nagios/tree/master/spec), [2](https://github.com/opscode-cookbooks/nginx/tree/master/spec), [3](https://github.com/opscode-cookbooks/chef-splunk), [4](https://github.com/opscode-cookbooks/yum/tree/master/spec/unit), [5](https://github.com/opscode-cookbooks/rsyslog/tree/master/spec)
* Serverspec: [1](https://github.com/opscode-cookbooks/mysql/tree/master/test/integration), [2](https://github.com/serverspec/specinfra/tree/master/spec), [3](https://github.com/phlipper/chef-postgresql/tree/master/test/integration/server/serverspec), [4](https://github.com/phlipper/chef-monit/tree/master/test/integration)
* Minitest: [1](https://github.com/opscode-cookbooks/ark/tree/master/files/default/tests/minitest), [2](https://github.com/opscode-cookbooks/varnish/tree/master/files/default/test), [3](https://github.com/elasticsearch/cookbook-elasticsearch/tree/master/tests), [4](https://github.com/bflad/chef-docker/tree/master/test/cookbooks/docker_test/files/default/tests/minitest), [5](https://github.com/ganglia/chef-ganglia/tree/master/test/cookbooks/ganglia_test/files/default/tests/minitest)
* Bats: [1](https://github.com/opscode-cookbooks/lvm/tree/master/test/integration/create/bats), [2](https://github.com/opscode-cookbooks/rsync/tree/master/test/integration), [3](https://github.com/fnichol/chef-rbenv/tree/master/test/integration/system_ruby/bats)
* Cucumber [1](http://www.jedi.be/blog/2011/03/29/vagrant-testing-testing-one-two/)

### Documentation

Last but not least, every cookbook should have a nice README and that could be rendered with [redcarpet](https://rubygems.org/gems/redcarpet) or if you're supporting more than just Markdown you could just throw [github/markup](https://github.com/github/markup) at it.

Alternatively you could of course just host your cookbooks on Github, Bitbucket or on a self-hosted Gitlab. Then these would take over rendering.

### Fin

The above `Tl,dr` section lined out my favorite tools. To sum up this post, I'd say that automation was essential for our setup. Each tool which I brought up in the post can be used for our internal recipes and all off them will be triggered by our CI server. With that toolchain at hand we were able to have fully open cookbooks were everyone can contribute easily. Besides that the tools lower the barrier for people who are new to Chef and they help the more integrated people to support others efficiently.