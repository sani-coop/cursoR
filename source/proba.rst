Probabilidad y Distribuciones
=============================


Conceptos de probabilidades basados en R
----------------------------------------

Definición de probabilidad
--------------------------

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

Variable aleatoria
^^^^^^^^^^^^^^^^^^

Una **variable aleatoria** es el resultado numérico de un experimento.

Puede ser:

- Discreta: cuando solamente puede tomar valores de un conjunto enumerable.
- 
- Continua: cuando puede tomar cualquier valor de la recta real o un
  subconjunto sobre esta.

Función de masa de probabilidad
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Una **función de masa de probabilidad** (FMP) evaluada en un valor corresponde a la
probabilidad que una variable aleatoria tome ese valor.

Un FMP válida debe satisfacer:

1. :math:`p(x) \geq 0`, para todo valor posible de :math:`x`
2. :math:`\sum_x p(x) = 1`

Probabilidad condicional
------------------------

Distribución de Variables aleatorias
------------------------------------

Simulación de probabilidad
--------------------------

Errores de decisión, significación y confianza
----------------------------------------------
