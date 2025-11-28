# Prototype pollution

[https://github.com/blabla1337/skf-labs](https://github.com/blabla1337/skf-labs)

El ataque **Prototype Pollution** es una técnica de ataque que aprovecha las vulnerabilidades en la implementación de objetos en JavaScript. Esta técnica de ataque se utiliza para modificar la propiedad “**prototype**” de un objeto en una aplicación web, lo que puede permitir al atacante ejecutar código malicioso o manipular los datos de la aplicación.

En JavaScript, la propiedad “prototype” se utiliza para definir las propiedades y métodos de un objeto. Los atacantes pueden explotar esta característica de JavaScript para modificar las propiedades y métodos de un objeto y tomar el control de la aplicación.

!\[\[Pasted image 20251008182458.png]]

se esta añadiendo "\_\_proto\_\_" con {"isAdmin": true}

en las variables en las que se define proto, si luego declaras otras variables sin valor como por ejemplo var jorge ={}, este herederá las que tenga proto

!\[\[Pasted image 20251008183103.png]]
