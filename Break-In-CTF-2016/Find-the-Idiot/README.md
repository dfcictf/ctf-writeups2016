<h2>Break In CTF - Find the Idiot 100pts</h2>
<hr>
Diberikan sebuah linux filesystem, disini tugas kita adalah mencari user yang dianggap 'idiot'. Mencoba mengecek direktori /home , lalu saya coba submit semua nama user tidak ada yang valid.
Kembali ke bagian lain yaitu direktori /etc kemudian membuka file passwd, dengan output seperti berikut.

```bash
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:99:99:Nobody:/:/sbin/nologin
avahi-autoipd:x:170:170:Avahi IPv4LL Stack:/var/lib/avahi-autoipd:/sbin/nologin
systemd-timesync:x:999:997:systemd Time Synchronization:/:/sbin/nologin
systemd-network:x:998:996:systemd Network Management:/:/sbin/nologin
systemd-resolve:x:997:995:systemd Resolver:/:/sbin/nologin
systemd-bus-proxy:x:996:994:systemd Bus Proxy:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
polkitd:x:995:993:User for polkitd:/:/sbin/nologin
abrt:x:173:173::/etc/abrt:/sbin/nologin
tss:x:59:59:Account used by the trousers package to sandbox the tcsd daemon:/dev/null:/sbin/nologin
colord:x:994:992:User for colord:/var/lib/colord:/sbin/nologin
geoclue:x:993:991:User for geoclue:/var/lib/geoclue:/sbin/nologin
unbound:x:992:990:Unbound DNS resolver:/etc/unbound:/sbin/nologin
rtkit:x:172:172:RealtimeKit:/proc:/sbin/nologin
pulse:x:171:171:PulseAudio System Daemon:/var/run/pulse:/sbin/nologin
chrony:x:991:987::/var/lib/chrony:/sbin/nologin
sddm:x:990:986:Simple Desktop Display Manager:/var/lib/sddm:/sbin/nologin
avahi:x:70:70:Avahi mDNS/DNS-SD Stack:/var/run/avahi-daemon:/sbin/nologin
openvpn:x:989:985:OpenVPN:/etc/openvpn:/sbin/nologin
mysql:x:27:27:MySQL Server:/var/lib/mysql:/sbin/nologin
rpc:x:32:32:Rpcbind Daemon:/var/lib/rpcbind:/sbin/nologin
nm-openconnect:x:988:984:NetworkManager user for OpenConnect:/:/sbin/nologin
rpcuser:x:29:29:RPC Service User:/var/lib/nfs:/sbin/nologin
nfsnobody:x:65534:65534:Anonymous NFS User:/var/lib/nfs:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
tcpdump:x:72:72::/:/sbin/nologin
aayn:x:1000:1000::/home/aayn:/bin/bash
mongodb:x:184:981:MongoDB Database Server:/var/lib/mongodb:/sbin/nologin
mysql-proxy:x:987:980:MySQL-Proxy user:/:/sbin/nologin
qemu:x:107:107:qemu user:/:/sbin/nologin
nm-openvpn:x:986:979:Default user for running openvpn spawned by NetworkManager:/:/sbin/nologin
apache:x:48:48:Apache:/usr/share/httpd:/sbin/nologin
saslauth:x:985:76:Saslauthd user:/run/saslauthd:/sbin/nologin
lirc:x:984:977:LIRC daemon user, runs lircd.:/var/log/lirc:/sbin/nologin
ash:x:1001:1001::/home/ash:/bin/bash
tyson:x:1002:1002::/home/tyson:/bin/bash
gohan:x:1003:1003::/home/gohan:/bin/bash
totoro:x:1004:1004::/home/totoro:/bin/bash
luffy:x:1005:1005::/home/luffy:/bin/bash
cleopatra:x:1006:1006::/home/cleopatra:/bin/bash
```

Beberapa user yang ada di output passwd , tidak sama dengan yang ada di /home. Karena berbeda saya coba submit semua user - user yang tidak sama.
Tetap tidak mendapatkan flag apapun, maka percobaan berikutnya adalah dengan membaca file shadow yang ada di direktori /etc dan itemukan output seperti berikut ini.

