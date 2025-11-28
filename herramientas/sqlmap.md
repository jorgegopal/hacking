# sqlmap

**SqlMap para auditar panel login.**

Lo primero que debemos hacer es interceptar la petición en Burpsuite y le damos en la opción **Copy to file**.

![](https://henkosec.gitbook.io/henkosec/~gitbook/image?url=https%3A%2F%2F989702460-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FemJOyEYVJus1nA7uz2Rc%252Fuploads%252FryJ9BAvNzdIA8Oyc6Slg%252F5.png%3Falt%3Dmedia%26token%3De96f7919-ec61-4bfa-bebb-6121ebc8bfcf\&width=768\&dpr=4\&quality=100\&sign=e2b9f4d9\&sv=2)

Ahora que tenemos guardado el archivo de la petición, vamos a lanzar la herramienta **sqlmap** con la siguiente sintaxis:

```
sqlmap -r <nombre_archivo>
```

De este modo lo que le estaremos indicando a sqlmap los parámetros en los que tiene que actuar.

El uso mas básico que le podríamos dar a esta herramienta seria el siguiente:

```
sqlmap -u '<url>' --forms --batch --dbs 
```

* _--forms:_ Este parámetro le indica a `sqlmap` que busque formularios en la página web. Los formularios son a menudo puntos de entrada para datos proporcionados por el usuario, lo que significa que son posibles vectores de inyección SQL. `sqlmap` escaneará los formularios que encuentre y los probará para detectar vulnerabilidades.
* _--batch:_ `sqlmap` interactúa con el usuario durante su ejecución para hacer preguntas o solicitar confirmaciones. El uso de `--batch` hace que `sqlmap` tome las decisiones automáticamente utilizando sus valores predeterminados, sin pedir interacción con el usuario. Es útil cuando se ejecuta `sqlmap` en scripts o cuando quieres automatizar el proceso.
* _--dbs:_ Este parámetro le dice a `sqlmap` que, si encuentra una vulnerabilidad de inyección SQL, debe enumerar las bases de datos disponibles en el servidor. Es una opción utilizada para extraer información crítica, que en este caso son los nombres de las bases de datos.

Una vez sepamos que bases de datos existen, vamos seguir utilizando _sqlmap_ pero esta vez le vamos a proporcionar otra sintaxis.

añadiremos `-D <nombre_db>` y `--tables` para indicarle por un lado que nos muestre la base de datos de _users_ y por otro lado que nos muestre las columnas de dicha base de datos.

```
sqlmap -u '<url>' --forms --batch -D <nombre_db> --tables
```

Ahora que sabemos el nombre de dicha columna, vamos a indicarle a _sqlmap_ que nos haga otra petición pero esta vez con el parámetro `--colums`

```
sqlmap -u '<url>' --forms --batch -T <nombre> --columns
```

Cuando no tenemos mucha información otro parámetro que podemos utilizar es: `--dump` que nos arrojará toda la información posible sin necesidad de tener que ir 1 por 1. (significativamente mas lento)

```
sqlmap -u '<url>' --forms --batch --dump
```

Copiamos las cabeceras que mandamos en una petición por post y las guardamos en un archivo llamado request.req

```
sqlmap -r request.req -p searchitem -dbs  -D baseDatos --tables 
```

-dbs para que nos arroje las base de datos -r para indicar la request --batch no te pide confirmacion de otras opciones

```
sqlmap -r request.req -p searchitem -dbs  -D baseDatos -T tabla --columns
```

-T tabla de la base de datos --columns: muestra las columnas para esa tabla --proxy \<url>
