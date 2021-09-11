## Overview  
 ![image](https://user-images.githubusercontent.com/22276823/132941055-f39da02b-a13c-42eb-8761-b22fd9943651.png)  

**Serialization** là quá trình chuyển đổi từ dạng data phức tạp (object) sang dạng stream byte. Thông thường dữ liệu sẽ được chuyển về dạng binary hoặc string tùy thuộc vào ngôn 
ngữ  
**Deserialization** ngược lại với quá trình trên, nó nhận dữ liệu là các chuỗi binary, string và thực hiện chuyển đổi ngược về dạng ban đầu. Và đây cũng là điểm mà lỗ hổng xảy ra. 

**Insecure deserialization** là lỗ hổng liên quan đến quá trình chuyển đổi dữ liệu từ các kiểu dữ liệu về dạng ban đầu của nó. Dữ liệu của người dùng không được xử lý dẫn đến kẻ tấn
công có thể lợi dụng để khai thác. Việc khai thác lỗ hổng này dựa trên việc attacker điều khiển luồng dữ liệu được gửi lên để có thể gây ra các ảnh hưởng lớn  

## Phân loại  
Có 2 loại chính:  
1. [Giả mạo dữ liệu để có thể vượt qua kiểm soát truy cập của trang web](#tamper-data)  
2. [Thay đổi luồng hoạt động của ứng dụng, khiên ứng dụng hoạt động không đúng như mục đích ban đầu](#broken-logic-flaw)  

### Tamper data  
Ví dụ một cookie của user có dạng như sau. Nhận thấy đây là kiểu đối tượng đã được serialiaze.  
`O:4:"User":2:{s:4:"name":s:6:"carlos"; s:10:"isLoggedIn":b:1;}`  
Một số ứng dụng web sử dụng serialiaze các user object để tạo cookie, token xác thực. Attacker có thể lợi dụng tạo ra chuỗi mới để nâng quyền cho user của mình  
`O:4:"User":2:{s:8:"username";s:6:"carlos";s:7:"isAdmin";b:0;}`  
Đây là kiểu tấn công cơ bản, nó vi phạm khá nhiều các tiêu chuẩn bảo mật ứng dụng web nên không còn được dùng phổ biến  

### Boken logic flaw
Đây là kiểu khai thác thường thấy và nguy hiểm nhất của lỗ hổng này. Một khi logic của ứng dụng bị thay đổi có thể gây ra hàng loạt các lỗ hổng khác mà nguy hiểm nhất là RCE. Tùy
vào ngữ cảnh, ngôn ngữ mà nó có các nguy hại khác nhau.  
Như đã nói, đây là lỗ hổng mà kẻ tấn công có thể điều khiển luồng dữ liệu của mình để có thể khai thác thành công ứng dụng web. Kẻ tấn công sẽ lợi dụng các đoạn mã sẵn có trong
ứng dụng, sắp xếp và điều khiển để chúng hoạt động như mong muốn  
Một số khái niệm:  
`Gadget`: là các method có thể gây ra lỗ hổng. Thực tế chúng không gây hại, tuy nhiên dữ liệu đưa vào chúng có thể lợi dụng để gây hại  
`Gadget chain`: là một chuỗi các gadget được nối tiếp nhau để có thể khai thác  
`Magic method`: là các hàm đặc biệt trong OOP. chúng có sẵn trong các đối tượng nhằm thực hiện các mục đích riêng biệt (constructor, destructor, toString,...)  
  
## Ảnh hưởng  
1. Thay đổi luồng hoạt động của chương trình  
2. Thay đổi thuộc tính của đối tượng  
3. Lợi dụng để gây ra các lỗi khác như RCE, Pathtraversal,...

## Hạn chế  
Để hạn chế được lỗ hổng này, cần phải xử lý kĩ càng các dữ liệu được nhận vào từ input. Quản lý các thư viện được dùng trong project để kiểm soát được các vị trí dùng đến deserialize
đối tượng  
1. Lọc dữ liệu đầu vào  
2. Quản lý các thư viện được dùng để kiểm soát việc sử dụng deserialization  

**ref**  
1 https://portswigger.net/web-security/deserialization  
