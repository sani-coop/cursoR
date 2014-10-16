****************
Gestión de Datos
****************

Datos crudos y datos ordenados
==============================

Se sabe que se está trabajando con *datos crudos* porque:

* No se ha ejecutado software sobre los datos
* No se ha manipulado ningún valor de los datos
* No se ha quitado ningún valor de los datos
* No se han resumido ni agregado los datos de ninguna manera.

El objetivo es obtener *datos ordenados*, los cuales se caracterizan por:

* Cada variable medida debe estar en una columna diferente.
* Cada observación diferente de esa variable debe estar en una fila diferente
* Debe haber una tabla para cada "tipo" de variable.
* Si tiene varias tablas, debe tener una columna en la tabla que permita
  enlazarlas.

Observar las siguientes sugerencias:

* Es conveniente añadir una fila en la parte superior de cada archivo
  (cabecera) con los nombres de las variables.
* Los nombres de las variables deben ser legibles. Por ejemplo,
  es preferible "EdadIngreso" en lugar de "EdadI".
* En general, se debe guardar una tabla por archivo.

Se debe contar con o construir un *libro de códigos*. Esto es, una descripción
del proceso de conversión de datos crudos a ordenados que contenga:

* Información acerca de las variables y sus unidades.
* Información sobre las decisiones de resumen o agregación de las variables
* Información sobre el diseño experimental utilizado.

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

Los archivos de Excel son el estándar de facto para el intercambio de datos.

.. code-block:: r

    if(!file.exists("data")){dir.create("data")}
    fileUrl <- "http://www.ine.gov.ve/documentos/Demografia/EstadisticasVitales/xls/Nacimientos_Entidad.xls"
    download.file(fileUrl, destfile="./data/nacimientos_entidad.xlsx", method="curl")
    dateDownloaded <- date()

Para leer datos desde archivos de Excel es necesario instalar el paquete
``xlsx``, el cual provee las funciones ``read.xlsx()`` y ``read.xlsx2()``.

.. code-block:: r

    library(xlsx)
    nacimientosData <- read.xlsx("./data/nacimientos_entidad.xlsx",
                                 sheetIndex=1, header=TRUE)
    head(cameraData)

Con frecuencia se requiere leer los datos de filas y columnas especificas.

.. code-block:: r

    colIndex <- 2:14
    rowIndex <- 10:32
    nacimientosFiltro <- read.xlsx("./data/nacimientos_entidad.xlsx",
                                   sheetIndex=1,
                                   colIndex=colIndex, rowIndex=rowIndex)
    nacimientosFiltro

* La función ``write.xlsx()`` puede utilizarse para generar archivos de Excel
  con argumentos similares.
* La función ``read.xlsx2()`` es mucho mas rápida que ``read.xlsx()`` pero
  puede ser inestable para leer de subconjuntos de filas.
* El paquete `XLConnect`_ tiene mas opciones para escribir y manipular
  archivos de Excel. La `viñeta de XLConnect`_ es un buen lugar para empezar
  con ese paquete.

.. important::

    Se recomienda encarecidamente almacenar los datos ya sea en bases de datos o
    archivos separados por comas (.csv) ya que permiten distribuir y procesar la
    información con mucha mayor facilidad.


.. _XLConnect: http://cran.r-project.org/web/packages/XLConnect/index.html
.. _viñeta de XLConnect: http://cran.r-project.org/web/packages/XLConnect/vignettes/XLConnect.pdf

Leyendo datos de la web
-----------------------

La tarea de extraer datos mediante programas de computadora del código HTML de
los sitios web se denomina `Webscraping`_

Hay muchos sitios web que publican periódicamente datos de interés que se
pueden leer de forma automática.

Intentar leer muchas páginas en un corto período de tiempo puede ocasionar
que su número IP sea bloqueado.

.. _Webscraping: http://en.wikipedia.org/wiki/Web_scraping)

Por ejemplo, para descargar datos de páginas en Google Scholar. Se pueden
descargar los datos de un sitio web aplicando la función ``readLines()``
sobre una conexión a un sitio web.

.. code-block:: r

    con = url("http://scholar.google.com/citations?user=HI-I6C0AAAAJ&hl=en")
    htmlCode = readLines(con)
    close(con)
    htmlCode

