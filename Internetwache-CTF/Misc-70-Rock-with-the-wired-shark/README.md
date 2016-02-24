Deskripsi soal:
<pre>
Sniffing traffic is fun. I saw a wired shark. Isn’t that strange?
File: https://github.com/internetwache/Internetwache-CTF-2016/tree/master/tasks/misc70/task
</pre>

<h3>Solved by snoww0lf</h3>

Kita diberikan sebuah file .pcapng, dimana tujuannya ialah untuk mencari file flag. Mencoba mengecek dan memfilter protokol HTTP, yang ditemukan bagi saya cukup menarik ialah "flag.zip".

```hex
GET /flag.zip HTTP/1.1
Host: 192.168.1.41:8080
Connection: keep-alive
Authorization: Basic ZmxhZzphenVsY3JlbWE=
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/46.0.2490.80 Safari/537.36
DNT: 1
Referer: http://192.168.1.41:8080/
Accept-Encoding: gzip, deflate, sdch
Accept-Language: en-US,en;q=0.8,ht;q=0.6

HTTP/1.0 200 OK
Server: servefile/0.4.4 Python/2.7.10
Date: Fri, 13 Nov 2015 18:41:09 GMT
Content-Length: 222
Connection: close
Last-Modified: Fri, 13 Nov 2015 18:41:09 GMT
Content-Type: application/octet-stream
Content-Disposition: attachment; filename="flag.zip"
Content-Transfer-Encoding: binary

PK..
.....x.mG....(...........flag.txtUT....-FV.-FVux..............;......q.........9.....H.!....>B.....+:PK......(.......PK....
.....x.mG....(.........................flag.txtUT....-FVux.............PK..........N...z.....
```

Kita dapat melihat content yang di encoding, untuk itu saya memasukkan content tersebut kedalam hex editor ( disini saya memakai HxD ).

```assembly
    00000136  50 4b 03 04 0a 00 0b 00  00 00 78 9c 6d 47 a7 cb PK...... ..x.mG..
    00000146  a5 15 28 00 00 00 1c 00  00 00 08 00 1c 00 66 6c ..(..... ......fl
    00000156  61 67 2e 74 78 74 55 54  09 00 03 84 2d 46 56 84 ag.txtUT ....-FV.
    00000166  2d 46 56 75 78 0b 00 01  04 e8 03 00 00 04 e8 03 -FVux... ........
    00000176  00 00 c3 3b a3 d8 fc 12  94 d1 71 99 c0 9b 17 fb ...;.... ..q.....
    00000186  9d 95 eb f7 39 00 bb df  e2 a7 48 84 21 d2 cf fe ....9... ..H.!...
    00000196  09 3e 42 99 fa 95 da 05  2b 3a 50 4b 07 08 a7 cb .>B..... +:PK....
    000001A6  a5 15 28 00 00 00 1c 00  00 00 50 4b 01 02 1e 03 ..(..... ..PK....
    000001B6  0a 00 0b 00 00 00 78 9c  6d 47 a7 cb a5 15 28 00 ......x. mG....(.
    000001C6  00 00 1c 00 00 00 08 00  18 00 00 00 00 00 01 00 ........ ........
    000001D6  00 00 a4 81 00 00 00 00  66 6c 61 67 2e 74 78 74 ........ flag.txt
    000001E6  55 54 05 00 03 84 2d 46  56 75 78 0b 00 01 04 e8 UT....-F Vux.....
    000001F6  03 00 00 04 e8 03 00 00  50 4b 05 06 00 00 00 00 ........ PK......
    00000206  01 00 01 00 4e 00 00 00  7a 00 00 00 00 00       ....N... z.....
```

Simpan dengan nama "flag.zip", lalu selain itu arsip ini juga membutuhkan sebuah autentikasi password untuk melihat isi file "flag.txt". Setelah kembali melihat TCP Header ditemukan Basic Authorization yang menyimpan sebuah encoding base64 ( mungkin itu adalah passwordya ).

```python
from base64 import b64decode
encodedb64 = "ZmxhZzphenVsY3JlbWE="
print b64decode(encodedb64)
```

Ditemukan flag:azulcrema, mencoba membuka file "flag.txt" dengan password yang berhasil didecode tadi ditemukanlah flagnya.

Flag: <b>IW{HTTP_BASIC_AUTH_IS_EASY}</b>
