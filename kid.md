## Overview  
>The "kid" (key ID) parameter is used to match a specific key.  This
   is used, for instance, to choose among a set of keys within a JWK Set
   during key rollover.  The structure of the "kid" value is
   unspecified.  When "kid" values are used within a JWK Set, different
   keys within the JWK Set SHOULD use distinct "kid" values.  
   
 The vulnerability occurs when developer don't sanitilize input or use weak methods that interactive with shell system.  With the vulnerability in `kid`, attacker
 can abuse to run abitrary command.  
 
 ![image](https://user-images.githubusercontent.com/22276823/125108354-79222680-e0d1-11eb-8157-03624cb27602.png)  
 
 For the challenge, when i decode JWT, i see it's use the `kid` that save the secrect key. 
![image](https://user-images.githubusercontent.com/22276823/125108617-cef6ce80-e0d1-11eb-8b01-58e5917f1da6.png)  

I try to insert the command
![image](https://user-images.githubusercontent.com/22276823/125112567-dec4e180-e0d6-11eb-8d81-0884229043d8.png)  

![image](https://user-images.githubusercontent.com/22276823/125112613-f1d7b180-e0d6-11eb-94e4-c233f8a52753.png)  



