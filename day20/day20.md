1. What does Elf 1 want? 
Run command `Get-ChildItem -Hidden`, I found 1 file, answer is content of file. Answer `2 front teeth`  

2. What is the name of that movie that Elf 2 wants?
Same as above, run command in Desktop folder. Answer `Scrooged`  

3. What is the name of the hidden folder?  
Run `Get-ChildItem -Filter '*3*' -Hidden -ErrorAction SilentlyContinue`. I found folder `3lfthr3e`. Answer `3lfthr3e`  

4. How many words does the first file contain?  
Run `Get-Content -Path 'C:\Windows\System32\3lfthr3e\1.txt' | measure-object -word`. Answer `999`  

5. What 2 words are at index 551 and 6991 in the first file  
Run `(Get-Content -Path 'C:\Windows\System32\3lfthr3e\1.txt')[551]` and `(Get-Content -Path 'C:\Windows\System32\3lfthr3e\1.txt')[6991]`. Answer `Red Ryder`  

6.  What does Elf 3 want?  
Run `Get-Content -Path 'C:\Windows\System32\3lfthr3e\2.txt' -String "Red Ryder"`. It don't found the result. I change to search from pattern 
`Get-Content -Path 'C:\Windows\System32\3lfthr3e\2.txt' -Pattern "Red Ryder"` also no result. So i search only `red` `Get-Content -Path 'C:\Windows\System32\3lfthr3e\2.txt' -Pattern "Red"`
and i see, the result is string without space, i modify command `Get-Content -Path 'C:\Windows\System32\3lfthr3e\2.txt' -Pattern "RedRyder"`. Answer `Red Ryder BB Gun`  

