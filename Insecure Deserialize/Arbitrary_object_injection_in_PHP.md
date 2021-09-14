## Overview  
Bài này yêu cầu chúng ta xóa file trong thư mục của user Carlos. Gợi ý đề bài cho là xem các file backup bằng cách thêm `~` vào cuối file. Dùng burpsuite để xem 
sitemap 

![image](https://user-images.githubusercontent.com/22276823/133264768-e12d0a32-b741-48b3-9f3a-81e898c5a9ce.png)  

Nhận thấy web app gọi tới `CustomTemplate.php`, chuyển nó qua tab `repeater` và thêm `~` vào cuối file thì ta truy cập được vào sourcecode của `CustomTemplate.php`  

![image](https://user-images.githubusercontent.com/22276823/133265068-8cdbf969-d9be-41e2-82d8-8a4255521b62.png)

Nhận xét, class `CustomTemplate` sẽ xóa các file bị `lock` khi method `__destruct()` được gọi. Method `__destruct()` được gọi khi chương trình PHP kết thúc hoặc khi 
một instance của class được thực thi xong. Vậy, khi ta set giá trị cho biến `lock_file_path` là `/home/carlos/morale.txt` thì khi class này gọi `__destruct()` sẽ xóa
file ta mong muốn. 

![image](https://user-images.githubusercontent.com/22276823/133265796-08f7224a-60b1-4a77-bf74-df6450d4c597.png)

![image](https://user-images.githubusercontent.com/22276823/133265828-061423bb-d3a0-4176-8316-57cc257a57bd.png)  

Việc tiếp theo cần làm là tìm chỗ nào input payload vào để nó tiến hành deserialize payload của chúng ta. Kiểm tra ở giá trị của cookie session thì thấy nó ở dạng 
base64. Decode nó ta được 1 chuỗi PHP object. Vậy đây là nơi ta input payload vào. Decode payload và thay thế vào giá trị của cookie

![image](https://user-images.githubusercontent.com/22276823/133266668-955de878-a985-40da-b20f-8469bee4b8b3.png)

![image](https://user-images.githubusercontent.com/22276823/133266763-c9cd4770-0534-4bb3-a49c-2cd78de9f157.png)

![image](https://user-images.githubusercontent.com/22276823/133266020-f1a2aed0-0df5-4b19-a0d0-66b62eca9b67.png)


