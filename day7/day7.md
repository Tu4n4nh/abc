![image](https://user-images.githubusercontent.com/22276823/122678215-5341ea00-d1d5-11eb-9450-71ce64a0caef.png)  

***Open "pcap1.pcap" in Wireshark. What is the IP address that initiates an ICMP/ping?***  
`10.11.3.2`  

***If we only wanted to see HTTP GET requests in our "pcap1.pcap" file, what filter would we use?***  
`http.request.method==GET`  

***what is the name of the article that the IP address "10.10.67.199" visited?***  
![image](https://user-images.githubusercontent.com/22276823/122678304-bd5a8f00-d1d5-11eb-9b3f-edee83930415.png)  

Filter with `HTTP` protocol, i found request to `GET /posts/reindeer-of-the-week/ HTTP/1.1`. Answer `reindeer-of-the-week`  

***Look at the captured FTP traffic; what password was leaked during the login process?***  
![image](https://user-images.githubusercontent.com/22276823/122678403-3fe34e80-d1d6-11eb-92c0-6850e15569b8.png)  
  
Filter `FTP` protocol, i found `plaintext_password_fiasco`.  

***what is the name of the protocol that is encrypted?***  
Answer `SSH`  

***What is on Elf McSkidy's wishlist that will be used to replace Elf McEager?***  
![image](https://user-images.githubusercontent.com/22276823/122678484-a5cfd600-d1d6-11eb-8415-06c06043de63.png)  

Follow the TCP stream, i found 1 zip file transfered between client and server. Using `File` > `Export Objects` > `HTTP` > choose zip file and save.  Extract zip
i found answer in text file. Answer `Rubber ducky`  


