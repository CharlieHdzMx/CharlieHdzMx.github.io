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

There is a specific function that allows establishing the connection between a signal and a particular slot, as shown in the following example:

_bool QObject::**connect**(senderQObjectPtr, SIGNAL(signalName(argList)), receiverQObjectPtr, SLOT(slotName(argList)), optConnectionType);_

Any **QObject** with a signal can emit it. Each time the signal is emitted, the connected slots are automatically invoked. The function parameters serve as a way to pass information from the signal to the slots. The _optionalConnectionType_ parameter enables you to specify whether the slot calls should be synchronous (blocking) or asynchronous (queued). 

If necessary, a slot can know which signal was emitted based on the _sender()_ function.

# QObject Lifecycle
Any **QObject** must be created on the heap and not defined as static, as these objects may depend on a QApplication whose lifetime can extend beyond the scope of the main() function, where stack objects might be created.


