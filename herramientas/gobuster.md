# gobuster

```
-gobuster -vhost -u <url> -w <wordlist>
```

```
gobuster dir -w=/usr/share/SecLists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt -u http://10.10.10.44/
```

```
gobuster dir -u http://192.168.1.141/ --wordlist=/usr/share/SecLists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt -t 20 -f
```

-x \<extension> podemos indicar el tipo de extension que queremos buscar -t 20 indica que queremos operar con 20 hilos

busqueda de archivos con gobuster

```
gobuster dir -u http://172.18.0.2 -w /usr/share/wordlists/dirb/common.txt -x php,html,txt -t 30
```

\--proxy para mandarlos al proxy de burpsuite
