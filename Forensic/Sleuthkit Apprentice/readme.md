# Sleuthkit Apprentice
## Description
> Download this disk image and find the flag.             
> Note: if you are using the webshell, download and extract the disk image into `/tmp` not your home directory     
[Download compressed disk image](https://artifacts.picoctf.net/c/330/disk.flag.img.gz)
## Solution
Tiến hành giải nén file, ta được file `disk.flag.img`. 
![image](https://user-images.githubusercontent.com/62021009/162721221-47eb070e-a474-4b84-9340-b43e89621ea7.png)      
Sử dụng lệnh `mmls` để xem các partition của disk    
![image](https://user-images.githubusercontent.com/62021009/162721353-2c3c4773-76d6-4ae2-ade8-9d2087ae47df.png)          
Ta chú ý 2 phân vùng linux, sử dụng lệnh `fls` để liệt kê các file có trong phân vùng, với `-o` để xác định offset phân vùng cần phân tích          
![image](https://user-images.githubusercontent.com/62021009/162721780-29b293db-db24-4d70-9dc9-8e39be4302b0.png)       
Không có gì đáng chú ý cả, tiếp tục với phân vùng còn lại      
![image](https://user-images.githubusercontent.com/62021009/162721921-8704a216-9175-4418-b870-1eda94ec8628.png)         
Ta chú ý thấy có thư mục root, thường sẽ có nhiều điều thú vị ở đây. Tiếp tục liệt kê xem trong thư mục root chứa những gì
![image](https://user-images.githubusercontent.com/62021009/162722059-e45d63da-8541-4469-9079-0b9c80fbbc03.png)          
Tiếp tục xem trong thư mục my_folder:
![image](https://user-images.githubusercontent.com/62021009/162722279-f9524c72-0152-4c30-af63-b21bd9cbbecc.png)            
Haha, như vậy là ta tìm thấy file flag.txt ở đây. Tiến hành extract file sử dụng lệnh `tsk_recover`
![image](https://user-images.githubusercontent.com/62021009/162722437-f56adab4-b192-406c-8640-c95afbd56965.png)
![image](https://user-images.githubusercontent.com/62021009/162722514-2ba685f4-72d3-43f2-953d-aae4101cbcfe.png)
![image](https://user-images.githubusercontent.com/62021009/162722565-88050ab9-3707-47fa-8ada-d00997d9ccb1.png)
### Flag: `picoCTF{by73_5urf3r_3497ae6b}`
