# formateo de cadenas

```python
name = "jorge"
print ("mi nombre es %s" % name)
```

en el caso de querer poner varias cadenas

```python
name = "jorge"

rol = "estudiante"

print ("mi nombre es %s y soy %s" % (name, rol))
```

hay que tener en cuenta que debido a como esta hecho python esto tambien seria correcto

```python
name = "jorge"

edad = 25

print ("mi nombre es %s y tengo %s años" % (name, edad))
```

sin embargo lo suyo es poner

```
%d para un entero 
```

tambien se puede representar con f-strings de la siguiente manera

```
edad =23
nombre = "jorge"
print(f"mi nombre es {nombre} y tengo {edad} años")
```
