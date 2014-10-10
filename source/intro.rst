****************
Introducción a R
****************

Conceptos básicos
=================

El lenguaje R tiene las características básicas:

- R es un Lenguaje y Entorno de programación dinámico para análisis
  estadístico y gráficos.

- Puede utilizarse en modo interactivo o script.

- Es Software Libre, con una amplia comunidad de desarrolladores,
  investigadores y usuarios.



Instalación de R y RStudio
==========================

Windows
-------

Descargar R desde http://cran.r-project.org/bin/windows/base/

Seleccionar el enlace donde dice: Download R X.X.X for Windows.
Donde X.X.X es la versión de R más reciente. La actual es 3.1.1

http://cran.r-project.org/bin/windows/base/R-3.1.1-win.exe

Ejecutar el instalador de R

Descargar la versión más reciente de RStudio desde la dirección:
http://rstudio.org/download/desktop

La versión más reciente para el 9 de octubre es 0.98.162

http://download1.rstudio.org/RStudio-0.98.1062.exe

Ejecutar el instalador de RStudio

Linux
-----

Debian estable
^^^^^^^^^^^^^^

Añadir el repositorio (a `/etc/apt/sources.list`)

deb http://cran.rstudio.com/bin/linux/debian wheezy-cran3/

Añadir claves backport de Debian

.. code-block:: bash

   $ apt-key adv --keyserver keys.gnupg.net --recv-key 381BA480

Finalmente instalar

.. code-block:: bash

   $ apt-get update
   $ apt-get install r-base r-base-dev

Ubuntu o Mint
^^^^^^^^^^^^^

Para instalar la versión mas actualizada de R añadir el repositorio:
(a `/etc/apt/sources.list` o mediante "Orígenes de software")

deb http://cran.rstudio.com/bin/linux/ubuntu trusty/

O la versión de Ubuntu que corresponda

Añadir claves para Ubuntu archive


.. code-block:: bash

   $ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E084DAB9

E instalar finalmente

.. code-block:: bash

   $ sudo apt-get update
   $ sudo apt-get install r-base r-base-dev


Dependiendo del caso, descargar
http://download1.rstudio.org/rstudio-0.98.1062-i386.deb
http://download1.rstudio.org/rstudio-0.98.1062-amd64.deb

Según la arquitectura sea de 32 o 64 bits, e instalar utilizando

sudo dpkg -i rstudio-0.98.1062-???.deb

O con gdebi-gtk

Interfaz gráfica de RStudio
===========================

Consta de 4 paneles configurables:

 - Editor
 - Consola
 - Entorno
 - Ayuda

El *Editor* por defecto está oculto y se activa cuando se crea un nuevo archivo
que puede ser código R, R Markdown, un archivo de texto u otros.

La *Consola* es el terminal interactivo de R donde se envían los comandos para
ser ejecutados.

En el *Entorno* se encuentra tanto la información de los objetos de R en la
memoria con funcionalidad para importar datos, y existen pestañas para manejo
del historial y del sistema de control de versiones.

Finalmente en el panel *Ayuda*, aparte de acceso integrado a la ayuda de R
y los paquetes activos, se ofrece un explorador de archivos, el gestor de
paquetes y el gestor de gráficos.

Comparación con EXCEL, SAS, SPSS, Stata
=======================================

En la actualidad, el SAS Institute e IBM SPSS, y otras compañías trabajan
para extender sus sistemas usando R. Al igual que hay complementos de EXCEL
para integrarlo con las capacidades de R. En buena medida, R es una alternativa
para Stata como lenguaje de programación y modelado estadístico.

Entre los beneficios directos de usar R se tiene:

 - Acceso a una gran variedad de métodos de análisis.
 - Acceso temprano a nuevos métodos.
 - Muchos paquetes computacionales permiten ejecutar programas en R. Puede
   realizar el manejo de datos en el sistema de su preferencia y después lanzar
   los análisis usando R.
 - La rápida difusión de R como lenguaje estadístico de referencia.
 - La gran calidad y flexibilidad de los gráficos generados por R.
 - La capacidad para analizar datos en una gran cantidad de formatos.
 - Cuenta con capacidades de orientación a objetos.
 - Facilidades para implantar sus propios métodos de análisis.
 - Posibilidad de revisar en detalle como están implantados los métodos de
   análisis.
 - Los métodos propios están desarrollados en el mismo lenguaje que la mayoría
   de los métodos del sistema.
 - Provee capacidades de álgebra matricial similares a Matlab.
 - Se ejecuta en prácticamente cualquier sistema operativo, ya sea Windows, Mac,
   Linux, o Unix.
 - R es libre.

El soporte que se espera de R se ofrece mediante las listas de discusión vía
email, y foros como stackoverflow. Estos espacios tienen dinámicas propias y
demandan de los usuarios destrezas para plantear preguntas y entender las
respuestas, y desarrollar criterios para distinguir entre distintas opciones.

