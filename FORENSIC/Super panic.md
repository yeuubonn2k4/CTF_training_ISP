## Challenge

![image](https://github.com/user-attachments/assets/5bdd1495-5d71-4dd1-9314-0949ada0e80e)

## Solution

- Minh se dung tool stegsolve va minh tim thay 1 QR

![image](https://github.com/user-attachments/assets/40f226a2-4c50-4c2a-9af6-f77aaeb24798)

- Sau khi quet QR duoc ispclub{h3y_br0_  -> van con 1 nua flag con thieu

- Check anh bang Hxd

![image](https://github.com/user-attachments/assets/dd5b4f5e-8a03-4afb-96ce-37b057574e01)

Một bức ảnh png sẽ kết thúc ở IEND và những thứ sau đó sẽ không được hiện thị nhưng ở bức ảnh này vẫn còn thêm 1 đoạn hex khá dài ở phía sau nữa nên mình liền nghĩ tới là chèn ảnh vào ảnh và mình đã check thử xem có tìm thấy file signature không. Và kết quả là không tìm ra gì cả. Cho đến khi mình check từ phía dưới lên:

10 00 64 94 64 A4 01 00 0E FF 8D FF mình nhận thấy có điểm gì đó lạ ở đây và mình đã đúng FF D8 FF E0 00 10 4A 46 49 46 00 01 <-- JPG signature vậy là đây là mã hex của 1 bức ảnh bị đảo sau đó chèn vào.

- Dao no lai, copy het tat ca cac dong nguoc lai

![image](https://github.com/user-attachments/assets/d3694f18-44fc-4283-aa85-0126dec23a65)


- Ta duoc flag la :

`
ispclub{h3y_br0_c4lm_d0wn}
`

