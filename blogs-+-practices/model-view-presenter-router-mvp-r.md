# Model View Presenter - Router (MVP-R)

### 1. Giới thiệu 👋 <a href="#_1-gioi-thieu--0" id="_1-gioi-thieu--0"></a>

Hôm nay mình sẽ giới thiệu một mô hình phổ biến trong iOS, đó là Model View Presenter - Router (MVP - R). Vậy nó khác gì so với mô hình MVC và mô hình MVP thuần tuý. MVP - R được tạo ra để giải quyết vấn đề gì? Chúng ta sẽ cùng tìm hiểu ngay bây giờ nhé !!!

### 2. Đặt vấn đề 🤔 <a href="#_2-dat-van-de--1" id="_2-dat-van-de--1"></a>

Đối với những người vừa tiếp cận lập trình iOS, chúng ta thường sẽ xây dựng dự án dưới dạng mô hình MVC (Model View Controller). Tuy nhiên khi dự án đã lớn dần, nhiều tính năng hơn, sẽ có lúc bạn thấy được Controller của chúng ta có thể lên đến 1000 dòng code. Nên người ta hay gọi vui MVC là Massive View Controller. Đó là vì Controller (trong MVC) nhận quá nhiều trách nhiệm: xử lý business logic, view logic, navigator, ... Vậy có cách nào tránh được trường hợp này hay không?

***

Tất nhiên rồi, đó là lý do chúng ta sẽ tìm hiểu một mô hình mới giải quyết vấn đề trong mô hình MVC. Đó là mô hình MVP-R

### 3. Cấu tạo của MVP architecture ♥️ <a href="#_3-cau-tao-cua-mvp-architecture--2" id="_3-cau-tao-cua-mvp-architecture--2"></a>

#### 3.1. Thành phần chính của MVP <a href="#_31-thanh-phan-chinh-cua-mvp-3" id="_31-thanh-phan-chinh-cua-mvp-3"></a>

