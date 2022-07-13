# Sleuthkit Intro

## Description
>  Download the disk image and use `mmls` on it to find the size of the Linux partition. Connect to the remote checker service to check your answer and get the flag.          
>  Note: if you are using the webshell, download and extract the disk image into `/tmp` not your home directory.  
>  [Download disk image](https://artifacts.picoctf.net/c/114/disk.img.gz)      
>  Access checker program: `nc saturn.picoctf.net 52279`

## Solution
Tiến hành giải nén file. Sau đó kiểm tra file vừa giải nén thì ta thấy:       
![image](https://user-images.githubusercontent.com/62021009/162719921-6014f0cf-4aea-443a-ae67-aa425005274f.png)     
Theo như hướng dẫn của challenge, sử dụng lệnh `mmls` để tìm kích thước của Linux partition:        
![image](https://user-images.githubusercontent.com/62021009/162720061-f8e024c7-71a1-4b00-b3e6-9c0d5ec26ddb.png)
Ta có kích thước của Linux partition là: 202752. Kết nối netcat đến server để lấy flag: 
![image](https://user-images.githubusercontent.com/62021009/162720352-81e41014-ed85-4abb-b595-09d76837943c.png)       
### Flag: `picoCTF{mm15_f7w!}`
