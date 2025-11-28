# domain

haciendo uso de rpcclient o enum4linux podemos listar dos usuarios bob y james

```
enum4linux <ip>
```

posteriormente con crackmapexec haciendo fuerza bruta hemos descubierto la contraseña para bob que es star

```
crackmapexec smb <ip> -u ' ' -p /usr/share/wordlist/rockyou.txt
```

nos dejaba subir archivos a la web, que interpretaba php, por lo que hemos subido una reverse shell y nos hemos conectado como www-data en el sistema

posteriormente hemos visto que el usuario bob reutilizaba su contraseña en el sistema por lo que hemos podido cambiar de usuario

luego hemos visto que nano tenia permisos suid con lo cual hemos podido cambiar el /etc/passwd y quitarle la contraseña al usuario root

y hemos podido cambiar a este usuario
