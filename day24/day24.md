1. What ports are open?  
Run nmap `nmap -sC -sV $IP`, i found 2 port 80,65000  

2. What's the title of the hidden website? It's worthwhile looking recursively at all websites on the box for this step.  
Find in nmap result. Answer `Light Cycle`  

3. What is the name of the hidden php page?  
After read instruction, i try common upload page's names. and found `uploads.php`. After i run `gobuster` to scan uploads directory
and found `grid`  

4. Bypass the filters. Upload and execute a reverse shell.  
As instructed, I need drop `filter.js` and double extension to bypass filter  

5. What is the value of the web.txt flag? Answer `THM{ENTER_THE_GRID}`  
6. What credentials do you find?  
Finding in web root, i found the directory of api, browsing the directory, i found credentials of mysql. Answer `tron:IFightForTheUsers`  
7. What is the name of the database you find these in? Answer `tron`  
8. Crack the password. What is it?  
Db save password in MD5, use hashcat and wordlist rockyou.txt to crack. Answer `@computer@`  
9. What is the value of the user.txt flag? Answer `THM{IDENTITY_DISC_RECOGNISED}`  
10. Which group can be leveraged to escalate privileges? Answer `lxd`  
11. What is the value of the root.txt flag? 
Flowing the instructions, get root and get flag. `THM{FLYNN_LIVES}`
