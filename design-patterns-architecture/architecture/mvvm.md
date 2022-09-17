# MVVM

Link tham khảo: [https://www.appypie.com/model-view-controller-mvc-swift](https://www.appypie.com/model-view-controller-mvc-swift)

### Định nghĩa

Là 1 phiên bản nâng cấp của MVC. Model View ViewModel. Mục đích là giúp khả năng kiểm thử và giảm công việc ở Controller

* Controller: Controller được giảm công việc và xem như là View
* View: Vẫn là View
* Model: Vẫn là Model
* ViewModel: Nó là ánh xạ của View về mặt dữ liệu. Là nơi xử lý logic, những business trong app

### Ưu điểm

* Là phiên bản nâng cấp của MVC, nên vẫn duy trì cấu trúc của MVC. Có nghĩa là ta có thể migrate 1 cái app có sẵn từ mô hình MVC sang mô hình MVVM
* Giảm khối lượng code chứa trong Controller
* Đưa các xử lý logic + business vào ViewModel ⇒ Code trở nên dễ hiểu, dễ maintain hơn
* Có khả năng kiểm thử.

### Nhược điểm

* Là mô hình khá phức tạp đối với người bắt đầu. Không phù hợp với những dự án nhỏ.
* Vì tương tự với MVC, nên không thể giải quyết triệt để vấn đề phình code của MVC. Giờ đây code có thể phình to ra ở ViewModel
