## Overview  
Firebase cloud message là một giải pháp nhắn tin và gửi thông báo miễn phí đến các thiết bị.  
FCM Architectural Overview [Nguồn](https://firebase.google.com/docs/cloud-messaging/fcm-architecture)  
![image](https://firebase.google.com/docs/cloud-messaging/images/diagram-FCM.png)

1. App server, chịu trách nhiệm tạo ra các message, notification để gửi đến FCM backend  
2. FCM backend sẽ gắn thêm các meta data, tạo ra các message ID sau đó gửi đến lớp platform transport  
3. Platform transport layer tiếp nhận data, khi device online sẽ gửi data đến device  
4. Device là một thiết bị được cài FCM SDK  

Thông tin về các loại token của firebase [link](https://firebase.google.com/docs/cloud-messaging/concept-options#credentials)  

## FCM token  
Với kiến trúc như hình trên, FCM backend sẽ do google quản lý, để app server có thể kết nối được với FCM backend, có một số cách sau.  
1. Firebase Admin SDK, hỗ trợ NodeJs, Java, Python, C#, Go  
2. FCM HTTP V1 API, dùng HTTP protocol, Firebase Admin SDK được build trên cái này)  
3. Legacy HTTP protocol. Loại cũ, bh đã hết hỗ trợ, những app cũ vẫn tiếp tục dùng được  
4. XMPP server protocol  

Để có thể gửi request lên FCM backend từ app server. Ta cần phải xác thực với FCM backend thông qua FCM token của app server gọi là `Server key`. FCM token là một chuỗi JWT token, ta có thể xem nó trong phần `Project Setting` > `Cloud Messaging`  
![image](https://user-images.githubusercontent.com/22276823/129891099-72c4968c-919e-41f8-9737-e1a1550dc0e7.png)  

Theo như blog của [abss](https://abss.me/posts/fcm-takeover/), có 3 loại `Server key`:  
1. `FCM server key` có dạng `AAAA[A-Za-z0-9_-]{7}:[A-Za-z0-9_-]{140}`  
2. `Legacy FCM server key` có dạng `AIzaSy[0-9A-Za-z_-]{33}`  
3. `GCP key` với quyền của FCM `AIzaSy[0-9A-Za-z_-]{33}`  

Loại 2, 3 hiện tại đã end of life. Chỉ những app đã tạo trước đó mới dó. Theo như bài [blog](https://abss.me/posts/fcm-takeover/) thì có một cách để verify cái `server key` có hoạt động hay không dựa theo [document](https://firebase.google.com/docs/cloud-messaging/auth-server#authorize-http-requests) trên trang chủ  
```
api_key=YOUR_SERVER_KEY

curl --header "Authorization: key=$api_key" \
     --header Content-Type:"application/json" \
     https://fcm.googleapis.com/fcm/send \
     -d "{\"registration_ids\":[\"ABC\"]}"
```  
Chi tiết hơn có thể xem trên 2 [link](https://apoorv487.medium.com/testing-fcm-push-notification-through-postman-terminal-part-1-5c2df94e6c8d) và [link](https://apoorv487.medium.com/testing-fcm-push-notification-http-v1-through-oauth-2-0-playground-postman-terminal-part-2-7d7a6a0e2fa0) này 

## Vulnerable  
Lỗ hổng ỏ đây nằm ở chỗ giao thức legacy HTTP dùng chính `Server key` để xác thực khi gửi message, noti. Nếu `Server key` bị lộ
attacker có thể gửi các thông báo fake để lừa client (tin nhắn giả mạo ngân hàng)
Để test ta có thể dùng chính payload trên trang chủ firebase  
```
api_key=YOUR_SERVER_KEY

curl --header "Authorization: key=$api_key" \
     --header Content-Type:"application/json" \
     https://fcm.googleapis.com/fcm/send \
     -d "{\"registration_ids\":[\"ABC\"]}"
```   
Nếu trả về `200 OK` với nội dung tương tư như sau thì attacker có thể tùy ý gửi noti đến client  
![image](https://user-images.githubusercontent.com/22276823/129899917-2f9909b1-1fff-4cd6-abaa-18a984fe0169.png)  
   
Theo dõi hướng dẫn tại trang chủ [link](https://firebase.google.com/docs/cloud-messaging/send-message#send-messages-using-the-legacy-app-server-protocols) hoặc [blog](https://apoorv487.medium.com/testing-fcm-push-notification-through-postman-terminal-part-1-5c2df94e6c8d)  
```
POST https://fcm.googleapis.com/fcm/send HTTP/1.1

Content-Type: application/json
Authorization: Key=AizaSy
{
  "message":{
    "topic" : "<TOPIC-NAME> or <FCM token>",
    "notification" : {
      "body" : "This is a Firebase Cloud Messaging Topic Message!",
      "title" : "FCM Message"
      }
   }
}
```  
Lưu ý:
1. Topic name có dạng `[a-zA-Z0-9-_.~%]+`  
2. `FCM token` là Registration token theo như trang chủ firebase  



