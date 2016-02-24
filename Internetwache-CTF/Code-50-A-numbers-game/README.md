Deskripsi soal:
<pre>
Description: People either love or hate math. Do you love it? Prove it! You just need to solve a bunch of equations without a mistake.

Service: 188.166.133.53:11027
</pre>

<h3>Solved by hamsterx</h3>

Pertama kami mencoba melakukan koneksi dengan "telnet 188.166.133.53 11027" kemudian akan mendapatkan soal matematika seperti ini Level 1: x - 10 = 2, ketika kita menjawab benar maka akan muncul soal selanjutnya dengan operator yang berbeda ( - , +, *, / ), nah untuk menyelesaikan soal itu dengan cepat kami membuat solvernya agar bisa diselesaikan secara automatis.

Codingnya seperti berikut : 

```ruby
	require 'socket'      

	hostname = '188.166.133.53'
	port = 11027

	s = TCPSocket.open(hostname, port)
	loop do
		while line = s.gets   
			puts a = line.chop
			a1 = a.split(" = ")[1]		
			a2 = a.split(" ")[4]		
			op = a.split(" ")[3]

			if op == "+"
				hit = a1.to_i - a2.to_i
				print "Answer : ",hit
				puts
				s.syswrite hit.to_s 			
			elsif op == "-"
				hit1 = a1.to_i + a2.to_i
				print "Answer : ",hit1
				puts
				s.syswrite hit1.to_s
				
			elsif op == "/"
				hit2 = a1.to_i * a2.to_i
				print "Answer : ",hit2
				puts
				s.syswrite hit2.to_s			
			elsif op == "*"
				hit3 = a1.to_i / a2.to_i
				print "Answer : ",hit3
				puts
				s.syswrite hit3.to_s			
			end	
		end
	end
	s.close   
```

Flag: <b>IW{M4TH_1S_34SY}</b>
