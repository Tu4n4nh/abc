## Overview  
Các bước deploy một web app chạy django, postgresql, memcache  
Heroku có 2 dạng deploy app: `buildpack` và `cotainer`  
`Container` lại có 2 loại deploy là: `docker registry`/`heroku registry` và `build manifest`. Hướng dẫn này chia sẻ cách build một container theo cách `build manifest`  

## Deploy app  
***Requirement***   
1.  Tạo tài khoản heroku, tạo app, kết nối với github, chuyển app sang dạng `container`    
2.  Tạo addons postgresql, memcachier
3.  Project django, dockerfile  
    - Các lib python cần thiết  
        - dj-database-url  
        - whitenoise  
        - gunicorn  
        - django-heroku  
        - django-bmemcached  
        - pytz  
4. Các lib python cần thiết  


### Tạo tài khoản heroku, tạo app, kết nối với github, chuyển app sang dạng `container`  
Sau khi tạo tài khoản, đăng nhập vào trang `dashboard` để tạo app  
![image](https://user-images.githubusercontent.com/22276823/128589567-c34dfdf2-824f-415b-8395-d9a10d31c0ef.png)  
Mặc định heroku sẽ cho tạo app theo kiểu `buildpack`  
![image](https://user-images.githubusercontent.com/22276823/128589576-253eb9a9-1f67-4b1e-a34d-f2d60d4d9474.png)  
Sau khi tạo app, `dashboard` sẽ xuất hiện app mới tạo  
![image](https://user-images.githubusercontent.com/22276823/128589618-1da4b1ba-272c-4759-9794-6af7626cf79e.png)  
Sau khi tạo app thành công, heroku cung cấp 3 cách deploy app: `heroku git`, `github`, `cotainer registry`. Bài này sử dụng cách kết nối với `github`  
  
Cài đặt `heroku-cli` https://devcenter.heroku.com/articles/heroku-cli  
Sau khi cài đặt và đăng nhập vào heroku bằng `heroku-cli`. Ta chuyển app của mình sang dạng `container`  
```
heroku stack:set container -a <app-name>  
```  
Vào setting để kiểm tra lại. Sau khi chuyển đổi thì app sẽ được chuyển tại lần deploy tiếp theo  
![image](https://user-images.githubusercontent.com/22276823/128589859-05f7411b-d6de-411f-ade7-fdcee196b377.png)  

### Tạo addons postgresql, memcachier  
Có 3 cách để tạo addons: cấu hình trong `heroku.yml`, cấu hình qua web interface, dùng `heroku-cli`. Bài này dùng cách cấu hình qua web interface  
Tại `dashboard` của app, vào mục `Resource`  
![image](https://user-images.githubusercontent.com/22276823/128590230-4cea5808-15e1-45be-98fe-ba3c8cb5b93e.png)  

Phần này sẽ hiển thị các `dynos` và `addons` đang được chạy trên app. Heroku triển khai các app theo mô hình container, mỗi container được xem là một `dynos`, hàng 
free thì được 1 `dynos` chạy đồng thời. Các addons là các manage service giúp mình triển khai app dễ dàng hơn và heroku dễ tính tiền hơn. Trong bài này, `dynos` sẽ
là `web` container, addons sẽ là `postgresql` và `cache`  

Để thêm 1 addons, ta tìm addons cần thêm  và submit  
![image](https://user-images.githubusercontent.com/22276823/128590588-524fe8f9-7351-4bdc-b445-effe8ab99552.png)  
![image](https://user-images.githubusercontent.com/22276823/128590598-08d0317e-5d97-464d-8f2a-911c1321c291.png)  

***lưu ý***  
Có những addons cần thẻ credit để submit mặc dù nó có phiên bản free. vd `Memcachier`  
Sau khi đã thêm addons, tại `Resource` sẽ xuất hiện các addon đã thêm  

![image](https://user-images.githubusercontent.com/22276823/128590691-e27edcba-d81b-45cd-ba6b-c14c8ea4ebed.png)  
Heroku sẽ setup và trả về các thông tin cần thiết cho mình connect vào addon. Chi tiết qua phần tiếp theo  


