****************
Gestión de Datos
****************

Datos crudos y datos ordenados
==============================

Se sabe que se está trabajando con *datos crudos* porque:

# No se ha ejecutado software sobre los datos
# No se ha manipulado ningún valor de los datos
# No se ha quitado ningún valor de los datos
# No se han resumido ni agregado los datos de ninguna manera.

El objetivo es obtener *datos ordenados*, los cuales se caracterizan por:

# Cada variable medida debe estar en una columna diferente.
# Cada observación diferente de esa variable debe estar en una fila diferente
# Debe haber una tabla para cada "tipo" de variable.
# Si tiene varias tablas, debe tener una columna en la tabla que permita
  enlazarlas.

Observar las siguientes sugerencias:

* Es conveniente añadir una fila en la parte superior de cada archivo
  (cabecera) con los nombres de las variables.
* Los nombres de las variables deben ser legibles. Por ejemplo,
es preferible "EdadIngreso" en lugar de "EdadI".
* En general, se debe guardar una tabla por archivo.

Se debe contar con o construir un *libro de códigos*. Esto es, una descripción
del proceso de conversión de datos crudos a ordenados que contenga:

# Información acerca de las variables y sus unidades.
# Información sobre las decisiones de resumen o agregación de las variables
# Información sobre el diseño experimental utilizado.

Algunas recomendaciones:

* Debería haber una sección denominada *Diseño del estudio* que contenga una
  descripción del proceso de recolección de los datos.
* Debe haber una sección denominada *Libro de códigos* que describa cada
  variable y sus unidades.

El proceso de conversión de los datos debe ser automatizado mediante un script.

* La entrada del script deben ser los datos crudos.
* La salida del script, los datos ordenados.
* El script no debe tener parámetros.

En los casos que el proceso no pueda ser completamente automático se deben
proveer instrucciones para la conversión.

Gestión de archivos
===================

Por defecto R trabaja con los archivos sobre el *directorio de trabajo*,
los dos comandos básicos relacionados son:

* ``getwd()`` que devuelve el directorio de trabajo (*working directory*)
  actual.
* ``setwd(<ruta>)`` que establece el directorio de trabajo en la ruta indicada.

Considere la diferencia entre rutas relativas y absolutas.

* Rutas *relativas*: ``setwd("./data")``, ``setwd("../")``
* Rutas *absolutas*: ``setwd("/home/usuario/datos")``

Nota: En Windows las rutas se escriben:
``setwd("C:\\Usuarios\\usuario\\descargas")``

Es frecuente al automatizar procesos verificar si existe un archivo y crear
carpetas, para esto se cuenta con las funciones:

* ``file.exists()`` para verificar si un archivo o carpeta existe.
* ``dir.create()`` creará una carpeta si no existe

Un uso frecuente de estas funciones es como sigue:

.. code-block:: r

    if !file.exists("datos") {
        dir.create("datos")
    }

Descargar archivos de Internet
------------------------------

Para descargar archivos de Internet se puede utilizar la función
``download.file()``.

Este tipo de funciones son muy útiles para automatizar tareas rutinarias y
para reproducir tareas.

.. code-block:: r

    fileURL <- "http://api.worldbank.org/v2/es/country/ven?downloadformat=csv")
    download.file(fileURL, destfile = "./data/ve_worldbank.zip", method = "curl")
    list.files("./data")

Si la ruta empieza con ``http`` se puede utilizar ``download.file()`` con las
opciones por defecto. Si la conexión comienza en ``https`` en Mac o Linux
puede necesitar ``method = "curl"``.

Registre la fecha de su descarga, tomando la fecha actual con la función
``date()``.

.. code-block:: r

    if (!file.exists("data")) {
        dir.create("data")
    }
    fileUrl <- "https://data.baltimorecity.gov/api/views/dz54-2aru/rows.csv?accessType=DOWNLOAD"
    download.file(fileUrl, destfile = "cameras.csv", method = "curl")
    dateDownloaded <- date()

Lectura de datos desde distintos tipos de fuentes
=================================================

Datos en texto plano
--------------------

Para leer datos en texto plano la función básica es ``read_table()``. Es la
función mas flexible y robusta pero la que requiere el mayor número de
parámetros. Los datos se cargan a la memoria RAM, conjuntos grandes de datos
pueden ser problemáticos.

* Parámetros importantes: ``file``, ``header``, ``sep``, ``row.names``,
  ``nrows``.
* Algunas funciones relacionadas son: ``read.csv()``, ``read.csv2()``.

.. code-block:: r

    cameraData <- read.table("./data/cameras.csv", sep = ",", header = TRUE)
    head(cameraData)

Si se utiliza ``read.csv``, se establece ``sep = ","``, ``header = TRUE``.

.. code-block:: r

    cameraData <- read.csv("./data/cameras.csv")
    head(cameraData)

Archivos de Excel
-----------------

.. code-block:: r

    if(!file.exists("data")){dir.create("data")}
    fileUrl <- "http://www.ine.gov.ve/documentos/Demografia/EstadisticasVitales/xls/Nacimientos_Entidad.xls"
    download.file(fileUrl, destfile="./data/nacimientos_entidad.xlsx", method="curl")
    dateDownloaded <- date()



Herramientas básicas para limpiar y manipular datos
===================================================



Filtrar
-------

Crear nuevos conjuntos de datos
-------------------------------

Generar nuevas variables
------------------------

Conectar con bases de datos
===========================


