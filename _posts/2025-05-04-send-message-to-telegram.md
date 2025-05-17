---
layout: post
title: Send message to Telegram chat by Telegram bot
---

สวัสดีครับ ในวันนี้ผมจะมาพูดถึงวิธีการที่เราจะสร้าง bot ใน telegram เพื่อจะส่งข้อความบางอย่างให้เรา แต่ก่อนหลาย ๆ คนอาจจะใช้ตัว Line Notify เอาไว้ส่งข้อความ เข้า group Line หรือส่งส่วนตัว แต่ว่าตอนนี้ Line Notify ได้ปิดให้บริการไปเรียนร้อยแล้ว Telegram ก็จะเป็นทางเลือกอีกทางหนึ่ง ที่สำคัญคือเป็นบริการที่ฟรี ซึ่งคงเป็นตัวเลือกที่ดีที่จะใช้แทนตัว Line Notify

ในตัวอย่างนี้ผมจะใช้ Node.js เป็นตัวอย่างในการส่งข้อความนะครับ

สิ่งที่ต้องเตรียมก่อนเริ่ม

1. บัญชี Telegram
2. Node.js

วิธีทำ

1. สร้างบัญชี Telegram ผ่าน Application Telegram บน iOS, Android หรือ ผ่าน store ของ OS นั้น ๆ

2. หลังจากสมัครแล้ว น่าจะเห็นตามรูปด้านล่าง

<p style="text-align: center;">
    <img src="/assets/telegram/home.png" alt="home" />
</p>

3.

https://api.telegram.org/bot<YourBOTToken>/getUpdates
