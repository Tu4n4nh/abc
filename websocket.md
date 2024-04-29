## Overview  
Websocket là công nghệ socket bất đồng bộ được dùng trên các ứng dụng web. Chúng khởi tạo một kết nối socket giữa browser và webserver thông qua giao thức HTTP. 
Websocket giúp giữ kết kết nối giữa server và browser trong một khoảng thời gian dài, giúp ta giảm bớt quá trình tạo kết nối TCP trong quá trình giao tiếp giữa 
browser và webserver  
Thông thường websocket thường được sử dụng trong việc cập nhật dữ liệu qua lại giữa client và server. Chúng thích hợp trong các hệ thống báo động, noti, messenger,...
Ta có thể dùng burp để bắt và thay đổi các message của web socket tương tự như việc bắt các request HTTP thông thường. Lịch sử các phiên trao đổi giữa client và
server được lưu lại trong tab `Proxy > Socket history`
### Cơ chế hoạt động  
Websocket là một TCP socket, khi khởi tạo nó cũng thực hiện quá trình bắt tay 3 bước như mọi phiên TCP thông thường. Ta có thể nhận biết quá trình khởi tạo qua 
request HTTP:
```
 GET /chat HTTP/1.1
Host: normal-website.com
Sec-WebSocket-Version: 13
Sec-WebSocket-Key: wDqumtseNBJdhkihL6PW7w==
Connection: keep-alive, Upgrade
Cookie: session=KOsEJNuflw4Rd9BDNrVmvwBF9rEijeE2
Upgrade: websocket 
```  
Nếu serer chấp nhận nó sẽ gửi trả  
```
HTTP/1.1 101 Switching Protocols
Connection: Upgrade
Upgrade: websocket
Sec-WebSocket-Accept: 0FFP+2nmNIf/h+4BP36k9uzrYGk= 
```  
Đến đây thì giữa client và server có thể trao đổi dữ liệu cho nhau. Điều này chính là điểm lợi thế của socket so với HTTP. Khi nó chỉ cần 1 lần tạo phiên TCP là có
thể dùng để trao đổi dữ liệu nhiều lần. Một điểm khác biệt nữa là client có thể request nhiều lần không nhất thiết phải đợi server phản hồi rồi mới request lần thứ 2. 
Đây là cơ chế hoạt động bất đồng bộ so với cơ chế đồng bộ của HTTP. Sở dĩ nó chạy được bất đồng bộ là do socket là một kết nối full duplex còn HTTP chỉ là kết nối
half-duplex  
## Vulnerability  
Bản chất của websocket là tạo một kênh truyền ổn định giữa client và server. Với các ứng dụng sử dụng websocket nhận user input hoặc các request từ client đèu có 
thể bị dính các lỗ hổng tương tự khi dùng HTTP.  

Thông thường các ứng dụng websocket thường xảy ra các lỗ hổng

1.  Unencrypted Communications
2.  Cross-Site WebSocket Hijacking (CSRF with WebSockets)
3.  Sensitive information disclosure over network
4.  Denial of Service
5.  User-input validation    

### Unencrypted Communications  
Tương tự như HTTP, HTTPS websocket cũng có 2 tiêu chuẩn `ws:` và `wss` tượng trưng cho kết nối mã hóa và không mã hóa dữ liệu. Attacker có thể khai thác MITM trên
các giao thức không được mã hóa để lấy cắp thông tin của victim  

### CSRF with WebSockets  
SOP không có tác dụng đối với websocket, chính vì vậy cần phải có các biện pháp để ngăn chặn CSRF. Có một cách để áp dụng token trong phòng chống CSRF  

#### Trường hợp có triển khai SOP  
Với các ứng dụng triển khai SOP. Ta có nhiều cách để import token  
1. Dùng session ID value
2. Nhúng token vào HTML page  
3. Tạo request để lấy token  

Websocket sẽ không thể lấy được token vì chính sách SOP ngăn chặn việc này. 

#### trường hợp triển khải CORS  
Đối với các ứng dụng triển khải CORS, cần chú ý không trong việc cấu hình cross-site, việc cấu hình sai có thể bị lợi dụng để attacker request lấy token trong trường
hợp 2, 3 bên trên. Thêm vào đó, session cookie quá dễ đoán, trang web bị dính XSS, có thể bị lợi dụng để lấy token. Thay vào đó, có thể cấu hình `Origin` header 
để kiểm soát việc request đến từ đâu. Bởi vì các browser không có khả năng snoofing `Origin` header nên có thể được áp dụng để chống CORS   

`Origin` header là cách được sử dụng nhiều trong việc phòng chống CSRF trên websocket. Đây cũng là dấu hiệu để nhận biết được khả năng bị ứng dụng websocket có bị
lỗi CSRF hay không 
### Sensitive information disclosure over network  
Websocket thường được dùng để trao đổi thông tin giữa client và webserver. Tuy nhiên, do lỗi của các lập trình viên, thường trả toàn bộ data của user mỗi khi request
mà không loại bỏ các thông tin nhạy cảm(vd `/api/get-deail-user/ID`). Nếu một ứng dụng socket không được mã hóa đường truyền, attacker có thể kết hợp 2 điểm yếu này để lấy toàn bộ thông tin của
người dùng. Để hạn chế điểm này, lập trình viên cần hạn chế, chỉ lấy các thông tin cần thiết mỗi khi request, hạn chế truyền các thông tin nhạy cảm, không cần thiết

### Denial of Service  
Cơ chế của socket cho ta tạo nhiều kết nối đồng thời đến server (đa luồng). Nếu không chỉ giới hạn số kết nối request đến server, attacker có thể lợi dụng gửi 1 lượng
lớn request làm cho server không kịp xử lý dẫn đến treo 

### User-input validation  
Vấn để này cũng tương tự như trên HTTP, user input không được xử lý, loại bỏ các kí tự đặc biệt. Việc này dẫn đến có thể khai thác XSS, XXE, SSRF, SQL injection.  

## Phòng tránh  
Để phòng tránh được tấn công trên websocket ta cần  
1. Sử dụng mã hóa đường truyền (`wss://`)  
2. Validate user input  
3. Giới hạn số request qua socket  
4. Cấu hình `Origin` header    
5. Loại bỏ các thông tin nhạy cảm được trả về   

**ref**  
https://cobalt.io/blog/a-pentesters-guide-to-websocket-pentesting  
https://dev.solita.fi/2018/11/07/securing-websocket-endpoints.html  


