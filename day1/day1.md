![1](https://user-images.githubusercontent.com/22276823/122676323-2ee20f80-d1cd-11eb-99f1-9f042df6fa07.png)  
  

***What is the name of the cookie used for authentication?***  
Regíter > Login > `F12` > `Storage` > `Cookies`  

***In what format is the value of this cookie encoded?***  
Look at the cookie's value. It's format is hexadecimal  

***Having decoded the cookie, what format is the data stored in?***  
Using decode tool, decode cookie value's. We have a Json string   

![2](https://user-images.githubusercontent.com/22276823/122676441-aca61b00-d1cd-11eb-84ad-fd616dee62ec.png)  

**What is the value of Santa's cookie?**  
With the Json string after decode. We know cookie encoded from string `company` and `username`. We can create
santa's cookie by replacing santa's username in Json string and encode  
  
![3](https://user-images.githubusercontent.com/22276823/122676625-61d8d300-d1ce-11eb-9cb2-9008988e2e01.png)  

***What is the flag you're given when the line is fully active?***  
`THM{MjY0Yzg5NTJmY2Q1NzM1NjBmZWFhYmQy}`  
