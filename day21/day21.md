1. What is the file hash for db.exe?  
go to Documents folder. I found the file  has hash. Answer `596690FFC54AB6101932856E6A78E3A1`  

2. What is the file hash of the mysterious executable within the Documents folder?  
In Documents folder, there are 2 file, file hash and file excuteable db. Using `Get-FileHash -Algorithm MD5 file.txt` to create hash for executable file. 
Answer `5F037501FB542AD2D9B06EB12AED09F0`  

3. Using Strings find the hidden flag within the executable?  
Using command `c:\Tools\strings64.exe -accepteula file.exe`, i found the flag  with format `thm{...}`. Answer `THM{f6187e6cbeb1214139ef313e108cb6f9}`  

4. What is the flag that is displayed when you run the database connector file?  
As instructed, the executable file has been replacing, so it can run correctly. Run command `Get-Item -Path file.exe -Stream *`. I see the stream attribute is redirected
to an alternate data stream. So, I use `wmic process call create $(Resolve-Path file.exe:streamname)` to launch hidden file  and get flag
Answer `THM{3088731ddc7b9fdeccaed982b07c297c}`  
