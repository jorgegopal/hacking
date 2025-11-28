# operadores

para formatear un número dado: con : indicamos donde comienza el formateo y con , como lo queremos ==formatear==

```python
numero = 100000000000000000000000000

print("{:,}".format(numero))
#resultado: 100,000,000,000,000,000,000,000,000
```

en este caso no nos deja, por la propia sintaxis de python cambiar el formato a un . , pero podemos hacelo de la siguiente forma meddiante ==replace==

```python

numero = 100000000000000000000000000

print("{:,}".format(numero).replace(",", "."))

#resultado: 100.000.000.000.000.000.000.000.000

```

para concatenar cadenas

```python
primera = "hola"
segunda " "
tercera "mundo"

print(primera + segunda + tercera)
```

tambien se pueden aplicar operadores para cadenas

```python

primera = "Hola"
print(primera*3)
# HolaHolaHola
```

para indicar el primer caracter de una cadena

```
primera = "Hola"
print(primera[0]*3)
# HHH
```

tambien se puede jugar con rangos

```
primera = "Hola"
print(primera[0:2]*3)
# HoHoHo
```

sumar los elementos de dos listas ==zip== lo que hace zip es compactar los elementos a pares en una lista

```python


primera = [1,3,5,7,9]

segunda = [2,4,6,8,10]

  

suma = []

for x, y in zip(primera, segunda):

    suma.append(x + y)  

print(suma)
#resultado: [3, 7, 11, 15, 19]
```

se puede iterar mediante ==map== por cada uno de los elementos de este ==zip== facilitando asi la suma de los elementos de las zumas por ejemplo

```python
primera = [1,3,5,7,9]

segunda = [2,4,6,8,10,9,98,75,4,3,2,1]

pares = map(sum, zip(primera, segunda))

for elemento in pares:

    print(elemento)
    
#resultado:

#3
#7
#11
#15
#19
```
