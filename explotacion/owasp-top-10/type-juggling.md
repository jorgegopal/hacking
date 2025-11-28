# TYPE JUGGLING

se hace cuando se comparan cadenas por ejemplo a la hora de registrarse, si nosotros, por ejemplo cambiamos el tipo de dato a una cadena como podria ser

```
usuario=admin&password[]=
```

ahi cambiamos el tipo de dato de password a un array y nos da login success

imaginemos que la contraseña = 0e389e572394875902835033;

nosotros metemos un input en el que la cadena sea igual a 0e29084910850'91835 aunque sean contraseñas diferentes php interpreta 0e como una potencia de 0 que siempre va a ser 0, porque el uso de comparación de cadenas con == hacer que esto se acontezca, con lo cual cualquier contraseña que sea 0e y cualquier numero nos va a dar un login exitoso con esa configuración. Estp se sanitizaría con el uso de tres iguales ===
