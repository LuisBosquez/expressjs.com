---
layout: page
title: Overriding sugar methods
menu: guide
lang: en
---
<div id="page-doc" markdown="1">
# Overriding sugar methods

Sugar methods are methods added by Express on Node's HTTP request and response objects, which make up the bulk of Express' documented methods. Since the request and response objects are accessible from an instance of Express, you can override their signature and behavior with your own implementation by assigning a custom function.

Here is a very simple example of overriding the [res.send](/{{ page.lang }}/4x/api.html#res.send) method. This will force all request to the app to be rejected with a 403 response.

```js
var express = require('express')
var app = express()

app.response.send = function () {
  this.statusCode = 403;
  this.end('FORBIDDEN')
}
```

Following is a more practical example of overriding the behavior of [res.sendStatus](/{{ page.lang }}/4x/api.html#res.sendStatus).

```js
app.response.sendStatus = function (statusCode, type, message) {
  // code is intentionally kept simple for demonstration purpose
  return this.contentType(type)
  .status(statusCode)
  .send(message);
}
```

The above implementation completely changes the original signature of [res.sendStatus](/{{ page.lang }}/4x/api.html#res.sendStatus). It now accepts a status code, encoding type, and the message to be sent to the client.

The overridden method may now be used this way:

```js
res.sendStatus(404, 'application/json', '{"error":"resource not found"}')
```
</div>
