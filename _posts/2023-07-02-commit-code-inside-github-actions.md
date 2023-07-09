---
layout: post
title: Commit code inside Github Actions
---

# Hi

สวัสดีครับ บทความในวันนี้จะเป็นเรื่องของ github actions ครับ ถ้าเพื่อยได้ใช้ github เป็นที่เก็บโค้ดก็น่าจะเคยได้ยินบริการตัวนี้จาก github มาบ้าง github actions เป็นบริการสำหรัยการทำ CI/CD pipeline ข้อดีของมันคือเราสารถใช้ได้ฟรีได้ระดับนึง ถ้าโปรเจกต์ของเราเป็นแค่งานอดิเรก free tier น่าจะครอบคลุมได้ทั้งหมด ซึ่งในบทความนี้จะแชร์เรื่องว่าถ้าเราอยากจะ format code ให้อัตโนมัติ เพื่อที่ให้โค้ดจองเราทีการ indent ตาม config ของเราอยู่เสมอ ตัวอย่างในบทความนี้จะใช้ scala3 แล้วก็ใช้ library scalafmt ในการ format code IDE จะเป็น IntelliJ Idea CE 2023.1.3

##  เริ่มกันเลย

- สร้างโปรเจกต์ตามนี้

<p>
    <img src="/assets/commit-code-inside-github-actions/intialize-project.png" />
</p>

- สร้างไฟล์ `Main.scala` ไว้ในโฟล์เดอร์ `src` แล้วพิมพ์โค้ดตามนี้

```scala
object Main extends App {
  val data = Data("Hello scala")
  println(data.content)
}

case class Data(content: String)

```
- สร้างไฟล์ `plugins.sbt` ที่โฟล์เดอร์ `project` เพิ่ม plugin scalafmt ตามนี้

```
addSbtPlugin("org.scalameta" % "sbt-scalafmt" % "2.4.6")
```

- จากนั้นให้ลองเทสโดยการแก้ format โค้ดใน `Main.scala` ตามนี้

```scala
object Main extends App {
  val data = Data("Hello scala")
  println(data.content)
}

case class Data(
                 
                 content: String)
```
- เปิด sbt shell ขึ้นมาโดยใช้คำสั่ง `scamafmtAll` หรือถ้าไม่ได้ใช้ sbt shell ใช้เป็น terminal ธรรมดา ให้ใช้คำสั่ง `sbt scalafmtAll`

- ลองเข้าไปดูไฟล์ `Main.scala` อีกรอบ จะได้ผลลัพธ์ตามนี้

```scala
object Main extends App {
  val data = Data("Hello scala")
  println(data.content)
}

case class Data(content: String)
```


