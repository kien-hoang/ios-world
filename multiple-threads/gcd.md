# GCD

Là 1 API cấp thấp giúp chúng ta quản lý và thực hiện đa luồng. Nó cung cấp 1 số API: DispatchQueue, DispatchGroup, Semaphore, Barrier.

Nhắc đến GCD ta có 1 số khái niệm:

* The main queue: truy cập bằng DispatchQueue.main ⇒ update UI
* The global queue: truy cập bằng [DispatchQueue.global](http://dispatchqueue.global) ⇒ Nó chia ra làm 4 loại chính và 1 loại default theo QoS (Quality of Service), theo mức độ ưu tiên từ cao nhất xuống thấp nhất:
  1. userInteractive (high priority): Used for animations, or updating UI
  2. userInitiated (high priority): Used for tasks like loading data from API, preventing the user from making interactions.
  3. default
  4. utility (low priority): Used for tasks that do not need to be tracked by the user.
  5. background (low priority): Used for tasks like saving data in the local database or any maintenance code which is not on high priority.

⇒ 2 mức độ utility và background thường được sử dụng cho những task nặng (lưu dữ liệu xuống local) để tránh việc block UI

* Custom Queues (hay còn gọi là private queues): chúng ta tạo ra nó, có thể là serial hoặc concurrent queues

**Ưu điểm:** Dễ sử dụng (GCD đã lo việc tạo thread, quản lý task)

**Nhược điểm:**

1. Không thể cancel hoặc tạm dừng 1 task khi task đó đang chạy
2. Không thể kiểm tra xem còn bao nhiêu tác vụ đang đợi trong 1 queue
3. Không thể cấu hình có tối đa bao nhiêu tác vụ chạy cùng lúc trong queue (can not define the maximum number of concurrent operations)

⇒ Trong 1 số trường hợp nhất định, chúng ta cần tracking 1 task vụ trong queue nhưng GCD không làm được

⇒ Sử dụng Operation Queue



### Reference link

* [https://www.swiftpal.io/articles/what-is-qos-quality-of-service-in-gcd-swift](https://www.swiftpal.io/articles/what-is-qos-quality-of-service-in-gcd-swift)
