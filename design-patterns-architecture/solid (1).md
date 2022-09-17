# SOLID

## **1. Tổng quan**

**SOLID** là viết tắt của 5 chữ cái đầu trong 5 nguyên tắc thiết kế hướng đối tượng. Giúp cho lập trình viên viết ra những đoạn code dễ đọc, dễ hiểu, dễ maintain. Nó được đưa ra bởi [Robert C. Martin](http://www.goodreads.com/author/show/45372.Robert\_C\_Martin) và Michael Feathers. 5 nguyên tắc đó bao gồm:

* **S**ingle responsibility priciple (SRP)
* **O**pen/Closed principle (OCP)
* **L**iskov substitution principe (LSP)
* **I**nterface segregation principle (ISP)
* **D**ependency inversion principle (DIP)

## **2. Các nguyên tắc**

### The Single Responsibility Principle (SRP)

> There should never be more than one reason for a class to change. In other words, every class should have only one responsibility.

Nguyên lý này nói rằng mỗi lớp chỉ nên chịu một trách nhiệm (chịu 1 công việc) cụ thể nào đó mà thôi.

Do đó mỗi lần chúng ta tạo/sửa một class. Phải luôn tự hỏi trong lớp này đã đảm nhiệm bao nhiêu vai trò rồi?

Hãy xem ví dụ sau:

```swift
class Handler {

    func handle() {
        let data = requestDataToAPI()
        let array = parse(data: data)
        saveToDB(array: array)
    }

    private func requestDataToAPI() -> Data {
        // send API request and wait for the response
    }

    private func parse(data: Data) -> [String] {
        // parse the data and create the array
    }

    private func saveToDB(array: [String]) {
        // save the array in a database (CoreData/Realm/...)
    }
}
```

Sau khi mọi người đọc đoạn code trên, thì mọi người thấy class trên chịu trách nhiệm làm bao nhiêu công việc?

Class _**Handler**_ chịu trách nhiệm lấy data từ API (1), parse data sang kiểu mảng string (2) và lưu data vào database (3). Áp dụng vào dự án thực tế, chúng ta dùng Alamofire để call API (1), ObjectMapper để parse data (2) và dùng CoreData để lưu data vào database (3). Đến lúc đó, code ở ví dụ sẽ trở nên cồng kềnh, khó bảo trì vào mở rộng.

Cách xử lý:

```swift
class Handler {

    let apiHandler: APIHandler
    let parseHandler: ParseHandler
    let dbHandler: DBHandler

    init(apiHandler: APIHandler, parseHandler: ParseHandler, dbHandler: DBHandler) {
        self.apiHandler = apiHandler
        self.parseHandler = parseHandler
        self.dbHandler = dbHandler
    }

    func handle() {
        let data = apiHandler.requestDataToAPI()
        let array = parseHandler.parse(data: data)
        dbHandler.saveToDB(array: array)
    }
}

class APIHandler {

    func requestDataToAPI() -> Data {
        // send API request and wait the response
    }
}

class ParseHandler {

    func parse(data: Data) -> [String] {
        // parse the data and create the array
    }
}

class DBHandler {

    func saveToDB(array: [String]) {
        // save the array in a database (CoreData/Realm/...)
    }
}
```

Nguyên lý này giúp class của bạn clean nhất có thể. Ngoài ra, ở ví dụ đầu tiên, bạn không thể test được _**requestDataToAPI**_, _**parse**_ and _**saveToDB**_ một cách trực tiếp, vì nó là các private methods. Sau khi sửa lại code, chúng ra có thể dễ dàng testing các hàm này.

### The Open-Closed Principle (OCP)

> Software entities ... should be open for extension, but closed for modification.

Nguyên lý nói rằng chúng ta không nên sửa đổi class có sẵn, chỉ nên mở rộng nó.

Nếu bạn muốn tạo một lớp dễ bảo trì, thì phải có 2 điều kiện quan trọng sau đây:

* Open for extension: Có thể dễ dàng thêm và thay đổi hành vi (behaviours) của class đó 1 cách dễ dàng.
* Close for modification: Không được thay đổi những hành vi đã có sẵn của class.

Chúng ta có ví dụ sau, class _**Logger**_ có nhiệm vụ in ra chi tiết những lớp _**Car**_

```swift
class Logger {

    func printData() {
        let cars = [
            Car(name: "Batmobile", color: "Black"),
            Car(name: "SuperCar", color: "Gold"),
            Car(name: "FamilyCar", color: "Grey")
        ]

        cars.forEach { car in
            print(car.printDetails())
        }
    }
}

class Car {
    let name: String
    let color: String

    init(name: String, color: String) {
        self.name = name
        self.color = color
    }

    func printDetails() -> String {
        return "I'm \(name) and my color is \(color)"
    }
}
```

Vậy nếu chúng ta muốn class _**Logger**_ in thêm chi tiết của lớp mới khác, thì chúng ta phải thay đổi hàm _**printData()**_ mỗi lần như vậy. Điều này vi phạm nguyên lý OCP mà chúng ta đang giới thiệu.

Ví dụ mình sẽ thêm 1 class _**Bicycle**_. Mọi người sẽ thấy mình phải thay đổi lại hàm _**printData()**_ của class _**Logger**_

```swift
class Logger {

    func printData() {
        let cars = [
            Car(name: "Batmobile", color: "Black"),
            Car(name: "SuperCar", color: "Gold"),
            Car(name: "FamilyCar", color: "Grey")
        ]

        cars.forEach { car in
            print(car.printDetails())
        }

        let bicycles = [
            Bicycle(type: "BMX"),
            Bicycle(type: "Tandem")
        ]

        bicycles.forEach { bicycles in
            print(bicycles.printDetails())
        }
    }
}

class Car {
    let name: String
    let color: String

    init(name: String, color: String) {
        self.name = name
        self.color = color
    }

    func printDetails() -> String {
        return "I'm \(name) and my color is \(color)"
    }
}

class Bicycle {
    let type: String

    init(type: String) {
        self.type = type
    }

    func printDetails() -> String {
        return "I'm a \(type)"
    }
}
```

**Cách giải quyết:** Chúng ta sẽ tạo ra 1 protocol **Printable**, những class phương tiện (_**Car**_, _**Bicycle**_) sẽ conform protocol này. Mà hàm _**printData()**_ sẽ in ra 1 mảng của **Printable**

Bằng cách đó, chúng ta đã tạo thêm 1 lớp trừu tượng nằm giữa _**printData()**_ và lớp cần in dữ liệu. Cho phép thêm những lớp mới (ví dụ như _**Bicycle**_) mà không cầ phải thay đổi code trong hàm _**printData()**_

```swift
protocol Printable {
    func printDetails() -> String
}

class Logger {

    func printData() {
        let cars: [Printable] = [
            Car(name: "Batmobile", color: "Black"),
            Car(name: "SuperCar", color: "Gold"),
            Car(name: "FamilyCar", color: "Grey"),
            Bicycle(type: "BMX"),
            Bicycle(type: "Tandem")
        ]

        cars.forEach { car in
            print(car.printDetails())
        }
    }
}

class Car: Printable {
    let name: String
    let color: String

    init(name: String, color: String) {
        self.name = name
        self.color = color
    }

    func printDetails() -> String {
        return "I'm \(name) and my color is \(color)"
    }
}

class Bicycle: Printable {
    let type: String

    init(type: String) {
        self.type = type
    }

    func printDetails() -> String {
        return "I'm a \(type)"
    }
}
```

### The Liskov Substitution Principle (LSP)

> Functions that use pointers or references to base classes must be able to use objects of derived classes without knowing it

Nguyên lý này nói rằng: nếu class A là class con của class B. Thì những hàm trong class A phải thực hiện những hành động giống với class B. Để hiểu hơn nguyên lý này. Chúng ta hãy xem ví dụ sau:

```swift
class Rectangle {
    var width: Int
    var height: Int

    init(width: Int, height: Int) {
        self.width = width
        self.height = height
    }

    func area() -> Int {
        return width * height
    }
}

class Square: Rectangle {
    override var width: Int {
        didSet {
            super.height = width
        }
    }

    override var height: Int {
        didSet {
            super.width = height
        }
    }
}
```

Chúng ta có class _**Rectangle**_ và 1 hàm tính diện tích (chiều dài nhân chiều rộng), class _**Square**_ vì là hình vuông nên chúng ta có chiều dài bằng chiều rộng.

Chúng ta hãy xem cách tính diện tích của class _**Square**_:

```swift
func main() {
    let square = Square(width: 10, height: 10)

    let rectangle: Rectangle = square

    rectangle.height = 7
    rectangle.width = 5

    print(rectangle.area())
    // As a rectangle we should expect the area as 7 x 5 = 35, but we got 5 x 5 = 25
}
```

Theo nguyên lý **LSP**, vì class _**Square**_ kế thừa từ class _**Rectangle**_. Nên hàm _**area()**_ phải luôn có giá trị bằng chiều dài nhân với chiều rộng (ở đây là 7x5 = 35). Tuy nhiên chúng ta lại nhận được diện tích là 25. Do đó, ví dụ trên đã vi phạm nguyên lý **LSP** này.

**Cách giải quyết:** Chúng ta sử dụng protocol _**Geometrics**_ chứa hàm tính diện tích. Vậy class _**Square**_ không còn kế thừa từ class _**Rectangle**_ nữa. Mà cả 2 class này kế thừa từ protocol.

```swift
protocol Geometrics {
    func area() -> Int
}

class Rectangle: Geometrics {
    var width: Int
    var height: Int

    init(width: Int, height: Int) {
        self.width = width
        self.height = height
    }

    func area() -> Int {
        return width * height
    }
}

class Square: Geometrics {
    var edge: Int

    init(edge: Int) {
        self.edge = edge
    }

    func area() -> Int {
        return edge * edge
    }
}
```

```swift
func main() {
    let rectangle: Geometrics = Rectangle(width: 10, height: 5)
    print(rectangle.area()) // 10*5 = 50

    let rectangle2: Geometrics = Square(edge: 5)
    print(rectangle2.area()) // 5*5 = 25
}
```

### The Interface Segregation Principle (ISP)

> Many client-specific interfaces are better than one general-purpose interface.

Nguyên lý này giới thiệu 1 trong những vấn đề của lập trình hướng đối tượng: interface quá to (the fat interface). Interface quá to, nghĩa là trong interface đó (ở trong lập trình iOS có thể hiểu interface là protocol) có quá nhiều hàm/thuộc tính. Nó chứa nhiều thông tin không cần thiết. Hãy xem ví dụ sau:

Chúng ta có protocol _**GestureProtocol**_ với hàm **didTap()**

```swift
protocol GestureProtocol {
    func didTap()
}
```

Sau 1 thời gian chúng ta làm việc, thì protocol này phình to ra. Như sau:

```swift
protocol GestureProtocol {
    func didTap()
    func didDoubleTap()
    func didLongPress()
}
```

Class _**SuperButton**_ của chúng ta cần cả 3 hàm trên, nên nó sẽ đúng khi chúng ta inform (kế thừa) _**SuperButton**_ với _**GestureProtocol**_

```swift
class SuperButton: GestureProtocol {
    func didTap() {
        // send tap action
    }

    func didDoubleTap() {
        // send double tap action
    }

    func didLongPress() {
        // send long press action
    }
}
```

Tuy nhiên, bắt đầu phát sinh 1 vấn đề khác là: chúng ta có 1 class _**PoorButton**_. Và class này chỉ cần duy nhất 1 hàm _**didTap()**_

```swift
class PoorButton: GestureProtocol {
    func didTap() {
        // send tap action
    }

    func didDoubleTap() { }

    func didLongPress() { }
}
```

Vậy là class _**PoorButton**_ này bỏ trống 2 hàm _**didDoubleTap()**_ và _**didLongPress()**_, nghĩa là chúng ta đã truyền bị dư 2 hàm không sử dụng cho class _**PoorButton**_. Do đó đã vi phạm nguyên lý **ISP** chúng ta đang đề cập ở đây.

**Cách giải quyết:** Đưa những hàm này ra thành các protocol riêng lẻ. Và chỉ truyền những protocol cần thiết.

```swift
protocol TapProtocol {
    func didTap()
}

protocol DoubleTapProtocol {
    func didDoubleTap()
}

protocol LongPressProtocol {
    func didLongPress()
}

class SuperButton: TapProtocol, DoubleTapProtocol, LongPressProtocol {
    func didTap() {
        // send tap action
    }

    func didDoubleTap() {
        // send double tap action
    }

    func didLongPress() {
        // send long press action
    }
}

class PoorButton: TapProtocol {
    func didTap() {
        // send tap action
    }
}
```

### The Dependency Inversion Principle (DIP)

> 1. High level modules should not depend upon low level modules. both should depend upon abstractions.
> 2. Abstractions should not depend upon details. details should depend upon abstractions.

Nguyên lý này khá quan trọng đối với lập trình viên. Và nó cũng liên quan đến **Dependency Injection** (DJ). Vậy **Dependency Inversion** (DI) là gì? Và DJ là gì? Giữa DI và DJ có mối quan hệ như thế nào? Mình sẽ giới thiệu ở bài blog sau.

## **3. Kết luận**

Nếu bạn làm theo nguyên lý SOLID,  bạn có thể tăng chất lượng code của mình. Code của chúng ta sẽ trở nên dễ đọc, dễ bảo trì và dễ mở rộng hơn cho sau này. Tuy nhiên, để áp dụng nguyên lý SOLID vào dự án thực tế sẽ có nhiều khó khăn hơn. Nhưng vì thế, chúng ta càng phải nên nắm vững kiến thức nền tảng này để có thể áp dụng và sửa đổi khi làm dự án thực tế nhé.

[SOLID Principles Applied To Swift](https://www.marcosantadev.com/solid-principles-applied-swift/)

Tham khảo:

[https://www.marcosantadev.com/solid-principles-applied-swift/](https://www.marcosantadev.com/solid-principles-applied-swift/)

[https://topdev.vn/blog/solid-la-gi/](https://topdev.vn/blog/solid-la-gi/)

[https://en.wikipedia.org/wiki/SOLID](https://en.wikipedia.org/wiki/SOLID)

[https://medium.com/movile-tech/liskov-substitution-principle-96f15559e363](https://medium.com/movile-tech/liskov-substitution-principle-96f15559e363)
