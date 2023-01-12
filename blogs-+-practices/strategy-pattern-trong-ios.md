# Strategy Pattern trong iOS

Độ khó: Beginner | **Easy** | Normal | Challenging\
Xcode 14.0.1\
Swift 5.7

***

### 1. Giới thiệu 👋 <a href="#_1-gioi-thieu--0" id="_1-gioi-thieu--0"></a>

Trong bài viết này mình sẽ giới thiệu đến các bạn một Fundamental Design Pattern - Strategy Pattern. Vậy Strategy Pattern là một pattern như thế nào, ứng dụng và cách cài đặt nó như thế nào?

***

Theo cá nhân mình thì học Design Pattern khá là khô khan và khó hiểu. Nên mình sẽ không nói về lý thuyết/định nghĩa ngay. Mình sẽ trình bày một ví dụ về Strategy Pattern trước, sau đó chúng ta sẽ đi sâu vào xem bản chất của nó nhé !!!

### 2. Ví dụ về Strategy Pattern 😎 <a href="#_2-vi-du-ve-strategy-pattern--1" id="_2-vi-du-ve-strategy-pattern--1"></a>

* Đầu tiên, mình có 1 protocol tên là **Strategy** có một hàm _execute_ với 2 tham số a và b. Còn làm gì với 2 tham số này thì protocol **Strategy** không quan tâm nhé (Tất nhiên rồi, vì đã được protocol này trừu tượng hoá rồi)

```swift
protocol Strategy {
    func execute(a: Int, b: Int)
}
```

***

* Phần tiếp theo mình sẽ có 3 class conform / implement protocol **Strategy**:

```swift
class ConcreteStrategyAdd: Strategy {
    func execute(a: Int, b: Int) {
        print("a + b = \(a + b)")
    }
}

class ConcreteStrategySubtract: Strategy {
    func execute(a: Int, b: Int) {
        print("a - b = \(a - b)")
    }
}

class ConcreteStrategyMultiply: Strategy {
    func execute(a: Int, b: Int) {
        print("a * b = \(a * b)")
    }
}
```

\=> Nhìn vào code trên thì chúng ta cũng sẽ hiểu được tác dụng của 3 hàm trên là gì phải không mọi người. Gồm 3 hàm mô tả: tổng 2 số a và b, hiệu 2 số a và b, tích 2 số a và b.

***

* Và chúng ta có 1 phần cuối cùng là class **Context** chứa protocol **Strategy**. Lưu ý quan trọng là class **Context** chứa protocol **Strategy** chứ không hề chứa 1 trong 3 class cụ thể **ConcreteStrategy** nhé.

```swift
class Context {
    private var strategy: Strategy?

    func setStrategy(_ strategy: Strategy) {
        self.strategy = strategy
    }

    func executeStrategy(a: Int, b: Int) {
        strategy?.execute(a: a, b: b)
    }
}
```

***

* Bây giờ chúng ta đã có đủ 3 thành phần cấu tạo nên Strategy Pattern, mình sẽ trình bày cách sử dụng pattern này cho mọi người ngay sau đây:

```swift
let context = Context()
context.setStrategy(ConcreteStrategyAdd())
context.executeStrategy(a: 10, b: 5)           // a + b = 15

context.setStrategy(ConcreteStrategySubtract())
context.executeStrategy(a: 10, b: 5)           // a - b = 5

context.setStrategy(ConcreteStrategyMultiply())
context.executeStrategy(a: 10, b: 5)           // a * b = 50
```

* Từ ví dụ trên chúng ta có thể hình dung qua về pattern này rồi phải không. Cùng là 1 class **Context**, nhưng khi được áp dụng "chiến thuật" cộng 2 số a và b (_ConcreteStrategyAdd_) thì khi chạy hàm _executeStrategy_ của class **Context**, chúng ta sẽ được kết quả là: a + b = 15
* Đồng thời, cũng trong lúc chạy class **Context** này, nếu mình áp dụng "chiến thuật" khác cho class. Ví dụ như "chiến thuật" hiệu 2 số a và b (_ConcreteStrategySubtract_), hoặc "chiến thuật" lấy tích 2 số a và b (_ConcreteStrategyMultiply_). Chúng ta sẽ được kết quả tương ứng như example code của mình nhé.

### 3. Strategy Pattern là gì? 🤔 <a href="#_3-strategy-pattern-la-gi--2" id="_3-strategy-pattern-la-gi--2"></a>

* Strategy Pattern thuộc nhóm **Behavioral Design Pattern**. Nó sẽ định nghĩa nhiều đối tượng (mà những đối tượng này có thể hoán đổi cho nhau) ở những class khác nhau. Đồng thời có thể thay đổi cho nhau trong run time của chương trình.

> * The strategy pattern defines a family of interchangeable objects that can be set or switched at runtime - Design Pattern by Tutorials
> * Strategy is a behavioral design pattern that lets you define a family of algorithms, put each of them into a separate class, and make their objects interchangeable - Dive Into Design Pattern

***

