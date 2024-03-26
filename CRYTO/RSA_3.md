# ĐỀ BÀI

mở file cipher rsa_3.py

có code python

class RSA:
    def __init__(self):
        self.n = 5835849141972415516562164777590600916570589166422039289302554835569696192150270851430968308687162649229377439380800096910180769127775637271780244052117395146078164421458617959724532644257601836154656137717505688047274291687336103924232013283899029988317854887436379713530001406810062108106729288439424569051388212049886935571366642883502049123
        self.flag = 'ISPCTF{....}'
    
    def encrypt(self):
        self.cipher = [pow(ord(c),1,self.n).to_bytes(length=5, byteorder='big') for c in self.flag] 
        with open('cipher','wb') as f:
            for cipher in self.cipher:
                f.write(cipher)  
                f.write('\n'.encode()) 
    def decrypt(self):
        with open('cipher','rb') as f:
            self.cipher = f.read().splitlines()
            
if __name__ == '__main__':
    rsa = RSA()
    rsa.encrypt()


  Tải source code về, ta thấy trong hàm encrypt thực hiện chuyển từng kí tự trong flag ở hàm init thành mã ASCII, sau đó nâng lên lũy thừa 1 mod n trong hàm init. Sau đó chuyển sang 1 xâu byte có độ dài là 5 và lưu vào một list tên là cipher. List đó sẽ được viết vào file cipher, kết thúc một kí tự sẽ xuống dòng 1 lần. Hàm decrypt đọc file cipher sau đó tách từng kí tự thành từng xâu byte và chuyển từng xâu byte đó sang dạng số nguyên và decrypt bằng key riêng. Nhưng vì e = 1 nên các kí tự trong file cipher không bị biến đổi. Ta mở file cipher bằng Notepad, thu được flag



Flag: `ISPCTF{e=1_any_thing_not_change}`
