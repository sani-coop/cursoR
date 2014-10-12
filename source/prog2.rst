**********************************
Elementos de Programación con R II
**********************************

A continuación, avanzaremos en las estructuras de repetición, procesos de simulación, herramientas de depuración y
de mejora de rendimiento del código.


Bucles en la línea de comando
===============================

Hacer bucles ``for``, ``while`` es muy útil cuando se programa, pero no resulta especialmente sencillo cuando se trabaja
interactivamente con la línea de comando. Es por ello que hay algunas funciones que implementan bucles para hacer el
trabajo más sencillo.

- ``lapply``: Bucle a lo largo de una lista que evalúa una función en cada elemento

- ``sapply``: Funciona del mismo modo que ``lapply`` pero trata de simplificar el resultado

- ``apply``: Aplica una función sobre los márgenes de un arreglo

- ``tapply``: Aplica una función sobre un subconjunto de un vector

- ``mapply``: Una versión multivariante de ``lapply``


Existe una función auxiliar llamada ``split`` que resulta también útil particularmente de modo conjunto con ``lapply``


``laply``
^^^^^^^

``lapply`` toma tres argumentos: (1) una lista d ``X``; (2) una función (o el nombre de una función) FUN; (3) otros
argumentos a través de su ... argumento. Si ``X`` no es una lista, será forzado a serlo utilizando as.list.

.. code-block:: r

    lapply

    ## function (X, FUN, ...)
    ## {
    ##    FUN <- match.fun(FUN)
    ##    if (!is.vector(X) || is.object(X))
    ##        X <- as.list(X)
    ##    .Internal(lapply(X, FUN))
    ## }
    ## <bytecode: 0x7ff7a1951c00>
    ## <environment: namespace:base>

El bucle actual se realiza internamente en código C

``lapply`` siempre devuelve una lista, dependiendo de la clase de la salida.

.. code-block:: r

    x <- list(a = 1:5, b = rnorm(10))
    lapply(x, mean)

    ## $a
    ## [1] 3
    ##
    ## $b
    ## [1] 0.4671


.. code-block:: r

    x <- list(a = 1:4, b = rnorm(10), c = rnorm(20, 1), d = rnorm(100, 5))
    lapply(x, mean)

    ## $a
    ## [1] 2.5
    ##
    ## $b
    ## [1] 0.5261
    ##
    ## $c
    ## [1] 1.421
    ##
    ## $d
    ## [1] 4.927


.. code-block:: r

    > x <- 1:4
    > lapply(x, runif)
    [[1]]
    [1] 0.2675082

    [[2]]
    [1] 0.2186453 0.5167968

    [[3]]
    [1] 0.2689506 0.1811683 0.5185761

    [[4]]
    [1] 0.5627829 0.1291569 0.2563676 0.7179353


.. code-block:: r

    > x <- 1:4
    > lapply(x, runif, min = 0, max = 10)
    [[1]]
    [1] 3.302142

    [[2]]
    [1] 6.848960 7.195282

    [[3]]
    [1] 3.5031416 0.8465707 9.7421014

    [[4]]
    [1] 1.195114 3.594027 2.930794 2.766946


``lapply`` y sus amigas tienen un uso muy importante en funciones *anónimas*.

.. code-block:: r

    > x <- list(a = matrix(1:4, 2, 2), b = matrix(1:6, 3, 2))
    > x
    $a

        [,1] [,2]
    [1,]  1   3
    [2,]  2   4

    $b
        [,1] [,2]
    [1,]  1   4
    [2,]  2   5
    [3,]  3   6


Una función anónima para extraer la primera columna de cada matriz

.. code-block:: r

    > lapply(x, function(elt) elt[,1])
    $a
    [1] 1 2

    $b
    [1] 1 2 3


``sapply``
^^^^^^^^
``sapply`` trata de simplificar el resultado de ``lapply`` si es posible.

- Si el resultado es una lista donde cada elemento tiene longitud 1, entonces se devuelve un vector

- Si el resultado es una lita donde cada elemento es un vector de la misma longitus (> 1), se devuelve una matriz

- Si no se puede encontrar, se devuelve una lista.


.. code-block:: r

    > x <- list(a = 1:4, b = rnorm(10), c = rnorm(20, 1), d = rnorm(100, 5))
    > lapply(x, mean)
    $a
    [1] 2.5

    $b
    [1] 0.06082667

    $c
    [1] 1.467083

    $d
    [1] 5.074749


.. code-block:: r

    > sapply(x, mean)
            a           b       c       d
    2.50000000 0.06082667 1.46708277 5.07474950

    > mean(x)
    [1] NA
    Warning message:
    In mean.default(x) : argument is not numeric or logical: returning NA


Simulación
============


Generación de números aleatorios
^^^^

Las funciones para distribuciones aleatorias en R son:

- ``rnorm``: genera variables aleatorias de tipo Normal con una media y desviación estándar dadas.

- ``dnorm``: evalúa la densidad de probabilidad de una Normal (con una media y desviación estándar dadas) en un punto (o
vector de puntos)

- ``pnorm``: evalúa la función de la distribución acumulada para una distribución Normal

- ``rpois``: genera variables aleatorias Poisson con un índice dado.


Las funciones de distribución de probabilidad, generalmente tienen cuatro funciones asociadas a ellas. Estas son:

- ``d`` para densidad

- ``r`` para generación de número aleatorio

- ``p`` para distribución acumulada

- ``q`` para función cuantil.


El trabajo con distribuciones tipo Normal requiere el uso de estas cuatro funciones

