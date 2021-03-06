Probabilidad y Distribuciones
=============================

Conceptos de probabilidades basados en R
----------------------------------------

Definición de probabilidad
^^^^^^^^^^^^^^^^^^^^^^^^^^

Considerando un enfoque de probabilidad como proporción de ocurrencia de
eventos, entonces es necesario definir un conjunto de eventos posibles.

Sea :math:`\Omega` denominado **espacio muestral**.

Entonces, un **evento** :math:`E` se definiría como cualquier subconjunto de
:math:`\Omega`.

Un evento simple o elemental :math:`\omega` es un elemento de :math:`\Omega`.

La no ocurrencia de ningún evento, se denomina **evento nulo** y se denota por
:math:`\emptyset`.

Si se realiza una serie de experimentos en cada uno de los cuales puede
ocurrir algún evento elemental perteneciente a :math:`\Omega`. Para un gran
número de experimentos se define la probabilidad de :math:`\omega` como el
cociente entre el número de veces que ocurre dicho evento y el número total de
experimentos.

Medida de probabilidad
^^^^^^^^^^^^^^^^^^^^^^

Una, **medida de probabilidad** :math:`P`, es una función sobre un conjunto de
eventos posibles tal que:

1. Para un evento :math:`E \subset \Omega`, :math:`0  \leq P(E) \leq 1`

2. :math:`P(\Omega) = 1`

3. Si :math:`E_1` y :math:`E_2` son eventos mutuamente excluyentes,
   :math:`P(E_1 \cup E_2) = P(E_1) + P(E_2)`.

El punto 3 implica **aditividad finita**

.. math::

    P(\cup_{i=1}^n A_i) = \sum_{i=1}^n P(A_i)

donde los math:`\{A_i\}` son mutuamente exclusivos.

Variable aleatoria
^^^^^^^^^^^^^^^^^^

Una **variable aleatoria** es el resultado numérico de un experimento.

Puede ser:

* *Discreta*: cuando solamente puede tomar valores de un conjunto enumerable.
  math:`P(X = k)`

* *Continua*: cuando puede tomar cualquier valor de la recta real o un
  subconjunto sobre esta. math:`P(X \in A)`

Función de masa de probabilidad
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Una **función de masa de probabilidad** (FMP) evaluada en un valor
corresponde  a la probabilidad que una variable aleatoria tome ese valor.

Un FMP válida debe satisfacer:

1. :math:`p(x) \geq 0`, para todo valor posible de :math:`x`
2. :math:`\sum_{x} p(x) = 1`

La suma se toma sobre todos los posibles valores de :math:`x`:

Un **función de densidad de probabilidad** (FDP), es una función asociada con
una variable aleatoria continua.

 *Las áreas bajo las FDP se corresponden con las probabilidades de esa variable*

Para ser una FDP, una función :math:`f` debe satisfacer:

1. :math:`f(x) \geq 0` para todo :math:`x`
2. El área bajo :math:`f(x)` es uno.

Un ejemplo:

Suponga que la proporción de llamadas de ayuda que se reciben cualquier día
por una línea de ayuda está dada por:

