<h2>Break In CTF - Oops 50pts</h2>
<hr>
Di CTF kali ini , diberikan sebuah soal dengan kategori web. Soal ini cukup mudah, diberikan sebuah soal akan tetapi soal tersebut blank dan tidak memberi keterangan apa - apa.
Maka saya mencoba untuk melakukan analisa terhadap sourcenya, didapatkan sebuah file yang berbeda dari tiap - tiap tantangan yaitu file jquery yang diminify.
Melihat kondisi tersebut, pertama saya mengira tidak ada yang aneh didalam file jquery tersebut, namun setelah coba menganalisa dengan baik ditemukan sebuah baris kode 
yang cukup menarik.

```javascript
var content=ajax.fetchPage("http://example.com").toDOM();content.querySelector("h1").parentNode.childNodes[3].innerHTML.split(" ").slice(26).join(" ");
```

Syntax javascript tersebut akan melakukan 'scrapping' terhadap beberapa data yang ada di website http://example.com, maka dari itu saya langsung menuju web yang dimaksud,
kemudian tanpa berpikir panjang saya melakukan eksekusi kode tersebut, berhubung object dari ajax tidak ada maka saya rubah sedikit kodenya dengan menggunakan document object.

```javascript
document.querySelector("h1").parentNode.childNodes[3].innerHTML.split(" ").slice(26).join(" ");
```

Jalankan di web browser console dan didapatkanlah hasil 'scrapping' yang merupakan sebuah flag.

Flag: <b>asking for permission.</b>
