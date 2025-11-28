# John the Ripper - HASH

## 🔐 **John the Ripper: Descifrado de Hashes y Claves Privadas**

John the Ripper es una de las herramientas más poderosas para la recuperación de contraseñas mediante ataques de fuerza bruta y diccionario. Es ampliamente utilizada en auditorías de seguridad y pruebas de penetración para descifrar **hashes de contraseñas**, **claves privadas SSH (id\_rsa)** y diversos formatos de cifrado.

En esta guía, exploraremos su uso en diferentes escenarios.

### 🚀 **Modos Principales de John the Ripper**

🔹 **Ataque de diccionario:** Utiliza un archivo de palabras clave para probar contraseñas comunes.\
🔹 **Ataque de fuerza bruta:** Prueba todas las combinaciones posibles dentro de un rango determinado.\
🔹 **Descifrado de claves privadas SSH:** Extrae contraseñas de archivos **id\_rsa** cifrados.\
🔹 **Soporte para múltiples formatos:** Compatible con hashes **MD5, SHA-256, NTLM, bcrypt, entre otros**.

***

### 🔍 **Ejemplos de Uso**

### **🔓 Descifrar Hashes de Contraseñas**

John the Ripper admite una amplia variedad de hashes. Para descifrar un hash almacenado en un archivo:

```bash
john --show hashes.txt # Para verificar el formato del hash antes de iniciar el ataque.
john --wordlist=/usr/share/wordlists/rockyou.txt <hash.txt>
```

### **🔄 Fuerza Bruta en Hashes**

Si no se cuenta con un diccionario adecuado, se puede utilizar un ataque de fuerza bruta:

```bash
john --incremental hashes.txt # Esto probará todas las combinaciones posibles de caracteres hasta encontrar la contraseña.
john –format=descrypt –wordlist /usr/share/wordlists/rockyou.txt <hash.txt>
john –format=descrypt <hash> –show
```

### **📌 Crackear Diferentes Tipos de Hash**

John the Ripper es compatible con múltiples formatos de hash, como:

**🔹 MD5:**

```bash
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt <hash.txt>.txt
```

🔹 **SHA-256:**

```bash
john --format=raw-sha256 --wordlist=/usr/share/wordlists/rockyou.txt <hash.txt>
```

🔹 **NTLM (Windows):**

```bash
john --format=nt --wordlist=/usr/share/wordlists/rockyou.txt <hash.txt>
```

[puerto-22-ssh](https://henkosec.gitbook.io/henkosec/ejptv2wiki/puerto-22-ssh)

#### En el apartado del puerto 22, podremos ver como descifrar una clave id\_rsa con john\_the\_ripper

### Hay diferentes formas de identificar un hash a continuación os mostraré alguna:

```bash
hashid <hash> # Tratar de identificar un hash.
hash-identifier # Y cuando inicie, proporcionarle un hash.
```

### 🔍 Como recurso adicionar podemos utilizar estas webs:

https://hashes.com/en/tools/hash\_identifier

https://md5hashing.net/
