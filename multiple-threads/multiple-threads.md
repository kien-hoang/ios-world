# Multiple Threads

### Giới thiệu

Có 2 loại thread chính: 1 Main thread và nhiều background thread. Main thread dùng để khởi tạo và quản lý UI. Background thread dùng để xử lý những task nặng hoặc lâu.

### GCD

Là 1 API cấp thấp dùng để quản lý 1 loạt các hoạt động (có thể đồng bộ hoặc bất đồng bộ). Cũng có thể sử dụng để xử lý các task nặng ở background thread để trải nghiệp người dùng tốt hơn. GCD cung cấp những thứ sau:

* DispatchQueue: Cho phép làm việc trên main thread, background thread, concurrent queue hoặc serial queue.
* DispatchGroup: Cho phép tracking công việc của nhiều task 1 lúc. Có thể biết được lúc nào thì mọi task đã hoàn thành
* Semaphore: Có tối đa bao nhiêu thread được làm việc cùng 1 lúc.
* Barrier: Tạo ra 1 điểm đồng bộ trong concurrent queue

### OperationQueue

Operation: Sẽ đóng gói và thực hiện 1 task.

OperationQueue: Là nơi chứa nhiều operation. Operation có thể huỷ (cancel). Cũng có thể kiểm tra trạng thái của task đó như thế nào (Đang làm, làm xong rồi).