Como sabemos, HTML es un dialecto particular de XML, de manera que se pueden
extraer elementos de un sitio web con el paquete ``XML``

.. code-block:: r

    library(XML)
    url <- "http://scholar.google.com/citations?user=HI-I6C0AAAAJ&hl=en"
    html <- htmlTreeParse(url, useInternalNodes=T)

    xpathSApply(html, "//title", xmlValue)
    xpathSApply(html, "//td[@id='col-citedby']", xmlValue)

la función `xpathSApply()` permiten extraer elementos de la estructura del
código HTML del sitio web.

El paquete `httr`_ puede utilizarse de forma similar para extraer elementos
desde las etiquetas de los sitios web. Este paquete provee varias funciones
útiles:

* la función `GET()` realiza una solicitud al sitio web,
  lo cual incluye metadatos de la conexión.
* la función ``content()`` toma el contenido de la solicitud
* y finalmente, la función ``htmlParse()`` del paquete ``XML`` nos permite
  extraer datos de las etiquetas del código HTML.

.. code-block:: r

    library(httr); html2 = GET(url)
    content2 = content(html2, as="text")
    parsedHtml = htmlParse(content2, asText=TRUE)
    xpathSApply(parsedHtml, "//title", xmlValue)

Cuando se requiere autenticación para acceder a un sitio web se puede
especificar el argumento ``authenticate`` de la función ``GET()``.

.. code-block:: r

    pg1 = GET("http://httpbin.org/basic-auth/user/passwd")
    pg1

.. code-block:: r

    pg2 = GET("http://httpbin.org/basic-auth/user/passwd",
              authenticate("user","passwd"))
    pg2
    names(pg2)

Utilizando "manejadores".

.. code-block:: r

    google = handle("http://google.com")
    pg1 = GET(handle=google,path="/")
    pg2 = GET(handle=google,path="search")

Los manejadores preservan las configuraciones y los "cookies" a los largo de
solicitudes múltiples. Es la base de todas las solicitudes hechas a través
del paquete ``httr``

El blog `R Bloggers`_ tiene una gran cantidad de ejemplos de webscraping.
La ayuda del paquete `httr`_ ofrece muchos ejemplos útiles.

.. _R Bloggers: http://www.r-bloggers.com/?s=Web+Scraping
.. _httr: http://cran.r-project.org/web/packages/httr/httr.pdf


Leyendo desde APIs
------------------

Las interfaces de programación de aplicaciones (APIs por su acrónimo en inglés)
ofrecen mecanismos potentes y flexibles para acceder a los datos que subyacen en
las aplicaciones web.

La gran ventaja es que permiten hacer solicitudes específicas en lugar de
descargar conjuntos completos de datos, y porque permite compartir datos entre
sistemas distintos.

Es de particular interés extraer datos de las aplicaciones de las "redes
sociales". Para esto generalmente hay que crear una aplicación en uno
de estos sistemas y obtener las claves de autenticación de dicha aplicación.

El ejemplo de una conexión al API sería como sigue:

.. code-block:: r

    myapp = oauth_app("twitter",
                      key="yourConsumerKeyHere",
                      secret="yourConsumerSecretHere")
    sig = sign_oauth1.0(myapp,
                        token = "yourTokenHere",
                        token_secret = "yourTokenSecretHere")
    homeTL = GET("https://api.twitter.com/1.1/statuses/home_timeline.json", sig)

El estándar de facto de transferencia de información entre aplicaciones
mediante sus APIs es JSON (Javascript Object Notation),
es una forma de expresar información como valores del lenguaje javascript.

El paquete ``jsonlite`` provee funciones para el manejo de información en
formato JSON.

.. code-block:: r

    json1 = content(homeTL)
    json2 = jsonlite::fromJSON(toJSON(json1))
    json2[1,1:4]

* Las solicitudes de ``httr``,  ``GET()``, ``POST()``, ``PUT()``,
  `DELETE()` ofrecen opciones para conexiones con autenticación.
* La mayoría de los APIs actuales utilizan mecanismos estándares de
  autenticación como OAUTH.
* ``httr`` funciona bien con Facebook, Google, Twitter, Github, etc.

Herramientas básicas para limpiar y manipular datos
===================================================

Un repaso sobre filtros
-----------------------

