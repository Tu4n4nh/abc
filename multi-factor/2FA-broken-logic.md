## 2FA broken logic  
Following the login request. After login success, the web app will set cookie `verify=account`. then the web app send the security code to mail of user in cookie.
So, i try to change the value of cookie to target account  
![image](https://user-images.githubusercontent.com/22276823/124102123-47cea880-da8a-11eb-8762-05bd5c0c476d.png)  

![image](https://user-images.githubusercontent.com/22276823/124102286-6a60c180-da8a-11eb-8017-52c7cef7cde3.png)  

Now, the security code have been sent to target's mail.  I capture the POST `login2` and brute force security code. Filter the response with status 302

![image](https://user-images.githubusercontent.com/22276823/124102459-967c4280-da8a-11eb-9d08-bc98a2cba65a.png)  

![image](https://user-images.githubusercontent.com/22276823/124102594-b3187a80-da8a-11eb-86aa-e7ec26396c7d.png)


