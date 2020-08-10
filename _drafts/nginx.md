---
layout: post
title: Proxy pass with Nginx at the ease!
---

# Hi, NGINX!

Some of you might know about NGINX. Some of you might not. So mayne it is good to explain about it a bit. From official [website](https://www.nginx.com/resources/glossary/nginx/), it is open source software for web serving, reverse proxying, caching, load balancing, media streaming, and more. It started out as a web server designed for maximum performance and stability. This is a first couple of sentences from website. Maybe this is too abstract, to explain in the pragmatic way, it is the tool which can help you manange the request that come into the server. For instance, if there are more than 1 service in you servers, NGINX can help you redirect the request to the correct service.

Today I will show you how to setup the NGINX for your backend API, using https with self-signed certificate. So first I will create sample backend API by using Express.js. Let's get started!

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

## Create self-signed certficate

You can use this command to create self-signed certificate. You will be asked to fill in the pass phase. That password also be needed for NGINX. Create a file to keep it.

```bash
$ openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365
```

You can place certificate and key whereever you like. For this example, it will be in `/root/cert`

## Configure NGINX

Before you config NGINX, you need to have a program first. There are a lot of OS in this world. Thus, I cannot give you all how to install nginx in each OS. I will give you the easy one, Ubuntu.

```bash
$ sudo apt update
$ sudo apt install nginx
```

Easy, isn't it?

Then, you go to `/etc/nginx/conf.d` . If there is an example file, rename that file to something with suffix `.conf.bk` to be ignored by NGINX.

Create a new file with suffix `conf.d` . E.g. host.conf and put this config into that file

```
server {
    listen              443 ssl;
    server_name         backend1.example.com;

    ssl_certificate     /root/cert/cert.pem;
    ssl_certificate_key /root/cert/key.pem;
    ssl_password_file   /root/cert/cert_pass.pass;

    location / {
        proxy_pass http://localhost:3000/;
    }
}
```
