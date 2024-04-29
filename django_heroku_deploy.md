## Overview  
Các bước deploy một web app chạy django, postgresql, memcache  
Heroku có 2 dạng deploy app: `buildpack` và `cotainer`  
`Container` lại có 2 loại deploy là: `docker registry`/`heroku registry` và `build manifest`. Hướng dẫn này chia sẻ cách build một container theo cách `build manifest`  
## Workflow  
![heroku-deployment-process-simplified](https://user-images.githubusercontent.com/22276823/129481773-5bf4e773-54f2-45e1-94bb-2991b2d155db.png)  
## Deploy app  
***Requirement***   
1.  Tạo tài khoản heroku, tạo app, kết nối với github, chuyển app sang dạng `container`    
2.  Tạo addons postgresql, memcachier
3.  Project django, dockerfile  
4.  heroku.yml, runtime.txt, Procfile    
5.  Deploy lên heroku


### 1. Tạo tài khoản heroku, tạo app, kết nối với github, chuyển app sang dạng `container`  
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

### 2. Tạo addons postgresql, memcachier  
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

### 3. Project django, dockerfile  
- Các lib python cần thiết  
    - dj-database-url  
    - whitenoise  
    - gunicorn  
    - django-heroku  
    - django-bmemcached  
    - pytz  
  
Lưu ý: viết `dockerfile`, run container, tạo project Django hoàn chỉnh, chạy thành công trên local trước sẽ deploy lên heroku.  
Sâu khi đã cài đặt xong các yêu cầu cần thiết, nếu triển khai với `DEBUG=False`, ta cần phải cấu hình `whitenoise` để load các file static  
Chi tiết xem link sau  
Cấu hình whitenoise Django: http://whitenoise.evans.io/en/stable/django.html  
Cấu hình whitenoise Django trên heroku: https://devcenter.heroku.com/articles/django-assets  

**entrypoint.sh**  
Trong context này, command run django được đặt trong file `entrypoint.sh`. Ta cần bỏ command này đi, chi tiết sẽ nói ở phần tiếp
![image](https://user-images.githubusercontent.com/22276823/128592107-477c432a-329c-4ac5-81bc-06c4e8835f69.png)  
Nếu chạy bằng `gunicorn` thì comment command `gunicorn`  
![image](https://user-images.githubusercontent.com/22276823/128592128-6dbccfaa-e7d7-4030-9830-8c607ca8d832.png)  

Disable `memcache`  
![image](https://user-images.githubusercontent.com/22276823/128592928-dfc45f9c-ed41-488c-ab55-84d3808e10dc.png)  


**Settings.py**  
Đối với local container, chúng ta chạy container `postgresql` và `cache` được cài đặt trên container django. Nhưng heroku tách cái này ra để tính tiền và cho dễ
quản lý nên ta cần chỉnh sửa lại file setting để có thể lấy được thông tin của addons.  
1. Active django-heroku  
```
import django_heroku  
# Activate Django-Heroku.
django_heroku.settings(locals())  
```  
Bước này sẽ giúp cho Django có thể lấy được các thông tin của các addons  
  
2. Connect addons  
  
Như bên trên đã đề cập, Heroku sau khi cài đặt các addons sẽ export ra các biến môi trường `variable environment`. Việc này cũng để lưu các thông tin nhạy cảm như
`SECRET_KEY`,.... Có thể xem các biến môi trường bằng cách vào `Setting` của app trên web interface, hoặc dùng `heroku-cli`  
![image](https://user-images.githubusercontent.com/22276823/128592655-28b00622-beee-49ca-b94a-67af1af3ba67.png)  
![image](https://user-images.githubusercontent.com/22276823/128592671-f16bf2dd-20e2-4076-a833-0a0c6b5e5016.png)  
  
Thêm đoạn code sau để extract info của `postgresql`  
```
import dj_database_url
DATABASE_URL = os.environ.get("DATABASE_URL")
db_from_env = dj_database_url.config(default=DATABASE_URL, conn_max_age=500, ssl_require=True)
DATABASES["default"].update(db_from_env)
```  
Đối với `cache` thêm đoạn code sau  
```
def get_cache():
    import os

    try:
        servers = os.environ["MEMCACHIER_SERVERS"]
        username = os.environ["MEMCACHIER_USERNAME"]
        password = os.environ["MEMCACHIER_PASSWORD"]
        return {
            "default": {
                "BACKEND": "django_bmemcached.memcached.BMemcached",
                # TIMEOUT is not the connection timeout! It's the default expiration
                # timeout that should be applied to keys! Setting it to `None`
                # disables expiration.
                "TIMEOUT": None,
                "LOCATION": servers,
                "OPTIONS": {
                    "username": username,
                    "password": password,
                },
            }
        }
    except:
        return {"default": {"BACKEND": "django.core.cache.backends.locmem.LocMemCache"}}


CACHES = get_cache()
```  
**insert.py** (Optional)  
Trong context của bài viết có script cần kết nối với DB nên cần sửa file này  
```
DATABASE_URL = os.environ.get('DATABASE_URL')
DATABASES = dj_database_url.config(default=DATABASE_URL, conn_max_age=500)

# Establishing the connection
conn = psycopg2.connect(database=DATABASES['NAME'], user=DATABASES['USER'], password=DATABASES['PASSWORD'], host=DATABASES['HOST'], port=DATABASES['PORT'])
```  

### 4. heroku.yml, runtime.txt, Procfile  
Đây là 3 file quan trọng, bắt buộc phải có, trong đó `runtime.txt`, `Profile` bắt buộc phải có khi deploy app trên Heroku. `heroku.yml` dùng để build container. 
Các file này được đặt tại thư mục gốc của project  
![image](https://user-images.githubusercontent.com/22276823/128593903-548e2d5a-810a-4ed5-aa86-1420294d4fb4.png)  

**heroku.yml***  
Nó tương tự như là `docker-compose.yml`, sẽ giúp build và run container image. `heroku.yml` chia quá trình deploy container ra làm các bước nhỏ.  
```
setup:
  addons:
    - plan: heroku-postgresql
      as: DATABASE
  config:
    S3_BUCKET: my-example-bucket
build:
  docker:
    web: Dockerfile
    worker: worker/Dockerfile
  config:
    RAILS_ENV: development
    FOO: bar
release:
  command:
    - ./deployment-tasks.sh
  image: worker
run:
  web: bundle exec puma -C config/puma.rb
  worker: python myworker.py
  asset-syncer:
    command:
      - python asset-syncer.py
    image: worker
 ```  
 Một file `heroku.yml` đầy đủ sẽ gồm 4 quá trình `setup`, `build`, `release`, `run`. 
 Chi tiết tham khảo: https://devcenter.heroku.com/articles/build-docker-images-heroku-yml  
 File `heroku.yml` dùng trong context bài viết  
 ```
 build:
  docker:
    web: Dockerfile.web
release:
  image: web
  command:
    - ./entrypoint.sh
run:
  web: gunicorn core.wsgi:application --bind 0.0.0.0:$PORT
```  
Lưu ý: 
- `release` sẽ chạy các command sau khi quá trình `build` hoàn tất và trước khi `run` app  
- `run` sẽ chạy command để start app lên. Nếu ko cấu hình `run` thì phải cấu hình trong `dockerfile`. Chỉ cấu hình tại một nơi  

**runtime.txt**  
File này được dùng để heroku nhận diện được ngôn ngữ và phiên bản của app đang được deploy  
![image](https://user-images.githubusercontent.com/22276823/128593631-d4f5d41a-2a77-448f-a31a-7378d5b1c33f.png)  

**Procfile**  
Heroku sẽ đọc file này đẻ nhận diện entrypoint, cái command nào được dùng để startapp  
```
web: gunicorn core.wsgi
```   
Lưu ý: `core.wsgi` sẽ thay đổi tùy thuộc vào tên `site` của project và vị trí đặt file `wsgi.py`. Mặc định khi tạo project Django, file `wsgi.py` sẽ nằm trong thư 
mục cùng với `settings.py`  
![image](https://user-images.githubusercontent.com/22276823/128594084-12a70a9a-9963-4c4f-91cf-67c0c289a2c8.png)  

Sau khi đã chuẩn bị đẩy đủ, ta đẩy toàn bộ lên github. 
  
### 5. Deploy lên heroku  
Tại `dashboard` của app, chuyển qua tab `Deploy`, chọn reposity chứa source code và connect  
![image](https://user-images.githubusercontent.com/22276823/128594221-f9561748-7bb9-415f-8cc8-aa9e32bbf3d5.png) 

Sau khi connect, có 2 tùy chọn auto và manual deploy, chọn nhánh chứa source code và nhấn deploy nếu dùng manual, Nếu dùng auto mình chỉ cần chọn nhánh, mỗi lần
push code lên là tự động deploy  
![image](https://user-images.githubusercontent.com/22276823/128594352-e5c64279-ee28-44d0-8612-a1d5a1031169.png)  
Trong quá trình deploy, ta có thể theo dõi log của từng step. Có thể vào tab `Activity` để xem lại toàn bộ log  
![image](https://user-images.githubusercontent.com/22276823/128594412-6905a725-549b-4944-8c2a-7ea7bc4232fe.png)  

Một số command `heroku-cli`  
```
heroku ps -a <app-name>
heroku ps:stop <process> -a a<app-name>
heroku run <command> -a <app-name>
```  
***ref***  
https://devcenter.heroku.com/articles/getting-started-with-python#define-a-procfile  
https://devcenter.heroku.com/categories/working-with-django  
https://devcenter.heroku.com/categories/deploying-with-docker  
https://testdriven.io/blog/deploying-django-to-heroku-with-docker/  
https://github.com/memcachier/examples-django-tasklist  
https://www.youtube.com/watch?v=Oy71OgKZbOQ&t=138s  
https://www.youtube.com/watch?v=4axmcEZTE7M&t=774s  
https://blog.heroku.com/build-docker-images-heroku-yml  






  
