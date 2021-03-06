# How to...in playgrounds

## 9. Decode a JSON string

In order to create a simple test for your decodable code:

```swift
import Foundation

// in this case the show name is {"A"-Team}, hence the \\ to espace the {"}
let jsonString = """
{
  "id": "345team",
  "name": "\\"A\\"-Team",
}
"""

struct Show: Decodable {
  let id: String
  let name: String
}

guard let rawData = jsonString.data(using: .utf8),
  let card = try? JSONDecoder().decode(Show.self, from: rawData) else {
    fatalError("couldn't parse the Raw JSON string, the json content must be malformed")
}

print(Show.name)
```

## 8. Decode a JSON file

Useful to provide test data to ViewControllers, when you don't have an internet connection.

Given that you have a `sample-data.json` file in your `Resources` folder

```swift
import Foundation

guard
  let jsonFileURL = Bundle.main.url(forResource: "sample-data", withExtension: "json"),
  let jsonData = try? Data(contentsOf: jsonFileURL),
  let decodedResult = try? JSONDecoder().decode(MyModel.self, from: jsonData) else {
    fatalError("coudln't read the file from the main bundle")
}
```

## 7. Use images in markdown

First, you need to add an image to the `Resource` folder of a playground named `design.png`, then from the code, you can simply add:

```swift
/*:
  ## Inserting a markdown image
  
  ![sample image](design.png)
*/
```

and it will render correctly

## 6. Programatically create an instance of NSViewController

Another one for mac, in playgrounds

```swift
import Cocoa
import PlaygroundSupport

class MyViewController: NSViewController {
  override func loadView() {
    self.view = NSView(
      frame: CGRect(x: 0, y: 0, width: 300, height: 200)
    )
    self.view.wantsLayer = true
    self.view.layer?.backgroundColor = NSColor.blue.cgColor
  }
}

let viewController = MyViewController()

PlaygroundPage.current.liveView = viewController
```

The important part, is that the method `loadView` must be implemented, as using the empty initializer,
the code will throw an error missing the view if `loadView` hasn't been implemented

## 5. Open a image for iOS

Given an image added to the _Resources_ folder, like `tutorial-01.jpg`, I would open it from a playground like:

```swift
let image = UIImage(named: "tutorial-01.jpg")
```

## 4. Open a image for macOS

Given an image added to the _Resources_ folder, like `tutorial-01.jpg`, I would open it from a playground like:

```swift
let fileURL = Bundle.main.url(forResource: "tutorial-01", withExtension: "jpg")!
let image = NSImage(contentsOf: fileURL)
```

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
