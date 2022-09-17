# SOLID

[Link thảm khảo](<solid (1).md>)

1. The Single Responsibility Principle (SRP): 1 class chỉ nên chịu 1 trách nhiệm duy nhất
2. The Open-Closed Principle (OCP): class phải dễ mở rộng nhưng khó chỉnh sửa. Nói đơn giản là bạn có thể thêm function vào class nhưng khó sửa các function hiện tại.
3. The Liskov Substitution Principle (LSP): nôm na là class con nên có thể thay thế class cha mà không phá vỡ logic của ứng dụng
4. The Interface Segregation Principle (ISP): Nguyên lý này nêu lên vấn đề interface quá to, khi kế thừa interface (trong swift thì là protocol) sẽ có những property/method không cần thiết ⇒ Chia ra những interface nhỏ hơn và chỉ kế thừa những interface cần thiết
5. The Dependency Inversion Principle (DIP):
   1. High-level modules should not depend on low-level modules. Both should depend on abstractions. Mô đun cấp cao không được phụ thuộc vào mô đun cấp thấp, cả hai phải phụ thuộc vào một định nghĩa trìu tượng hay một giao diện (interface)
   2. Abstractions should not depend upon details. Details should depend upon abstractions. Định nghĩa trìu tượng (hoặc interface) không được phụ thuộc vào mô đun cụ thể mà ngược lại mô đun cụ thể phải phụ thuộc vào khung trìu tượng.

{% hint style="info" %}
Hệ thống điện không nên phụ thuộc vào cái máy sấy tóc mà cái máy sấy tóc nên phụ thuộc vào hệ thống điện. Module cấp cao (hệ thống điện) cần một định nghĩa trìu tượng (ổ cắm) để máy sấy tóc có thể cắm vào (máy sấy tóc là một mô đun cụ thể của interface ổ cắm) tức là máy sấy tóc muốn cắm được vào ổ cắm thì nó phải tuân thủ chuẩn ổ cắm, tất cả các thiết bị khác tuân thủ chuẩn ổ cắm đều cắm được vào hệ thống điện. Đó chính là lý do tại sao Dependency Inversion Principle có tên gọi là Nguyên lý đảo ngược sự phụ thuộc.
{% endhint %}
