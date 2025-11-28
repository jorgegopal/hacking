# SQUID proxies

El **Squid Proxy** es un servidor web **proxy-caché** con licencia **GPL** cuyo objetivo es funcionar como **proxy** de la red y también como zona caché para almacenar páginas web, entre otros. Se trata de un servidor situado entre la máquina del usuario y otra red (a menudo Internet) que actúa como protección separando las dos redes y como zona caché para acelerar el acceso a páginas web o poder restringir el acceso a contenidos

Es decir, la función de un servidor proxy es centralizar el tráfico de una red local hacia el exterior (Internet). Sólo el equipo que incorpora el servicio proxy debe disponer de conexión a Internet y el resto de equipos salen a través de él:

Ahora bien, puede darse el caso en el que un servidor Squid Proxy se encuentre **mal configurado**, permitiendo en consecuencia a los atacantes recopilar información de dispositivos a los que normalmente no deberían tener acceso. Por ejemplo, en este tipo de situaciones, un atacante podría ser capaz de realizar peticiones a direcciones IP internas pasando sus consultas a través del Squid Proxy, pudiendo así realizar un escaneo de puertos contra determinados servidores situados en una **red interna**.

Para ello, simplemente podríamos probar a hacer uso de extensiones de navegador como **FoxyProxy** o desde consola haciendo uso del comando ‘**curl**‘:

➜  `curl --proxy http://10.10.11.131:3128 http://<ip>:<port>`

En el mejor de los casos, si la conexión no requiere de autenticación, podríamos llevar a cabo una enumeración de puertos en servidores internos concretos, siendo necesario sustituir port por el puerto deseado que se desea enumerar del servidor ip correspondiente. En caso de requerir autenticación, si el atacante dispone de las credenciales, estas podrían ser especificadas haciendo uso del parámetro ‘-u‘.

Una de las herramientas que se suelen emplear para enumerar puertos de un servidor concreto pasando por el Squid Proxy es **spose**. Spose es una herramienta de escaneo de puertos diseñada específicamente para trabajar a través de servidores Squid Proxy. Esta herramienta permite a los atacantes buscar posibles servicios y puertos abiertos en una red interna “protegida” por un servidor Squid Proxy.

[https://github.com/aancw/spose](https://github.com/aancw/spose)

[https://www.vulnhub.com/entry/sickos-11,132/](https://www.vulnhub.com/entry/sickos-11,132/)

!\[\[Pasted image 20251009132844.png]]
