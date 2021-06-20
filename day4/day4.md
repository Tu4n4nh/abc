***Given the URL "http://shibes.xyz/api.php", what would the entire wfuzz command look like to query the "breed" parameter using the wordlist "big.txt" 
(assume that "big.txt" is in your current directory***  

`wfuzz -c -z file,big.txt http://shibes.xyz/api.php?breed=FUZZ`  

***Use GoBuster (against the target you deployed -- not the shibes.xyz domain) to find the API directory. What file is there?***  
`goubuster dir -u IP -w wordlist`. Answer  `site-log.php`  

***Fuzz the date parameter on the file you found in the API directory. What is the flag displayed in the correct post?***  
`site-log.php` receive the GET request with `date` parameter. As instruction, date parameter format is `YYYMMDD` and we need to download provided wordlist.
`wfuzz -c -z file,wordlist -u IP/api/site-log.php?date=FUZZ`  
Answer `THM{D4t3_AP1}`  
  
  
