## Công nghệ 
1. lita.gg: vue.js, nginx, WAF cloudfront, corejs 
2. api.funbit.me: php 8.0.8, java, nginx 1.20.0 
3. tobsnssdk.com: nginx 
4. websocket wss://socket.im.servicecloudweb.com/v1/websocket 

## subdomain 

1. funbit.me 
```
api.funbit.me
api-test.funbit.me
www.funbit.me
share.funbit.me
apply.funbit.me
im.funbit.me
``` 
2. tobsnssdk.com 
```
toblog.tobsnssdk.com
mcs.tobsnssdk.com
tobapplog.tobsnssdk.com 
toblog-alink.tobsnssdk.com
success.tobsnssdk.com 
ailab.tobsnssdk.com
``` 
## Directory 
1. lita.gg
```
/pages/user/orders
/pages/chat?id
/pages/user/wallet
/pages/user/edit-info
/pages/user/skill?id=
/pages/index/index?si=
/recharge
....
```
2. api.funbit.me 
```
/funbit/v2/usersearch/searchByUserNo?userNo=1439524
/funbit/v2/api/imchat/advancedList?nextSort=0
/funbit/v2/player/detail?id=440082
/funbit/v2/api/user/profile/basic
/api/user/imtoken/acquire
/api/user/account
/api/user/profile/gender/update
/api/user/profile/intro/update
/api/order/refund/accept
...
```
3. mcs.tobsnssdk.com 
```
/v2/event/list
/v2/user/ssid
```
## Chức năng 
Login: FB hoặc điện thoại  
Chat: https://www.lita.gg/pages/chat  
Nap tien: https://www.lita.gg/recharge  
Edit profile: https://www.lita.gg/pages/user/edit-info (upload image, voice, sua thong tin ca nhan)  
Order: https://www.lita.gg/pages/user/orders  
Xem thông tin người chơi: https://www.lita.gg/pages/user/skill?id=  

## Js file 
https://vi.lita.gg/static/js/chunk-vendors.e7826bb1.js  
https://vi.lita.gg/static/js/index.d3ea7f13.js (trang chủ, thông tin dường dẫn api)  
https://vi.lita.gg/static/js/pages-chat~pages-index-index~pages-user-edit-info~pages-user-orders~pages-user-skill~pages-user-wallet.644a92da.js (thông tin user)  
https://vi.lita.gg/static/js/pages-index-index.63130a47.js  
https://vi.lita.gg/static/js/pages-index-index~pages-moving-apply.9160369e.js  

## Testing  
Tính năng nạp tiền: test sql, đổi giá tiền 
