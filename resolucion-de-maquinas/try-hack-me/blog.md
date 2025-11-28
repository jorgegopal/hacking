# blog

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>



crackmapexec sin proporcionar usuarios nos da un directorio compartido billySMB en el cual tenemos capacidad de lectura y escritura



<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

```
```

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

```
wpscan --url <url> --passwords /usr/share/whordlists/rockyou.txt
```

creamos una lista con esos dos usuarios bjoel y kwheel y vemos la contraseña para kwheel cutiepie1

con searchsploit anteriormente habiamos visto que con wordpress 5.0 si teniamos autenticación podiamos obtener ejecución remota de comandos con lo cual lo hacemos con metasploit con el modulo&#x20;



```
multi/http/wp_crop_rce
```

importante poner (aunque no este en las opciones requeridas)

set target 0 si no nos nos daba la sesión de meterpreter

