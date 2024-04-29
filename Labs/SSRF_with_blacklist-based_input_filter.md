## Overview  
Challenge này khi thử thay thế bằng link `http://localhost/admin` thì server báo block request. Do đó cần phải thử một số cách để bypass.  
1. Chuyển dạng biểu điễn của IP về hexa, binary,...  
2. Kết hợp encode các kí tự  

Áp dụng 2 cách trên sau khi ta thử 
![image](https://user-images.githubusercontent.com/22276823/130430742-be0edfa8-95a8-48eb-8e8e-d39d6152ecdf.png)  
![image](https://user-images.githubusercontent.com/22276823/130430918-e0159ce0-32b2-4a30-920c-7d0acda0e591.png)  

Ta nghĩ đến việc kết hợp cả 2 phương pháp  
![image](https://user-images.githubusercontent.com/22276823/130430999-d1df9db8-60ec-4bff-a897-094d435dda0a.png)  
  
Khi áp dụng cả 2 phương pháp vẫn không thành công. Ta áp dụng `double hex encode` lên kí tự `a`  

![image](https://user-images.githubusercontent.com/22276823/130431155-b954aec0-635a-4dd0-850d-c79751597d57.png)

Bypass thành công



