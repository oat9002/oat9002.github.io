---
layout: post
title: Proxy pass with Nginx at the ease!
---

# Hi, NGINX!

Some of you might know about NGINX. Some of you might not. So perhaps it would be good to explain about it a bit. From official [website](https://www.nginx.com/resources/glossary/nginx/), it is `open source software for web serving, reverse proxying, caching, load balancing, media streaming, and more. It started out as a web server designed for maximum performance and stability`. This is a first couple of sentences from website. Maybe this is too abstract, to explain in the pragmatic way, it is the tool which can help you manange the request whoch come into the server. For instance, if there are more than 1 service in you servers, NGINX can help you redirect the request to the correct service. You can also spwan your application more than 1 instance and let NGINX do the load balancing.

Today I will show you how to setup the NGINX for your backend API, using https with self-signed certificate, in linux. For API, I will create sample web server by Express.js. Let's get started!

## Prerequisite

1. Node (I'm using version 14.8.0)
2. Ubuntu or other distro

## Create an API

You can use either **npm** or **yarn**. I will use **yarn** in this example

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

You can use this command to create self-signed certificate. You will be asked to fill in the pass phase. That password also be needed for NGINX. Thus, create a file to keep it.

```bash
$ openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365
```

You can place certificate and key whereever you like. For this example, it will be in `/root/cert`

## Configure NGINX

Before you config NGINX, you need to have a program first. You cannot use NGINX without NGINX installed. To install, you can run this command

```bash
$ sudo apt update
$ sudo apt install nginx
```

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

After that you can restart NGINX by this command

```bash
$ sudo systemctl restart nginx
```

> Tip: You can make NGINX start at the startup by ussing this command `$ sudo systemctl enable nginx`

Let's test it. Open a browser and go to `https://<your-server-domain or server-ip>/hello`. You should see `Hello world` return back from API.

Not too difficult, isn't it? This is it. By the way, there are other services which behave like this as well, such as, kong, Apache etc. Thank you for reading. Till next time

### See ya!
