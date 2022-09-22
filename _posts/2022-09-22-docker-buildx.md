---
layout: post
title: Build multi platform docker image
---

# こんにちは！

ที่มาของ blog นี้เนื่องมาจากผมได้ซื้อ Raspberry Pi 4 มาใช้เพื่อรันแอปบางตัวที่ผมทำขึ้นมา แต่ด้วยว่า docker image ที่ผมมีอยู่นั้นไม่สามารถรันบน Raspberry Pi ได้เพราะมันใช้ cpu คนละสถาปัตยกรรมกัน (Raspberry Pi เป็น arm) ผมได้ไปเจอว่า docker มีคำสั่ง buildx ที่เอาไว้ใช้ build multi platform image โดยบทความนี้เราจะมาลองสร้าง image ที่เอาไว้รันได้ทั้งบนคอมทั่วไปกับ Raspberry Pi กัน

## สิ่งที่ต้องใช้

-   Docker account ที่เอาไว้ push image
-   Docker พร้ออม **buildx cli** (ถ้าลง Docker desktop น่าจะมีลงไว้ให้อยู่อยู่เเล้ว)
-   Raspberry Pi พร้อมลง docker ที่เอาไว้รัน docker image
-   Node js สำหรับทำโปรเจกต์ทดลอง

## สร้างโปรเจกต์ทดลอง

เราจะสร้าง API มา 1 ตัวที่จะเอาใช้ทดสอบการรันในครั้งนี้ โดยจะใช้ Node js กับ framework Fastify

1. สร้างโฟล์เดอร์ชื่อ `buildx-example` แล้วก็ไปที่โฟล์เดอร์ที่สร้างไว้
2. รันคำสั่งด้านล่างเพื่อสร้างโปรเจกต์

    ```bash
    $ npm init
    ```

3. รันคำสั่งด้านล่างเพื่อลง fastify

    ```bash
    $ npm install fastify
    ```

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

5. รองรัน API ด้วยคำสั่งด้านล่าง จะเห็นว่ามันกำลังทำงานที่ port 3000

    ```bash
    $ node server
    ```

6. ลองใช้คำสั่งด้านล่าง จะได้ผลลัพธ์ดังนี้ `{"hello":"world"}`

    ```bash
    $ curl localhost:3000
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

1. รันคำสั่งด้านล่างเพื่อที่จะเตรียมพร้อมในการ build `linux/arm/v7`

    ```bash
    $ docker run --privileged --rm tonistiigi/binfmt --install all
    ```

2. สร้างตัว builder ด้วยคำสั่ง

    ```bash
    $ docker buildx create --use --name multi-arch-builder
    ```

3. build image ด้วยคำสั่ง

    ````bash
    $ docker buildx build --platform=linux/amd64,linux/arm/v7 -t {your-docker-username}/buildx-example:latest --push .

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
    ``` ```

    ````

4. เมื่อ build เสร็จแล้วให้ลองไปที่เว็บไซต์ docker เพื่อเช็คว่า image มีการ push ไปหรือไม่

_ตัวอย่าง account ของผม_

![repo-home](/assets/docker-buildx/buildx-home.png)

จะเห็นว่า image อันนี้ support **linux/amd64** กับ **linux/arm/v7**

![repo-tag](/assets/docker-buildx/buildx-tag.png)

## ทดลองใช้ใน Raspberry Pi 4

1. ใน Raspberry Pi ให้รันคำสั่งตามด้านล่าง

    ```bash
    $ docker run -p 3000:3000 --name buildx-example -d {your-docker-username}/buildx-example:latest
    ```

2. จากนั้นให้ลองใช้ curl ดูเพื่อเช็คว่าได้ผมลัพธ์ตามที่ต้องการหรือไม่

    ```bash
    $ curl localhost:3000
    ```

![result in pi](/assets//docker-buildx/pi.png)

## ส่งท้าย

เป็นยังไงบ้างครับสำหรับการ build docker image ที่ support หลาย cpu นอกจากลองรันที่ตัว Raspberry Pi แล้วสามารถ pull มาลองที่เครื่องที่ใช้ cpu ที่เป็น x64 ได้ด้วย แล้วก็หวังว่าบทความนี้จะมีประโยชน์นะครับ

#### References

-   [Docker buildx document](https://docs.docker.com/build/building/multi-platform/)
-   [Fastify Getting Started](https://www.fastify.io/docs/latest/Guides/Getting-Started/)

# またね！
