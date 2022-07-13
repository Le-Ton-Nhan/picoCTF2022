# Packets Primer
## Description
> AUTHOR: LT 'SYREAL' JONES       
> Download the packet capture file and use packet analysis software to find the flag.        

[Download packet capture](https://artifacts.picoctf.net/c/199/network-dump.flag.pcap)

## Solution
Dùng `wireshark` để mở, ta thấy chỉ có 9 packet:          
![image](https://user-images.githubusercontent.com/62021009/162920078-6564a645-103c-4607-a3f7-398e1bf4b896.png)      
Follow TCP Stream và ta có flag:
![image](https://user-images.githubusercontent.com/62021009/162920181-23b3f28a-c5c7-405b-b756-bd41aa7aa6f4.png)    

### Flag: `picoCTF{p4ck37_5h4rk_ceccaa7f}`
