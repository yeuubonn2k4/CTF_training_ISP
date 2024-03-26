# ĐỀ BÀI :

https://github.com/dxisdh/ISPCTF/blob/main/File%20chall/What%20is%20File%20Format/mizuku

## SOLUTION

- Phân tích HxD ta lưu FF E1 

![github](https://raw.githubusercontent.com/dxisdh/ISPCTF/main/File%20chall/What%20is%20File%20Format/whatisfileformat_2.png)

- Mở HxD lên để sửa lại đoạn chunk chứa định dạng ảnh: 37 48 26 93 19 10 -> FF D8 FF E0 00 10 và lưu lại.

sửa : thêm.jpeg 

![github](https://raw.githubusercontent.com/dxisdh/ISPCTF/main/File%20chall/What%20is%20File%20Format/mizuku.jpeg)
