# SSRF 
SSRF là dạng tấn công mà kẻ tấn công có thể khiến máy chủ thực hiện các yêu cầu HTTP đến một miền tùy ý
Kẻ tấn công có thể yêu cầu máy chủ trả về chính nó hoặc các dịch vụ khác tùy thuộc vào hạ tầng của đối tượng 
Dạng tấn công phổ biến thường gặp là ở các trang web thương mại dùng api để lấy thông tin 

## VD: 
một yêu cầu HTTP thông thường 
```
*POST /product/stock HTTP/1.0*</br>
*Content-Type: application/x-www-form-urlencoded*</br>
*Content-Length: 118*

*stockApi=http://stock.weliketoshop.net:8080/product/stock/check%3FproductId%3D6%26storeId%3D1*
```  
Server thực hiện yêu cầu sẽ trả về số lượng hàng còn trong kho. Với yêu cầu này, kể tấn công có thể thay đổi trả về trang admin để liệt kê toàn bộ user 
```  
*POST /product/stock HTTP/1.0*</br>
*Content-Type: application/x-www-form-urlencoded*</br>
*Content-Length: 118*

*stockApi=http://localhost/admin*
```  
Hoặc có thể yêu cầu tới các máy chủ khác 
```
*POST /product/stock HTTP/1.0</br>  Content-Type: application/x-www-form-urlencoded</br>  Content-Length: 118*

*stockApi=http://192.168.0.68/admin*
```  

## Các loại SSRF   

### Blacklist  
Những gì không nằm trong blacklist mới được phép truy cập. Tuy nhiên có một số cách để bypass blacklist  
* Chuyển IP về dạng decimal, hexa, octo hoặc lược bỏ các vị trí có số 0. Tool này hỗ trợ đổi dạng hiển thị của IP [link](https://github.com/vysecurity/IPFuscator)  
**VD** 127.0.0.1 thành 2130706433, 017700000001, hoặc 127.1
* Đăng kí tên miền trả về 127.0.0.1
* Dùng các kĩ thuật encode URL để bypass bộ lọc 

### Whitelist 
Ngược lại blacklist, chỉ cho truy cập các URL trong whitelist. Cách này dựa vào việc làm rối loạn việc phân tích một url bằng cách lợi dụng các thuộc tính sẵn có
của URL. 
`http://username@host:port/path#fragment`  
* Thêm thông tin đăng nhập vào trước URL bằng @ **VD** http://example.com@127.0.0.1 hoặc http://127.0.0.1@example.com  
* Thêm # để tạo ra URL fragment **VD** http://127.0.0.1#example.com  
* Lợi dụng tính năng phân cấp DNS để tạo ra một tên miền dưới domain mà mình quản lý **VD** http://domain.127.0.0.1  
* Dùng các kĩ thuật encode URL, kí tự viết hoa như blacklist để bypass bộ lọc  
* Có thể kết hợp nhiều kĩ thuật với nhau 

### SSRF filter thông qua lỗ hổng open redirection 
Đôi khi máy chủ được triển khai một bộ lọc để tránh khai thác lỗ hổng SSRF nhưng lại không kiểm duyệt yêu cầu chuyển hướng khiến ứng dụng bị dính lỗi open redirection. Trong trường hợp này vẫn có thể tận dụng lỗi open redirection để khai thác SSRF
**VD:** /product/nextProduct?currentProductId=6&path=http://evil-user.net sẽ điều hướng người dùng đến trang web của kẻ tấn công
```
*POST /product/stock HTTP/1.0*
*Content-Type: application/x-www-form-urlencoded*
*Content-Length: 118*

*stockApi=http://weliketoshop.net/product/nextProduct?currentProductId=6&path=http://192.168.0.68/admin* 
```  

### Blind SSRF
Blind SSRF xảy ra khi máy chủ thực hiện các yêu cầu tới URL được cung cấp nhưng phản hồi của nó không được trả về trên trang web. Blind SSRF khó để khai thác hơn nhưng vẫn có thể thực hiện để chạy shell trong một số trường hợp. Blind SSRF có thể ít gây ảnh hưởng hơn so với loại thông thường nhưng cũng có thể được dùng để 
thu thập thông tin về ứng dụng.  

## Ảnh hưởng  
Lổ hổng SSRF giúp attacker có toàn quyền trên ứng dụng nếu truy cập được vào admin page.  
Ngoài ra SSRF có thể kết hợp với các cách tấn công khác để truy cập, đọc file tùy ý, RCE  

## Phát hiện lỗ hổng SSRF
### Partial URLs in requests
Một số trang ứng chỉ đặt một phần của đường dẫn trong các request gửi lên, đường dẫn hoàn chỉnh sẽ được thêm vào khi server xử lý yêu cầu. Các đường dẫn có thể bị 
đoán ra bằng phương pháp brute force. SSRF vẫn có thể xảy ra, tuy nhiên có một số hạn chế do không thể điều khiển hoàn toàn đường dẫn khi gửi request 
### URL trong các data format 
Các ứng dụng có thể truyền dữ liệu dưới các định dạng đặc biệt(xml, json,..) cho phép đính kèm dường dẫn vào request. Các đường dẫn này có thể dược truy vấn bởi 
các trình phân tích dữ liệu của các định dạng đó. Một ví dụ nổi bật là định dạng xml, khi một ứng dụng chấp nhận định dạng xml và phân tích nó. Có thể xảy lỗi
**XXE** dẫn đến **SSRF  thông qua XXE** 
### Blind SSRF  
Đối với blind SSRF, response sẽ không được trả về frontend. Do đó ta cần phải dùng kĩ thuật out-of-band để xác định xem web apps có thể bị SSRF hay không. 

## Migitation  
Để đảm bảo server không bị SSRF cần verify kĩ các URL, IP. Do input bh là các URL, IP nên cần phải có bước verify IP, Domain  
1. Verify user input tương tự như các trường hợp thông thường  
2. Sử dụng blacklist, allowlist, hạn chế các đường route từ webapp tới các localserver khác để giới hạn các việc truy cập tới các server khác trong mạng local  
3. Với các ứng dụng cho phép kết nối tới các external URL, cần đảm bảo không thể gọi tới các internal URL  
  - Đối với IP: verify để đảm bảo đó nếu là IP public thì nó không liên quan đến mạng global của tổ chức, đảm bảo nó không phải là IP private  
  - Đối với domain: dùng các internal DNS resolver để query các domain được user gửi lên để xác nhận rằng nó không phải là domain local. Cần đảm bảo ko bị `DNS pining`  
4. Đối với các ứng dụng nhận các token từ userinput. Cần đảm bảo chỉ có chứa các kí tự hợp lệ  
5. Verify giao thức (HTTP, HTTPS, gopher, file,..) để đảm bảo ứng dụng yêu cầu đúng như kết nối  

**Ref**  
https://cheatsheetseries.owasp.org/cheatsheets/Server_Side_Request_Forgery_Prevention_Cheat_Sheet.html#available-protections_1  
https://book.hacktricks.xyz/pentesting-web/ssrf-server-side-request-forgery  
https://portswigger.net/web-security/ssrf  
