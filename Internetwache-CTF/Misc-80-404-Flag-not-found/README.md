Deskripsi soal:
<pre>
I tried to download the flag, but somehow received only 404 errors :(
File: https://github.com/internetwache/Internetwache-CTF-2016/tree/master/tasks/misc80/task
</pre>

<h3>Solved by snoww0lf</h3>

Diberikan sebuah file dengan format .pcapng, file tersebut dapat dibuka menggunakan aplikasi wireshark. Didalam soal dijelaskan bahwa file flag tidak dapat ditemukan kemudian angkah berikutnya yang saya lakukan ialah melakukan filter pada protokol HTTPnya, setelah coba mengakses url yang terfilter ternyata tidak satupun didapat sebuah flagnya ( ex: http://496e2074686520656e642c206974277320616c6c2061626f757420666c61.2015.ctf.internetwache.org/flag.txt ).

Oke, ternyata tidak satupun URL yang didapatkan adalah sebuah flag. Beberapa percobaan yang saya lakukan ialah melakukan decoding hexadecimal ke teks terhadap url - url yang ditemukan tadi. Saya menjalankan perintah sederhana untuk melakukan filtering terhadap url didalam file flag.pcapng.


```bash
snoww0lf-2:task 4 root# strings flag.pcapng | grep Host
Host: 496e2074686520656e642c206974277320616c6c2061626f757420666c61.2015.ctf.internetwache.org
Host: 67732e0a5768657468657220796f752077696e206f72206c6f736520646f.2015.ctf.internetwache.org
Host: 65736e2774206d61747465722e0a7b4f66632c2077696e6e696e67206973.2015.ctf.internetwache.org
Host: 20636f6f6c65720a44696420796f752066696e64206f7468657220666c61.2015.ctf.internetwache.org
Host: 67733f0a4e6f626f62792066696e6473206f7468657220666c616773210a.2015.ctf.internetwache.org
Host: 53757065726d616e206973206d79206865726f2e0a5f4845524f2121215f.2015.ctf.internetwache.org
Host: 0a48656c70206d65206d7920667269656e642c2049276d206c6f73742069.2015.ctf.internetwache.org
Host: 6e206d79206f776e206d696e642e0a416c776179732c20616c776179732c.2015.ctf.internetwache.org
Host: 20666f72206576657220616c6f6e652e0a437279696e6720756e74696c20.2015.ctf.internetwache.org
Host: 49276d206479696e672e0a4b696e6773206e65766572206469652e0a536f.2015.ctf.internetwache.org
Host: 20646f20492e0a7d210a.2015.ctf.internetwache.org
```
Dengan beberapa baris kode python ternyata semua url tersebut dapat terdecode dengan baik menjadi sebuah teks.

```python
url_lists = [
"496e2074686520656e642c206974277320616c6c2061626f757420666c61",
"67732e0a5768657468657220796f752077696e206f72206c6f736520646f",
"65736e2774206d61747465722e0a7b4f66632c2077696e6e696e67206973",
"20636f6f6c65720a44696420796f752066696e64206f7468657220666c61",
"67733f0a4e6f626f62792066696e6473206f7468657220666c616773210a",
"53757065726d616e206973206d79206865726f2e0a5f4845524f2121215f",
"0a48656c70206d65206d7920667269656e642c2049276d206c6f73742069",
"6e206d79206f776e206d696e642e0a416c776179732c20616c776179732c",
"20666f72206576657220616c6f6e652e0a437279696e6720756e74696c20",
"49276d206479696e672e0a4b696e6773206e65766572206469652e0a536f",
"20646f20492e0a7d210a"
]

def search():
	out = []
	for i in range(0, len(url_lists)):
		out.append(url_lists[i].decode("hex"))

	print ''.join(out)

if __name__ == '__main__':
	search()
```

Output yang ditemukan: 

<pre>
snoww0lf-2:task 4 root# python search.py 
In the end, it's all about flags.
Whether you win or lose doesn't matter.
{Ofc, winning is cooler
Did you find other flags?
Noboby finds other flags!
Superman is my hero.
_HERO!!!_
Help me my friend, I'm lost in my own mind.
Always, always, for ever alone.
Crying until I'm dying.
Kings never die.
So do I.
}!
</pre>

Jika diperhatikan secara seksama disetiap karakter posisi pertama dalam tiap - tiap kalimat tersebut akan membentuk sebuah flag, dengan ini saya update sedikit kode python diatas menjadi seperti berikut ini.

```python
url_lists = [
"496e2074686520656e642c206974277320616c6c2061626f757420666c61",
"67732e0a5768657468657220796f752077696e206f72206c6f736520646f",
"65736e2774206d61747465722e0a7b4f66632c2077696e6e696e67206973",
"20636f6f6c65720a44696420796f752066696e64206f7468657220666c61",
"67733f0a4e6f626f62792066696e6473206f7468657220666c616773210a",
"53757065726d616e206973206d79206865726f2e0a5f4845524f2121215f",
"0a48656c70206d65206d7920667269656e642c2049276d206c6f73742069",
"6e206d79206f776e206d696e642e0a416c776179732c20616c776179732c",
"20666f72206576657220616c6f6e652e0a437279696e6720756e74696c20",
"49276d206479696e672e0a4b696e6773206e65766572206469652e0a536f",
"20646f20492e0a7d210a"
]

def search():
	out = []
	for i in range(0, len(url_lists)):
		out.append(url_lists[i].decode("hex"))

	v = ''.join(out).split("\n")
	v_out = ""

	for j in range(0, len(v)-1):
		v_out += v[j][0]
	print ''.join(v_out)

if __name__ == '__main__':
	search()
```

Flag: <b>IW{DNS_HACKS}</b>
