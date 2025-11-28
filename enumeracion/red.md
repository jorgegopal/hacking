# Comandos de Red y host

### Una recopilación de comandos esenciales para gestionar y diagnosticar redes en **Linux, Windows y macOS**.

***

### 🚏 **1. Routing - Tabla de Enrutamiento**

| **Sistema Operativo**   | **Comando**   |
| ----------------------- | ------------- |
| 🐧 **Linux**            | `ip route`    |
| 🖥️ **Windows**         | `route print` |
| 🍏 **Mac OS X / Linux** | `netstat -r`  |

<figure><img src="https://989702460-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FemJOyEYVJus1nA7uz2Rc%2Fuploads%2F2t2BACMezahJvSUIr16e%2Fimage.png?alt=media&#x26;token=98f4b5c3-58ad-4826-9cc4-0af4fa8cfb80" alt=""><figcaption></figcaption></figure>

### 📌 **Descripción:** Estos comandos muestran la tabla de enrutamiento de la red, permitiendo identificar cómo se dirigen los paquetes.

***

### 📡 **2. IP - Información de Interfaces de Red**

| **Sistema Operativo**   | **Comando**                                          |
| ----------------------- | ---------------------------------------------------- |
| 🐧 **Linux**            | <p><code>ip a</code><br><code>ip -br -c a</code></p> |
| 🖥️ **Windows**         | `ipconfig /all`                                      |
| 🍏 **Mac OS X / Linux** | `ifconfig`                                           |

<figure><img src="https://989702460-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FemJOyEYVJus1nA7uz2Rc%2Fuploads%2FWJQGTcqObCmfkILyJzHw%2Fimage.png?alt=media&#x26;token=d575b156-4a30-44cb-820a-11dfa227bf5b" alt=""><figcaption><p>Posible sálida del comando <code>ip -a</code></p></figcaption></figure>

📌 **Descripción:** Muestra información detallada sobre las interfaces de red, direcciones IP asignadas y configuraciones activas.

## comandos red en linux

```bash
ifconfig 
ip a s# Muestra interfaces de red + segmento
netstat -antup # Conexiones activas y puertos abiertos.
route / ip route # Tabla de enrutamiento.
cat /etc/resolv.conf # Configuración DNS.
arp -a # Tabla ARP.
cat /etc/networks ##(interfaces & their config.)
cat /etc/hosts ##(hosts + local domains)
```

## comandos red en windows

```bash
ipconfig /all  # Configuración de red
netstat -ano  # Conexiones activas y procesos asociados
arp -a  # Tabla ARP
route print  # Tabla de enrutamiento
netstat -ano ##(Protocols & ports of the services [the 0.0.0.0 are from the host])
netsh firewall show state ##(firewall state)
netsh advfirewall firewall dump ##(dumpe config file of the firewall)
netsh advfirewall show allprofiles ##check if the firewall is active or not
```

### Meterpreter

```bash
ifconfig ##(IP address + interfaces + MAC + IPv4 + Netmask)
netstat ##(list active TCP/UDP services & their ports + other PCs in the network)
route ##(routing table, gateway is important)
arp ##(hosts connected to the network)
```

### Copiar archivos mediante SCP

De local a servidor:

```bash
scp archivo.txt usuario@dominio.com:/home/usuario #local a servidor
scp usuario@dominio.com:/home/usuario/archivo.txt Documentos #servidor a local
scp -r /home/mario/carpeta usuario@dominio.com:/home/usuario #un directorio completo

```

### Descargar archivos con windows

```bash
certutil.exe -urlcache -split -f "https://download.sysinternals.com/files/PSTools.zip" pstools.zip
```

### comandos de host en linux

```bash
uname -a #kernel, os, hostname, processor
cat /etc/issue #distribución + version
cat /etc/*release #distribution + version, codename in parenthesis
env #env variables
lscpu ##cpu info
free -h ##ram consumption
df -h ##hard drives and mounted units
df -h ext4 ##only units that are in format ext4
lsblk | grep sd ##(list hard disks & filter by “sd” annotation)
dpkg -l ##(lists packages installed in debian & their version)

##PRIVILEGED USER###
cat /etc/shadow 
    ##$1 -> MD5
    ##$2 -> Blowfish
    ##$5 -> SHA-256
    ##$6 -> SHA-512

```

### comandos de host en windows cmd

```bash
whoami (actual user)
whoami /priv (actual user privileges)
query user (logged in users)
net users (all the user accounts)
net user x (info about x user)
net localgroup (lists all the groups on the system)
net localgroup xgrupo (to see the users from x group)
### PRIVILEGED USER Recolectar información de hash/password desde el objetivo:
Mimikatz -> help or ? -> privilege::debug (if says 20 OK) -> lsadump::sam (we get the syskey;SAMkey;RID[500=admin]) -> lsadump::secrets -> sekurlsa::logonpassword (to get passwords in plain text if they're used &/or available)
Crack NTLM -> john –format=NT hash.txt –wordlist=/route/to/wordlist.txt
hashcat -m 1000 -a 0 ó 3 hashNTLM.txt /route/to/wordlist.txt
In the dump: 1º LM & 2º NTLM (LM is not used anymore, separated by ":")
```

### comandos de host meterpreter

```bash
#################### WINDOWS
getuid ##(to see the user you are, like whoami)
sysinfo ##(hostname, O.S. & Service Pack, arch, system language & domain or hostname, distribution + release version, kernel & arch)
C:\Windows\system32\eula.txt ##(info OS, nº build, service pack)
show_mount ##(show all active units)
##YOU HAVE TO BE A PRIVILEGED USER:
hashdump (Windows: pgrep lsass -> migrate PID lsass -> hashdump)
kiwi -> help or ? -> 
creds_all ##(dump all the credentials hashes) 
lsa_dump_sam ##(dump all the NTLM hashes of the users) 
lsa_dump_secrets ##(sometimes dump credentials in plain text)
password_change ##(to change the pass or the hash of a user)


##################### LINUX
whoami ##(actual user)
groups ##(to see the groups of the system)
groups xuser ##(to see the groups of xuser)
cat /etc/passwd ##(to see the system accounts, users account have a shell at the end “/bin/sh ó /bin/bash”)
last ##(last users connected to the system rightfully)
lastlog ##(users that connected the system [SSH or rightfully])
```
