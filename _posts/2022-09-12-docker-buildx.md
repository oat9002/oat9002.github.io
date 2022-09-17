---
layout: post
title: Build multiplatform docker image
---

# こんにちは！

ที่มาของ blog นี้เนื่องมาจากผมได้ซื้อ Raspberry Pi 4 มาใช้เพื่อรันแอปบางตัวที่ผมทำขึ้นมา แต่ด้วยว่า docker image ที่ผมมีอยู่นั้นไม่สามารถรันบน Raspberry Pi ได้เพราะมันใช้ cpu คนละสถาปัตยกรรมกัน (Raspberry Pi เป็น arm) ผมได้ไปเจอว่า docker มีคำสั่ง buildx ที่เอาไว้ใช้ build multiplatform architecture ซึ่งบทความนี้เราจะมาลองสร้าง image ที่เอาไว้รันได้ทั้งบนคอมทั่วไปกับ Raspberry Pi กัน

## สิ่งที่ต้องใช้

-   Docker account ที่เอาไว้ push image
-   Docker desktop
-   Raspberry Pi พร้อมลง docker ที่เอาไว้รัน docker image
-   Node js

## สร้างโปรเจกต์ทดลอง

เราจะสร้าง API มา 1 ตัวที่จะเอาใช้ทดสอบการรันในครั้งนี้ โดยจะใช้ Node js กับ framework Fastify

1. สร้าง folder ชื่อ `buildx-example` แล้วก็ไปที่ folder ที่สร้างไว้
2. รัน `npm init` เพื่อสร้างโปรเจกต์
3. รัน `npm install fastify`
4. สร้างไฟล์ชื่อ `server.js` แล้วใส่โค้ดด้างล่างลงไป

```js
// Require the framework and instantiate it
const fastify = require("fastify")({ logger: true });

// Declare a route
fastify.get("/", async (request, reply) => {
    return { hello: "world" };
});

// Run the server!
const start = async () => {
    try {
        await fastify.listen({ port: 3000, host: "0.0.0.0" });
    } catch (err) {
        fastify.log.error(err);
        process.exit(1);
    }
};
start();
```

5. รองรัน API ด้วยคำสั่ง `node server` จะเห็นว่ามันกำลังทำงานที่ port 3000
6. ลองใช้คำสั่ง `curl localhost:3000` จะได้ผลลัพธ์ตามด้านล่างนี้

```
{"hello":"world"}
```

7. สร้าง `Dockerfile` ตามนี้

```docker
FROM node:alpine # เลือก image ที่ support cpu ของเรา (linux/arm/v7 สำหรับ Raspberry Pi 4)
WORKDIR /app
COPY package.json .
COPY package-lock.json .
COPY server.js .
EXPOSE 3000
RUN npm install
CMD ["node", "server"]

```

## Buildx

1. รันคำสั่งด้านล่างเพื่อที่จะเตรียมพร้อมในการ build linux/arm/v7

```
$ docker run --privileged --rm tonistiigi/binfmt --install all
```

2. สร้างตัว builder ด้วยคำสั่ง

```
$ docker buildx create --use --name multi-arch-builder
```

3. build image ด้วยคำสั่ง

```
$ docker buildx build --platform=linux/amd64,linux/arm/v7 -t {your-docker-username}/buildx-example:latest --push .
```

4. เมื่อ build เสร็จแล้วให้ลองไปที่เว็บไซต์ docker เพื่อเช็คว่า image มีการ push ไปหรือไม่

![repo-home](/assets/docker-buildx/buildx-home.png)

![repo-tag](/assets/docker-buildx/jsbuildx-tag.png)

## ทดลองใช้ใน Raspberry Pi 4

1. ใน Raspberry Pi ให้รันคำสั่งตามด้านล่าง

```
$ docker run -p 3000:3000 --name buildx-example -d {your-docker-username}/buildx-example:latest
```

2. จากนั้นให้ลองใช้ curl ดูเพื่อเช็คว่าได้ผมลัพธ์ตามที่ต้องการหรือไม่

```
$ curl localhost:3000
```
