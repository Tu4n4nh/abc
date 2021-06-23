***What type of privilege escalation involves using a user account to execute commands as an administrator?***  
As instruction, a vertical privilege escalation attack involves exploiting a vulnerability that allows you to perform actions like commands or accessing data acting as a higher privileged account such as an administrator.
Answer `vertical`  

***What is the name of the file that contains a list of users who are a part of the sudo group?***  
Answer `sudoers`  

***You may find uploading some of the enumeration scripts that were used during today's task to be useful***  
Use ` find / -perm -04000 -type f 2>/dev/null` to find processes which is set SUID. I found `/bin/bash`. `/bin/bash` auto run 
which $USER priviles, but if it's set flag `-p`, `/bin/bash` will run with owner priviles  

***What are the contents of the file located at /root/flag.txt?***  
Answer `thm{2fb10afe933296592}`  

