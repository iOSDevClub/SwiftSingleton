SwiftSingleton
==============

An exploration of the Singleton pattern in Swift. All approaches below support lazy initialization and thread safety.

Issues and pull requests welcome.

### Approach A: Global constant

```swift
let _SingletonASharedInstance = SingletonA()

class SingletonA  {

    class var sharedInstance : SingletonA {
        return _SingletonASharedInstance
    }
    
}
```
We use a global constant because class constants are not yet supported.

This approach supports lazy initialization because Swift lazily initializes global constants (and variables), and is thread safe by the definition of `let`.

### Approach B: Nested struct

```swift
class SingletonB {
    
    class var sharedInstance : SingletonB {
        struct Static {
            static let instance : SingletonB = SingletonB()
        }
        return Static.instance
    }
    
}
```

Unlike classes, structs do support static constants. By using a nested struct we can leverage its static constant as a class constant.

### Approach C: dispatch_once

The traditional Objective-C approach ported to Swift.

```swift
class SingletonC {
    
    class var sharedInstance : SingletonC {
        struct Static {
            static var onceToken : dispatch_once_t = 0
            static var instance : SingletonC? = nil
        }
        dispatch_once(&Static.onceToken) {
            Static.instance = SingletonC()
        }
        return Static.instance!
    }
}
```
