# CSRF

[https://seedsecuritylabs.org/Labs\_20.04/Files/Web\_CSRF\_Elgg/Labsetup.zip](https://seedsecuritylabs.org/Labs_20.04/Files/Web_CSRF_Elgg/Labsetup.zip)

Si a la hora de hacer el ‘**docker-compose up -d**‘, os salta un error de tipo: “**networks.net-10.9.0.0 value Additional properties are not allowed (‘name’ was unexpected)**“, lo que tenéis que hacer es en el archivo ‘**docker-compose.yml**‘, borrar la línea número 41, la que pone “**name: net-10.9.0.0**“.

cross site request forgery

comandos no autorizados son transmitidos por un usuario en el cual el sitio web confia

permite hacer una peticion por get para cambiar por ehemplo el nombre de usuario, nosotros le mandamos la solicitud por GET con su id de usuario y si el servidor la interpreta este cambiara su nombre de usuario ya que lo hara con su propio identificador de usuario. Esto mismo se podria hacer si al cambiar la contraseña, la solicitud también se envia por GET y de esta forma podriamos cambiarle la contraseña a la que nosotros queramos

!\[\[Pasted image 20251007192729.png]]

se le manda a alicia en este caso con su id que es 56 para alicia
