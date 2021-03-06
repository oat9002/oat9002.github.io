---
layout: post
title: Dotnet conference 2021
---

# Hi!

It has been a while I haven not wrote any content. Anyhow, because of this, I have something to write hahaha. This is my first time joining dotnet conference. To be honest, I have never thought I would join or do anything which relates to dotnet. In the past, when I heard about dotnet. I felt that it is kind of old-fashioned technology. Not sure why I thought like that at that time. Perhaps because I saw dotnet framework used in legacy application or the application which has 90s user interface. However, my thought is changed. After I have to use C# in my work and they introduce dotnet Core. Everything is changed. It is open source and able to run cross platform. I am very happy to use it everyday. Due to during covid pandemic, this conference was hold online at 23 Jan 2021. Moreover, it's free conference and no limit for seats. Let's see what this conference give us!

## Discovery the world of .NET

##### What's new in .NET 5

-   Window forms Disigner
-   Blazor WebAssembly
    -   Performance improve
    -   Component visualize
    -   Lazy loading
    -   CSS isolation

##### What really new in .NET 5

-   Lots of performance improve
    -   30% socket performance
    -   JSON serialization
    -   .NET gRPC
    -   More performance for large cloud scale services at ,icrosoft
-   New libs
    -   \> 90% nullable annotated
    -   System.Text.Json
    -   Significat inves in HTTP/2 and gRPC
    -   Better localization control
    -   HTTP Client convinient API
    -   Cloud native investment
    -   Single file and Trimming
    -   New diagnostics tool
    -   Visual Studio new productivity

## What's new in C# 9.0

-   Top-level statement - no namespace, class for main class
-   init0only setters - no constructor for immutable anymore
-   Records
    ```c#
    puhblic record Money(....) // similar to scala case class
    ```
-   Pattern matching enhancement
-   Target-Typed new expressions
-   Static lambdas / anonymous function
-   Covariant Return Types
-   Target-Types conditional expressions

## Develop/Debug.NET Core with Microsoft Bridge to K8s

## Github Action with .NET

-   Github Action?
    -   Yaml based
    -   Base-As-Code
    -   Visual UI reporting
    -   Packaged with github and github enterprise
-   How can I use it
    -   gitbuh.com
    -   Runners? clould based runner. Can create by your own as well - for enterprise wide, org, repo
-   The rest they are show code example

## Devops & Identity Management with Azure

## Azure static web app with Github Action

-   Offer a few options
    1.  Azure Storage
    2.  Azure App Service

## High-performance services with gRPC

#### HTTP/2

-   Multiplex (solve the request pipline block)
-   Single TCP connection parallellism
-   Binary instead of texttual
-   Header compression
-   Streaming

#### What are REST problems?

-   Rest cannot use some features
-   Textual represented (JSON)
-   Streaming is complicated
-   Bidirection streaming is not possible
-   Need to do Authentication, healthcheck, loadbalance...

#### gRPC

-   Modern high-performance Remote Procedure Call framework
    -   gRPC = HTTP/2 + protobuf
        > protobuf = serialize to binary
    -   Client - gRPC stub
    -   Server -> gRPC service

#### Performance improve in .NET 5

-   Reduce latency, improve concurrency
-   HTTP/2 allocation reduce 92%
-   Kestrel support response header compression

## Securing Blazor WebAssembly

## Entity Framework Core 5.0

-   .NET Standard 2.1
-   Support SQL server, MySQL, Postgresql ....
-   Query using language integrated (LINQ)

#### EF 101

-   Model class
-   Data Context
-   Strongly Type
-   Set an entity's property to update data in database

#### Many to Many

-   Remove link
-   Explicit mapping

#### TPH & TPT

-   Table per hierachy (TPH) - All typs in the hierachy save to single table
-   Table per Type (TPT) - Same each type in hierachy to different table

## UniRx

-   Coroutine isn't good pracetice for async opration
-   Use LINQ-style query operator
-   Improve UI programming - MVRP pattern

## Introduction to AzureForm Recognizer

-   Form recognizer is a cognitive service

#### Form recognizer service

-   Prebuild model
    -   receipt
    -   invoice
    -   business
-   Customer model - Layout API

## Breaking the monolith

p.s. You can watch full video [here](https://www.youtube.com/watch?v=-iQkrvBBcTA&ab_channel=.NETFoundation).

# See ya!