```bash
root:$6$PBY4XOYn$JJOSF.0aOqVNd/yqQ4jdLLAmgXCe6RNekSv6JmOenId3z0xkR/u72M2TFhpGO8.Y0IyHiZuB50WlIxmONz/KZ.:16816:0:99999:7:::
bin:*:16489:0:99999:7:::
daemon:*:16489:0:99999:7:::
adm:*:16489:0:99999:7:::
lp:*:16489:0:99999:7:::
sync:*:16489:0:99999:7:::
shutdown:*:16489:0:99999:7:::
halt:*:16489:0:99999:7:::
mail:*:16489:0:99999:7:::
operator:*:16489:0:99999:7:::
games:*:16489:0:99999:7:::
ftp:*:16489:0:99999:7:::
nobody:*:16489:0:99999:7:::
avahi-autoipd:!!:16576::::::
systemd-timesync:!!:16576::::::
systemd-network:!!:16576::::::
systemd-resolve:!!:16576::::::
systemd-bus-proxy:!!:16576::::::
dbus:!!:16576::::::
polkitd:!!:16576::::::
abrt:!!:16576::::::
tss:!!:16576::::::
colord:!!:16576::::::
geoclue:!!:16576::::::
unbound:!!:16576::::::
rtkit:!!:16576::::::
pulse:!!:16576::::::
chrony:!!:16576::::::
sddm:!!:16576::::::
avahi:!!:16576::::::
openvpn:!!:16576::::::
mysql:!!:16576::::::
rpc:!!:16576:0:99999:7:::
nm-openconnect:!!:16576::::::
rpcuser:!!:16576::::::
nfsnobody:!!:16576::::::
sshd:!!:16576::::::
tcpdump:!!:16576::::::
mongodb:!!:16731::::::
mysql-proxy:!!:16731::::::
qemu:!!:16731::::::
nm-openvpn:!!:16731::::::
apache:!!:16733::::::
saslauth:!!:16755::::::
lirc:!!:16788::::::
ash:$6$fiNGabLk$upevBoVjL32aEQTuSvA2SuXJZedMc.9vvbugtJDquYORgMT3TXNFS.S/W9buFzXpl/agOpGQAYHPstcis8gIf.:16816:0:99999:7:::
tyson:$6$8GCtc6QE$gsuCLj7OYs0wnSFRdUcuazxyrds.hhsit90JRZuq/i13e89z06/4eysfxA0t4h4FBUh0RZxd7UEV2TSDqR.Iz1:16816:0:99999:7:::
gohan:$6$.xVcX/EJ$fB3PgzXpih0vrbP4BOikB.Es.eMJi/5V8nVq85CRur/4XwQJrrqgiQILGNRdX3CACrQa3y/fR6XkRmOs9cPah.:16816:0:99999:7:::
totoro:$6$9JY5ReB4$IXYmKZH/J99kkBDYvbnKwCXgVlaFAPP5zlwajl3nrs46Eq9VGS39JhROAWZTU0OiNevhp180a/uVnPbOH220H1:16816:0:99999:7:::
luffy:$6$zHCmYkJq$6AjTyQjUjjFqChpnimyiXLKfSDAYSbVVHckt8dMXb3zgge/Avk4hTSJdKvjrv0Jc5fnc5H5OzEdNHkkxDCcjV.:16816:0:99999:7:::
cleopatra:$6$D5UasmHS$tOGQVx8zahL/WGwF/rKdugb7wBdiwrHKUYsOjIjGRTMdDgUlV6qyX5mBu7tlfvVm3WrC.JM2jTtgcFvauMTgs.:16816:0:99999:7:::
```

Sedikit melakukan tebak - tebakan apakah flagnya nanti adalah password user yang berhasil di crack ? Disini saya menggunakan John The Ripper dalam melakukan cracking password usernya.

```bash
snoww0lf-2:find_the_idiot root# john etc/shadow
Press 'q' or Ctrl-C to abort, almost any other key for status
dragon1          (gohan)
```

Ditemukan password yang berhasil di crack, setelah mencoba untuk disubmit ternyata password tersebut dianggap valid sebagai sebuah flag.

Flag: <b>dragon1</b>
