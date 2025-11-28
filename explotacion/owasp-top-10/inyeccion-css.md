# Inyeccion CSS

* **SKF-LABS CSSI**: [https://github.com/blabla1337/skf-labs/tree/master/nodeJs/CSSI](https://github.com/blabla1337/skf-labs/tree/master/nodeJs/CSSI)

Las **Inyecciones CSS** (**CSSI**) son un tipo de vulnerabilidad web que permite a un atacante inyectar código CSS malicioso en una página web. Esto ocurre cuando una aplicación web confía en entradas no confiables del usuario y las utiliza directamente en su código CSS, sin realizar una validación adecuada.

El código CSS malicioso inyectado puede alterar el estilo y diseño de la página, permitiendo a los atacantes realizar acciones como la **suplantación de identidad** o el **robo de información confidencial**.

Si el código CSS inyectado es lo “suficientemente complejo”, puede hacer que el navegador web interprete el código como si fuera código JavaScript. Esto significa que el código CSS malicioso puede ser utilizado para inyectar código JavaScript en la página web, lo que se conoce como una inyección de JavaScript inducida por CSS (CSS-Induced JavaScript Injection).

!\[\[Pasted image 20251011143411.png]]

cierrta el corchete css cierra la etiqueta style y añade etiqueta script con css

!\[\[Pasted image 20251011143526.png]]

asi queda en el codigo
