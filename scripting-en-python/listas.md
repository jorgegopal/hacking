# listas

para añadir elementos a una lista

```
my_lista.append
```

para ordenar los elementos de una lista

```
my_lista = sorted(my_lista)
```

para eliminar un elemento de una lista

```
del my_lista[0]
```

```python
	
my_lista = []

my_lista.append(1)
my_lista.append(2)
my_lista.append(6)
my_lista.append(4)

del my_lista[1]
my_lista = sorted(my_lista)
for i in my_lista:
  print(f"los elementos de la lista son: {i}")    # Salida: [1, 2, 4, 6]
```

en el caso de querer solo mostrar los dos primeros elementos de una lista

```
my_lista[:2]
```

tambien se puede hacer por rangos, mostrar los dos primeros elementos

```
my_lista[0:2]
```

tambien se puede hacer al reves, en el caso de querer mostrar todo los restante desde x elemento

```
my_lista[2:]
```

para recorrer una lista de derecha a izquierda: (mostraría el último elemento), (hay que tener en cuenta que el índice cero sigue mostrando el primer elemento, empieza en -1)

```
my_lista[-1]
```

mismo concepto: desde el ultimo elemento en adelante hacia la izquierda

```
my_lista[-1:]
```

para insertar elementos en una lista:

el primer valor de insert es el indice y el segundo el elemento que queramos insertar en este indice

```python

my_lista = [1,2,3,5]
my_lista.insert(2,4)
print(my_lista) #cambiaría en la lista el numero 3 por un 4


```

para eliminar el ultimo elemento de una lista

```
my_lista.pop()
```

para mostrar el indice de un elemento concreto (siempre lista la primera aparición)

```
my_lista.index(<elemento>)
```

de esta manera mostramos todos los numeros con sus indices

```python
my_lista = [45, 23, 67, 12, 89, 34]

#listar los elementos de la lista con sus indices

for indice, valor in enumerate(my_lista):

    print(f"Indice: {indice}, Valor: {valor}")
    
#Indice: 0, Valor: 45
#Indice: 1, Valor: 23
#Indice: 2, Valor: 67
#Indice: 3, Valor: 12
#Indice: 4, Valor: 89
#Indice: 5, Valor: 34
```

para mostrar los indices de todas las apariciones de un numerro podemos hacer lo siguiente

```python

my_lista = [45, 23, 67, 12, 89, 45, 84, 43, 45, 34]

indice = [indice for indice, valor in enumerate(my_lista) if valor == 45]

print(indice)  # Salida: [0, 5, 8]

```

para ver cuantas veces aparece un elemnto

```python
my_lista = [45, 23, 67, 12, 89, 45, 84, 43, 45, 34]

my_lista.count(45)
# 3
```

para eliminar elementos repetidos en una lista. Esto convertirá la lista a un tipo set

```
set(my_lista)
```

para volver a poner set como lista

```
my_lista = list(set(my_lista))
```

para mostrar el numero más alto en una lista

```
max(my_lista)
```

el mas bajo

```
min(my_lista)
```

para sumar todos los elemtnos de una lista

```
sum(my_lista)
```

para calcular la media

```
sum(my_lista)/len(my_lista)
```

para redondear a un numero en concreto (por ejemplo 2)

```
round(sum(my_lista)/len(my_lista),2)
```

combinar listas

```
primera = [1,3,5,7,9]

segunda = [2,4,6,8,10]

  

combinada = primera + segunda

print(combinada)
```
