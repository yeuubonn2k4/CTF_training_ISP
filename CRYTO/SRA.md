### Lên Google Colab:

#### Cell 1 — cài PARI/GP

```
!apt-get update -qq
!apt-get install -y pari-gp
!pip install -q pycryptodome sympy
```

#### Cell 2 — ghi solver mới

```
%%writefile solve_sra_fast.py
import socket
import re
import subprocess
import string
from sympy import isprime
from Crypto.Util.number import long_to_bytes

HOST = "saturn.picoctf.net"
PORT = 57514
E = 65537

ALNUM = set(string.ascii_letters + string.digits)

LOW = 2**127 - 1
HIGH = 2**128


def recv_until(sock, marker: bytes) -> bytes:
    data = b""
    while marker not in data:
        chunk = sock.recv(4096)
        if not chunk:
            break
        data += chunk
    return data


def pari_factor(n, timeout=25):
    script = f"""
default(parisizemax, 4000000000);
n = {n};
f = factorint(n);
for(i=1, matsize(f)[1], print(f[i,1], " ", f[i,2]));
\\\\q
"""
    try:
        r = subprocess.run(
            ["gp", "-q"],
            input=script,
            text=True,
            capture_output=True,
            timeout=timeout,
        )
    except subprocess.TimeoutExpired:
        return None

    factors = {}

    for line in r.stdout.splitlines():
        line = line.strip()
        if not line:
            continue

        parts = line.split()
        if len(parts) != 2:
            continue

        try:
            p = int(parts[0])
            e = int(parts[1])
        except ValueError:
            continue

        factors[p] = e

    if not factors:
        return None

    prod = 1
    for p, e in factors.items():
        prod *= p ** e

    if prod != n:
        return None

    return factors


def gen_divisors_filtered(factors):
    items = list(factors.items())
    out = []

    def dfs(i, cur):
        if cur >= HIGH:
            return

        if i == len(items):
            if LOW <= cur < HIGH:
                out.append(cur)
            return

        p, e = items[i]
        v = 1

        for _ in range(e + 1):
            dfs(i + 1, cur * v)
            v *= p

    dfs(0, 1)
    return out


def recover_pride(c, d):
    ed1 = E * d - 1

    candidates = []

    for k in range(1, E):
        if ed1 % k != 0:
            continue

        phi = ed1 // k

        # p,q are 128-bit primes => phi = (p-1)(q-1) has about 254..256 bits.
        if not (254 <= phi.bit_length() <= 256):
            continue

        # p,q odd => p-1 and q-1 are even => phi divisible by 4.
        if phi % 4 != 0:
            continue

        candidates.append((k, phi))

    print("[*] filtered k count:", len(candidates))

    for k, phi in candidates:
        print(f"[*] Trying k={k}, phi_bits={phi.bit_length()}")

        factors = pari_factor(phi, timeout=25)

        if factors is None:
            print("[-] PARI timeout/fail")
            continue

        print("[+] factors:", factors)

        divs = gen_divisors_filtered(factors)
        print("[*] possible p-1 divisors:", len(divs))

        for a in divs:
            if phi % a != 0:
                continue

            b = phi // a

            if not (LOW <= b < HIGH):
                continue

            p = a + 1
            q = b + 1

            if not isprime(p) or not isprime(q):
                continue

            n = p * q
            m = pow(c, d, n)
            pt = long_to_bytes(m)

            if len(pt) == 16 and all(chr(x) in ALNUM for x in pt):
                print("[+] FOUND")
                print("[+] k =", k)
                print("[+] p =", p)
                print("[+] q =", q)
                print("[+] n =", n)
                print("[+] pride =", pt)
                return pt.decode()

    return None


def solve_one():
    sock = socket.create_connection((HOST, PORT), timeout=10)

    data = recv_until(sock, b"> ")
    text = data.decode(errors="replace")
    print(text)

    c = int(re.search(r"anger = (\d+)", text).group(1))
    d = int(re.search(r"envy = (\d+)", text).group(1))

    pride = recover_pride(c, d)

    if pride is None:
        sock.close()
        return False

    print("[+] Sending:", pride)
    sock.sendall(pride.encode() + b"\n")

    out = b""
    while True:
        chunk = sock.recv(4096)
        if not chunk:
            break
        out += chunk

    print(out.decode(errors="replace"))
    sock.close()
    return True


def main():
    for attempt in range(1, 20):
        print("=" * 60)
        print("[*] Attempt", attempt)

        try:
            if solve_one():
                return
        except Exception as e:
            print("[-] attempt failed:", repr(e))

    print("[-] Chưa ra. Chạy lại cell này thêm lần nữa.")


if __name__ == "__main__":
    main()
```

#### Cell 3 — chạy

`!python solve_sra_fast.py`

<img width="1642" height="797" alt="image" src="https://github.com/user-attachments/assets/ab3bf19e-6c0d-4ae4-9edd-bbf369423c23" />



`picoCTF{7h053_51n5_4r3_n0_m0r3_b2f9b414}`
