## Overview  
Path traversal (directory traversal) là lỗ hổng nhấm vào các file, thư mục nằm bên ngoài phạm vi của web server. Kiểu tấn công
này chèn thêm các kí tự `../` hoặc chèn một đường dẫn mới vào url để truy cập các file, thư mục tùy ý trên web server. Dựa vào
cách tấn công mà nó còn được gọi là  kiểu tấn công dot-dot-slash  

## Impact  
Với lỗ hổng này attacker có thể tùy ý đọc file trên, kết hợp để tạo ra một lỗ hổng khác ([Zipslip](https://www.infoq.com/news/2018/06/zip-slip/), gọi file thực thi shell)
Tuy nhiên, hiện tại các server được phân quyền khá kĩ, thông thường cần phải áp dụng thêm các kĩ thuật bypass để vượt qua  

## Các kĩ thuật bypass  
Một số kĩ thuật bypass:
1. [Absolute path bypas](#absolute-path-bypass)  
2. [Stripped non-recursively](#stripped-non-recursively)  
3. [Stripped with superfluous URL-decode](#stripped-with-superfluous-URL-decode)  
4. [Validation of start of path](#validation-of-start-of-path)  
5. [Validation of file extension with null byte bypass](#validation-of-file-extension-with-null-byte-bypass)  
6. [Including content from an external source](#including-content-from-an-external-source)  
  
Điều kiện tiên quyết là cần tìm được các input parameter mà attacker có thể kiểm soát dữ liệu đầu vào. Một trong những chỗ dễ nhận biết nhất là link image. Sau đó cần xác định được backend đang dùng là windows hay linux để có thể chọn payload phù hợp. Điều quan trọng, attacker cần phải biết được ví trí gọi file của web app 
nằm ở chỗ nào để có thể chèn payload cho thích hợp  
khi nhấp vào xem image, web app sẽ trả về một link với parameter có value là tên image  
![image](https://user-images.githubusercontent.com/22276823/131245552-a10bada6-c3b9-40f0-831d-d2d9c8e36dd8.png)  

Lúc này ta, sẽ thấy giá trị của image bằng đường dẫn tùy ý  
![image](https://user-images.githubusercontent.com/22276823/131245645-0520966a-b566-49e1-8103-13b01a9aaf6d.png)  

### Absolute path bypass  
Có những trường hợp, server chỉ xử lý các file trong thư mục, dẫn đến chèn `../` sẽ trả về lỗi. Tuy nhiên vẫn xử lý các input chứa đường dẫn tuyệt đối. 
Lúc này ta có thể chèn luôn đường dẫn tuyệt đối đến file mà ta cần xem  

![image](https://user-images.githubusercontent.com/22276823/131245875-75dd5aeb-f669-4993-8d2b-6996c3aecfd7.png)  

Sửa thành đường dẫn tuyệt đối  
![image](https://user-images.githubusercontent.com/22276823/131245884-a3bee25f-739f-40a7-866f-56a4ec2b794e.png)  

### Stripped non-recursively  
Trường hợp này, web app thực hiện loại bỏ các kí tự `../`. Tuy nhiên, việc này chỉ thực hiện một lần (non-recursively). Việc này dẫn đến có thế bị bypass bằng cách
chèn double `../`  
![image](https://user-images.githubusercontent.com/22276823/131246062-bd108633-ecd6-47ce-a7c1-c13d3a90791e.png)  

### Stripped with superfluous URL-decode  
Trường hợp này chặt chẽ hơn trường hợp trên khi đã loại bỏ hoàn toàn `../`. Tuy nhiên vẫn có thể bypass bằng cách encode các kí tự `../`  
![image](https://user-images.githubusercontent.com/22276823/131246281-f1803c9f-2cf1-4694-aa1b-de4951a23e08.png)  

### Validation of start of path  
Trường hợp này thực hiện validate đường dẫn của file. Tuy nhiên lại không filter các kí tự đặc biệt `../` dẫn đến ta có thể bypass. Nếu web app kết hợp filter kí tự
đặc biệt thì ta cần kết hợp các các bypass lại với nhau  
![image](https://user-images.githubusercontent.com/22276823/131246359-8c64a682-202e-4939-b16c-e88752284612.png)  

### Validation of file extension with null byte bypass  
Trường hợp này thực hiện kiểm tra đuôi file, đảm bảo file là image. Ta áp dụng kĩ thuật chèn null byte vào cuối tên file để bypass  
![image](https://user-images.githubusercontent.com/22276823/131246447-4a6cb7a4-3121-4894-8100-e0fe2e8ff843.png)  

### Including content from an external source  
Một số trường hợp có thể gọi các đường dẫn từ các server khác, nếu web app có kết nối đến server đó. Cách tấn công này gọi là remote file inclusion (RFI)  
`http://example.com/index.php?file=http://www.owasp.org/malicioustxt`  

## Prevent  
1. Filter user input, chỉ cho phép các kí tự [0-9a-zA-z]    
2. Cố gắng hạn chế user có thể thao tác với tệp trên hệ thống  
3. Không để user có thể control toàn bộ đường dẫn của tệp. Chỉ cho user submit tên tệp, backed sẽ thực hiện chèn thêm đường dẫn app cơ sở (base dir) và các kí chuỗi
đặc biệt vào cuối file  
4. Sử dụng các thư viện chuẩn hóa khi sử dụng user input để thao tác với tệp  
5. Phân quyền chặt chẽ, hạn chế việc truy cập vào các file tệp khác  

Ref:  
[1]: https://portswigger.net/web-security/file-path-traversal  
[2]: https://owasp.org/www-community/attacks/Path_Traversal  


