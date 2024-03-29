### Пассивный сбор информации

### dig 

```bash
dig google.com
; <<>> DiG 9.18.18-0ubuntu0.22.04.1-Ubuntu <<>> google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 19507
;; flags: qr rd ra; QUERY: 1, ANSWER: 6, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494
;; QUESTION SECTION:
;google.com.			IN	A

;; ANSWER SECTION:
google.com.		93	IN	A	74.125.205.101
google.com.		93	IN	A	74.125.205.102
google.com.		93	IN	A	74.125.205.139
google.com.		93	IN	A	74.125.205.138
google.com.		93	IN	A	74.125.205.113
google.com.		93	IN	A	74.125.205.100

;; Query time: 8 msec
;; SERVER: 127.0.0.53#53(127.0.0.53) (UDP)
;; WHEN: Tue Jan 16 16:34:37 +06 2024
;; MSG SIZE  rcvd: 135

```
### nslookup
только первичный dns server
```bash
tansh@tansh:~$ nslookup google.com
Server:		127.0.0.53
Address:	127.0.0.53#53

Non-authoritative answer:
Name:	google.com
Address: 173.194.222.101
Name:	google.com
Address: 173.194.222.100
Name:	google.com
Address: 173.194.222.139
Name:	google.com
Address: 173.194.222.113
Name:	google.com
Address: 173.194.222.102
Name:	google.com
Address: 173.194.222.138
Name:	google.com
Address: 2a00:1450:4010:c02::64
Name:	google.com
Address: 2a00:1450:4010:c02::71
Name:	google.com
Address: 2a00:1450:4010:c02::8b
Name:	google.com
Address: 2a00:1450:4010:c02::8a
```

### nmap 

#### tcp scan
```
nmap -sT -p1-20 test.net
```

```
tansh@tansh:~$ nmap -sT -p1-20 test.net
Starting Nmap 7.80 ( https://nmap.org ) at 2024-01-16 16:38 +06
Nmap scan report for test.net (85.214.110.167)
Host is up (0.16s latency).
rDNS record for 85.214.110.167: h2439270.stratoserver.net

PORT   STATE  SERVICE
1/tcp  closed tcpmux
2/tcp  closed compressnet
3/tcp  closed compressnet
4/tcp  closed unknown
5/tcp  closed rje
6/tcp  closed unknown
7/tcp  closed echo
8/tcp  closed unknown
9/tcp  closed discard
10/tcp closed unknown
11/tcp closed systat
12/tcp closed unknown
13/tcp closed daytime
14/tcp closed unknown
15/tcp closed netstat
16/tcp closed unknown
17/tcp closed qotd
18/tcp closed msp
19/tcp closed chargen
20/tcp closed ftp-data

Nmap done: 1 IP address (1 host up) scanned in 1.92 seconds
```

#### udp scan
```bash
tansh@tansh:~$ sudo nmap -sU test.net
Starting Nmap 7.80 ( https://nmap.org ) at 2024-01-16 16:40 +06
Nmap scan report for test.net (85.214.110.167)
Host is up (0.14s latency).
rDNS record for 85.214.110.167: h2439270.stratoserver.net
Not shown: 994 closed ports
PORT      STATE         SERVICE
67/udp    open|filtered dhcps
389/udp   open|filtered ldap
944/udp   open|filtered unknown
5000/udp  open|filtered upnp
5060/udp  open|filtered sip
10000/udp open          ndmp

Nmap done: 1 IP address (1 host up) scanned in 1051.72 seconds
```

### host -t A
```
tansh@tansh:~$ host -t A google.com
google.com has address 74.125.205.139
google.com has address 74.125.205.100
google.com has address 74.125.205.101
google.com has address 74.125.205.113
google.com has address 74.125.205.138
google.com has address 74.125.205.102
```

## ipconfig
- очистить локальный dns cash
```
ipconfig /flushdns
```
- информация об IP для всех сетевых карт
```
ipconfig /all
```
## netstat 
показывает таблицу маршрутизации, TCP соединения

```
tansh@tansh:~$ netstat
Active Internet connections (w/o servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State      
tcp        1      1 tansh:42314             10.80.1.6:http          LAST_ACK   
tcp        0      0 tansh:57238             li-in-f157.1e100.:https TIME_WAIT  
tcp        0      0 tansh:56642             ec2-63-35-30-167.:https ESTABLISHED
tcp        1      1 tansh:56620             134.213.193.62:https    LAST_ACK   
tcp        0      0 tansh:33592             20.119.174.243:https    ESTABLISHED
tcp        0      0 tansh:43996             10.80.2.4:https         ESTABLISHED
tcp        0      0 localhost:46162         localhost:40047         ESTABLISHED
tcp        0      0 tansh:53866             162.159.136.234:https   ESTABLISHED

```
