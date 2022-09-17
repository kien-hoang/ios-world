# MVC

Link tham khảo: [https://www.appypie.com/model-view-controller-mvc-swift](https://www.appypie.com/model-view-controller-mvc-swift)

### Định nghĩa

&#x20;Là 1 mẫu kiến trúc Model-View-Controller, trong đó:

* Model: Là nơi chứa những đối tượng, dữ liệu mình tự định nghĩa, đóng gói các đối tượng và logic của dữ liệu đó.
* View: Là đối tượng mà user có thể nhìn thấy, có thể thao tác lên đó.
* Controller: Là đối tượng nằm giữa Model và View. Nhận trách nhiệm quản lý điều khiển mọi logic giữa View và Model. Ví dụ như là nhận action từ View rồi xử lý dữ liệu ở Model

### Ưu nhược điểm

* Có 2 luồn chính trong MVC:
  * View ⇒ Controller ⇒ Model: Khi user thao tác trên View dẫn đến data thay đổi ở Model.
  * Model ⇒ Controller ⇒ View: Khi data thay đổi ở Model và View được thông báo để thay đổi theo cho phù hợp.

#### Ưu điểm:

* Là kiến trúc phổ biến và dễ sử dụng nhất trong lập trình iOS
* Nó giúp mình thiết kế hệ thống 1 cách rõ ràng hơn. Giúp mình định nghĩa luồng dữ liệu 1 cách rõ ràng

#### Nhược điểm

* Controller nhận quá nhiều trách nhiệm, dẫn đến phình to controller. Hầu như mọi thứ trong app đều được xử lý ở controller ⇒ Chỉ sử dụng trong dự án vừa và nhỏ.
* Khó kiểm thử. Vì View và Controller trong iOS dính quá chặt với nhau. Nguyên nhân: Controller (UIViewController) quản lý life cycle của View.
