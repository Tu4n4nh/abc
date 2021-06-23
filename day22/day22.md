1. What is the password to the KeePass database?  
As instructed, I realized, the folder's name of keypass is a base64 string. I guess it's a key of keypass. Decode it and paste to keypass. Answer `thegrinchwashere`  

2. What is the encoding method listed as the 'Matching ops'? Answer `base64`  
3. What is the decoded password value of the Elf Server? 
Elf Server password is found in Network, Copy encode password and decode, It's a hexdecimal string. Answer `sn0wMan!`  
![21 1](https://user-images.githubusercontent.com/22276823/123042909-0cc3d980-d3e7-11eb-9724-a72d40f99a8a.png)


4. What is the decoded password value for ElfMail?  
ElfMail's password is in Mail, It's use Html Entities encoder. Answer `ic3Skating!`   
![Screenshot at 2021-06-23 11-32-08](https://user-images.githubusercontent.com/22276823/123042984-2533f400-d3e7-11eb-9b21-69ba16c72160.png)  


5. Decode the last encoded value. What is the flag?  
The last encoded value is in Recycle Bin, I found the string in notes. It uses double charcode encode, so I need to decode 2 times. After decode, the result 
is URL link, visit the link and get the flag. Answer `THM{657012dcf3d1318dca0ed864f0e70535}`  
![Screenshot at 2021-06-01 12-29-37](https://user-images.githubusercontent.com/22276823/123042990-28c77b00-d3e7-11eb-901a-5d4dcabc00d3.png)  
