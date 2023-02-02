# Memento Pattern trong Swift iOS

Độ khó: Beginner | Easy | Normal | **Challenging**\
Xcode 14.0.1\
Swift 5.7

***

### 1. Giới thiệu 👋 <a href="#_1-gioi-thieu--0" id="_1-gioi-thieu--0"></a>

Trong bài viết này mình sẽ giới thiệu đến các bạn một Fundamental Design Pattern - Memento Pattern. Vậy Memento Pattern là một pattern như thế nào, ứng dụng và cách cài đặt nó như thế nào?

***

Như thường lệ, mình sẽ không nói về lý thuyết/định nghĩa ngay. Mình sẽ trình bày một ví dụ về Memento Pattern trước, sau đó chúng ta sẽ đi sâu vào xem bản chất của nó nhé !!!

### 2. Đặt vấn đề 🤔 <a href="#_2-dat-van-de--1" id="_2-dat-van-de--1"></a>

Mình có một đoạn code như sau:

```swift
class BankAccount {
    private(set) var balance: Double = 0.0

    func setBalance(balance: Double) {
        self.balance = balance
    }
}

class BankCustomer {
    let account: BankAccount

    init(account: BankAccount) {
        self.account = account
    }

    func withdraw(amount: Double) {
        account.setBalance(balance: account.balance - amount)
    }

    func deposit(amount: Double) {
        account.setBalance(balance: account.balance + amount)
    }
}
```

* Trong đoạn code trên, mình có 2 class là `BankCustomer` và `BankAccount`. Mỗi khách hàng sẽ có một tài khoản ngân hàng và số dư riêng. Dễ hiểu chứ nhỉ?
* Khi chúng ta rút tiền thì hàm _withdraw(amount:)_ sẽ được gọi đến.
* Mọi thứ trông có vẻ ổn? Vậy vấn đề ở đây là gì nhỉ?
* Giả sử mình muốn chuyển tiền đến cho người thân, sau khi hệ thống gọi đến hàm _withdraw(amount:)_ và số dư của mình đã bị trừ đi. Tuy nhiên mình đã nhập sai số tài khoản của người thân, từ đây hệ thống phải trả lại số tiền mình đã gửi cho mình đúng không nhỉ? Vậy là code phía trên chưa làm được việc restore lại số dư cho mình rồi
* Hoặc giả sử mình chỉ có 50 đồng trong tài khoản nhưng mình muốn rút đến 100 đồng. Sau khi hệ thống trừ 100 đồng thì số dư của mình không đủ. Vậy giao dịch rút 100 đồng đó là không hợp lệ, nên hệ thống cần phải restore lại số tiền là 50 đồng cho tài khoản của mình.

\=> Đó cũng là lý do chúng ta có Memento Pattern để giúp restore lại trạng thái trước đó của một object và vẫn không vi phạm nguyên lý đóng gói (Encapsulation)\
\=> Cùng mình áp dụng Memento Pattern vào example code phía trên nhé

### 3. Ví dụ về Memento Pattern 😎 <a href="#_3-vi-du-ve-memento-pattern--2" id="_3-vi-du-ve-memento-pattern--2"></a>

Sau đây là ví dụ về Memento Pattern của 2 class `BankCustomer` và `BankAccount` :

```swift
protocol Originator {
    func save() -> Memento
    func restore(memento: Memento)
}

class BankAccount: Originator {
    private(set) var balance: Double = 0.0

    func setBalance(balance: Double) {
        self.balance = balance
    }

    func save() -> Memento {
        return Memento(balance: balance)
    }

    func restore(memento: Memento) {
        balance = memento.balance
    }
}

class Memento {
    let balance: Double

    init(balance: Double) {
        self.balance = balance
    }
}

class BankCustomer {
    let account: BankAccount

    init(account: BankAccount) {
        self.account = account
    }

    func withdraw(amount: Double) {
        let memento = account.save()
        account.setBalance(balance: account.balance - amount)
        if account.balance < 0 {
            account.restore(memento: memento)
            print("Insufficient funds.")
        }
    }

    func deposit(amount: Double) {
        account.setBalance(balance: account.balance + amount)
    }
}
```

* Có thể hơi khó hiểu phải không mọi người 😁. Tuy nhiên đừng lo lắng, chúng ta sẽ tìm hiểu từng thành phần trong Memento Pattern ngay sau đây.
* Trước đó hãy thử chạy phần code có sử dụng Memento Pattern trước nhé.

```swift
let account = BankAccount()
let customer = BankCustomer(account: account)
print("Balance: \(customer.account.balance)")     // Balance: 0.0

print("Deposit 100")                              // Deposit 100
customer.deposit(amount: 100)
print("Balance: \(customer.account.balance)")     // Balance: 100.0

print("Withdraw 50")                              // Withdraw 50
customer.withdraw(amount: 50)
print("Balance: \(customer.account.balance)")     // Balance: 50.0

print("Withdraw 100")                             // Withdraw 100
customer.withdraw(amount: 100)                    // Insufficient funds.
print("Balance: \(customer.account.balance)")     // Balance: 50.0
```

