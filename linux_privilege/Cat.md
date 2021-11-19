## Tổng quan 
`Cat` là một command trong linux có chức năng ghép và in nội dung tập tin ra màn hình. `Cat` được dùng nhiều
nhất là in nội dung của tập tin ra màn hình. Mặc định `cat` chỉ thực thi vớia người dùng chạy command `cat`.
Tuy nhiên việc phân quyền sai có thể tạo điều kiện để kẻ tấn công có thể đọc nội dung của các tập tin không
được phép 
- Có 2 trường hợp phân quyền sai cho `cat`: 
1. SUDO 
2. SUID 

## SUDO 
`sudo` là command cho phép người dùng thực thi một command với đặc quyền quản trị cao nhất. Để một người dùng
có thể dùng được `sudo`. Tài khoản của họ phải được khai báo trong `sudoers`. 
Người quản trị có thể phân quyền cho người dùng toàn quyền dùng `sudo` hoặc chỉ dùng `sudo` với các command 
nhất định. 
Khi chưa phân quyền cho người dùng  
  
![image](https://user-images.githubusercontent.com/22276823/142622588-b85c69dc-b0f3-4f52-ab83-56be164b60c6.png)  
  
Khi phân quyền cho người dùng `u1` thực thi `cat` tại thư mục `/home/os/`. Khi đó người dùng `u1` chỉ có thể
thực thi `cat` command với đặc quyền superuser tại đường dẫn `/home/os` 
  
![image](https://user-images.githubusercontent.com/22276823/142623302-1f18dcd8-b900-485d-9d99-d36c0c477649.png)  
  
![image](https://user-images.githubusercontent.com/22276823/142623169-fad7a0fa-a463-476d-b66b-a79bbb94ac2c.png)  
  
Khi không chỉ định đường dẫn, người dùng `u1` có thể thực thi `cat` command tại bất cứ đường dẫn nào với đặc
quyền superuser. 
  
![image](https://user-images.githubusercontent.com/22276823/142623557-e0db5b23-fdcf-4e2c-b21c-3abaa646e75a.png)  
  
![image](https://user-images.githubusercontent.com/22276823/142623614-a6d78da9-c932-4db7-99bd-6981cdf25cff.png)  
  
## SUID 
SUID (Set owner User ID up on execution). Thông thường khi thực thi command, câu lệnh sẽ được thực thi với quyền
của người thực thi câu lệnh. Tuy nhiên, khi được phân quyền SUID, các câu lệnh sẽ được thực thi dưới quyền của người sở hữu. 
  
![image](https://user-images.githubusercontent.com/22276823/142624271-2a59a57b-e684-47a7-a35a-3fd5335f4630.png) 
  
Điều này đặc biệt hữu dụng để giúp bảo vệ nội dung của tập tin nhưng vẫn để người dùng khác có thể tùy chỉnh được dữ liệu. 
VD `PASSWD` command. Khi người dùng thay đổi mật khẩu, mật khẩu mới sẽ được thay thế mật khẩu cũ trong tập tin `/etc/shadow`.
Ngườii dùng không thể xem, sửa đổi được tập tin mật khẩu `/etc/shadow`. Tuy nhiên với command `passwd` được phân quyền suid,
người dùng thông thường vẫn có thể thay đổi mật khẩu của mình. 
  
![image](https://user-images.githubusercontent.com/22276823/142624972-dc6576bd-83e2-4f9d-85f5-b09213b8342c.png)  
  
## Exploit 
Lợi dụng khả năng này, kẻ tấn công có thể xem được nội dung các tập tin không được phép truy cập. 
Để kiểm tra người dùng có thể thực thi `cat` command với quyền superuser hay không, ta kiểm tra tập tin sudoer 
```
sudo -l  
``` 
![image](https://user-images.githubusercontent.com/22276823/142625520-2797f292-78e4-4608-9adf-59abb2412e75.png) 
  
Để kiểm tra các tập tin được phân quyền SUID. 
```
find / -perm 4000
```  
![image](https://user-images.githubusercontent.com/22276823/142626241-e6bbfb0d-0fe7-473b-b5b2-c71f20c57c94.png) 
 
![image](https://user-images.githubusercontent.com/22276823/142626384-2828cf10-911f-4e5f-be6f-1386b69609de.png) 
 
![image](https://user-images.githubusercontent.com/22276823/142626928-ff7e0186-89f9-4db6-8c46-8a99b5f4f120.png) 

**reference** 
1. https://gtfobins.github.io/gtfobins/cat/ 
2. https://www.linuxnix.com/suid-set-suid-linuxunix/ 
3. https://www.hackingarticles.in/linux-for-pentester-cat-privilege-escalation/ 