![](https://images.viblo.asia/317b4cdb-2da9-441c-9ff8-5ee2837d2a53.png)

Trên đây là sơ đồ luồng hoạt động của các thành phần chính trong MVP. MVP bao gồm 3 thành phần chính: Model, View và Presenter. Mình sẽ giới thiệu từng thành phần nhé:

* `Model`: Là nơi giữ các struct/class mà ứng dụng chúng ta sử dụng. Thường thì mình sẽ tách **Model** ra khỏi mô hình để sử dụng chung cho toàn bộ project (tham khảo ở example code ở phần tiếp theo nhé)
* `View`: bao gồm UIViewController và storyboard (hoặc xibs nếu bạn không dùng storyboard). Bây giờ ở **View** chỉ cần quan tâm đến view logic: setup UI, nhận các event/action từ user,.. Sau đó **View** sẽ truyền các event/action đó sang cho **Presenter** xử lý. Tất nhiên nó cũng phải chứa một reference đến **Presenter** nhé
* `Presenter`:
  * Đây chính là trái tim của mô hình MVP. Là nơi chúng ta sẽ nhận các event/action từ **View**, là nơi chúng ta thực hiện restful API (gọi API đến server để lấy dữ liệu), validate input, ... Nhìn chung mọi business logic chúng ta đều xử lý ở **Presenter**
  * Sau khi có data (data đã được xử lý), **Presenter** sẽ truyền data đó về lại cho **View** và nhờ **View** hiển thị ra màn hình
  * **Presenter** cần chứa 1 liên kết yếu đến **View** để tránh tình trạng retain cycle nhé
  * Một lưu ý quan trọng: là **Presenter** không nên import UIKit nhé (vì có thể gây ra code smell)

\=> Note: Các thành phần này sẽ giao tiếp với nhau thông qua Protocol nhé mọi người. Điều này khá là quan trọng trong mô hình này và cũng là ưu điểm so với MVC\
\=> Qua đây chúng ta đã hiểu sơ được các thành phần chính và công dụng của từng thành phần trong mô hình MVP.

***

Vậy logic navigate (push, present, dismiss, pop) thì chúng ta sẽ thực hiện ở đâu nhỉ? Đáp án là chúng ta sẽ thực hiện ở View (vì có UIViewController).\
\=> Như vậy vô hình chung, View sẽ nhận thêm trách nhiệm navigate. Chúng ta có thể tối ưu được thêm 1 chút nữa không nhỉ? Chúng ta sẽ đến 1 thành phần khác trong mô hình MVP-R nhé.

#### 3.2. Router trong MVP architecture <a href="#_32-router-trong-mvp-architecture-4" id="_32-router-trong-mvp-architecture-4"></a>

![](https://images.viblo.asia/422de2de-7830-4446-904f-972a0c989173.png)\
Trên đây là sơ đồ toàn bộ các thành phần trong mô hình MVP-R. Mô hình này sẽ khác với mô hình MVP thuần tuý ở việc nó sẽ tách logic navigate sang 1 class mới tên là `Router`. Và để **Router** này có thể xử lý được việc push, present, dismiss hay pop view thì nó phải chứa 1 liên kết yếu đến **View** nhé.

* `Router`: này có trách nhiệm xử lý việc di chuyển giữa các màn hình trong mô hình MVP-R nhé

### 4. Cài đặt (Implementing) MVP-R Architecture 😎 <a href="#_4-cai-dat-implementing-mvp-r-architecture--5" id="_4-cai-dat-implementing-mvp-r-architecture--5"></a>

Mình có 1 example code nho nhỏ về MVP-R architecture trong iOS bằng Swift. Mọi người có thể tham khảo ở [link](https://github.com/kien-hoang/TMDB-Movies-MVP-R). Nếu thấy hay mọi người hay thả sao (Star) cho mình nhé.![structure.png](https://images.viblo.asia/37f4bad5-61c4-4f02-a0f8-c4f0a65e4912.png)\
Ở đây mình có 2 class khá lạ nên mình sẽ giới thiệu sơ qua cho mọi người hiểu nhé

* `Contract`: Đây là nơi chứa toàn bộ Protocol để giao tiếp giữa các thành phần: protocol giao tiếp giữa View và Presenter, giữa Presenter và Router
* `Builder`: Là nơi chứa 1 hàm build() và hàm này có nhiệm vụ kết nối các thành phần trong mô hình MVP-R lại với nhau cho chúng ta và trả về 1 reference UIViewController\


***

**Note:** Mình có kèm template để tạo nhánh MVP-R architecture ở link trên. Mọi người có thể tải về và import vào Templates Xcode để sử dụng nhanh nhé.

### 5. Ưu và nhược điểm của MVP-R Architecture 🤙 <a href="#_5-uu-va-nhuoc-diem-cua-mvp-r-architecture--6" id="_5-uu-va-nhuoc-diem-cua-mvp-r-architecture--6"></a>

#### 5.1. Ưu điểm <a href="#_51-uu-diem-7" id="_51-uu-diem-7"></a>

Vì Presenter đã tách code business logic ra khỏi View nên sẽ giúp chúng ta:

* Dễ dàng đọc hiểu code.
* Dễ dàng viết Unit test cho logic.
* Dễ maintain, bảo trì.

#### 5.2. Nhược điểm <a href="#_52-nhuoc-diem-8" id="_52-nhuoc-diem-8"></a>

* Số lượng code và file code tăng lên nhiều so với MVC
* Vì mô hình chỉ xử lý việc tách business logic ra khỏi View, nên việc call API, lưu data xuống local,... đều được xử lý ở Presenter. Sẽ gây ra hiện tượng Massive Presenter (Presenter nhận nhiều trách nhiệm nên bị phình to ra)
* Sẽ khó để cấu hình/tổ chức code mô hình MVP-R cho người mới (tuy nhiên mình có kèm template ở mục 4 nên không còn vấn đề gì nữa nhé)

### 6. Kết luận 📔 <a href="#_6-ket-luan--9" id="_6-ket-luan--9"></a>

* MVP bao gồm 3 phần: Model, View và Presenter. MVP-R có thêm Router
* View và Presenter không biết sự tồn tại của nhau, vì chúng giao tiếp với nhau thông qua protocol
* Router nhận trách nhiệm navigate giữa các màn hình (thay vì View trong mô hình MVP thuần tuý)
* Presenter là nơi xử lý business logic và truyền lại cho View để hiển thị data

***

* Vậy là chúng ta đã tìm hiểu xong MVP-R architecture là gì và cách cài đặt nó như thế nào thông qua blog này.
* Nếu các bạn có thắc mắc hay có cách nào tối ưu được example code của mình thì đừng ngần ngại hãy comment nhé. Mình cảm ơn các bạn rất nhiều.

### Reference 🥳 <a href="#_reference--10" id="_reference--10"></a>

* [https://daddycoding.com/2021/06/21/model-view-presenter-mvp/](https://daddycoding.com/2021/06/21/model-view-presenter-mvp/)
* [https://magz.techover.io/2020/06/19/mvp-with-swift/](https://magz.techover.io/2020/06/19/mvp-with-swift/)