\=> Ở lần withdraw cuối cùng, số dư (balance) trong tài khoản của mình chỉ còn 50 đồng. Tuy nhiên mình lại thực hiện lệnh rút 100 đồng. Nên hệ thống sẽ in ra thông báo: "**Insufficient funds**", và restore lại số dư là 50 đồng cho mình\
\=> Đã đúng mục đích của chúng ta nhỉ. Tuyệt vời, phải không?

***

Vậy là chúng ta đã xem qua một ví dụ về việc sử dụng Memento Pattern rồi. Chúng ta sẽ đến với phần định nghĩa / lý thuyết của Memento Pattern nhé

### 3. Memento Pattern là gì? 🤔 <a href="#_3-memento-pattern-la-gi--3" id="_3-memento-pattern-la-gi--3"></a>

* Memento Pattern thuộc nhóm **Behavioral Design Pattern**. Bởi vì pattern này là pattern nói về hành động lưu và khôi phục dữ liệu. Nó sẽ giúp chúng ta lưu và khôi phục trạng thái trước đó của một object mà không làm tiết lộ cách mà chúng ta lưu hoặc lấy lại.

> * Memento is a behavioral design pattern that lets you save and restore the previous state of an object without revealing the details of its implementation. - Dive Into Design Pattern

***

Đó là một số định nghĩa của Memento Pattern. Chúng ta có sơ đồ như sau:![Memento\_Diagram.png](https://images.viblo.asia/8e1e0fab-0620-4ba2-9fbc-ef8f1c1fcd2a.png)

* Memento Pattern nó sẽ gồm 3 phần:
  * **Originator**: Là một class mà chúng có thể lưu (chúng ta hay gọi là snapshot) lại trạng thái của nó, hoặc khôi phục (restore) trạng thái nếu cần. Ở ví dụ mục 2, `BankAccount` là **Originator** của chúng ta.
  * **Memento**: Là nơi chứa những property mà chúng ta cần lưu lại. Chúng ta nên cho class **Memento** này immutable (không thể thay đổi) và chỉ truyền data 1 lần duy nhất thông qua hàm khởi tạo (init). Ở ví dụ mục 2, `Memento` của chúng ta cần lưu lại _balance_
  * **Care Taker**: Là nơi lưu và khôi phục **Memento** từ **Originator**. **Care Taker** sẽ biết được lúc nào và tại sao (when and why) cần lưu lại giá trị gốc và khi nào thì cần được khôi phục giá trị gốc.

### 4. Khi nào thì sử dụng Memento Pattern? ⏳️ <a href="#_4-khi-nao-thi-su-dung-memento-pattern--4" id="_4-khi-nao-thi-su-dung-memento-pattern--4"></a>

* Như đã nói ở trên, chúng ta sử dụng Memento Pattern để lưu và khôi phục trạng thái của một object/class nào đó. Ngoài ra nó còn giúp chúng ta không vi phạm nguyên tắc bao đóng (Encapsulation)

> * Use the memento pattern whenever you want to save and later restore an object’s state. - Design Pattern by Tutorials
> * Use the memento pattern when you want to produce snap-shots of the object’s state to be able to restore a previous state of the object - Dive Into Design Pattern
> * Use the pattern when direct access to the object’s fields/get- ters/setters violates its encapsulation. - Dive Into Design Pattern

### 5. Ưu và nhược điểm 🤙 <a href="#_5-uu-va-nhuoc-diem--5" id="_5-uu-va-nhuoc-diem--5"></a>

#### 5.1. Ưu điểm <a href="#_51-uu-diem-6" id="_51-uu-diem-6"></a>

* Giúp chúng ta lưu và khôi phục trạng thái trước đó của object/class
* Giúp chúng ta không bị vi phạm nguyên tắc bao đóng (Encapsulation)

#### 5.2. Nhược điểm <a href="#_52-nhuoc-diem-7" id="_52-nhuoc-diem-7"></a>

* Ứng dụng có thể sử dụng rất nhiều bộ nhớ bởi vì người dùng có thể tạo Memento liên tục (ví dụ chúng ta lưu lại snapshot quá nhiều chẳng hạn)
* "Caretakers should track the originator’s lifecycle to be able to destroy obsolete mementos." - Dive Into Design Pattern

### 6. Kết luận 📔 <a href="#_6-ket-luan--8" id="_6-ket-luan--8"></a>

* Vậy là chúng ta đã tìm hiểu xong Memento Pattern là gì và cách cài đặt nó như thế nào thông qua blog này.
* Nếu các bạn có thắc mắc hay có cách nào tối ưu được ví dụ [Memento Pattern](https://gist.github.com/kien-hoang/60e026d1f26342e22a9690373be9f2bc) của mình thì đừng ngần ngại hãy comment nhé. Mình cảm ơn các bạn rất nhiều.

***

Một số ví dụ hay các bạn có thể tham khảo:

* [https://github.com/ochococo/Design-Patterns-In-Swift#-memento](https://github.com/ochococo/Design-Patterns-In-Swift#-memento)
* [https://code.tutsplus.com/courses/swift-design-patterns/lessons/memento](https://code.tutsplus.com/courses/swift-design-patterns/lessons/memento)

### Reference 🥳 <a href="#_reference--9" id="_reference--9"></a>

* [Dive Into Design Pattern - Alexander Shvets](https://refactoring.guru/design-patterns/book)
* [Design Pattern by Tutorials - Raywenderlich](https://www.kodeco.com/books/design-patterns-by-tutorials/v3.0)
