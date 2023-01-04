# Autolayout Priority

Link tham khảo: [https://thanhvu.dev/2019/09/16/autolayout-priority/](https://thanhvu.dev/2019/09/16/autolayout-priority/)

**Content hugging priority** là độ ưu tiên quyết định view không bị dãn ra.&#x20;

Ví dụ: có 2 label trong 1 hàng.&#x20;

* Label 1 nếu có hugging lớn hơn Label 2 thì Label 1 sẽ có width bằng với title của nó, còn Label 2 thì lấy đầy phần width còn lại (bị kéo dài ra so với title của nó).
* Label 1 nếu có hugging nhỏ hơn Label 2 thì Label 1 sẽ bị kéo dài ra, còn Label 2 thì có width bằng với title của nó.



**Content compression resistance priority** là độ ưu tiên quyết định view không bị co vào.&#x20;

Ví dụ: có 2 button trong 1 hàng. 2 button này có title dài hơn chiều dài màn hình.

* Button 1 nếu có priority lớn hơn Button 2 thì Button 1 sẽ được kéo dài (so với title của nó) còn Button 2 bị co vào
