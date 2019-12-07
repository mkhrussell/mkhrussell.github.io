---
title: "Live View for SwiftUI in Xcode Playground"
date: 2019-12-06T17:23:21+00:00
author: Kamrul Hasan
categories:
  - blog
tags:
  - SwiftUI
  - Xcode Playground
  - Live View
  - swiftui
  - liveview
  - playground
---

In WWDC 2019, Apple announced SwiftUI, and innovative UI Library for iOS. SwiftUI's declarative syntax and simplicity made many developers fan of this new libray instantly.

In this article, I have demonstrated how you can enable live view of a SwiftUI view in Xcode playground.

![image1](/assets/images/swiftui-playground-liveview/image1.png)

1. You have to import __PlaygroundSupport__ package
2. Then set your SwiftUI view as live view by calling _setLiveView_ method of PlaygroundPage

```swift
import PlaygroundSupport

import SwiftUI

struct ContentView: View {
    var body: some View {
        VStack {
            Text("Hello!")
            Button(action: {
                print("Clicked")
            }, label: { Text("Click Me!") })
        }
    }
}


PlaygroundPage.current.setLiveView(ContentView())
```

{:start="3"}
3. Now click on "Adjust Editior Options" button
4. Then select "Live View" from the popup menu

![image2](/assets/images/swiftui-playground-liveview/image2.png)

{:start="5"}
5. "Adjust Editior Options" button's icon will change

![image3](/assets/images/swiftui-playground-liveview/image3.png)

{:start="6"}
6. Click on "Play" button to run the Playground file 

![image4](/assets/images/swiftui-playground-liveview/image4.png)

{:start="7"}
7. Live View will appear in preview area in the right side

![image5](/assets/images/swiftui-playground-liveview/image5.png)

{:start="8"}
8. You can even click on the button from the Live View. Here output of the print statement of button's click event appears in the debug area.

Download [SwiftUI.playground.zip](/assets/files/swiftui-playground-liveview/SwiftUI.playground.zip).
