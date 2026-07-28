```
This embedded system allows you to measure the power consumption of the CPU while it is running an AES encryption algorithm. Use this information to leak the key via dynamic power analysis.

Access the running server with nc saturn.picoctf.net 57691. It will encrypt any buffer you provide it, and output a trace of the CPU's power consumption during the operation. The flag will be of the format picoCTF{<encryption key>} where <encryption key> is 32 lowercase hex characters comprising the 16-byte encryption key being used by the program.
```

```
import socket
import re
import time
import random
import numpy as np

HOST = "saturn.picoctf.net"
PORT = 57691

NTRACES = 250        # Nếu score yếu thì tăng 400, 600
TIMEOUT = 8

SBOX = np.array([
0x63,0x7c,0x77,0x7b,0xf2,0x6b,0x6f,0xc5,0x30,0x01,0x67,0x2b,0xfe,0xd7,0xab,0x76,
0xca,0x82,0xc9,0x7d,0xfa,0x59,0x47,0xf0,0xad,0xd4,0xa2,0xaf,0x9c,0xa4,0x72,0xc0,
0xb7,0xfd,0x93,0x26,0x36,0x3f,0xf7,0xcc,0x34,0xa5,0xe5,0xf1,0x71,0xd8,0x31,0x15,
0x04,0xc7,0x23,0xc3,0x18,0x96,0x05,0x9a,0x07,0x12,0x80,0xe2,0xeb,0x27,0xb2,0x75,
0x09,0x83,0x2c,0x1a,0x1b,0x6e,0x5a,0xa0,0x52,0x3b,0xd6,0xb3,0x29,0xe3,0x2f,0x84,
0x53,0xd1,0x00,0xed,0x20,0xfc,0xb1,0x5b,0x6a,0xcb,0xbe,0x39,0x4a,0x4c,0x58,0xcf,
0xd0,0xef,0xaa,0xfb,0x43,0x4d,0x33,0x85,0x45,0xf9,0x02,0x7f,0x50,0x3c,0x9f,0xa8,
0x51,0xa3,0x40,0x8f,0x92,0x9d,0x38,0xf5,0xbc,0xb6,0xda,0x21,0x10,0xff,0xf3,0xd2,
0xcd,0x0c,0x13,0xec,0x5f,0x97,0x44,0x17,0xc4,0xa7,0x7e,0x3d,0x64,0x5d,0x19,0x73,
0x60,0x81,0x4f,0xdc,0x22,0x2a,0x90,0x88,0x46,0xee,0xb8,0x14,0xde,0x5e,0x0b,0xdb,
0xe0,0x32,0x3a,0x0a,0x49,0x06,0x24,0x5c,0xc2,0xd3,0xac,0x62,0x91,0x95,0xe4,0x79,
0xe7,0xc8,0x37,0x6d,0x8d,0xd5,0x4e,0xa9,0x6c,0x56,0xf4,0xea,0x65,0x7a,0xae,0x08,
0xba,0x78,0x25,0x2e,0x1c,0xa6,0xb4,0xc6,0xe8,0xdd,0x74,0x1f,0x4b,0xbd,0x8b,0x8a,
0x70,0x3e,0xb5,0x66,0x48,0x03,0xf6,0x0e,0x61,0x35,0x57,0xb9,0x86,0xc1,0x1d,0x9e,
0xe1,0xf8,0x98,0x11,0x69,0xd9,0x8e,0x94,0x9b,0x1e,0x87,0xe9,0xce,0x55,0x28,0xdf,
0x8c,0xa1,0x89,0x0d,0xbf,0xe6,0x42,0x68,0x41,0x99,0x2d,0x0f,0xb0,0x54,0xbb,0x16
], dtype=np.uint8)

HW = np.array([bin(i).count("1") for i in range(256)], dtype=np.float64)


def recv_some(sock, wait=0.35):
    data = b""
    sock.settimeout(wait)
    while True:
        try:
            chunk = sock.recv(65536)
            if not chunk:
                break
            data += chunk
        except socket.timeout:
            break
    return data


def parse_trace(text):
    # Tìm list số trong output, ví dụ: [1.2, 3.4, ...]
    m = re.search(r"\[([^\]]+)\]", text, re.S)
    if not m:
        return None

    arr = np.fromstring(m.group(1).replace("\n", " "), sep=",", dtype=np.float64)
    if arr.size < 10:
        arr = np.fromstring(m.group(1).replace("\n", " "), sep=" ", dtype=np.float64)

    if arr.size < 10:
        return None

    return arr


def query_trace(pt):
    """
    Mỗi lần mở 1 connection mới cho chắc.
    Nếu server cho nhiều query/connection thì cách này vẫn ổn, chỉ chậm hơn chút.
    """
    pt_hex = pt.hex()

    with socket.create_connection((HOST, PORT), timeout=TIMEOUT) as sock:
        banner = recv_some(sock, wait=0.5)
        sock.sendall(pt_hex.encode() + b"\n")
        time.sleep(0.2)
        out = banner + recv_some(sock, wait=1.0)

    text = out.decode(errors="replace")
    trace = parse_trace(text)

    if trace is None:
        print("[-] Cannot parse trace. Server output was:")
        print(text[:1000])
        raise RuntimeError("parse trace failed")

    return trace


def collect_traces(n):
    pts = []
    traces = []

    target_len = None

    for i in range(n):
        pt = random.randbytes(16)

        try:
            tr = query_trace(pt)
        except Exception as e:
            print(f"[-] trace {i} failed:", e)
            continue

        if target_len is None:
            target_len = len(tr)
            print("[+] trace length =", target_len)

        # Nếu có trace lỗi độ dài khác thì bỏ.
        if len(tr) != target_len:
            print(f"[-] skip trace with different length: {len(tr)}")
            continue

        pts.append(list(pt))
        traces.append(tr)

        if len(pts) % 25 == 0:
            print(f"[*] collected {len(pts)}/{n}")

    return np.array(pts, dtype=np.uint8), np.vstack(traces)


def cpa_recover_key(P, T):
    T = T.astype(np.float64)

    # Center trace
    Tc = T - T.mean(axis=0)
    Tnorm = np.sqrt((Tc * Tc).sum(axis=0))
    Tnorm[Tnorm == 0] = 1

    key = []

    for byte_idx in range(16):
        hyp = np.zeros((256, P.shape[0]), dtype=np.float64)

        for k in range(256):
            hyp[k] = HW[SBOX[P[:, byte_idx] ^ k]]

        hc = hyp - hyp.mean(axis=1, keepdims=True)
        hnorm = np.sqrt((hc * hc).sum(axis=1))
        hnorm[hnorm == 0] = 1

        corr = (hc @ Tc) / (hnorm[:, None] * Tnorm[None, :])
        score = np.nanmax(np.abs(corr), axis=1)

        order = np.argsort(score)[::-1]
        best = int(order[0])
        second = int(order[1])

        key.append(best)

        print(
            f"[+] byte {byte_idx:02d}: {best:02x} "
            f"score={score[best]:.5f} second={second:02x}:{score[second]:.5f}"
        )

    return bytes(key)


def main():
    print("[*] Collecting traces from server...")
    P, T = collect_traces(NTRACES)

    print("[+] collected:", P.shape[0], "traces")
    np.savez("traces_live.npz", P=P, T=T)

    print("[*] Running CPA...")
    key = cpa_recover_key(P, T)
    key_hex = key.hex()

    print()
    print("[+] key  =", key_hex)
    print("[+] flag =", f"picoCTF{{{key_hex}}}")


if __name__ == "__main__":
    main()
```

<img width="1232" height="397" alt="image" src="https://github.com/user-attachments/assets/4759548c-17a6-45b7-a506-ac665165777f" />
