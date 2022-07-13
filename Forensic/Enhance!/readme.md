# Enhance!
## Description
> AUTHOR: LT 'SYREAL' JONES           
> Download this image file and find the flag.          

[Download image file](https://artifacts.picoctf.net/c/136/drawing.flag.svg)         
## Solution
Kiểm tra file:
```
┌──(kali㉿kali)-[~/Documents/picoCTF/Enhance!]
└─$ file drawing.flag.svg 
drawing.flag.svg: SVG Scalable Vector Graphics image
```
Kiểm tra metadata của file:
```
┌──(kali㉿kali)-[~/Documents/picoCTF/Enhance!]
└─$ exiftool drawing.flag.svg 
ExifTool Version Number         : 12.40
File Name                       : drawing.flag.svg
Directory                       : .
File Size                       : 4.1 KiB
File Modification Date/Time     : 2022:03:15 07:34:12+01:00
File Access Date/Time           : 2022:04:12 09:42:04+02:00
File Inode Change Date/Time     : 2022:03:16 03:26:33+01:00
File Permissions                : -rw-r--r--
File Type                       : SVG
File Type Extension             : svg
MIME Type                       : image/svg+xml
Xmlns                           : http://www.w3.org/2000/svg
Image Width                     : 210mm
Image Height                    : 297mm
View Box                        : 0 0 210 297
SVG Version                     : 1.1
ID                              : svg8
Version                         : 0.92.5 (2060ec1f9f, 2020-04-08)
Docname                         : drawing.svg
Metadata ID                     : metadata5
Work Format                     : image/svg+xml
Work Type                       : http://purl.org/dc/dcmitype/StillImage
Work Title                      : 
```
Không có gì cả, thử `strings` xem có gì không:
![image](https://user-images.githubusercontent.com/62021009/162909431-465ab48b-e09e-487a-9e65-3d53c4429b37.png)
![image](https://user-images.githubusercontent.com/62021009/162909620-4011fa56-b841-4fbf-a7cf-7cbaf2019946.png)     
Nối các kí tự lại, ta được flag       
### Flag: `picoCTF{3nh4nc3d_aab729dd}`
