Deskripsi soal:
<pre>
My friend really can't remember passwords. So he uses some kind of obfuscation. Can you restore the plaintext?

File: 

0000000 126 062 126 163 142 103 102 153 142 062 065 154 111 121 157 113 
0000020 122 155 170 150 132 172 157 147 123 126 144 067 124 152 102 146 
0000040 115 107 065 154 130 062 116 150 142 154 071 172 144 104 102 167 
0000060 130 063 153 167 144 130 060 113 012 
0000071
</pre>

<h3>Solved by snoww0lf</h3>

Diberikan sebuah pesan yang terenkripsi, ini adalah sebuah sebuah angka dengan basis 8 ( octal ) untuk mendecode pesan tersebut saya menggunakan python.

```python
def decode():
	encoded = "126 062 126 163 142 103 102 153 142 062 065 154 111 121 157 113 122 155 170 150 132 172 157 147 123 126 144 067 124 152 102 146 115 107 065 154 130 062 116 150 142 154 071 172 144 104 102 167 130 063 153 167 144 130 060 113 012"
	out = ""
	for dec in encoded.split():
		out += chr(int(dec, 8))
	print out

if __name__ == '__main__':
	decode()
```

Ditemukan output yang saya rasa ini adalah sebuah base64 encode: V2VsbCBkb25lIQoKRmxhZzogSVd7TjBfMG5lX2Nhbl9zdDBwX3kwdX0K, lalu saya mengupdate sedikit kode diatas untuk menambah fungsi base64 decode.

```python
from base64 import b64decode

def decode():
	encoded = "126 062 126 163 142 103 102 153 142 062 065 154 111 121 157 113 122 155 170 150 132 172 157 147 123 126 144 067 124 152 102 146 115 107 065 154 130 062 116 150 142 154 071 172 144 104 102 167 130 063 153 167 144 130 060 113 012"
	out = ""
	for dec in encoded.split():
		out += chr(int(dec, 8))
	print b64decode(out)

if __name__ == '__main__':
	decode()
```

Flag: <b>IW{N0_0ne_can_st0p_y0u}</b>
