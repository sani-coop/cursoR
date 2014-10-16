Investigación Reproducible
==========================

Estructura y organización de un análisis de datos
-------------------------------------------------

Archivos de un análisis de datos

* Datos

  * Datos "crudos"
  * Datos procesados

* Figuras

  * Figuras exploratorias
  * Figuras definitivas

* Código R

  * Scripts crudo / no utilizados
  * Scripts definitivos
  * Archivos R Markdown

* Texto

  * Archivos LEÁME (README)
  * Texto de análisis / reporte

Datos crudos

* Deben estar almacenados en la carpeta de análisis
* Si se acceden desde la web, deben incluir el URL, la descripción y las
  fechas de acceso en el LEÁME

Datos procesados

* Los datos procesados deben tener nombres tales que sea fácil saber con cual
  script fueron generados.
* En el LEÁME debe indicarse la relación entre el script de procesamiento y
  los datos procesados.
* Los datos procesados deben ser `ordenados`_

.. _ordenados: http://vita.had.co.nz/papers/tidy-data.pdf


Figuras exploratorias

* Las figuras hechas durante los análisis, que no son necesariamente parte
  del reporte final.
* No necesitan ser "bonitas".

Figuras finales

* Usualmente un pequeño conjunto de las figuras originales
* Ejes/colores configurados para hacer la figura clara
* Posiblemente múltiples paneles

Scripts crudos

* Pueden estar menos comentados (¡pero los comentarios le ayudarán!)
* Pueden haber múltiples versiones
* Pueden incluir análisis que serán descartados

Scripts definitivos

* Claramente comentados

  * Pequeños comentarios con generosidad - qué, cuándo, porqué, cómo
  * Los bloques de comentarios mas grandes para secciones completas

* Incluyen detalles de procesamiento
* Solamente análisis que aparecen en el escrito final

Archivos R markdown

* Los archivos `R markdown`_ pueden usarse para generar reportes reproducibles
* Integra texto y código R
* Son fáciles de crear en RStudio_

.. _R markdown: http://www.rstudio.com/ide/docs/authoring/using_markdown
.. _RStudio: http://www.rstudio.com/

Archivos LEÁME

* No es necesario si utiliza R markdown
* Debe incluir instrucciones paso a paso para el análisis
* Aquí hay un ejemplo https://github.com/jtleek/swfdr/blob/master/README

Texto del documento

* Debe incluir: título, introducción  (motivación), métodos (estadísticos que
  utilizó), resultados (incluyendo medidas de incertidumbre),
  y conclusiones (incluyendo problemas potenciales).
* Debe contar una historia
* *No debe incluir todos los análisis que realizó*
* Deben incluirse las referencias para los métodos estadísticos

Recursos adicionales

* Información sobre un estudio no reproducible que ocasionó que pacientes de
  cáncer no fueran atendidos adecuadamente `The Duke Saga Starter Set`_

* `Investigación reproducible y bioestadística`_

* `Buenas prácticas para el manejo de proyectos de análisis estadístico`_

* `Plantilla de proyecto`_ un conjunto de archivos pre-organizados para
  analisis de datos.


.. _The Duke Saga Starter Set: http://simplystatistics.org/2012/02/27/the-duke-saga-starter-set/
.. _Investigación reproducible y bioestadística: http://biostatistics.oxfordjournals.org/content/10/3/405.full
.. _Buenas prácticas para el manejo de proyectos de análisis estadístico: http://www.r-statistics.com/2010/09/managing-a-statistical-analysis-project-guidelines-and-best-practices/
.. _Plantilla de proyecto: http://projecttemplate.net/

Generación de documentos utilizando R markdown
----------------------------------------------

¿Qué es Markdown?

* Creado por `John Gruber`_ y Aaron Swartz.
* Una versión simplificada de los lenguajes de "marcado"
* Permite enfocarse en escribir en lugar de formatear
* Elementos de formato simples/mínimos
* Se pueden convertir fácilmente a HTML (y otros formatos) utilizando las
  herramientas existentes.
* Complete information is available at http://daringfireball.net/projects/markdown/
* Some background information at http://daringfireball.net/2004/03/dive_into_markdown

.. _John Gruber: http://daringfireball.net

¿Qué es R Markdown?

* R markdown es la integración de código R con markdown
* Permite crear documentos que contienen código R "vivo"
* El código R es evaluado como parte del procesamiento de markdown
* Los resultado del código R se insertan en el documento markdown
* Un herramienta fundamental de la **programación estadística literal**

