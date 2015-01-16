webAppEKGAPI
============
[![Stories in Ready](https://badge.waffle.io/ekgapi/webappekgapi.png?label=ready&title=Ready)](https://waffle.io/ekgapi/webappekgapi) [![Build Status](https://travis-ci.org/EKGAPI/webAppEKGAPI.svg?branch=master)](https://travis-ci.org/EKGAPI/webAppEKGAPI)
===========

Check out our site [Kardia](http://kardia.io/)!

<!-- To view our commented code, please click [here](http://www.explainjs.com/explain?src=https%3A%2F%2Fraw.githubusercontent.com%2FEKGAPI%2FwebAppEKGAPI%2Fmaster%2Fdist%2FnewConcat.js)! -->

App Architecture
============
![alt text](http://res.cloudinary.com/kardia-io/image/upload/v1421366596/Screen_Shot_2015-01-15_at_4_02_38_PM_d3unqx.png "App Architecture")

Server
============
### Web Sockets: Require the Socket Functionality
```javascript
require('./server/python/pythonComm.js')(io);
```

### Socket.on() && Socket.emit()
Whenever our Swift app emits the event 'message', the node.js server will listen for any event called 'message' using socket.on().
```javascript
socket.on('message')
```
The server can then relay the data to our web app with its own emit labeled '/analysisChart'.
```javascript
socket.broadcast.emit('/analysisChart', { "data": data });
```

### Client.invoke()
While listening to the event 'message', node will call the python server's own functions using zerorpc's native 'invoke'.
```javascript
client.invoke("functionName", data, function(error, result, more){
  if (error) {
    console.log('error');
  }
  console.log('data is whatever info you want to send to the python server')
  console.log('result is whatever the python server returns');
});
```

### Socket.emit('crunch');
When the invoking our created python function called 'crunch', the python server will send back an analysis of the data it received, which will be emitted in an event called '/node.js'. Anything listening 'on' these emits will receive the result of the 'emit'.
```javascript
socket.emit('/node.js', result); //emit to swift app

socket.broadcast.emit('/node.js', result); //emit to webapp or anything else listening
```









Client
============