Đó là một số định nghĩa của Strategy Pattern. Nếu mọi người vẫn thấy khó hiểu thì mình sẽ giải thích sơ qua như thế này:![Strategy\_Diagram.png](https://images.viblo.asia/14e4e222-105a-4f6c-a2c0-95902d7eb24d.png)

* Strategy Pattern nó sẽ gồm 3 phần (như ảnh hoặc ví dụ ở phần 2):
  * Một protocol **Strategy** chứa những hàm mà chúng ta cần sử dụng (tương ứng với ô vuông "_<\<Protocol>> Strategy Protocol_")
  * Một class **Context** chứa protocol **Strategy** ở trên (tương ứng với ô vuông "_Object using a Strategy_"
  * Và những class conform **Strategy** (những class này gọi là **Concrete Strategy** là những class có những hành vi/chiến thuật khác nhau). Chúng cần conform **Strategy** để có thể truyền vào class **Context** ở trên. Và vì chúng đều conform protocol **Strategy** nên chúng có thể thay đổi / hoán đổi cho nhau trong runtime của chương trình\


\=> **Note**: từ khoá quan trọng trong Strategy Pattern là những "chiến thuật" (**Concrete Strategy**) này có thể thay đổi trong **RUNTIME** nhé mọi người.

### 4. Khi nào thì sử dụng Strategy Pattern? ⏳️ <a href="#_4-khi-nao-thi-su-dung-strategy-pattern--3" id="_4-khi-nao-thi-su-dung-strategy-pattern--3"></a>

* Sử dụng Strategy Pattern khi bạn có 2 hay nhiều biến thể trong cùng 1 đối tượng (những biến thể đó có các hành vi khác nhau). Và những biến thể đó có thể thay đổi cho nhau trong runtime.

> * Use the strategy pattern when you have two or more different behaviors that are interchangeable. - Design Pattern by Tutorials
> * Use the Strategy pattern when you want to use different variants of an algorithm within an object and be able to switch from one algorithm to another during runtime. - Dive Into Design Pattern

### 5. Ưu và nhược điểm 🤙 <a href="#_5-uu-va-nhuoc-diem--4" id="_5-uu-va-nhuoc-diem--4"></a>

#### 5.1. Ưu điểm <a href="#_51-uu-diem-5" id="_51-uu-diem-5"></a>

* Có thể thay đổi các thuật toán / hành vi trong đối tượng trong suốt giai đoạn runtime.
* Có thể tách biệt việc triển khai chi tiết (implementation) của thuật toán / hành vi ra khỏi code sử dụng nó ( ở ví dụ 2 chúng ta đã tách việc implementation - 3 class **Concrete** ra khỏi code sử dụng nó - class **Context**)
* Khi chúng ta dùng Strategy Pattern, chúng ta đã sử dụng composition thay cho inheritance
* Đúng với nguyên lý Open/Closed Principle (nguyên lý O trong SOLID). Khi chúng ta muốn thêm 1 thuật toán / hành vi, chúng ta sẽ không cần sửa dụng thuật toán / hành vi đã có.

#### 5.2. Nhược điểm <a href="#_52-nhuoc-diem-6" id="_52-nhuoc-diem-6"></a>

* Nếu chúng ta chỉ có 2 hoặc ít thuật toán / hành vi, và chúng không thay đổi trong quá trình runtime. Thì không nên sử dụng Strategy Pattern nhé, việc sử dụng Strategy Pattern trong trường hợp này sẽ làm phức tạp hoá vấn đề không cần thiết
* Người dùng / lập trình viên phải biết + hiểu được sự khác nhau giữa các thuật toán / hành vi để chọn cho đúng thuật toán / hành vi để sử dụng.
* Việc tạo thêm class để tạo ra thuật toán / hành vi nhiều khi không cần thiết. Vì trong một số ngôn ngữ đã có hàm ẩn danh (anonymous function) để giúp thực hiện việc này.

### 6. Kết luận 📔 <a href="#_6-ket-luan--7" id="_6-ket-luan--7"></a>

* Vậy là chúng ta đã tìm hiểu xong Strategy Pattern là gì và cách cài đặt nó như thế nào thông qua blog này.
* Nếu các bạn có thắc mắc hay có cách nào tối ưu được ví dụ [Strategy Pattern](https://gist.github.com/kien-hoang/5871f7c3f87980881680e08f84ed174d) của mình thì đừng ngần ngại hãy comment nhé. Mình cảm ơn các bạn rất nhiều.

***

Ngoài ra có thể đọc thêm 1 số ví dụ về Strategy Pattern:

* [https://github.com/ochococo/Design-Patterns-In-Swift#-strategy](https://github.com/ochococo/Design-Patterns-In-Swift#-strategy)
* [https://medium.com/@JuanpeCatalan/strategy-pattern-in-swift-1462dbddd9fe](https://medium.com/@JuanpeCatalan/strategy-pattern-in-swift-1462dbddd9fe)
* [https://refactoring.guru/design-patterns/strategy/swift/example](https://refactoring.guru/design-patterns/strategy/swift/example)

### Reference 🥳 <a href="#_reference--8" id="_reference--8"></a>

* [Dive Into Design Pattern - Alexander Shvets](https://refactoring.guru/design-patterns/book)
* [Design Pattern by Tutorials - Raywenderlich](https://www.kodeco.com/books/design-patterns-by-tutorials/v3.0)
