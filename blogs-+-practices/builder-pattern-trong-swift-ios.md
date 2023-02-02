# Builder Pattern trong Swift iOS

Độ khó: Beginner | Easy | **Normal** | Challenging\
Xcode 14.0.1\
Swift 5.7

***

### 1. Giới thiệu 👋 <a href="#_1-gioi-thieu--0" id="_1-gioi-thieu--0"></a>

Trong bài viết này mình sẽ giới thiệu đến các bạn một Fundamental Design Pattern - Builder Pattern. Vậy Builder Pattern là một pattern như thế nào, ứng dụng và cách cài đặt nó như thế nào?

***

Như thường lệ, mình sẽ không nói về lý thuyết/định nghĩa ngay. Mình sẽ trình bày một ví dụ về Builder Pattern trước, sau đó chúng ta sẽ đi sâu vào xem bản chất của nó nhé !!!

### 2. Đặt vấn đề 🤔 <a href="#_2-dat-van-de--1" id="_2-dat-van-de--1"></a>

Mình có một ví dụ về việc khởi tạo 1 URLRequest phục vụ cho việc call API `https://api.themoviedb.org/3/movie/upcoming?api_key=7cdeb275d08259b817a3b80a7ff85f15` như sau:

```swift
class URLRequestHelper {
    private var components = URLComponents()
    private var httpMethod: String = "GET"
    private var headers: [String: String] = [:]
    private var body: [String: Any]?

    init(baseUrl: String = "https://api.themoviedb.org",
         path: String = "",
         queryItems: [String: String] = [:],
         httpMethod: String = "GET",
         headers: [String: String] = [:],
         body: [String: Any]? = nil) {
        
        components = URLComponents(string: baseUrl)!
        
        var path = path
        if !path.hasPrefix("/") {
            path = "/" + path
        }
        components.path = path
        
        components.queryItems = queryItems.map { key, value in
            URLQueryItem(name: key, value: "\(value)")
        }
        components.percentEncodedQuery = components.percentEncodedQuery?.replacingOccurrences(of: "+", with: "%2B")
        
        self.httpMethod = httpMethod
        
        self.headers = headers
        
        self.body = body
    }
    
    func build() -> URLRequest? {
        guard let url = components.url else { return nil }
        
        var request = URLRequest(url: url)
        request.httpMethod = httpMethod
        request.setValue("application/json", forHTTPHeaderField: "Content-Type")
        request.setValue("application/json", forHTTPHeaderField: "Accept")
        headers.forEach { request.addValue($0.value, forHTTPHeaderField: $0.key) }
        
        if let body = body {
            let jsonData = try? JSONSerialization.data(withJSONObject: body, options: [])
            request.httpBody = jsonData
        }
        
        return request
    }
}
```

Các bạn đừng lo lắng khi không thể hiểu được hết toàn bộ phần code phía trên. Nội dung chính của code trên là chúng ta có một class `URLRequestHelper` có một hàm _build()_ trả về một URLRequest cho chúng ta. Chúng ta chỉ cần lấy kết quả (URLRequest này) để thực hiện call API:

```swift
let request = URLRequestHelper(path: "3/movie/upcoming",
                               queryItems: ["api_key": "7cdeb275d08259b817a3b80a7ff85f15"])
    .build()
let task = URLSession.shared.dataTask(with: request!) { data, response, error in
    if error != nil {
        print(error!)
    } else {
        if let returnData = String(data: data!, encoding: .utf8) {
            print(returnData)
        }
    }
}
task.resume()
```

Vậy là chúng ta sẽ nhận được danh sách các bộ phim từ API `https://api.themoviedb.org/3/movie/upcoming?api_key=7cdeb275d08259b817a3b80a7ff85f15`

***

Vậy class `URLRequestHelper` của chúng ta có vấn đề gì hay không? Và tại sao mình lại đề cập đến trong bài viết này.\
\=> Tất nhiên class `URLRequestHelper` của chúng ta hoàn toàn ổn, chúng ta có thể sử dụng nó trong hầu hết các dự án.\
Giả sử ở một dự án nào đó, mình muốn thêm **cachePolicy** (URLRequest.CachePolicy) hoặc **timeoutInterval** (TimeInterval),... thì hàm init của chúng ta sẽ như thế nào?

```swift
init(baseUrl: String = "https://api.themoviedb.org",
     path: String = "",
     queryItems: [String: String] = [:],
     httpMethod: String = "GET",
     headers: [String: String] = [:],
     body: [String: Any]? = nil,
     cachePolicy: URLRequest.CachePolicy = .useProtocolCachePolicy,
     timeoutInterval: TimeInterval = 20) {
}
```

Nhìn hàm init vẫn cứ ổn đúng không?

