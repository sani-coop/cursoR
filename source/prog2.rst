**********************************
Elementos de Programación con R II
**********************************

A continuación, avanzaremos en las estructuras de repetición, procesos de simulación, herramientas de depuración y
de mejora de rendimiento del código.


Bucles en la línea de comando
=============================

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
==========


Generación de números aleatorios
--------------------------------

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

If :math:"\Phi" es la función de distribución acumulada de una distribución Normal estandar, entonces
``pnorm(q)``=:math:"\Phi(q)" y ``qnorm(p)``= :math:"\Phi^{-1}(p)"




Herramientas de depuración
==========================
debugging


Mejora del rendimiento del código
=================================

profiling


