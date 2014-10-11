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

Se pueden crear objetos vacíos con la función ``vector()``.

Valores numéricos
-----------------

En R los números se tratan por defecto como objetos numéricos (es decir, valores
de punto flotante de doble precisión). Si se desea establecer un valor numérico
como entero se le debe agregar el sufijo ``L``.

Por ejemplo: ``1`` denota a un objeto numérico; mediante ``1L`` el usuario establece
explícitamente un entero.

El valor numérico especial ``Inf`` denota simbólicamente al infinito. Por ejemplo:

.. code-block:: rconsole

   > 1 / 0
   [1] Inf

El valor ``NaN`` (Not a number) representa un valor indefinido.

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
``attributes()``.

Asignación
----------

En el terminal/consola de R se escriben expresiones. El símbolo ``<-`` es el
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

El operador de asignación requiere del valor a asignar. El símbolo ``#`` indica
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

El ``[1] 6`` indica que es el primer elemento de ``N``.

Creación de vectores
--------------------

Se utiliza la función ``c()`` "combinar", para crear objetos de vectores.

.. code-block:: rconsole

   > x <- c(3.3, 8) # numérico
   > x <- c(FALSE, TRUE) # lógico
   > x <- c("x", "y", "z") # carácter
   > x <- 5:10 # entero (secuencia)
   > x <- c(2+3i, 4-5i) # complejo

Se puede utilizar la función ``vector()`` o una función de cuyo nombre sea clase
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

Funciones del tipo ``as.numeric()`` o ``as.logical`` se pueden utilizar para
realizar una coerción explícita del vector. Cuando los valores no pueden ser
coercionados al tipo indicado devuelve valores especiales del tipo ``NA``
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
mediante el argumento ``byrow = TRUE``.

Se puede crear una matriz añadiendo un atributo de dimensión a un vector.

.. code-block:: rconsole

   > w <- 1:10
   > dim(w) <- c(2,5)
   > w
        [,1] [,2] [,3] [,4] [,5]
   [1,]    1    3    5    7    9
   [2,]    2    4    6    8   10

También se pueden crear matrices uniendo vectores como filas (``rbind()``, row
binding) o como columnas (``cbind()``, column binding).

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

La función ``levels()`` puede utilizarse para especificar el orden de los niveles,
y los posibles valores que puede tener un factor.

