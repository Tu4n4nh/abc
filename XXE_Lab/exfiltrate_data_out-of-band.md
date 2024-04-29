## Overview  
This is a blind xxe lab. Because, It's response error when I use external entíty and parameter entity don't response file contents  
![image](https://user-images.githubusercontent.com/22276823/127676629-5c9ca912-8675-4049-a9e7-07b6dc151f75.png)  
***Parameter entity don't response file content***  

![image](https://user-images.githubusercontent.com/22276823/127676728-693716b8-bf01-4bf0-b5e4-12345da9b0ac.png)  
***External entity return error***  

But out-of-band is OK  
![image](https://user-images.githubusercontent.com/22276823/127677125-036d3bd2-19bf-40ed-97c6-f21293c9f58b.png)  

Show, I create my dtd file and setup to web app call to the file  
![image](https://user-images.githubusercontent.com/22276823/127677284-b36476ee-b509-4a49-b72b-5069150ddeeb.png)  
***My dtd file***  
The dtd file have 3 entities:
1. The `file` entity holds the content of `/etc/hostname`  
2. The `xxe` entity holds the other entity  
3. The `abc` entity is a external entiy that call the collaborator with the `file` parameter that has value is `file` entity's value  

![image](https://user-images.githubusercontent.com/22276823/127677592-ec09e1d5-6d78-40e3-96e1-f50ccb0f2697.png)  
In the request, I declare the parameter entity calls to my dtd file  

