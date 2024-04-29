## Overview

Server-side template injection là một lỗ hổng injection. Xảy ra trên các ứng dụng web có sử dụng template để tự động render nội dung nhưng không thực hiện kiểm tra
input của user mà truyền thẳng template. Việc này cho phép attacker có thể thao túng và điều khiển được dữ liệu trả nếu biết được cách mà template thao tác với input.

## Ảnh hưởng

Là một lỗ hổng server-side injection, ảnh hưởng của nó cực kì nghiêm trọng đối với web app. Tùy thuộc vào context đang hoạt động và tác dụng của entrypoint. `SSTI`
cho phép attacker có thể kết hợp tạo ra lỗ hổng `LFI`, `RCE`,...

## Detect

Vì behaviour tương đối giống với `XSS` (user input được hiện thị ngược lại trang web) nên nó dễ dàng bị nhầm lẫn và bị bỏ qua. Do tương đồng về behaviour, việc kiếm và
phát hiện `SSTI` cũng gần giống với `XSS` khi cũng thực hiện truyền các macilous input vào và phân tích các kết quả trả về để tìm thấy sự khác biệt.

Do template có thể hoạt động trên 2 context: `plaintext context` và `code context`. Sau khi xác định được entrypoint, ta cần xác định được context mà chúng đang hoạt
động.

### Plaintext context

Giá trị của entrypoint có thể được truyền thẳng vào HTML. Repsonse của context giống với XSS, input được chèn vào tag HTML. Cách để nhận biết và phân biệt nó với XSS
là truyền cho nó 1 statement. Vì `XSS` là một lỗ hổng client, các giá trị truyền vào của nó đơn thuần là các chuỗi string, text. Còn template được dùng với mục đích
render tự động nội dung của web app nên nó có thể xử lý các statement

```
smarty=Hello ${7*7}
Hello 49
```

### Code context

Giá trị của entrypoint được truyền vào HTML thông qua template statement

```
smarty= TuanAnh
<h1>Hello {{ smarty }}</h1>
```

https://portswigger.net/web-security/images/template-decision-tree.png

### Tổng kết

Để có thể khai thác SSTI ta cần 2 điều kiện.

1. Xác định được entrypoint gây lỗi. Bằng cách truyền vào các macilous input và phân tích giá trị trả về
2. Xác định được context của response trả về

## Phòng chống

1. Luôn luôn santinizer input.
2. Dùng sandbox.

**_ref_**  
https://portswigger.net/research/server-side-template-injection
