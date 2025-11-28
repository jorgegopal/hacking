# Abuso de subida de archivos

[https://github.com/moeinfatehi/file\_upload\_vulnerability\_scenarios](https://github.com/moeinfatehi/file_upload_vulnerability_scenarios)

por ejemplo para php puedes renombrarlo como otras extensiones para sacar saltarnos la sanitizacion

tambien si nos permite subir un .htaccess podriamos vulnar la sanitización:

```
Addtype apllication/x-httpd-php .php16
```

este codigo dice que todos los archivos con .php16 nos lo va a interpretar como php

podemos cambiar los primeros bytes de un archivo (el cual se pueden ver con la heraramienta xxe) y si aplica la validación de esta forma podriamos cambiarlo manualmente.

!\[\[Pasted image 20251008181031.png]]
