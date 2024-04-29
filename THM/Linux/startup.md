## Enum 
Run nmap, scan dir. I found the server run ftp service with anonymous account. The ftp service can access from browser with `/files`. I found it when running scan dir 
 
![image](https://user-images.githubusercontent.com/22276823/148799888-404438d9-0784-470d-80e5-f71f290d28fa.png) 
 
 ![image](https://user-images.githubusercontent.com/22276823/148799801-184287a6-a73b-4e42-93ce-2aa9609e92b8.png) 
  
## Upload reverse shell 
 Upload reverse shell. I found the first answer 
  
 ![image](https://user-images.githubusercontent.com/22276823/148800071-4a76fe88-14b9-4d79-8e8e-ab08a3ec924b.png) 
   
## Escalate 
 Let enum to escalate. After enum and try some attack vector but don't success. I found the www-data's folder. It has a pcap file 
 
 ![image](https://user-images.githubusercontent.com/22276823/148800501-542fc114-05fd-4f40-9ce4-b88d96b38a53.png) 
  
 Download pcap file and analyst. I found user and password to ssh. SSH with user/pass, I got `user.txt` flag  
 
 ![image](https://user-images.githubusercontent.com/22276823/148800733-da6fae38-0dec-4198-8bb3-dc4528b4bca1.png) 
 
## Get root 
Because i try all attack vector that i know to escalate. But it didn't success. At the lennie's home, this is a folder that owner by root. Now use [pspy64](https://github.com/DominicBreuker/pspy) 
to monitor process on system. I detected, the script run as crontab 

![image](https://user-images.githubusercontent.com/22276823/148801721-3fe7c2a7-4cc7-4b35-9d0a-243ac4f74b9b.png) 

![image](https://user-images.githubusercontent.com/22276823/148801854-1ca89e0d-060e-4ab3-b4ff-daed4b602cce.png) 

Now, We can control the script `/etc/print.sh` and the crontab run with root priviles. I edit `/etc/print.sh` and get `root.flag` 

![image](https://user-images.githubusercontent.com/22276823/148802171-0ffb9d02-7f52-4f08-b545-c952e106142b.png) 







