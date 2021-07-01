## 2FA simple bypass  

![image](https://user-images.githubusercontent.com/22276823/124087956-b3117e00-da7c-11eb-9872-671e6740f9d8.png)

Following the list request history, i know that the web application use 2 steps for checking the login credentials  
1. First, I input user/pass and submit. If true, go to step 2 or return false status  
2. Next, Return form which requests security code. If right code return login success or return false status  

![image](https://user-images.githubusercontent.com/22276823/124089213-e4d71480-da7d-11eb-8668-30c49ff9b4cb.png)

![image](https://user-images.githubusercontent.com/22276823/124089385-0df7a500-da7e-11eb-99e6-9c42c7ef18ca.png)

You can look at 2 image above. One case when i submit right the login credentials and another is false. In the first case, the web appilication will redirect to
`login2` to check security code. IN contrast, the webapp will be return `login` with status false. That's mean, web app check value i submit before redirect. So, i 
can bypass by redirect to `my-account` without `login2`  

![image](https://user-images.githubusercontent.com/22276823/124091782-5f089880-da80-11eb-8b41-2c2e7d059188.png)
