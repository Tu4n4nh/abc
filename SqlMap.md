# Overview  
SqlMap là tool tự động được viết bằng ngôn ngữ python để khai thác lỗ hổng Sql Injection. Hỗ trợ 5 kiểu khai thác SQL khác nhau  
1. Boolean-based  
2. Time-based  
3. Error-based  
4. Union query-based  
5. Stacked queries aka piggy backing: 

# Sử dụng  
SQL Map hỗ trợ rất chi tiết các tham số đầu vào để ta có thể khai thác hoàn chỉnh một cuộc tấn công SQL Injection. Các thành phần của SQL Map bao gồm: `Target, Request, Optimization, Injection, Detection, Techniques, Fingerprint, Enumeration, Brute Force,..`
`-v` tham số này để xác định độ chi tiết của message trả về. Có 7 level khác nhau từ `0-6`, mặc định 
là `1`.  Để set t dung `-v level`  
  - 0: Show only Python tracebacks, error and critical messages.
  - 1: Show also information and warning messages.
  - 2: Show also debug messages.
  - 3: Show also payloads injected.
  - 4: Show also HTTP requests.
  - 5: Show also HTTP responses' headers.
  - 6: Show also HTTP responses' page content.  
 
 `-r` Trong trường hợp là một request post, SqlMap hỗ trợ import file request để scan
 `sqlmap -r req.txt`  
 `-u` hoặc `--url` xác định đối tượng cần scan  
 
 **Request** hỗ trợ thêm nhiều header trong request. Một số tham số thường dùng  
 - `-A AGENT` hoặc `--user`     Chỉ định User-Agent header  
 - `-H HEADER` hoặc `--header`  Chỉ định các header khác(`X-Forwarded-For: 127.0.0.1`)  
 - `--method=`                  Chỉ định method gửi request  
 - `--data=`                    Data dùng cho method POST
 - `--cookie=`                  Set cookie cho request  
 - `--proxy=PROXY`              Set proxy cho request  
 
 **Techniques** Chỉ định các phương pháp để scan  
 - `technique=`       Chỉ đinh phương pháp test (mặc định test toàn bộ)  
      `technique` Có các phương pháp:

    -  `B`: Boolean-based blind
    -  `E`: Error-based
    -  `U`: Union query-based
    -  `S`: Stacked queries
    -  `T`: Time-based blind
    -  `Q`: Inline queries

 - `time-sec=`        Thời gian đợi phản hồi  
 - `union-cols=`      Số cột để test UNION  
 
 **Injection** chỉ định các tham số để test như parameter, dbms, os, tamper( tamper là các script tùy chỉnh)  
 - `-p`         Chỉ định tham số để test  
 - `-dbms=`     Chỉ định backend DBMS  
 - `--tamper`   Chỉ định các script bên ngoài  

**Detection** module này được sử dụng để customize lại quá trình scan
- `--level=`          level test (1-5 mặc đinh 1). 
- `--risk=`           mức độ ảnh hưởng (1-3 mặc định )  
- `--string=String`   Chuỗi match nếu điều kiện đúng  
- `--not-string=`     Chuỗi match nếu điều kiện trả về sai
- `--text-only`       Chỉ so sánh thay đổi trang web dựa vào text  

**Enumerate** Module hỗ trợ cho việc thu thập dữ liệu của DB  
- `-b`              In banner  
- `-privileges`     thu thập quyền của user  
- `--roles`         thu thập roles ucả user  
- `--dbs`           Xác định DBMS backend  
- `--tables`        Xác định table của DBMS  
- `--columns`       Xác định colums của table 
- `--count`         Đếm số dòng của table  
- `--dump`          dump ra dữ liệu của một table  
- `--dump-all`      dump ra toàn bộ dữ liệu của 1 table  
- `-D`              Chỉ định tên DBMS cần thu thập  
- `-T`              Chỉ định table  
- `-C`              Chỉ định column  
- `-U`              Chỉ định User  
- `sql-query=`      Thực thi câu query sql  
- `sql-file=`       Thực thi câu query từ file  
- `sql-shell`       Khởi tạo shell sql  

**Brute-force** scan các table, column phổ biển hoặc các file cấu hình database  
- `--common-files`     Scan file  
- `--common-tables`   Scan table  
- `--common-columns`  Scan column

**Operating system access** Module hỗ trợ truy cập vào HĐH của DBMS, khởi tạo shell  
- `--os-cmd=`     execute các command system  
- `--os-shell`    Khởi tạo interactive system shell  


`LEVEL` level chỉ định về độ rộng của một cuộc tấn công SQL, mặc địn SQLMap chỉ test 2 method GET, POST, level càng cao
SQLmap sẽ test trên cả các header request(HTTP Cookie, HTTP User-Agent)  
`Risk` risk chỉ định về độ sâu của cuộc tấn công, risk càng cao thì payloads được sử dụng càng nhiều. Tuy nhiên, có những payload sẽ gây ảnh hưởng đến performance và dữ liệu của đối tượng test nên cần chú ý. (Payload được lưu tại `/usr/share/sqlmap/data/xml/payloads/`)  
`technique` Có các phương pháp:

  -  `B`: Boolean-based blind
  -  `E`: Error-based
  -  `U`: Union query-based
  -  `S`: Stacked queries
  -  `T`: Time-based blind
  -  `Q`: Inline queries


***ref***  
[!https://github.com/sqlmapproject/sqlmap/wiki/Usage](https://github.com/sqlmapproject/sqlmap/wiki/Usage)
