# commands

para instalar mariadb y entrar a la base de datos con el usuario root:

```
sudo apt install mariadb-server apache2 php-mysql
```

```
$ service mysql start  
```

```
sudo mysql -uroot -p 
```

```
mysql -u db_admin -h 192.190.150.3 -p
```

cuando estamos dentro de mysql

```sql
show databases;
```

```
use <db>
```

```
show tables;
```

para mostrar las columnas de una tabla

```
describe tabla;
```

```
select user,password from user;
```

para que nos muestre los datos de user, password por ejemplo de cierta tabla

```
select user,password from user where user = 'root';
```

para acotar la busqueda a un usuario en concreto. En el caso de que el usuario no exista te dara la respuesta que esta vació esto lo podemos controlar por ejemplo en php para saber si el usuario esta activo

```
select user,password from user where password = 'contraseña';
```

tambien podriamos poner where password = 'contraseña' para ver si existe o no esa contraseña

Creamos nuesta propia base de datos y tablas con valores:

```
create database gopal;
```

```
use gopal;
```

```
create table users (id int(32), username varchar(32), password int(32));
```

```
describe users;
```

```
MariaDB [gopal]> ALTER TABLE users
    -> MODIFY password VARCHAR(255);

```

en el caso de que queramos modificar el valor de una de las columnas

```
insert into users(id, username, password) values(1, 'admin', 'admin123$!p@$$');
```

en el caso de que queramos insertar valores para una tabla

```
update users set id=2 where username='gopal';
```

en el caso de que nos equivoquemos y quereamos cambiar por ejemplo el id de un usuario porque hayamos introducido el mismo id que el admin

```
select * from users;
```

```
select username from users;
```

para ver los valores de las columnas de la tabla users

```
create user 'gopal'@'localhost' identified by 'jorge123';
```

para crear una conexion en la base de datos, importante que este usuario debe estar registrado en users

```
grant all privileges on gopal.* to 'gopal'@'localhost';
```

elevamos sus privilegios
