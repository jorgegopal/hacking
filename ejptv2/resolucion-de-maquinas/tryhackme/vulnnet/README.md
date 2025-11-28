# vulnNet

!\[\[Pasted image 20251025193510.png]]

```
crackmapexec smb 10.10.129.212 -u '' -p '' --shares
```

nos enumera que tenemos permiso de lectura a vulnNet business

!\[\[Pasted image 20251025193732.png]]

```
smbclient -L 10.10.129.212 -N
```

accedemos al recurso mediante smbclient

```
smbclient //10.10.129.212/shares -N
```

y nos encontramos los siguientes archivos:

```bash
cat business-req.txt                                           
We just wanted to remind you that we’re waiting for the DOCUMENT you agreed to send us so we can complete the TRANSACTION we discussed.
If you have any questions, please text or phone us.
```

```bash
cat data.txt 
Purge regularly data that is not needed anymore
```

```
cat services.txt             
THM{0a09d51e488f5fa105d8d866a497440a}
```

con

```
enum4linux <ip>
```

hemos enumerado a los usuarios none y nobody
