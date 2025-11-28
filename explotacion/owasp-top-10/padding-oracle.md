# PADDING ORACLE

[https://www.vulnhub.com/?q=padding+oracle](https://www.vulnhub.com/?q=padding+oracle)

Un ataque de oráculo de relleno (**Padding Oracle Attack**) es un tipo de ataque contra datos cifrados que permite al atacante **descifrar** el contenido de los datos **sin conocer la clave**. Un atacante puede usar un oráculo de relleno, en combinación con la manera de estructurar los datos de CBC, para enviar mensajes ligeramente modificados al código que expone el oráculo y seguir enviando datos hasta que el oráculo indique que son correctos. Desde esta respuesta, el atacante puede descifrar el mensaje byte a byte

!\[\[Pasted image 20251008121426.png]]

!\[\[Pasted image 20251008121356.png]]

padbuster para descifrar la cookie correspondiente que es el texto cifrado

```
padbuster <url> <encripted message> <blocksize> <numero> -cookies <auth=cookie>
```

esto en el ejemplo nos da user=gopal -plaintext 'user=admin' añadimos esto a padbuster y nos proporcionará el cifrado correspondiente

se puede hacer con burpsuite con bit flipper con el payload en la cookie correspondiente y despúes filtras por por length por ejemplo
