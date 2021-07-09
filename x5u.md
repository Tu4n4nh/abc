## Overview  
As `jku`, `x5u` is a key in header of jwt that defines the url save the public key certificate or the certificate chain. For more information [RFC 7516](https://datatracker.ietf.org/doc/html/rfc7516)  

For the challenge. When i decode jwt, i see it has `x5u`. 
![image](https://user-images.githubusercontent.com/22276823/125096130-5f2e1700-e0c4-11eb-8834-defecd1e7ce5.png)  

I try to change the url and submit the token. This is notification i get in response  
![image](https://user-images.githubusercontent.com/22276823/125096650-e24f6d00-e0c4-11eb-9de9-da6fd6441887.png)  

![image](https://user-images.githubusercontent.com/22276823/125096721-f1ceb600-e0c4-11eb-95ee-f1d82fe8d3b1.png)  

The server response can't see the certificate but not the url is invalid. this is mean i can try to change url abitrarily.  

The follow steps i will:
1. Create `private key` and sign my certificate  
2. Host the web that return the certificate if requested  
3. Create a new token with `private key` and `certificate`  
4. submit new token  

### Create `private key` and sign my certificate  
```
openssl req -new -newkey rsa:2048 -keyout priv.pem -out abc.csr -nodes  
openssl x509 -req -in abc.csr -signkey priv.pem -days 365 -out abc.crt
```  
![image](https://user-images.githubusercontent.com/22276823/125101080-57bd3c80-e0c9-11eb-9338-ffc0f2c4591a.png)  

### Host the web server  
![image](https://user-images.githubusercontent.com/22276823/125101187-791e2880-e0c9-11eb-9c43-aa3349d5eacf.png)  

### Create new token  

![image](https://user-images.githubusercontent.com/22276823/125101533-d619de80-e0c9-11eb-8b60-18cf49f1003a.png)  

![image](https://user-images.githubusercontent.com/22276823/125101569-e16d0a00-e0c9-11eb-88b7-d1a3078983bd.png)  

## Mitigate  
The vulnerable occurs when the server trust the all jku address. To mitigate, create a list of trusted server that allows to verify JWT