¿Qué es R Markdown? (cont.)

* R markdown puede convertirse en markdown estándar utilizando el paquete
  knitr_.
* Markdown puede convertirse a HTML utilizando el paquete markdown_ de R.
* Cualquier editor básico de txto puede utilizarse para crear un dcumento
  markdown; no se requieren heramientas especiales de edición.
* El flujo de trabajo R markdown --> markdown --> HTML se puede gestionar con
 facilidad usando `RStudio`_.
* Se pueden generar presentaciones con R markdown usando el paquete
  slidify_

.. _knitr: http://yihui.name/knitr/
.. _markdown: https://github.com/rstudio/markdown
.. _slidify: http://slidify.org

Compilación de estos documentos utilizando knitr
------------------------------------------------

Programación estadística literaria con knitr
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Problemas, problemas

* Los autores deben realizar un esfuerzo considerable para poner datos /
  resultados en la web
* Los lectores deben descargar datos / resultados individualmente y reconstruir
  los datos que van con secciones que código, etc
* Autores / lectores deben interactuar de forma manual con los sitios web
* No existe un documento único para integrar el análisis de datos con
  representaciones textuales; es decir, datos, código,
  y texto no están vinculados

Programación estadística literaria

* La idea original proviene de Don Knuth
* Un artículo es un flujo de texto y código
* El código del análisis se divide en texto y trozos de código "chunks"
* El código de la presentación da formato a los resultados (tablas, figuras,
  etc)
* El texto del artículo explica lo que está sucediendo
* Los programas literarios se *tejen* para producir documentos legibles y se
  *enredan* para producir documentos de lectura mecánica.


* La programación literaria es un concepto general. se necesita:

   * Un lenguaje de documentación
   * Un lenguaje de programación

* El sistema Sweave original desarrollado por Friedrich Leisch utiliza LaTeX y R
* knitr soporta varios lenguajes de documentación

¿Cómo hacer que mi trabajo sea reproducible?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Decidirse a hacerlo (lo ideal desde el principio)
* Llevar un registro de las cosas, tal vez con un sistema de control de
  versiones para rastrear instantáneas / cambios
* Utilice de software cuyo funcionamiento se puede codificar
* No guarde la salida
* Guarde los datos en formatos no propietarios

Beneficios la literate Programming

* Texto y código, todo en un solo lugar, siguiendo un orden lógico
* Los datos, resultados actualizados automáticamente para reflejar los cambios
  externos
* Código es "vivo"-automático - "prueba de regresión" mientras se construye el
  documento

Debilidades

* El texto y código en un solo lugar; puede hacer que los documentos sean
  difíciles de leer, sobre todo si hay una gran cantidad de código.
* Puede ralentizar sustancialmente el procesamiento de documentos (aunque hay
  herramientas para resolverlo)

¿Qué es knitr?

* Un paquete R escrito por Yihui Xie (mientras que él era un estudiante
  de postgrado en Iowa State)
* Disponible en CRAN
* Soporta RMarkdown, LaTeX y HTML como lenguajes de documentación
* Puede exportar a PDF, HTML
* Integrada en RStudio para su conveniencia

Requisitos

* Una versión reciente de R
* Un editor de texto (el que viene con RStudio está bien)
* Algunos paquetes de apoyo también disponibles en CRAN
* Algún conocimiento de Markdown, LaTeX o HTML
* Usaremos Markdown aquí

¿Para que es bueno knitr?

* Manuales
* Documentos técnicos de longitud media/corta
* Tutoriales
* Informes (En especial si se generan periódicamente)
* Preprocesamiento de datos documentos/resúmenes

¿Para que NO es bueno?

* Artículos de investigación muy largos
* Cálculos complejos que consumen mucho tiempo
* Documentos que requieren un formato preciso

Mi primer documento knitr
^^^^^^^^^^^^^^^^^^^^^^^^^

Nuevo archivo -> R markdown
Dentro del archivo insertar "trozos de R" (R chunks)
Para procesar hacer clic en "Knit HTML"

Se podría hacer lo anterior por línea de comandos

.. code-block:: r

    library(knitr)
    setwd(<working directory>)
    knit2html(“document.Rmd”)
    browseURL(“document.html”)

Salida HTML

Se puede observar la conversión a HTML del texto a Markdown
Y el código y el resultado del código R convertido.
knitr produce Markdown a partir de los trozos de R

