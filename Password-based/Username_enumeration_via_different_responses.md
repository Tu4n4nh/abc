## Username_enumeration_via_different_responses  

![image](https://user-images.githubusercontent.com/22276823/123829029-4b4c1d80-d92c-11eb-9c6f-49bb4e5be757.png)  
Capture the POST request login and send this to Burp Intruder.  
In Burp Intruder, highlight value of username parameter and add payload position. Set static value for password  
In tab Payloays, choose option 'Simple lists', load list user in 'Payload Options'. When we run attack, notice to length of respone that is longer than the others. Make a note of the username in 'Payloads' column  

![image](https://user-images.githubusercontent.com/22276823/123829214-73d41780-d92c-11eb-8090-d83eca13343f.png)

Repeat steps above, Set username parameter's value that you identified. Run attack, notice to response code, you will see the response code is different than the others. Take password and login
