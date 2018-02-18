# How to...in playgrounds

## 3. Write unit tests

Tip taken from john sundell's [Writing unit tests in Swift playgrounds][john-testing-playground] post

```swift
import Foundation
import XCTest

class MyTestClass: XCTestCase {
  func testSomething() {
    // write test here
  }
}

class TestObserver: NSObject, XCTestObservation {
  func testCase(_ testCase: XCTestCase,
                  didFailWithDescription description: String,
                  inFile filePath: String?,
                  atLine lineNumber: Int) {
    assertionFailure(description, line: UInt(lineNumber))
  }
}

let testObserver = TestObserver()
XCTestObservationCenter.shared.addTestObserver(testObserver)
MyTestClass.defaultTestSuite.run()
```

## 2. Test an instance of UIView

When attempting to preview an instance of `UIView`, it's important to call `view.layoutIfNeeded()` otherwise the view would never layout on Playgrounds.

```swift
let view = MyCustomView()
view.frame = CGRect(x: 0, y: 0, width: 300, height: 150)
view.layoutIfNeeded()
```

## 1. Use a custom font

First, add the `my-custom-font.otf` file to the `Resources` folder of the playground, then within the playground, you can use the font as follows:

```swift
let fontName = "my-custom-font"
let fontURL = Bundle.main.url(forResource: fontName, withExtension: "otf")
CTFontManagerRegisterFontsForURL(fontURL! as CFURL, .process, nil)

let uiFontName = "MyCustomFont-Bold"
let customFont = UIFont(name: uiFontName, size: 16.0)
```

[john-testing-playground]: https://www.swiftbysundell.com/posts/writing-unit-tests-in-a-swift-playground
