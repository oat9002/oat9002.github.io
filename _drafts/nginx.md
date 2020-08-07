---
layout: post
title: Proxy pass with Nginx at the ease!
---

# Hi, NGINX!

Some of you might know about NGINX. Some of you might not. So mayne it is good to explain about it a bit. From official [website](https://www.nginx.com/resources/glossary/nginx/), it is open source software for web serving, reverse proxying, caching, load balancing, media streaming, and more. It started out as a web server designed for maximum performance and stability. This is a first couple of sentences from website. Maybe this is too abstract, to explain in the pragmatic way, it is the tool which can help you manange the request that come into the server. For instance, if there are more than 1 service in you servers, NGINX can help you redirect the request to the correct service.

Today I will show you how to setup the NGINX for your backend API. So first I will create sample backend API by using Express.js. Let's get started!

## Create an API

It requires either **npm** or **yarn**. I will use **yarn** in this example

1. Run command

```bash
$ yarn init
```

2. Install Express.js

```bash
$ yarn add express
```

3. Create **index.js** with this code

```javascript
const express = require("express");
const app = express();
const port = 3000;

app.get("/hello", (req, res) => {
    res.send("Hello World!");
});

app.listen(port, () => {
    console.log(`Example app listening at http://localhost:${port}`);
});
```

4. Test your api by open any browser and go to `http://localhost:3000/hello` or you can use **curl**

```bash
$ curl http://localhost:3000/hello
```

You should see `Hello world` text return from API
