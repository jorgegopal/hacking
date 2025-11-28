# Friendly

maquina muy sencilla debido a que solo tenía dos puertos abiertos el 80 y el 21



nos dejaba conectarnos con un usuairo anonimo y nos daba el index.html por defecto de la pagina apache



posteriormente creando una reverse shell con pentest monkey y subiendola mediante ftp con el comando put y otorgandole permisos de ejecución nos reconocía la reverse shell



en la escalada de privilegios nos permitía hacer vim con sudo con lo cual automáticamente nos permitia ser root.&#x20;



al leer el root.txt dentro de la carpeta /root nos decía que buscasemos otro archivo root.txt



```bash
find / -name root.txt 2>/dev/null
```

nos daba un archivo en la configuración de apache que hemos podido leer y encontrar la flag