.. code-block:: r

    dnorm(x, mean = 0, sd = 1, log = FALSE)
    pnorm(q, mean = 0, sd = 1, lower.tail = TRUE, log.p = FALSE)
    qnorm(p, mean = 0, sd = 1, lower.tail = TRUE, log.p = FALSE)
    rnorm(n, mean = o, sd = 1)

If :math:`\Phi` es la función de distribución acumulada de una distribución Normal estandar, entonces
``pnorm(q)`` = :math:`\Phi(q)` y ``qnorm(p)`` = :math:`\Phi^{-1}(p)`

.. code-block:: r

    > x <- rnorm(10)
    > x
      [1] 1.38380206 0.48772671 0.53403109 0.66721944
      [5] 0.01585029 0.37945986 1.31096736 0.55330472
      [9] 1.22090852 0.45236742
    > x <- rnorm(10, 20, 2)
    > x
      [1] 23.38812 20.16846 21.87999 20.73813 19.59020
      [6] 18.73439 18.31721 22.51748 20.36966 21.04371
    > summary(x)
       Min.   1st Qu. Median  Mean    3rd Qu.  Max.
      18.32   19.73   20.55   20.67   21.67    23.39


Configurar semillas de números aleatorios con ``set.seed`` asegura su replicación

.. code-block:: r

    > set.seed(1)
    > rnorm(5)
    [1] -0.6264538  0.1836433  -0.8356286  1.5952808
    [5]  0.3295078
    > rnorm(5)
    [1] -0.8204684  0.4874291   0.7383247  0.5757814
    [5] -0.3053884
    > set.seed(1)
    > rnorm(5)
    [1] -0.6264538  0.1836433  -0.8356286  1.5952808
    [5]  0.3295078


¡Recuerde siempre establecer la semilla del número aleatorio cuando ejecuta una simulación!


Para generar datos Pisson

.. code-block:: r

    > rpois(10, 1)
     [1] 3 1 0 1 0 0 1 0 1 1
    > rpois(10, 2)
    [1] 6 2 2 1 3 2 2 1 1 2
    > rpois(10, 20)
    [1] 20 11 21 20 20 21 17 15 24 20


    > ppois(2, 2)  ## Cumulative distribution
    [1] 0.6766764  ## Pr(x <= 2)
    > ppois(4, 2)
    [1] 0.947347   ## Pr(x <= 4)
    > ppois(6, 2)
    [1] 0.9954662  ## Pr(x <= 6)


Generando números aleatorios a partir de un modelo lineal
-------------------------------------------------------------

Supongamos que queremos hacer una simulación a partir del siguiente modelo lineal


    :math:`y = \beta_0 + \beta_1 x + \epsilon`


donde :math:`\epsilon \sim N(0,2^2)`. Se asume :math:`x ~ N(0,1^2), \beta_0 = 0.5` y :math:`\beta_1 = 2`

.. code-block:: r

    > set.seed(20)
    > x <- rnorm(100)
    > e <- rnorm(100, 0, 2)
    > y <- 0.5 + 2 * x + e
    > summary(y)
       Min.     1st Qu.   Median
     -6.4080   -1.5400    0.6789  0.6893  2.9300   6.5050
    > plot(x, y)


¿Qué pasa si ``x`` es binaria?

.. code-block:: r

    > set.seed(10)
    > x <- rbinom(100, 1, 0.5)
    > e <- rnorm(100, 0, 2)
    > y <- 0.5 + 2 * x + e
    > summary(y)
       Min.   1st Qu.   Median
    -3.4940  -0.1409    1.5770    1.4320   2.8400   6.9410
    > plot(x, y)


Generando números aleatorios a partir de un Modelo Lineal Generalizado
-------------------------------------------------------------------------

Supongamos que queremos hacer una simulación a partir de un modelo Poisson donde

:math:`Y \sim Poisson(\mu)`

:math:`log \mu = \beta_0 + \beta_1x`

y :math:`\beta_0 = 0.5` y :math:`\beta_1 = 0.3`

En ese caso, se requiere el uso de la función ``rpois``

.. code-block:: r

    > set.seed(1)
    > x <- rnorm(100)
    > log.mu <- 0.5 + 0.3 * x
    > y <- rpois(100, exp(log.mu))
    > summary(y)
       Min.  1st Qu. Median    Mean  3rd Qu.  Max.
       0.00    1.00   1.00    1.55    2.00   6.00
    > plot(x, y)

Muestreo Aleatorio
--------------------

La función ``sample`` hace un gráfico aleatorio a partir de un conjunto específico de objetos (escalares) permitiéndole
hacer un muestreo a partir de distribuciones arbitrarias.

.. code-block:: r

    > set.seed(1)
    > sample(1:10, 4)
    [1] 3 4 5 7
    > sample(1:10, 4)
    [1] 3 9 8 5
    > sample(letters, 5)
    [1] "q" "b" "e" "x" "p"
    > sample(1:10)  ## permutation
     [1] 4 710 6 9 2 8 3 1 5
    > sample(1:10)
     [1] 2 3 4 1 9 5 10 8 6 7
    > sample(1:10, replace = TRUE) ## Sample w/replacement
     [1] 2 9 7 8 2 8 5 9 7 8

Resumen
----------

- Para realizar gráficos de muestras para distribuciones específicas de probabilidad pueden utilizarse las funciones ``r*``

- Las distribuciones estandar son: Normal, Poisson, Binomial, Exponencial, Gamma, etc.

- La función ``sample`` puede utilizarse para graficar muestras aleatorias a partir de vectores arbitrarios

- Para la replicación del modelo, resulta muy importante configurar el generador de números aleatorios a través de
  ``set.seed``


Herramientas de depuración
==========================
debugging


Mejora del rendimiento del código
=================================

profiling


