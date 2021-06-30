
## Username enumeration via response timing  

![image](https://user-images.githubusercontent.com/22276823/123833180-49845900-d930-11eb-9aa8-cccd7ad60048.png)

![image](https://user-images.githubusercontent.com/22276823/123833283-6587fa80-d930-11eb-9fd6-f40c7786239c.png)
Capture the request and send to Repeater. i test with some case. Finally, The behaviour has vulnerability
1. Wrong user -> fastest
2. True user True pass -> fast  
3. True user Wrong pass -> slowest  

The web application check the user first, if false, return imediately without check password. The longer pass, the longer time repsonse. 

![image](https://user-images.githubusercontent.com/22276823/123840479-8fddb600-d938-11eb-9d37-a359ed0e82e2.png)
Because, the lab also implements a form of IP-based brute-force protection. I add a custom header `X-Forwarded-For` to bypass protection. Set the password with long value. After run attack, choose the username with longest time response
![image](https://user-images.githubusercontent.com/22276823/123840585-b26fcf00-d938-11eb-90a1-3f04da3445f9.png)

![image](https://user-images.githubusercontent.com/22276823/123840631-c3204500-d938-11eb-8146-37e4c991b330.png)

![image](https://user-images.githubusercontent.com/22276823/123840824-08dd0d80-d939-11eb-911a-8df0117cb76f.png)

After found username, repeat steps to brute force password
