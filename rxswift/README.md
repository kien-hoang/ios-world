# RxSwift

#### **Observable**

The first thing you need to understand is that everything in _**RxSwift**_ is an _**observable sequence**_ or something that operates on or subscribes to events emitted by an _**observable sequence**_

Observable sequences can emit **zero** **or more events** over their [lifetimes.In](http://lifetimes.in) _**RxSwift**_ an _Event_ is just an **Enumeration Type** with 3 possible states:

* **.next**(value: T) — When a value or collection of values is added to an observable sequence it will send the _**next event**_ to its subscribers as seen above. The associated value will contain the actual value from the sequence.
* **.error**(error: Error) — If an Error is encountered, a sequence will emit an _**error event**_. This will also terminate the sequence.
* **.completed** — If a sequence ends normally it sends a _**completed**_** event** to its subscribers

#### **Subjects**

Subject trong RxSwift hoạt động như vừa là một Observable, vừa là một Observer. Khi một Subject nhận một .next event thì ngay lập tức nó sẽ phát ra các element cho các subscriber của nó.

Trong RxSwift, chúng ta có 4 loại Subject với các cách thức hoạt động khác nhau, bao gồm:

* **PublishSubject**: Khởi đầu "empty" và chỉ emit các element mới cho subscriber của nó.
* **BehaviorSubject**: Khởi đầu với một giá trí khởi tạo và sẽ relay lại element cuối cùng của chuỗi cho Subscriber mới.
* **ReplaySubject**: Khởi tạo với một kích thước bộ đệm cố định, sau đó sẽ lưu trữ các element gần nhất vào bộ đệm này và relay lại các element chứa trong bộ đệm cho một Subscriber mới.
* **Variable**: Lưu trữ một giá trị như một state và sẽ relay duy nhất giá trị cuối cùng cho Subscriber mới.
