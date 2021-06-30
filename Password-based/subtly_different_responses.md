## Username enumeration via subtly different responses  

![image](https://user-images.githubusercontent.com/22276823/123830868-ea254980-d92d-11eb-8f8e-d44bd0c63075.png)
Capture the request and send to the Burp Intruder  
In Burp Intruder, set payloads. Because the repsonse return with random length, we can't compair length's response to enumerate username. As instruction, we need to extract request to analyze. Follow the image to setup.  

![image](https://user-images.githubusercontent.com/22276823/123830945-00330a00-d92e-11eb-99d3-9b129856814b.png)
After run attack, we see the string extracted is similar. But notice to the strings, i have found 1 string don't have the dot at last. Make a note this username, that is what i need

![image](https://user-images.githubusercontent.com/22276823/123830594-a29ebd80-d92d-11eb-878d-d320652bc033.png)


![image](https://user-images.githubusercontent.com/22276823/123831217-4c7e4a00-d92e-11eb-9a75-88576b5b9cf4.png)


![image](https://user-images.githubusercontent.com/22276823/123831172-3f615b00-d92e-11eb-872a-1a31600bb4d8.png)
After found username, setup Burp to brute force password. As previous lab, i filter response code 302 and get password
