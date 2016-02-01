<h3>Crypto 200 ( Solved by snoww0lf )</h3>
Deskripsi soal:

<pre>
He is influential, he is powerful. He is your next contact you can get you out of this situation. 
You must reach him soon. Who is he? The few pointers intrecpted by KGB are in the file. 
Once we know him, we can find his most valuable possession, his PRIDE.

whatsHisPride.md5s ( http://ctf.nullcon.net/crypto/whatsHisPride.md5s )
</pre>

Didalam url tersebut kita diberikan sebuah list data hash md5, langsung saja saya mencoba melakukan cracking web ini :
https://hashkiller.co.uk/md5-decrypter.aspx, didapatkan output berikut ini.

<pre>
d80517c8069d7702d8fdd89b64b4ed3b MD5 : Carrie
088aed904b5a278342bba6ff55d0b3a8 MD5 : Grease
56cdd7e9e3cef1974f4075c03a80332d MD5 : Perfect
0a6de9d8668281593bbd349ef75c1f49 MD5 : Shout
972e73b7a882d0802a4e3a16946a2f94 MD5 : Basic
1cc84619677de81ee6e44149845270a3 MD5 : Actor
b95086a92ffcac73f9c828876a8366f0 MD5 : Aircraft
b068931cc450442b63f5b3d276ea4297 MD5 : name
</pre>

Dari seluruh hasil yang berhasil di-crack, saya mencoba untuk melakukan googling dan dari hasil tersebut terkait serta mengarah ke wiki berikut ini.
https://en.wikipedia.org/wiki/John_Travolta

Dan ditemukanlah flag yang merujuk pada soal dibagian https://en.wikipedia.org/wiki/John_Travolta#Pilot.

Flag: <b>Jett Clipper Ella</b>

<hr>

<h3>Crypto 400 ( Solved by nirwana )</h3>
Deskripsi soal:
<pre>
Some one was here, some one had breached the security and had infiltrated here. 
All the evidences are touched, Logs are altered, records are modified with key as a text from book.
The Operation was as smooth as CAESAR had Conquested Gaul. 
After analysing the evidence we have some extracts of texts in a file. 
We need the title of the book back, but unfortunately we only have a portion of it...

The_extract.txt ( http://ctf.nullcon.net/crypto/The_extract.txt )
</pre>

Soal kali ini kita diminta untuk mencari judul buku yang diinginkan, terdapat sebuah file .txt yang berisi caesar cipher. Kita coba decrypt isi dari file .txt tersebut dengan menggunakan tools online http://practicalcryptography.com/ciphers/caesar-cipher/. Setelah di didecrypt, terdapat cerita dari hasil decrypt tersebut, setelah membacanya terdapat kalimat yang menarik "a botnet called qualnto" hmm menarik..

mari kita coba search di google dengan menggunakan keyword tersebut, ada sebuah situs yang menampilkan kalimat tersebut http://www.crimsonromance.com/romantic-suspense-novels/in-the-shadow-of-greed/ dan didapatkan flagnya yaitu title dari buku yang kita cari.

Flag : <b>In the Shadow of Greed</b>

<hr>

<h3>Crypto 500 ( Solved by snoww0lf )</h3>
Deskripsi soal:

<pre>
You are in this GAME.
A critical mission, and you are surrounded by the beauties, ready to shed slik Reviews their gowns on your beck. 
On onside your feelings are pulling you apart and another side you are called by the duty. 
The biggiest question is seX OR success? 
The signals of subconcious mind are not clear, cryptic. 
You also have the message of heart the which is clear and cryptic. 
You just need to use three of them and find whats the clear message of your Mind … What you must do?

crypto1.zip ( http://ctf.nullcon.net/crypto/crypto1.zip )
</pre>

Terdapat 3 buah file teks dengan nama.
<pre>
.
├── Heart_clear.txt
├── Heart_crypt.txt
├── Mind_crypt.txt
</pre>

Disoal tersebut diberikan clue bahwa enkripsi tersebut menggunakan XOR. Tugas kita adalah dapat mendekripsi pesan yang ada didalam Mind_crypt.txt, akan tetapi kita disini tidak diberikan keynya. Setelah membaca baik - baik soal tersebut, untuk mendapatkan keynya ialah melakukan XOR-ing Heart_clear.txt dengan Heart_crypt.txt. Berikut kode python yang saya gunakan :

```python
from itertools import cycle, izip

def get_key():
	message = open('Heart_clear.txt','r').read()
	key = open('Heart_crypt.txt','r').read()
	key_final = ""
	for cipher,key in izip(message, cycle(key)):
		key_final += chr(ord(cipher) ^ ord(key))
	print ''.join(key_final)

if __name__ == '__main__':
	get_key()
```

Ditemukan outputnya sebagai berikut.

<pre>
Its right there what you are looking for.
Its right there what you are looking for.
Its right there what you are looking for.
Its right there what you are looking for.
Its right there what you are looking for.
Its right there what you are looking for.
Its right there what you are looking for.
Its right there what you a
</pre>

Oke, kita anggap kita sudah mendapatkan keynya, maka selanjutnya ialah melakukan dekripsi terhadap isi dari file Mind_crypt.txt. Saya tambahkan method untuk melakukan dekripsi terhadap isi dari file Mind_crypt.txt.

```python
from itertools import cycle, izip

def get_key():
	message = open('Heart_clear.txt','r').read()
	key = open('Heart_crypt.txt','r').read()
	key_final = ""
	for cipher,key in izip(message, cycle(key)):
		key_final += chr(ord(cipher) ^ ord(key))
	return ''.join(key_final)

def get_flag():
	message = open('Mind_crypt.txt','r').read()
	key = get_key()
	flag = ""
	for cipher,key in izip(message, cycle(key)):
		flag += chr(ord(cipher) ^ ord(key))
	print "Found :", ''.join(flag)

if __name__ == '__main__':
	get_key()
	get_flag()
```
Ditemukan output url berikut.
<pre>
Found : https://play.google.com/store/apps/collection/promotion_3001629_watch_live_games?hl=en
</pre>

Setelah mengecek situs tersebut, heading dari url tersebut cukup menarik untuk dianggap sebagai sebuah flag.

Flag: <b>Never Miss a Game</b>
