# Lookey here
## Description
> AUTHOR: LT 'SYREAL' JONES / MUBARAK MIKAIL        
> Attackers have hidden information in a very large mass of data in the past, maybe they are still doing it      

Download the data [here](https://artifacts.picoctf.net/c/294/anthem.flag.txt).

## Solution
Bài này thì đơn giản, chỉ cần `grep` là ra :3  
```
┌──(kali㉿kali)-[~/Documents/picoCTF/Lookey here]
└─$ cat anthem.flag.txt | grep picoCTF
      we think that the men of picoCTF{gr3p_15_@w3s0m3_4c479940}
```
### Flag `picoCTF{gr3p_15_@w3s0m3_4c479940}`