.. math::

    f(x) = \left\{\begin{array}{ll}
        2 x & \mbox{ for } 1 > x > 0 \\
        0                 & \mbox{ otherwise}
    \end{array} \right.

¿Es una densidad matemáticamente valida?

.. math::

    x <- c(-0.5, 0, 1, 1, 1.5); y <- c( 0, 0, 2, 0, 0)
    plot(x, y, lwd = 3, frame = FALSE, type = "l")

¿Cuál es la probabilidad que 75% o menos de las llamadas sean atendidas?

.. math::

    plot(x, y, lwd = 3, frame = FALSE, type = "l")
    polygon(c(0, .75, .75, 0), c(0, 0, 1.5, 0), lwd = 3, col = "lightblue")
    1.5 * .75 / 2
    pbeta(.75, 2, 1)

La **función de distribucion acumulada** (FDA) de una variable aleatoria
:math:`X``está definida como la función:

.. math::

    F(x) = P(X \leq x)

* Esta definición aplica sin importar que :math:`X` sea discreta o continua.

La **función de supervivencia** de una variable aleatoria :math:`X` está
definida como:

..math::
    S(x) = P(X > x)

* Note que :math:`S(x) = 1 - F(x)`
* Para variables aleatorias continuas, la FDP es la derivada de la FDA.

Ejemplo,

¿Cuál es la función de supervivencia y la FDA de la función de densidad
considerada antes?

para :math:`1 \geq x \geq 0`

.. math::

    F(x) = P(X \leq x) = \frac{1}{2} Base \times Altura = \frac{1}{2} (x) \times (2 x) = x^2

    S(x) = 1 - x^2

.. code-block:: r

    pbeta(c(0.4, 0.5, 0.6), 2, 1)

El **cuantil** :math:`alpha^{ésimo}` de una distribución con función de
distribución :math:`F` es el punto :math:`x_\alpha` tal que:

.. math::

    F(x_\alpha) = \alpha

* Un **percentil** es simplemente un cuantil con :math:`\alpha` expresado
  como un porcentaje.
* La **mediana** es el $50^{avo}$ percentile
.
Ejemplo

* Se quiere resolver :math:`0.5 = F(x) = x^2`
* Resulta en la solución:

.. code-block:: r

    sqrt(0.5)

* En consecuencia, alrededor de `r sqrt(0.5)` de las llamadas que se
  responden un día cualquiera es la mediana.
* R puede aproximar cuantiles para las distribuciones mas comunes.

.. code-block:: r

    qbeta(0.5, 2, 1)


Notas

* Cuando se refiere **medidas de la población** se trata de la mediana de
  los datos. La **mediana de la población** es la que se obtiene integrando
  la función de densidad.
* Un modelo de probabilidad conecta los datos a la población en base a
  supuestos.
* La mediana de la que se habla es el **estimando**, la mediana de la muestra
  será el **estimador**

Valores esperados
-----------------

El **valor esperado** o la **media** de una variable aleatoria es el centro
de su distribución.

Para variables aleatorias discretas :math:`X` con FMP :math:`p(X)`,
se define como sigue:

.. math::

    E[X] = \sum_x xp(x).

donde la suma se toma sobre todo los posibles valores de :math:`x`.

:math:`E[X]` representa el centro de masa de una colección de posiciones y
pesos :math:`\{x, p(x)\}`

Ejemplo, encuentre el centro de masa de las barras

.. code-block:: r

    library(UsingR); data(galton)
    par(mfrow=c(1,2))
    hist(galton$child,col="blue",breaks=100)
    hist(galton$parent,col="blue",breaks=100)

Utilizando ``manipulate``

.. code-block:: r

    library(manipulate)
    myHist <- function(mu){
      hist(galton$child,col="blue",breaks=100)
      lines(c(mu, mu), c(0, 150),col="red",lwd=5)
      mse <- mean((galton$child - mu)^2)
      text(63, 150, paste("mu = ", mu))
      text(63, 140, paste("Imbalance = ", round(mse, 2)))
    }
    manipulate(myHist(mu), mu = slider(62, 74, step = 0.5))

El centro de masa es la media empírica

.. code-block:: r

   hist(galton$child, col="blue", breaks=100)
   meanChild <- mean(galton$child)
   lines(rep(meanChild,100), seq(0,150,length=100), col="red", lwd=5)

Ejemplo, suponga que se lanza una moneda y :math:`X` se denota :math:`0` o
:math:`1 que corresponden a cara y sello, respectivamente.

¿Cuál es el valor esperado de :math:`X`?

.. math::

   E[X] = .5 \times 0 + .5 \times 1 = .5

Note, que si se piensa de forma geométrica, la respuesta es obvia; si se
colocan dos pesos iguales en 0 y 1, el centro de masa será :math:`0.5`.

.. code-block:: r

    barplot(height = c(.5, .5), names = c(0, 1),
            border = "black",
            col = "lightblue",
            space = .75)

Ejemplo, suponga que se lanza un dado y :math:`X` es el número que queda boca
arriba.

¿Cuál es el valor esperado de :math:`X`?

.. math::

   E[X] = 1 \times \frac{1}{6} + 2 \times \frac{1}{6} +
   3 \times \frac{1}{6} + 4 \times \frac{1}{6} +
   5 \times \frac{1}{6} + 6 \times \frac{1}{6} = 3.5

De nuevo, el argumento geométrico hace que la respuesta sea obvia sin cálculos.

Variables aleatoria continuas
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Para una variable aleatoria continua, :math:`X` con densidad :math:`f`,
el valor esperado se define como:

.. math::

    E[X] = \mbox{the area under the function}~~~ t f(t)

Esta definición está derivada de la definición del centro de masa para un
cuerpo continuo.


Ejemplo, considere una densidad donde :math:`f(x) = 1` para `x` entre cero y
uno.

¿Es una densidad válida?

Suponga que :math:`X` sigue esta densidad; ¿Cuál es el valor esperado?

.. code-block:: r

    par(mfrow = c(1, 2))
    plot(c(-0.25, 0, 0, 1, 1, 1.25), c(0, 0, 1, 1, 0, 0),
         type = "l", lwd = 3, frame = FALSE, xlab="", ylab = "")
    title('f(t)')
    plot(c(-0.25, 0, 1, 1, 1.25), c(0, 0, 1, 0, 0),
         type = "l", lwd = 3, frame = FALSE, xlab="", ylab = "")
    title('t f(t)')

Reglas sobre los valores esperados
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

El valor esperado es una operador lineal.

Si :math:`a` y :math:`b` no son aleatorias y :math:`X` y :math:`Y` son dos
variables aleatorias entonces:

* :math:`E[aX + b] = a E[X] + b`
* :math:`E[X + Y] = E[X] + E[Y]`

Ejemplo, si lanza una moneda :math:`X` y simula una variable aleatoria
uniforme :math:`Y`, ¿Cuál es el valor esperado de su suma?

.. math::

    E[X + Y] = E[X] + E[Y] = .5 + .5 = 1

Otro ejemplo, si se lanza un dado dos veces. ¿Cuál es el valor esperado del
promedio?

Sean :math:`X_1` y :math:`X_2` los resultados de los dos lanzamientos.

.. math::

    E[(X_1 + X_2) / 2] = \frac{1}{2}(E[X_1] + E[X_2])
    = \frac{1}{2}(3.5 + 3.5) = 3.5

Ejemplo,

1. Sea :math:`X_i` para :math:`i=1,\ldots,n` sea una colección de variables
aleatorias, cada una de una distribución con media :math:`\mu`.
2. Calcule el valor esperado del promedio muestral de :math:`X_i`.

.. math::

  \begin{eqnarray*}
    E\left[ \frac{1}{n}\sum_{i=1}^n X_i\right]
    & = & \frac{1}{n} E\left[\sum_{i=1}^n X_i\right] \\
    & = & \frac{1}{n} \sum_{i=1}^n E\left[X_i\right] \\
    & = & \frac{1}{n} \sum_{i=1}^n \mu =  \mu.
  \end{eqnarray*}

.. important::

   * En consecuencia, el valor esperado de la **media muestral** es la media
     de la población que se está intentando estimar.
   * Cuando el valor esperado de un estimador es lo que trata de estimar,
     se dice que el estimador es **no sesgado**.

La varianza
^^^^^^^^^^^

* La varianza de una variable aleatoria es una medida de *dispersión*
* Si :math:`X` es una variable aleatoria con media :math:`\mu`,
  la varianza de :math:`X` está definida como:

.. math::

   Var(X) = E[(X - \mu)^2]

La distancia esperada (al cuadrado) alrededor de la media.

* La densidades con una mayor varianza están mas dispersas que las densidades
  con una menor varianza.


Forma computacional conveniente
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. math::

   Var(X) = E[X^2] - E[X]^2

* Si :math:`a` es una constante entonces :math:`Var(aX) = a^2 Var(X)`
* La raíz cuadrada de la varianza es denominada **desviación estándar**
* La desviación estándar tiene las mismas unidades que :math:`X`


Ejemplo, ¿Cuál es la varianza muestral del resultado de lanzar una moneda?

* :math:`E[X] = 3.5`
* :math:`E[X^2] = 1 ^ 2 \times \frac{1}{6} + 2 ^ 2 \times \frac{1}{6} + 3 ^ 2 \times \frac{1}{6} + 4 ^ 2 \times \frac{1}{6} + 5 ^ 2 \times \frac{1}{6} + 6 ^ 2 \times \frac{1}{6} = 15.17`
* :math:`Var(X) = E[X^2] - E[X]^2 \approx 2.92`

Ejemplo, ¿Cuál es la varianza muestral del resultado del lanzamiento de una
moneda con con probabilidad de obtener cara igual a :math:`p`?

* :math:`E[X] = 0 \times (1 - p) + 1 \times p = p`
* :math:`E[X^2] = E[X] = p`
* :math:`Var(X) = E[X^2] - E[X]^2 = p - p^2 = p(1 - p)`

Interpretando las varianzas

* La desigualdad de Chebyshev's es útil para interpretar las varianzas
* Esta desigualdad establece que:

.. math::

   P(|X - \mu| \geq k\sigma) \leq \frac{1}{k^2}

Por ejemplo, la probabilidad que una variable aleatoria caiga más allá de
:math:`k` desviaciones estándar desde la media es menor que :math:`1/k^2`

.. math::

   \begin{eqnarray*}
       2\sigma & \rightarrow & 25\% \\
       3\sigma & \rightarrow & 11\% \\
       4\sigma & \rightarrow &  6\%
   \end{eqnarray*}

* Note que esto es solamente una cota, la probabilidad verdadera puede ser un
  poco menor.

Ejemplo:

* Se dice que los CIs están distribuidos con una media de :math:`100` y una
  desviación estándar de :math:`15`
* ¿Cuál es la probabilidad que una persona seleccionada aleatoriamente tenga
  un CI superior a :math:`160` o inferior a :math:`40`?
* Por lo tanto se quiere conocer la probabilidad que una persona tenga mas de
  :math:`4` desviaciones estándar desde la media.
* La desigualdad de Chebyshev sugiere que no será superior a 6\%
* Con frecuencia se cita que las distribuciones de los CIs tienen forma de
  campana, en este caso este límite es muy conservador.
* La probabilidad que obtener un valor aleatorio más allá de :math:`4`
  desviaciones estándar desde la media está por el orden de :math:`10^{-5}`
  (una milésima de uno por ciento)

Distribución de Variables aleatorias
------------------------------------

Simulación de probabilidad
--------------------------

Errores de decisión, significación y confianza
----------------------------------------------
