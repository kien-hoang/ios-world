# Observer Pattern trong Swift iOS

Độ khó: Beginner | Easy | **Normal** | Challenging\
Xcode 14.0.1\
Swift 5.7

***

### 1. Giới thiệu 👋 <a href="#_1-gioi-thieu--0" id="_1-gioi-thieu--0"></a>

Trong bài viết này mình sẽ giới thiệu đến các bạn một Fundamental Design Pattern - Observer Pattern. Vậy Observer Pattern là một pattern như thế nào, ứng dụng và cách cài đặt nó như thế nào?

***

Observer Pattern là một trong những pattern thường gặp nhất trong lập trình iOS. Mình sẽ trình bày một ví dụ về Observer Pattern trước, sau đó chúng ta sẽ đi sâu vào xem bản chất của nó nhé !!!

### 2. Ví dụ về Observer Pattern 😎 <a href="#_2-vi-du-ve-observer-pattern--1" id="_2-vi-du-ve-observer-pattern--1"></a>

* Đầu tiên, mình có 1 class tên là **AppCoordinator**. Class này nhận trách nhiệm điều hướng / quản lý root view cho ứng dụng của chúng ta

```swift
final class AppCoordinator {
    init() {
        NotificationCenter.default.addObserver(self,
                                               selector: #selector(loginSuccess),
                                               name: Notification.Name("LoginSuccessNotification"),
                                               object: nil)
    }
    
    deinit {
        NotificationCenter.default.removeObserver(self)
    }
    
    @objc private func loginSuccess() {
        // Change root view here
        print("Change root view to Home screen for example")
    }
}
```

