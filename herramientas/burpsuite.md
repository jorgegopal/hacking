# burpsuite

nos ponemos en escucha con intercept on y mandamos la solicitud desde el navegador

por otro lado con control R podemos mandar la solicitud al repeater

podemos darle en proxy options miscellaneous a Dont send items to proxy history or live task, if out of scope

!\[\[Pasted image 20251006110608.png]]

podemos definir el scope en target --> scope --> add --> añadilmos la url del dominio que estamos auditando

!\[\[Pasted image 20251006110542.png]]

en el caso de que captures una respuesta tambien se pued mirar la respuesta desde el proxy dandole click derecho do intercept, y si le das a forward veras la respuesta del servidor

con control i se manda al intruder

ataque de tipo snipper

aqui si seleccionamos donde queremos meter el payload y en positions le damos a add, este pondra como payload lo que hayamos seleccionado

!\[\[Pasted image 20251006111500.png]]

si posteriormente en payload le damos a load y seleccionamos una wordlist, este comenzará un ataque de fuerza bruta en el payload que hayamos seleccionado anteriormente. por defecto esta seleccionado el url encode, lo cual no es del todo optimo

podemos ver en la request el payload que estamos mandando y si lo seleccionamos y le damos a control u este se url encodeará, en caso contrario si le damos a control shift u se quitará el url encode

en el intruder si nos vamos a options y cargamos la respuesta con fetch response, en el campo grep-extract podemos poner la solicitud como invalid password

con ==cluster bomb==, se pueden seleccionar varios payloads, como usuarios y contraseñas y probara para todos los usuarios todas las contraseñas

si queremos usar el mismo payload para el usuario y para la contraseña ==batteling ram==

para hacerlo en paralelo, por ejemplo la primera linea con la primera linea la segunda linea con la segunda linea en el caso de tener dos payloads, entonces utiliizaremos el ==pitchfork==

en el comparer puedes mandar la solicitud a este campo y te compará dos solicitudes

en el extender puedes añadir payloads copy as python request (si le das click derecho y le puedes dar a copy as python request y la puedes copiar en un script de python) también se puede copiar como una curl request

en user options puedes cambiar la fuente y la apariencia de burpsuite
