Análisis Exploratorio de Datos
==============================

FALTA UNA PEQUEÑA INTRODUCCION AQUI

Representaciones visuales de los datos
----------------------------------------

El análisis exploratorio de datos a través de gráficos cumple con los siguientes principios:

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


Sin embargo, los **gráficos exploratorios** se usan sólo para **comunicar resultados**.


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

- La Agencia de Protección Medioambiental de Estados Unidos (EPA por sus siglas en inglés) establece estándares nacionales
  ambientales de la calidad del aire para la contaminación del aire exterior

    - `Estándares Nacionales Ambientales de Calidad del Aire <http://www.epa.gov/air/criteria.html>`_

- Para contaminción de partícula fina (PM2.5), la "media anual, premediada durante 3 años" no puede exceder :math:`12\mu g/m^3`

- Los datos de PM2.5 diarios están disponibles desde la página web del EPA

    - `Sistema de Calidad del Aire de EPA <http://www.epa.gov/ttn/airs/airsaqs/detaildata/downloadaqsdata.htm>`_



- **Pregunta**: ¿Hay lugares en Estados Unidos que superen ese estandar nacional de contaminación de partícula fina?


Datos
******

Promedio anual PM2.5 acumulado desde el 2008 al 2010

.. code-block:: r

    pollution <- read.csv("data/avgpm25.csv", colClasses = c("numeric", "character",
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

    barplot(table(pollution$region), col = "wheat", main = "Number of Counties in Each Region")


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

- Permiten resumir los datos (a menudo de forma gráfica) y destacar cualquier característica común

- Explora preguntas básicas e hipótesis (y quizás las descarte)

- Sugiere estrategias de modelado para los siguientes pasos


lattice
^^^^^^^^



ggplot2
^^^^^^^^

¿Qué es ``ggplot2``?

- Es una implementación de *Grammar of Graphics* de Leland Wilkinson

- Fue escrita por Hadley Wickham (mientras era estudiante en la Universidad de Iowa)

- Es un sistema gráfico de "terceros" para R (junto a ``base`` y ``lattice``)

- Está disponible desde CRAN a través de ``install.packages()`` (http://ggplot2.org)

- *Grammar of graphics* representa una abstracción de ideas/objetos

- Es pensar los gráficos en términos de "verbo", "sustantivo", "adjetivo"

- Posibilita una "teoría" de gráficos sobre la cual construir nuevos gráficos y objetos gráficos

Elementos Básicos
******************

``qplot()``
^^^^^^^^^^^^

- Trabaja muy parecido a la función ``plot`` en sistemas gráficos base

- Busca datos en un conjunto de datos, tal como ``lattice``, o el entorno inmediato

- Los gráficos se hacen con *estética* (tamaño, color, forma)


Creación de gráficos análiticos
-------------------------------

Resúmenes exploratorios de datos
--------------------------------

Visualización multidimensional exploratoria
-------------------------------------------

