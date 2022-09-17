# Introduce

### Giới thiệu

* Thread: Một tập hợp các câu lệnh được thực hiện
* Multi Thread: Khi ứng dụng có nhiều hơn 1 thread
* Công dụng của đa luồng: Tăng hiệu năng của ứng dụng, giúp quản lý những công việc được thực hiện đồng thời

Có 2 loại thread chính: main thread và nhiều background thread. Main thread dùng để khởi tạo và quản lý UI. Background thread dùng để xử lý những task nặng hoặc lâu.

⇒ Khi call API: sẽ call ở background thread ⇒ xử lý data ⇒ update data vào UI ở main thread

Có 3 cách để tạo đa luồng:

1. Tự tạo và quản lý thread manually
2. Sử dụng [GCD](gcd.md)
3. Sử dụng [Operation Queue](operation-queue.md)

⇒ Thường sử dụng cách 2, một số trường hợp cần sẽ sử dụng cách 3
