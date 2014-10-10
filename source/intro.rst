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

Finalmente en la sección *Ayuda*, aparte de acceso integrado a la ayuda de R
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