\=> Nếu thấy khó hiểu về class này thì các bạn cũng có thể thay **AppCoordinator** bằng **AppDelegate**. Nghĩa là mình sẽ đăng ký [_Notification.Name_](http://notification.name/)_("LoginSuccessNotification")_ ở **AppDelegate**

***

* Phần tiếp theo mình sẽ có class **LoginViewController**. Giả sử chúng ta đang ở màn Login, nếu chúng ta đăng nhập thành công thì ứng dụng của chúng ta sẽ điều hướng đến màn hình Home

```swift
final class LoginViewController {
    func login() {
        // Call login API
        DispatchQueue.main.asyncAfter(deadline: .now() + 1) {
            // Login Success
            NotificationCenter.default.post(name: Notification.Name("LoginSuccessNotification"),
                                            object: nil)
        }
    }
}
```

\=> Vì đây là ví dụ nên mình không kế thừa UIViewController cho **LoginViewController**\
\=> Vậy là khi chúng ta đăng nhập thành công. Class **LoginViewController** sẽ gửi thông báo [_Notification.Name_](http://notification.name/)_("LoginSuccessNotification")_.\
Và notification này đã được đăng ký hành động ở **AppCoordinator**. Nghĩa là hàm _loginSuccess()_ của **AppCoordinator** sẽ được gọi và in ra dòng "_Change root view to Home screen for example_"

***

Đó là một ví dụ điển hình cho Observer Pattern mà chúng ta hay dùng trong dự án thật. Vậy bây giờ chúng ta hãy xem qua một tý lý thuyết về nó nhé.

### 3. Observer Pattern là gì? 🤔 <a href="#_3-observer-pattern-la-gi--2" id="_3-observer-pattern-la-gi--2"></a>

* Observer Pattern thuộc nhóm **Behavioral Design Pattern**. Pattern này sẽ cung cấp một cơ chế đăng ký cho đối tượng để phát ra thông báo đến những đối tượng cần thiết khác khi nó (đối tượng được đăng ký) có sự thay đổi.

> * The observer pattern lets one object observe changes on another object - Design Pattern by Tutorials
> * Observer is a behavioral design pattern that lets you define a subscription mechanism to notify multiple objects about any events that happen to the object they’re observing. - Dive Into Design Pattern

***

Đó là một số định nghĩa của Observer Pattern. Mình có sơ đồ của Observer Pattern như sau:![Observer\_Diagram.png](https://images.viblo.asia/c03983f5-fdf5-4735-b0fc-afc0a593d1f4.png)

* Observer Pattern nó sẽ gồm 2 phần chính:
  * **Subscriber**: Là class quan sát (observer) những đối tượng khác, cũng là nơi nhận updates nào đó. Ở ví dụ mục 2, **Subscriber** của chúng ta chính là class **AppCoordinator**. Đây là nơi quan sát toàn bộ ứng dụng của chúng ta, đợi để nhận được một thông báo gọi đến [_Notification.Name_](http://notification.name/)_("LoginSuccessNotification")_ (nghĩa là AppCoordinator đang nhận updates)
  * **Publisher**: Là class được quan sát (observable) và là nơi gửi các updates đến **Subscriber**. Ở ví dụ mục 2, **Publisher** chính là class **LoginViewController**. Vì **LoginViewController** được **AppCoordinator** quan sát, và **LoginViewController** đã gửi thông báo đăng nhập thành công [_Notification.Name_](http://notification.name/)_("LoginSuccessNotification")_ đến **AppCoordinator**

\=> **Note**: **Value** ở sơ đồ chính là thông báo [_Notification.Name_](http://notification.name/)_("LoginSuccessNotification")_ nhé mọi người

### 4. Khi nào thì sử dụng Observer Pattern? ⏳️ <a href="#_4-khi-nao-thi-su-dung-observer-pattern--3" id="_4-khi-nao-thi-su-dung-observer-pattern--3"></a>

* Observer Pattern được sử dụng khi chúng ta muốn một đối tượng (object) thay đổi thì một (hoặc nhiều) đối tượng khác được thay đổi theo.

> * Use the observer pattern whenever you want to receive changes made on another object. - Design Pattern by Tutorials
> * Use the observer pattern when changes to the state of one object may require changing other objects, and the actual set of objects is unknown beforehand or changes dynamically. - Dive Into Design Pattern
> * Use the pattern when some objects in your app must observe others, but only for a limited time or in specific cases. - Dive Into Design Pattern

### 5. Ưu và nhược điểm 🤙 <a href="#_5-uu-va-nhuoc-diem--4" id="_5-uu-va-nhuoc-diem--4"></a>

#### 5.1. Ưu điểm <a href="#_51-uu-diem-5" id="_51-uu-diem-5"></a>

* Đúng với nguyên lý **O** trong SOLID (Open/Closed Principle). Vì khi chúng ta thêm một class **Subscriber**, chúng ta không cần sửa gì trong class **Publisher**. Ví dụ ở mục 2, khi đăng nhập thành công, mình muốn thêm một thông báo chào mừng user (bên cạnh thông báo chuyển root view đã có sẵn), thì mình chỉ cần khai báo thêm một notificaton khác, chứ không cần sửa gì ở class **LoginViewController**
* Chúng ta có thể thêm / xoá những sự liên kết này trong lúc **RUNTIME**. Nghĩa là mình có thể thêm/xoá một notification trong lúc sử dụng app.

#### 5.2. Nhược điểm <a href="#_52-nhuoc-diem-6" id="_52-nhuoc-diem-6"></a>

* **Subscriber** được thông báo không theo một thứ tự nào cả. Ví dụ khi đăng nhập thành công, mình không thể quản lý được việc chuyển root view trước, hay là việc thông báo chào mừng user đăng nhập trước.
* Nếu chúng ta quên xoá những **Subscriber** không còn dùng đến. Nó có thể gây ra hiện tượng leak memory cho ứng dụng của chúng ta. Ví dụ khi chúng ta thêm notification (_NotificationCenter.default.addObserver_) nhưng chúng ta quên xoá những notification (_NotificationCenter.default.removeObserver_) đó

### 6. Kết luận 📔 <a href="#_6-ket-luan--7" id="_6-ket-luan--7"></a>

* Vậy là chúng ta đã tìm hiểu xong Observer Pattern là gì và cách cài đặt nó như thế nào thông qua blog này.
* Nếu các bạn có thắc mắc hay có cách nào tối ưu được ví dụ [Observer Pattern](https://gist.github.com/kien-hoang/74930e56a7867e5940a9bc9077af2e7a) của mình thì đừng ngần ngại hãy comment nhé. Mình cảm ơn các bạn rất nhiều.
* Mình có thêm một ví dụ về Observer Pattern. Các bạn có thể tham khảo tại [link](https://gist.github.com/kien-hoang/df50a4d0cc83ffc901b8ebbf5f06ecae)

***

Ngoài ra có thể đọc thêm 1 số ví dụ về Observer Pattern:

* [https://github.com/ochococo/Design-Patterns-In-Swift#-observer](https://github.com/ochococo/Design-Patterns-In-Swift#-observer)

### Reference 🥳 <a href="#_reference--8" id="_reference--8"></a>

* [Dive Into Design Pattern - Alexander Shvets](https://refactoring.guru/design-patterns/book)
* [Design Pattern by Tutorials - Raywenderlich](https://www.kodeco.com/books/design-patterns-by-tutorials/v3.0)
