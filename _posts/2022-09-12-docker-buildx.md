---
layout: post
title: Build multiplatform docker image
---

# こんにちは！

ที่มาของ blog นี้เนื่องมาจากผมได้ซื้อ Raspberry Pi 4 มาใช้เพื่อรันแอปบางตัวที่ผมทำขึ้นมา แต่ด้วยว่า docker image ที่ผมมีอยู่นั้นไม่สามารถรันบน Raspberry Pi ได้เพราะมันใช้ cpu คนละสถาปัตยกรรมกัน (Raspberry Pi เป็น arm) ผมได้ไปเจอว่า docker มีคำสั่ง buildx ที่เอาไว้ใช้ build multiplatform architecture ซึ่งบทความนี้เราจะมาลองสร้าง image ที่เอาไว้รันได้ทั้งบนคอมทั่วไปกับ Raspberry Pi กัน
