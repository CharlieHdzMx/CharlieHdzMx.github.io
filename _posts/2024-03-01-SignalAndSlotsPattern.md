---
layout: post
title: Signals and Slot Design Pattern
excerpt: Signals and Slot Design Pattern Introduction
modified: 1/03/2024, 9:00:24
tags:
  - Cpp
  - Qt
comments: true
category: blog
---
# Introduction

## Signals and Slots
A **signal** is a `void` message defined within a class. It has a list of parameters but no function body.
It resembles a function but is not invoked like a regular function. Instead, it is emitted by an object of the class. 

A **slot** is a `void` function that can be called normally but can also be invoked indirectly by the *QMetaObject* system. A signal from one class can be connected to one or more slots of another class. This requires that the objects exist at the time of the connection (and during its use). Additionally, the parameter lists must match between the signal and the slot; if they do not match, the signal and slot cannot be connected.

