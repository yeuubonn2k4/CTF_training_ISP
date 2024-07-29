## Challenge

Ki3nM1ddL3 mới tập lập trình Python và hắn nhận ra Python cũng có vài điểm chung với Pascal. Xem xem ai học Python nhanh hơn nhé.

from flag import flag 
out = ""
for i in flag:
	out += str(ord(i)+160) + " "
print out
# 265 275 272 259 268 277 258 283 211 257 275 281 255 274 209 263 264 276 223 285

## Solution 

- Nhan thay day so kia tru di 160 la co the dua vao bang ma ASCII de decode duoc

- Day so sau khi giam la :

105 115 112 99 108 117 98 123 51 97 115 121 95 114 49 103 104 116 63 125

![image](https://github.com/user-attachments/assets/4c59f2f4-297a-4876-9ae0-bdce2351337c)

-> Ta duoc Flag la :

`
ispclub{3asy_r1ght?}
`
