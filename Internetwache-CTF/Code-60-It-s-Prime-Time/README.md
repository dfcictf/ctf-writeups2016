Deskripsi soal:
<pre>
We all know that prime numbers are quite important in cryptography. Can you help me to find some?
</pre>

Disini kita hanya disuruh mencari bilangan prima berikutnya setelah nilai n yang ditentukan dalam response dari server. Dengan sedikit bantuan library pwntools , penyelesaiannya dapat dijalankan melalui kode python berikut.

```python
from pwn import *
import re

s = remote("188.166.133.53", 11059)
r = s.recv(1024)

def prima_berikutnya(n):
    return cek_prima(n, 2*n)

def cek_prima(a, b):
    for p in range(a, b):
        for i in range(2, p):
            if p % i == 0:
                break
        else:
            return p
    return None

print r

def main():
  while True:
  	pass
  	recv = s.recv(1024)
  	print recv
  	g_prime = re.findall(r'-?\d*\.{0,1}\d+', recv)
  	prime = int(g_prime[1])
  	next_primt = prima_berikutnya(prime+1)
  	print next_primt
  	s.send(str(next_primt))
  	recv = s.recv(1024)
  	print recv

#s.interactive()
if __name__ == '__main__':
  main()
```

Flag: <b>IW{Pr1m3s_4r3_!mp0rt4nt}</b>