En sistemas como SAS y SPSS se distinguen 5 componentes tales como:

 - Gestión de datos, que permiten leer, transformar y organizar los datos.
 - Procedimientos estadísticos y gráficos.
 - Sistemas de extracción de salidas que permiten extraer salidas de unos
   procedimientos para utilizarlos como entradas en otros: SAS Output Delivery
   System (ODS) y SPSS Output Management System (OMS).
 - Un lenguaje de macros que facilita el uso de los anteriores componentes.
 - Un lenguaje matricial para implantar nuevos métodos: SAS/IML y SPSS Matrix.

La diferencia es que R realiza estas funciones de una forma tal que las integra
a todas. En particular facilita la gestión de salidas, una característica poco
utilizada por los usuarios de los otros sistemas.

Instalación de paquetes
=======================

Cuando se descarga R del Comprehensive Archive Network (CRAN), se obtiene el
sistema *base* que ofrece la funcionalidad básica del lenguaje R.

Se encuentra disponible una gran cantidad de paquetes que extienden la
funcionalidad básica de R. Estos paquetes son desarrollados y publicados por la
comunidad de usuarios de R.

La principal ubicación para obtener paquetes de R es `CRAN`_. Se dispone de
muchos paquetes para aplicaciones de bioinformática,del Proyecto
`Bioconductor`_.

Se puede obtener información de los paquetes disponibles en los repositorios
configurados mediante la función `available_packages()`.

En la actualidad se dispone de caso 6 mil paquetes en CRAN que cubren una gran
diversidad de temas. Una buena orientación inicial se puede encontrar en el
enlace `Task Views`_ (Vista por tareas) de la página principal de CRAN, que
agrupa los paquetes de R por área de aplicación.

Instalar paquetes de R
----------------------

Los paquetes se pueden instalar con la función de R `install.packages()`. Para
instalar un paquete se pasa su nombre como primer argumento. El código a
continuación instala el paquete **knitr** desde CRAN.

.. code-block:: r

   install.packages("knitr")

Este comando descarga el paquete **knitr** desde CRAN y lo instala en su
computadora. De igual manera, se descargan e instalan todas sus dependencias.

Si se introduce como parámetro un vector tipo carácter se pueden instalar
varios paquetes en simultáneo.

.. code-block:: r

   install.packages(c("knitr", "dplyr", "ggplot2"))

Instalación desde RStudio
-------------------------

Desde la interfaz de RStudio se pueden instalar desde el menu
`Tools>Install Packages...`, o bien desde la pestaña *Packages* del panel
*Ayuda*.

En ambos casos se despliega un diálogo de instalación de paquetes que permite
indicar el nombre del paquete en una caja de texto. Si el paquete se encuentra
en el repositorio después de escribir unas pocas letras del nombre debería
autocompletarse.

Instalación desde Bioconductor
------------------------------

Para instalar un paquete desde Bioconductor se deben instalar las funciones
básicas de este repositorio mediante las instrucciones:

.. code-block:: r

   source("http://bioconductor.org/biocLite.R")
   biocLite()

El primer comando carga funciones de R desde el script `biocLite.R`, el segundo
ejecuta una función contenida en este.

A partir de este momento, Bioconductor queda configurado como repositorio y
es posible instalar paquetes del mismo utilizando la función
`install_packages()`.

Cargar paquetes
---------------

Para que las funcionalidades de los paquetes estén disponibles en la sesión de
R tienen que ser *cargados* en la memoria. Esto se realiza mediante la función
`library()`. Por ejemplo, para cargar el paquete `reshape`:

.. code-block:: r

   library(reshape)

Nótese que a diferencia de la instalación, en este caso no son necesarias las
comillas para introducir el nombre del paquete.

Este comando carga tanto el paquete indicado como todas sus dependencias.

Al cargar un paquete, todos los objetos contenidos en el mismo quedan
disponibles en el entorno, y su documentación es incluida en el sistema de
ayuda.

.. code-block:: rconsole

   > library("rstudio", lib.loc="~/R/x86_64-pc-linux-gnu-library/3.1")
   > search()
    [1] ".GlobalEnv"        "package:rstudio"   "tools:rstudio"
    [4] "package:stats"     "package:graphics"  "package:grDevices"
    [7] "package:utils"     "package:datasets"  "package:methods"
   [10] "Autoloads"         "package:base"

Desde la interfaz de RStudio en la pestaña *Packages* del Panel Ayuda, se pueden
cargar paquetes haciendo clic en la casilla de verificación que se encuentra a
la izquierda del nombre correspondiente.

.. _CRAN: http://cran.r-project.org/
.. _Bioconductor: http://www.bioconductor.org/
.. _Task Views: http://cran.r-project.org/web/views/