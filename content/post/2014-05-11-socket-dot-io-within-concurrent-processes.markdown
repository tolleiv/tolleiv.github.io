---
categories: null
comments: true
date: 2014-05-11T15:43:53Z
title: Socket.IO within concurrent processes
---

The following is going to show some pitfalls and their solution when working with NodeJs, Socket.IO and concurrent processes.

### Basic setup

#### Socket connections
In order to use socket communication with NodeJS a basic server side code block might look this [see online docs](http://socket.io/):

```
var io = require('socket.io').listen(80);
io.sockets.on('connection', function (socket) {
  socket.emit('news', { hello: 'world' });
});
```
This enables you to send some data through the socket connection to the client and with just a little more code it could even retrieve some data from the client. I left that part out to keep it simple.

#### Event emitters
The second component which I've to add in the mix is a simple [EventEmitter](http://nodejs.org/api/events.html). It's build like this:

```
var emitter = new (require('events').EventEmitter);
 // listening to a given event:
emitter.on("news", function (data) { console.log(data); });
 // actually trigger the event:
emitter.emit("news", {my: data})
```
The emitter would be used to gather data from within the program and the listeners would trigger ```socket.emit(...)``` in case the event is relevant for the connected client. This way the program could be decoupled from the socket implementation and other listeners could use the same event data without additional event (a nice way to introduce logging).

#### Concurrent processes

Besides all the event logic, the application should run in a robust way and that's typically done with the [cluster module](http://nodejs.org/api/cluster.html) like this:

```
var cluster = require('cluster');
if (cluster.isMaster) {
    var cpuCount = require('os').cpus().length;
    for (var i = 0; i < cpuCount; i += 1) {
        cluster.fork();
    }
    cluster.on('exit', function(worker) {
        cluster.fork();
    });
} else {
    // Your actual code come here
}
```
That's now the application entry point, it creates worker processes for the actual processing. In case a worker dies our application would keep running and a new worker process would come up.


### Mixing the incredients

Throwing all this into the mix creates some problems. First of all, the worker processes use their own memory, so everything which is sent to the EventEmitter in one process can't reach the others.

The second problem occurs when Socket.IO is included in the concurrent setup. Due to the fact that the worker processes are assign randomly to the clients, your actual work (e.g. triggered by HTTP requests) might been done in one process, while your socket connection might be established to another process. If you'd use the basic setup from above, your events would then never reach the right clients and therefore you'd loose information.

### Redis store helping out

The solution is to replace the in-memory store of Socket.IO with a Redis based store (see [Adam N England](http://adamnengland.wordpress.com/2013/01/30/node-js-cluster-with-socket-io-and-express-3/) for further details). This would look like this:

```
var RedisStore = require('socket.io/lib/stores/redis')
    , redis = require('socket.io/node_modules/redis')
    , pub = redis.createClient()
    , sub = redis.createClient()
    , client = redis.createClient();

var io = require('socket.io').listen(80);
io.set('store', new RedisStore({
    redis: redis, redisPub: pub, redisSub: sub, redisClient: client
}));
io.sockets.on('connection', function (socket) {
  socket.emit('news', { hello: 'world' });
});

```

With that, the native [Redis Publisher/Subscriber](http://redis.io/topics/pubsub) functionality is used to implement the required interprocess communication and it makes sure all processes can react on the passed information.

#### EventEmitter replacement

In order to participate in that setup, all event based processes should then also start using the Redis functionality. Especially the events which are meant to "reach" out to the socket connection should be replaced with [Redis calls](https://github.com/mranney/node_redis#publish--subscribe) then:

```
var redis = require('redis');

  // subscription replace the event listener
var sub = redisClient.createClient()
sub.subscribe("channelname");
sub.on("message", function (channel, msg) {
   console.log(msg)
})

  // publishing replaces the event emitter
var pub = redis.createClient();
pub.publish("channelname", JSON.stringify({ my: data}));

```
This introduces some latency into the system which [might cause scaling problems](http://blog.lightstreamer.com/2013/05/benchmarking-socketio-vs-lightstreamer.html) with higher numbers of clients - but first of all it enables higher numbers of clients at all.

### Final setup

The overall setup can be seen in the small demonstration app on [Github](https://github.com/tolleiv/concurrent-sockets). Within that application a simple HTTP frontend call would trigger the events, which then report back through Redis and the Socket.IO connection. That's quite trivial but complex enough to play with it and to run some tests against it.

Btw. reviewing the demo app works best starting from the related test within [spec/socket-spec.js](https://github.com/tolleiv/concurrent-sockets/blob/master/spec/socket-spec.js).

### Benchmarks

The NodeJS Redis client comes with some nice [benchmarks](https://github.com/mranney/node_redis#performance) already. They reach up to 40000 ops/sec for simple calls with 50 concurrent connections.

For NodeJS there are tons of benchmarks around already - especially the [PayPal benchmark](https://www.paypal-engineering.com/2013/11/22/node-js-at-paypal/) and the [related debate](https://vividcortex.com/blog/2013/12/09/analysis-of-paypals-node-vs-java-benchmarks/), which show that Node can outperform Java significantly, should be mentioned. Along with that the [Practical socket.io Benchmarking](http://drewww.github.io/socket.io-benchmarking/) by Drew Harry is worth a lookup.

What's left for me is the question how many concurrent socket connections this could withstand and how all this scales depending on the amount of messages. The assumption from [existing benchmarks](http://mrjoes.github.io/2011/12/15/sockjs-bench.html) would be that every worker process should be able to bind ~2k socket connections with approx. 10k messages per second. But those are not closely related to the described setup and they leave room for further investigations.