Notas

* knitr llenará un nuevo documento con el texto de relleno; elimínelo
* Los trozos de código empiezan con::

   ```{r} y terminan con```


* Todo el código R va en entre estos marcadores
* Los trozos de código pueden tener nombres, lo cual es útil cuando empezamos a
  hacer gráficos::

   ```{r primer trozo}
   ## El código R va aquí
   ```

Por defecto, el código en un trozo de código se reproduce, al igual que los
resultados de la computación (si hay resultados para imprimir)

Flujo de procesamiento con el documento en knitr

* Usted escribe el documento RMarkdown (.Rmd)
* knitr produce un documento Markdown (.md)
* knitr convierte el documento Markdown a HTML (por defecto)
* .RMD -> .md -> Html
* No debe modificar (o guardar) los documentos .md o html hasta que haya
  terminado

En Resumen

* Programación estadística literaria puede ser una forma útil para poner texto,
  código, datos y salidas en un solo documento
* knitr es una poderosa herramienta para la integración de código y de texto
  en un formato de documento sencillo

Investigación Reproducible
--------------------------

SI: Comience con buena ciencia
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Basura entra, basura sale
* Una pregunta coherente y enfocada simplifica muchos los problemas
* Trabajar con buenos colaboradores refuerza las buenas prácticas
* Algo que es interesante para usted será (esperemos) motivar buenos
  hábitos

NO: haga las cosas a mano
^^^^^^^^^^^^^^^^^^^^^^^^^

* Editar hojas de cálculo de datos para "limpiarlas"

   * Eliminación de los valores atípicos
   * Control de calidad
   * Validar

* Edición de tablas o figuras (por ejemplo: redondeo, formato)
* Descarga de datos de un sitio web (hacer clic en enlaces en un navegador web)
* Mover datos en su equipo; separar / reformatear archivos datos
* "Sólo vamos a hacer esto una vez ...."

Las cosas hechas a mano deben estar documentadas con precisión (esto es más
difícil de lo que parece)

NO: Point And Click
^^^^^^^^^^^^^^^^^^^

* Muchos paquetes de análisis estadísticos/procesamiento de datos tienen
  interfaces gráficas de usuario (GUI)

* Las GUIs son convenientes/intuitivas, pero las acciones que se realizan con
  una interfaz gráfica de usuario puede ser difícil para que otros reproduzcan

* Algunos GUIs producen un archivo de registro o script que incluye comandos
  equivalentes; se pueden guardar para revisarse posteriormente

* En general, tenga cuidado con el software de análisis de datos que es altamente
   *interactivo*; facilidad de uso a veces puede conducir a análisis
   no-reproducible

* Otro software interactivo, como editores de texto, por lo general están bien


SI: Enseñar a la computadora
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Si algo hay que hacer como parte de su análisis/investigación, trate de
  enseñar a su equipo a hacerlo (incluso si sólo tiene que hacerlo una vez)

* Con el fin de dar instrucciones a su computadora, es necesario anotar
  exactamente lo que quiere decir que hacer y cómo se debe hacer

* Enseñar a un equipo casi garantiza que sea replicable "reproducible"

Por ejemplo, se puede hacer a mano:

  1. Ir al repositorio de UCI Machine Learning en
  http://archive.ics.uci.edu/ml/

  2. Descargar el `Conjunto de datos de intercambio de bicicletas`_
  haciendo clic en el enlace a la carpeta de datos, a continuación, hacer clic
  en el enlace al archivo zip del conjunto de datos, y elegir "Guardar enlace
  como ... " y después guardarla en una carpeta de su computadora

.. _Conjunto de datos de intercambio de bicicletas:http://archive.ics.uci.edu/ml/datasets/Bike+Sharing+Dataset

O puede enseñar a su computadora a hacer lo mismo con el código de R:

