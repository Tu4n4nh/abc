## Overview  
Sandbox là một cơ chế bảo mật, giúp cô lập môi trường hoạt động của một ứng dụng. Điều này hạn chế các tài nguyên khác của máy tính trong mạng cục bộ không bị ảnh 
hưởng bởi các điểm yếu của ứng dụng. Nó thường được sử dụng để kiểm tra các chương trình chưa được xác đinh, kiểm tra các bản cập nhật phần mềm để tránh ảnh hưởng đến
môi trường hiện tại, cô lập phạm vị hoạt động của phần mềm để không ảnh hưởng đến tài nguyên cục bộ.  

## Phân loại  
Phần lớn các ứng dụng ngày nay đều hoạt động dưới môi trường sandbox. Nó sẽ tạo ra một môi trường hạn chế vói low permission để hạn chế ảnh hưởng của ứng dụng lên tài nguyên cục bộ. Tùy thuộc vào nhu cầu, độ phức tạp của đối tượng cần theo dõi mà sẽ có các loại sandbox khác nhau. Các sandbox sẽ tập trung vào việc cố gắng cô lập hoàn toàn đối tượng với các tài nguyên cục bộ. Trong lĩnh vực bảo mật, sandbox có thể có thêm các tính năng như theo dõi hành vi của đối tượng, phân tích và xuất báo cáo,... Tuy nhiên có thể chia ra gồm các loại:

1. Ảo hóa môi trường
2. Giả lập hệ điều hành  
3. Giả lập thiết bị  

### Ảo hóa môi trường  
Loại này thường được sử dụng trong quá trình phát triển phần mềm, và cũng là loại phố biến nhất nhất. Cơ chế làm việc của loại này là sẽ tạo ra một vùng nhớ cô lập với tài nguyên cục bộ, phần mềm chỉ có thể hoạt động trong này. Hệ đièu hành sẽ chịu trách nhiệm quản lý 
![index](https://user-images.githubusercontent.com/22276823/126892837-96af26be-752f-4e8e-aa28-4f6517e43147.png)  
![1](https://user-images.githubusercontent.com/22276823/126892844-66a0e382-fcd9-4e90-9b0a-181e5f708f70.png)  
