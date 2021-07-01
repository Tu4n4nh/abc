## Broken brute-force protection, IP block  

![image](https://user-images.githubusercontent.com/22276823/123904904-7e79c580-d99b-11eb-8512-28e679bbb198.png)  

![image](https://user-images.githubusercontent.com/22276823/123904952-93565900-d99b-11eb-88c4-6e2f0aa2c7d7.png)

![image](https://user-images.githubusercontent.com/22276823/123904972-9cdfc100-d99b-11eb-870b-707ee7a391bb.png)

As usual, to brute force, i capture the request and send it to the Burp Intruder. Because Web application will block if you input wrong password many time, i create
a wordlist that alternates between right accout and account needs to brute force 

![image](https://user-images.githubusercontent.com/22276823/123905076-d44e6d80-d99b-11eb-84fe-5eba23bc4353.png)

I have setup to decrease in the number of threads to be sure request is sent alternate acounts


![image](https://user-images.githubusercontent.com/22276823/123904860-643fe780-d99b-11eb-9e6c-955168a560e3.png)

After running the attack, find the response that returns 302. It's user/password you're finding
