***how many users are there on the Samba server (10.10.154.16)?***  
Run command `./enum4linux.pl -U 10.10.154.16`. Answer 3  


***Now how many "shares" are there on the Samba server?***  
With comand above, answer in `Sahre Enumeration`. Answer 4  

***smbclient //REPLACE_INSTANCE_IP_ADDRESS/**sharename***  
Use `smbclient //REPLACE_INSTANCE_IP_ADDRESS/**sharename**`. With "share", we enumerated, access with anonymous user. Answer `tbfc-santa`  

***what directory did ElfMcSkidy leave for Santa?***  
In this share, use `ls` to list all file, folder. Answer `jingle-tunes`  
