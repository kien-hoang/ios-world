# Access Levels

Link tham khảo: [https://stackoverflow.com/a/24012515](https://stackoverflow.com/a/24012515)

### \`open\` và \`public\`

Cả 2 keywords trên đều cho phép chúng ta truy cập nó từ bên trong target nơi nó được define (define module) và cả bên ngoài - những module khác.

* **open**: classes and class members can be subclassed and overridden both **within** and **outside** the defining module (target).

```swift
// First.framework – A.swift

open class A {}
```

```swift
// Second.framework – C.swift

import First

internal class C: A {} // ok
```

* **public**: classes and class members can only be subclassed and overridden **within** the defining module (target). Allow members from other modules to use them, but NOT to override them

```swift
// First.framework – B.swift

public class B: A {} // ok
```

```swift
// Second.framework – D.swift

import First

internal class D: B {} // error: B cannot be subclassed
```

### \`internal\` - default access level

Chỉ cho phép chúng ta truy cập nó từ bên trong target nơi nó đực define (define module). Là default level, nếu không khai báo access level => mặc định là internal

### \`fileprivate\`

Chỉ được truy cập trong cùng 1 file code.

```swift
// First.framework – A.swift

internal struct A {
    fileprivate static let x: Int
}

A.x // ok
```

```swift
// First.framework – B.swift

A.x // error: x is not available
```

### \`private\`

Chỉ được truy cập trong cùng 1 class (bao gồm extensions trong cùng 1 file với class đó)

Extensions khác file cũng không truy cập được

```swift
// Declaring "A" class that has the two types of "private" and "fileprivate":
class A {
    private var aPrivate: String?
    fileprivate var aFileprivate: String?

    func accessMySelf() {
        // this works fine
        self.aPrivate = ""
        self.aFileprivate = ""
    }
}

// Declaring "B" for checking the abiltiy of accessing "A" class:
class B {
    func accessA() {
        // create an instance of "A" class
        let aObject = A()

        // Error! this is NOT accessable...
        aObject.aPrivate = "I CANNOT set a value for it!"

        // this works fine
        aObject.aFileprivate = "I CAN set a value for it!"
    }
}
```
