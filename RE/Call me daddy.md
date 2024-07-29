## Challenge

Trong quá trình hiện thực hoá giấc mơ trở thành hacker thành đạt kiêm bố đường, H3nl0r gặp vấn đề với việc tốt nghiệp cấp 3 khi gặp phải kẻ thù không đội trời chung là môn tin học 11. Hãy tìm hiểu xem H3nl0r có đạt được mơ ước của mình không, hay mãi vẫn không được lên lớp và phải ở nhà ăn bám!

program whosyourdaddy;

uses crt;

var

        name: string;
        
        daddy: string ='ISP';
        
        flag: string = 'hrqbmtczqsnfs`llhofC`rhb|';

procedure printFlag(var flag: string);

var i: integer;

BEGIN

        for i := 1 to length(flag) do
        
                flag[i] := chr(ord(flag[i]) xor 1);
                
        writeln(flag);
END;


BEGIN
        writeln('Who''s your daddy?');
        
        readln(name);
        
        if name = daddy then printFlag(flag)
        
        else
        
                BEGIN
                
                        writeln('Neu ma ngoan em se bi thuong.');
                        
                        writeln('Neu ma hu em se duoc phat.');
                        
                END;
                
        readln()
        
END.

## Solve

- Duoi .pas -> Day la 1 file code cua pascal

- Duoc chia thanh cac phan :

+ Phan khai bao:

![image](https://github.com/user-attachments/assets/4ccdff8b-7470-47cf-90df-3a20781610e8)

+ Phan ham con :

![image](https://github.com/user-attachments/assets/bb0ba989-36d2-4364-a056-cc8990de1f29)

+ Phan ham main :

![image](https://github.com/user-attachments/assets/21f0aafa-6be5-4a8a-af67-e5f1b976fdda)

Ở phần khai báo biến ta thấy có 1 biến "name" kiểu string, 1 biến "daddy" kiểu string có giá trị là 'ISP' và 1 biến "flag" kiểu string có giá trị là hrqbmtczqsnfs`llhofC`rhb|

Ở phần chương trình con ta có thể thấy đó là hàm này dùng để xử lý flag đã được khai báo ở trên kia thành flag mà chúng ta có thể submit được.

Đọc trong main thì ta thấy có câu lệnh so sánh, nếu như input của người dùng nhập vào bằng với giá trị của biến daddy thì sẽ gọi hàm con printFlag.

Vậy thì dễ rồi, chỉ cần chạy rồi nhập input = 'ISP' là có thể ra được flag.

![image](https://github.com/user-attachments/assets/b1b3bd8f-e926-4242-8989-acbdbe2e239b)

-> Flag :

`
ispclub{programmingBasic}
`
