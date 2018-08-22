---
uid: web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
title: Almacenamiento en caché de datos con ObjectDataSource (C#) | Microsoft Docs
author: rick-anderson
description: Almacenamiento en caché puede significar la diferencia entre una lenta y una aplicación Web rápida. Este tutorial es el primero de cuatro que toman una visión detallada de almacenamiento en caché en ASP.NET...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: bd87413c-8160-4520-a8a2-43b555c4183a
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 770b1ae92db8384a0c0b7f038b1ea994ae4c1b4c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837962"
---
<a name="caching-data-with-the-objectdatasource-c"></a>Datos en caché con ObjectDataSource (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_58_CS.exe) o [descargar PDF](caching-data-with-the-objectdatasource-cs/_static/datatutorial58cs1.pdf)

> Almacenamiento en caché puede significar la diferencia entre una lenta y una aplicación Web rápida. Este tutorial es el primero de cuatro que toman una visión detallada de almacenamiento en caché en ASP.NET. Obtenga información sobre los conceptos clave de almacenamiento en caché y cómo aplicar el almacenamiento en caché a la capa de presentación mediante el control ObjectDataSource.


## <a name="introduction"></a>Introducción

En informática, *almacenamiento en caché* es el proceso de tomar datos o información que resulta caro obtener y almacenar una copia de ella en una ubicación que es más rápida para el acceso. Para las aplicaciones controladas por datos, grandes y complejas consultas normalmente consumen la mayoría del tiempo de ejecución de la aplicación s. A menudo se puede mejorar este tipo un s rendimiento de la aplicación, a continuación, almacenando los resultados de consultas costosas de la base de datos en la memoria de s de la aplicación.

ASP.NET 2.0 ofrece una variedad de opciones de caché. Una página web completa o el marcado de s representado del Control de usuario puede almacenarse en caché a través de *la caché de resultados*. Los controles de ObjectDataSource y SqlDataSource proporcionan capacidades de almacenamiento en caché también, lo que permite a datos en la memoria caché en el nivel de control. Y ASP.NET s *memoria caché de datos* proporciona una API enriquecida de almacenamiento en caché que permite a los desarrolladores de páginas a objetos de caché mediante programación. En este tutorial y los tres siguientes que examinaremos usando la s ObjectDataSource almacenamiento en caché de características, así como la caché de datos. También veremos cómo almacenar en caché datos de toda la aplicación en el inicio y cómo mantener datos almacenados en caché actualizados mediante el uso de dependencias de caché de SQL. Estos tutoriales se exploran no almacenamiento en caché de salida. Para una visión detallada de la caché de resultados, vea [caché de resultados en ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx).

Almacenamiento en caché se puede aplicar en cualquier lugar en la arquitectura, desde la capa de acceso a datos de seguridad a través de la capa de presentación. En este tutorial, buscaremos en la aplicación de almacenamiento en caché a la capa de presentación mediante el control ObjectDataSource. En el siguiente tutorial, que examinaremos almacenar en caché datos de la capa de lógica empresarial.

## <a name="key-caching-concepts"></a>Conceptos de almacenamiento en caché de clave

Almacenamiento en caché puede mejorar notablemente una s aplicación rendimiento y la escalabilidad de los datos que es costosos generar y almacenar una copia de ella en una ubicación que se puede acceder de forma más eficaz. Puesto que la memoria caché contiene una copia de los datos subyacentes, reales, puede quedar obsoleto, o *obsoletos*, si cambian los datos subyacentes. Para combatir esta situación, un desarrollador de páginas puede indicar los criterios de forma que será el elemento de caché *expulsan* desde la memoria caché, utilizando:

- **Criterios de tiempo** puede agregarse un elemento a la memoria caché para una absoluta o variable de duración. Por ejemplo, un desarrollador de páginas puede indicar una duración de, digamos, 60 segundos. Con una duración absoluta, el elemento en caché se expulsa 60 segundos después de agregarlo a la memoria caché, independientemente de la frecuencia con se obtuvo acceso. Con una duración deslizante, el elemento en caché se expulsa 60 segundos después del último acceso.
- **Criterios de dependencia** una dependencia puede asociarse con un elemento cuando se agrega a la caché. Cuando cambia la dependencia de elemento s se extrae de la memoria caché. La dependencia puede ser un archivo, otro elemento en caché o una combinación de ambos. ASP.NET 2.0 también permite que las dependencias de caché SQL, que permiten a los desarrolladores agregar un elemento a la memoria caché y que expulsa cuando cambian los datos de la base de datos subyacente. Examinaremos las dependencias de caché SQL en la próxima [utilizando las dependencias de caché de SQL](using-sql-cache-dependencies-cs.md) tutorial.

Independientemente de los criterios de expulsión especificados, puede ser un elemento en la memoria caché *compactarse* antes de que se ha cumplido los criterios basados en el tiempo o dependencia. Si la memoria caché ha alcanzado su capacidad, los elementos existentes deben quitarse antes de pueden agregar otros nuevos. Por lo tanto, cuando mediante programación trabajar con datos almacenados en caché s vital suponga siempre que los datos en caché no pueden estar presentes. Examinaremos el patrón que se utilizará al obtener acceso a datos de la memoria caché mediante programación en nuestro tutorial siguiente, *almacenamiento en caché de datos en la arquitectura*.

Almacenamiento en caché, proporciona una forma económica para obtiene mayor rendimiento de una aplicación. Como [Steven Smith](http://aspadvice.com/blogs/ssmith/) articula en su artículo [almacenamiento en caché de ASP.NET: técnicas y prácticas recomendadas](https://msdn.microsoft.com/library/aa478965.aspx):

Almacenamiento en caché puede ser una buena forma de obtener una buena suficiente rendimiento sin necesidad de mucho tiempo y análisis. Memoria es barata, por lo que si se puede obtener el rendimiento que necesita almacenar en caché el resultado durante 30 segundos en lugar de dedicar un día o semana intenta optimizar el código o la base de datos, realice la solución de almacenamiento en caché (suponiendo que los datos 30 - antiguos de segundo es correcto) y seguir adelante. Finalmente, un diseño deficiente probablemente mantendrá el ritmo, por lo que por supuesto debe intentar diseñar sus aplicaciones correctamente. Pero si desea obtener una buena suficiente rendimiento hoy en día, el almacenamiento en caché puede ser un excelente [método], comprando el tiempo suficiente para refactorizar la aplicación en una fecha posterior cuando tenga el momento para hacerlo.

Si bien almacenamiento en caché puede proporcionar mejoras de rendimiento considerables, no es aplicable en todas las situaciones, como con las aplicaciones que utilizan datos en tiempo real, se actualiza con frecuencia o que incluso en breve duración datos obsoletos están inaceptables. Pero para la mayoría de las aplicaciones, se debe usar almacenamiento en caché. Para obtener más información sobre almacenamiento en caché en ASP.NET 2.0, consulte el [almacenamiento en caché para el rendimiento](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/default.aspx) sección de la [tutoriales de ASP.NET 2.0](https://quickstarts.asp.net/QuickStartv20/aspnet/).

## <a name="step-1-creating-the-caching-web-pages"></a>Paso 1: Creación de las páginas Web de almacenamiento en caché

Antes de empezar la exploración de las características de almacenamiento en caché de ObjectDataSource s, permiten s primero dedique un momento para crear las páginas ASP.NET en nuestro proyecto de sitio Web que necesitaremos para este tutorial y los tres siguientes. Empiece por agregar una nueva carpeta denominada `Caching`. A continuación, agregue las siguientes páginas ASP.NET a la carpeta y asegúrese de asociar cada página con el `Site.master` página maestra:

- `Default.aspx`
- `ObjectDataSource.aspx`
- `FromTheArchitecture.aspx`
- `AtApplicationStartup.aspx`
- `SqlCacheDependencies.aspx`


![Agregar las páginas ASP.NET para los tutoriales relacionados con el almacenamiento en caché](caching-data-with-the-objectdatasource-cs/_static/image1.png)

**Figura 1**: agregar las páginas ASP.NET para los tutoriales relacionados con el almacenamiento en caché


Al igual que en las demás carpetas `Default.aspx` en el `Caching` carpeta mostrará una lista de los tutoriales en su sección. Recuerde que el `SectionLevelTutorialListing.ascx` Control de usuario proporciona esta funcionalidad. Por lo tanto, agrega este Control de usuario a `Default.aspx` arrastrándolo desde el Explorador de soluciones en la página de vista de diseño de s.


[![Figura 2: Agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx](caching-data-with-the-objectdatasource-cs/_static/image3.png)](caching-data-with-the-objectdatasource-cs/_static/image2.png)

**Figura 2**: figura 2: agregar el `SectionLevelTutorialListing.ascx` Control de usuario `Default.aspx` ([haga clic aquí para ver imagen en tamaño completo](caching-data-with-the-objectdatasource-cs/_static/image4.png))


Por último, agregue estas páginas como entradas para el `Web.sitemap` archivo. En concreto, agregue el siguiente marcado después de trabajar con datos binarios `<siteMapNode>`:


[!code-xml[Main](caching-data-with-the-objectdatasource-cs/samples/sample1.xml)]

Después de actualizar `Web.sitemap`, dedique un momento para ver el sitio Web de tutoriales a través de un explorador. El menú de la izquierda ahora incluye elementos para los tutoriales de almacenamiento en caché.


![El mapa del sitio incluye ahora entradas para los tutoriales de almacenamiento en caché](caching-data-with-the-objectdatasource-cs/_static/image5.png)

**Figura 3**: el mapa del sitio incluye ahora entradas para los tutoriales de almacenamiento en caché


## <a name="step-2-displaying-a-list-of-products-in-a-web-page"></a>Paso 2: Mostrar una lista de productos en una página Web

Este tutorial describe cómo usar las ObjectDataSource control s características integradas de almacenamiento en caché. Antes de que podemos observar estas características, sin embargo, primero es necesario trabajar desde una página. S permiten crear una página web que usa un control GridView para mostrar información de producto recuperada por un origen ObjectDataSource desde el `ProductsBLL` clase.

Comience abriendo la `ObjectDataSource.aspx` página en el `Caching` carpeta. Arrastre un control GridView del cuadro de herramientas hasta el diseñador, establezca su `ID` propiedad `Products`y, en su etiqueta inteligente, elija para enlazarla a un nuevo control ObjectDataSource denominado `ProductsDataSource`. Configurar el origen ObjectDataSource para trabajar con el `ProductsBLL` clase.


[![Configurar el origen ObjectDataSource para usar la clase ProductsBLL](caching-data-with-the-objectdatasource-cs/_static/image7.png)](caching-data-with-the-objectdatasource-cs/_static/image6.png)

**Figura 4**: configurar el origen ObjectDataSource que se usarán el `ProductsBLL` clase ([haga clic aquí para ver imagen en tamaño completo](caching-data-with-the-objectdatasource-cs/_static/image8.png))


Para esta página, permiten crear un control GridView editable para que podamos examinar lo que sucede cuando se modifican los datos almacenados en caché en el origen ObjectDataSource a través de la interfaz de s GridView de s. Deje la lista desplegable en la ficha Seleccionar con su valor predeterminado, `GetProducts()`, pero cambie el elemento seleccionado en la pestaña de la actualización a la `UpdateProduct` sobrecarga que acepta `productName`, `unitPrice`, y `productID` como sus parámetros de entrada.


[![Establecer la lista desplegable s UPDATE a la sobrecarga adecuada UpdateProduct](caching-data-with-the-objectdatasource-cs/_static/image10.png)](caching-data-with-the-objectdatasource-cs/_static/image9.png)

**Figura 5**: establece la lista desplegable de ficha actualización s en el adecuado `UpdateProduct` sobrecargar ([haga clic aquí para ver imagen en tamaño completo](caching-data-with-the-objectdatasource-cs/_static/image11.png))


Por último, establezca las listas desplegables en las fichas de INSERT y DELETE en (None) y haga clic en Finalizar. Al finalizar el asistente Configurar origen de datos, Visual Studio establece la s ObjectDataSource `OldValuesParameterFormatString` propiedad `original_{0}`. Como se describe en el [una visión general de insertar, actualizar y eliminar datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial, esta propiedad debe quitarse la sintaxis declarativa o vuelve a establecer en su valor predeterminado, `{0}`, en orden de nuestro flujo de actualización continuar sin errores.

Además, al finalizar el Asistente para Visual Studio agrega un campo en el control GridView para cada uno de los campos de datos del producto. Quite todos menos a los `ProductName`, `CategoryName`, y `UnitPrice` BoundFields. A continuación, actualice el `HeaderText` las propiedades de cada uno de estos BoundFields al producto, categoría y el precio, respectivamente. Puesto que la `ProductName` campo es obligatorio, convertir la BoundField en TemplateField y agregue un control RequiredFieldValidator a la `EditItemTemplate`. Asimismo, convertir la `UnitPrice` BoundField en TemplateField y agregue un control CompareValidator para asegurarse de que el valor especificado por el usuario es una moneda válida valor s mayor o igual que cero. Además de estas modificaciones, no dude en realizar los cambios estéticos, como Alinear a la derecha el `UnitPrice` valor o especificar el formato para el `UnitPrice` texto en sus interfaces de solo lectura y edición.

Asegúrese de GridView editable activando la casilla Habilitar edición en la etiqueta inteligente de s GridView. Compruebe también las casillas de verificación Habilitar paginación y habilitar la ordenación.

> [!NOTE]
> ¿Necesita una revisión de cómo personalizar la interfaz de edición de s GridView? Si es así, vuelva a consultar el [personalizar la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) tutorial.


[![Habilitar la compatibilidad de GridView para la edición, ordenación y paginación](caching-data-with-the-objectdatasource-cs/_static/image13.png)](caching-data-with-the-objectdatasource-cs/_static/image12.png)

**Figura 6**: habilitar la compatibilidad con GridView para edición, ordenación y paginación ([haga clic aquí para ver imagen en tamaño completo](caching-data-with-the-objectdatasource-cs/_static/image14.png))


Después de realizar estas modificaciones GridView, el marcado declarativo de s GridView y ObjectDataSource debe ser similar al siguiente:


[!code-aspx[Main](caching-data-with-the-objectdatasource-cs/samples/sample2.aspx)]

Como se muestra en la figura 7, el control GridView editable muestra el nombre, la categoría y el precio de cada uno de los productos en la base de datos. Dedique un momento para probar los resultados de página a través de ellos, la ordenación de la funcionalidad de página s y editar un registro.


[![Cada nombre de producto, categoría y el precio se muestran en un ordenable, Pageable, control GridView Editable](caching-data-with-the-objectdatasource-cs/_static/image16.png)](caching-data-with-the-objectdatasource-cs/_static/image15.png)

**Figura 7**: s nombre de cada producto, categoría y el precio se muestra en un ordenable, Pageable, control GridView Editable ([haga clic aquí para ver imagen en tamaño completo](caching-data-with-the-objectdatasource-cs/_static/image17.png))


## <a name="step-3-examining-when-the-objectdatasource-is-requesting-data"></a>Paso 3: Examinar ObjectDataSource the cuando es solicitar datos

El `Products` GridView recupera sus datos para mostrar invocando el `Select` método de la `ProductsDataSource` ObjectDataSource. Este origen ObjectDataSource crea una instancia de la capa de lógica empresarial de s `ProductsBLL` clase y llama a su `GetProducts()` método, que a su vez llama a la capa de acceso a datos de s `ProductsTableAdapter` s `GetProducts()` método. El método DAL se conecta a la base de datos Northwind y emite el configurado `SELECT` consulta. Estos datos se devuelven a la capa DAL, que empaqueta en un `NorthwindDataTable`. Se devuelve el objeto DataTable a la capa BLL, que se devuelve para el origen ObjectDataSource, que lo devuelve a la GridView. El control GridView, a continuación, crea un `GridViewRow` para cada objeto `DataRow` en la tabla de datos y cada uno de ellos `GridViewRow` finalmente se representa en el código HTML que se devuelve al cliente y se muestra en el explorador del visitante s.

Esta secuencia de eventos se produce cada vez que el control GridView necesita enlazarse a los datos subyacentes. Esto sucede cuando se visita la página en primer lugar, al pasar de una página de datos a otro, al ordenar el control GridView, o al modificar los datos de GridView s a través de su integrado editen o eliminen las interfaces. Si se deshabilita el estado de vista GridView s, el control GridView se vuelve a enlazar en cada devolución de datos también. El control GridView puede también explícitamente enlazarse a sus datos mediante una llamada a su `DataBind()` método.

Para apreciar la frecuencia con la que se recuperan los datos de la base de datos, permiten s muestra un mensaje que indica cuando se vuelve a recuperados los datos. Agregar un control Web de etiqueta por encima del control GridView denominado `ODSEvents`. Borrar su `Text` propiedad y establezca su `EnableViewState` propiedad `false`. Debajo de la etiqueta, agregue un control Web de botón y establezca su `Text` propiedad a la devolución de datos.


[![Agregar una etiqueta y un botón a la página situada encima del control GridView](caching-data-with-the-objectdatasource-cs/_static/image19.png)](caching-data-with-the-objectdatasource-cs/_static/image18.png)

**Figura 8**: agregar una etiqueta y un botón a la página encima de GridView ([haga clic aquí para ver imagen en tamaño completo](caching-data-with-the-objectdatasource-cs/_static/image20.png))


Durante el flujo de trabajo de acceso de datos, la s ObjectDataSource `Selecting` evento se desencadena antes de crear el objeto subyacente y su método configurado. Cree un controlador de eventos para este evento y agregue el código siguiente:


[!code-csharp[Main](caching-data-with-the-objectdatasource-cs/samples/sample3.cs)]

Cada vez que el origen ObjectDataSource realiza una solicitud a la arquitectura de datos, la etiqueta mostrará el evento de selección de texto que se desencadena.

Visite esta página en un explorador. Cuando primero se visita la página, se muestra el evento de selección de texto que se desencadena. Haga clic en el botón de devolución de datos y tenga en cuenta que el texto desaparece (suponiendo que la s GridView `EnableViewState` propiedad está establecida en `true`, el valor predeterminado). Esto es porque, en el postback, se reconstruye el control GridView de su estado de vista y, por tanto, t a ObjectDataSource para sus datos. Ordenación, paginación, o la edición de los datos, sin embargo, hace que el control GridView a enlazarse a su origen de datos, y por lo tanto, desencadena el evento cuando se selecciona texto vuelve a aparecer.


[![Cuando se vuelve a enlazar el control GridView al origen de datos, se mostrará cuando se selecciona el evento](caching-data-with-the-objectdatasource-cs/_static/image22.png)](caching-data-with-the-objectdatasource-cs/_static/image21.png)

**Figura 9**: GridView cada vez que se vuelve a enlazar al origen de datos, se muestra cuando se selecciona el evento ([haga clic aquí para ver imagen en tamaño completo](caching-data-with-the-objectdatasource-cs/_static/image23.png))


[![Al hacer clic en el botón de devolución de datos hace que el control GridView a se reconstruye a partir de su estado de vista](caching-data-with-the-objectdatasource-cs/_static/image25.png)](caching-data-with-the-objectdatasource-cs/_static/image24.png)

**Figura 10**: al hacer clic en el botón de devolución de datos hace que el control GridView a se reconstruye a partir de su estado de vista ([haga clic aquí para ver imagen en tamaño completo](caching-data-with-the-objectdatasource-cs/_static/image26.png))


Puede parecer una pérdida de tiempo recuperar la base de datos cada vez que se paginar a través de los datos o se ordenan. Después de todo, desde que se usa paginación predeterminada, el origen ObjectDataSource ha recuperado todos los registros al mostrar la primera página. Incluso si no proporcionan el control GridView de ordenación y paginación de soporte técnico, los datos se deben recuperar de la base de datos cada vez que primero se visita la página por cualquier usuario (y en cada postback, si se deshabilita el estado de vista). Pero si el control GridView muestra los mismos datos para todos los usuarios, estas solicitudes de base de datos adicional superfluas. ¿Por qué no almacenar en caché los resultados devueltos desde el `GetProducts()` método y enlazar el control GridView a los que se almacena en caché los resultados?

## <a name="step-4-caching-the-data-using-the-objectdatasource"></a>Paso 4: Almacenamiento en caché los datos con ObjectDataSource

Al establecer simplemente una serie de propiedades, ObjectDataSource puede configurarse para almacenar en caché automáticamente sus datos recuperados en la caché de datos ASP.NET. En la lista siguiente se resume las propiedades relacionadas con la memoria caché del origen ObjectDataSource:

- [EnableCaching](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.enablecaching.aspx) debe establecerse en `true` para habilitar el almacenamiento en caché. De manera predeterminada, es `false`.
- [CacheDuration](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheduration.aspx) la cantidad de tiempo, en segundos, que se almacena en caché los datos. El valor predeterminado es 0. ObjectDataSource solo almacenará datos si `EnableCaching` es `true` y `CacheDuration` se establece en un valor mayor que cero.
- [CacheExpirationPolicy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheexpirationpolicy.aspx) se puede establecer en `Absolute` o `Sliding`. Si `Absolute`, ObjectDataSource almacena en caché los datos recuperados para `CacheDuration` segundos; si `Sliding`, expiran los datos solo después de no se ha accedido durante `CacheDuration` segundos. De manera predeterminada, es `Absolute`.
- [CacheKeyDependency](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cachekeydependency.aspx) utilizar esta propiedad para asociar las entradas de caché de ObjectDataSource s con una dependencia de caché existente. Las entradas de datos de origen ObjectDataSource s se pueden expulsar prematuramente de la memoria caché mediante la expiración de su asociado `CacheKeyDependency`. Esta propiedad se usa con más frecuencia para asociar una dependencia de caché SQL con la caché de ObjectDataSource s, un tema, exploraremos en el futuro [utilizando las dependencias de caché de SQL](using-sql-cache-dependencies-cs.md) tutorial.

Permiten s configurar el `ProductsDataSource` ObjectDataSource para almacenar en caché sus datos durante 30 segundos en una escala absoluta. Establezca la s ObjectDataSource `EnableCaching` propiedad `true` y su `CacheDuration` propiedad a 30. Deje el `CacheExpirationPolicy` propiedad establecida en su valor predeterminado, `Absolute`.


[![Configurar el origen ObjectDataSource para almacenar sus datos en caché durante 30 segundos](caching-data-with-the-objectdatasource-cs/_static/image28.png)](caching-data-with-the-objectdatasource-cs/_static/image27.png)

**Figura 11**: configurar el origen ObjectDataSource para almacenar sus datos en caché durante 30 segundos ([haga clic aquí para ver imagen en tamaño completo](caching-data-with-the-objectdatasource-cs/_static/image29.png))


Guarde los cambios y volver a visitar esta página en un explorador. El texto del evento se desencadena cuando se selecciona aparecerá cuando la primera vez que visite la página, como inicialmente los datos no están en la memoria caché. Pero no los postbacks subsiguientes desencadenados haciendo clic en el botón de Postback, ordenación, paginación, o haciendo clic en los botones de edición o en Cancelar *no* texto que desencadena el evento cuando se selecciona volver a mostrar. Esto es porque el `Selecting` evento solo se desencadena cuando el origen ObjectDataSource obtiene sus datos de su objeto subyacente; el `Selecting` no se desencadena un evento si se extraen los datos de la caché de datos.

Después de 30 segundos, se expulsen los datos de la memoria caché. También se expulsen los datos de la memoria caché si la s ObjectDataSource `Insert`, `Update`, o `Delete` los métodos se invocan. Por lo tanto, una vez transcurridos 30 segundos o el botón de actualización se ha hecho clic, ordenación, paginación, o al hacer clic en los botones de edición o en Cancelar hará que el ObjectDataSource obtener sus datos de los objetos subyacentes, mostrando el evento de selección desencadena texto cuando el `Selecting` desencadena el evento. Estos resultados devueltos se colocan en la caché de datos.

> [!NOTE]
> Si ve el texto de evento se desencadena cuando se selecciona con frecuencia, incluso cuando se espera que el ObjectDataSource a trabajar con datos almacenados en caché, es posible debido a restricciones de memoria. Si no hay suficiente memoria libre, es posible que haya ha compactarse los datos agregados a la memoria caché por el origen ObjectDataSource. Si t ObjectDataSource parece ser correctamente en caché los datos o solo las memorias caché los datos de forma esporádica, cierre algunas aplicaciones para liberar memoria y vuelva a intentarlo.


Figura 12 se ilustra la s de ObjectDataSource almacenamiento en caché de flujo de trabajo. Cuando se desencadena el evento cuando se selecciona el texto aparece en la pantalla, es porque los datos no estaba en la caché y tenían que recuperarse desde el objeto subyacente. Cuando falta este texto, sin embargo, lo s porque los datos disponibles de la memoria caché. Cuando se devuelven los datos de la memoria caché ahí s ejecutan ninguna llamada al objeto subyacente y, por lo tanto, ninguna consulta de base de datos.


![Los almacenes de ObjectDataSource y recupera sus datos desde la caché de datos](caching-data-with-the-objectdatasource-cs/_static/image30.png)

**Figura 12**: ObjectDataSource almacena y recupera sus datos de la caché de datos


Cada aplicación de ASP.NET tiene su propia memoria caché de datos que s compartía entre todas las páginas y los visitantes de instancia. Esto significa que los datos almacenados en la caché de datos por el origen ObjectDataSource del mismo modo se comparten entre todos los usuarios que visitan la página. Para comprobarlo, abra el `ObjectDataSource.aspx` página en un explorador. Cuando se visita primero la página, aparecerá el texto de evento se desencadena cuando se selecciona (suponiendo que los datos agregados a la caché de las pruebas anteriores se, en este momento, ha desalojado). Abra una segunda instancia del explorador y copie y pegue la dirección URL de la primera instancia de explorador a la segunda. En la segunda instancia del explorador, no se muestra el texto del evento se desencadena cuando se selecciona porque lo s usando la misma en memoria caché los datos que la primera.

Al insertar los datos recuperados en la memoria caché, el origen ObjectDataSource utiliza un valor de clave de caché que incluye: el `CacheDuration` y `CacheExpirationPolicy` los valores de propiedad; el tipo del objeto comercial subyacente que usa el origen ObjectDataSource, que se especifica a través de la [ `TypeName` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.typename.aspx) (`ProductsBLL`, en este ejemplo); el valor de la `SelectMethod` propiedad y el nombre y los valores de los parámetros en el `SelectParameters` colección; y los valores de sus `StartRowIndex`y `MaximumRows` propiedades, que se usan al implementar [paginación personalizada.](../paging-and-sorting/paging-and-sorting-report-data-cs.md)

Diseñar el valor de clave de caché como una combinación de estas propiedades garantiza una entrada de caché única, como cambian estos valores. Por ejemplo, en los tutoriales anteriores se ve examinado utilizando el `ProductsBLL` clase s `GetProductsByCategoryID(categoryID)`, que devuelve todos los productos de la categoría especificada. Un usuario puede llegar a la página y vista de bebidas, que tiene un `CategoryID` de 1. Si el origen ObjectDataSource almacenado en caché sus resultados sin tener en cuenta el `SelectParameters` valores, cuando otro usuario llegó a la página ver los condimentos mientras los productos de bebidas estuviera en la memoria caché, d verán los productos de bebidas en caché en lugar de condimentos. Mediante la variación de la clave de caché por estas propiedades, que incluyen los valores de la `SelectParameters`, ObjectDataSource mantiene una entrada de caché independiente para bebidas y condimentos.

## <a name="stale-data-concerns"></a>Problemas de datos obsoletos

ObjectDataSource extrae automáticamente sus elementos de la memoria caché cuando cualquiera de sus `Insert`, `Update`, o `Delete` los métodos se invocan. Esto ayuda a protegerse contra datos obsoletos desactivando las entradas de caché cuando se modifican los datos a través de la página. Sin embargo, es posible que un origen ObjectDataSource con almacenamiento en caché para seguirá mostrando datos obsoletos. En el caso más simple, puede ser debido a los datos de cambio directamente en la base de datos. Quizás un administrador de base de datos acaba de ejecutar un script que modifica algunos de los registros en la base de datos.

También puede expandir este escenario de una manera más sutil. Aunque el origen ObjectDataSource extrae sus elementos de la memoria caché cuando se llama a uno de sus métodos de modificación de datos, quitados los elementos almacenados en caché son para la combinación ObjectDataSource s los valores de propiedad (`CacheDuration`, `TypeName`, `SelectMethod`, y así sucesivamente). Si tiene dos ObjectDataSources que usan diferentes `SelectMethods` o `SelectParameters`, pero todavía puede actualizar los mismos datos, entonces un ObjectDataSource puede actualizar una fila e invalidar sus propias entradas de caché, pero la fila correspondiente para el segundo ObjectDataSource se atenderán desde la caché. Le animo a crear páginas para presentar esta funcionalidad. Crear una página que muestra un control GridView editable que extrae sus datos de un origen ObjectDataSource que usa el almacenamiento en caché y está configurado para obtener datos desde el `ProductsBLL` clase s `GetProducts()` método. Agregue otro ObjectDataSource a esta página (o a otra), pero para este origen ObjectDataSource segundo y GridView editable que utilice el `GetProductsByCategoryID(categoryID)` método. Desde el dos ObjectDataSource `SelectMethod` difieren de las propiedades, cada tendrá sus propios valores almacenados en caché. Si edita un producto en una cuadrícula, la próxima vez que enlazar los datos a la cuadrícula de otra (mediante la paginación, ordenación etc.), se continuar sirviendo a los datos antiguos, almacenado en caché y no refleja el cambio realizado en la cuadrícula de otra.

En resumen, use solo expirados basados en tiempo si está dispuesto a tienen el potencial de los datos obsoletos y usar expirados más cortos para escenarios donde es importante la actualización de datos. Si los datos obsoletos no están aceptables, ya sea renuncian a almacenamiento en caché o usar dependencias de caché de SQL (suponiendo que es la base de datos que se van a almacenar en caché). Exploraremos las dependencias de caché SQL en un futuro tutorial.

## <a name="summary"></a>Resumen

En este tutorial se examinan las capacidades de almacenamiento en caché de ObjectDataSource s integradas. Estableciendo simplemente algunas propiedades, nos podemos indicar a ObjectDataSource para almacenar en caché los resultados devueltos de especificado `SelectMethod` en la memoria caché de datos ASP.NET. El `CacheDuration` y `CacheExpirationPolicy` propiedades indican la duración se almacena en caché el elemento y si es absoluto o la expiración variable. El `CacheKeyDependency` propiedad asocia todas las entradas de caché de ObjectDataSource s con una dependencia de caché existente. Esto se puede usar para expulsar las entradas de ObjectDataSource s de la memoria caché antes de que se alcanza la caducidad de tiempo y se suele utilizar con dependencias de caché de SQL.

Puesto que el origen ObjectDataSource simplemente almacena en caché sus valores a la caché de datos, nos podríamos replicar la funcionalidad integrada de s de ObjectDataSource mediante programación. Tiene sentido hacerlo en la capa de presentación, porque el origen ObjectDataSource ofrece esta funcionalidad desde el principio, pero podemos implementar capacidades de almacenamiento en caché en una capa independiente de la arquitectura. Para ello, debemos repetir la misma lógica usada por el origen ObjectDataSource. Exploraremos cómo trabajar mediante programación con la memoria caché de datos desde dentro de la arquitectura de nuestro siguiente tutorial.

Feliz programación.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Almacenamiento en caché de ASP.NET: Técnicas y prácticas recomendadas](https://msdn.microsoft.com/library/aa478965.aspx)
- [Guía de arquitectura de almacenamiento en caché para aplicaciones de .NET Framework](https://msdn.microsoft.com/library/ee817645.aspx)
- [Salida de almacenamiento en caché en ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Teresa Murphy. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](caching-data-in-the-architecture-cs.md)
