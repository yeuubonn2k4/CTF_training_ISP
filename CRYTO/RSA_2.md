# ĐỀ BÀI

Giá trị n và e là : 

n = 882564595536224140639625987659416029426239230804614613279163
e = 65537
def rsa(mess):
    return pow(mess, e, n)
mess = 'ISPCTF{....}'
cipher = [rsa(ord(c)) for c in mess]
with open('cipher.txt', 'w') as f:
    f.write('\n'.join([str(x) for x in cipher]))

  - ## Solution
                       Ta thấy trong hàm rsa thực hiện mã hoá từng kí tự trong flag thành số với phép toán lấy mod n của mess^-e.
                       -> Bước 1 : Phân tích n thành 2 thừa ố nguyên tố p và q để tìm key d

  (https://www.dcode.fr/prime-factors-decomposition)  // web phân tích số nguyên tố thành 2 thừa số nguyên tố
                       

- Nhập n vào ô rồi chọn List of factors -> FACTORIZE

                       Làm tương tự bài rsa_1, ta tìm được key d: 

Áp dụng công thức giải mã: m = pow(c, d, n) với c là cipher.

Ta có chương trình Python như sau với c là từng dòng trong file cipher.txt:

    d = 121832886702415731577073962957377780195510499965398469843281
    c = 601748807654457784008912152319168422704193274988376714148917 // dòng đầu tiên trong file cipher.txt
    n = 882564595536224140639625987659416029426239230804614613279163
    m = pow(c, d, n)
    print(m)
Sau khi chạy chương trình ta thu được 1 số là 73, tương đương với I trong bảng mã ASCII. Tương tự với các dòng sau, ta thu được 1 dãy số hoàn chỉnh như sau:

73 83 80 67 84 70 123 78 111 119 95 121 111 117 95 107 110 111 119 95 102 97 99 116 111 114 95 78 125

Dùng tool chuyển đổi hệ thập phân sang bảng mã ASCII, ta thu được flag: 

Flag: ISPCTF{Now_you_know_factor_N}
