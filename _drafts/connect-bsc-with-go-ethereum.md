---
layout: post
title: เชื่อมต่อ Smart Contract บน Binance Smart Chain ด้วย go-ethereum
categories: tutorial
---

# สวัสดี!

Blog post นี้เราจะมาเขียนโปรแกรมเพื่อที่จะติดต่อกับ Smart Contract บน Binance Smart Chain (ผมจะเรียกย่อ ๆ ว่า BSC นะครับ) ด้วยภาษา golang จุดเริ่มต้นของ blog นี้เริ่มจากผมเริ่มเข้าสู้วงการ DeFi เเล้วก็มารู้จักการทำ staking เหรียญ (เหมือนเราเอาเงินไปวางเดิมพันเเล้วเราจะได้ reward กลับคืนมา) ซึ่งผมได้ไปลองเล่น pancakeswap ซึ่งเป็น exchange เจ้าดังเจ้าหนึ่ง โดยผมได้ลองเอาเหรียญ cake ไป staking เพื่อเอา cake กลับมา แต่ทีนี้ระบบมันจะมีสิ่งที่เรียกว่าการ compound คือการหลอมเหรียญ reward ที่ได้กลับเข้าไปใน pool ซึ่งจะทำให้เราได้รับรางวัลมากขึ้น ซึ่งปกติเราก็ต้องเข้าเว็บ pancakeswap เเล้วก็เข้าไปกด compound ด้วยตัวเอง จริง ๆ เเล้วทาง pancakeswap ก็มีบริการทำ auto compound สำหรับ pool cake-cake แต่ต้องต้องเสียค่าธรรมเนียมให้กับเค้า ซึ่งผมก็ไม่อยากเสีย 5555 ก็เลยลองนั่งคิดว่าเป็นได้มั้ยว่าเราจะทำ application อันนึงที่ทำแบบนี้ได้บ้าง ก็เลยได้ลองศึกษาวิธีที่เราจะสามารถเขียน application ที่จะสามารถติดต่อกับ BSC ได้ แล้วก็ที่เลือกใช้ golang เพราะเป็นภาษาที่ผมไม่เคยเขียนก็เลยอยากจะลองใช้มันดูด้วย

ตัวอย่างใน blog นี้เราจะมาลองสร้าง smart contract แบบง่าย ๆ เพื่อที่จะ deploy ขึ้น test network ของ BSC เเล้วก็ลองใช้ application ของเราติดต่อกับ contract นั้นดู

## สิ่งที่ต้องมีก่อนที่จะเริ่มทำ

-   account บน metamask ที่มี BNB บน testnet ของ BSC (ผมใช้ metamask google chrome extension เป็นตัวจัดการ สามารถไปค้นหาวิธีการสร้าง account เเละการ setup ให้ account ของเราไปต่อกับ test network และขอ BNB ฟรีใน google ได้)

## สร้าง Smart Contract แบบง่าย ๆ

การเขียน Smart Contract บน BSC จะใช้ภาษา solidity ซึ่งเป็นภาษาเดียวกันกับ Smart Contract บน Ethereum และการ deploy Smart Contract ของเรานั้นจะใช้ https://remix.ethereum.org/ deploy

1.  ให้เข้าไปที่เว็บ https://remix.ethereum.org/ แล้วจะเจอหน้าแรกแบบนี้
2.  ในแถบด้านซ้าย ให้สร้างไฟล์ `MyContract.sol` ใน folder `contracts`
3.  copy code ด้านล่างไปที่ไฟล์ MyContract.sol ที่เราสร้างไว้

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract MyContract {
    mapping(address => int256) public counter;

    function increase() public {
        counter[msg.sender] += 1;
    }

    function decrease() public {
        counter[msg.sender] -= 1;
    }

    function getYourCounter() public view returns (int256) {
        return counter[msg.sender];
    }
}

```

การทำงาน contract ด้านบนจะเป็นการนับขึ้นหรือนับลงสำหรับ address ที่มากระทำกับ contract นี้ จะเห็นได้ว่าเมื่อเราทำการ increase หรือ decrease counter ether ใน account test ที่ ของเราจะลดลงเรื่อย ที่เป็นแบบนั้นเพราะว่าการเปลี่ยนแปลงใน blockchain แต่ละครั้งจะต้องมีการ validate transaction ซึ่งเราจะต้องเสียค่าธรรมเนียมให้กับการ validate นั้น

4.  compile contract ของเรา
5.  test contract เราโดยการ deploy contract ไปที่ `JavaScript VM (London)` environment
6.  เมื่อเราคลิก `Deploy` จะมี `Deployed Contracts` ปรากฎอยู่ด้านล่าง
7.  ลองทดสอบ contract ของเราโดยลองคลิก increase, decrease หรือ getYourCounter
8.  เมื่อมันใจเเล้วว่า contract ทำงานถูกต้อง ให้ไป set metamask ของเราให้ไปต่อกับ test network ของ BSC จากนั้นให้เปลี่ยน environment ที่จะ deploy เป็น `Injected Web3`
9.  ตัว metamask จะมีการขอ permission ให้เราต่อกับเว็บ remix ซึ่งให้ allow ไป
10. ให้ลอง deploy contract เเล้วก็ลองเทส contract ดู

    **_ป.ล. ให้ copy contract address เก็บไว้ด้วย_**