# Nessus

descargamos Nessus-10.0.0-debian6-amd64.deb

permisos de ejecución

`sudo dpkg -i <.deb>`

```
sudo systemctl start nessusd.service
sudo systemctl status nessusd.service
```

nos da la ruta donde esta corriendo el servicio de nessus en nuestro localhost



a partir de ahi podemos hacer un scanner de un host en concreto y nos dará las vulnerabilidades&#x20;
