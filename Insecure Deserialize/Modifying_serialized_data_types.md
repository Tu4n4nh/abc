## Overview  
Mục tiêu của bài này là đăng nhập được vào administrator để xóa user Carlos. Gợi ý để bài cho là dựa vào cách so sánh của PHP. Từ gợi ý có thể hiểu bài này ta lợi
dụng việc so sánh lỏng lẻo của PHP [link](https://owasp.org/www-pdf-archive/PHPMagicTricks-TypeJuggling.pdf)  

Đăng nhập vào user, lấy cookie và decode  
![image](https://user-images.githubusercontent.com/22276823/132993620-3d6d7297-c83b-4ef4-b2f2-74711f321d1f.png)  
![image](https://user-images.githubusercontent.com/22276823/132993675-0c3c859a-48d7-4088-b6dd-c6a8f219df4c.png)  

Ta thấy cookie này có biến username, đổi giá trị của nó thành `administrator` rồi gửi thử  
![image](https://user-images.githubusercontent.com/22276823/132993813-7f04c76d-edc7-42b7-9204-f6ce3adc7287.png)  
Lỗi cho thấy `access_token` không chính xác. Đến lúc này ta sử dụng gợi ý của đề bài. Lợi dụng việc so sánh lỏng lẻo của PHP để bypass  

![image](https://user-images.githubusercontent.com/22276823/132993868-0bee5e48-b57d-47d7-882c-e5620ddcc11b.png)  
![image](https://user-images.githubusercontent.com/22276823/132993882-50305401-6e11-425d-9968-5bd9953399e4.png)  

Bapass thành công  
![image](https://user-images.githubusercontent.com/22276823/132993914-910ff69e-e7ca-46b0-9860-b3fdbc9d46b8.png)




