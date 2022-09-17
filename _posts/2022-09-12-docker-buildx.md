---
layout: post
title: Build multiplatform docker image
---

# こんにちは！

ที่มาของ blog นี้เนื่องมาจากผมได้ซื้อ Raspberry Pi 4 มาใช้เพื่อรันแอปบางตัวที่ผมทำขึ้นมา แต่ด้วยว่า docker image ที่ผมมีอยู่นั้นไม่สามารถรันบน Raspberry Pi ได้เพราะมันใช้ cpu คนละสถาปัตยกรรมกัน (Raspberry Pi เป็น arm) ผมได้ไปเจอว่า docker มีคำสั่ง buildx ที่เอาไว้ใช้ build multiplatform architecture ซึ่งบทความนี้เราจะมาลองสร้าง image ที่เอาไว้รันได้ทั้งบนคอมทั่วไปกับ Raspberry Pi กัน

## สิ่งที่ต้องใช้

-   docker 19.03 ขึ้นไป
-   Raspberry Pi ที่เอาไว้รัน docker image
-   Node js

## สร้างโปรเจกต์ทดลอง

เราจะสร้าง API มา 1 ตัวที่จะเอาใช้ทดสอบการรันในครั้งนี้ โดยจะใช้ Node js กับ framework Fastify

1. สร้าง folder ชื่อ docker-buildx แล้วก็ไปที่ folder ที่สร้างไว้
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
        await fastify.listen({ port: 3000 });
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

7. สร้าง Dockerfile ตามนี้

```docker
FROM node:alpine
WORKDIR /app
COPY package.json .
COPY package-lock.json .
COPY server.js .
EXPOSE 3000
RUN npm install
CMD ["node", "server"]

```