.. code-block:: rconsole

   > t[3] <- "o"
   Mensajes de aviso perdidos
   In ``[<-.factor``(``*tmp*``, 3, value = "o") :
     invalid factor level, NA generated

Valores Faltantes
-----------------

Son aquellos denotados por ``NA`` o ``NaN`` para los que se generan por operaciones
matemáticas indefinidas.

Para verificar si los objetos son ``NA`` o ``NaN`` se pueden utilizar
respectivamente las funciones ``is.na()`` o ``is.nan()``.

Los valores ``NA`` pertenecen a una clase, por esta razón se tienen valores
``NA`` enteros, ``NA`` lógicos, etc.

Un valor ``NaN`` es ``NA`` pero no a la inversa.

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

Los objetos de R tienen nombres ``names()`` que se asocian a cada elemento.

.. code-block:: rconsole

   > f <- 1:5
   > f
   [1] 1 2 3 4 5
   > names(f) <- c("a", "b", "c", "d", "e")
   > f
   a b c d e
   1 2 3 4 5

Los data frames tienen el atributo especial ``row.names()`` que permite asociar
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
matriz con las funciones ``rownames()`` y ``colnames()`` respectivamente. O de
forma simultánea con ``dimnames()``.

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

Funciones básicas
-----------------

Hay unas cuantas funciones básicas para introducir datos a R:

* ``read.table()``, ``read.csv()``, para leer datos de forma tabular desde archivos
  de texto.
* ``readLines()``, para leer información de archivos de texto como un vector de
  clase carácter.
* ``source()``, para ejecutar código R. El contrario de ``dump()``.
* ``dget()``, carga un objeto de R guardado como una representación en texto
  almacenado con ``dput()``.
* ``load()``, para cargar *espacios de trabajo* almacenados en formato ``.RData``.
* ``unserialize()``, para leer objetos de R individuales guardados en formato
  binario.

Existen las siguientes funciones análogas para escribir datos:

* ``write.data()``
* ``writeLines()``
* ``dump()``
* ``dput()``
* ``save()``
* ``serialize()``

Leer archivos con ``read.table()``
--------------------------------

La función ``read.table()`` es una de las mas utilizadas, entre sus argumentos
mas importantes tenemos:

* ``file``, nombre de un archivo o conexión.
* ``header``, valor lógico que indica si el archivo tiene una línea de cabecera.
* ``sep``, la cadena de caracteres usada como separador de columnas.
* ``colClasses``, un vector clase carácter que indica la clase de cada columna.
* ``nrows``, el número de filas de un conjunto de datos.
* ``comment.char``, la cadena de caracteres usada como indicador de comentarios.
* ``skip``, el número de líneas a saltar al principio.
* ``stringsAsFactors``, valor lógico que indica si las columnas de tipo carácter
  serán codificadas como factores.

Para conjuntos de datos pequeños y medianos, se pueden ejecutar ``read.table()``
sin ningún otro argumento.

.. code-block:: r

   data <- read.table("chiguire.txt")

La función automáticamente:

* Saltará todas las líneas que empiezan con ``#``.
* Determinará cuantas líneas son y cuanta memoria necesitará.
* Determinará la clase mas conveniente para cada columna.
* ``read.csv()`` es similar, pero asume que el separador es una coma.

Para conjuntos de datos mas grandes, las siguientes recomendaciones pueden ser
útiles:

* Leer la página de ayuda de ``help.table()``, que contiene muchas pistas.
* Hacer un cálculo grueso de la memoria requerida, si excede la cantidad de
  RAM disponible es hora de pensar en otro método.
* Establecer ``comment.char = ""`` si no hay líneas comentadas en el archivo.
* Especificar el argumento ``colClasses``, hará la lectura mucho mas rápida.

.. code-block:: r

   initial <- read.table("datatable.txt", nrows = 100)
   classes <- sapply(initial, class)
   tabAll <- read.table("datatable.txt", colClasses = classes)

En este caso se utilizan las clases que el propio R estima leyendo las primeras
100 filas para leer el archivo completo.

 * Establecer el argumento ``nrows``, lo que permite controlar el uso de memoria.
   Puede usarse para leer un archivo muy grande por partes.

Todo pasa por conocer nuestro sistema, las especificaciones de hardware, la
arquitectura del procesador, el sistema operativo utilizado, las aplicaciones
en memoria y los usuarios con sesiones abiertas.

Por ejemplo, un data.frame de millón y medio de filas y 120 columnas de datos
numéricos (8 bytes por valor) requerirá aproximadamente de:

.. code-block:: rconsole

   > mem <- 1500000 * 120 * 8 # bytes
   > mem <- mem / 2^20 # megabytes
   > mem
   [1] 1373.291
   > mem <- mem / 1024 # gigabytes
   > mem
   [1] 1.341105

Representaciones de texto
-------------------------

Utilizando ``dput()`` se pueden obtener representaciones de los datos en archivos
de texto, que se pueden editar y recuperar.

Estas representaciones preservan los metadatos, y funcionan mejor con sistemas
de control de versiones y la "filosofía Unix" en general

Tienen el problema que pueden requerir un gran espacio de almacenamiento.

.. code-block:: rconsole

   > y <- data.frame(a = c(1, 2), b = c("uno", "dos"))
   > dput(y)
   structure(list(a = c(1, 2), b = structure(c(2L, 1L),
   .Label = c("dos", "uno"), class = "factor")),
   .Names = c("a", "b"), row.names = c(NA, -2L), class = "data.frame")
   > dput(y, file = "y.R")
   > y.nuevo <- dget("y.R")
   > y.nuevo
     a   b
   1 1 uno
   2 2 dos

Se utiliza ``dump()`` para almacenar representaciones de texto de objetos como
asignaciones que pueden ser cargados en memoria por lotes con ``source()``.

.. code-block:: rconsole

   > x <- pi
   > y <- data.frame(a = c(1, 2), b = c("uno", "dos"))
   > dump(c("x", "y"), file = "data.R")
   > rm(x, y)
   > source("data.R")
   > x
   [1] 3.141593
   > y
     a   b
   1 1 uno
   2 2 dos

Interfaces con el mundo exterior
--------------------------------

Se pueden obtener datos utilizando *interfaces* de conexión. La conexiones
pueden ser archivos u otras cosas mas exóticas:

* ``file()``, abre una conexión a un archivo.
* ``gzfile()``, abre una conexión a un archivo comprimido como ``gzip``.
* ``bzfile()``, abre una conexión a un archivo comprimido como ``bzip2``.
* ``url()``, abre una conexión a un recurso en Internet, usualmente un sitio web.

Las funciones de conexión en general tienen los argumentos:

* ``description``, para ``file()`` y otras conexiones a archivo es la ruta y nombre
  del archivo, para ``url()`` la dirección web.
* ``open``, es el tipo de la conexión, ``"r"`` para solo lectura, ``"w"`` para iniciar
  un nuevo archivo y escribir, ``"a"`` para añadir, y ``"rb"``, ``"wb"`` y ``"ab"`` los
  equivalentes en modo binario (Windows).

Las conexiones permiten leer archivos de forma secuencial. Por ejemplo, si
tenemos el archivo de texto comprimido ``"palabras.txt.gz"``. Se podría leer como
sigue:

.. code-block:: rconsole

   > con <- gzfile("palabras.txt.gz")
   > x <- readLines(con)
   > x
   [1] "hola" "chao" "ula"  "luna"

La función ``writeLines()`` toma como argumento un vector de clase carácter y
escribe cada elemento como una línea de una archivo de texto.

Se puede igualmente utilizar una conexión para obtener el código de una página
web.

.. code-block:: rconsole

   > con <- url("http://www.ine.gob.ve", "r")
   > y <- readLines(con)
   Mensajes de aviso perdidos
   ...
   > head(y)
   [1] ""
   [2] "<!DOCTYPE html PUBLIC \"-//W3C//DTD XHTML 1.0 Transitional//EN\" \"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd\">"
   [3] "<html xmlns=\"http://www.w3.org/1999/xhtml\">"
   [4] "<head>"
   [5] ""
   [6] "<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\" />"

Estructuras de control
======================

Las estructuras de control básicas de R son:

* ``if``, ``else``: ejecuta un bloque de código si se satisface una condición.
* ``for``: ejecuta un bloque de código un número fijo de veces.
* ``while``: ejecuta un bloque de código mientras se cumpla una condición.
* ``repeat``: ejecuta un boque de código hasta encontrar un ``break``.
* ``next``: salta a la siguiente iteración en un ``for``, ``while`` o ``repeat``.
* ``return``: devuelve el resultado de una función y sale.

La mayoría de las estructuras de control no se utilizan en sesiones interactivas
sino en programas de R.

if
---

Un estructura ``if`` valida es como sigue:

.. code-block:: r

   if (x > 3) {
       y <- 10
   } else if (x < -3) {
       y <- -10
   } else {
       y <- 0
   }

Esto es: si la condición ``x > 3`` se satisface se ejecuta el código a
continuación encerrado entre llaves. La siguientes sentencias son opcionales,
se pueden colocar tantos ``else if(<condición>)`` "de lo contrario si" como sean
necesarios, y de ser necesario una sentencia ``else`` final.

Debido a la naturaleza funcional de R, la siguiente expresión es equivalente:

.. code-block:: r

   y <- if (x > 3) {
       10
   } else if (x < -3) {
       -10
   } else {
       0
   }

   # los bloques de una sola línea pueden prescindir de las llaves

   y <- if (x > 3) 10 else if (x < -3) -10 else 0

for
---

En los bucles ``for`` a una variable *iteradora* le asignan valores sucesivos de
un vector o secuencia.

Los siguientes bucles son equivalentes:

.. code-block:: r

   x <- c("a", "b", "c", "d")

   for (i in 1:4) {
       print(x[i])
   }
   for (i in seq_along(x)) {
       print(x[i])
   }
   for (letter in x) {
       print(letter)
   }
   for (i in 1:4) print(x[i])

Es posible escribir bucles dentro de bucles, esto es, bucles anidados.


Funciones
=========

Las funciones se utilizan para reorganizar el código, ya sea para contener
secuencias de expresiones utilizadas de forma reiterada, o para separar el
código en componentes mas comprensibles.

Se crean utilizando la directiva ``function()`` y se almacenan como cualquier
otro objeto. Son, de hecho, objetos de la clase *function*.

Tienen la siguiente sintaxis básica:

.. code-block:: r

   function( arglist )
       expr
       return(value)

* ``arglist`` es una lista de argumentos.
* Si ``expr`` consta de más de una expresión debe estar encerrado entre llaves.
* la sentencia ``return`` es opcional, por defecto las funciones en R devuelven el
  valor de la última expresión.

En R, en virtud de su naturaleza funcional, las funciones son *objetos de
primera clase*, lo que implica que:

* Pueden pasarse como argumentos de otras funciones.
* Pueden anidarse, esto es, definir funciones dentro de funciones.
* Devuelven el valor de la última expresión, a menos que hay una indicación
  explícita con ``return()``

Argumentos
----------

Las funciones tienen argumentos con nombre a los que pueden asignarse valores
por defecto.

* Los argumentos que aparecen en la definición de la función se denominan
  *argumentos formales*.
* La función ``formals`` devuelve una lista con los argumentos formales de una
  función.

.. code-block:: rconsole

   > f <- function(a, b) a + b
   > formals(f)
   $a

   $b

* Las llamadas a las funciones de R no tienen que utilizar todos los argumentos.
  Algunos pueden ser quedar *faltantes* y otros tener valores por defecto.

Coincidencia de argumentos
--------------------------

Los argumentos pueden coincidir por posición o por el nombre. Todas las llamadas
a continuación de la función ``sd`` son equivalentes:

.. code-block:: rconsole

   > midata <- rnorm(100)
   > sd(midata)
   > sd(x = midata)
   > sd(x = midata, na.rm = FALSE)
   > sd(na.rm = FALSE, x = midata)
   > sd(na.rm = FALSE, midata)

Cuando un argumento coincide por nombre, se saca de la lista de argumentos.
De manera que los restantes mantienen el mismo orden.

Por ejemplo, en el caso de la función ``lm()`` que se utiliza para ajustar
modelos lineales que tiene los siguientes argumentos:

.. code-block:: rconsole

   > args(lm)
   function (formula, data, subset, weights, na.action, method = "qr",
       model = TRUE, x = FALSE, y = FALSE, qr = TRUE, singular.ok = TRUE,
       contrasts = NULL, offset, ...)

Las dos llamadas siguientes son equivalentes:

.. code-block:: r

   lm(data = mydata, y ~ x, model = FALSE, 1:100)
   lm(y ~ x, mydata, 1:100, model = FALSE)

Los argumentos pueden tener una *coincidencia parcial*. Esto es, se pueden hacer
coincidir los argumentos por nombre sin tener que escribir el argumento completo
siempre que no haya ambigüedad.

Los siguientes llamados son equivalentes:

.. code-block:: r

   seq.int(0, 1, len = 11)
   seq.int(0, 1, length.out = 11)

   ls(all = TRUE)
   ls(all.names = TRUE)

Los argumentos de las funciones de R también emplean *evaluación perezosa*, esto
implica que solamente se consideran necesarias en la medida que se utilizan
dentro de la función. Por ejemplo, el siguiente código corre sin problemas.

.. code-block:: r

   f <- function(a, b) {
       a^2
   }
   f(2)

En este caso, como b nunca es utilizado, no genera error. De hecho, se
ejecutarían todas las sentencias hasta encontrar una referencia a ``b``.

Se puede utilizar ``...`` para indicar un número de argumentos variable, o el pase
de argumentos de forma implícita. Generalmente se utilizan para extender
funciones.

.. code-block:: r

   myplot <- function(x, y, type = "l", ...) {
   plot(x, y, type = type, ...)
   }

Los argumentos formales que aparecen después de ``...`` deben ser explícitos y no
admiten coincidencias parciales.

.. code-block:: r

   > args(paste)
   function (..., sep = " ", collapse = NULL)
   > paste("a", "b", sep = ":")
   [1] "a:b"
   > paste("a", "b", se = ":")
   [1] "a b :"

Reglas de alcance
=================

Manejo de datos temporales
==========================
