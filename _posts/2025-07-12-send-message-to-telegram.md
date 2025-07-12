---
layout: post
title: Send message to Telegram chat by Telegram bot
---

สวัสดีครับ ในวันนี้ผมจะมาพูดถึงวิธีการที่เราจะสร้าง bot ใน telegram เพื่อจะส่งข้อความบางอย่างให้เรา แต่ก่อนหลาย ๆ คนอาจจะใช้ตัว Line Notify เอาไว้ส่งข้อความ เข้า group Line หรือส่งส่วนตัว แต่ว่าตอนนี้ Line Notify ได้ปิดให้บริการไปเรียนร้อยแล้ว Telegram ก็จะเป็นทางเลือกอีกทางหนึ่ง ที่สำคัญคือเป็นบริการที่ฟรี ซึ่งคงเป็นตัวเลือกที่ดีที่จะใช้แทนตัว Line Notify

> ในตัวอย่างนี้ผมจะใช้ Bun เป็นตัวอย่างในการส่งข้อความนะครับ

# สิ่งที่ต้องเตรียมก่อนเริ่ม

1. บัญชี Telegram
2. Bun - Javascript runtime

# วิธีทำ

1.  สร้างบัญชี Telegram ผ่าน Application Telegram บน iOS, Android หรือ ผ่าน store ของ OS นั้น ๆ

2.  หลังจากสมัครแล้ว น่าจะเห็นตามรูปด้านล่าง

    <p style="text-align: center;">
        <img src="/assets/telegram/home.png" alt="home" />
    </p>

3.  ไปที BotFather แล้วใช้คำสั่ง `/newbot' เพื่อสร้างบอทของตัวเอง แล้วใส่ชือที่อยากได้ลงไป

    <p style="text-align: center;">
        <img src="/assets/telegram/newbot.png" alt="newbot" />
    </p>

4.  จากนั้นตั้ง username ของบอท โดยชื่อบอทต้องลงท้ายด้วย `Bot` หรือ `_bot`

    <p style="text-align: center;">
        <img src="/assets/telegram/username.png" alt="username" />
    </p>

5.  เสร็จแล้วจะได้บอท token มา เราจะใช้ในการส่ง message อันจะเป็น secret ควรที่จะหาที่เก็บที่ปลอดภัย

    <p style="text-align: center;">
        <img src="/assets/telegram/token.png" alt="token" />
    </p>

6.  ถ้าเราไปดูเอกสารของ Telegram API นอกจาก bot token แล้วเรายังต้องมีอีกอย่างนึงคือ chat id ซึ่งวิธีง่าย ๆ คือให้เรา search หาชื่อ bot ตัวเอง แล้วส่งข้อความอะไรก็ได้ลงไป จากนั้นให้ไปที่ `https://api.telegram.org/bot<YourBOTToken>/getUpdates` แล้วเราจะได้ chat id มา

    <p style="text-align: center;">
        <img src="/assets/telegram/chatid.png" alt="chatId" />
    </p>

7.  พอได้ ิ bot token กับ chat id มาเรียบร้อย ตอนนี้ก็ถึงเวลาที่เรื่มทำตัวแอปที่ไว้ใช้ส่งข้อความ เมื่อเราลง bun และสร้างโปรเจคเสร็จเรียบร้อยแล้ว ให้แก้ไฟล์ `index.ts` ตามนี้ (อย่าลืมแก้ botToken กับ chatId ด้วยนะครับ)

    ```typescript
    const chatId = <chatId>;
    const botToken = "<botToken>";
    const url = `https://api.telegram.org/bot${botToken}/sendMessage`;

    await fetch(url, {
      method: "POST",
      body: JSON.stringify({
        text: "Hello, this is a test message from your bot!",
        chat_id: chatId,
        parse_mode: "HTML",
      }),
      headers: {
        "Content-Type": "application/json",
      },
    }).catch((err: unknown) => {
      if (err instanceof Error) {
        console.log("telegram notify failed", err);
      }
    });
    ```

8.  ใช้คำสั่ง `bun index.ts` จะได้ผลลัพธ์ตามนี้

    <p style="text-align: center;">
        <img src="/assets/telegram/message.png" alt="message" />
    </p>

อันนี้จะเป็นตัวโปรเจคเผื่อถ้ารันไม่ได้ยังไงก็สามารถโคลนตัวโค้ดจาก [github](https://github.com/oat9002/telegram-message-api-demo) นี้ไปได้เลยครับ
