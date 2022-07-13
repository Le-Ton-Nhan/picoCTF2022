# File types
## Description
> AUTHOR: GEOFFREY NJOGU         
> This file was found among some files marked confidential but my pdf reader cannot read it, maybe yours can.          
> You can download the file from [here](https://artifacts.picoctf.net/c/323/Flag.pdf).   

## Solution
Sử dụng lệnh `file` để kiểm tra
```
┌──(kali㉿kali)-[~/Documents/picoCTF/File types]
└─$ file Flag.pdf 
Flag.pdf: shell archive text
```
Có vẻ như đây là một file nén chứ không phải file pdf, sử dụng `binwalk` để kiểm tra xem có file nào được nén không
```
┌──(kali㉿kali)-[~/Documents/picoCTF/File types]
└─$ binwalk Flag.pdf                         

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             Executable script, shebang: "/bin/sh"
168           0xA8            Executable script, shebang: "/bin/sh' line above, then type 'sh FILE'."
3029          0xBD5           uuencoded data, file name: "flag", file permissions: "600"
```
Có tới 3 file được nén ở đây, sử dụng `binwalk -e` để giải nén:
```
┌──(kali㉿kali)-[~/Documents/picoCTF/File types]
└─$ binwalk -e Flag.pdf                                                                                                       1 ⨯

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             Executable script, shebang: "/bin/sh"
168           0xA8            Executable script, shebang: "/bin/sh' line above, then type 'sh FILE'."
3029          0xBD5           uuencoded data, file name: "flag", file permissions: "600"
┌──(kali㉿kali)-[~/Documents/picoCTF/File types]
└─$ ls
Flag.pdf
```
Nhưng không biết vì sao không thể giải nén được, đành phải tìm cách khác, sau khi quan sát thấy trong file chứa file thực thi, ta thử chạy xem sao thì extract được
```
┌──(kali㉿kali)-[~/Documents/picoCTF/File types]
└─$ ./Flag.pdf                                                                                                           
x - created lock directory _sh00046.
x - extracting flag (text)
x - removed lock directory _sh00046.
┌──(kali㉿kali)-[~/Documents/picoCTF/File types]
└─$ ls
flag  Flag.pdf
┌──(kali㉿kali)-[~/Documents/picoCTF/File types]
└─$ file flag    
flag: current ar archive
```
Sử dụng `binwalk` để xem có file nào nén trong file `flag` không
```
┌──(kali㉿kali)-[~/Documents/picoCTF/File types]
└─$ binwalk flag       

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
32            0x20            bzip2 compressed data, block size = 900k
```
Ta thấy có một file bzip2, tiến hành extract file này ra
```
┌──(kali㉿kali)-[~/Documents/picoCTF/File types]
└─$ binwalk -e flag    

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
32            0x20            bzip2 compressed data, block size = 900k

                                                                                                                                  
┌──(kali㉿kali)-[~/Documents/picoCTF/File types]
└─$ ls
flag  _flag.extracted  Flag.pdf
                                                                                                                                  
┌──(kali㉿kali)-[~/Documents/picoCTF/File types]
└─$ cd _flag.extracted  
                                                                                                                                  
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ ls
20
```
Kiểm tra file `20`
```
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ file 20  
20: gzip compressed data, was "flag", last modified: Tue Mar 15 06:50:36 2022, from Unix, original size modulo 2^32 329
```
Đổi tên và tiến hành giải nén
```
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ mv 20 20.gz                                                                                                               2 ⨯
                                                                                                                                  
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ gzip -d 20.gz 
                                                                                                                                  
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ ls
20
                                                                                                                                  
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ file 20
20: lzip compressed data, version: 1
```
Tiếp tục giải nén
```
──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ lzip -d 20                          
                                                                                                                                  
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ ls        
20.out
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ file 20.out 
20.out: LZ4 compressed data (v1.4+)
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ lz4 -d 20.out flag                                                                                                        1 ⨯
20.out               : decoded 266 bytes
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ ls                
20.out  flag
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ file flag  
flag: LZMA compressed data, non-streamed, size 255
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ mv flag flag.lzma 
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ lzma -d flag.lzma                                                                                                                                  
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ ls               
20.out  flag                                                                                                                                  
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ file flag
flag: lzop compressed data - version 1.040, LZO1X-1, os: Unix
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ mv flag flag.lzo
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ lzop -d flag.lzo
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ ls              
20.out  flag  flag.lzo
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ file flag
flag: lzip compressed data, version: 1
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ lzip -d flag 
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ ls          
20.out  flag.lzo  flag.out
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ file flag.out     
flag.out: XZ compressed data, checksum CRC64
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ mv flag.out flag.xz                                                                                                     130 ⨯
                                                                                                                                  
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ xz -d flag.xz         
                                                                                                                                  
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ ls
20.out  flag  flag.lzo
                                                                                                                                  
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ file flag    
flag: ASCII text
                                                                                                                                  
┌──(kali㉿kali)-[~/Documents/picoCTF/File types/_flag.extracted]
└─$ cat flag          
7069636f4354467b66316c656e406d335f6d406e3170756c407431306e5f
6630725f3062326375723137795f33633739633562617d0a
```
Sau một hồi giải nén điên cuồng, ta lấy được file flag có nội dung như trên. Hmm, đem đi decode hex, ta có flag :3
![image](https://user-images.githubusercontent.com/62021009/162918668-c615acdb-2aac-49d6-8821-f305a13ade04.png)      
### Flag: `picoCTF{f1len@m3_m@n1pul@t10n_f0r_0b2cur17y_3c79c5ba}`
