# docker

***

```
docker build -t my-image 
```

para crear una imagen de un contenedor

```
docker pull 
```

para crear una imagen publica

```
docker run -dit -p 80:80
```

d: dejar en segundo plano it: de forma interactiva --name: para utilizar la imagen para montar el contenedor -p 80:80: para hacer port forwarding del contenedor hacia el host -v carpeta host:carpeta contenedor : esto lo que hace es que metas el contenido de la carpeta del host en el contenedor y se cambia tambien el contenedor

```
docker ps -a -q
```

para ver los contenedores que hay activos -q: para mostrar los identificadores

```
docker rm $(docker ps -a -q)
```

este comando borraria todos los contenedores que no se esten ejecutando --force los borraria aunque esten corriendo

```
docker images -q --filter "dangling=true"
```

\--filter "dangling=true": te filtras por las que tienen tag \<none> que no sirven para nada

```
docker rmi
```

para borrar imagenes

```
docker exec -it myConteiner bash
```

para entrar al contenedor mediante una bash

```
docker rm <id_contenedor> --force
```

docker volume rm $(docker volume ls -q)

En el dockerfile:

RUN apt update && apt install -y net-tools \ iputils-ping \ curl \ git \ nano

!\[\[Pasted image 20251002210055.png]] env DEBIAN\_FRONTEND noninteractive: para que no te pida cosas por consola expose 80 ENTRYPOINT: para que te ejecute tales comandos al montar el contenedor COPY: para copiar algo del host en el contenedor

lsof -i: para ver los puertos que tenemos abiertos en el equipo
