# Overview  
Cross-site scripting (XSS) là lỗ hổng web cho phép kẻ tấn công có thể thực thi tùy ý các đoạn mã độc hại
Nó vỡ phá chính sách same-origin policy trên mỗi trang web.  

# How does XSS work  
XSS hoạt động bằng cách gửi các đoạn mã độc hại lên máy chủ web. Máy chủ web xử lý request mà gửi trả về response có kèm mã độc hại. Trình duyệt web sẽ
thực thi mã độc hại này.

# The types of XSS attacks  
XSS được chia làm 3 dạng khác nhau, dựa trên cách thức hoạt động  
1. Reflected  
2. Store (persistent)  
3. DOM  

## Reflected XSS  
Đây là dạng tấn công khi mà input đầu vào của user được trả về hiện thị lại trên giao diện web. Máy chủ sẽ nhận yêu cầu gửi lên và đính kèm nội dung input
của user tại response ngay sau đó. 
```
 https://insecure-website.com/status?message=All+is+well.

<p>Status: All is well.</p> 
```
Ở ví dụ trên có thể thấy, nội dung của biến `message` sẽ được chèn vào tag `<p></p>` trong response. Kẻ tấn công có thể lợi dụng để gửi lên 1 đoạn JS. Khi máy chủ
web gửi về response, đoạn JS sẽ tự động được thực thi  
```
https://insecure-website.com/status?message=<script>alert(1);</script>
<p>Status:<script>alert(1);</script></p>
```

## Store XSS
Đối với dạng tấn công reflected, kẻ tấn công phải lừa user nhấp vào link độc hại thì mới có thể thực thi được đoạn mã. Với Store XSS, kẻ tấn công sẽ không cần như vậy 
vẫn có thể gây ra XSS. Store XSS thường xảy ra với các trang web có chức năng lưu trữ input của người dùng để hiện thị lại lên trang web( chức năng comment), 
khi đó kẻ tấn công sẽ thực hiện gửi một payload lên server, payload này sẽ được lưu lại database, mỗi khi user truy cập trang web, máy chủ sẽ load payload lên và gây
ra XSS  
```
POST /post/comment HTTP/1.1
Host: vulnerable-website.com
Content-Length: 100

postId=3&comment=This+post+was+extremely+helpful.&name=Carlos+Montoya&email=carlos%40normal-user.net 
``` 
`<p>This post was extremely helpful.</p>`  
Trên đây là vs dụ một request `Post` thực hiện chức năng comment trên trang web. Mõi lần user truy cập, comment sẽ tự động được load lên và hiện thị trên giao diện
```
POST /post/comment HTTP/1.1
Host: vulnerable-website.com
Content-Length: 100

postId=3&comment=%3Cscript%3Ealert(1);%3C%2Fscript%3E&name=Carlos+Montoya&email=carlos%40normal-user.net 
``` 
`<p><script>alert(1);</script></p>`  
Nếu kẻ tấn công gửi lên một đoạn payload, thì khi user truy cập, payload sẽ tự động được load lên và thực thi  

## DOM XSS  
DOM XSS là lỗ hổng lợi dụng các trang web có thể thao tác lên Document Object Model (DOM) để thực thi JS. Với DOM XSS dữ liệu đầu vào của user sẽ được xử lý và thực
thi ngay tại client. Vì thế đây là một lỗ hổng client-site. Về cách thức khai thác thì DOM XSS cũng tương tự như Reflected XSS nhưng DOM XSS xử lý dữ liệu ngay tại 
client, việc import dữ liệu sẽ do client thực hiện, nghĩa là dữ liệu được gửi lên server và dữ liệu DOM xử lý sẽ là 2 luồng khác nhau. Còn Reflected XSS, dữ liệu được gửi lên server sau đó được trả về, việc 
import dữ liệu vào trang web sẽ do server thực hiện, nghĩa là dữ liệu chỉ có 1 luồng xử lý tại server.  
`http://www.example.com/userdashboard.html?context=Mary`  
```
</head>
Main Dashboard for
<script>
	var pos=document.URL.indexOf("context=")+8;
	document.write(document.URL.substring(pos,document.URL.length));
</script>
```  
Đoạn `<script>...</script>` trên có chức năng lấy ra giá trị của biến `context` và ghi vào HTML. Out put của ví dụ trên sẽ là `Main Dashboard for Mary`  
Nếu kẻ tấn công không gửi một chuôi thông thường mà sẽ là `http://www.example.com/userdashboard.html?context=<script>alert(1);</script>`. Khi đó output  
```
</head>
Main Dashboard for <script>alert(1);</script>
<script>
	var pos=document.URL.indexOf("context=")+8;
	document.write(document.URL.substring(pos,document.URL.length));
</script>
```  
# Impact  
Với XXS, kẻ tấn công có thể lấy cắp cookie của user, truy cập vào tài khoản với đầy đủ các chức năng của user. Nếu user có quyền quản trị, kẻ tấn công sẽ có quyền
điều khiển hệ thống, thu thập thông tin của toàn bộ user
  
# Detect XSS  
Để phát hiện XSS, cần phải đọc source code của trang web, xác định được các điểm nơi mà user input đầu vào > xác định đường đi của dữ liệu và cách mà chúng được 
xử lý  
Với DOM XSS, ta có thể review các đoạn JS, tìm kiếm những điểm mà JS thao tác với DOM  (`document.URL`, `innerHTML`) để phân tích cách chúng xử lý dữ liệu  
  
# Prevent 
Để ngăn chặn XXS, chúng ta cần phải làm sạch dữ liệu đầu vào
1. Escape HTML character   
2. Disallow bad content  
3. Clean content, loại bỏ các tag nguy hiểm(`<script>`)  
4. Strip content, không cho chèn bất kì tag HTML nào vào nội dung  
5. Replace content, xây dựng một danh sách user nhập nontag HTML sẽ tự động chuyển thành tag HTML  


*Reference*  
[https://portswigger.net/web-security/cross-site-scripting](https://portswigger.net/web-security/cross-site-scripting)  
[https://www.acunetix.com/blog/articles/dom-xss-explained/](https://www.acunetix.com/blog/articles/dom-xss-explained/)  
[https://happycoding.io/tutorials/java-server/sanitizing-user-input#sanitizing-user-input](https://happycoding.io/tutorials/java-server/sanitizing-user-input#sanitizing-user-input)  


