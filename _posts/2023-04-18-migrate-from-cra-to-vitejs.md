---
layout: post
title: Migrate from CRA to Vite.js
---

# こんにちは！

เมื่อไม่นานมานี้ในเว็บ react official ได้เอา CRA (Create React App) ออกจากเว็บไซต์ ซึ่งมันบ่งบอกเป็นนัยได้ว่าให้ย้ายออกไปใช้ตัวอื่นแทน lol หลังจากนั้นผมก็ลอง ๆ ไปหาดูว่าควรจะเปลี่ยนไปใช้อะไรดั แล้วก็ได้ไปเจอตัวนี้ซึ่งก็คือ Vite.js เหมือนแต่เดิมเค้าทำให้ Vue.js แต่ตอนนี้เค้าก็ support React แล้วเลยตัดสินใจลองเปลี่ยนมาใช้ตัวนี้ดู

ปล. โปรเจดต์ของผมเป็น Typescript นะครับ
ปล2. Vite.js จะไม่ได้ support testing by default ต้องไปหาวิธีเพิ่มต่อ

# ขั้นตอนการ migrate

## เพิ่ม Vite.js library

ไปที่โปรเจกต์แล้วรันคำสั่งนี้เพื่อเพิ่ม Vite.js library
ิ

```bash
$ yarn add --dev vite @vitejs/plugin-react vite-tsconfig-paths vite-plugin-svgr
```

เสร็จเเล้วให้เข้าไปแก้ไฟล์ package.json จาก

```json
scripts: {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject"
    ..
    ..
}
```

เป็น

```json
scripts: {
    "start": "vite",
    "build": "tsc && vite build",
    "serve": "vite preview"
    ..
    ..
}
```
