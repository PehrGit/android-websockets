# WebSocket client for Android

A very simple bare-minimum WebSocket client for Android.

## Credits

The hybi parser is based on code from the [faye project](https://github.com/faye/faye-websocket-node). Faye is Copyright (c) 2009-2012 James Coglan. Many thanks for the great open-source library!

Ported from JavaScript to Java by [Eric Butler](https://twitter.com/codebutler) <eric@codebutler.com>.

## Usage

Here's the entire WebSocket API:

```java
List<BasicNameValuePair> extraHeaders = Arrays.asList(
    new BasicNameValuePair("Cookie", "session=abcd");
);

WebSocketClient client = new WebSocketClient(URI.create("wss://irccloud.com"), new WebSocketClient.Handler() {
    @Override
    public void onConnect() {
        Log.d(TAG, "Connected!");
    }

    @Override
    public void onMessage(String message) {
        Log.d(TAG, String.format("Got string message! %s", message));
    }

    @Override
    public void onMessage(byte[] data) {
        Log.d(TAG, String.format("Got binary message! %s", toHexString(data));
    }

    @Override
    public void onDisconnect(int code, String reason) {
        Log.d(TAG, String.format("Disconnected! Code: %d Reason: %s", code, reason));
    }

    @Override
    public void onError(Exception error) {
        Log.e(TAG, "Error!", error);
    }
}, extraHeaders);

client.connect();

// Later… 
client.send("hello!");
client.send(new byte[] { 0xDE, 0xAD, 0xBE, 0xEF });
client.disconnect();
```

And here's the Socket.IO API built on top of that:


```java
WebSocketClient client = new WebSocketClient(URI.create("wss://example.com"), new WebSocketClient.Handler() {
    @Override
    public void onConnect() {
        Log.d(TAG, "Connected!");
    }

    @Override
    public void on(String event, JSONArray arguments) {
        Log.d(TAG, String.format("Got event %s: %s", event, arguments.toString()));
    }

    @Override
    public void onDisconnect(int code, String reason) {
        Log.d(TAG, String.format("Disconnected! Code: %d Reason: %s", code, reason));
    }

    @Override
    public void onError(Exception error) {
        Log.e(TAG, "Error!", error);
    }
});

client.connect();

// Later… 
JSONArray arguments = new JSONArray();
arguments.put("first argument");
JSONObject second = new JSONObject();
second.put("dictionary", true);
arguments.put(second)
client.send("hello", arguments);
client.disconnect();
```



## TODO

* Run [autobahn tests](http://autobahn.ws/testsuite)
* Investigate using [naga](http://code.google.com/p/naga/) instead of threads.

## License

(The MIT License)
	
	Copyright (c) 2009-2012 James Coglan
	Copyright (c) 2012 Eric Butler 
	
	Permission is hereby granted, free of charge, to any person obtaining a copy of
	this software and associated documentation files (the 'Software'), to deal in
	the Software without restriction, including without limitation the rights to use,
	copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the
	Software, and to permit persons to whom the Software is furnished to do so,
	subject to the following conditions:
	
	The above copyright notice and this permission notice shall be included in all
	copies or substantial portions of the Software.
	
	THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
	IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
	FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
	COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
	IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
	CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
	 
