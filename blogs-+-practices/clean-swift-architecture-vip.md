---
description: An iOS architecture created by https://clean-swift.com/
---

# Clean Swift Architecture (VIP)

### 1. Giới thiệu <a href="#_1-gioi-thieu-0" id="_1-gioi-thieu-0"></a>

Hôm nay mình sẽ giới thiệu một architecture trong iOS, đó là Clean Swift Architecture (hay còn gọi là VIP). VIP được [Raymond Law](https://clean-swift.com/about/) giới thiệu tại [https://clean-swift.com/](https://clean-swift.com/) dựa trên Clean Architecture của Uncle Bob. Vậy nó khác gì so với architecture khác như MVC, MVVM, MVP, VIPER,... và VIP được tạo ra để giải quyết vấn đề gì. Chúng ta sẽ cùng tìm hiểu ngay bây giờ.

### 2. Đặt vấn đề <a href="#_2-dat-van-de-1" id="_2-dat-van-de-1"></a>

Trước khi đi vào VIP architecture. Mình có 1 bức ảnh mô tả flow của VIPER architecture (tất nhiên vì đây là bài nói về VIP, nên mình sẽ không giải thích nhiều về VIPER).![](https://images.viblo.asia/6df494d8-c981-4075-9e3a-323917bd47e1.png)

Như mọi người thấy ở ảnh trên, **Presenter** (trong VIPER) nhận rất nhiều trách nhiệm: giao tiếp với **View**, giao tiếp với **Interactor**, giao tiếp với **Router**,... Và không ít lần khi mọi người làm việc với VIPER, mọi người sẽ thấy việc truyền action / data từ **View** qua **Interactor** (hay ngược lại từ **Interactor** về **View**) đều phải thông qua **Presenter** rất là tốn tài nguyên.\
\=> Đó là lý do vì sao Raymond đã tạo ra VIP khắc phục nhược điểm trên của VIPER.

### 3. Cấu tạo của VIP architecture <a href="#_3-cau-tao-cua-vip-architecture-2" id="_3-cau-tao-cua-vip-architecture-2"></a>

#### 3.1. Thành phần chính của VIP <a href="#_31-thanh-phan-chinh-cua-vip-3" id="_31-thanh-phan-chinh-cua-vip-3"></a>

![](https://images.viblo.asia/92037776-5d32-4ccf-bd22-ac727eba3bc3.png)

Ở đây mình tạm thời chỉ cho mọi người tập trung 3 thành phần chính cấu tạo nên VIP: **View**, **Interactor** và **Presenter**. Mình sẽ giới thiệu lần lượt từng thành phần

* `View`: Trong VIP architecture, **View** của chúng ta sẽ bao gồm UIViewController và storyboard (hoặc file xibs nếu bạn không dùng storyboard). Nó nhận trách nhiệm giữ reference đến **Interactor** (và cả **Router** - sẽ tìm hiểu ngay bây giờ). **View** sẽ truyền action qua cho **Interactor**, ví dụ: interactor.viewDidLoad() hoặc khi nhấn 1 button ở View interactor.submitButtonTapped(),...
* `Interactor`: Đây là nơi chúng ta sẽ chứa những business logic (ví dụ sẽ validate input của các text được nhận từ **View**,...), là nơi sẽ nhận data (từ server thông qua API hoặc local data thông qua Core Data). Sau khi đã nhận được data, nó sẽ truyền toàn bộ data sang cho **Presenter**. 1 lưu ý nhỏ là ở Interactor chúng ta không nên import UIKit nhé
* `Presenter`: Đây là nơi chúng ta sẽ nhận được data từ **Interactor**. Nó sẽ "xào/nấu" lại data đó (ví dụ như kết hợp data từ nhiều properties,...) để tạo ra các View Model thích hợp. Sau đó **Presenter** sẽ gửi View Model đó sang cho **View** để hiển thị. 1 lưu ý nhỏ là ở **Presenter** phải giữ 1 liên kết yếu (weak reference) với **View** nhé , nếu không có thể gây ra leak memory

\=> Vậy là chúng ta đã hiểu được lifecycle chính của VIP rồi. Tiếp theo mình sẽ giới thiệu chi tiết hơn những thành phần khác (có thể có hoặc không - optional) trong VIP architecture nhé.

#### 3.2. Các thành phần khác (optional) của VIP architecture <a href="#_32-cac-thanh-phan-khac-optional-cua-vip-architecture-4" id="_32-cac-thanh-phan-khac-optional-cua-vip-architecture-4"></a>

![](https://images.viblo.asia/503e3cb9-84f5-4cfe-9890-e3c92fe4bc28.png)

* `Worker`:
  * Như lúc nảy mình đã giới thiệu ở `Interactor`, nó sẽ gửi yêu cầu lấy data (từ Network hoặc Local) sang 1 thành phần khác là **Worker**. Ở **Worker**, đây là nơi xảy ra việc restful APIs (nếu cần data từ Network), sau khi có được data, **Worker** sẽ truyền data đó về lại cho **Interactor**. Thường ở những mô hình khác như MVC, MVP, VIPER,... chúng ta sẽ có 1 class khác tương tự với **Worker** nhưng nằm ngoài mô hình của nó. Ví dụ: UserAPI.shared.getInformation(), ...
  * Có thể có nhiều **Worker** phục vụ cho **Interactor**. Như ảnh mọi người có thể thấy, mình có 2 **Workers**: Networking Worker và Core Data Worker,... Nếu chúng ta cần lấy thêm data ở 1 nơi khác, chúng ta có thể tạo thêm một hoặc nhiều **Worker** khác tuỳ vào nhu cầu của chúng ta. Ví dụ mình có thể tạo thêm Realm Worker, Cloud Firestore Worker, ...
* `Router`:
  * Dĩ nhiên nghe tên chúng ta cũng hình dung được nó có trách nhiệm gì rồi nhỉ. Nó sẽ nhận trách nhiệm di chuyển giữa các màn hình trong ứng dụng iOS của chúng ta (push, pop, presenter, dismiss,...).
  * Trong VIP architecture, **Router** được liên kết với **View** nên chúng ta có thể sử dụng segue (là 1 đặc trưng cũng là lý do mà Raymond tạo ra VIP). Tuy nhiên mình không đánh giá cao ưu điểm này của VIP, vì trong project thực tế, mình thích tách rời từng Scene ra từng Storyboard hơn là việc 1 Storyboard chứa nhiều Scenes
  * 1 lưu ý là nó phải giữ 1 liên kết yếu với **View** nhé

\=> Vậy là chúng ta đã nắm rõ các thành phần chính và phụ của VIP architecture rồi

### 4. Cài đặt (Implementing) VIP Architecture <a href="#_4-cai-dat-implementing-vip-architecture-5" id="_4-cai-dat-implementing-vip-architecture-5"></a>

Mình có 1 example code nho nhỏ về VIP architecture trong iOS bằng Swift. Mọi người có thể tham khảo ở [link](https://github.com/kien-hoang/TMDB-Movies-VIP). Nếu thấy hay mọi người hay thả sao (Star) cho mình nhé.![](https://images.viblo.asia/888d955e-ff75-4615-a1c4-58b74da504cb.png)\
**Note:** Mình có kèm template để tạo nhánh VIP architecture ở link trên. Mọi người có thể tải về và import vào Templates Xcode để sử dụng nhanh nhé.

### 5. Ưu và nhược điểm của VIP Architecture <a href="#_5-uu-va-nhuoc-diem-cua-vip-architecture-6" id="_5-uu-va-nhuoc-diem-cua-vip-architecture-6"></a>

#### 5.1. Ưu điểm <a href="#_51-uu-diem-7" id="_51-uu-diem-7"></a>

* Như ảnh sơ đồ VIP ở mục 3.1, chúng ta thấy rằng flow của VIP chỉ đi theo 1 chiều (Unidirectional flow). Dẫn đến code của chúng ta sẽ rõ ràng hơn, dễ debug và maintain hơn.
* Dễ kiểm thử vì có sự phân chia rõ ràng giữa các thành phần trong VIP (và những thành phần này giao tiếp với nhau thông qua protocols)
* Có cấu trúc module tốt
* Việc thêm tính năng (scaleable) cho dự án VIP sẽ dễ dàng hơn nhiều
* Có sẵn template VIP (mình có đính kèm ở link github ở mục 4 nhé)

#### 5.2. Nhược điểm <a href="#_52-nhuoc-diem-8" id="_52-nhuoc-diem-8"></a>

* Sẽ có sự khó khăn cho người mới vì VIP architecture có khá nhiều file cần phải tạo khi thêm 1 module (tuy nhiên có thể khắc phục bằng cách sử dụng template VIP để tạo)
* Vì những thành phần trong VIP giao tiếp với nhau thông qua protocols => Nên việc đặt tên protocol sẽ trở nên khó khăn hơn. Nếu chúng ta không cẩn thận có thể gây ra hiểu lầm (confuse) cho những developer khác
* Size app sẽ tăng nhiều vì nhiều protocol (là hệ quả cứ nhược điểm trên)
* Khá ít tài liệu. Trong quá trình tìm hiểu VIP, mình thấy khá ít tài liệu và example code có thể tham khảo, 1 số tài liệu đã lỗi thời. Cũng gây khó khăn cho mình trong lúc tìm hiểu VIP
* Vấn đề với việc truyền dữ liệu (passing data) giữa các màn hình với nhau. Trong lúc làm example code cho blog này, mình gặp vấn đề trong việc truyền dữ liệu. Mình thấy không hài lòng với cách truyền dữ liệu hiện tại của [example code](https://github.com/Clean-Swift/CleanStore) (từ trang chính chủ [https://clean-swift.com/](https://clean-swift.com/) ). Nếu bạn đọc có giải pháp nào tối ưu hơn hãy nói cho mình và mọi người khác biết nhé
* Ưu điểm segue là 1 điểm mạnh của VIP, tuy nhiên nó không thực sự hữu dụng với mình để áp dụng vào project thực tế

### 6. Kết luận <a href="#_6-ket-luan-9" id="_6-ket-luan-9"></a>

* Vậy là chúng ta đã tìm hiểu xong VIP architecture là gì và cách cài đặt nó như thế nào thông qua blog này.
* Nếu các bạn có thắc mắc hay có cách nào tối ưu được example code của mình thì đừng ngần ngại hãy comment nhé. Mình cảm ơn các bạn rất nhiều.

### Reference <a href="#_reference-10" id="_reference-10"></a>

* [https://youtu.be/Szlgqnk6gHg](https://youtu.be/Szlgqnk6gHg)
* [https://medium.com/@nirajpaul.ios/vip-architecture-pattern-vip-viper-302d7d1069df](https://medium.com/@nirajpaul.ios/vip-architecture-pattern-vip-viper-302d7d1069df)
* [https://www.netguru.com/blog/clean-swift-ios-architecture-pattern](https://www.netguru.com/blog/clean-swift-ios-architecture-pattern)
