# Ataque de deserialización pickel

* **SKF-LABS DES-Pickle**: [https://github.com/blabla1337/skf-labs/tree/master/python/DES-Pickle](https://github.com/blabla1337/skf-labs/tree/master/python/DES-Pickle)

Un Ataque de **Deserialización Pickle** (**DES-Pickle**) es un tipo de vulnerabilidad que puede ocurrir en aplicaciones Python que usan la biblioteca Pickle para serializar y deserializar objetos.

La vulnerabilidad se produce cuando un atacante es capaz de controlar la entrada Pickle que se pasa a una función de deserialización en la aplicación. Si el código de la aplicación no valida adecuadamente la entrada Pickle, puede permitir que un atacante inyecte código malicioso en el objeto deserializado.

Una vez que el objeto ha sido deserializado, el código malicioso puede ser ejecutado en el contexto de la aplicación, lo que puede permitir al atacante tomar el control del sistema, acceder a datos sensibles, o incluso ejecutar código remoto.

!\[\[Pasted image 20251011144825.png]]

!\[\[Pasted image 20251011144909.png]]

!\[\[Pasted image 20251011144957.png]]
