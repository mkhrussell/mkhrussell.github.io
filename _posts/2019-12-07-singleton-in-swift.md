---
title: "Singleton Pattern in Swift"
excerpt: "How to create Singleton classes in Swift."
date: 2019-12-07T21:05:24+00:00
author: Kamrul Hasan
classes: wide
categories:
  - blog
tags:
  - swift
  - singleton
  - design pattern
---

**Singleton** pattern is very widely used pattern. The purpose of creating a Singleton is to ensure that a class has only one instance.

Creating a singleton class is pretty easy in *Swift* programming language. There must be a *static* class property which will hold the globally accessible instance and *initializer* must be be private so that no other class can create instance directly.

Let's say we have to create a Logger class for our app. And in the lifecycle of the app there will be only one instance of the Logger class. In that case our solution could be like below:

```swift
class Logger {
    static let shared = Logger()
    
    private init() {}
    
    func log(tag: String, message: String) {
        print("\(tag): \(message)")
    }
}


// MARK: Test
let logger1 = Logger.shared
logger1.log(tag: "Logger1", message: "Instance 1!")

let logger2 = Logger.shared
logger2.log(tag: "Logger2", message: "Is logger1 and logger2 same? => \(logger1 === logger2) ")

let logger3 = Logger.shared
logger2.log(tag: "Logger3", message: "Is logger2 and logger3 same? => \(logger2 === logger3) ")

/*
Output:

Logger1: Instance 1!
Logger2: Is logger1 and logger2 same? => true 
Logger3: Is logger2 and logger3 same? => true

*/
```

You can check [apple developer documentation here](https://developer.apple.com/documentation/swift/cocoa_design_patterns/managing_a_shared_resource_using_a_singleton) for more information.
{: .notice--info}
