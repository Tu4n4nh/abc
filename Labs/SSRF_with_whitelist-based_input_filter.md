## Overview  
![image](https://user-images.githubusercontent.com/22276823/130431538-88f4f77f-7d30-4774-a4e1-842afe25df0f.png)  
Với dạng này ta thấy, server response lại chỉ chấp nhận các request từ subdomain `stock.weliketoshop.net`. Đây à một dạng whitelist filter  
Để bypass dạng này tùy thuộc vào các webmaster cấu hình bộ lọc. Nếu bộ lọc được cấu hình tốt thì không thể bypass được. Có một số cách  
1. `domain@127.0.0.1` hoặc `127.0.0.1@domain`  
2. `1270.0.1#domain`  
Chi tiết xem [link](https://book.hacktricks.xyz/pentesting-web/ssrf-server-side-request-forgery#basic-bypass-localhost)  

Với bài này 
![image](https://user-images.githubusercontent.com/22276823/130432430-de74f239-17c4-47b9-9ece-6eb527a398dd.png)  
![image](https://user-images.githubusercontent.com/22276823/130432764-35e2d5d4-ac9d-47b6-aa5e-e55aa097a298.png)
 

Nhìn 2 response ta nhận thấy với payload `localhost@domain` đã bypass được phần filter ban đầu. Ở đây `localhost` được server hiểu như là user.
Khi ta đổi payload sang `localhost#domain` thì payload không hoạt động. Do phần fragment đăng sau đã bị server bỏ qua. Bộ lọc kiểm tra không có nên block request  
![image](https://user-images.githubusercontent.com/22276823/130433680-e8d8c957-4fa1-4b1f-9415-2df5da7adfb5.png)  

Ta thử kết hợp cả 2 thì vẫn báo lỗi  
![image](https://user-images.githubusercontent.com/22276823/130433973-d4f2f2bf-ec5e-4ba4-91d6-8fd73db07203.png)  

Đến đây ta thử hex encode các kí tự để xem có bypass được không. Sau vài lần thử encode với các payload đã thử từ trước. payload hoạt động là `localhost%2523@domain`  

![image](https://user-images.githubusercontent.com/22276823/130434125-efb5ce60-04bf-42dc-b3b1-4f3532e19dd3.png)

