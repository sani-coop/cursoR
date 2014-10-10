*********************************
Elementos de Programación con R I
*********************************

Tipos de datos y objetos básicos
================================

Objetos
-------

R cuenta con 5 clases de objetos "atómicos":

* character: Cadenas de carácter.
* numeric: Valores numéricos de punto flotante.
* integer: Valores numéricos enteros.
* complex: Valores numéricos complejos. Es decir con parte real e imaginaria.
* logical: Valores de verdadero o falso.

El tipo mas básico de objeto es un *vector*, el cual solamente puede contener
objetos de la misma clase.

Se pueden crear objetos vacíos con la función `vector()`.

Valores numéricos
-----------------

En R los números se tratan por defecto como objetos numéricos (es decir, valores
de punto flotante de doble precisión). Si se desea establecer un valor numérico
como entero se le debe agregar el sufijo `L`.

Por ejemplo: `1` denota a un objeto numérico; mediante `1L` el usuario establece
explícitamente un entero.

El valor numérico especial `Inf` denota simbólicamente al infinito. Por ejemplo:

.. code-block:: rconsole

   > 1 / 0
   [1] Inf

El valor `NaN` (Not a number) representa un valor indefinido.

.. code-block:: rconsole

   > 0 / 0
   [1] NaN

Atributos
---------

Los objetos de R pueden tener atributos descriptivos, tales como:

* names, dimnames: nombres de las columnas y de los ejes.
* dimensions: número de dimensiones de un arreglo o matriz.
* class: clase de un objeto.
* length: número de elementos que contiene un objeto.
* otros metadatos/atributos definidos por el usuario o la usuaria.

Se puede acceder a los atributos de un objeto mediante la función
`attributes()`.

Asignación
----------

En el terminal/consola de R se escriben expresiones. El símbolo `<-` es el
operador de asignación.

.. code-block:: rconsole

   > x <- 1
   > print(x)
   [1] 1
   > x
   [1] 1
   > msg <- "hola"

La sintaxis del lenguaje determina si una expresión es completa o no.

.. code-block:: rconsole

   > x <- # Expresión incompleta
   +

El operador de asignación requiere del valor a asignar. El símbolo `#` indica
un comentario, es decir todo lo que se encuentra a la derecha del símbolo es
ignorado por R. Es el mecanismo empleado para añadir textos explicativos.

Evaluación
----------

Cuando se introduce una sentencia completa en la consola, esta es evaluada. En
la consola interactiva, el contenido de los objetos de imprime directamente.

.. code-block:: rconsole

   > N <- 6
   > N          # impresión automática.
   [1] 6
   > print(N)   # impresión explícita.
   [1] 6

El `[1] 6` indica que es el primer elemento de `N`.

Creación de vectores
--------------------

Se utiliza la función `c()` "combinar", para crear objetos de vectores.

.. code-block:: rconsole

   > x <- c(3.3, 8) # numérico
   > x <- c(FALSE, TRUE) # lógico
   > x <- c("x", "y", "z") # carácter
   > x <- 5:10 # entero (secuencia)
   > x <- c(2+3i, 4-5i) # complejo

Se puede utilizar la función `vector()` o una función de cuyo nombre sea clase
del vector a crear:

.. code-block:: rconsole

   > vector("character", 5)
   [1] "" "" "" "" ""
   > character(5)
   [1] "" "" "" "" ""

Mezcla de objetos
-----------------

Si se mezclan valores de distintas clases en un vector, estos se "coercionan".
Es decir, se cambia la clase de los valores para obligar que todos sean de la
misma clase.

.. code-block:: rconsole

   > x <- c("hola", 4) # character
   > x
   [1] "hola" "4"
   > y <- c(TRUE, 5) # numerical
   > y
   [1] 1 5

Funciones del tipo `as.numeric()` o `as.logical` se pueden utilizar para
realizar una coerción explícita del vector. Cuando los valores no pueden ser
coercionados al tipo indicado devuelve valores especiales del tipo `NA`
(No disponible, "Not Available").

.. code-block:: rconsole

   > z <- as.numeric(x)
   Mensajes de aviso perdidos
   NAs introducidos por coerción
   > z
   [1] NA  4

Matrices
--------

Son arreglos con un atributo de dimensión de dos valores enteros que hacen
referencia al número de filas y columnas.

.. code-block:: rconsole

   > A <- matrix(1:12, nrow = 3, ncol = 4)
   > A
        [,1] [,2] [,3] [,4]
   [1,]    1    4    7   10
   [2,]    2    5    8   11
   [3,]    3    6    9   12
   > dim(A)
   [1] 3 4

