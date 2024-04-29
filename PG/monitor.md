## Overview 
- Difficulty: Easy  
- OS: Linux  

## Enumeration  
- Nmap scan  
```
nmap -sV -sV -oN scan 192.168.55.136 
```  
 
![scan](https://user-images.githubusercontent.com/22276823/155135674-ff0b40f7-ce74-4caa-baa3-2c1d44e22a32.png) 
  
The server open 5 ports. The service run on port 80, 443 is Nagios XI that is web interface monitor of Nagios engine.  
 
![nagios_login](https://user-images.githubusercontent.com/22276823/155135730-4b3ac5d6-169d-4ee2-a3e1-9c4bf07af237.png)  
  
I try the default account admin/admin, admin/password, user/password,... But it didn't success. So, I google the default account is `nagiosadmin`. Try one more time and found the
credential is `nagiosadmin:admin`. After logging, I found the server's version is `5.6.0`  
 
![version](https://user-images.githubusercontent.com/22276823/155136321-47cf133e-a65d-4cb3-adf7-038644d02eea.png)  
  
Googling, I found 2 link [metasploit](https://www.rapid7.com/db/modules/exploit/linux/http/nagios_xi_mibs_authenticated_rce/) and [github](https://github.com/jakgibb/nagiosxi-root-rce-exploit).
Try metasploit first but it didn't success. 
Clone github link and run exploit. Now, To run exploit, i needed install some packages `php-curl`, `php-xml`  

![cai_php-curl](https://user-images.githubusercontent.com/22276823/155137243-d0d0cedb-0f19-4486-bc3e-9c74a22540f0.png)  
   
![cai_php-xml1](https://user-images.githubusercontent.com/22276823/155137266-958700ca-afe4-43b6-a3c8-79a25ee928a6.png)  
  
Now, run exploit and get flag  
   
![run-shell](https://user-images.githubusercontent.com/22276823/155137333-fcd81460-230f-45af-8921-002cf2c84081.png)  
   
![get flag](https://user-images.githubusercontent.com/22276823/155137361-dcd4fef6-43d7-4a3e-9f1a-da8556a634bd.png)  
  
