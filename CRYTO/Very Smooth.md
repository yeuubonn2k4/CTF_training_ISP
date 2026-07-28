### Bài này là Pollard p−1 attack.

Trong gen.py, prime p và q không random “an toàn”, mà được tạo sao cho:

```
p - 1
q - 1
```

toàn là tích của các prime nhỏ cỡ 16-bit / 17-bit. Hàm get_smooth_prime() nhân nhiều prime nhỏ rồi check tmpp + 1 là prime.

Vì vậy factor n bằng Pollard p−1 rất nhanh. output.txt chỉ cho n và c.

Flag mình recover được:

`picoCTF{p0ll4rd_f4ct0r1z4at10n_FTW_148cbc0f}`

Script solve:

```
import re
import math
from sympy import primerange
from Crypto.Util.number import long_to_bytes

e = 65537

with open("output.txt", "r") as f:
    data = f.read()

n = int(re.search(r"n = ([0-9a-f]+)", data).group(1), 16)
c = int(re.search(r"c = ([0-9a-f]+)", data).group(1), 16)


def pollard_pm1(n, B):
    a = 2

    for prime in primerange(2, B + 1):
        # dùng prime power lớn nhất <= B
        pp = prime
        while pp * prime <= B:
            pp *= prime

        a = pow(a, pp, n)
        g = math.gcd(a - 1, n)

        if 1 < g < n:
            return g

    return None


p = pollard_pm1(n, 2**16)
assert p is not None

q = n // p

phi_lcm = math.lcm(p - 1, q - 1)
d = pow(e, -1, phi_lcm)

m = pow(c, d, n)
flag = long_to_bytes(m)

print(flag.decode())
```

Cài rồi chạy:

```
pip install sympy pycryptodome
python solve.py
```

Kinh nghiệm nhận diện:

```
safe prime tốt: p = 2r + 1, r cũng prime lớn
bài này nguy hiểm: p - 1 smooth, toàn factor nhỏ
→ dùng Pollard p−1
```
