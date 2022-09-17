# Struct vs Class

Chúng ta có thể liệt kê một số điểm giống nhau giữa Struct và Class trong Swift như sau:

* Công dụng: Giúp cấu hình/định nghĩa 1 object.
* Properties: struct và class đều có property (thuộc tính).
* Method: cả struct và class đều có method (phương thức).
* Initializier: struct và class hỗ trợ các phương thức khởi tạo mặc định, hoặc các phương thức khởi tạo tuỳ chỉnh theo mục đích và logic của lập trình viên.
* Subscript: Struct và class hỗ trợ các subscript syntax (câu lệnh con bên trong một thuộc tính, hoặc một hàm).
* Extension: Extension là khái niệm mới trong Swift, nó giúp lập trình viên có thể mở rộng các struct, hoặc class sẵn có hoặc đã được xây dựng từ trước.

Khác nhau:

* Type (kiểu): struct là value type còn class là reference type.
* Inheritance (kế thừa): Struct không thể kế thừa, còn class thì có thể ( hiển nhiên – OOP mà).
* Deinitializers: Struct không có hàm huỷ (destructor trong java/C++), chỉ có hàm khởi tạo initializer (constructor trong java/C++), còn Class thì có đầy đủ
* Multiple reference (đa tham chiếu): chúng ta có thể có nhiều đối tượng cùng tham chiếu đến 1 class instance, còn ở struct thì không thể (vì nó là value type mà).
