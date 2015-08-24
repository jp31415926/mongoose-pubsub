# Mongoose Pub/Sub

This node module uses the "tailable cursor" feature of MongoDB capped collections to implement pub/sub messaging.


### Installation


npm install mongoose-pubsub


### Use

```
var MessageQueue = require('mongoose-pubsub');
var messenger = new MessageQueue();

//channel names are used as filters
var channelName = 'news';
messenger.subscribe(channelName, true); //subscribe
messenger.subscribe(channelName, false); //unsubscribe

// connect() begins "tailing" the collection
messenger.connect(function(){
  // emits events for each new message on the channel
  messenger.on(channelName, function(message){
    console.log(channelName, message);
  });
});

// you can send without connect() first.
messenger.send(channelName, {some: 'message'}, function(err){
  console.log('Sent message');
});
```

See the test directory for more information.

Note: The best way to use this in your application is to create a file like the following that exports a singleton. Then, when you require this in multiple files in your app, you always get the same instance. 

```
// in lib/messenger.js ...
var MessageQueue = require('mongoose-pubsub');
module.exports = new MessageQueue({retryInterval: 100});

// in other files in your app ...
var messenger = require('./lib/messenger');
messenger.on(...
```



### Tests

```
npm test
npm run lint
npm run converage
```
