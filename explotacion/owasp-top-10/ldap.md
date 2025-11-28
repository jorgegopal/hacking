# LDAP

&#x20;[https://github.com/motikan2010/LDAP-Injection-Vuln-App](https://github.com/motikan2010/LDAP-Injection-Vuln-App)

**NOTA (Actualización 24/04/2023)**: A la hora de clonar el proyecto y hacer un ‘**docker build -t ldap-client-container .**‘, es probable que tras ejecutar la instrucción ‘**apt-get update**‘, os salga un error que os impide construir la imagen correctamente.

Para evitar este problema, tan solo es necesario cambiar en el archivo ‘**Dockerfile**‘ la primera línea de ‘**FROM php:7.0-apache**‘ a ‘**FROM php:8.0-apache**‘. De esta forma, ya no tendréis problemas y el laboratorio se podrá desplegar correctamente:

protocolo ligero de acceso a directorio

un directorio remoto es un conjunto de objetos que están organizados de forma jerárquica tales comonombre dclaves de direcciones. etc.

!\[\[Pasted image 20251008133819.png]]

esta comentando la linea con %00 y aplicando fuerza bruta para enumerar todos los atributos de admin

!\[\[Pasted image 20251008134027.png]]

en este caso al descubrir que uno de los atributos es el numero de telefono y que empiza por 6 estamos fuzzeando para descubrir todos los demás digitos del numero de telefono del usuario en cuestion
