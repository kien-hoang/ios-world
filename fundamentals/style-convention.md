# Style convention

## Delegates

Khi tạo các hàm custom delegate, cái gốc (delegate source) nên được ẩn đi.

Nên:

```swift
func namePickerView(_ namePickerView: NamePickerView, didSelectName name: String)
func namePickerViewShouldReload(_ namePickerView: NamePickerView) -> Bool
```

Không nên:

```swift
func didSelectName(namePicker: NamePickerViewController, name: String)
func namePickerShouldReload() -> Bool
```

## **Use Type Inferred Context (Sử dụng bối cảnh suy luận)**

Trình biên dịch (compiler) có thể tự suy luận ra ngữ cảnh phù hợp để giúp code ngắn và rõ ràng hơn.

Nên:

```swift
let selector = #selector(viewDidLoad)
view.backgroundColor = .red
let toView = context.view(forKey: .to)
let view = UIView(frame: .zero)
```

Không nên:

```swift
let selector = #selector(ViewController.viewDidLoad)
view.backgroundColor = UIColor.red
let toView = context.view(forKey: UITransitionContextViewKey.to)
let view = UIView(frame: CGRect.zero)
```

## **Code Organization**

Sử dụng extensions để tổ chức code theo từng khối logic chức năng (logical blocks of functionality). Nên thêm // MARK: - để thể hiện chức năng của khối lệnh đó.

### **Protocol Conformance**

Khi thêm protocol thì cũng nên chia ra theo từng khối extension.

Nên:

```swift
class MyViewController: UIViewController {
  // class stuff here
}

// MARK: - UITableViewDataSource
extension MyViewController: UITableViewDataSource {
  // table view data source methods
}

// MARK: - UITableViewDelegate
extension MyViewController: UITableViewDelegate {
  // table view delegate methods
}
```

Không nên:

```swift
class MyViewController: UIViewController, UITableViewDataSource, UITableViewDelegate {
  // all methods
}
```

### **Unused Code**

Bỏ những dòng code không sử dụng. Ví dụ như code do template tự tạo ra, hoặc là những placeholder comment khi tạo file mới.

Nên:

```swift
override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  return Database.contacts.count
}
```

Không nên:

```swift
override func didReceiveMemoryWarning() {
  super.didReceiveMemoryWarning()
  // Dispose of any resources that can be recreated.
}

override func numberOfSections(in tableView: UITableView) -> Int {
  // #warning Incomplete implementation, return the number of sections
  return 1
}

override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  // #warning Incomplete implementation, return the number of rows
  return Database.contacts.count
}
```

⇒ Lưu ý: Không cần thêm hàm numberOfSections. Vì mặc định của UITableView nó sẽ có 1 section rồi. Không cần hàm didReceiveMemoryWarning vì chúng ta không sử dụng gì đến nó.

### **Minimal Imports**

Chỉ import những thư viện cần thiết

Nên:

```swift
import UIKit
var view: UIView
var deviceModels: [String]
```

Không nên:

```swift
import Foundation
import UIKit
var view: UIView
var deviceModels: [String]
```

⇒ Trong UIKit đã có Foundation rồi. Nếu import cả Foundation và UIKit thì sẽ bị duplicate.

### **Spacing**

Nên:

```swift
if user.isHappy {
  // Do something
} else {
  // Do something else
}
```

Không nên:

```swift
if user.isHappy
{
  // Do something
}
else {
  // Do something else
}
```

\======

Nên:

```swift
let user = try await getUser(
  for: userID,
  on: connection)
```

Không nên:

```swift
let user = try await getUser(
  for: userID,
  on: connection
)
```

\======

Nên:

```swift
class TestDatabase: Database {
  var data: [String: CGFloat] = ["A": 1.2, "B": 3.2]
}
```

Không nên:

```swift
class TestDatabase : Database {
  var data :[String:CGFloat] = ["A" : 1.2, "B":3.2]
}
```

⇒ Chú ý trước và sau dấu “:”
