<h2>Break In CTF - Bots are Awesome! 100pts</h2>
<hr>
Deskripsi soal:
<pre>
Aren't bots awesome!.
</pre>
Cukup aneh dan perlu tebak - tebakan ( lagi ) mengenai challenge ini, setelah mengatakan 'bot' maka saya coba untuk join ke channel IRC dari Break In CTF.
Diberitahu bahwa bot bernama teheeBreakIn_ , kemudian saya ketik command !teheeBreakIn_ help dan munculah output dari bot tersebut.

<pre>
>> !teeheeBreakIn_ help
TEEHEEBOT : written in Golang.
Current Functions: help, about
USAGE: '!teheeBreakIn_ <function> [flags]
</pre>

Fungsi yang tersedia hanya 'help' dan 'about' saja, mencoba menjalankan command about ditemukan output.

<pre>
This is where all the magic happens. ;)
</pre>

Tidak ada yang istimewa menurut saya, maka dari itu saya mencoba googling mengenai TEHEEBOT yang dibuat menggunakan bahasa pemrograman Golang ini.
Ditemukan repositori pembuat dari TEHEEBOT ini ( https://github.com/amoghbl1/goIrcBot/blob/master/ircbot.go ).

```bash
package main

import ("net"
        "log"
        "bufio"
        "fmt"
        "net/textproto"
        "strings"
      )
type Bot struct{
        server string
        port string
        nick string
        user string
        channel string
        pass string
        pread, pwrite chan string
        conn net.Conn
}

func NewBot() *Bot {
        return &Bot{server: "irc.freenode.net",
                    port: "6667",
                    nick: "teeheeBot",
                    channel: "#osdg-iiith",
                    pass: "",
                    conn: nil,
                    user: "blaze"}
}
func (bot *Bot) Connect() (conn net.Conn, err error){
  conn, err = net.Dial("tcp",bot.server + ":" + bot.port)
  if err != nil{
    log.Fatal("unable to connect to IRC server ", err)
  }
  bot.conn = conn
  log.Printf("Connected to IRC server %s (%s)\n", bot.server, bot.conn.RemoteAddr())
  return bot.conn, nil
}

func (bot *Bot) WriteMessage(message string){
  fmt.Fprintf(bot.conn, "PRIVMSG %s :%s\r\n", bot.channel, message)
}
func (bot *Bot) Pong(server string){
  fmt.Fprintf(bot.conn, "PONG %s\r\n", server)
  fmt.Printf("PONG %s", server)
}
func (bot *Bot) EvaluateLine(line string){
  splitUp := strings.Split(line, ":")
  
  if len(splitUp) > 2 {
    name := strings.Split(splitUp[1], "!")
    if strings.HasPrefix(splitUp[2], "!teehee ") {
      flags := strings.Split(splitUp[2], " ")
      if flags[1] == "help" {
        bot.WriteMessage(name[0]+": TEEHEEBOT : written in Golang.")
        bot.WriteMessage(name[0]+": Current Functions: help, about")
        bot.WriteMessage(name[0]+": USAGE: '!teehee <function> [flags]'") 
      } else if flags[1] == "about" {
        bot.WriteMessage(name[0]+": Open Source Developers Group @ IIIT - H")
        bot.WriteMessage(name[0]+": Mailing List : https://groups.google.com/forum/?fromgroups#!forum/iiit-osdg")
	bot.WriteMessage(name[0]+": Blog : http://iiitosdg.wordpress.com/")
        bot.WriteMessage(name[0]+": IRC : Well, you guys are already here aren't you :P")
        bot.WriteMessage(name[0]+": GitHub : https://github.com/OSDG-IIITH/")
        bot.WriteMessage(name[0]+": Want to get a project forked under the github group? Register it at http://osdg.iiit.ac.in/github/")
        bot.WriteMessage(name[0]+": Doing GSoC this summer? Check out http://osdg.iiit.ac.in/gsoc15/") 
      } else { 
        bot.WriteMessage(name[0]+": I didn't get that, try '!teehee help' ??")
      }
      // INLINE GUIDES BE THE SHIZZ
      // Adding functionality, use the template suggested below.
      // else if flags[1] == "foobar" {
      // Do some stuff
      // }
    } else if strings.Contains(splitUp[2], "teeheeBot") {
       if strings.Contains(splitUp[1], "PRIVMSG") {
         bot.WriteMessage(name[0]+": Use '!teehee help' to get help!");
        }
      }
  }
  if strings.HasPrefix(splitUp[0], "PING") {
    bot.Pong(splitUp[1])
  }
  fmt.Printf("%q\n", splitUp)
}

func main(){
  ircbot := NewBot()
  conn, _ := ircbot.Connect()
  
  fmt.Fprintf(conn, "USER %s 8 * :%s\r\n", ircbot.nick, ircbot.nick)
  fmt.Fprintf(conn, "NICK %s\r\n", ircbot.nick)
  fmt.Fprintf(conn, "JOIN %s\r\n", ircbot.channel) 
  defer conn.Close()
  
  reader := bufio.NewReader(conn)
  tp := textproto.NewReader( reader )
  for {
        line, err := tp.ReadLine()
        if err != nil {
            break // break loop on errors    
        }
        ircbot.EvaluateLine(line)
    }
}

```

Setelah menganalisa cukup lama, ternyata tidak ada yang aneh dari kode berikut dan tidak ada fungsi yang mengarahkan ke fungsi pemanggilan flag.
Menelusuri menu commits juga tidak ada yang dirubah, maka menu branches adalah jalan terakhir. Menariknya, saya menemukan source code yang persis seperti yang dijalankan oleh bot IRC.

```bash
package main

import ("net"
        "log"
        "bufio"
        "fmt"
        "net/textproto"
        "strings"
      )
type Bot struct{
        server string
        port string
        nick string
        user string
        channel string
        pass string
        pread, pwrite chan string
        conn net.Conn
}

func NewBot() *Bot {
        return &Bot{server: "irc.freenode.net",
                    port: "6667",
                    nick: "teeheeBreakIn",
                    channel: "#breakin",
                    pass: "",
                    conn: nil,
                    user: "teehee"}
}
func (bot *Bot) Connect() (conn net.Conn, err error){
  conn, err = net.Dial("tcp",bot.server + ":" + bot.port)
  if err != nil{
    log.Fatal("unable to connect to IRC server ", err)
  }
  bot.conn = conn
  log.Printf("Connected to IRC server %s (%s)\n", bot.server, bot.conn.RemoteAddr())
  return bot.conn, nil
}

func (bot *Bot) WriteMessage(message string, name string){
  fmt.Fprintf(bot.conn, "PRIVMSG %s :%s\r\n", name, message)
}
func (bot *Bot) Pong(server string){
  fmt.Fprintf(bot.conn, "PONG %s\r\n", server)
  fmt.Printf("PONG %s", server)
}
func (bot *Bot) EvaluateLine(line string){
  splitUp := strings.Split(line, ":")
  
  if len(splitUp) > 2 {
    name := strings.Split(splitUp[1], "!")
    if strings.HasPrefix(splitUp[2], "!teeheeBreakIn ") {
      flags := strings.Split(splitUp[2], " ")
      mymsg := splitUp[2]
      //if strings.Contains(splitUp[2], "knock") or string.Contains(splitUp[2], "Knock") {
      //  bot.WriteMessage("Who's there?",name[0])
      // }
      if flags[1] == "help" {
        bot.WriteMessage("TEEHEE IRC BOT : written in Golang.", name[0])
        bot.WriteMessage("Current Functions: help, about", name[0])
        bot.WriteMessage("USAGE: '!teeheeBreakIn <function> [flags]'", name[0]) 
      } else if flags[1] == "about" {
        // bot.WriteMessage(name[0]+": Open Source Developers Group @ IIIT - H")
        // bot.WriteMessage(name[0]+": Mailing List : https://groups.google.com/forum/?fromgroups#!forum/iiit-osdg")
	    // bot.WriteMessage(name[0]+": Blog : http://iiitosdg.wordpress.com/")
        // bot.WriteMessage(name[0]+": IRC : Well, you guys are already here aren't you :P")
        // bot.WriteMessage(name[0]+": GitHub : https://github.com/OSDG-IIITH/")
        // bot.WriteMessage(name[0]+": Want to get a project forked under the github group? Register it at http://osdg.iiit.ac.in/github/")
        // bot.WriteMessage(name[0]+": Doing GSoC this summer? Check out http://osdg.iiit.ac.in/gsoc15/") 
        bot.WriteMessage("This is where all the magic happens ;)", name[0])
      } else if strings.Contains(mymsg, "knock") || strings.Contains(mymsg, "Knock"){
        bot.WriteMessage("Who's there??", name[0])
      } else if flags[1] == "breakInEnter" {
      } else {
        bot.WriteMessage("I didn't get that, try '!teehee help' ??", name[0])
      }
      // INLINE GUIDES BE THE SHIZZ
      // Adding functionality, use the template suggested below.
      // else if flags[1] == "foobar" {
      // Do some stuff
      // }
    }  
  }
  if strings.HasPrefix(splitUp[0], "PING") {
    bot.Pong(splitUp[1])
  }
  fmt.Printf("%q\n", splitUp)
}

func main(){
  ircbot := NewBot()
  conn, _ := ircbot.Connect()
  
  fmt.Fprintf(conn, "USER %s 8 * :%s\r\n", ircbot.nick, ircbot.nick)
  fmt.Fprintf(conn, "NICK %s\r\n", ircbot.nick)
  fmt.Fprintf(conn, "JOIN %s\r\n", ircbot.channel) 
  defer conn.Close()
  
  reader := bufio.NewReader(conn)
  tp := textproto.NewReader( reader )
  for {
        line, err := tp.ReadLine()
        if err != nil {
            break // break loop on errors    
        }
        ircbot.EvaluateLine(line)
    }
}
```

Cukup meyakinkan, langsung melihat kondisi baru yang membandingkan antara inputan user dengan string 'breakInEnter'.

```bash
else if flags[1] == "breakInEnter"
```

Dan menjalankan kembali dengan perintah berikut di IRC.

<pre>
>> !teeheeBreakIn_ breakInEnter
sugarHowYouGetSoFlyyy
</pre>

Flag: <b>sugarHowYouGetSoFlyyy</b>