.. code-block:: r

    set.seed(13435)
    X <- data.frame("var1"=sample(1:5),"var2"=sample(6:10),"var3"=sample(11:15))
    X <- X[sample(1:5),]; X$var2[c(1,3)] = NA
    X

Se puede acceder a los datos de un data frame con una notación matricial
``data[num_fila, num_col|nombre_col]``. O bien de la forma
``data$nombre_col[num_fila]``.

Pasar un vector como índice permite reordenar o seleccionar (incluso
repetidas) filas y/o columnas del data frame.

Las columnas se pueden acceder ya sea por posición o por nombre.

.. code-block:: r

    X[,1]
    X[,"var1"]
    X[1:2,"var2"]

También se pueden aplicar filtros como expresiones lógicas. Estas devuelven
vectores lógicos.

.. code-block:: r

    X[(X$var1 <= 3 & X$var3 > 11), ]
    X[(X$var1 <= 3 | X$var3 > 15), ]

La función ``which()`` devuelve los índices para los que se cumple una
condición.

.. code-block:: r

    X[which(X$var2 > 8), ]

Para ordenar un vector se puede utilizar la función ``sort()``. El argumento
``decreasing`` indica si se ordena en orden decreciente,
y ``na.last`` si los valores faltantes se colocan al final.

.. code-block:: r

    sort(X$var1)
    sort(X$var1, decreasing=TRUE)
    sort(X$var2, na.last=TRUE)

La función ``order()`` devuelve una lista con los índices que permiten
reordenar una columna. Se pueden introducir columnas adicionales para romper
empates. Es útil para ordenar un data frame completo.

.. code-block:: r

    X[order(X$var1), ]
    X[order(X$var1,X$var3), ]

El paquete ``plyr`` ofrece un conjunto de herramientas para facilitar tareas
de separar, aplicar y combinar datos. La función ``arrange()`` de ``plyr``
ofrece un mecanismo mas comprensible para ordenar datos.

.. code-block:: r

    library(plyr)
    arrange(X, var1)
    arrange(X, desc(var1))

Para añadir un nueva variable a un data frame simplemente se asigna un vector
a un nuevo nombre de columna.

.. code-block:: r

    X$var4 <- rnorm(5)
    X

Se pueden añadir las funciones ``cbind()`` y ``rbind()`` para unir vectores o
data frames como columnas o filas respectivamente. Las dimensiones de los
objetos deben coincidir.

.. code-block:: r

Y <- cbind(X,rnorm(5))
Y

Mas sobre este tema en la `notas de Andrew Jaffe`_.

.. _notas de Andrew Jaffe: http://www.biostat.jhsph.edu/~ajaffe/lec_winterR/Lecture%202.pdf

Haciendo resúmenes de sus datos
-------------------------------

Para los siguientes ejemplos, empezar por descargar algunos datos de la web:

.. code-block:: r

    if(!file.exists("./data")){dir.create("./data")}
    fileUrl <- "https://data.baltimorecity.gov/api/views/k5ry-ef3g/rows.csv?accessType=DOWNLOAD"
    download.file(fileUrl,destfile="./data/restaurants.csv",method="curl")
    restData <- read.csv("./data/restaurants.csv")

Un pequeño vistazo a los datos descargados:

.. code-block:: r

    head(restData, n=3)
    tail(restData, n=3)

Se obtiene un resumen descriptivo:

.. code-block:: r

    summary(restData)

Información en mayor profundidad:

.. code-block:: r

    str(restData)

Los cuantiles de las variables cuantitativas:

.. code-block:: r

    quantile(restData$councilDistrict, na.rm=TRUE)
    quantile(restData$councilDistrict, probs=c(0.5,0.75,0.9))

Se construye una tabla de frecuencias

.. code-block:: r

    table(restData$zipCode, useNA="ifany")

Ahora una tabla de frecuencias cruzadas:

.. code-block:: r

    table(restData$councilDistrict, restData$zipCode)

Se verifica la existencia de valores faltantes:

.. code-block:: r

    sum(is.na(restData$councilDistrict))
    any(is.na(restData$councilDistrict))
    all(restData$zipCode > 0)

Valores faltantes por columna:

.. code-block:: r

    colSums(is.na(restData))
    all(colSums(is.na(restData))==0)

