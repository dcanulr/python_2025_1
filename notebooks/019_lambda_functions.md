```python
import pandas as pd
```

## 1. Funciones lambda
Una función lambda es como una función definida por el usuario (las que hemos creado con **def**) que pueden ser definidas por sin nombre, también son conocidas como funciones anónimas.

### Sintaxis:
![](../lambda.png)


```python
def potencia(num):
    return num**2
```


```python
potencia(3)
```




    9




```python
lambda num: num**2
```




    <function __main__.<lambda>(num)>




```python
(lambda num: num**2)(3)
```




    9




```python
potencia = lambda num: num**2
```


```python
potencia(3)
```




    9



### Cuándo usar una función lambda?
Para crear expresiones simples y específicas, funciones que se utilizan una sola vez y no es necesario definir una función completa. Pueden tomar ventaja en funciones que pueden ser descritas en una sola línea y no tiene operaciones complejas como el uso de ciclos


```python
lambda x: x**3
```




    <function __main__.<lambda>(x)>




```python
_(3.4)
```




    39.303999999999995




```python
_(6.5)
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    Cell In[24], line 1
    ----> 1 _(6.5)
    

    TypeError: 'float' object is not callable



```python
lambda x, y: x**y
```




    <function __main__.<lambda>(x, y)>




```python
_(4)
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    Cell In[26], line 1
    ----> 1 _(4)
    

    TypeError: <lambda>() missing 1 required positional argument: 'y'



```python
_(5,3)
```




    125




```python
lambda x, y, z: x**y + z**y
```




    <function __main__.<lambda>(x, y, z)>




```python
_(y=3, x=2, z=5)
```




    133



## 2. Uso de funciones lambda con otras funciones anónimas
Existen funciones nativas (built-in functions) de python que requieren el uso de una función como argumento
1. map()
2. filter()
3. sorted()
4. apply()


https://docs.python.org/3/library/functions.html

### 2.1 map()
Aplica una función a cada elemento de un iterable y devuelve un objeto _map_. Se utiliza cuando se requiere realizar una operación en cada elemento del iterable.

**map(function, iterable)**



```python
numeros = [1,2,3,4]
```


```python
def cuadrado(num):
    return num**2
```


```python
cuadrado(2)
```




    4




```python
cuadrado(numeros)
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    Cell In[34], line 1
    ----> 1 cuadrado(numeros)
    

    Cell In[32], line 2, in cuadrado(num)
          1 def cuadrado(num):
    ----> 2     return num**2
    

    TypeError: unsupported operand type(s) for ** or pow(): 'list' and 'int'



```python
map(cuadrado, numeros)
```




    <map at 0x1ac123a0730>




```python
list(map(cuadrado, numeros))
```




    [1, 4, 9, 16]




```python
list(map(lambda num: num**2, numeros))
```




    [1, 4, 9, 16]



## 2.2 filter()
Filtra los elementos de un iterable utilizando una función que devuelve True o False. Retorna un objeto _map_ con los elementos que cumplen la condición.

**filter(function, iterable)**


```python
numeros = [1,2,3,4,5,6,7,8,9,10]
```


```python
4/2
```




    2.0




```python
4%2 == 0
```




    True




```python
5%2 == 0
```




    False




```python
filter(lambda num: num%2 == 0, numeros)
```




    <filter at 0x1ac12351a20>




```python
list(filter(lambda num: num%2 == 0, numeros))
```




    [2, 4, 6, 8, 10]



## 2.3 sorted()
Es una función que ordena los elementos de un iterable y devuelve una lista ordenada. El criterio de ordenamiento puede ser utilizando el parámetro _key_.

**sorted(iterable, key=None, reverse=False)**


```python
# Ordena la siguiente lista por orden de longitud de los nombres
nombres = ["Sophie", "Xochitl", "Eric", "Diego", "Eva"]

```


```python
sorted(nombres)
```




    ['Diego', 'Eric', 'Eva', 'Sophie', 'Xochitl']




```python
sorted(nombres, key=len(nombres))
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    Cell In[53], line 1
    ----> 1 sorted(nombres, key=len(nombres))
    

    TypeError: 'int' object is not callable



