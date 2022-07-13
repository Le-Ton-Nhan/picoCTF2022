# Operation Oni
## Description
> AUTHOR: LT 'SYREAL' JONES           
> Note: you must launch a challenge instance in order to view your disk image download link.              
> Download this disk image, find the key and log into the remote machine.            
> Note: if you are using the webshell, download and extract the disk image into /tmp not your home directory             
[Download disk image](https://artifacts.picoctf.net/c/373/disk.img.gz)          
> Remote machine: `ssh -i key_file -p 59726 ctf-player@saturn.picoctf.net`     

## Solution
Tiến hành giải nén file, ta được file `disk.img`         
```
┌──(kali㉿kali)-[~/Documents/picoCTF/Operation Oni]
└─$ file disk.img     
disk.img: DOS/MBR boot sector; partition 1 : ID=0x83, active, start-CHS (0x0,32,33), end-CHS (0xc,223,19), startsector 2048, 204800 sectors; partition 2 : ID=0x83, start-CHS (0xc,223,20), end-CHS (0x1d,81,52), startsector 206848, 264192 sectors
```
Sử dụng lệnh `mmls` để kiểm tra các phân vùng        
```
┌──(kali㉿kali)-[~/Documents/picoCTF/Operation Oni]
└─$ mmls disk.img     
DOS Partition Table
Offset Sector: 0
Units are in 512-byte sectors

      Slot      Start        End          Length       Description
000:  Meta      0000000000   0000000000   0000000001   Primary Table (#0)
001:  -------   0000000000   0000002047   0000002048   Unallocated
002:  000:000   0000002048   0000206847   0000204800   Linux (0x83)
003:  000:001   0000206848   0000471039   0000264192   Linux (0x83)
```
Liệt kê các file có trong 2 phân vùng linux 
```
┌──(kali㉿kali)-[~/Documents/picoCTF/Operation Oni]
└─$ fls -o 2048 disk.img            
d/d 11: lost+found
r/r 12: ldlinux.sys
r/r 13: ldlinux.c32
r/r 15: config-virt
r/r 16: vmlinuz-virt
r/r 17: initramfs-virt
l/l 18: boot
r/r 20: libutil.c32
r/r 19: extlinux.conf
r/r 21: libcom32.c32
r/r 22: mboot.c32
r/r 23: menu.c32
r/r 14: System.map-virt
r/r 24: vesamenu.c32
V/V 25585:      $OrphanFiles
```
Không có gì ngoài các file hệ thống. Tiếp tục với phân vùng linux còn lại
```
┌──(kali㉿kali)-[~/Documents/picoCTF/Operation Oni]
└─$ fls -o 206848 disk.img   
d/d 458:        home
d/d 11: lost+found
d/d 12: boot
d/d 13: etc
d/d 79: proc
d/d 80: dev
d/d 81: tmp
d/d 82: lib
d/d 85: var
d/d 94: usr
d/d 104:        bin
d/d 118:        sbin
d/d 464:        media
d/d 468:        mnt
d/d 469:        opt
d/d 470:        root
d/d 471:        run
d/d 473:        srv
d/d 474:        sys
V/V 33049:      $OrphanFiles
```
Tiếp tục với folder `root`
```
┌──(kali㉿kali)-[~/Documents/picoCTF/Operation Oni]
└─$ fls -o 206848 disk.img 470
r/r 2344:       .ash_history
d/d 3916:       .ssh
```
Challenge yêu cầu ta tìm key để đăng nhập vào hệ thống, và thông thường ssh key sẽ được lưu trữ trong thư mục .ssh. Tiến hành extract 2 file này:      
```
┌──(kali㉿kali)-[~/Documents/picoCTF/Operation Oni]
└─$ tsk_recover -o 206848 disk.img -e 3916
Files Recovered: 3147
```
Trong thư mục `.ssh` có 2 file key: private key và public key
```
┌──(kali㉿kali)-[~/…/picoCTF/Operation Oni/3916/root]
└─$ cd .ssh         
                                                                                                                                  
┌──(kali㉿kali)-[~/…/Operation Oni/3916/root/.ssh]
└─$ ls -a
.  ..  id_ed25519  id_ed25519.pub
┌──(kali㉿kali)-[~/…/Operation Oni/3916/root/.ssh]
└─$ cat id_ed25519  
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACBgrXe4bKNhOzkCLWOmk4zDMimW9RVZngX51Y8h3BmKLAAAAJgxpYKDMaWC
gwAAAAtzc2gtZWQyNTUxOQAAACBgrXe4bKNhOzkCLWOmk4zDMimW9RVZngX51Y8h3BmKLA
AAAECItu0F8DIjWxTp+KeMDvX1lQwYtUvP2SfSVOfMOChxYGCtd7hso2E7OQItY6aTjMMy
KZb1FVmeBfnVjyHcGYosAAAADnJvb3RAbG9jYWxob3N0AQIDBAUGBw==
-----END OPENSSH PRIVATE KEY-----
```
Đã có file key, tiến hành remote machine sử dụng lệnh mà challenge đã gợi ý, tuy nhiên chưa kết nối được vì phân quyền của file, file key phải không cho người khác truy cập. Tiến hành
![image](https://user-images.githubusercontent.com/62021009/162724775-7500addf-8355-4120-a2be-175c8d8479d5.png)
![image](https://user-images.githubusercontent.com/62021009/162724952-11fb6d05-c2e1-4f5d-80ae-c62b0a873034.png)      
Truy cập thành công :))
```
ctf-player@challenge:~$ ls
flag.txt
ctf-player@challenge:~$ cat flag.txt 
picoCTF{k3y_5l3u7h_b5066e83}
```
### Flag: `picoCTF{k3y_5l3u7h_b5066e83}`


