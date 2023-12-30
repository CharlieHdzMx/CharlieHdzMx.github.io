---
layout: post
title: MISRA Enum Loop
excerpt: Use of enums for loops with MISRA rules
modified: 5/07/2021, 9:00:24
tags:
  - Qt
  - Cpp
comments: true
category: blog
---
This article discusses the utilization of enum types as loop indexes, highlighting their value in expressing the desired iteration type without conflating conceptual types. Enum types serve to enhance code readability and maintainability by avoiding cumbersome casts and mitigating quality assurance warnings. 

Consider a scenario where a system comprises several power supplies. During the system initialization process, it becomes essential to inspect the states of all power supplies. Opting for an enum type proves superior to macros for iteration purposes. Therefore, a power supply enum type is created to facilitate this process.

`
  /*Available power supplies.*/
  typedef struct
  {
    PS_NORMAL = 0u,
    PS_AUX_1,

`
