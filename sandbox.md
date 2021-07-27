## Overview  
Sandbox là một cơ chế bảo mật, giúp cô lập môi trường hoạt động của một ứng dụng. Điều này hạn chế các tài nguyên khác của máy tính trong mạng cục bộ không bị ảnh 
hưởng bởi các điểm yếu của ứng dụng. Nó thường được sử dụng để kiểm tra các chương trình chưa được xác đinh, kiểm tra các bản cập nhật phần mềm để tránh ảnh hưởng đến
môi trường hiện tại, cô lập phạm vị hoạt động của phần mềm để không ảnh hưởng đến tài nguyên cục bộ.  

## Phân loại  
Phần lớn các ứng dụng ngày nay đều hoạt động dưới môi trường sandbox. Nó sẽ tạo ra một môi trường hạn chế vói low permission để hạn chế ảnh hưởng của ứng dụng lên tài nguyên cục bộ. Tùy thuộc vào nhu cầu, độ phức tạp của đối tượng cần theo dõi mà sẽ có các loại sandbox khác nhau. Các sandbox sẽ tập trung vào việc cố gắng cô lập hoàn toàn đối tượng với các tài nguyên cục bộ. Trong lĩnh vực bảo mật, sandbox có thể có thêm các tính năng như theo dõi hành vi của đối tượng, phân tích và xuất báo cáo,... Tuy nhiên có thể chia ra gồm các loại:

1. Ảo hóa môi trường  
2. Giả lập hệ điều hành  
3. Giả lập thiết bị  
4. Các loại sandbox của từng ứng dụng  

### Ảo hóa môi trường  
Loại này thường được sử dụng trong quá trình phát triển phần mềm, và cũng là loại phố biến nhất nhất. Cơ chế làm việc của loại này là sẽ tạo ra một vùng nhớ cô lập với tài nguyên cục bộ, phần mềm chỉ có thể hoạt động trong này. Mọi hoạt động tác động lên hệ thống đều bị giới hạn đến mức thấp nhất. Hầu hết các phần mềm hiện nay đều được phát triển dưa trên cách này: snap, sandboxies, windows sandbox.  
![index](https://user-images.githubusercontent.com/22276823/126892837-96af26be-752f-4e8e-aa28-4f6517e43147.png)   
Ngày này, khái niệm OS-level virtualization với đại diện là docker có thể giúp cho các developer tự chủ hơn trong việc xây dựng môi trường. Nó phù hợp cho việc phát triển các ứng dụng phía server khi đảm bảo được việc các ứng dụng được hoạt động độc lập vừa tự chủ được các thành phần trong kiến trúc: băng thông, ổ đĩa, ram,....   
Loại sandbox này chỉ quan tâm nếu không đúng với các mẫu của nó sẽ báo ra lỗi. VD như trường hợp các web browser chặn tải các file độc hại  
***Ưu điểm:*** 
Triển khai nhanh chóng, đặc biệt phù hợp với các developer cần môi trường để test các bản phá, update của ứng dụng  
Phù hợp với người dùng thông thường, cần môi trường để mở các file, đường link lạ  
Phù hợp với các ứng dụng tính độc lập giữa các process, chỉ quan tâm đến loại dữ liệu  
  
***Nhược điểm***  
Không phù hợp với yêu cầu cần để test virus hoặc malware  

### Giả lập hệ điều hành  
Loại này sẽ ảo hóa luôn một hệ điều hành đầy đủ, ứng dụng sẽ chạy và chịu sự kiểm soát của hệ điều hành ảo này, hệ điều hành ảo này sẽ chịu trách nhiệm liên lạc với các tài nguyên cục bộ. Loại này sẽ cho cái nhìn tổng quát hơn về đối tượng cần theo dõi. Phù hợp cho các yêu cầu cần theo dõi bahaviour của một loại virus hay malware. Để tạo được một máy ảo ta có thể dùng: KVM, Vmware, Virtualbox,... Hoặc có thế dùng luôn Cuckoo sandbox, project này tích hợp sẵn các công cụ theo dõi, phân tích và xuất báo cáo,...  
![1](https://user-images.githubusercontent.com/22276823/126892844-66a0e382-fcd9-4e90-9b0a-181e5f708f70.png)  
Loại sandbox này tập trung vào việc điều tra hành vi của một đối tượng. Mục đích của nó là thu thập các mẫu, template độc hại để phân tích và đưa vào các ứng dụng anti virus để dùng cho lần sau.  

***Ưu điểm***  
Có cái nhìn chi tiết hơn về hoạt động của đối tượng  
Chủ động hơn trong việc customize sandbox 

***Nhược điểm***  
Khó triển khai, tốn công hơn so với loại một  

### Giả lập hệ thống mạng  
Đây là mô hình tốn kém và tốn công nhất, mô phỏng hoàn chỉnh một hệ thống mạng. Hệ thống sandbox loại này thường được xây dựng trong các hệ thống lớn để đánh lừa các cuộc tấn công vào hệ thống công ty. Nó có khả năng chịu được các cuộc tấn công lớn, đông thời theo dõi và cảnh bảo tới trung tâm điều khiển.  Về bản chất nó là một hệ thống đầy đủ nhưng được dựng lên để đánh lạc hướng kẻ tấn công. Ví dụ của loại này điển hinh như honeynet project      

***Ưu điểm***  
Là một hệ thống mạng hoàn chỉnh, dẽ dàng tùy chỉnh với nhiều mục đích khác nhau

***Nhược điểm***  
Tốn kém chi phí triển khai  
  
### Các loại sandbox của từng ứng dụng  
Hầu hết các ứng dụng ngày nay đều có cơ chế sandbox khi phát triển lên ứng dụng. Cơ chế này đều dựa trên ý tưởng tách riêng `untrust source code`, hạn chế các method gây ảnh hưởng đến tài nguyên nội bộ, chạy các tác vụ ko an toàn trong các process con với quyền thấp hơn 
## Hạn chế SSTI  
Hầu hết các template thông dụng hiện nay đều có cơ chế sandbox để bảo vệ ứng dụng web. Cơ chế làm việc của chúng là xây dựng một bộ các class, command có thể gây lỗi, khi sandbox được kíck hoạt, source code sẽ được kiểm tra qua bộ lọc này trước khi được thực thi  
***ví dụ với template Jinja2***  
![image](https://user-images.githubusercontent.com/22276823/126982277-ca4621fb-856a-4a1d-abd1-863f3c8583de.png)  

danh sách các method ko an toàn  

***ref***  
https://searchsecurity.techtarget.com/answer/Whats-the-difference-between-software-containers-and-sandboxing  





