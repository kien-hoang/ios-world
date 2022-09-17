# Memory leaks

Link tham khảo: [https://ali-akhtar.medium.com/all-about-memory-leaks-in-ios-cdd450d0cc34](https://ali-akhtar.medium.com/all-about-memory-leaks-in-ios-cdd450d0cc34)

### Định nghĩa

Memory leak là hiện tượng bộ nhớ không được giải phóng bởi ARC bởi khi không thể biết được cái không gian bộ nhớ này có đang còn được sử dụng hay không.

### Các rule để tránh memory leak

* Tại sao lại bị memory leak:
  * Khi 2 đối tượng giữ strong reference đến nhau. Gây ra hiện tượng không bên nào có thể được giải phóng cả. ⇒ Rule 1: Khi 2 đối tượng có liên kết với nhau, luôn sử dụng weak hoặc unowned
  * Memory lead ở closure. Closure là 1 khối lệnh, nếu khối lệnh này giữ strong reference đến lớp khác thì có thể dẫn đến không bên nào được giải phóng ⇒ Sử dụng weak và unowned.
* Khác nhau giữa weak và unowned:
  * weak: không phải là 1 strong reference, có thể bằng nil. Sử dụng weak khi không chắc khi closure thực hiện thì reference có tồn tại hay không
  * unowned: không phải là 1 strong reference, không thể bằng nil. Sử dụng unowned khi chắc chắn closure luôn có thể thực hiện

⇒ Rule 2: Khi bạn có 1 lớp và có 1 closure trong lớp đó. Nếu ở closure truy cập vào bất cứ cái gì ở lớp đó thì phải dùng weak/unowned
