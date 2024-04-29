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

Now, the `dev-note.txt` said: 

![image](https://user-images.githubusercontent.com/22276823/146573077-9b0ffba6-2553-4ec1-829e-a1e333d634de.png)

I knock port to open tcp docker port 

![image](https://user-images.githubusercontent.com/22276823/146573441-e5eed2ec-964b-4baf-9e2d-87881dc329e4.png) 

After knock port, check port docker 

![image](https://user-images.githubusercontent.com/22276823/146573643-4ffdbd9c-5a7c-4a64-8c14-cfd5dcd23839.png) 

Connect to docker host 
```
docker -H host:2375 version 

Client:
 Version:           20.10.11+dfsg1
 API version:       1.41
 Go version:        go1.17.5
 Git commit:        dea9396
 Built:             Tue Dec 14 23:54:43 2021
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.2
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.13.15
  Git commit:       8891c58
  Built:            Mon Dec 28 16:15:09 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.4.3
  GitCommit:        269548fa27e0089a8b8278fc4fc781d7f65a939b
 runc:
  Version:          1.0.0-rc92
  GitCommit:        ff819c7e9184c13b7c2607fe6c30ae19403a7aff
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```  

Search exploit on GTFOBin and got flag 

![image](https://user-images.githubusercontent.com/22276823/146574110-b68f9eb9-b040-4c3c-8543-f48ab7ead954.png) 


**ref**  
https://gtfobins.github.io/gtfobins/docker/  
https://book.hacktricks.xyz/pentesting/2375-pentesting-docker  
[Bug Bounty Bootcamp: The Guide to Finding and Reporting Web Vulnerabilities](https://books.google.com.vn/books?id=ScwkEAAAQBAJ&pg=PT365&lpg=PT365&dq=git+cat-file+without+in+.git&source=bl&ots=DyfvCG24cH&sig=ACfU3U1-Ehq100yvM9NHiPB3xbM0aD5zWw&hl=en&sa=X&ved=2ahUKEwjmsbri7ur0AhXE8HMBHTVoDegQ6AF6BAgZEAM#v=onepage&q=git%20cat-file%20without%20in%20.git&f=false)  







