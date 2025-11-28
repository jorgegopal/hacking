# RPC -p 111

Your earlier nmap port scan will have shown port 111 running the service rpcbind. This is just a server that converts remote procedure call (RPC) program number into universal addresses. When an RPC service is started, it tells rpcbind the address at which it is listening and the RPC program number its prepared to serve.&#x20;

In our case, port 111 is access to a network file system. Lets use nmap to enumerate this.

1. **`nfs-showmount`**
   * Muestra los directorios que el servidor NFS está exportando
   * Equivalente a `showmount -e [IP]`
2. **`nfs-ls`**
   * Lista el contenido de los directorios NFS exportados
   * Muestra archivos, permisos, dueños
3. **`nfs-statfs`**
   * Muestra estadísticas del sistema de archivos
   * Espacio disponible, capacidad, uso del disco

```
nmap -p 111 --script nfs-ls --script nfs-statfs --script nfs-showmount 10.10.249.249
```
