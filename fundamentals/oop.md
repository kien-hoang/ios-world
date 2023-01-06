# OOP

### Tính kế thừa (Inheritance)

Sub-class có thể kế thừa toàn bộ các property, method của base-class. Trong swift không ủng hộ việc đa kế thừa bằng class

### Tính đóng gói (Encapsulation)

Trong 1 class có thể hạn chế phạm vi truy xuất đến các property/method thuộc class ra bên ngoài. Trong swift thường sử dụng access modifier như là keyword để hạn chế điều này

### Tính đa hình (Polymorphism)

Tính đa hình thể hiện ở việc 1 object có thể đóng vai trò ở nhiều hình dạng. Trong swift, có thể override method như là 1 cách thể hiện tính đa hình

### Tính trừu tượng (Abstraction)

1 lớp có thể là sự trừu tượng hoá, mang những cốt lõi cần thiết về property/method cho các lớp con. Phục vụ cho việc kế thừa đúng bản chất. POP về bản chết cũng được xem như 1 cách trừu tượng hoá, vì POP sinh ra để phục vụ cho việc tách rời các thành phần khỏi sự thực thi ( tức là ko bao gồm implement, trừ khi extension protocol bên trong Swift chúng ta có cung cấp default implementation)