```python
sorted(nombres, key=lambda name: len(name))
```




    ['Eva', 'Eric', 'Diego', 'Sophie', 'Xochitl']




```python
sorted(nombres, key=lambda name: len(name), reverse=True)
```




    ['Xochitl', 'Sophie', 'Diego', 'Eric', 'Eva']



## 2.4 apply()
Es utilizado para aplicar funciones de un eje de un DataFrame.


```python
df = pd.DataFrame({'Producto': ["Helado", "Pan", "Shampoo", "Desodorante"],
    'Precio': [24, 8, 67, 50]})
```


```python
df["Producto"].upper()
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    ~\AppData\Local\Temp\ipykernel_22728\2716095996.py in ?()
    ----> 1 df["Producto"].upper()
    

    ~\Documents\WPy64-31160\python-3.11.6.amd64\Lib\site-packages\pandas\core\generic.py in ?(self, name)
       6200             and name not in self._accessors
       6201             and self._info_axis._can_hold_identifiers_and_holds_name(name)
       6202         ):
       6203             return self[name]
    -> 6204         return object.__getattribute__(self, name)
    

    AttributeError: 'Series' object has no attribute 'upper'



```python
df["Producto"].apply(lambda nombre: nombre.upper())
```




    0         HELADO
    1            PAN
    2        SHAMPOO
    3    DESODORANTE
    Name: Producto, dtype: object




```python
def barato(precio):
    if precio <= 30:
        print("Barato")
    else:
        print("Caro")
```


```python
df.Precio.apply(barato)
```




    0    Barato
    1    Barato
    2      Caro
    3      Caro
    Name: Precio, dtype: object




```python
df.Precio.apply(lambda precio: "Barato" if precio <= 30 else "Caro")
```




    0    Barato
    1    Barato
    2      Caro
    3      Caro
    Name: Precio, dtype: object




```python
df["Producto"]+"_"+str(df["Precio"])
```




    0    Helado_0    24\n1     8\n2    67\n3    50\nNam...
    1    Pan_0    24\n1     8\n2    67\n3    50\nName: ...
    2    Shampoo_0    24\n1     8\n2    67\n3    50\nNa...
    3    Desodorante_0    24\n1     8\n2    67\n3    50...
    Name: Producto, dtype: object




```python
df.apply(lambda column: (column["Producto"]+"_"+str(column["Precio"])), axis=1)
```




    0         Helado_24
    1             Pan_8
    2        Shampoo_67
    3    Desodorante_50
    dtype: object




```python

```

## Ejercicios


1. Dadas dos listas, donde la primer lista son bases y la segunda lista son exponentes, calcula la potencia de cada base elevada al exponente correspondiente

Input:

base = [7,20,5]

exp = [2,4,5]

Output:

[49, 160000, 3125]


```python
base = [7,20,5]
exp = [2,4,5]

list(map(lambda x, y: x**y, base, exp))
```




    [49, 160000, 3125]



2. Ordena la siguiente lista de acuerdo al orden alfabético del tercer elemento de cada tupla

Input:

tuplas = [(2,"kg","verdura"),(3,"pieza","pan"),(1,"litro","leche")]

Output:

[(1, 'litro', 'leche'), (3, 'pieza', 'pan'), (2, 'kg', 'verdura')]


```python
tuplas = [(2,"kg","verdura"),(3,"pieza","pan"),(1,"litro","leche")]

sorted(tuplas, key=lambda tupla: tupla[2])
```




    [(1, 'litro', 'leche'), (3, 'pieza', 'pan'), (2, 'kg', 'verdura')]




```python
# Crear un DataFrame de ejemplo
df = pd.DataFrame({
    'Nombre Completo': ['ana perez', 'LUIS GARCÍA', 'carlos saenz', 'María López']
})

# Aplicar una función lambda para normalizar el formato de nombres
df['Nombre Normalizado'] = df['Nombre Completo'].apply(lambda x: ' '.join(word.capitalize() for word in x.split()))
print(df)
```

      Nombre Completo Nombre Normalizado
    0       ana perez          Ana Perez
    1     LUIS GARCÍA        Luis García
    2    carlos saenz       Carlos Saenz
    3     María López        María López
    


```python

```
