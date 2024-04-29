# Overview  
Cross-site request forgery is a vulnerability that allows attacker to induce the user to perform actions
which they don't intend to perform.  

# Impact  
In a successful CSRF attack, the user can perform unintentional actions depend on what actions the user
can perform. It can be change password, delete other user, change email, make fund transfer. If the 
victim has a privileged role within the application, the attacker might be ablt to take full control
of all the application  

# How it works  
For a CSRF attack, it has three key conditions:  
1. **A relevant action**: There might be a privileged action( modify permissions for the other users) 
or an actions modify user data.  
2. **Cookie-based**: Performing HTTP requests and the application depends on session cookie to indentify
the user who has made requests  
3. **No unpredictable request parameters**: The requests dont have any parameter whose value the attacker
can't guess or detemine. For example, when perform action change password, the functions isn't vulnerable
 if it require an existing paaword.  
 
**Example**  
The HTTP request for function change password like:
```
POST /email/change HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 30
Cookie: session=yvthwsztyeQkAPzeQ5gHgTvlyxHfsAfE

email=wiener@normal-user.com 
```  
This match the conditions required for csrf  
- This is modify user data  
- The application use cookie session to indentity. There isn't no token or mechanisms in place to track user sessions.
- The attacker can easy to determine the value of the request parameter that are needed to perform the action  

The payload attacker like
```
<html>
  <body>
    <form action="https://vulnerable-website.com/email/change" method="POST">
      <input type="hidden" name="email" value="pwned@evil-user.net" />
    </form>
    <script>
      document.forms[0].submit();
    </script>
  </body>
</html> 
```  
The attacker contruct a web page containing the payload. When victim visit the attacker's web page:
- The attacker's web page will trigger HTTP request to the vulnerable web
- If user logged in to the vulnerable web, browser will automatically include victim's cookie into
the request  
- The vulnerable web will process request and change password of victim  

# How to construct and deliver a CSRF exploit  
CSRF can manually create. It's an HTML form wthin javascript that automatical submit the form. 
Burpsuite supports function CSRF PoC generator that is built in to Burp Pro  
The delivery mechisms are essentially same as reflect xss. The attacker feed their website 
that has malicious HTMl to victim or induce vimtim to visit their website  

# Prevent  
- Validate every case before action is executed  
- Using double submit cookie. When user visit, the application generate the value and set it as cookie in the user's browser. When the user perform action, the application require the value as the hidden value in form. If the server check both value match at server side, the application will accept it.  
- Generate the CSRF token, set it as hidden value in form  
- Don't click any strange link  

***ref***  
https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html#double-submit-cookie  
https://portswigger.net/web-security/csrf  


