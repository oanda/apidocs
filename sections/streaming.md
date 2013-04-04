# Experimental WebSocket Streaming API

## socket.io

We offer streaming rates using the socket.io protocol over WebSockets.

Please see the [socket.io documentation](https://github.com/learnboost/socket.io/wiki) for more information about the protocol and to find a client for your favourite language.

## Getting the js client

    <script src="http://api-sandbox.oanda.com/ratestream/socket.io.js"></script>

## Connecting to the streaming server

    var socket = io.connect('http://api-sandbox.oanda.com' , {
      resource : 'ratestream'
    });

## Subscribing to ticks for a set of instruments

    socket.emit('subscribe', {'instruments': ['EUR/USD','USD/CAD']});

## Consuming a tick

    socket.on('tick', function (data) {
      console.log("Received tick:" + JSON.stringify(data));
    });

## Tick format

    {
     "instrument":"USD/CAD",
     "bid":1.01174,
     "ask":1.0118,
     "timestamp":1365098247,
     "timeusec":585363
    }
    
## Example application
[Streaming API Demo](https://github.com/oanda/streamingapi-demo)