---
layout: post
title: Commit code inside Github Actions
---

# Hi

สวัสดีครับ บทความในวันนี้จะเป็นเรื่องของ github actions ครับ ถ้าเพื่อยได้ใช้ github เป็นที่เก็บโค้ดก็น่าจะเคยได้ยินบริการตัวนี้จาก github มาบ้าง github actions เป็นบริการสำหรัยการทำ CI/CD pipeline ข้อดีของมันคือเราสารถใช้ได้ฟรีได้ระดับนึง ถ้าโปรเจกต์ของเราเป็นแค่งานอดิเรก free tier น่าจะครอบคลุมได้ทั้งหมด ซึ่งในบทความนี้จะแชร์เรื่องว่าถ้าเราอยากจะ format code ให้อัตโนมัติ เพื่อที่ให้โค้ดจองเราทีการ indent ตาม config ของเราอยู่เสมอ ตัวอย่างในบทความนี้จะใช้ scala3 แล้วก็ใช้ library scalafmt ในการ format code IDE จะเป็น IntelliJ Idea CE 2023.1.3

## โปรแกรมที่ต้องใช้

1. [IntelliJ Idea CE](https://www.jetbrains.com/idea/download/)
2. [java](https://adoptium.net/temurin/releases/)
3. [sbt](https://www.scala-sbt.org/download.html)

## เตรียมโค้ดที่ใช้ทดสอบ

- เปิดโปรแกรม IntelliJ Idea แล้วสร้างโปรเจกต์ตามนี้

  <p>
      <img src="/assets/commit-code-inside-github-actions/intialize-project.png" alt="initialize project" />
  </p>

- สร้างไฟล์ `Main.scala` ไว้ในโฟล์เดอร์ `src` แล้วพิมพ์โค้ดตามนี้

  ```scala
  object Main extends App {
    val data = Data("Hello scala")
    println(data.content)
  }

  case class Data(content: String)

  ```
- สร้างไฟล์ `plugins.sbt` ที่โฟล์เดอร์ `project` เพิ่ม plugin **scalafmt** ตามนี้

  ```
  addSbtPlugin("org.scalameta" % "sbt-scalafmt" % "2.4.6")
  ```

- สร้างไฟล์ `.scalafmt.conf` ไว้ที่ root โฟล์เดอรื ใส่ config ตามนี้

  ```
  version = 3.5.9
  runner.dialect = scala3
  maxColumn = 100
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

- ในการที่เราจะใช้ github actions นั้น เราต้องสร้างโฟล์เดอร์ `.github` ไว้ที่ root โฟล์เดอร์แล้วก็ข้างในให้สร้างโฟล์เดอร์ `workflows`

  <p>
    <img src="/assets/commit-code-inside-github-actions/workflow-folder.png" alt="workflow folder" />
  </p>

- จากนี้นให้สร้างไฟล์ `pr_build.yml` ไว้ในโฟล์เดอร์ `workflows` แล้วใส่โค้ดตามนี้

  {% raw %}
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
      - name: Set up JDK 20
        uses: actions/setup-java@v3
        with:
          distribution: 'temurin'
          java-version: '20'
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
          git push origin
  ```
{% endraw %}

จนถึงขั้นตอนนี้เราก็มีโค้ดที่เตรียมพร้อมสำหรับใช้ใน github actions

## สร้างโปรเจกต์บน github 

- สร้างโปรเจกต์ชื่อว่า `scalafmt-test` แล้วก็ให้เพิ่ม `README.md` ไฟล์ไปด้วยเลย

- จากนั้นไปที่ `Settings` แล้วเลือไปที่ `Actions -> General`

  <p>
    <img src="/assets/commit-code-inside-github-actions/github-setting-1.png" />
  </p>

-  อนุญาติ permission ให้ `GITHUB_TOKEN` สามารถ write ไปยัง reporisitory ของเราได้

  <p>
    <img src="/assets/commit-code-inside-github-actions/github-setting-2.png" />
  </p>

- จากนั้นกลับมาที่โค้ดของเรา ให้เชื่อมโค้ดเราให้เข้ากับ repo ที่เราสร้างขึ้น จากนั้นนให้สร้าง branch ชื่อว่า `feature/github-actions`

- แล้วก็ให้ push code ของเราขึ้น github ไป 

  *ถ้าเห็นว่าไฟล์ที่จะ push ขึ้นมีเยอะผิดปกติ ให้เพื่มไฟล์ `.gitignore` ตามด้านล่างนี้*

    ```
    .DS_Store
    *.class
    *.log
    *~

    # sbt specific
    dist/*
    target/
    lib_managed/
    src_managed/
    project/boot/
    project/plugins/project/
    project/local-plugins.sbt
    .history
    .bsp

    # Scala-IDE specific
    .scala_dependencies
    .cache
    .classpath
    .project
    .settings
    classes/

    # idea
    .idea
    .idea_modules
    /.worksheet/

    # Dotty-IDE
    .dotty-ide-artifact
    .dotty-ide.json

    # Visual Studio Code
    .vscode

    .env
    src/main/resources/application.local.conf
    ```

- จากนั้นไปที่เว็บ github ของเรา ให้ไปสร้าง pull request เป็นชื่ออะไรก็ได้ แล้วก็รอดูผลลัพธ์ 

  จะเห็นได้ว่าถ้าตัว github actions รันเสร็จแล้ว มันจะมี commit ให้เพิ่มขึ้นมา เป็นว่าเสร็จเรียบร้อย

  <p>
    <img src="/assets/commit-code-inside-github-actions/result.png" />
  </p>


จากนี้ทุก pull request ที่มีการสร้างขึ้นมาก็จะถูก format ตาม config ของเราตลอดเวลา