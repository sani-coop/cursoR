Análisis Exploratorio de Datos
==============================

Representaciones visuales de los datos
----------------------------------------

El análisis exploratorio de datos a través de gráficos cumple con los
siguientes principios:

- Principio 1: muestra comparaciones

- Principio 2: muestra causalidad, mecanismos y explicación

- Principio 3: muestra datos multivariantes

- Principio 4: integra múltiples nosod de evidencia

- Principio 5: describe y documenta la evidencia

- Principio 6: es el reino del contenido

¿Por qué se utilizan gráficos en análisis de datos?
*****************************************************

- Para entender las propiedades de los datos.

- Para encontrar patrones en los datos.

- Para sugerir estrategias de modelado.

- Para análisis de "depuración".

- Para comunicar resultados.


Sin embargo, los **gráficos exploratorios** se usan sólo para **comunicar
resultados**.


Características de los gráfcos exploratorios
***********************************************

- Se hacen rápido.

- Se hacen en gran cantidad.

- El propósito es el entendimiento personal.

- Los ejes y las leyendas generalmente son limpiadas (a posteriori).

- El color y el tamaño se utilizan generalmente para obtener información.

Ejemplo:

La contaminación ambiental en Estados Unidos
**********************************************

- La Agencia de Protección Medioambiental de Estados Unidos (EPA por sus siglas
  en inglés) establece estándares nacionales ambientales de la calidad del aire
  para la contaminación del aire exterior

    - `Estándares Nacionales Ambientales de Calidad del Aire <http://www.epa.gov/air/criteria.html>`_

- Para contaminción de partícula fina (PM2.5), la "media anual, premediada
  durante 3 años" no puede exceder :math:`12\mu g/m^3`

- Los datos de PM2.5 diarios están disponibles desde la página web del EPA

    - `Sistema de Calidad del Aire de EPA`_

.. _Sistema de Calidad del Aire de EPA: http://www.epa.gov/ttn/airs/airsaqs/detaildata/downloadaqsdata.htm

- **Pregunta**: ¿Hay lugares en Estados Unidos que superen ese estandar nacional
  de contaminación de partícula fina?


Datos
******

Promedio anual PM2.5 acumulado desde el 2008 al 2010

.. code-block:: r

    pollution <- read.csv("data/avgpm25.csv",
                          colClasses = c("numeric", "character",
                                         "factor", "numeric", "numeric"))
                          head(pollution)

    ##        pm25   fips   region   longitude   latitude
    ##  1    9.771  01003     east      -87.75      30.59
    ##  2    9.994  01027     east      -85.84      33.27
    ##  3   10.689  01033     east      -87.73      34.73
    ##  4   11.337  01049     east      -85.80      34.46
    ##  5   12.120  01055     east      -86.03      34.02
    ##  6   10.828  01069     east      -85.35      31.19


¿Alguna localidad excede el estándar de :math:`12\mu g/m^3`?


Resúmenes simples de datos con una dimensión
**********************************************


- Resumen de cinco números

- Boxplots

- Histogramas

- Gráfico de densidad

- Gráfico de barras


Sumario de cinco números
^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: r

    summary(pollution$pm25)

    ##  Min.  1st Qu.    Median   Mean   3rd Qu.    Max.
    ##  3.38     8.55     10.00   9.84    11.40    18.40


Boxplot
^^^^^^^^^

.. code-block:: r

    boxplot(pollution$pm25, col = "blue")


Histograma
^^^^^^^^^^^

.. code-block:: r

    hist(pollution$pm25, col = "green")


