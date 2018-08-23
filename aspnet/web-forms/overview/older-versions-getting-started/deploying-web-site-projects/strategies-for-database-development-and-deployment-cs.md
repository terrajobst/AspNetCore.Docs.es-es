---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-cs
title: Las estrategias de desarrollo de base de datos y la implementación (C#) | Microsoft Docs
author: rick-anderson
description: Al implementar una aplicación controlada por datos por primera vez ciegamente puede copiar la base de datos en el entorno de desarrollo al entorno de producción. B...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: 3e8b0627-3eb7-488e-807e-067cba7cec05
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-cs
msc.type: authoredcontent
ms.openlocfilehash: 029c17d5f996f595aef1af68c3ddb9b21f99f5cf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837206"
---
<a name="strategies-for-database-development-and-deployment-c"></a>Estrategias de desarrollo de base de datos y la implementación (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial10_DBDevel_cs.pdf)

> Al implementar una aplicación controlada por datos por primera vez ciegamente puede copiar la base de datos en el entorno de desarrollo al entorno de producción. Pero realizar un blind copia en implementaciones posteriores sobrescribirán los datos introducidos en la base de datos de producción. En su lugar, la implementación de una base de datos implica aplicar los cambios realizados en la base de datos de desarrollo desde la última implementación en la base de datos de producción. En este tutorial se examina estos desafíos y ofrece varias estrategias para ayudar a que relata y aplicar los cambios realizados en la base de datos desde la última implementación.


## <a name="introduction"></a>Introducción

Como se describe en los tutoriales anteriores, implementar una aplicación ASP.NET implica copiar el contenido pertinente desde el entorno de desarrollo al entorno de producción. Implementación no es un evento único, pero en su lugar algo que ocurre cada vez que se lanza una nueva versión del software, o cuando los errores o problemas de seguridad se han identificado y solucionado. Cuando se copian las páginas ASP.NET, imágenes, archivos JavaScript y otros archivos de este tipo en el entorno de producción que no es necesario preocuparse de cómo estos archivos se cambiaron desde la última implementación. Ciegamente puede copiar el archivo a producción, sobrescribiendo el contenido existente. Lamentablemente, esta sencillez no se extiende a la implementación de la base de datos.

Al implementar una aplicación controlada por datos por primera vez ciegamente puede copiar la base de datos en el entorno de desarrollo al entorno de producción. Pero realizar un blind copia en implementaciones posteriores sobrescribirán los datos introducidos en la base de datos de producción. En su lugar, la implementación de una base de datos implica aplicar el *cambios* realizados en la base de datos de desarrollo desde la última implementación en la base de datos de producción. En este tutorial se examina estos desafíos y ofrece varias estrategias para ayudar a que relata y aplicar los cambios realizados en la base de datos desde la última implementación.

## <a name="the-challenges-of-deploying-a-database"></a>Los desafíos de la implementación de una base de datos

Antes de que se ha implementado una aplicación controlada por datos por primera vez, hay solo una base de datos, es decir, la base de datos en el entorno de desarrollo, lo que al implementar una aplicación controlada por datos por primera vez ciegamente puede copiar la base de datos en el entorno de desarrollo al entorno de producción. Sin embargo, una vez que se ha implementado la aplicación hay dos copias de la base de datos: uno en uno de producción y desarrollo.

Entre las implementaciones de las bases de datos de desarrollo y producción pueden dejen de estar sincronizados. Mientras permanece sin modificar el esquema s de base de datos de producción, puede cambiar el esquema s de base de datos de desarrollo cuando se agregan nuevas características. Puede agregar o quitar columnas, tablas, vistas o procedimientos almacenados. También puede haber datos importantes que se agregan a la base de datos de desarrollo. Muchas aplicaciones controladas por datos incluyen tablas de búsqueda que se rellena con datos codificados de forma rígida, específica de la aplicación que no se puede modificar el usuario. Por ejemplo, un sitio Web de subasta podría tener una lista desplegable con las opciones que describen el estado del elemento que se contemple la subasta: nuevo, como nuevo, bueno y justo. En lugar de codificar estas opciones directamente en la lista desplegable es normalmente lo mejor colocarlos en una tabla de base de datos. Si, durante el desarrollo, una nueva condición denominada a deficiente se agrega a la tabla, a continuación, al implementar la aplicación en este mismo registro debe agregarse a la tabla de búsqueda en la base de datos de producción.

Idealmente, la implementación de la base de datos implicaría copiar la base de datos de desarrollo a producción. Pero tenga en cuenta que después de haber implementado la aplicación y reanudar el desarrollo, la base de datos de producción se rellena con datos reales de usuarios reales. Por lo tanto, si tuviera que simplemente copiar la base de datos de desarrollo a producción en la siguiente implementación de sobrescribir la base de datos de producción y perder los datos existentes. El resultado neto es que la implementación de la base de datos se reduce a aplicar los cambios realizados en la base de datos de desarrollo desde la última implementación.

Dado que la implementación de una base de datos implica aplicar los cambios en el esquema y, posiblemente, los datos desde la última implementación, un historial de cambios debe ser mantiene (o puede determinar en tiempo de implementación) para que se puedan aplicar esos cambios en producción. Hay una variedad de técnicas para administrar y aplicar los cambios al modelo de datos.

### <a name="defining-the-baseline"></a>Definición de la línea de base

Para mantener los cambios en la base de datos de aplicación s deberá tener algún estado inicial, una línea base a los que se aplican los cambios. En un extremo, el estado inicial podría ser una base de datos vacía con ningún tablas, vistas o procedimientos almacenados. Una línea base de este tipo da como resultado un registro de cambios grandes porque debe incluir la creación de todas las tablas de base de datos s, vistas y procedimientos almacenados junto con los cambios realizados después de la implementación inicial. En el otro extremo del espectro se pudo establecer la línea de base como la versión de la base de datos que se implementa inicialmente en el entorno de producción. Esta opción da como resultado un registro de cambios mucho más pequeño porque solo incluye los cambios realizados en la base de datos después de la primera implementación. Este es el enfoque que prefiera. Y por supuesto puede elegir un medio más del enfoque de carretera, definir la línea de base como en algún momento entre la creación inicial de la base de datos y se implementa por primera vez cuando la base de datos.

Una vez haya elegido una línea de base puede ser conveniente generar una secuencia de comandos SQL que se puede ejecutar para volver a crear la versión de línea de base. Este script permite volver rápidamente a la versión de línea de base de la base de datos. Esta funcionalidad es especialmente útil en proyectos grandes, donde puede haber varios desarrolladores que trabajan en el proyecto o entornos adicionales, como una prueba o ensayo, que cada uno tenga su propia copia de la base de datos.

Hay una variedad de herramientas a su disposición para generar un script SQL de la versión de referencia. Desde SQL Server Management Studio (SSMS) puede hacer clic en la base de datos, vaya al submenú de tareas y elija la opción Generar Scripts. Esto inicia al Asistente para la secuencia de comandos, que puede indicar a generar un archivo que contiene los comandos SQL para crear objetos de s de la base de datos. Otra opción es el Asistente para publicación de base de datos, lo que puede generar los comandos SQL para crear no solo el esquema de base de datos, sino también los datos en las tablas de base de datos. El Asistente para publicación de base de datos se describe detalladamente en la *implementar una base de datos* tutorial. Independientemente de la herramienta que utilice, al final debe tener un archivo de script que puede usar para volver a crear la versión de línea de base de la base de datos, debe la necesidad de surgir.

## <a name="documenting-the-database-changes-in-prose"></a>Documentar los cambios de la base de datos en texto

Es la manera más sencilla para mantener un registro de cambios al modelo de datos durante la fase de desarrollo registrar los cambios en texto. Por ejemplo, si agrega una nueva columna a durante el desarrollo de una aplicación ya ha implementado el `Employees` de tabla, quitar una columna de la `Orders` de tabla y agregar una nueva tabla (`ProductCategories`), podría mantener un archivo de texto o un documento de Microsoft Word con el historial de la siguiente:

<a id="0.4_table01"></a>


| **Fecha de cambio** | **Cambiar detalles** |
| --- | --- |
| 2009-02-03: | Columna agregada `DepartmentID` (`int`, no es NULL) para el `Employees` tabla. Agrega una restricción foreign key de `Departments.DepartmentID` a `Employees.DepartmentID`. |
| 2009-02-05: | Columna quitada `TotalWeight` desde el `Orders` tabla. Asociados de datos ya está capturados en `OrderDetails` registros. |
| 2009-02-12: | Crea el `ProductCategories` tabla. Hay tres columnas: `ProductCategoryID` (`int`, `IDENTITY`, `NOT NULL`), `CategoryName` (`nvarchar(50)`, `NOT NULL`), y `Active` (`bit`, `NOT NULL`). Agrega una restricción primary key a `ProductCategoryID`y un valor predeterminado de 1 a `Active`. |


Existen varios inconvenientes de este enfoque. Para empezar, no hay ninguna esperanza para la automatización. En cualquier momento estos cambios deben aplicarse a una base de datos -, como cuando se implementa la aplicación: un desarrollador deberá implementar manualmente cada uno de ellos cambia, una vez. Además, si necesita reconstruir una versión concreta de la base de datos desde la línea de base mediante el registro de cambios, si lo hace por lo que tardará más tiempo a medida que crece el tamaño del registro. Otro inconveniente de este método es que la claridad y el nivel de detalle de cada entrada de registro de cambios se deja a la persona que registrar el cambio. En un equipo con varios desarrolladores algunos pueden realizar entradas más detalladas, más legibles o más precisas que otras. Además, son posibles errores tipográficos y otros errores de entrada de datos relacionados con humanos.

La principal ventaja de documentar los cambios de la base de datos en texto es la simplicidad. Cuando no está familiarizado de necesidad de t con la sintaxis SQL para crear y modificar objetos de base de datos. En su lugar, puede registrar los cambios en prose e implementarlos a través de la interfaz gráfica de usuario de SQL Server Management Studio s.

Mantener el registro de cambios en texto, es verdad, no es muy sofisticado y logradas funcionar bien con determinados proyectos, por ejemplo, si son grandes en el ámbito, tiene cambios frecuentes en el modelo de datos o implicar varios desarrolladores. Pero he visto este enfoque funciona bastante bien en proyectos pequeños, solo miembro que tienen sólo realicen cambios en el modelo de datos y donde el único desarrollador no tiene un gran conocimiento de la sintaxis SQL para crear y modificar objetos de base de datos.

> [!NOTE]
> Mientras que la información en el registro de cambios es, técnicamente, solo es necesario hasta que el tiempo de implementación, recomienda mantener un historial de cambios. Pero en vez de mantener una sola, crece constantemente el archivo de registro de cambios, considere la posibilidad de tener un archivo de registro de cambio diferente para cada versión de la base de datos. Normalmente, le interesará a la versión de la base de datos cada vez que se implementa. Al mantener un registro de los registros de cambio puede, a partir de la línea base, volver a cualquier versión de la base de datos mediante la ejecución de las secuencias de comandos de cambio de registro a partir de la versión 1 y continuando hasta llegar a la versión debe volver a crear.


## <a name="recording-the-sql-change-statements"></a>Grabación de las instrucciones de cambio SQL

El principal inconveniente de mantener el registro de cambios en texto es la falta de automatización. Idealmente, la implementación de los cambios de la base de datos en la base de datos de producción en tiempo de implementación sería tan fácil como hacer clic en un botón para ejecutar un script en lugar de tener que realizar manualmente una lista de instrucciones. Tal automatización es posible por mantener un registro de cambios que contiene los comandos SQL que se usa para modificar el modelo de datos.

La sintaxis SQL incluye una serie de instrucciones para crear y modificar distintos objetos de base de datos. Por ejemplo, el [ *instrucción CREATE TABLE*](https://msdn.microsoft.com/library/ms174979.aspx), cuando se ejecuta, crea una nueva tabla con las restricciones y las columnas especificadas. El [ *instrucción ALTER TABLE* ](https://msdn.microsoft.com/library/ms190273.aspx) modifica una tabla existente, agregar, quitar o modificar sus columnas o restricciones. También existen instrucciones para crear, modificar y quitar índices, vistas, funciones definidas por el usuario, procedimientos almacenados, desencadenadores y otros objetos de base de datos.

Volviendo a nuestro ejemplo anterior, que la imagen durante el desarrollo de una aplicación ya implementadas, agregar una nueva columna a la `Employees` de tabla, quitar una columna de la `Orders` de tabla y agregar una nueva tabla (`ProductCategories`). Estas acciones daría como resultado en un archivo de registro de cambios con los siguientes comandos SQL:

[!code-sql[Main](strategies-for-database-development-and-deployment-cs/samples/sample1.sql)]

Inserción de estos cambios en la base de datos de producción en tiempo de implementación es una operación de un solo clic: Abra SQL Server Management Studio, conectarse a la base de datos de producción, abra una ventana nueva consulta, pegue el contenido del registro de cambios y haga clic en ejecutar para ejecutar el script.

## <a name="using-a-comparison-tool-to-synchronize-the-data-models"></a>Uso de una herramienta de comparación para sincronizar los modelos de datos

Documentar los cambios de la base de datos en texto es fácil, pero los cambios de implementación requiere un desarrollador tomar cada cambio en la base de datos de producción uno cada vez; Documentar los comandos de cambio SQL facilita la implementación de esos cambios en la base de datos de producción lo más fácil y rápido como hacer clic en un botón, pero requiere aprender y dominar las instrucciones SQL y sintaxis para crear y modificar objetos de base de datos. Herramientas de comparación de la base de datos toman lo mejor de ambos enfoques y descartar la peor.

Una herramienta de comparación de la base de datos compara el esquema o cuyos datos de dos bases de datos y muestra un informe de resumen que muestra cómo difieren de las bases de datos. A continuación, con sólo pulsar un botón, puede generar los comandos SQL para la sincronización de uno o más objetos de base de datos. En pocas palabras, puede usar una herramienta de comparación de la base de datos para comparar el desarrollo y los comandos de las bases de datos de producción en tiempo de implementación, generar un archivo que contiene la instrucción SQL que, cuando se ejecute, se aplicarán los cambios en el esquema de base de datos s de producción por lo que ese TI refleja el esquema de base de datos s de desarrollo.

Hay una variedad de herramientas de comparación de base de datos de terceros ofrecidas por distintos proveedores. Un ejemplo es [ *SQL Compare*](http://www.red-gate.com/products/SQL_Compare/), de manera [ *Red Gate Software*](http://www.red-gate.com/). Permiten s recorra el proceso de uso de SQL Compare para comparar y sincronizar los esquemas de bases de datos de desarrollo y producción.

> [!NOTE]
> En el momento de redactar este artículo la versión actual de SQL Compare era la versión 7.1, con la edición Standard valoración 395 dólares. Puede seguir descargando una evaluación gratuita de 14 días.


Cuando se inicia SQL Compare abre el cuadro de diálogo de proyectos de comparación, que muestra los proyectos de SQL Compare guardados. Cree un nuevo proyecto. Esto inicia el Asistente de configuración del proyecto, que solicita información acerca de las bases de datos para comparar (consulte la figura 1). Escriba la información de las bases de datos de entorno de desarrollo y producción.


[![Comparar el desarrollo y las bases de datos de producción](strategies-for-database-development-and-deployment-cs/_static/image2.jpg)](strategies-for-database-development-and-deployment-cs/_static/image1.jpg)

**Figura 1**: comparar el desarrollo y las bases de datos de producción ([haga clic aquí para ver imagen en tamaño completo](strategies-for-database-development-and-deployment-cs/_static/image3.jpg))


> [!NOTE]
> Si la base de datos del entorno de desarrollo es un archivo de base de datos de SQL Express Edition en el `App_Data` carpeta del sitio Web deberá registrar la base de datos en el servidor de base de datos de SQL Server Express para poder seleccionarlo en el cuadro de diálogo se muestra en la figura 1. La manera más fácil de hacerlo es abrir SQL Server Management Studio (SSMS), conectar con el servidor de base de datos de SQL Server Express y adjuntar la base de datos. Si no tiene SSMS instalado en el equipo puede descargar e instalar la versión gratuita [ *versión de SQL Server 2008 Management Studio Basic*](https://www.microsoft.com/downloads/details.aspx?FamilyId=7522A683-4CB2-454E-B908-E805E9BD4E28&amp;displaylang=en).


Además de seleccionar las bases de datos para comparar, también puede especificar una variedad de opciones de comparación de la ficha Opciones. Una opción es posible que desee activar es la "Omitir restricción e índice nombres". Recuerde que en el tutorial anterior se ha agregado que la aplicación de servicios de objetos de base de datos para las bases de datos de desarrollo y producción. Si ha usado el `aspnet_regsql.exe` herramienta para crear estos objetos en la base de datos de producción, a continuación, encontrará que la clave principal y los nombres de restricción unique difieren entre las bases de datos de desarrollo y producción. Por lo tanto, SQL Compare marcará todas las tablas de servicios de aplicación como distintos. O bien puede dejar los "Omitir restricción e índice nombres" desactivada y sincronizar los nombres de restricción, o indicar SQL Compare para omitir estas diferencias.

Después de seleccionar las bases de datos para comparar (y revisar las opciones de comparación), haga clic en el botón Comparar ahora para iniciar la comparación. A través de los segundos siguientes, SQL Compare examina los esquemas de las dos bases de datos y genera un informe de qué se diferencian. Se ve intencionadamente realiza algunas modificaciones en la base de datos de desarrollo para mostrar cómo se indica el tipo de discrepancias en la interfaz de comparación de SQL. Como se muestra en la figura 2, puedo ve agrega un `BirthDate` columna a la `Authors` tabla, se quitan el `ISBN` columna desde la `Books` de tabla y agrega una nueva tabla, `Ratings`, que está pensado para permitir que los usuarios que visitan los libros revisados de la tasa de sitio.

> [!NOTE]
> Se realizaron los cambios realizados en este tutorial del modelo de datos para ilustrar el uso de una herramienta de comparación de la base de datos. No encontrará estos cambios en la base de datos en tutoriales futuros.


[![Comparación SQL enumera las diferencias entre el desarrollo y las bases de datos de producción](strategies-for-database-development-and-deployment-cs/_static/image5.jpg)](strategies-for-database-development-and-deployment-cs/_static/image4.jpg)

**Figura 2**: comparación SQL enumera las diferencias entre el desarrollo y las bases de datos de producción ([haga clic aquí para ver imagen en tamaño completo](strategies-for-database-development-and-deployment-cs/_static/image6.jpg))


SQL Compare desglosan todos los objetos de base de datos en grupos, mostrar rápidamente los objetos que existen en ambas bases de datos, pero son diferente, que los objetos existen en una base de datos, pero no el otro y qué objetos son idénticos. Como puede ver, hay dos objetos que existen en ambas bases de datos, pero son diferentes: el `Authors` tabla, que tenía que agregar una columna, y el `Books` tabla, lo que ha tenido quitado. Hay un objeto que solo existe en la base de datos de desarrollo, es decir, el recién creado `Ratings` tabla. Y hay 117 objetos que son idénticos en ambas bases de datos.

Seleccionar un objeto de base de datos muestra la ventana de diferencias de SQL, que se muestra cómo se diferencian estos objetos. La ventana de diferencias de SQL, se muestra en la parte inferior en la figura 2, que resalta el `Authors` tabla en la base de datos de desarrollo tiene el `BirthDate` columna, que no se encuentra en la `Authors` tabla en la base de datos de producción.

Después de revisar las diferencias y seleccionar los objetos que desea sincronizar, el siguiente paso es generar los comandos SQL necesarios para actualizar el esquema s de base de datos de producción para que coincida con la base de datos de desarrollo. Esto se logra mediante el Asistente para la sincronización. El Asistente para sincronización Confirma qué objetos se deben para sincronizar y resume la acción previsto (consulte la figura 3). Puede sincronizar las bases de datos inmediatamente o generar un script con los comandos SQL que se pueden ejecutar en su tiempo libre.


[![Utilice al Asistente para la sincronización para sincronizar los esquemas de bases de datos](strategies-for-database-development-and-deployment-cs/_static/image8.jpg)](strategies-for-database-development-and-deployment-cs/_static/image7.jpg)

**Figura 3**: usar el Asistente para la sincronización para sincronizar los esquemas de bases de datos ([haga clic aquí para ver imagen en tamaño completo](strategies-for-database-development-and-deployment-cs/_static/image9.jpg))


Herramientas de comparación de la base de datos, como s SQL Compare Asegúrese de aplicar los cambios al esquema de base de datos de desarrollo para la base de datos de producción tan sencillo como señalar y haga clic en Red Gate Software.

> [!NOTE]
> Comparación de SQL compara y sincroniza dos bases de datos *esquemas*. Lamentablemente, no comparar y sincronizar los datos dentro de las tablas de dos bases de datos. Red Gate Software ofrece un producto denominado [ *SQL Compare datos* ](http://www.red-gate.com/products/SQL_Data_Compare/) que compara y sincroniza los datos entre dos bases de datos, pero es un producto independiente de SQL Compare y cuesta otro 395 dólares.


## <a name="taking-the-application-offline-during-deployment"></a>Poner la aplicación sin conexión durante la implementación

Como se ve, visto a lo largo de estos tutoriales, la implementación es un proceso que implica varios pasos: copiar las páginas ASP.NET, las páginas maestras, CSS archivos, JavaScript archivos, imágenes y otros contenidos necesarios desde el entorno de desarrollo para la producción entorno; copia de seguridad de la información de configuración específicos del entorno de producción, si es necesario; y aplicar los cambios al modelo de datos desde la última implementación. Según el número de archivos y la complejidad de los cambios de la base de datos, estos pasos pueden tardar desde unos segundos hasta varios minutos en completarse. Durante esta ventana de la aplicación web es inestable y los usuarios que visitan el sitio pueden experimentar errores o un comportamiento inesperado.

Al implementar un sitio Web es mejor desconectar la aplicación web "" hasta que finalice la implementación. Desconectar la aplicación (y ponerla copia de seguridad una vez finalizado el proceso de implementación) es tan sencillo como cargar un archivo y, a continuación, eliminarlo. A partir de ASP.NET 2.0, la mera presencia de un archivo denominado `app_offline.htm` s de la aplicación en el directorio raíz toma todo el sitio Web "sin conexión". Cualquier solicitud a una página ASP.NET en el sitio se respondió automáticamente con el contenido de la `app_offline.htm` archivo. Una vez que se quita ese archivo, la aplicación vuelve a conectarse.

Tomar una aplicación sin conexión durante la implementación, a continuación, es tan sencillo como cargar un `app_offline.htm` archivo en el entorno de producción s directorio raíz antes de comenzar el proceso de implementación y, a continuación, eliminarlo (o al cambiar el nombre a otra cosa) una vez implementación completada. Para obtener más información sobre esta técnica consulte el artículo de John Peterson s, teniendo un [ *aplicación ASP.NET sin conexión*](http://www.15seconds.com/issue/061207.htm).

## <a name="summary"></a>Resumen

El desafío principal en la implementación de una aplicación controlada por datos se centra en la implementación de la base de datos. Dado que hay dos versiones de la base de datos, uno en el entorno de desarrollo y uno en el entorno de producción estos esquemas de dos bases de datos pueden dejen de estar sincronizados cuando se agregan nuevas características en desarrollo. ¿Qué más, dado que la base de datos de producción como que se rellena con datos reales de usuarios reales, no se puede sobrescribir la base de datos de producción con la base de datos de desarrollo modificadas igual que al implementar los archivos que componen la aplicación (las páginas ASP.NET, s archivos de imagen y así sucesivamente). En su lugar, la implementación de una base de datos implica implementar el conjunto preciso de los cambios realizados en la base de datos de desarrollo en la base de datos de producción desde la última implementación.

En este tutorial examinado tres técnicas para mantener y aplicar un registro de cambios de la base de datos. Es el enfoque más sencillo registrar los cambios en texto. Mientras esta táctica facilita la implementación de estos cambios en la base de datos de producción, un proceso manual, no requieren conocimientos de los comandos SQL para crear y modificar objetos de base de datos. Un enfoque más sofisticado y otro que es mucho más aceptable en proyectos o proyectos más grandes con varios desarrolladores consiste en registrar los cambios como una serie de comandos SQL. Esto agiliza en gran medida notablemente implementando estos cambios en la base de datos de destino. Lo mejor de ambos enfoques se consigue mediante una herramienta de comparación de la base de datos, como s Red Gate Software SQL Compare.

En este tutorial concluye nuestro enfoque en la implementación de una aplicación controlada por datos. El siguiente conjunto de tutoriales explica cómo responder a errores en el entorno de producción. Echemos un vistazo cómo mostrar una página de error descriptivo en su lugar, en lugar de la pantalla amarilla de la muerte. Y veremos cómo registrar los detalles del error s y cómo le alerte cuando se producen estos errores.

Feliz programación.

> [!div class="step-by-step"]
> [Anterior](configuring-a-website-that-uses-application-services-cs.md)
> [Siguiente](displaying-a-custom-error-page-cs.md)
