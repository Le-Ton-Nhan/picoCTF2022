# Operation Orchid
## Description
> AUTHOR: LT 'SYREAL' JONES        
> Download this disk image and find the flag.           
> Note: if you are using the webshell, download and extract the disk image into /tmp not your home directory.           
[Download compressed disk image](https://artifacts.picoctf.net/c/236/disk.flag.img.gz)     

## Solution
Tiến hành giải nén file, ta được `disk.flag.img`     
```
┌──(kali㉿kali)-[~/Documents/picoCTF/Operation Orchid]
└─$ file disk.flag.img        
disk.flag.img: DOS/MBR boot sector; partition 1 : ID=0x83, active, start-CHS (0x0,32,33), end-CHS (0xc,223,19), startsector 2048, 204800 sectors; partition 2 : ID=0x82, start-CHS (0xc,223,20), end-CHS (0x19,159,6), startsector 206848, 204800 sectors; partition 3 : ID=0x83, start-CHS (0x19,159,7), end-CHS (0x32,253,11), startsector 411648, 407552 sectors
```
Sử dụng lệnh `mmls` để kiểm tra các phân vùng
```
┌──(kali㉿kali)-[~/Documents/picoCTF/Operation Orchid]
└─$ mmls disk.flag.img 
DOS Partition Table
Offset Sector: 0
Units are in 512-byte sectors

      Slot      Start        End          Length       Description
000:  Meta      0000000000   0000000000   0000000001   Primary Table (#0)
001:  -------   0000000000   0000002047   0000002048   Unallocated
002:  000:000   0000002048   0000206847   0000204800   Linux (0x83)
003:  000:001   0000206848   0000411647   0000204800   Linux Swap / Solaris x86 (0x82)
004:  000:002   0000411648   0000819199   0000407552   Linux (0x83)
```
Liệt kê các file trong phân vùng linux
```
┌──(kali㉿kali)-[~/Documents/picoCTF/Operation Orchid]
└─$ fls -o 411648 disk.flag.img    
d/d 460:        home
d/d 11: lost+found
d/d 12: boot
d/d 13: etc
d/d 81: proc
d/d 82: dev
d/d 83: tmp
d/d 84: lib
d/d 87: var
d/d 96: usr
d/d 106:        bin
d/d 120:        sbin
d/d 466:        media
d/d 470:        mnt
d/d 471:        opt
d/d 472:        root
d/d 473:        run
d/d 475:        srv
d/d 476:        sys
d/d 2041:       swap
V/V 51001:      $OrphanFiles
```
Tiếp tục với file `root`
```
┌──(kali㉿kali)-[~/Documents/picoCTF/Operation Orchid]
└─$ fls -o 411648 disk.flag.img 472
r/r 1875:       .ash_history
r/r * 1876(realloc):    flag.txt
r/r 1782:       flag.txt.enc
```
Ta thấy có file flag ở đây, tiến hành extract:
```
┌──(kali㉿kali)-[~/Documents/picoCTF/Operation Orchid]
└─$ tsk_recover -o 411648 -e disk.flag.img 472
Files Recovered: 3173
┌──(kali㉿kali)-[~/…/picoCTF/Operation Orchid/472/root]
└─$ ls
flagreal.txt  flag.txt  flag.txt.enc
┌──(kali㉿kali)-[~/…/picoCTF/Operation Orchid/472/root]
└─$ cat flag.txt                                                                                                              1 ⨯
           -0.881573            34.311733
                                                                                                                                  
┌──(kali㉿kali)-[~/…/picoCTF/Operation Orchid/472/root]
└─$ cat flagreal.txt 
�r��k��jw��4
E�=dR%�g]�@���g(#��X▒K�]t�A
┌──(kali㉿kali)-[~/…/picoCTF/Operation Orchid/472/root]
└─$ cat flag.txt.enc 
Salted__A��)o�B�Aq��ncb���#�>=D� /� >4Z�������Ȥ7� ���؎$�'%
```
Có vẻ như flagreal đã được mã hoá mất. Nhìn kĩ lại thì thấy ở thư mục `root` còn có 1 file `.ash_history` lưu trữ lịch sử gõ lệnh. Thử xem thử ở đây có gì
```
┌──(kali㉿kali)-[~/…/picoCTF/Operation Orchid/472/root]
└─$ cat .ash_history   
touch flag.txt
nano flag.txt 
apk get nano
apk --help
apk add nano
nano flag.txt 
openssl
openssl aes256 -salt -in flag.txt -out flag.txt.enc -k unbreakablepassword1234567
shred -u flag.txt
ls -al
halt
```
Như vậy, file flag đã được mã hoá với aes256 có salt với  key là unbreakablepassword1234567. Tiến hành giải mã:
```
┌──(kali㉿kali)-[~/…/picoCTF/Operation Orchid/472/root]
└─$ openssl aes256 -d -salt -in flag.txt.enc -out flag.txt -k unbreakablepassword1234567
*** WARNING : deprecated key derivation used.
Using -iter or -pbkdf2 would be better.
bad decrypt
139707941811584:error:06065064:digital envelope routines:EVP_DecryptFinal_ex:bad decrypt:../crypto/evp/evp_enc.c:610:
┌──(kali㉿kali)-[~/…/picoCTF/Operation Orchid/472/root]
└─$ cat flag.txt    
picoCTF{h4un71ng_p457_0a710765}
```
### Flag: `picoCTF{h4un71ng_p457_0a710765}`
