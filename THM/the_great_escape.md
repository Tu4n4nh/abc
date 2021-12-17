## Overview 
This machine learn you how to extract data from `.git` folder 

## Write up 
### Enumeration 
Run `nmap`, `ffuf`, check interesting files, the web server has rate limit connection so, i can't run `ffuf`  

![image](https://user-images.githubusercontent.com/22276823/146551129-d1ace12c-c4ed-43a2-853d-05e83f7f936c.png) 

![image](https://user-images.githubusercontent.com/22276823/146551337-c52695bb-791d-45be-b84b-13f1477bd62c.png) 

![image](https://user-images.githubusercontent.com/22276823/146551222-fc28de73-575d-4a32-acb9-49b18526b420.png) 

![image](https://user-images.githubusercontent.com/22276823/146551273-a9cfb386-e9e0-46e2-9420-7212c759af09.png) 

![image](https://user-images.githubusercontent.com/22276823/146551730-5e945ad0-c418-41f9-867e-81df3f8d739c.png) 

After enumeration, i got the first flag 

![image](https://user-images.githubusercontent.com/22276823/146552432-53b6077d-ce1a-4828-89f1-60f1f196bc3a.png) 
 
 As the nmap's result, i know the web server has the endpoint with suffix `*.bak.txt$`. Let try with `api` and `exif-util` 
 
 ![image](https://user-images.githubusercontent.com/22276823/146552942-d98909cd-43fe-4ea8-857a-ac181841ed00.png) 
 
 ![image](https://user-images.githubusercontent.com/22276823/146552988-b9f4f9fe-ed81-438f-9918-067417164c47.png) 

Analyzing the file. I got the dev's api of server. 

![image](https://user-images.githubusercontent.com/22276823/146553159-05bda227-72dd-4616-80dd-033d19efb781.png) 

Look back the endpoint `/exif-util`. It gives the image or link of image and return the exif data of image. I have checked the SSRF, command injection but they don't success. But.
when i submit the dev's api, i detect the api uses `curl` to get link. Check command injection on endpoint. 

![image](https://user-images.githubusercontent.com/22276823/146553978-c40c6281-9c5f-411c-a0cb-acef08484c0c.png) 

Let enum the dev's api server. 

![image](https://user-images.githubusercontent.com/22276823/146554490-8cdd6e71-60b1-470b-bd3c-47ee86bcbd9e.png) 

Get content of `dev-note.txt` 

![image](https://user-images.githubusercontent.com/22276823/146554380-9e79aabb-9330-43c6-936c-aabad0a7cd2f.png) 

I found the `.git` folder. Let extract data from it 

![image](https://user-images.githubusercontent.com/22276823/146554703-dce65398-28af-4b02-9b80-09f83f07a6b1.png) 

Get content of `dev-note.txt` 

![image](https://user-images.githubusercontent.com/22276823/146555749-7ad6c9c7-12ca-4e12-a6b7-0a0ea0a95dd7.png) 

![image](https://user-images.githubusercontent.com/22276823/146555808-c2c8a4fe-20ee-4821-b8f9-cadac191ebde.png) 

Content of `dev-note.txt` 

![image](https://user-images.githubusercontent.com/22276823/146555906-d9770be8-7b3e-41f2-87c3-d81e2ab7d06d.png) 

Content of `flag.txt` 

![image](https://user-images.githubusercontent.com/22276823/146555995-f1e3bb8a-ede4-4542-ba5f-debbcd4b7a5e.png) 














