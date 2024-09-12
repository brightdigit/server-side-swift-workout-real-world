# Communication with Redis and WebSockets

At first we were focused on just serving the json by send the data from the push request to the web socket:


```How do we can track of web socket```

```
chart or code here
```

However for both scaling purposes, we added Redis as an intermediary. We used specifically pub/sub channels to post a health stat to Redis while subscribing to any post and sending it to the websocket.