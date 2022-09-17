# Singleton

Định nghĩa: Tạo ra 1 instance duy nhất và có thể truy cập mọi nơi trong app.

Vấn đề:

* Có thể gặp vấn đề Read-Write problem. (Dữ liệu chưa write xong, đã có nơi khác read ⇒ sai xót dữ liệu)
* Cách khắc phục:
  * Dùng thread sync: đưa write và read vào 1 hàng đồng bộ. 1 lúc chỉ có làm được 1 việc: read hoặc write ⇒ Chậm
  * Dùng barrier của GCD: có 1 điểm sync ⇒ Nhanh

Nhược điểm: Vì nó tồn tại từ lúc khởi tạo cho đến cuối cùng. Nên nếu tạo nhiều singleton thì tốn bộ nhớ.
