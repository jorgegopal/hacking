# Reconocimiento

***

### Puertos comunes

***

<figure><img src="../.gitbook/assets/Pasted image 20250216205904.png" alt=""><figcaption></figcaption></figure>

podemos listar ante que sistema operativo nos estamos enfrentando haciendo un `ping -c1 <ip>` y en base al ttl podemos saber si estamos ante un sistema linux o windows (windows --> 128 y linux --> 64)

### Enumeración de red

***

### 🔍 Descubrir equipos activos en nuestra red:

```bash
ip a # Muestra las direcciones IP asignadas a las interfaces de red del sistema.
ifconfig # Saber nuestra interfaz de red y nuestra ip
netdiscover -r <ip>/24 # Indicamos nuestro segmento de red
fping -a -g <ip/24>
sudo masscan -p<puertos> -Pn <rango_de_red> # Podemos analizar nuestra interfaz de red indicandole que nos busque por puertos específicos.
arp-scan -I <interfaz_de_red> --localnet --ignoredups # Buscar hosts en nuestra red local.
nmap -sn <ip>/24 #manda paquetes ICMP a ese segmento de red
sudo nmap -sS --min-rate 5000 -p- --open <ip> -oN tcp_scan.txt # Podemos añadir multiples ips separadas por ","
grep '^[0-9]' tcp_scan.txt | cut -d '/' -f1 | sort -u | xargs | tr ' ' ','
nmap --open -p<puertos> -sC -sV <ips> -Pn -On full_scan.txt`
```

#### NETSTAT

***

&#x20; `netstat -rn`nos mostrará las redes accesibles a través de la VPN.

### PRIMER ESCANEO DE RECONOMIENTO

***

```
sudo nmap -sS --min-rate 5000 -p- -Pn --open 10.10.2.43,45,46,53,55 -oN tcp.txt
```

\--top-ports 500: Nos tiraría el reconocimiento a los 500 primeros puertos -sS: envía paquetes SYN pero no completa el handshake, por lo que es más complicado de detectar por firewalls -v: verbose nos lo va reportando a medida que los encuentra -T\[1-5]: 5 seria la maxima velocidad -sT hace un three-way handshake para ver los puertos que estan abiertos -sU: para realizar el scanneo mediante UDP en vez de TCP -sn mediante ping da los puertos que estan activos -f fragmented: para eludir firewalls ya que manda paquetes fragmentados -D \<ip> siendo ip una dirección ip que quieres spoofear tambien como evasión de ips --source-port puerto: para decir por que puerto quuieres que se mande el paquete al destino tambien como tecnica de evasión de firewall --data-length tamaño: para spoofear el tamaño del archivo que se envía en el three-way --spoof-mac Dell por ejemplo: para spoofear el oui de la mac

Posteriormente deberiamos obtener los servicios y las versiones que corren en esos puertos pero antes veamos una forma de hacerlos para todas las ips disponibles mediante expresiones regulares y grep

<figure><img src="../.gitbook/assets/Pasted image 20250228233638.png" alt=""><figcaption></figcaption></figure>

como podemos ver en la captura, todas las lineas que nos interesan EMPIEZAN con un numero y ninguna linea mas tiene esta particularidad, con lo cual, mediante el uso de regex:

```
''grep '^[0-9]' tcp.txt
```

en el caso de que queramos ver las que TERMINAN por un numero

```
grep '[0-9]$' tcp.txt
```

Queremos quedarnos unicamente con los puertos unicos (sin que se repitan) de todas las direcciones ip disponibles. para poder lanzar el escaneo de servicios que corren en esos puertos podemos hacer eso mediante la siguiente expresion regular:

```
grep '^[0-9]' tcp.txt | cut -d '/' -f1 | sort -u | xargs  | tr ' ' ','
```

esta expresion nos data todos los puertos disponibles (sin que se repitan) ordenados y seprados por comas (xargs pone la columna como una linea)

### SEGUNDO ESCANEO PARA VER PUERTOS Y VERSIONES

***

```
nmap -p <puertos> -sC -sV -Pn  -oN fullScan.txt <ip>
```

-sC: scripts basicos de reconocimiento -sV enumeracion de versiones --open: solo los puertos que esten abiertos -Pn: para que no aplique resolucion dns --script "vuln and safe" para que te lance solo script que corrresponda a esas categorias tmb se puede poner or para que sea uno de los dos --script http-enum para descubrimiento de directorios en web

#### 📜 **Scripts Específicos de Nmap**

Una vez que tengamos el informe de los servicios y las versiones de cada puerto, es importante tener en cuenta que **Nmap** incluye una serie de **scripts específicos** para cada puerto detectado. Estos scripts nos permiten realizar análisis más profundos y detallados, tales como:

| Puerto       | Servicio                                | Scripts NSE                                                                                                 |
| ------------ | --------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| **21**       | FTP (File Transfer Protocol)            | ftp-anon, ftp-bounce, ftp-brute, ftp-syst, ftp-proftpd-backdoor                                             |
| **22**       | SSH (Secure Shell)                      | ssh-brute, ssh-auth-methods, ssh-hostkey, ssh-run, sshv1                                                    |
| **23**       | Telnet                                  | telnet-brute, telnet-encryption, telnet-ntlm-info, telnet-robot                                             |
| **25**       | SMTP (Simple Mail Transfer Protocol)    | smtp-brute, smtp-open-relay, smtp-enum-users, smtp-ntlm-info, smtp-strangeport                              |
| **53**       | DNS (Domain Name System)                | dns-nsid, dns-recursion, dns-zone-transfer, dns-service-discovery, dns-random-srcport                       |
| **80**       | HTTP (Servidor Web No Cifrado)          | http-title, http-enum, http-methods, http-robots.txt, http-shellshock, http-sql-injection, http-php-version |
| **139, 445** | SMB (Server Message Block)              | smb-enum-shares, smb-enum-users, smb-os-discovery, smb-brute, smb-vuln-ms17-010, smb-vuln-ms08-067          |
| **143**      | IMAP (Internet Message Access Protocol) | imap-brute, imap-capabilities, imap-ntlm-info                                                               |
| **443**      | HTTPS (Servidor Web Seguro)             | ssl-enum-ciphers, ssl-cert, ssl-ccs-injection, ssl-heartbleed, ssl-poodle                                   |
| **3306**     | MySQL                                   | mysql-brute, mysql-databases, mysql-empty-password, mysql-users, mysql-vuln-cve2016-6662                    |
| **3389**     | RDP (Remote Desktop Protocol)           | rdp-brute, rdp-enum-encryption, rdp-vuln-ms12-020                                                           |
| **1521**     | Oracle Database                         | oracle-brute, oracle-enum-users, oracle-tns-version, oracle-vuln-cve2012-3137                               |
| **5900**     | VNC (Virtual Network Computing)         | vnc-brute, vnc-info, vnc-title, realvnc-auth-bypass                                                         |

### whois

para saber quien registro el dominio, cuando lo registro en que registro etc

```
whois <dominio>
```

### netcraft

netcraft recopila todo el footprint de un navegador, todo lo que hemos visto anteriormente esta accesible con netcraft con una interfaz web

### DNS records

```
dnsrecon -d <domino>
```

```
https://dnsdumpster.com
```

### Deteccion de firewall con wafw00f

```
wafw00f <dominio>
```

### transferencia de zona

```
dig axfr @<servername> <dominio>
```

Dichos scripts los podríamos ejecutar mediante la siguiente sintaxis:

```ruby
nmap --script=ftp-anon -p 21 <ip_objetivo> # Utilizar script específico.
nmap --script "ftp-*" -p 21 <ip_objetivo> # Utilizar todos los scripts relacionados con el serivicio ftp.
```

### Osint

***

**enumeracion de emails**

```
theHarverster
```

* [ ] hunter.io
* [ ] phonebook.cz
* [ ] emailchecker --> para verificar si esta activo
* [ ] emailidchecker

### Enumeración de subdominios

***

* [ ] wfuzz
* [ ] ffuf
* [ ] gobuster
* [ ] dirbuster
* [ ] sublist3r
* [ ] dirb
* [ ] dnsdumpster

### Descargar codigo fuente de una aplicación

podemos descargar el codigo fuente de una aplicacion mediante ==httrack website coppier== el cual funcionará como un proxy por el puerto 8080 por defecto

```
sudo apt-get install webhttrack
```

### tecnologias web

* [ ] whatweb
* [ ] wappalayzer
* [ ] builtwith.com

### google dorking

* [ ] pentest-tools.com

### clonar una parte de un repositorio de git-hub

***

en el caso de solo querer clonar una carpeta de un repositorio de github se puede hacer mediatne:

```
 npx degit vulhub/vulhub/imagemagick/CVE-2016-3714
```

de esta manera se descargará solo la parte del repositorio que queremos
