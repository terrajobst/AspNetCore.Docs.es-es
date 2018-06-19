---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-cs
title: Las estrategias de desarrollo de base de datos y la implementación (C#) | Documentos de Microsoft
author: rick-anderson
description: Al implementar una aplicación controlada por datos por primera vez a ciegas puede copiar la base de datos en el entorno de desarrollo al entorno de producción. B...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 3e8b0627-3eb7-488e-807e-067cba7cec05
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-cs
msc.type: authoredcontent
ms.openlocfilehash: 801eedba50e03b2fd9327e9a2902178b35b4275a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889532"
---
<a name="strategies-for-database-development-and-deployment-c"></a>Estrategias de desarrollo de base de datos y la implementación (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga de PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial10_DBDevel_cs.pdf)

> Al implementar una aplicación controlada por datos por primera vez a ciegas puede copiar la base de datos en el entorno de desarrollo al entorno de producción. Pero realizar un blind copia en implementaciones posteriores sobrescribirán todos los datos insertados en la base de datos de producción. En su lugar, la implementación de una base de datos implica aplicar los cambios realizados en la base de datos de desarrollo desde la última implementación en la base de datos de producción. Este tutorial examina estos desafíos y ofrece diversas estrategias para ayudar a que relata y aplicar los cambios realizados en la base de datos desde la última implementación.


## <a name="introduction"></a>Introducción

Como se describe en los tutoriales anteriores, implementar una aplicación de ASP.NET implica copiar el contenido pertinente desde el entorno de desarrollo al entorno de producción. Implementación no es un evento único, pero en su lugar algo que se produce cada vez que se lanza una nueva versión del software o errores o problemas de seguridad se han identificado y dirigido. Cuando se copian las páginas ASP.NET, imágenes, archivos JavaScript y otros archivos de este tipo en el entorno de producción que no es necesario que preocupar por el uso de estos archivos se cambiaron desde la última implementación. Seguirá a ciegas puede copiar el archivo a la producción, sobrescribiendo el contenido existente. Por desgracia, esta simplicidad no se extiende a implementar la base de datos.

Al implementar una aplicación controlada por datos por primera vez a ciegas puede copiar la base de datos en el entorno de desarrollo al entorno de producción. Pero realizar un blind copia en implementaciones posteriores sobrescribirán todos los datos insertados en la base de datos de producción. En su lugar, la implementación de una base de datos implica la aplicación de la *cambios* realizados en la base de datos de desarrollo desde la última implementación en la base de datos de producción. Este tutorial examina estos desafíos y ofrece diversas estrategias para ayudar a que relata y aplicar los cambios realizados en la base de datos desde la última implementación.

## <a name="the-challenges-of-deploying-a-database"></a>Los desafíos de la implementación de una base de datos

Antes de que se ha implementado una aplicación controlada por datos por primera vez, hay una base de datos, es decir, la base de datos en el entorno de desarrollo, por eso, al implementar una aplicación controlada por datos por primera vez a ciegas puede copiar la base de datos en el entorno de desarrollo al entorno de producción. Pero una vez que se ha implementado la aplicación, hay dos copias de la base de datos: uno en desarrollo y otro en producción.

Entre las implementaciones de las bases de datos de desarrollo y producción pueden dejen de estar sincronizados. Mientras que el esquema s de base de datos de producción permanece sin cambios, puede cambiar el esquema s de base de datos de desarrollo a medida que se agregan nuevas características. Puede agregar o quitar columnas, tablas, vistas o procedimientos almacenados. También puede haber datos importantes que se agregan a la base de datos de desarrollo. Muchas aplicaciones orientadas a datos incluyen tablas de búsqueda que se rellena con datos codificados de forma rígida, específica de la aplicación que no se puede modificar el usuario. Por ejemplo, un sitio Web de subasta podría tener una lista desplegable con las opciones que describen el estado del elemento que se contemple la subasta: nuevo, como nuevo, bueno y aceptable. En lugar de codificar de forma rígida directamente en la lista desplegable estas opciones es mejor colocarlos en una tabla de base de datos. Si, durante el desarrollo, una nueva condición denominada a malo se agrega a la tabla, a continuación, al implementar la aplicación en este mismo registro debe agregarse a la tabla de búsqueda en la base de datos de producción.

Idealmente, implementar la base de datos, implicaría copiar la base de datos de desarrollo a producción. Pero tenga en cuenta que después de haber implementado la aplicación y reanudar el desarrollo, la base de datos de producción se va a rellenar con datos reales de los usuarios reales. Por lo tanto, si fuera a simplemente copiar la base de datos de desarrollo a entornos de producción en la siguiente implementación sobrescribir la base de datos de producción y perder los datos existentes. El resultado neto es que la implementación de la base de datos se reduce a aplicar los cambios realizados en la base de datos de desarrollo desde la última implementación.

Dado que la implementación de una base de datos implica aplicar los cambios en el esquema y, posiblemente, los datos desde la última implementación, un historial de cambios debe ser mantiene (o determina en tiempo de implementación) para que se puedan aplicar los cambios en producción. Hay una variedad de técnicas para administrar y aplicar los cambios al modelo de datos.

### <a name="defining-the-baseline"></a>Definición de la línea de base

Para mantener los cambios realizados en la base de datos de aplicación s debe tener un estado inicial, una línea de base para que los cambios se aplican a. Situado en un extremo, el estado inicial podría ser una base de datos vacía con ningún tablas, vistas o procedimientos almacenados. Una línea de base se produce en un registro de cambio grande porque debe incluir la creación de todas las tablas de base de datos s, vistas y procedimientos almacenados junto con los cambios realizados después de la implementación inicial. En el otro extremo del espectro podría establecer la línea de base como la versión de la base de datos que se implementa inicialmente en el entorno de producción. Esta opción da como resultado un registro de cambios mucho menor porque solo incluye los cambios realizados en la base de datos después de la primera implementación. Éste es el enfoque que prefiera. Y por supuesto puede elegir un medio más del enfoque de carretera, definición de la línea de base como en algún momento entre la creación inicial de la base de datos y que la base de datos se implementó por primera vez.

Una vez que haya elegido una línea base puede ser conveniente generar un script SQL que se puede ejecutar para volver a crear la versión de línea de base. Una secuencia de comandos permite volver rápidamente a la versión de línea de base de la base de datos. Esta funcionalidad es especialmente útil en proyectos grandes, donde puede haber varios desarrolladores que trabajan en el proyecto o en otros entornos, como pruebas o de almacenamiento provisional, que cada uno tenga su propia copia de la base de datos.

Hay una variedad de herramientas a su disposición para generar un script SQL de la versión de línea de base. Desde SQL Server Management Studio (SSMS) puede hacer clic en la base de datos, vaya al submenú de tareas y elige la opción de generar secuencias de comandos. Esto inicia al Asistente de Script, que puede indicar a generar un archivo que contiene los comandos SQL para crear objetos de s de la base de datos. Otra opción es el Asistente para publicación de base de datos, lo que puede generar los comandos SQL para crear no solo el esquema de base de datos, sino también los datos en las tablas de base de datos. El Asistente para publicación de base de datos se examinan en detalle en la *implementar una base de datos* tutorial. Independientemente de qué herramienta que utilice, en el extremo debe tener un archivo de script que puede usar para volver a crear la versión de línea de base de la base de datos, fuese necesario surgen.

## <a name="documenting-the-database-changes-in-prose"></a>Documentar los cambios de base de datos de texto

Es la manera más sencilla para mantener un registro de los cambios en el modelo de datos durante la fase de desarrollo registrar los cambios en el texto. Por ejemplo, si durante el desarrollo de una aplicación ya implementadas se agrega una nueva columna a la `Employees` tabla, quitar una columna de la `Orders` de tabla y agregar una nueva tabla (`ProductCategories`), se mantendría un archivo de texto o documento de Microsoft Word con el historial de la siguiente:

<a id="0.4_table01"></a>


| **Fecha de cambio** | **Cambie los detalles** |
| --- | --- |
| 2009-02-03: | Columna agregada `DepartmentID` (`int`, no es NULL) para el `Employees` tabla. Agrega una restricción foreign key de `Departments.DepartmentID` a `Employees.DepartmentID`. |
| 2009-02-05: | Columna quitada `TotalWeight` desde el `Orders` tabla. Datos que ya ha capturado en asociados `OrderDetails` registros. |
| 2009-02-12: | Crea el `ProductCategories` tabla. Hay tres columnas: `ProductCategoryID` (`int`, `IDENTITY`, `NOT NULL`), `CategoryName` (`nvarchar(50)`, `NOT NULL`), y `Active` (`bit`, `NOT NULL`). Agrega una restricción primary key a `ProductCategoryID`y un valor predeterminado de 1 a `Active`. |


Hay una serie de inconveniente de este enfoque. Para empezar, no hay ninguna nueva esperanza para la automatización. En cualquier momento estos cambios deben aplicarse a una base de datos -, como cuando se implementa la aplicación: un desarrollador debe implementar manualmente cada uno de ellos cambia, uno en uno. Además, si necesita reconstruir una versión concreta de la base de datos de la línea de base mediante el registro de cambios, hacerlo por lo que tardará más tiempo a medida que aumenta el tamaño del registro. Otro inconveniente de este método es que la claridad y el nivel de detalle de cada entrada de registro de cambio se deja a la persona que registrar el cambio. En un equipo con varios desarrolladores algunos pueden realizar entradas más detalladas, más legibles ni más precisas que otros. Además, son posibles errores tipográficos y otros errores de entrada de datos relacionados con el lenguaje.

La principal ventaja de documentar los cambios de base de datos de texto es su sencillez. Cuando no está familiarizado de necesidad de t con la sintaxis SQL para crear y modificar objetos de base de datos. En su lugar, puede registrar los cambios en el texto y su implementación a través de la interfaz gráfica de usuario de SQL Server Management Studio s.

Mantener el registro de cambios de texto, es verdad, no es muy sofisticados y logradas funcionar bien con algunos proyectos, como aquellos que son grandes en el ámbito, tiene cambios frecuentes en el modelo de datos o implicar varios desarrolladores de software. Pero he visto este enfoque funciona muy bien en proyectos pequeños, solo miembro que tienen solo ocasionales cambios en el modelo de datos y donde el desarrollador solo no tiene un gran conocimiento sobre la sintaxis SQL para crear y modificar objetos de base de datos.

> [!NOTE]
> Aunque la información en el registro de cambios es, técnicamente, solo necesita hasta el tiempo implementar, recomienda mantener un historial de cambios. Pero en lugar de mantener un único, el creciente archivo de registro de cambios, considere la posibilidad de tener un archivo de registro de cambio diferente para cada versión de la base de datos. Por lo general deseará versión de la base de datos cada vez que se implementa. Al mantener un registro de los registros de cambio puede, a partir de la línea de base, volver a cualquier versión de la base de datos mediante la ejecución de las secuencias de comandos de cambio de registro a partir de la versión 1 y continuando hasta que llegue a la versión se deben volver a.


## <a name="recording-the-sql-change-statements"></a>Grabación de las instrucciones de cambio SQL

El principal inconveniente de mantener el registro de cambios de texto es la falta de automatización. Idealmente, implementar los cambios de base de datos a la base de datos de producción en tiempo de implementación sería tan fácil como hacer clic en un botón para ejecutar un script en lugar de tener que realizar manualmente una lista de instrucciones. Automatización de este tipo es posible por mantener un registro de cambios que contiene los comandos SQL que se utiliza para modificar el modelo de datos.

La sintaxis SQL incluye una serie de instrucciones para crear y modificar varios objetos de base de datos. Por ejemplo, el [ *instrucción CREATE TABLE*](https://msdn.microsoft.com/library/ms174979.aspx), cuando se ejecuta, crea una nueva tabla con las restricciones y las columnas especificadas. El [ *instrucción ALTER TABLE* ](https://msdn.microsoft.com/library/ms190273.aspx) modifica una tabla existente, agregar, quitar o modificar sus columnas o restricciones. También existen instrucciones para crear, modificar y quitar índices, vistas, funciones definidas por el usuario, procedimientos almacenados, desencadenadores y otros objetos de base de datos.

Volver a nuestro ejemplo anterior, que la imagen durante el desarrollo de una aplicación ya implementadas se agrega una nueva columna a la `Employees` tabla, quitar una columna de la `Orders` de tabla y agregar una nueva tabla (`ProductCategories`). Estas acciones se crearán en un archivo de registro de cambios con los comandos SQL siguientes:

[!code-sql[Main](strategies-for-database-development-and-deployment-cs/samples/sample1.sql)]

Aplicación de estos cambios en la base de datos de producción en tiempo de implementación es una operación de un solo clic: Abra SQL Server Management Studio, conéctese a la base de datos de producción, abra una ventana nueva consulta, pegar el contenido del registro de cambios y haga clic en ejecutar para ejecutar la secuencia de comandos.

## <a name="using-a-comparison-tool-to-synchronize-the-data-models"></a>Utiliza una herramienta de comparación para sincronizar los modelos de datos

Documentar los cambios de la base de datos en texto es fácil, pero los cambios de la implementación requiere un programador realizar cada modificación en la base de datos de producción uno a uno; Documentar los comandos SQL de cambio hace que la implementación de los cambios en la base de datos de producción lo más fácil y rápido que hacer clic en un botón, pero requiere aprendizaje y controla las instrucciones SQL y la sintaxis para crear y modificar objetos de base de datos. Herramientas de comparación de la base de datos toman lo mejor de ambos enfoques y descartar el peor.

Una herramienta de comparación de la base de datos compara el esquema o los datos de dos bases de datos y muestra un informe de resumen que muestra la diferencia entre las bases de datos. A continuación, haciendo clic en un botón, puede generar los comandos SQL para la sincronización de uno o varios objetos de base de datos. En pocas palabras, se puede usar una herramienta de comparación de la base de datos para comparar el desarrollo y las bases de datos de producción durante la implementación, generar un archivo que contiene la instrucción SQL comandos que, cuando se ejecuta, se aplicarán los cambios en el esquema de base de datos s de producción por lo que ese TI refleja el esquema de base de datos s de desarrollo.

Hay una variedad de herramientas de comparación de base de datos de otros fabricantes que ofrece muchos proveedores diferentes. Un ejemplo es [ *SQL Compare*](http://www.red-gate.com/products/SQL_Compare/), [ *Red Gate Software*](http://www.red-gate.com/). Permiten s guían en el proceso de uso de SQL Compare para comparar y sincronizar los esquemas de bases de datos de desarrollo y producción.

> [!NOTE]
> En el momento de redactar este artículo la versión actual de SQL Compare fue la versión 7.1, con la edición Standard costes 395 dólares. Puede seguir el tutorial mediante la descarga de una prueba gratuita de 14 días.


Cuando se inicia la comparación de SQL se abre el cuadro de diálogo de proyectos de comparación, que muestra los proyectos de SQL Compare guardados. Cree un nuevo proyecto. Esto inicia el Asistente de configuración del proyecto, que solicite información acerca de las bases de datos para comparar (consulte la figura 1). Escriba la información de las bases de datos de entorno de desarrollo y producción.


[![Comparar el desarrollo y las bases de datos de producción](strategies-for-database-development-and-deployment-cs/_static/image2.jpg)](strategies-for-database-development-and-deployment-cs/_static/image1.jpg)

**Figura 1**: comparar el desarrollo y las bases de datos de producción ([haga clic aquí para ver la imagen a tamaño completo](strategies-for-database-development-and-deployment-cs/_static/image3.jpg))


> [!NOTE]
> Si la base de datos del entorno de desarrollo es un archivo de base de datos de SQL Express Edition en el `App_Data` carpeta del sitio Web debe registrar la base de datos en el servidor de base de datos de SQL Server Express para poder seleccionarlo en el cuadro de diálogo se muestra en la figura 1. Abra SQL Server Management Studio (SSMS), conéctese al servidor de base de datos de SQL Server Express y adjuntar la base de datos es la manera más fácil de lograr esto. Si no tiene instalado en el equipo SSMS puede descargar e instalar gratuitamente [ *versión de SQL Server 2008 Management Studio básica*](https://www.microsoft.com/downloads/details.aspx?FamilyId=7522A683-4CB2-454E-B908-E805E9BD4E28&amp;displaylang=en).


Además de seleccionar las bases de datos para comparar, también puede especificar diversas opciones de configuración de la comparación de la pestaña Opciones. Una opción que desea activar es el "Omitir restricción e índice nombres". Recuerde que en el tutorial anterior hemos agregado que la aplicación de servicios de objetos de base de datos para las bases de datos de desarrollo y producción. Si ha usado la `aspnet_regsql.exe` herramienta para crear estos objetos en la base de datos de producción, a continuación, encontrará que la clave principal y los nombres de restricción unique difieren entre las bases de datos de desarrollo y producción. Por lo tanto, comparar SQL marcará todas las tablas de servicios de aplicación como distinto. O bien puede dejar los "Omitir restricción e índice nombres" desactivada y sincronizar los nombres de restricción, o indicar a SQL Compare para pasar por alto estas diferencias.

Después de seleccionar las bases de datos para comparar (y revisar las opciones de comparación), haga clic en el botón Comparar ahora para iniciar la comparación. A través de los segundos siguientes, comparar SQL examina los esquemas de las dos bases de datos y genera un informe de cómo se diferencian. Se ha realizado intencionadamente modificaciones a la base de datos de desarrollo para mostrar cómo se indican estos tipos de discrepancias en la interfaz de comparación de SQL. Tal y como se muestra en la figura 2, se ha agregado un `BirthDate` columna a la `Authors` tabla, quitar la `ISBN` columna desde la `Books` tabla y agrega una nueva tabla, `Ratings`, que está diseñado para permitir que los usuarios que visitan los libros revisados de la tasa de sitio.

> [!NOTE]
> Se realizaron los cambios del modelo de datos realizados en este tutorial para ilustrar el uso de una herramienta de comparación de la base de datos. No encontrará estos cambios en la base de datos en tutoriales futuros.


[![Comparación SQL enumera las diferencias entre el desarrollo y las bases de datos de producción](strategies-for-database-development-and-deployment-cs/_static/image5.jpg)](strategies-for-database-development-and-deployment-cs/_static/image4.jpg)

**Figura 2**: comparación SQL enumera las diferencias entre el desarrollo y las bases de datos de producción ([haga clic aquí para ver la imagen a tamaño completo](strategies-for-database-development-and-deployment-cs/_static/image6.jpg))


Comparación de SQL desglosa los objetos de base de datos en grupos, rápidamente que muestra los objetos existen en ambas bases de datos, pero son diferente, que los objetos existen en una base de datos pero no en el otro y qué objetos son idénticos. Como puede ver, hay dos objetos que existen en ambas bases de datos, pero son diferentes: el `Authors` tabla, que tenía una columna agregada, y el `Books` tabla, lo que tenía uno quitado. Hay un objeto que solo existe en la base de datos de desarrollo, es decir, creada recientemente `Ratings` tabla. Y hay 117 objetos que son idénticos en ambas bases de datos.

Al seleccionar un objeto de base de datos, muestra la ventana de diferencias de SQL, que muestra la diferencia entre estos objetos. La ventana de diferencias de SQL, en la parte inferior en la figura 2, que resalta el `Authors` tabla en la base de datos de desarrollo tiene la `BirthDate` columna, que no se encuentra en la `Authors` tabla en la base de datos de producción.

Después de revisar las diferencias y seleccionar los objetos que desea sincronizar, el paso siguiente consiste en generar los comandos SQL necesarios para actualizar el esquema de s de base de datos de producción para que coincida con la base de datos de desarrollo. Esto se consigue mediante el Asistente para sincronización. El Asistente para sincronización Confirma qué objetos se deben para sincronizar y resume la acción plan (consulte la figura 3). Puede sincronizar las bases de datos inmediatamente o generar un script con los comandos SQL que se pueden ejecutar en su tiempo libre.


[![Utilice al Asistente para sincronización para sincronizar los esquemas de bases de datos](strategies-for-database-development-and-deployment-cs/_static/image8.jpg)](strategies-for-database-development-and-deployment-cs/_static/image7.jpg)

**Figura 3**: utilizar el Asistente de sincronización para sincronizar los esquemas de bases de datos ([haga clic aquí para ver la imagen a tamaño completo](strategies-for-database-development-and-deployment-cs/_static/image9.jpg))


Herramientas de comparación de la base de datos, como Red Gate Software s comparación de SQL que aplica los cambios en el esquema de base de datos de desarrollo para la base de datos de producción tan sencillo como señalar y haga clic en.

> [!NOTE]
> Comparación de SQL compara y sincroniza dos bases de datos *esquemas*. Desafortunadamente, comparar y sincronizar los datos en tablas de dos bases de datos. Red Gate Software ofrece un producto denominado [ *comparación de datos de SQL* ](http://www.red-gate.com/products/SQL_Data_Compare/) que compara y sincronice los datos entre dos bases de datos, pero es un producto independiente de comparación de SQL y el precio es otro 395 dólares.


## <a name="taking-the-application-offline-during-deployment"></a>Poner la aplicación sin conexión durante la implementación

Como se ha visto a lo largo de estos tutoriales, la implementación es un proceso que implica varios pasos: copiar las páginas ASP.NET, páginas maestras, archivos CSS, archivos de JavaScript base, imágenes y otro contenido necesario desde el entorno de desarrollo a producción entorno; copiar la información de configuración específicos del entorno de producción, si es necesario; y aplica los cambios en el modelo de datos desde la última implementación. Según el número de archivos y la complejidad de los cambios de la base de datos, estos pasos pueden tardar en cualquier parte desde unos segundos hasta varios minutos en completarse. Durante este período de la aplicación web es un flujo y los usuarios que visitan el sitio pueden experimentar errores o un comportamiento inesperado.

Al implementar un sitio Web es mejor desconectar la aplicación web "" hasta que se ha completado la implementación. Desconectar de la aplicación (y ponerlos realizar copias de seguridad una vez que ha finalizado el proceso de implementación) es tan fácil como cargar un archivo y, a continuación, eliminarlo. A partir de ASP.NET 2.0, la mera presencia de un archivo denominado `app_offline.htm` en las operaciones de asignación aplicación directorio raíz tiene todo el sitio Web "sin conexión". Cualquier solicitud para una página ASP.NET en el sitio se respondió automáticamente con el contenido de la `app_offline.htm` archivo. Una vez que se quite ese archivo, la aplicación vuelve a estar conectada.

Poner una aplicación sin conexión durante la implementación, a continuación, es tan sencillo como cargar un `app_offline.htm` archivos en el entorno de producción s directorio raíz antes de comenzar el proceso de implementación y, a continuación, eliminarlo (o cambiarle el nombre a otra cosa) una vez implementación está completa. Para obtener más información sobre esta técnica, consulte al artículo de John Peterson s, teniendo un [ *aplicación de ASP.NET sin conexión*](http://www.15seconds.com/issue/061207.htm).

## <a name="summary"></a>Resumen

El desafío más importante para implementar una aplicación controlada por datos se centra en la implementación de la base de datos. Dado que hay dos versiones de la base de datos, uno en el entorno de desarrollo y uno en el entorno de producción estos esquemas de dos bases de datos pueden dejen de estar sincronizados cuando se agregan nuevas características de desarrollo. ¿Qué más, dado que la base de datos de producción como que se rellena con datos reales de los usuarios reales, no se puede sobrescribir la base de datos de producción con la base de datos de desarrollo modificadas igual que al implementar los archivos que componen la aplicación (las páginas ASP.NET, s archivos de imagen y así sucesivamente). En su lugar, la implementación de una base de datos conlleva implementar el conjunto de cambios realizados en la base de datos de desarrollo en la base de datos de producción desde la última implementación.

Este tutorial examinando tres técnicas para mantener y aplicar un registro de cambios de base de datos. El enfoque más sencillo consiste en registrar los cambios en el texto. Mientras esta táctica facilita la implementación de estos cambios en la base de datos de producción un proceso manual, no requieren conocimientos de los comandos SQL para crear y modificar objetos de base de datos. Un enfoque más sofisticado y otro que es mucho más agradable ya en proyectos o proyectos más grandes con varios desarrolladores, consiste en registrar los cambios como una serie de comandos SQL. Esto en gran medida apresura implementando estos cambios en la base de datos de destino. Lo mejor de ambos enfoques se puede lograr mediante el uso de una herramienta de comparación de la base de datos, como s de Red Gate Software comparación de SQL.

Este tutorial concluye nuestro enfoque sobre la implementación de una aplicación controlada por datos. El siguiente conjunto de tutoriales examina cómo responder a errores en el entorno de producción. Analizaremos cómo mostrar una página de error descriptivo en su lugar en lugar de la pantalla de muerte amarillo. Y veremos cómo registrar los detalles de error s y que le avise cuando se producen estos errores.

Feliz programación.

> [!div class="step-by-step"]
> [Anterior](configuring-a-website-that-uses-application-services-cs.md)
> [Siguiente](displaying-a-custom-error-page-cs.md)
