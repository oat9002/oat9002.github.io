---
layout: post
title: Commit code inside Github Actions
---

# Hi

สวัสดีครับ บทความในวันนี้จะเป็นเรื่องของ github actions ครับ ถ้าเพื่อยได้ใช้ github เป็นที่เก็บโค้ดก็น่าจะเคยได้ยินบริการตัวนี้จาก github มาบ้าง github actions เป็นบริการสำหรัยการทำ CI/CD pipeline ข้อดีของมันคือเราสารถใช้ได้ฟรีได้ระดับนึง ถ้าโปรเจกต์ของเราเป็นแค่งานอดิเรก free tier น่าจะครอบคลุมได้ทั้งหมด ซึ่งในบทความนี้จะแชร์เรื่องว่าถ้าเราอยากจะ format code ให้อัตโนมัติ เพื่อที่ให้โค้ดจองเราทีการ indent ตาม config ของเราอยู่เสมอ ตัวอย่างในบทความนี้จะใช้ scala3 แล้วก็ใช้ library scalafmt ในการ format code IDE จะเป็น IntelliJ Idea CE 2023.1.3

#  เริ่มกันเลย

## เตรียมโค้ดที่ใช้ทดสอบ

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

## เข้าสู่ github actions

- ในการที่เราจะใช้ github action นั้น เราต้องสร้างโฟล์เดอร์ `.github` ไว้ที่ root โฟล์เดอร์แล้วก็ข้างในให้สร้างโฟล์เดอร์ `workflows`

- จากนี้นให้สร้างไฟล์ `pr_build.yml` แล้วใส่โค้ดตามนี้

```yml
name: pr_build

on:
  pull_request:
    branches: [ main ]

jobs:
  formatCode:
    runs-on: ubuntu-latest
    if: ${{ !contains(github.actor, 'github-actions[bot]') }}
    steps:
    - uses: actions/checkout@v3
      with:
        ref: ${{ github.head_ref }}
        token: ${{ secrets.ACTION_TOKEN }}
    - name: Set up JDK 16
      uses: actions/setup-java@v3
      with:
        distribution: 'temurin'
        java-version: '16'
    - name: Set Git Username
      run: |
        git config --global user.name "github-actions[bot]"
        git config --global user.email "github-actions[bot]@users.noreply.github.com"
    - name: Format code
      run: |
        sbt scalafmtAll
    - name: Check for changes
      run: |
        if git diff-index --quiet HEAD; then
          echo "IS_CHANGED=false" >> $GITHUB_ENV
        else
          echo "IS_CHANGED=true" >> $GITHUB_ENV
        fi
    - name: Push code if format is needed
      if: ${{ env.IS_CHANGED == 'true' }}
      run: |
        git add --all
        git commit -m ":recycle: format code"
        git push origin ${{ github.head_ref }}
```