* Giả sử mình còn cần thêm nhiều tham số khác thì sao? Như thế hàm init của chúng ta sẽ trở nên phức tạp rất nhiều phải không nhỉ?
* Trường hợp khác là mình chỉ cần dùng **queryItems** (như ví dụ call API movie phía trên) thì sao? (trong khi chúng ta cần cung cấp nhiều tham số cho hàm init)\
  \=> Tất nhiên vì chúng ta đã có `default value` nên chúng ta chỉ cần truyền những tham số cần thiết (đó là lý do Builder Pattern có thể là anti-pattern, tuy nhiên chúng ta sẽ thảo luận về phần này sau nhé)\


\=> Đó là lý do chúng ta có Builder Pattern để giúp giảm thiểu sự phức tạp của hàm khởi tạo (init) của chúng ta\
\=> Cùng mình áp dụng Builder Pattern vào example code phía trên nhé

### 3. Ví dụ về Builder Pattern 😎 <a href="#_3-vi-du-ve-builder-pattern--2" id="_3-vi-du-ve-builder-pattern--2"></a>

Vẫn là ví dụ về việc tạo ra một URLRequest để call API `https://api.themoviedb.org/3/movie/upcoming?api_key=7cdeb275d08259b817a3b80a7ff85f15` nhé:

```swift
class URLRequestBuilder {
    private var components = URLComponents()
    private var httpMethod: String = "GET"
    private var headers: [String: String] = [:]
    private var body: [String: Any]?


    init(baseUrl: String = "https://api.themoviedb.org") {
        components = URLComponents(string: baseUrl)!
    }

    func setPath(_ path: String) -> URLRequestBuilder {
        var path = path
        if !path.hasPrefix("/") {
            path = "/" + path
        }
        components.path = path
        return self
    }

    func setQueryItems(_ queryItems: [String: String]) -> URLRequestBuilder {
        components.queryItems = queryItems.map { key, value in
            URLQueryItem(name: key, value: "\(value)")
        }
        components.percentEncodedQuery = components.percentEncodedQuery?.replacingOccurrences(of: "+", with: "%2B")
        return self
    }

    func setHTTPMethod(_ method: String) -> URLRequestBuilder {
        httpMethod = method
        return self
    }

    func setHeaders(_ headers: [String: String]) -> URLRequestBuilder {
        self.headers = headers
        return self
    }

    func setBody(_ body: [String: Any]) -> URLRequestBuilder {
        self.body = body
        return self
    }

    func build() -> URLRequest? {
        guard let url = components.url else { return nil }

        var request = URLRequest(url: url)
        request.httpMethod = httpMethod
        request.setValue("application/json", forHTTPHeaderField: "Content-Type")
        request.setValue("application/json", forHTTPHeaderField: "Accept")
        headers.forEach { request.addValue($0.value, forHTTPHeaderField: $0.key) }

        if let body = body {
            let jsonData = try? JSONSerialization.data(withJSONObject: body, options: [])
            request.httpBody = jsonData
        }

        return request
    }
}
```

Dễ hiểu hơn nhiều rồi đúng không mọi người 😁. Mình có thể lấy URLRequest từ hàm _build()_ như sau:

```swift
let request = URLRequestBuilder()
    .setPath("3/movie/upcoming")
    .setQueryItems(["api_key": "7cdeb275d08259b817a3b80a7ff85f15"])
    .build()
```

***

Quay lại vấn đề ở phần 2, giả sử chúng ta cần thêm những tham số khác để xây dựng URLRequest ví dụ: **cachePolicy** (URLRequest.CachePolicy) hoặc **timeoutInterval** (TimeInterval),..\
Chúng ta chỉ cần tạo thêm những hàm tương ứng, ví dụ:

```swift
func setTimeoutInterval(_ timeoutInterval: TimeInterval) -> URLRequestBuilder {
    self.timeoutInterval = timeoutInterval
    return self
}
```

Rất đơn giản phải không mọi người. Không những vậy, Builder Pattern còn giúp chúng ta tuân thủ nguyên tắc S (trong SOLID) - mỗi hàm chỉ có một nhiệm vụ duy nhất.

***

Vậy là chúng ta đã xem qua một ví dụ về việc sử dụng Builder Pattern rồi. Chúng ta sẽ đến với phần định nghĩa / lý thuyết của Builder Pattern nhé

### 3. Builder Pattern là gì? 🤔 <a href="#_3-builder-pattern-la-gi--3" id="_3-builder-pattern-la-gi--3"></a>

* Builder Pattern thuộc nhóm **Creational Design Pattern**. Nó sẽ giúp chúng ta tạo ra một đối tượng phức tạp bằng cách cung cấp từng tham số, đồng thời chúng còn tăng tính tái sử dụng của các hàm cung cấp tham số.