.. code-block:: r

    download.file("http://archive.ics.uci.edu/ml/machine-learning-databases/00275/
                   Bike-Sharing-Dataset.zip", "ProjectData/Bike-Sharing-Dataset.zip")

Note que:

* Se especifica la URL completa del archivo de base de datos (sin hacer clic a
  través de un serie de enlaces)

* Se especifica el nombre del archivo guardado en el equipo local

* Se escribe el directorio en el que se guardó el archivo especificado
  ("ProjectData")

* Código siempre se puede ejecutar en R (siempre y cuando el enlace esté
  disponible)

SI: Utilice un sistema de control de versiones
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Haga las cosas más despacio

* Añadir los cambios en pequeños partes (no sólo realice commits masivos)

* Seguimiento de pasos; permite volver a versiones antiguas

* Software como GitHub / BitBucket / SourceForge le facilitará publicar los
  resultados

SI: Lleve registro de su entorno de trabajo
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Si trabaja en un proyecto complejo que involucra muchas herramientas /
  conjuntos de datos, el software y el entorno computacional informática
  puede ser fundamental para la reproducción de sus análisis

* **Arquitectura computacional**: CPU (Intel, AMD, ARM), GPUs,

* **Sistema operativo**: Windows, Mac OS, Linux / Unix

* **Cadena de herramientas**: Compiladores, interpretes, línea de comandos,
  lenguajes de programación (C, Perl, Python, etc.), bases de datos,
  software de análisis de datos

* **Software de soporte / Infraestructura**: Bibliotecas, paquetes de R,
   dependencias

* **Dependencias externas**: sitios web, repositorios de datos,
   bases de datos remotas, repositorios de software

* **Números de versión**: Lo ideal, para todo (si está disponible)


---

## DO: Keep Track of Your Software Environment

.. code-block:: rconsole

    > sessionInfo()
    R version 3.1.1 (2014-07-10)
    Platform: x86_64-pc-linux-gnu (64-bit)

    locale:
     [1] LC_CTYPE=es_VE.UTF-8       LC_NUMERIC=C
     [3] LC_TIME=es_VE.UTF-8        LC_COLLATE=es_VE.UTF-8
     [5] LC_MONETARY=es_VE.UTF-8    LC_MESSAGES=es_VE.UTF-8
     [7] LC_PAPER=es_VE.UTF-8       LC_NAME=C
     [9] LC_ADDRESS=C               LC_TELEPHONE=C
    [11] LC_MEASUREMENT=es_VE.UTF-8 LC_IDENTIFICATION=C

    attached base packages:
    [1] stats     graphics  grDevices utils     datasets
    [6] methods   base

    other attached packages:
    [1] stringr_0.6.2   openxlsx_2.0.15

    loaded via a namespace (and not attached):
    [1] Rcpp_0.11.3 tools_3.1.1

NO: Guarde la salida
^^^^^^^^^^^^^^^^^^^^

* Evite guardar la salida de análisis de datos (tablas, figuras, resúmenes,
  datos procesados​​, etc), excepto tal vez temporalmente con fines de
  eficiencia.

* Si un archivo de salida "extraviado" no se puede conectar fácilmente con los
  medios con los que fue creado, entonces no es reproducible.

* Guarde código + datos que generaron la salida, en lugar de la misma salida

* Los archivos intermedios están bien siempre y cuando exista una documentación
  clara de cómo fueron creados

SI: Establezca su semilla
^^^^^^^^^^^^^^^^^^^^^^^^^

* Los generadores de números aleatorios generan números pseudo-aleatorios
  basados en un semilla inicial (normalmente un número o conjunto de números)

   * En R se puede utilizar la función ``set.seed ()`` para establecer la
     semilla y para especificar el generador de números aleatorios a usar

* El ajuste de la semilla permite que el flujo de números aleatorios sea
  exactamente reproducible

* Cada vez que se genera números aleatorios para un propósito no trivial,
  **configure siempre la semilla**

SI: Piense en la secuencia completa
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* El análisis de datos es un proceso largo; no es solamente tablas / figuras
  / reportes

* datos crudos -> datos procesados -> análisis -> reportes

* Como obtuvo el resultado es tan importante como el resultado.

* Cuanto más replicable se puede hacer la secuencia de análisis de los datos
  es mejor para todos


En resumen
^^^^^^^^^^

* ¿Estamos haciendo buena ciencia?

* ¿Alguna parte de este análisis se hace a mano?
   * Si es así, ¿esas partes están documentadas *con precisión*?
   * ¿La realidad coincide con la documentación?

* ¿Hemos enseñado a la computadora para hacer tanto como sea posible (es decir,
  codificado)?

* ¿Estamos utilizando un sistema de control de versiones?

* ¿Hemos documentado nuestro entorno de software?

* ¿Hemos guardado cualquier salida que no podemos reconstruir a partir de
  datos originales + código?

* ¿Cuánto se puede retroceder en la secuencia del análisis antes que los
  resultados no sean reproducibles (automáticamente)?