Frecuencia de valores con características particulares.

.. code-block:: r

    table(restData$zipCode %in% c("21212"))
    table(restData$zipCode %in% c("21212", "21213"))

Valores con características particulares.

.. code-block:: r

    restData[restData$zipCode %in% c("21212", "21213"), ]

Tablas cruzadas

Se toma como ejemplo de referencia la tabla `UCBAdmissions`. Se convierte a
data frame.

.. code-block:: r

    data(UCBAdmissions)
    DF = as.data.frame(UCBAdmissions)
    summary(DF)

Se crea una tabla de referencia cruzada, nótese como se utilizan los factores
``Gender`` y ``Admit``.

.. code-block:: r

    xt <- xtabs(Freq ~ Gender + Admit, data=DF)
    xt

Para crear *tablas planas*. Empezamos por crear una columna que nos permita
tener observaciones únicas. Luego se aplica la función ``ftable()`` (flat table)

.. code-block:: r

    warpbreaks$replicate <- rep(1:9, len = 54)
    xt = xtabs(breaks ~., data=warpbreaks)
    xt
    ftable(xt)

Para obtener una medida del uso de memoria de cualquier objeto mediante la
función ``object.size()``.

.. code-block:: r

    fakeData = rnorm(1e5)
    object.size(fakeData)
    print(object.size(fakeData),units="Mb")


Conectar con bases de datos
===========================

MySQL
-----

* `MySQL`_ Sistema de base de datos libre ampliamente usada.
* Ampliamente utilizado por aplicaciones de Internet
* Los dato están estructurados en:
  * Bases de datos
  * Tablas dentro de bases de datos
  * Campos dentro de tablas
* Cada fila es un registro

Desde la adquisición de SUN por Oracle, existe una versión comunitaria de
MySQL denominada MariaDB.

.. _mySQL: http://en.wikipedia.org/wiki/MySQL

Instalación de RMySQL
---------------------

* En Linux o Mac: ```install.packages("RMySQL")```
* En Windows:

  * Instrucciones oficiales - http://biostat.mc.vanderbilt.edu/wiki/Main/RMySQL
    (tambien puede ser útil para los usuarios Mac/Linux)
  * Guía potencialmente útil - http://www.ahschulz.de/2013/07/23/installing-rmysql-under-windows/


Conexión a bases de datos. Obtener una lista de las bases de datos disponibles.

.. code-block:: r

    ucscDb <- dbConnect(MySQL(),user="genome",
                        host="genome-mysql.cse.ucsc.edu")
    result <- dbGetQuery(ucscDb,"show databases;"); dbDisconnect(ucscDb);
    result

Conexión a la base de datos "hg19" y obtener una lista de sus tablas.

.. code-block:: r

    hg19 <- dbConnect(MySQL(),user="genome", db="hg19",
                        host="genome-mysql.cse.ucsc.edu")
    allTables <- dbListTables(hg19)
    length(allTables)
    allTables[1:5]

Para obtener las dimensiones de una tabla en particular

.. code-block:: r

    dbListFields(hg19,"affyU133Plus2")
    dbGetQuery(hg19, "select count(*) from affyU133Plus2")

Finalmente, obtener datos de la base de datos.

.. code-block:: r

    affyData <- dbReadTable(hg19, "affyU133Plus2")
    head(affyData)

Si se quiere obtener un subconjunto de la tabla.

.. code-block:: r

    query <- dbSendQuery(hg19, "select * from affyU133Plus2 where misMatches between 1 and 3")
    affyMis <- fetch(query); quantile(affyMis$misMatches)
    affyMisSmall <- fetch(query,n=10); dbClearResult(query);
    dim(affyMisSmall)

No hay que olvidar cerrar la conexión.

.. code-block:: r

    dbDisconnect(hg19)

Recursos adicionales:

* RMySQL vignette http://cran.r-project.org/web/packages/RMySQL/RMySQL.pdf
* Lista de comandos http://www.pantz.org/software/mysql/mysqlcommands.html

  * En ningún caso borrar, añadir, o enlazar tablas desde ensembl. Solamente
    select.
  * En general, tener cuidados con los comandos de MySQL

* Un post con un buen resúmen de comandos http://www.r-bloggers.com/mysql-and-r/
