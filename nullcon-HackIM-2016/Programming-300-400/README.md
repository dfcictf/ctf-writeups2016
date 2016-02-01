Note:
<b>Sebelum membahas write-up mungkin menurut saya kategori Programming di challenge kali ini kurang tepat, ini lebih mengarah ke reconnaissance dan tebak - menebak.</b>

<h3>Programming 300</h3>
Deskripsi soal:

<pre>
Still Hungry and unsutisfied, you are looking for more. 
Some more, unique un heard dishes. 
Then you can find one to make it your self. 
Its his Dish. 
He has his own website which is he describes as " a social home for each of our passions". 
The link to his website is on his google+ page. whats the name of his site. 
By the way he loves and hogs on "Onion Kheer". 
Have you heard of "Onion Kheer"?
</pre>

Bagian "a social home for each of our passions" tampaknya menarik sebagai sebuah clue / kata kunci. 
https://plus.google.com/s/a%20social%20home%20for%20each%20of%20our%20passions

Ditemukan post yang membahas mengenai "Onion Kheer" di halaman https://plus.google.com/+bibhutibhusanPanigrahyneedrecipes/posts/hCdLCijwSF7 , karena yang ditanyakan adalah alamat / nama website dari orang tersebut maka itu saya mencoba men-submit nama website tersebut, dan ternyata valid dianggap sebagai sebuah flag.

Flag: <b>affimity.com</b>

<hr>

<h3>Programming 400</h3>
Deskripsi soal:

<pre>
One of the NullCon vidoes talked about a marvalous Russian Gift.
The Vidoe was uploaded on [May of 2015] What is the ID of that youtube video.
</pre>
Cukup mudah, saya hanya perlu ke youtube mencari dengan keyword "nullcon 2015", kemudian melakukan sortir berdasarkan tanggal. Lalu mencoba satu persatu alias tebak - menebak pada bagian ID Youtubenya ( pada parameter ?v= ). https://www.youtube.com/results?sp=CAI%253D&q=nullcon+2015. Pada video ini https://www.youtube.com/watch?v=a4_PvN_A1ts ( nullcon Goa 2015: The NSA Playset RF Retroreflectors by Michael Ossman ), dianggap valid sebagai flagnya.

Flag: <b>a4_PvN_A1ts</b>
