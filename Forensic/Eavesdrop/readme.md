# Eavesdrop
## Description
> AUTHOR: LT 'SYREAL' JONES     
> Download this packet capture and find the flag.

[Download packet capture](https://artifacts.picoctf.net/c/358/capture.flag.pcap)      
## Solution
Sử dụng `wireshark` để mở file. Follow TCP Stream thì ta thấy có đoạn đối thoại:        
![image](https://user-images.githubusercontent.com/62021009/162921681-c9dd8fc1-c947-43fe-8415-5876f6d6bd6e.png)        
Nội dung của cuộc nói chuyện liên qan đến 1 file gì đó, cách thức giải mã file đã được cung cấp, và file sẽ được chuyển thông qua port 9002. Tiến hành export file này, decrypt để xem có gì
![image](https://user-images.githubusercontent.com/62021009/162922164-c216bb66-7caf-4c6b-80b8-f840cda84621.png)         
Lưu file với tên `file.des3` (nhớ lưu dưới dạng `raw`), sau đó tiến hành giải mã:
```
┌──(kali㉿kali)-[~/Documents/picoCTF/Eavesdrop]
└─$ openssl des3 -d -salt -in file.des3 -out file.txt -k supersecretpassword123         
*** WARNING : deprecated key derivation used.
Using -iter or -pbkdf2 would be better.
                                                                                                                                  
┌──(kali㉿kali)-[~/Documents/picoCTF/Eavesdrop]
└─$ ls
capture.flag.pcap  file.des3  file.txt
                                                                                                                                  
┌──(kali㉿kali)-[~/Documents/picoCTF/Eavesdrop]
└─$ cat file.txt                 
picoCTF{nc_73115_411_5786acc3}
```
### Flag: `picoCTF{nc_73115_411_5786acc3}`
