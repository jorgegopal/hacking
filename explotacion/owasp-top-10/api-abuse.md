# API ABUSE

[https://github.com/OWASP/crAPI](https://github.com/OWASP/crAPI)

**Actualización 24/05/2023**: Si a la hora de desplegar el laboratorio con Docker, os encontráis con problemas y alguno de los contenedores que se despliegan véis que causan error, probad a desplegar como alternativa el laboratorio de desarrollo.

Primeramente instalad la última versión de ‘**docker-compose**‘ y una vez hecho, ejecutad los siguientes comandos:

* **curl -o docker-compose.yml https://raw.githubusercontent.com/OWASP/crAPI/develop/deploy/docker/docker-compose.yml**
* **VERSION=develop docker-compose pull**
* **VERSION=develop docker-compose -f docker-compose.yml –compatibility up -d**

En caso de que veáis que tras desplegar el laboratorio, siguen habiendo errores en el despliegue de ciertos contenedores, probad a hacer un ‘**docker rm $(docker ps -a -q) –force**‘ y aplicad el último comando de los 3 mencionados anteriormente para volver a desplegar los contenedores. Llegará un momento en el que todos serán desplegados sin ningún problema.

Por otro lado, si de pronto véis que el comando ‘**docker rm $(docker ps -a -q) –force**‘ os da algún problema, esperad unos segundos y volved a probar el comando hasta que veáis que todos los contenedores han sido eliminados.

***

Cuando hablamos del abuso de APIs, a lo que nos referimos es a la explotación de vulnerabilidades en las interfaces de programación de aplicaciones (**APIs**) que se utilizan para permitir la comunicación y el intercambio de datos entre diferentes aplicaciones y servicios en una red.

***

Postman es una herramienta muy popular utilizada para probar y depurar APIs. Con Postman, los desarrolladores pueden enviar solicitudes a diferentes endpoints y ver las respuestas para verificar que la API está funcionando correctamente. Sin embargo, los atacantes también pueden utilizar Postman para comprobar ciertos ffuncionamientos de la api.

lo comodo es que podemos jugar con variables para por ejemplo indicar nuestro web tokken y que se muestre en todas las solicitudes y si cambia nuestro web tokken, nosotros podemos cambiarlo y se arrastrará a todas las solicitudes

***

Algunas de las vulnerabilidades comunes que se pueden explotar a través del abuso de APIs incluyen:

* **Inyección de SQL**: los atacantes pueden enviar datos maliciosos en las solicitudes para intentar inyectar código SQL en la base de datos subyacente.
* **Falsificación de solicitudes entre sitios (CSRF)**: los atacantes pueden enviar solicitudes maliciosas a una API en nombre de un usuario autenticado para realizar acciones no autorizadas.
* **Exposición de información confidencial**: los atacantes pueden explorar los endpoints de una API para obtener información confidencial, como claves de API, contraseñas y nombres de usuario.

!\[\[Pasted image 20251008173548.png]]

jugando con postman nos hemos dado cuenta de que hay una v2 que no nos bloquea las peticiones mientras que la v3 si que nos la bloqueaba de esta forma hemos sido capaces de sacar por fuerza bruta el otp que nos permitía cambiar la contraseña de un usuario con email conocido