> * The builder pattern allows you to create complex objects by providing inputs step- by-step, instead of requiring all inputs upfront via an initializer - Design Pattern by Tutorials
> * Builder is a creational design pattern that lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction code - Dive Into Design Pattern

***

Đó là một số định nghĩa của Builder Pattern. Chúng ta có sơ đồ như sau:![Builder\_Diagram.png](https://images.viblo.asia/7ea20528-bf25-42e1-a9ee-e4fa5ccd69c1.png)

* Builder Pattern nó sẽ gồm 3 phần:
  * **Product**: Là class/object phức tạp mà chúng ta cần khởi tạo (ở ví dụ 2 thì **Product** của chúng ta là **URLRequest**)
  * Một class **Builder**: Là nơi chứa những hàm cung cấp tham số và 1 hàm _build()_ trả về **Product** cho chúng ta (ở đây là trả về **URLRequest**)
  * **Director**: Nó có thể là một **UIViewController** hoặc một class **Helper**. Là nơi chúng ta khởi tạo class **Builder** để nhận được **Product** (trong ví dụ 2 thì **Director** của mình là nơi thực hiện phần code cung cấp _path_ và _queryItems_)

### 4. Khi nào thì sử dụng Builder Pattern? ⏳️ <a href="#_4-khi-nao-thi-su-dung-builder-pattern--4" id="_4-khi-nao-thi-su-dung-builder-pattern--4"></a>

* Như đã nói ở trên, chúng ta sử dụng Builder Pattern để giảm thiểu sự phức tạp khi khởi tạo một class/object phức tạp và chúng ta không cần quan tâm thứ tự cung cấp tham số (như ví dụ 2 thì chúng ta có thể cung cấp _path_ trước _queryItems_ hoặc ngược lại)

> * Use the builder pattern when you want to create a complex object using a series of steps. - Design Pattern by Tutorials
> * Use the Builder pattern to get rid of a "telescopic constructor" - Dive Into Design Pattern
> * Use the Builder pattern when you want your code to be able to create different representations of some product - Dive Into Design Pattern
> * Use the Builder to construct Composite trees or other complex objects - Dive Into Design Pattern

### 5. Ưu và nhược điểm 🤙 <a href="#_5-uu-va-nhuoc-diem--5" id="_5-uu-va-nhuoc-diem--5"></a>

#### 5.1. Ưu điểm <a href="#_51-uu-diem-6" id="_51-uu-diem-6"></a>

* Chúng ta có thể khởi tạo object theo từng bước và không quan tâm thứ tự của các bước đó
* Có thể sử dụng lại các hàm cung cấp tham số
* Đúng với nguyên tắc S (trong SOLID) - _Single Responsibility Principle_

#### 5.2. Nhược điểm <a href="#_52-nhuoc-diem-7" id="_52-nhuoc-diem-7"></a>

* Độ phức tạp của code có thể tăng khi mà chúng ta cần tạo nhiều class mới (cung cấp thêm nhiều tham số => độ phức tạp của class _Builder_ tăng)

### 6. Kết luận 📔 <a href="#_6-ket-luan--8" id="_6-ket-luan--8"></a>

* Vậy là chúng ta đã tìm hiểu xong Builder Pattern là gì và cách cài đặt nó như thế nào thông qua blog này.
* Nếu các bạn có thắc mắc hay có cách nào tối ưu được ví dụ [Builder Pattern](https://gist.github.com/kien-hoang/65f95ddf93d52ccfb56aa5b2833bb444) của mình thì đừng ngần ngại hãy comment nhé. Mình cảm ơn các bạn rất nhiều.

***

Một số ví dụ hay các bạn có thể tham khảo:

* Class Builder trong [MVP-R](https://viblo.asia/p/model-view-presenter-router-mvp-r-trong-ios-GAWVpdm5V05)
* [https://www.swiftyvn.com/2019/05/builder-design-pattern/](https://www.swiftyvn.com/2019/05/builder-design-pattern/)
* [https://theswiftdev.com/swift-builder-design-pattern/](https://theswiftdev.com/swift-builder-design-pattern/)
* [https://magz.techover.io/2021/08/02/design-pattern-builder-pattern-trong-ios/](https://magz.techover.io/2021/08/02/design-pattern-builder-pattern-trong-ios/)
* [https://github.com/ochococo/Design-Patterns-In-Swift#-builder](https://github.com/ochococo/Design-Patterns-In-Swift#-builder)

### Reference 🥳 <a href="#_reference--9" id="_reference--9"></a>

* [Dive Into Design Pattern - Alexander Shvets](https://refactoring.guru/design-patterns/book)
* [Design Pattern by Tutorials - Raywenderlich](https://www.kodeco.com/books/design-patterns-by-tutorials/v3.0)
