# Overview  
SQL injection là lỗ hổng cho phép kẻ tấn công có thể chèn các câu query tùy ý vào CSDL để khai thác
thông tin nhạy cảm, thay đổi thông tin trong CSDL, xóa CSDL  

# Impact
Ảnh hưởng của lỗ hổng này cực kì nghiêm trọng. Toàn bộ thông tin nhạy cảm của user có thể bị khai thác, thậm chí kẻ tấn công có thể lợi dụng chiếm được tài khoản admin, tấn công vào máy chủ backend.  
  
# Nguyên nhân
SQL Injection xảy ra chủ yếu do 2 nguyên nhân chính:
1. Dữ liệu đầu vào từ một nguồn không tin cậy, không có các biện pháp làm sạch dữ liệu  
2. Dữ liệu đầu vào được chuyển thẳng vào câu query  
  
# Phân loại  
Có 3 loại chính, trong mỗi loại sẽ có từng kiểu tấn công khác nhau.  
1. In-band SQL Injection  
  - Error-based  
  - Union-based  
2. Inferential SQLi (Blind SQLi)  
  - Bollean-based  
  - Time-based  
3. Out-of-band SQLi  

## In-band SQL  
Là loại tấn công xảy ra khi mà kẻ tấn công lợi dụng việc chính việc giao tiếp của webapp với cdsl. Là loại dễ khai thác nhất, dữ liệu trả về sẽ hiện thị lên trang web  
1. ### Error-based  
kẻ tấn công lợi dụng thông báo lỗi từ phía CSDL trả về để khai thác thông tin cần thiết bằng cách thêm vào câu query để gây ra lỗi  

### UNION ATTACK

Command `UNION` giúp ta lấy được dữ liệu từ bảng khác trong cùng cơ sở dữ liệu  
`SELECT a, b FROM table1 UNION SELECT c, d FROM table2 `  

**Yêu cầu**
1. số lượng column khi query trên 2 bảng phải bằng nhau  
2. Các kiểu dữ liệu trong mỗi cột phải tương thích với các truy vẫn lẻ   
VD: Khi truy vấn về column username  có kiểu dữ liệu là **varchar** thì table dược dùng phải có column có kiểu dữ liệu **varchar**  

Để có thể thực hiện **UNION attack** cần đảm bảo dáp ứng được 2 yêu cầu bên trên. Điều này được thực hiện bằng cách  
1. Xác định số column được trả về từ original query  
2. Từ số column được trả về có column nào có kiểu dữ liệu phù hợp với dữ liệu trả về từ injected query không  

### Xác đinh số column trả về từ original query  
1. Phương pháp đầu tiền là sử mệnh đề `ORDER BY`, tăng dần số cột lên cho tới khi xảy ra lỗi  
```
' ORDER BY 1--
' ORDER BY 2--
' ORDER BY 3--
```  
Số column trong mệnh đề `ORDER BY` có thể chỉ định bằng số index, không cần phải biết
tên của column. Khi số column vượt qua số column được trả về trong original query thì database sẽ báo lỗi
`The ORDER BY position number 3 is out of range of the number of items in the select list`  

2. Phương pháp thứ 2 là dùng pay load `UNION SELECT` để xác định số lượng các giá trị null  
```
' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
' UNION SELECT NULL,NULL,NULL--
```  
Nếu số lượng giá trị null không bằng với số column, database sẽ báo lỗi  
`All queries combined using a UNION, INTERSECT or EXCEPT operator must have an equal number of expressions in their target lists.`  

Trong cả hai phương pháp trên, ứng dụng thực sự có thể trả lại thông báo lỗi này hoặc có thể chỉ trả lại một lỗi chung hoặc 
không có kết quả. Khi số lượng rỗng khớp với số cột, cơ sở dữ liệu trả về một hàng bổ sung trong tập kết quả, 
chứa các giá trị null trong mỗi cột. Ảnh hưởng đến phản hồi HTTP kết quả phụ thuộc vào mã của ứng dụng. Nếu may mắn, bạn sẽ thấy một số nội dung bổ sung trong 
phản hồi, chẳng hạn như một hàng bổ sung trên bảng HTML. Với điều kiện bạn có thể phát hiện một số khác biệt trong phản hồi của ứng dụng, bạn có thể suy ra có bao nhiêu cột đang được trả về từ truy vấn.  

### Xác định những column với định dạng dữ liệu cần thiết  
Lí do để thực hiện một cuộc tấn công `UNION` vì nso trả về kết quả của câu truy vấn injection.
 Thông thường các dữ liệu nhạy cảm sẽ là một chuỗi string, khi đã xác định được số column, xác định được column nào có 
 thể chứa chuỗi string bằng cách gửi tuần tự giá trị chuỗi thay thế từng column. Ví dụ với một câu truy vấn trả về 4 column  
 ```
' UNION SELECT 'a',NULL,NULL,NULL--
' UNION SELECT NULL,'a',NULL,NULL--
' UNION SELECT NULL,NULL,'a',NULL--
' UNION SELECT NULL,NULL,NULL,'a'--
```  
Nếu dữ liệu của cột đó không phải định dạng string thì sẽ báo lỗi  
`Conversion failed when converting the varchar value 'a' to data type int.`  
Nếu không có lỗi thì trang web sẽ trả về kết quả kèm theo giá trị của string đã gửi lên.  

### Sử dụng UNION để lấy thông tin  
Sau khi đã xác định được số column và column nào có thể trả về chuối string. Nếu như số cột trả về ít hơn số cột mà ta cần truy vấn, ta có thể sử dụng nối chuỗi
để gộp thông tin của 2 cột thành 1 cột  

## BLIND SQL  
Là dạng lỗi khi trang web sẽ không trả về kết quả hoặc lỗi của câu truy vấn. Lỗ hổng này vẫn có thể bị khai thác, tuy nhiên cách khai thác sẽ khó và mất nhiều thời gian hơn  
Các phương pháp để xác định lỗ hổng:  
1.  Thay đổi logic của trang web để xác định sự thay đổi của trang web phụ thuộc vào điều kiện của câu truy vấn(Boolean-based)  
2.  Truy vấn điều kiện trigger time delay, cho phép bạn nhận biết được câu truy vấn là đúng hay sai dựa vào thời gian response(Time-based)  
  
# Cách phòng chống  
1. Filter input data bằng blacklist, whitelist  
2. Giới hạn quyền thao tác với database cho user    
3. Sử dụng parameterized queries. Tách rời tham số và câu query   
```
<?php
$stmt = $dbh->prepare ("INSERT INTO user (firstname, surname) VALUES (:f-name, :s-name)");
$stmt -> bindParam(':f-name', 'John');
$stmt -> bindParam(':s-name', 'Smith');
$stmt -> execute();
?>
```  
4. Sử dụng Stored Procedures  
5. Escape special character  
