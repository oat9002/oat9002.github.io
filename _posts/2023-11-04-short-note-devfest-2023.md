---
layout: post
title: "Short note from DevFest Bangkok 2023"
---

## Hi

Welcome back to my blog. For this topic, I want to write in English so please bare with me a bit lol. Today, I have a chance to join developer event called **Devfest Bangkok 2023** which is set up by GDG Bangkok. This is my very first time join to this event. By the way, because I don't really use Flutter or create application. So in some session which relates to that I don't really much play attention to it lol. Let's see what we have.

## Sessions

There are 10 sessions

1. View transition API the next generation of web transition
2. Better UX on the Web with Core Web Vital
3. Firestore Advanced Queries
4. Build a generative AI chatbot with PaLM API and Cloud Functions for Firebase 2nd Gen
5. Ship faster with Feature Flags
6. Managing design system in Flutter
7. Make for Large, Make for Fold with Flutter
8. Diversity in AI
9. Test Strategy for Kotlin Multiplatform Apps

## View transition API the next generation of web transition

- Pain point for web developer is transition between page in web
- Not as smooth as mobile app
- There are some restricition
  - Only support SPA
  - Safariy has not been supported yet T_T

## Better UX on the Web with Core Web Vital

- UX -> Look, Feel, **Usability**
- Loading Performance, Visual Stability, Input Responsiveness
  - LCP <= 2.5 s
  - CLS <= 0.1 s
  - FID <= 100 ms
  - INP <= 200 ms
- We can use Web Vital chrome extension to check these things
  - page speed inside
  - chrome ux report
  - unlighthouse
- Ensure LCP has prioritize in resource
- Use CDN to optimize doc and resource TTFB
- Ensure web is eligible for bfcache
- Avoid animation/transition which uses css-induction
- Avoid or break up long task
- Void unnecessary javascripot
- Avoid large rendering update

## Firestore Advanced Quries

- One view, one collection

## Build a Generative AI Chatbot with PaLM API and Cloud functions for Firebase 2nd Gen

- Firebase extension
  - Group of lib
  - transaction is one of this lib
- Palm API extension
  - Vertex AI - wrapper from google
  - Chat bison

## Ship faster with Feature flag

- How to deploy code under development
- Deploy Vs Release
  - Deploy -> user get new binary
  - Release -> user get access to new feature
- **Done** means **Release**, not just work on my machine
- Feature flag
  - Allow us to frequently merge code to master without exposing it
- Testing
  - from master
  - enabled flag for tester
- Monitoring
  - Small change between release easy to debug and fix

## Managing design system in Flutter

- Why need design system?
  - Common language
  - Speed up development
  - Ensure consistency and familarity
- Colors (Don't use Primary and Secondary palette directly)
  - Primary palette
  - Secondary palette
  - Usage palette

## Make for large Make for fold with Flutter

- 23% more on iOS
- 9% more on Android
- Large Screen
  - Continuity
    - Rotations
    - Folding-Unfold
    - Multi-Window mode
- For the best app experience
  - No orientation restriction
  - Resizable

## Diversity in AI

- Bias
  - Age
  - Gender
  - Race
- They are from **Data**
- Resulting in unfaire product
- How to make AI fair
  - Aware of bias(es)
  - LIT (Language Interpreting tool) in NLP
  - Know your data
- Responsible AI
  - AI should be
    - Transpaency
    - Fairness
    - Accountability
    - Privacy

> Trust is hard to earn, easy to lose

## Testing Strategy for Kotlin multiplatform projects

- I haven't had any project on Kotlin so I didn't get much out of it lol.
