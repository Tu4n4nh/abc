![image](https://user-images.githubusercontent.com/22276823/123029844-4984d600-d3d1-11eb-9052-c22d52c974f5.png)  

1.  What is the value of local_ch when its corresponding movl instruction is called (first if multiple)?   
in screenshot we see, `mov dword[local_ch], 1`. Answer `1`  

2. What is the value of eax when the imull instruction is called  
`imul eax, dword[local_8h]`. `imul` instruction multiple des and soure and save to des. Answer `6`  
3. What is the value of local_4h before eax is set to 0?  
`mov dword[local_4h], eax`. So `local_4h`'value is set to 6. Answer `6`  