Del ejemplo anterior, se puede observar que los valores se ordenan por
defecto en la matriz por columnas. A menos que se establezca lo contrario
mediante el argumento `byrow = TRUE`.

Se puede crear una matriz añadiendo un atributo de dimensión a un vector.

.. code-block:: rconsole

   > w <- 1:10
   > dim(w) <- c(2,5)
   > w
        [,1] [,2] [,3] [,4] [,5]
   [1,]    1    3    5    7    9
   [2,]    2    4    6    8   10

También se pueden crear matrices uniendo vectores como filas (`rbind()`, row
binding) o como columnas (`cbind()`, column binding).

.. code-block:: rconsole

   > x <- 3:5
   > y <- 10:12
   > rbind(x, y)
     [,1] [,2] [,3]
   x    3    4    5
   y   10   11   12

Listas
------

Las listas son objetos que contienen elementos de distintas clases. Son tipos
de datos muy importantes en R, ya que son la base de datos estructurados.

.. code-block:: rconsole

   > l <- list("a", 5, TRUE, 1 + 4i)
   > l
   [[1]]
   [1] "a"

   [[2]]
   [1] 5

   [[3]]
   [1] TRUE

   [[4]]
   [1] 1+4i

Factores
--------

Los factores se utilizan para representar datos categóricos ordenados o
desordenados. Se pueden concebir como valores enteros asociados a etiquetas.
A diferencia de los enteros los factores son descriptivos.

Son especialmente importantes para especificar modelos estadísticos, ya sea
en modelos de regresión, o para la generación de gráficos.

Los distintos valores de un factor se conocen como "niveles" (levels).

.. code-block:: rconsole

   > t <- factor(c("f", "e", "d"))
   > t
   [1] f e d
   Levels: d e f
   > levels(t) <- c("c", "d", "e", "f")
   > t
   [1] e d c
   Levels: c d e f

La función `levels()` puede utilizarse para especificar el orden de los niveles,
y los posibles valores que puede tener un factor.

.. code-block:: rconsole

   > t[3] <- "o"
   Mensajes de aviso perdidos
   In `[<-.factor`(`*tmp*`, 3, value = "o") :
     invalid factor level, NA generated

Valores Faltantes
-----------------

Son aquellos denotados por `NA` o `NaN` para los que se generan por operaciones
matemáticas indefinidas.

Para verificar si los objetos son `NA` o `NaN` se pueden utilizar
respectivamente las funciones `is.na()` o `is.nan()`.

Los valores `NA` pertenecen a una clase, por esta razón se tienen valores
`NA` enteros, `NA` lógicos, etc.

Un valor `NaN` es `NA` pero no a la inversa.

.. code-block:: rconsole

   > s <- c(NA, 5, 0 / 0)
   > s
   [1]  NA   5 NaN
   > is.na(s)
   [1]  TRUE FALSE  TRUE
   > is.nan(s)
   [1] FALSE FALSE  TRUE

Data Frames
-----------

La traducción literal sería "marcos de datos. Representan datos tabulares.

* Son un tipo especial de lista en la que todos sus elementos tienen la misma
  longitud.
* Cada elemento puede concebirse como una columna. Y cada fila denota a
  los objetos que están en la misma posición en todas las columnas.
* A diferencia de las matrices pueden tener distintas clases de objetos en
  cada columna.

Los objetos de R tienen nombres `names()` que se asocian a cada elemento.

.. code-block:: rconsole

   > f <- 1:5
   > f
   [1] 1 2 3 4 5
   > names(f) <- c("a", "b", "c", "d", "e")
   > f
   a b c d e
   1 2 3 4 5

Los data frames tienen el atributo especial `row.names()` que permite asociar
nombres a las filas.

.. code-block:: rconsole

   > data.frame(label=c("a", "b", "c"),
   + value= 1:3,
   + row.names = c("uno", "dos", "tres"))
        label value
   uno      a     1
   dos      b     2
   tres     c     3

Del mismo modo, se pueden asociar nombres a las filas y a las columnas de una
matriz con las funciones `rownames()` y `colnames()` respectivamente. O de
forma simultánea con `dimnames()`.

.. code-block:: rconsole

   > f <- matrix(1:8, nrow = 2, ncol = 4,
   + dimnames = list(c("uno", "dos"),1:4))
   > f
       1 2 3 4
   uno 1 3 5 7
   dos 2 4 6 8
   > colnames(f) <- c("I", "II", "III", "IV")
   > f
       I II III IV
   uno 1  3   5  7
   dos 2  4   6  8

Lectura y escritura de datos
============================

Estructuras de control
======================

Funciones
=========

Reglas de alcance
=================

Manejo de datos temporales
==========================
