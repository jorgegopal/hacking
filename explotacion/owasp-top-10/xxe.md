# XXE

https://github.com/jbarone/xxelab

XXE= XML external injection XML: extensible markup language

entra en juego cuando en una petición somos capaces de declarar una entidad

!\[\[Pasted image 20251006194334.png]]

aqui vemos que con \&myName se referencia a la entidad myName en este caso previamente definida como s4avitar y el servidor nos representa ese campo email como savitar

!\[\[Pasted image 20251006194517.png]]

en este otro caso referenciando a un archivo del sistema, este devuelve el contenido de /etc/passwd

Es posible que por la forma en la que esta diseñado el serrvidor, este no nos muestre la respuesta en este caso entra en juego las XXE OOB

out of band

!\[\[Pasted image 20251006194838.png]]

en el caso de que no te deje declarar la entidad en la estructura

!\[\[Pasted image 20251006195840.png]]

creamos con python un servidor que aloje este malicious.dtd y antes no lo estaba representando, pero llamando al siguiente script creado por el atacante, nos lo va a representar de manera correcta:

!\[\[Pasted image 20251006195909.png]]
