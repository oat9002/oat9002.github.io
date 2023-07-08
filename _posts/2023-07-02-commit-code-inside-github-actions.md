---
layout: post
title: Commit code inside Github Actions
---

# Hi

สวัสดีครับ บทความในวันนี้จะเป็นเรื่องของ github actions ครับ ถ้าเพื่อยได้ใช้ github เป็นที่เก็บโค้ดก็น่าจะเคยได้ยินบริการตัวนี้จาก github มาบ้าง github actions เป็นบริการสำหรัยการทำ CI/CD pipeline ข้อดีของมันคือเราสารถใช้ได้ฟรีได้ระดับนึง ถ้าโปรเจกต์ของเราเป็นแค่งานอดิเรก free tier น่าจะครอบคลุมได้ทั้งหมด ซึ่งในบมความนี้จะแชร์เรื่องว่าถ้าเราอยากจะ format code ให้อัตโนมัติ เพื่อที่ให้โค้ดจองเราทีการ indent ตาม config ของเราอยู่เสมอ ตัวอย่างในบทความนี้จะใช้ scala3 แล้วก็ใช้ library scalafmt ในการ format code IDE จะเป็น IntelliJ Idea CE 2023.1.3

##  เรื่มกันเลย

- สร้างโปรเจกต์ตามนี้

<p>
    <img src="/assets/commit-code-inside-github-actions/intialize-project.png" />
</p>

- สร้างไฟล์ 