.. code-block:: r

    hist(pollution$pm25, col = "green")
    rug(pollution$pm25


.. code-block:: r

    hist(pollution$pm25, col = "green", breaks = 100)
    rug(pollution$pm25)


Características externas
^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: r

    boxplot(pollution$pm25, col = "blue")
    abline(h = 12)

.. code-block:: r

    hist(pollution$pm25, col = "green")
    abline(v = 12, lwd = 2)
    abline(v = median(pollution$pm25), col = "magenta", lwd = 4)


Gráfico de barras
^^^^^^^^^^^^^^^^^^

.. code-block:: r

    barplot(table(pollution$region), col = "wheat",
            main = "Number of Counties in Each Region")


Resumenes simples de datos con dos dimensiones
************************************************

- Puntos 1-D múltiples/superpuestos (``Latice`` y ``ggplot2``)

- Diagramas de dispersión

- Diagramas de dispersión suavizados


Más de 2 dimensiones

- Gráficos 2-D múltiples/superpuestos; coplots

- Uso de color, tamaño, y sobertura para añadir dimensiones

- Graficos Spinning

- Gráficos reales 3D (no muy útiles)


Boxplots múltiples
^^^^^^^^^^^^^^^^^^^

.. code-block:: r

    boxplot(pm25 ~ region, data = pollution, col = "red")


Múltiples histogramas
^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: r

    par(mfrow = c(2, 1), mar = c(4, 4, 2, 1))
    hist(subset(pollution, region == "east")$pm25, col = "green")
    hist(subset(pollution, region == "west")$pm25, col = "green")


Diagrama de dispersión
^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: r

    with(pollution, plot(latitude, pm25))
    abline(h = 12, lwd = 2, lty = 2)

Usando color

.. code-block:: r

    with(pollution, plot(latitude, pm25, col = region))
    abline(h = 12, lwd = 2, lty = 2)



Diagramas de dispresión múltiple
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: r

    par(mfrow = c(1, 2), mar = c(5, 4, 2, 1))
    with(subset(pollution, region == "west"), plot(latitude, pm25, main = "West"))
    with(subset(pollution, region == "east"), plot(latitude, pm25, main = "East"))


Resumen
********

- Los gráficos exploratorios son "rápidos y sucios"

- Permiten resumir los datos (a menudo de forma gráfica) y destacar cualquier
  característica común

- Explora preguntas básicas e hipótesis (y quizás las descarte)

- Sugiere estrategias de modelado para los siguientes pasos


lattice
^^^^^^^^

Utilizando ``lattice``:

- Los gráficos se crean con un simple llamado a la función (``xyplot``,
  ``bwplot``, etc.)

- Son muy útiles para tipos de gráficos condicionados: observar como :math:`y`
  cambia con :math:`x` a través de niveles de :math:`z`

- Elementos como márgenes/espaciado se establecen automáticamente ya que todo
  el gráfico se especifica al principio.

- Es muy útil para colocar muchos, muchos gráficos juntos en una pantalla

- Muchas veces resulta poco útil para especificar un gráfico completo en una
  única llamada de función

- Los comentarios y anotaciones en el gráfico no son intuitivos

- El uso de las funciones del panel y los subscripts se dificulta y requiere
  una preparación intensa.

- No se puede añadir al gráfico una vez que se ha creado.


ggplot2
^^^^^^^^

- Divide la diferencia entre ``base`` y ``lattice``

- Trabaja de modo automático con espaciado, texto, títulos, y también permite
  hacer anotaciones a través de ``adding``

- Tiene una similaridad superficial con el ``lattice`` pero en general es mucho
  más intuitivo y sencillo de utlizar.

- El modo por defecto tiene muchas opciones disponibles (¡que pueden
  configurarse!)

- Es una implementación de *Grammar of Graphics* de Leland Wilkinson

- Fue escrita por Hadley Wickham (mientras era estudiante en la Universidad de
  Iowa)

- Está disponible desde CRAN a través de ``install.packages()`` (http://ggplot2.org)

- Es pensar los gráficos en términos de "verbo", "sustantivo", "adjetivo"


Elementos Básicos
******************

``qplot()``
^^^^^^^^^^^^

- Trabaja muy parecido a la función ``plot`` en sistemas gráficos base

- Busca datos en un conjunto de datos, tal como ``lattice``, o el entorno
  inmediato

- Los gráficos se hacen con *estética* (tamaño, color, forma) y *geometría*
  (puntos, líneas)

- Aquellos elementos que son importates para indicar subconjuntos de datos
  deben ser **etiquetados** (si tienen propiedades distintas)

- ``qplot()`` oculta lo que ocurre en el interior, lo cual está bien para la
  mayoría de las operaciones

- ``ggplot()`` es la función central y es mucho más flexible para hacer cosas
  que no se pueden hacer con ``qplot()``


Ejemplo
^^^^^^^^

.. code-block:: r

    library(ggplot2)
    str(mpg)

    'data.frame':   234 obs. of  11 variables:
     $ manufacturer: Factor w/ 15 levels "audi","chevrolet",..: 1 1 1 1 1 1 1 1 1 1 ...
     $ model       : Factor w/ 38 levels "4runner 4wd",..: 2 2 2 2 2 2 2 3 3 3 ...
     $ displ       : num  1.8 1.8 2 2 2.8 2.8 3.1 1.8 1.8 2 ...
     $ year        : int  1999 1999 2008 2008 1999 1999 2008 1999 1999 2008 ...
     $ cyl         : int  4 4 4 4 6 6 6 4 4 4 ...
     $ trans       : Factor w/ 10 levels "auto(av)","auto(l3)",..: 4 9 10 1 4 9 1 9 4 10 ...
     $ drv         : Factor w/ 3 levels "4","f","r": 2 2 2 2 2 2 2 1 1 1 ...
     $ cty         : int  18 21 20 21 16 18 18 18 16 20 ...
     $ hwy         : int  29 29 31 30 26 26 27 26 25 28 ...
     $ fl          : Factor w/ 5 levels "c","d","e","p",..: 4 4 4 4 4 4 4 4 4 4 ...
     $ class       : Factor w/ 7 levels "2seater","compact",..: 2 2 2 2 2 2 2 2 2 2 ...


``ggplot2`` "Hello, world!"
^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: r

    qplot(displ, hwy, data = mpg)


Modificando la estética
^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: r

    qplot(displ, hwy, data = mpg, color = drv)


Añadiendo geometría
^^^^^^^^^^^^^^^^^^^^

.. code-block:: r

    qplot(displ, hwy, data = mpg, geom = c("point", "smooth"))

Histogramas
^^^^^^^^^^^^

.. code-block:: r

    qplot(hwy, data = mpg, fill = drv)

Facetas
^^^^^^^^

.. code-block:: r

    qplot(displ, hwy, data = mpg, facets = . ~ drv)
    qplot(hwy, data = mpg, facets = drv ~ ., binwidth = 2)



Creación de gráficos analíticos
-------------------------------


Resúmenes exploratorios de datos
--------------------------------


Visualización multidimensional exploratoria
-------------------------------------------

El análisis multivariante está relacionado con conjuntos de datos que
involucran más de una variable para cada unidad de observación. En general
tales conjuntos de datos vienen representados por matrices de datos :math:`X`
con :math:`n` filas y :math:`p` columnas, donde las filas representan los casos,
y las columnas las variables. En cuanto al análisis de tales matrices,
unas veces estaremos interesados en estudiar las filas (casos),
otras veces las relaciones entre las variables (las columnas) y algunas veces
en estudiar simultáneamente ambas cosas.

Graficos sin hacer transformaciones
***********************************

El primer comando que tenemos disponible para la tarea de explorar datos
multivariantes es:

.. code-block:: rconsole

   > pairs()

Vamos a ver cómo se puede utilizar para el conjunto de datos iris,
que contiene una base de 4 medidas tomadas sobre una muestra de 150 flores
ornamentales (lirios). Las medidas tomadas son la longitud y anchura de
pétalos y sépalos. En el problema original, se trataba de clasificar las
flores en tres especies predeterminadas, utilizando las cuatro medidas
mencionadas.

.. code-block:: rconsole

   > data(iris) # Se carga el conjunto de datos iris
   > iris[1:4,] # Se muestran 4 registros
   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
   1          5.1         3.5          1.4         0.2  setosa
   2          4.9         3.0          1.4         0.2  setosa
   3          4.7         3.2          1.3         0.2  setosa
   4          4.6         3.1          1.5         0.2  setosa
   > pairs(iris[,1:4]) # Se muestran los gráficos de dispersion de las variables iris

El equivalente (mejorado) de este gráfico con el paquete ``ggplot2`` se
realizaba con la función ``plot.matrix()``. Esta función ha quedado obsoleta
como indica el siguiente mensaje:

.. code-block:: rconsole

   > library(ggplot2)
   > plotmatrix(iris[, 1:4])
   Error: Please use GGally::ggpairs. (Defunct; last used in version 0.9.2)

De manera que, tal como nos indican, se instala el paquete ``GGally`` y se
utiliza la función ``ggpairs()``

.. code-block:: rconsole

   > library(GGally)
   > ggpairs(iris[, 1:4])

Ahora se van a dibujar los datos etiquetados por especie. Asi se podra ver si
las flores de una misma especie aparecen agrupadas. Las flores que aparezcan
entre dos grupos corresponderán a flores difíciles de clasificar claramente
en uno de los grupos.

Debido a que las etiquetas son bastante largas ("setosa", "virginica",
"versicolor"), se sustituyen por letras, para facilitar la visualización. Se
usará la "s" para setosa, la "v" para virginica, y la "c" para versicolor:

.. code-block:: rconsole

    > iris.especies = c(rep("s",50),rep("c",50),rep("v",50))

Para dibujar incluyendo las etiquetas, podemos proceder asi:

.. code-block:: rconsole

    > pairs(iris[,1:4], panel=function(x,y,...) text(x,y,iris.especies))

El comando anterior dibujará las coordenadas de los datos, 2 a 2,
etiquetando con la clase de cada flor.

Un modo alternativo de proceder es generarnos nosotros mismos gráficos
similares. Podríamos hacer (entre otras posibilidades), algo así como:

.. code-block:: rconsole

   > par(mfrow=c(1,2))
   > plot(iris[,1], iris[,2])
   > points(iris[iris.especies=="s", 1], iris[iris.especies=="s", 2], col=2)
   > points(iris[iris.especies=="c", 1], iris[iris.especies=="c", 2], col=3)
   > points(iris[iris.especies=="v", 1], iris[iris.especies=="v", 2], col=4)
   > plot(iris[,1], iris[, 3])
   > points(iris[iris.especies=="s", 1], iris[iris.especies=="s", 3], col=2)
   > points(iris[iris.especies=="c", 1], iris[iris.especies=="c", 3], col=3)
   > points(iris[iris.especies=="v", 1], iris[iris.especies=="v", 3], col=4)

La primera orden hace que la pantalla de dibujo quede dividida como una
matrix 1 x 2, esto es, una matriz con una fila y dos columnas, por tanto,
dos graficos en uno de esa manera. La segunda orden dibuja en el primer
gráfico las coordenadas 1 y 2 de los datos. La tercera orden y siguientes van
repintando los puntos que corresponden a cada especie. Obsérvese que al dar
cada orden lo que sucede. El segundo bloque de ordenes pinta la coordenada 1
frente a la dos y hace lo mismo.

Graficos transformando los datos
********************************

Cuando los datos tienen más de 3 dimensiones, dibujar las coordenadas 2 a 2
puede ser interesante, pero cuando hay muchas, puede ser más interesante
hacer una proyección de los datos usando algún tipo de transformación lineal
o no linea, que nos permita resumir en un gráfico de dos dimensiones las
características más importantes de los mismos en cuanto a variabilidad,
en cuanto a conservación de distancias en la proyección,
o en cuanto a la representación gráfica de los clusters hallados en el
conjunto de datos.

A continuación pasamos a exponer las diversas técnicas,
junto con los comandos en R que las implementan.

Análisis de componentes principales
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Esta técnica funciona construyendo automáticamente un conjunto de
combinaciones lineales de las variables del conjunto de datos,
con la máxima variabilidad posible. Cada componente principal viene
determinada por un vector propio de la matriz de covarianzas o correlaciones
de los datos, esto es, un vector propio de la matriz  :math:`X^T X`.

Geométricamente, el primer vector propio de la matriz corresponde a la
dirección de máxima variabilidad en el conjunto de datos,
que es la dirección sobre la que se proyectará. Por ejemplo,
si tenemos datos sobre una elipse alargada, y hacemos un análisis de
componentes principales con una sola componente,
la coordenada que representará nuestro conjunto de datos corresponderá a la
proyección de los datos sobre el eje principal de la elipse.

Se puede encontrar información más detallada de la teoría en cualquier libro
de análisis multivariante, y en la red, podéis acudir a los siguientes links,
por ejemplo:

http://cran.r-project.org/doc/vignettes/HSAUR/Ch_principal_components_analysis.pdf (Análisis de componentes principales, Proyecto R).

http://csnet.otago.ac.nz/cosc453/student_tutorials/principal_components.pdf (Lindsay I. Smith, Universidad de Otago, Nueva Zelanda).


Para usar componentes principales hay que cargar una librería de las varias
que hay con R para análisis multivariante (mva, multiv y otras). Por ejemplo:

.. code-block:: rconsole

    > data(iris)   # Se cargan los datos de las flores
    > iris.pca = prcomp(iris[,1:4])  #  Se reliza el análisis de componentes principales
    > attributes(iris.pca)        # Se observa qué devuelve el análisis
    $names
    [1] "sdev"     "rotation" "x"

    $class
    [1] "prcomp"

Ahora dibujamos las dos primeras componentes principales y etiquetamos:

.. code-block:: rconsole

    > plot(iris.pca$x[,1:2],type="n")
    > text(iris.pca$x[iris$Species=="setosa",1:2],col=2,"s")
    > text(iris.pca$x[iris$Species=="virginica",1:2],col=3,"v")
    > text(iris.pca$x[iris$Species=="versicolor",1:2],col=4,"c")

La estructura ``iris.pca$x`` contiene las coordenadas de los puntos,
luego al pedir que dibuja las ``1:2`` estamos pidiendo que dibuje las dos
primeras componentes. El ``type="n"`` hace que dibuje pero que no se vea,
esto es, marca las dos primeras coordenadas. Las demás órdenes dibujan los
puntos por clases con un color distinto para cada clase.

Como se puede apreciar en el gráfico, hemos conseguido representar un
conjunto de dimensión 4 por un conjunto de dimensión 2,
y si se compara con los gráficos ya hechos de los datos iris,
puede verse que la estructura se ha respetado bastante fielmente. Puede
concluirse que hay 3 grupos, uno bien separado y otros dos más juntos. Y en
efecto, así es.

La técnicas de componentes principales se pueden emplear para cualquier
conjunto de datos del cual se quiera tener una idea de la estructura. También
puede utilizarse para codificar información, en el sentido de sustituir la
codificación original de ciertos datos por otra que ocupe menos espacio. Así
por ejemplo, si se dispone de imágenes de fotos (en blanco y negro,
representadas por niveles de gris), puede utilizarse la técnica de
componentes principales para reducir la dimensión de las imágenes. Se puede
encontrar información más detallada a este respecto en:

http://www.willamette.edu/~gorr/classes/cs449/Unsupervised/pca.html (G. B. Orr, Willamette University, USA).

Para saber cúantas componentes son suficientes para representar
fidedignamente un conjunto de datos (por ejemplo, si el conjunto tiene
dimensión 10, deberemos usar 2, 3, ¿cuántas componentes?) se miran los
valores propios asociados a la descomposición comentada más arriba. Tales
valores se guardan en "sdev", de manera que en el ejemplo del iris:

.. code-block:: rconsole

   > iris.pca$sdev
   [1] 2.0562689 0.4926162 0.2796596 0.1543862

Se observa que los valores se dan de mayor a menor. Tales valores corresponden
a variabilidades a lo largo de los ejes de proyección (esto es,
las componentes principales). Para hacernos una idea de su importancia
relativa, iremos calculando la suma acumulada, dividida por la suma total
para que resulte 1:

.. code-block:: rconsole

   > cumsum(iris.pca$sdev^2)/sum(iris.pca$sdev^2)
   [1] 0.9246187 0.9776852 0.9947878 1.0000000

Como seve, con 1 componente se recoge aproximadamente el 92% de la
variabilidad de los datos, lo que no es poco a efectos de visualización. También
se ve que con 3 componentes ya se explica el 99% de la variabilidad. Esto
es, una componente no aporta casi nada de información a efectos de explicar
la estructura de los datos. La razón de tomar los cuadrados es porque la
varianza es el cuadrado de la desviación típica.

Escalado multidimensional
^^^^^^^^^^^^^^^^^^^^^^^^^

La idea del escalado multidimensional es la misma que la del análisis de
componentes principales: Se trata de representar los datos en baja dimensión
(usualmente 2), pero en este caso, en vez de utilizar las coordenadas de los
datos para llevar a cabo la transformación, se usará la información
proporcionada por las distancias entre los datos.

Esta manera de proceder tiene mucho sentido para datos cuyas coordenadas no
se conocen, pero de los cuales conocemos las distancias entre ellos. Por
ejemplo, en un mapa de carreteras vienen las distancias en kilometros entre
diversas ciudades españolas. Esta técnica permite reconstruir las coordenadas
de los datos, conociendo las distancias entre los mismos.

Podéis consultar ejemplos y la teoría más detallada en:

http://www.analytictech.com/networks/mds.htm (Analytic Technologies, USA)

Otro caso en el que el escalado multidimensional resulta útil se da cuando la
información viene dada por matrices de preferencias. Este es el caso cuando
se pide a un grupo de alumnos que asignen preferencias al resto de sus
compañeros.

Como primer ejemplo, consideraremos el que viene con la función ``cmdscale()``.

.. code-block:: rconsole

    > data(eurodist)     # Se cargan los datos en memoria
    > attributes(eurodist)
    $Size
    [1] 21

    $Labels
    [1] "Athens"          "Barcelona"       "Brussels"        "Calais"
    [5] "Cherbourg"       "Cologne"         "Copenhagen"      "Geneva"
    [9] "Gibralta"        "Hamburg"         "Hook of Holland" "Lisbon"
    [13] "Lyons"           "Madrid"          "Marseilles"      "Milan"
    [17] "Munich"          "Paris"           "Rome"            "Stockholm"
    [21] "Vienna"

    $class
    [1] "dist"

    > eurodist.mds = cmdscale(eurodist)

    > x = eurodist.mds[,1]
    > y = eurodist.mds[,2]
    > plot(x, y, type="n", xlab="", ylab="")

    > eurodist2 = as.matrix(eurodist)
    > text(x, y, dimnames(eurodist2)[[1]], cex=0.5)

Asi pues, ``eurodist`` es un objeto que guarda los nombres de las distancias,
la matriz de distancias entre las ciudades, asi como el numero de objetos, que
se podría recuperar escribiendo:

.. code-block:: rconsole

   > attr(eurodist,"Size")
   [1] 21

El proceso de escalado se realiza con la función  cmdscale    y después
se dibuja el mapa de las ciudades con sus nombres. Podéis repetir el proceso
con algunas ciudades españolas. El mapa que obtendréis puede aparecer
rotado o invertido, dado que el método garantiza la conservación de las
distancias, pero eso no garantiza que la orientación en el plano sea la correcta.

A continuación vamos a dibujar un mapa de preferencias entre un grupo
de 8 personas. Cada persona asigna 1 a las personas de su preferencia.
Dicho mapa reflejará grupitos de amigos. Obsérvese que en la diagonal hay
unos, asumiendo que uno es amigo de sí mismo.

Los datos se pueden encontrar aquí:

amigos.txt

La matriz de datos presenta el siguiente aspecto::

    X        paco pepe lola juan carlos luis ana pedro
    paco      1    1    0    0      0    0   0     1
    pepe      1    1    0    0      0    1   0     1
    lola      0    0    1    1      0    0   1     0
    juan      0    0    1    1      0    0   1     1
    carlos    0    1    0    0      1    0   0     1
    luis      1    0    1    0      1    1   0     0
    ana       1    1    0    0      0    0   1     0
    pedro     1    1    0    0      0    0   0     1

Para leer dicha matriz de un archivo, se puede leer directamente, como hago yo
a continuación, o de otra forma que prefiráis. El directorio que pongo yo
en la orden scan es el mío. En vuestro caso, podrá variar.

.. code-block:: rconsole

   > amigos.txt = scan("datos//amigos.txt")
   Read 64 items
   > amigos.txt = structure(amigos.txt,dim=c(8,8),byrow=T)
   > amigos.txt = t(amigos.txt)
   > amigos.txt
   [,1] [,2] [,3] [,4] [,5] [,6] [,7] [,8]
   [1,]    1    1    0    0    0    0    0    1
   [2,]    1    1    0    0    0    1    0    1
   [3,]    0    0    1    1    0    0    1    0
   [4,]    0    0    1    1    0    0    1    1
   [5,]    0    1    0    0    1    0    0    1
   [6,]    1    0    1    0    1    1    0    0
   [7,]    1    1    0    0    0    0    1    0
   [8,]    1    1    0    0    0    0    0    1
   attr(,"byrow")
   [1] TRUE
   > rownames(amigos.txt)<-c("paco","pepe","lola","juan","carlos","luis","ana","pedro")
   > colnames(amigos.txt)<-c("paco","pepe","lola","juan","carlos","luis","ana","pedro")
   > amigos.txt
   > amigos.mds <- cmdscale(amigos.txt)
   > plot(amigos.mds[,1],amigos.mds[,2],type="n")
   > text(amigos.mds[,1],amigos.mds[,2],labels=rownames(amigos.txt))

El comando rownames asigna nombres a las filas, y colnames hace lo propio con
las columnas. El gráfico final muestra una imagen bidimensional de las
preferencias de esas personas

Análisis de cluster
^^^^^^^^^^^^^^^^^^^

El *análisis de cluster*, denominado en español a veces *análisis de
conglomerados*, es una técnica no supervisada para descubrir la estructura de
datos de un conjunto. Entra en el apartado de técnicas gráficas para datos
multivariantes por el siguiente motivo: Cuando unos datos son de alta
dimensión, es bastante dificil descubrir cuántos grupos hay en el conjunto de
datos, y qué dato pertenece a cada grupo, en su caso. El análisis de cluster
engloba una serie de técnicas para dividir el conjunto de datos en una
serie de grupos, y frecuentemente se apoya en métodos de visualización
propios para la visualización de tales grupos.

La información que se usa para llevar a cabo la mayoría de los procedimientos
de análisis de cluster es la proporcionada por las distancias entre los
datos, al igual que en el caso del escalado. Aunque hay varias maneras de
llevar a cabo la construcción de los grupos, hay dos grandes grupos de
técnicas: las técnicas jerárquicas y las técnicas no jerárquicas.

Las técnicas jerárquicas proceden por niveles a la hora de construir los
grupos. En un caso se procede de abajo hacia arriba,
considerando tantos grupos como datos hay en el conjunto,
y agrupando sucesivamente hasta que solo queda un grupo. La visualización del
procedimiento utilizando una herramienta gráfica denominada dendrograma,
ayudará a descubrir la estructura de grupos en los datos. Para ir agrupando
los datos, primero se buscan los datos más cercanos entre sí. En el segundo
nivel se agrupan los grupos más cercanos entre si, y asi sucesivamente. El
otro caso frecuente es partir de un sólo grupo e ir partiendolo en grupos
más pequeños hasta llegar al nivel de los datos individuales. Para todas
estas técnicas jerárquicas hay librerías en R. Aquí Se utilizará la función
``hclust()``.

Para ilustrar el funcionamiento, vamos a utilizar una base de animales descritos por algunas características.
Los datos están en::

    animal.txt
    animal.des
    anima2.des

Se leen los datos:

.. code-block:: rconsole

    > animal = scan(""datos//Animal.txt")
    Read 208 items
    > animal = structure(animal,dim=c(13,16),byrow=T)

Hasta aqui se tiene la matrix animal como una matriz de 16 animales por 13
características. Ahora se leen los nombres de los animales y de las
características:

.. code-block:: rconsole

    animal.nombres <-
    c("paloma","gallina","pato","ganso","lechuza","halcon",
      "aguila","zorro","perro","lobo","gato","tigre","leon",
      "caballo","cebra","vaca")

    > animal.nombres
    [1] "paloma"  "gallina" "pato"    "ganso"   "lechuza" "halcon"  "aguila"  "zorro"
    [9] "perro"   "lobo"    "gato"    "tigre"   "leon"    "caballo" "cebra"   "vaca"

    animal.carac <-
    c("peque","mediano","grande","bipedo","cuadrupedo","pelo","pezunas",
      "melena","plumas","caza","corre","vuela","nada")

Se asignan nombres a filas y columnas:

.. code-block:: rconsole

    > dimnames(animal) <- list(animal.carac,animal.nombres)

Y por ultimo, echamos un vistazo a una parte de la matriz:

.. code-block:: rconsole

    > animal[1:5,1:10]
            peque mediano grande bipedo cuadrupedo pelo pezuñas melena plumas caza
    paloma      1       0      0      1          0    0       0      0      1    0
    gallina     1       0      0      1          0    0       0      0      1    0
    pato        1       0      0      1          0    0       0      0      1    0
    ganso       1       0      0      1          0    0       0      0      1    0
    lechuza     1       0      0      1          0    0       0      0      1    1

Se puede revisar toda la matriz, y ver que las descripciones son correctas,
aunque algo simples, por supuesto.

Ahora el objetivo en un análisis de cluster sería visualizar de alguna manera
la estructura de los datos, y descubrir qué grupos de animales pueden existir.

Con este fin se calcula la matriz de distancias entre los animales,
descritos por sus características, se realiza el análisis de cluster sobre
los grupos usando la función hclust, y después se visualizan los grupos
utilizando el dendrograma:

.. code-block:: rconsole

   > animal.dist <- dist(animal)
   > animal.hclust <- hclust(animal.dist)
   > plot(animal.hclust)

Como se puede ver al repetir estos comandos, salen dos grandes grupos,
uno con las aves, y otro con los mamiferos. Dentro de las aves,
se verá un grupo formado por las rapaces y otro por las demás,
y dentro de los mamíferos, una distinción entre mamíferos grandes y menos
grandes, distinguiendo los cazadores de los que no lo son. ¿Qué tal queda?

Esta técnica aplicada a datos menos evidentes, puede ayudarnos a descubrir
grupos interesantes en los datos bajo estudio.
