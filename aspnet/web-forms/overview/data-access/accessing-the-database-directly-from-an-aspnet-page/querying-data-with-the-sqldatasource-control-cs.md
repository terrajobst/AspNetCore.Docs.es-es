---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs
title: Consultar datos con el Control SqlDataSource (C#) | Microsoft Docs
author: rick-anderson
description: En los tutoriales anteriores, hemos usado el control ObjectDataSource para separar completamente la capa de presentación de la capa de acceso a datos. A partir de este tutor...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: 60512d6a-b572-4b7a-beb3-3e44b4d2020c
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 0d15e09c2b790c4d1e6b278c4ea35bab7f66b861
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829441"
---
<a name="querying-data-with-the-sqldatasource-control-c"></a>Consultar datos con el Control SqlDataSource (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_CS.exe) o [descargar PDF](querying-data-with-the-sqldatasource-control-cs/_static/datatutorial47cs1.pdf)

> En los tutoriales anteriores, hemos usado el control ObjectDataSource para separar completamente la capa de presentación de la capa de acceso a datos. A partir de este tutorial, se obtenga información sobre cómo se puede usar el control SqlDataSource para aplicaciones simples que no requieren una estricta separación de la presentación y el acceso a datos.


## <a name="introduction"></a>Introducción

Todos los tutoriales se ve examinar hasta ahora ha usado una arquitectura en niveles que consta de presentación, lógica de negocios y niveles de acceso a datos. La capa de acceso a datos (DAL) se ha diseñado en el primer tutorial ([crear una capa de acceso a datos](../introduction/creating-a-data-access-layer-cs.md)) y la capa de lógica empresarial en el segundo ([crear una capa de lógica empresarial](../introduction/creating-a-business-logic-layer-cs.md)). A partir de la [mostrar datos con el origen ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) tutorial, vimos cómo usar el control nuevo de ObjectDataSource de ASP.NET 2.0 s para interactuar mediante declaración con la arquitectura de la capa de presentación.

Mientras todos los tutoriales hasta ahora han usado la arquitectura para trabajar con datos, también es posible tener acceso, insertar, actualizar y eliminar la base de datos directamente desde una página ASP.NET, omitiendo la arquitectura. Al hacerlo, coloca las consultas de base de datos específica y la lógica de negocios directamente en la página web. Para las aplicaciones lo suficientemente grandes o complejas, diseñar, implementar y usar una arquitectura en capas es de vital importancia para el éxito, actualización y mantenimiento de la aplicación. Desarrollar una arquitectura robusta, sin embargo, puede ser necesario al crear aplicaciones excepcionales y sumamente simples.

ASP.NET 2.0 proporciona controles de origen de datos integrados cinco [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx), y [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx). SqlDataSource puede utilizarse para tener acceso y modificar datos directamente desde una base de datos relacional, incluido Microsoft SQL Server, Microsoft Access, Oracle, MySQL y otros usuarios. En este tutorial y los tres siguientes, examinaremos cómo trabajar con el control SqlDataSource, explorar cómo consultar y filtrar los datos de base de datos, así como el uso de SqlDataSource para insertar, actualizar y eliminar datos.


![ASP.NET 2.0 incluye cinco controles de origen de datos integrados](querying-data-with-the-sqldatasource-control-cs/_static/image1.gif)

**Figura 1**: ASP.NET 2.0 incluye cinco controles de origen de datos integrados


## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>Comparación de ObjectDataSource y SqlDataSource

Conceptualmente, el origen ObjectDataSource y SqlDataSource controles son simplemente los servidores proxy a los datos. Como se describe en el [mostrar datos con el origen ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) tutorial, el origen ObjectDataSource tiene propiedades que indican el tipo de objeto que proporciona los datos y los métodos que se invocará para seleccionar, insertar, actualizar y eliminar datos desde el tipo de objeto subyacente. Una vez que se han configurado las propiedades de s ObjectDataSource, se pueden enlazar un control Web como un control GridView, DetailsView o DataList de datos al control, mediante la s ObjectDataSource `Select()`, `Insert()`, `Delete()`, y `Update()` métodos para interactuar con la arquitectura subyacente.

SqlDataSource proporciona la misma funcionalidad, pero funciona con una base de datos relacional en lugar de una biblioteca de objetos. Con SqlDataSource, se debe especificar la cadena de conexión de base de datos y las consultas SQL ad hoc o procedimientos almacenados para ejecutar para insertar, actualizar, eliminar y recuperar los datos. Las operaciones de asignación SqlDataSource `Select()`, `Insert()`, `Update()`, y `Delete()` métodos, cuando se invoca, conectarse a la base de datos especificada y emitir la consulta SQL adecuada. Como se ilustra en el diagrama siguiente, estos métodos realizan el trabajo de grunt de conectarse a una base de datos, emitir una consulta y devolver los resultados.


![SqlDataSource actúa como un Proxy para la base de datos](querying-data-with-the-sqldatasource-control-cs/_static/image2.gif)

**Figura 2**: SqlDataSource actúa como un Proxy para la base de datos


> [!NOTE]
> En este tutorial, nos centraremos en recuperar los datos de la base de datos. En el [Insertar, actualizar y eliminar el datos con el SqlDataSource Control](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md) tutorial, veremos cómo configurar SqlDataSource para admitir la inserción, actualización y eliminación.


## <a name="the-sqldatasource-and-accessdatasource-controls"></a>Los controles de AccessDataSource y SqlDataSource

Además del control SqlDataSource, ASP.NET 2.0 también incluye un control AccessDataSource. Estos dos tipos de controles conducir a muchos desarrolladores nuevas de ASP.NET 2.0 para sospechar que el control AccessDataSource está diseñado para trabajar exclusivamente con Microsoft Access con el control SqlDataSource diseñado para trabajar exclusivamente con Microsoft SQL Server. Aunque el AccessDataSource está diseñado para trabajar específicamente con Microsoft Access, el control SqlDataSource funciona con *cualquier* base de datos relacional que puede obtenerse a través. NET. Esto incluye cualquier compatibles con OLE DB o ODBC almacenes de datos, como Microsoft SQL Server, Microsoft Access, Oracle, Informix, MySQL y PostgreSQL, entre muchas otras.

La única diferencia entre los controles AccessDataSource y SqlDataSource es cómo se especifica la información de conexión de base de datos. El control AccessDataSource necesita simplemente la ruta de acceso al archivo de base de datos de Access. SqlDataSource, por otro lado, requiere una cadena de conexión completa.

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>Paso 1: Creación de las páginas Web de SqlDataSource

Antes de empezar a explorar cómo trabajar directamente con la base de datos mediante el control SqlDataSource, permiten s primero dedique un momento para crear las páginas ASP.NET en nuestro proyecto de sitio Web que necesitaremos para este tutorial y los tres siguientes. Empiece por agregar una nueva carpeta denominada `SqlDataSource`. A continuación, agregue las siguientes páginas ASP.NET a la carpeta y asegúrese de asociar cada página con el `Site.master` página maestra:

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`


![Agregar las páginas ASP.NET para los tutoriales relacionados con SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image3.gif)

**Figura 3**: agregar las páginas ASP.NET para los tutoriales relacionados con SqlDataSource


Al igual que en las demás carpetas `Default.aspx` en el `SqlDataSource` carpeta mostrará una lista de los tutoriales en su sección. Recuerde que el `SectionLevelTutorialListing.ascx` Control de usuario proporciona esta funcionalidad. Por lo tanto, agrega este Control de usuario a `Default.aspx` arrastrándolo desde el Explorador de soluciones en la página de vista de diseño de s.


[![Agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx](querying-data-with-the-sqldatasource-control-cs/_static/image5.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image4.gif)

**Figura 4**: agregar el `SectionLevelTutorialListing.ascx` Control de usuario `Default.aspx` ([haga clic aquí para ver imagen en tamaño completo](querying-data-with-the-sqldatasource-control-cs/_static/image6.gif))


Por último, agregue estas cuatro páginas como entradas para el `Web.sitemap` archivo. En concreto, agregue el siguiente marcado después de agregar botones de personalizado para los controles DataList y Repeater `<siteMapNode>`:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample1.sql)]

Después de actualizar `Web.sitemap`, dedique un momento para ver el sitio Web de tutoriales a través de un explorador. El menú de la izquierda ahora incluye elementos para la edición, inserción y eliminación de tutoriales.


![El mapa del sitio incluye ahora entradas para los tutoriales de SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image7.gif)

**Figura 5**: el mapa del sitio incluye ahora entradas para los tutoriales de SqlDataSource


## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>Paso 2: Agregar y configurar el Control SqlDataSource

Comience abriendo la `Querying.aspx` página en el `SqlDataSource` carpeta y cambie a la vista de diseño. Arrastre un control SqlDataSource desde el cuadro de herramientas hasta el diseñador y el conjunto de sus `ID` a `ProductsDataSource`. Al igual que con el origen ObjectDataSource, SqlDataSource no genera ninguna salida representada y, por tanto, aparece como un cuadro gris en la superficie de diseño. Para configurar SqlDataSource, haga clic en el vínculo Configurar origen de datos de la etiqueta inteligente de SqlDataSource s.


![Haga clic en la configuración de vínculo de origen de datos de la etiqueta inteligente de SqlDataSource s](querying-data-with-the-sqldatasource-control-cs/_static/image8.gif)

**Figura 6**: haga clic en la configuración de vínculo de origen de datos de la etiqueta inteligente de SqlDataSource s


Se abrirá el Asistente para configurar origen de datos de SqlDataSource control s. Aunque los pasos del asistente s difieren en el control ObjectDataSource s, el objetivo final es el mismo para proporcionar los detalles sobre cómo recuperar, insertar, actualizar y eliminar datos mediante el origen de datos. Para SqlDataSource esto conlleva especifica la base de datos subyacente para utilizar y proporcionar las instrucciones SQL ad hoc o procedimientos almacenados.

El primer paso del asistente nos pide la base de datos. La lista desplegable incluye esas bases de datos se encuentra en la s de la aplicación web `App_Data` carpeta y los que se han agregado en el nodo Conexiones de datos en el Explorador de servidores. Desde que se ha agregado ya una cadena de conexión para el `NORTHWIND.MDF` en la base de datos la `App_Data` carpeta a nuestro proyecto s `Web.config` archivo, la lista desplegable incluye una referencia a esa cadena de conexión, `NORTHWINDConnectionString`. Seleccione este elemento de la lista desplegable y haga clic en siguiente.


![Elija el NORTHWINDConnectionString en la lista desplegable](querying-data-with-the-sqldatasource-control-cs/_static/image9.gif)

**Figura 7**: elija la `NORTHWINDConnectionString` en la lista desplegable


Después de elegir la base de datos, el asistente pregunta para que la consulta devolver los datos. O bien podemos especificar las columnas de una tabla o vista a devolver o puede especificar una instrucción SQL personalizada o un procedimiento almacenado. Puede alternar entre esta elección mediante la especificación de una instrucción SQL personalizada o procedimiento almacenado y especificar columnas de una tabla o ver los botones de radio.

> [!NOTE]
> Para este primer ejemplo, permitir que s use la especificar columnas de una opción de tabla o vista. Se deberá volver al asistente más adelante en este tutorial y explorar la especificación de una opción de procedimiento almacenado o una instrucción SQL personalizada.


Figura 8 muestra la pantalla de la instrucción Select de la configuración cuando se selecciona el especificar columnas de un botón de radio de la tabla o vista. La lista desplegable contiene el conjunto de tablas y vistas de la base de datos Northwind, con la tabla seleccionada o columnas de vista s muestra en la casilla de verificación de la lista siguiente. En este ejemplo, s permiten devolver la `ProductID`, `ProductName`, y `UnitPrice` columnas desde el `Products` tabla. Como se muestra en la figura 8, después de realizar estas selecciones del asistente muestra la instrucción SQL resultante `SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`.


![Devolver datos de la tabla de productos](querying-data-with-the-sqldatasource-control-cs/_static/image10.gif)

**Figura 8**: devolver datos desde el `Products` tabla


Una vez haya configurado el Asistente para devolver el `ProductID`, `ProductName`, y `UnitPrice` columnas desde el `Products` de tabla, haga clic en el botón siguiente. Esta pantalla final proporciona una oportunidad para examinar los resultados de la consulta configurado en el paso anterior. Al hacer clic en el botón consulta de prueba ejecuta configurado `SELECT` instrucción y muestra los resultados en una cuadrícula.


![Haga clic en el botón de consulta de prueba para revisar la consulta de selección](querying-data-with-the-sqldatasource-control-cs/_static/image11.gif)

**Figura 9**: haga clic en el botón de consulta de prueba para revisar su `SELECT` consulta


Para completar al asistente, haga clic en Finalizar.

Al igual que con el origen ObjectDataSource, el Asistente para la s SqlDataSource simplemente asigna valores a las propiedades del control s, es decir, el [ `ConnectionString` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx) y [ `SelectCommand` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx) propiedades. Después de completar al asistente, el marcado declarativo de SqlDataSource control s debe ser similar al siguiente:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample2.aspx)]

El `ConnectionString` propiedad proporciona información sobre cómo conectarse a la base de datos. Esta propiedad se puede asignar un valor de cadena de conexión completa, codificado de forma rígida o puede apuntar a una cadena de conexión en `Web.config`. Para hacer referencia a un valor de cadena de conexión en Web.config, use la sintaxis `<%$ expressionPrefix:expressionValue %>`. Por lo general, *expressionPrefix* es ConnectionStrings y *expressionValue* es el nombre de la cadena de conexión en el `Web.config` [ `<connectionStrings>` sección](https://msdn.microsoft.com/library/bf7sd233.aspx). Sin embargo, se puede usar la sintaxis para hacer referencia a `<appSettings>` elementos o contenido de archivos de recursos. Consulte [información general sobre las expresiones de ASP.NET](https://msdn.microsoft.com/library/d5bd1tad.aspx) para obtener más información sobre esta sintaxis.

El `SelectCommand` propiedad especifica la instrucción de SQL ad hoc o un procedimiento almacenado que se ejecutan para devolver los datos.

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>Paso 3: Agregar un Control Web de datos y enlazarla de SqlDataSource

Una vez que se ha configurado SqlDataSource, se puede enlazar a un control Web, como GridView o DetailsView de datos. Para este tutorial, permiten s mostrar los datos en un control GridView. En el cuadro de herramientas, arrastre un control GridView a la página y enlazarlo a la `ProductsDataSource` SqlDataSource eligiendo el origen de datos de la lista desplegable en la etiqueta inteligente de s GridView.


[![Agregar un control GridView y enlazarlo con el SqlDataSource Control](querying-data-with-the-sqldatasource-control-cs/_static/image13.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image12.gif)

**Figura 10**: agregar un control GridView y enlazarlo con el SqlDataSource Control ([haga clic aquí para ver imagen en tamaño completo](querying-data-with-the-sqldatasource-control-cs/_static/image14.gif))


Una vez que ha había seleccionado el control SqlDataSource en la lista desplegable de la etiqueta inteligente de GridView s, Visual Studio agregará automáticamente un BoundField o CampoCasillaVerificación en GridView para cada una de las columnas devueltas por el control de origen de datos. Puesto que SqlDataSource devuelve tres columnas de la base de datos `ProductID`, `ProductName`, y `UnitPrice` hay tres campos en el control GridView.

Dedique un momento a configurar las tres operaciones de asignación GridView BoundFields. Cambiar el `ProductName` campo s `HeaderText` propiedad al nombre de producto y el `UnitPrice` campo s al precio. También dar formato a la `UnitPrice` campo como moneda. Después de realizar estas modificaciones, el marcado declarativo de GridView s debe ser similar al siguiente:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample3.aspx)]

Visite esta página a través de un explorador. Como se muestra en la figura 11, el control GridView enumera cada producto s `ProductID`, `ProductName`, y `UnitPrice` valores.


[![El control GridView muestra cada producto s ProductID, ProductName y los valores de UnitPrice](querying-data-with-the-sqldatasource-control-cs/_static/image16.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image15.gif)

**Figura 11**: The GridView muestra cada producto s `ProductID`, `ProductName`, y `UnitPrice` valores ([haga clic aquí para ver imagen en tamaño completo](querying-data-with-the-sqldatasource-control-cs/_static/image17.gif))


Cuando se visita la página de GridView invoca su control de origen de datos s `Select()` método. Cuando usamos el control ObjectDataSource, esto se denomina la `ProductsBLL` clase s `GetProducts()` método. Con SqlDataSource, sin embargo, el `Select()` método establece una conexión a la base de datos especificada y los problemas del `SelectCommand` (`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`, en este ejemplo). SqlDataSource devuelve sus resultados que el control GridView, a continuación, enumera, creando una fila en el control GridView para cada registro de base de datos devuelto.

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>Las características de Control de datos integradas Web y el Control SqlDataSource

En general, las características inherentes a los datos de que controles Web de paginación, ordenación, edición, eliminación, inserción y así sucesivamente son específicos para el control de datos Web y no son dependientes de usa el control de origen de datos. Es decir, el control GridView puede utilizar su integrado paginación, ordenación, edición y eliminación si se enlaza a un origen ObjectDataSource o SqlDataSource. Sin embargo, ciertas características de control Web de datos son sensibles a la que se va a usar el control de origen de datos o a la configuración de control s de origen de datos.

Por ejemplo, en el [eficazmente paginar a través de grandes cantidades de datos](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) tutorial analizamos cómo hacerlo, de forma predeterminada, la lógica de paginación para los datos Web controla otra vez devuelve *todas* registros subyacente origen de datos y, a continuación, muestran sólo el subconjunto adecuado de registros según el índice de la página actual y el número de registros que se muestran por página. Este modelo es muy ineficaz al paginar a través de conjuntos de resultados lo suficientemente grande. Afortunadamente, el origen ObjectDataSource puede configurarse para admitir la paginación personalizada, que devuelve solo el subconjunto preciso de registros que deben mostrarse. El control SqlDataSource, sin embargo, no tiene las propiedades para implementar la paginación personalizada.

Se produce otra sutileza con paginación y ordenación con SqlDataSource. De forma predeterminada, se pueden paginar los datos devueltos de SqlDataSource o revisaban GridView. Para demostrar esto, compruebe las opciones Habilitar paginación y habilitar la ordenación en la etiqueta inteligente de GridView s en `Querying.aspx` y compruebe que funciona según lo previsto.

Ordenar y paginar funcionan porque SqlDataSource recupera la base de datos en un conjunto de datos fuertemente tipado. El número total de registros devueltos por la consulta un aspecto esencial para implementar la paginación se puede comprobar desde el conjunto de datos. Además, se pueden ordenar los resultados de s de conjunto de datos a través de un objeto DataView. Estas capacidades se usan automáticamente por SqlDataSource cuando las solicitudes de GridView paginar o datos ordenan.

SqlDataSource puede configurarse para devolver un objeto DataReader en lugar de un conjunto de datos cambiando su [ `DataSourceMode` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx) desde `DataSet` (valor predeterminado) a `DataReader`. Utilizando un objeto DataReader podría preferir situaciones al pasar los resultados de SqlDataSource s en el código existente que espera un objeto DataReader. Además, puesto que DataReaders son objetos bastante más sencillas de conjuntos de datos, ofrecen un mejor rendimiento. Si realiza este cambio, sin embargo, no puede ordenar el control Web de datos ni página puesto que SqlDataSource no puede determinar cuántos registros devueltos por la consulta, ni tampoco DataReader ofrecen las técnicas para ordenar los datos devueltos.

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>Paso 4: Uso de una instrucción SQL personalizada o un procedimiento almacenado

Al configurar el control SqlDataSource, la consulta utilizada para devolver los datos se puede especificar en uno de estos dos enfoques como una instrucción SQL personalizada o un procedimiento almacenado, o como columnas de una tabla o vista. En el paso 2, hemos visto, seleccionar columnas de la `Products` tabla. Permiten s una mirada al uso de una instrucción SQL personalizada.

Agregue otro control GridView a la `Querying.aspx` página y elija crear un nuevo origen de datos en la lista desplegable de la etiqueta inteligente. A continuación, indicar que los datos se extraerá de una base de datos se creará un nuevo control SqlDataSource. Nombre del control `ProductsWithCategoryInfoDataSource`.


![Crear un nuevo Control SqlDataSource denominado ProductsWithCategoryInfoDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image18.gif)

**Figura 12**: crear un nuevo Control SqlDataSource denominado `ProductsWithCategoryInfoDataSource`


La siguiente pantalla nos pide que especifique la base de datos. Como hicimos en la figura 7, seleccione el `NORTHWINDConnectionString` en la lista desplegable lista y haga clic en siguiente. En la configuración de la pantalla de la instrucción Select, elija la especificación de un botón de opción de procedimiento almacenado o instrucción SQL personalizada y haga clic en siguiente. Se abrirá la pantalla definir personalizado instrucciones o procedimientos almacenados, que ofrece pestañas con la etiqueta SELECT, UPDATE, INSERT y DELETE. En cada pestaña puede especificar una instrucción SQL personalizada en el cuadro de texto o elija un procedimiento almacenado en la lista desplegable. En este tutorial veremos escribir una instrucción SQL personalizada; el siguiente tutorial incluye un ejemplo que utiliza un procedimiento almacenado.


![Escriba una instrucción SQL personalizada o seleccionar un procedimiento almacenado](querying-data-with-the-sqldatasource-control-cs/_static/image19.gif)

**Figura 13**: escriba una instrucción SQL personalizada o seleccionar un procedimiento almacenado


La instrucción SQL personalizada se puede escribir manualmente en el cuadro de texto o se puede construir gráficamente haciendo clic en el botón Generador de consultas. En el generador de consultas o en el cuadro de texto, use la siguiente consulta para devolver el `ProductID` y `ProductName` campos desde el `Products` tabla mediante una `JOIN` para recuperar el producto s `CategoryName` desde el `Categories` tabla:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample4.sql)]


![Puede gráficamente construir la consulta con el generador de consultas](querying-data-with-the-sqldatasource-control-cs/_static/image20.gif)

**Figura 14**: puede construir gráficamente la consulta mediante el generador de consultas


Después de especificar la consulta, haga clic en siguiente para ir a la pantalla de consulta de prueba. Haga clic en Finalizar para completar al Asistente de SqlDataSource.

Después de completar el asistente, el control GridView tendrá tres BoundFields agregados a la que mostrar el `ProductID`, `ProductName`, y `CategoryName` columnas devueltas por la consulta y lo que produce el siguiente marcado declarativo:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample5.aspx)]


[![El control GridView muestra cada identificador de producto s, nombre de la categoría de nombre y asociados](querying-data-with-the-sqldatasource-control-cs/_static/image22.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image21.gif)

**Figura 15**: The GridView muestra cada s Id. del producto, nombre y nombre de la categoría asociada ([haga clic aquí para ver imagen en tamaño completo](querying-data-with-the-sqldatasource-control-cs/_static/image23.gif))


## <a name="summary"></a>Resumen

En este tutorial hemos visto cómo consultar y mostrar los datos mediante el control SqlDataSource. Al igual que el origen ObjectDataSource, SqlDataSource actúa como un proxy, que proporciona un enfoque declarativo para tener acceso a datos. Sus propiedades especifican para conectarse a la base de datos y SQL `SELECT` una consulta para ejecutar; se pueden especificar a través de la ventana Propiedades o mediante el Asistente para configurar el origen de datos.

El `SELECT` ejemplos de consultas que hemos visto en este tutorial devuelven todos los registros de la consulta especificada. El control SqlDataSource, sin embargo, puede incluir un `WHERE` cláusula con parámetros cuyos valores se asignan mediante programación o se extraen automáticamente desde un origen especificado. Examinaremos cómo crear y utilizar consultas parametrizadas en el siguiente tutorial!

Feliz programación.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Acceso a los datos de la base de datos relacional](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [Información general del Control SqlDataSource](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [Tutoriales rápidos de ASP.NET: El Control SqlDataSource](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [El archivo Web.config `<connectionStrings>` elemento](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [Referencia de cadena de conexión de base de datos](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Connery Susan, Bernadette Leigh y David Suru. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](using-parameterized-queries-with-the-sqldatasource-cs.md)
