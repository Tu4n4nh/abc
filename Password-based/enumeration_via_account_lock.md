## enumeration via account lock  

The web application will be block account if you input wrong password many. The behaviour can be exploitable by attacker to enumerate username. If there's no username, the web app responses 'Username is invalid'  

![image](https://user-images.githubusercontent.com/22276823/123979633-194dc080-d9eb-11eb-883b-6da9f10e5b64.png)  

![image](https://user-images.githubusercontent.com/22276823/123979678-266aaf80-d9eb-11eb-9a18-3e2da9788453.png)  

![image](https://user-images.githubusercontent.com/22276823/123979732-2ff41780-d9eb-11eb-9cad-759b46c12c70.png)  

![image](https://user-images.githubusercontent.com/22276823/123979826-40a48d80-d9eb-11eb-902f-f281adfff844.png)  
 The above images show you how to setup to brute force username. Each username in list will be repeat 5 times(usually, application will block after 3 times). I matched all response with 'valid' string  

![image](https://user-images.githubusercontent.com/22276823/123979942-57e37b00-d9eb-11eb-99dc-234863083347.png)  

![image](https://user-images.githubusercontent.com/22276823/123980035-6d58a500-d9eb-11eb-96fe-40d43ce273cf.png)  

After running attack, sort the 'invalid' columns, i have found 1 username don't match. The string return is `'You have made too many incorrect login attempts'`. That
is mean i found the right username

![image](https://user-images.githubusercontent.com/22276823/123981203-5bc3cd00-d9ec-11eb-919a-b357ca8bbb73.png)

![image](https://user-images.githubusercontent.com/22276823/123981427-7f871300-d9ec-11eb-91f9-3f8c700ef183.png)

![image](https://user-images.githubusercontent.com/22276823/123981723-ac3b2a80-d9ec-11eb-8121-c1844e7b043c.png)

Next, I brute force the password. The webapp has the another logic flaw when if account is blocking, you input the right password. The webapp doesn't response the string `'You have made too many incorrect login attempts'`. So this time, i match the string `'You have made too many incorrect login attempts'`. After running attack, i just filter the responses doesn't match 
