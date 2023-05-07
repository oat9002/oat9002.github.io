---
layout: post
title: Migrate from CRA to Vite.js
---

# มาเรื่มกันเลย !

เมื่อไม่นานมานี้ในเว็บ react official ได้เอา **CRA (Create React App)** ออกจากเว็บไซต์ ซึ่งมันก็เป็นสัญญาณบ่งบอกได้ว่าให้ย้ายออกไปใช้ตัวอื่นแทนได้แล้วนะ หลังจากนั้นผมก็ลอง ๆ ไปหาดูว่าควรจะเปลี่ยนไปใช้อะไรดั แล้วก็ได้ไปเจอตัวนี้ซึ่งก็คือ **Vite.js** เหมือนแต่เดิมเค้าทำให้ Vue.js แต่ตอนนี้เค้าก็ support React แล้วก็เลยตัดสินใจลองเปลี่ยนมาใช้ตัวนี้ดู ซึ่งสำหรับผมการ migrate นี้ทำได้ค่อนข้างง่าย ไม่ยุ่งยากเหมือนที่ผมเคยคิดไว้

ปล. ตัวอย่างในที่นี้จะใช้ Typescript นะครับ

ปล2. Vite.js เท่าที่ผมรู้จะไม่ได้ support testing by default อาจจะต้องไปหาวิธีการเพิ่มต่อ

# ขั้นตอนการ migrate

1. ไปที่โปรเจกต์แล้วรันคำสั่งนี้เพื่อเพิ่ม Vite.js lib

    ```bash
    $ yarn add --dev vite @vitejs/plugin-react vite-tsconfig-paths vite-plugin-svgr
    ```

2. เสร็จเเล้วให้เข้าไปแก้ไฟล์ `package.json` จาก

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

3. แก้ไฟล์ `tsconfig.json` บางจุดตามนี้

    ```json
    {
        "compilerOptions": {
            "target": "ESNext",
            "lib": [
                "dom",
                "dom.iterable",
                "esnext"
            ],
            "types": ["vite/client", "vite-plugin-svgr/client"],
            "allowJs": false,
            "skipLibCheck": false,
            "esModuleInterop": false,
            ..
            ..
    }
    ```

4. เพิ่มไฟล์ `vite.config.ts` ไว้ที่ root โฟล์เดอร์

    ```typescript
    import react from "@vitejs/plugin-react";
    import { defineConfig } from "vite";
    import svgrPlugin from "vite-plugin-svgr";
    import viteTsconfigPaths from "vite-tsconfig-paths";

    // https://vitejs.dev/config/
    export default defineConfig({
        plugins: [react(), viteTsconfigPaths(), svgrPlugin()],
        build: {
            outDir: "build",
        },
    });
    ```

5. แก้ evnvironment variable prefix จาก `REACT_APP` เป็น `VITE`

6. สร้างไฟลฺ์ `evn.d.ts` ใว้ในโฟล์เดอรฺ์ `src` ให้สร้าง property ตามที่ environment variable ของเรา

    ```typescript
    interface ImportMetaEnv {
        readonly <variable name>: <varibale type>; // ex. readonly VITE_SERVER_URL: string;
        ..
        ..
    }

    interface ImportMeta {
        readonly env: ImportMetaEnv;
    }
    ```

7. แก้โค้ดที่เรียกใช้ environment variable จาก `process.evn.REACT_....` เป็น `import.meta.evn.VITE_...`

8. จากนั้นลอง start web app ด้วย `yarn start` ถ้า web app start ได้ปกตืก็เป็นอันเสร็จเรียบร่อย

**_ตัวอย่าง commit ของการ migrate_** _[Click Here](https://github.com/oat9002/GoldPriceTracking/commit/c91413e23e99e6ce573f56dffc1c695a6c398901)_

## ไว้เจอกันใหม่ !
