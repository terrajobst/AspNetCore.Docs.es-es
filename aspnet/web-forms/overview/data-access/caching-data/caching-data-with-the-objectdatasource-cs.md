---
uid: web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
title: Almacenar en caché datos con ObjectDataSource (C#) | Documentos de Microsoft
author: rick-anderson
description: Almacenamiento en caché puede significar la diferencia entre una lenta y un rápido de las aplicaciones Web. Este tutorial es el primero de cuatro que toman una visión detallada de almacenamiento en caché de ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: bd87413c-8160-4520-a8a2-43b555c4183a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: c30dcba7d6ff9849371ef92c84d4ec32aa1dc73d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879197"
---
<a name="caching-data-with-the-objectdatasource-c"></a>Almacenar en caché datos con ObjectDataSource (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_58_CS.exe) o [descarga de PDF](caching-data-with-the-objectdatasource-cs/_static/datatutorial58cs1.pdf)

> Almacenamiento en caché puede significar la diferencia entre una lenta y un rápido de las aplicaciones Web. Este tutorial es el primero de cuatro que toman una visión detallada de almacenamiento en caché de ASP.NET. Obtenga información acerca de los conceptos clave de almacenamiento en caché y cómo aplicar el almacenamiento en caché a la capa de presentación mediante el control ObjectDataSource.


## <a name="introduction"></a>Introducción

En informática, *almacenamiento en caché* es el proceso de tomar datos o información que resulta caro obtener y almacenar una copia del mismo en una ubicación que es más rápida para el acceso. Para las aplicaciones orientadas a datos, las consultas grandes y complejas normalmente consumen la mayor parte del tiempo de ejecución de la aplicación s. A menudo se puede mejorar este tipo un s rendimiento de la aplicación, a continuación, almacenando los resultados de consultas costosas de la base de datos en la memoria de s de la aplicación.

ASP.NET 2.0 proporciona una variedad de opciones de caché. Una página web completa o marcado de s representa el Control de usuario puede almacenarse en caché a través de *caché de resultados*. Los controles de ObjectDataSource y SqlDataSource proporcionan capacidades de almacenamiento en caché también, lo que permite a datos en la memoria caché en el nivel de control. Y ASP.NET s *memoria caché de datos* proporciona una API de almacenamiento en caché enriquecida que permite a los desarrolladores de página a los objetos de caché mediante programación. En este tutorial y los tres siguientes que examinaremos mediante las operaciones de asignación ObjectDataSource almacenamiento en caché de características, así como la caché de datos. También veremos cómo almacenar en caché datos de toda la aplicación en el inicio y cómo mantener los datos almacenados en caché actualizado mediante el uso de las dependencias de caché SQL. Estos tutoriales no explore la caché de resultados. Para obtener una visión detallada de caché de resultados, vea [caché de resultados en ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx).

Almacenamiento en caché puede aplicarse en cualquier lugar en la arquitectura, desde la capa de acceso a datos de seguridad a través de la capa de presentación. En este tutorial se examinará aplicar el almacenamiento en caché a la capa de presentación mediante el control ObjectDataSource. En el siguiente tutorial que examinaremos el almacenamiento en caché los datos en la capa de lógica de negocios.

## <a name="key-caching-concepts"></a>Clave de almacenamiento en caché de conceptos

Almacenamiento en caché puede mejorar considerablemente una s de aplicación general rendimiento y escalabilidad mediante la obtención de datos que resulta caros generar y almacenar una copia del mismo en una ubicación que se puede acceder de forma más eficaz. Puesto que la memoria caché contiene solo una copia de los datos reales, subyacentes, puede quedar obsoleto, o *obsoletos*, si cambian los datos subyacentes. Para hacer frente a esto, un desarrollador de páginas puede indicar los criterios de forma que será el elemento en caché *expulsan* desde la memoria caché, utilizando:

- **Criterios de tiempo** puede agregarse un elemento a la memoria caché para una absoluta o variable duración. Por ejemplo, un desarrollador de páginas puede indicar una duración de, digamos, 60 segundos. Con una duración absoluta, el elemento almacenado en caché se expulsa 60 segundos después de que se agregó a la memoria caché, sin tener en cuenta la frecuencia con se obtuvo acceso. Con una duración de una variable, el elemento almacenado en caché se expulsa 60 segundos después del último acceso.
- **Criterios de dependencia** una dependencia puede estar asociada a un elemento cuando se agrega a la memoria caché. Cuando cambia la dependencia de elemento s se expulsa de la memoria caché. La dependencia puede ser un archivo, otro elemento en caché o una combinación de ambos. ASP.NET 2.0 también permite que las dependencias de caché SQL, que permiten a los desarrolladores agregar un elemento a la memoria caché y hacer que se desalojan cuando cambian los datos de la base de datos subyacente. Examinaremos las dependencias de caché SQL en el próximo seminario Web [utilizando las dependencias de caché de SQL](using-sql-cache-dependencies-cs.md) tutorial.

Sin tener en cuenta los criterios de expulsión especificados, puede ser un elemento en la memoria caché *compactarse* antes de que se ha cumplido los criterios basados en el tiempo o dependencia. Si la memoria caché ha alcanzado su capacidad, los elementos existentes deben quitarse antes de que se pueden agregar otros nuevos. Por lo tanto, cuando mediante programación al trabajar con datos almacenados en caché, s vitales suponer siempre los datos almacenados en caché no pueden estar presentes. Echemos un vistazo el patrón que se utiliza cuando se obtiene acceso a datos desde la memoria caché mediante programación en nuestro tutorial siguiente, *almacenar datos en caché en la arquitectura*.

Almacenamiento en caché, proporciona una forma económica para obtiene mayor rendimiento de una aplicación. Como [Steven Smith](http://aspadvice.com/blogs/ssmith/) articula en su artículo [almacenamiento en caché de ASP.NET: técnicas y prácticas recomendadas](https://msdn.microsoft.com/library/aa478965.aspx):

Almacenamiento en caché puede ser una buena forma de obtener una buena suficiente rendimiento sin necesidad de una gran cantidad de tiempo y los análisis. Memoria es barata, por lo que si se puede obtener el rendimiento que necesitan almacenando en caché el resultado de 30 segundos en lugar de dedicar un día o una semana intenta optimizar el código o la base de datos, realice la acción de la solución de almacenamiento en caché (suponiendo que los datos 30 - antiguos de segundos es correcto) y avanzar. Finalmente, un diseño deficiente se probablemente ponerse al día, por lo que por supuesto debe intentar diseñar aplicaciones correctamente. Pero si desea obtener una buena suficiente rendimiento hoy en día, el almacenamiento en caché puede ser un excelente [método], compra de tiempo para refactorizar la aplicación en una fecha posterior cuando tenga el tiempo para hacerlo.

Aunque el almacenamiento en caché puede proporcionar mejoras de rendimiento apreciables, no es aplicable en todas las situaciones, como con aplicaciones que utilizan datos en tiempo real, actualizar con frecuencia o que no están aceptable incluso en breve duración datos obsoletos. Pero para la mayoría de las aplicaciones, se debe utilizar el almacenamiento en caché. Para obtener más información sobre el almacenamiento en caché de ASP.NET 2.0, consulte el [almacenamiento en caché para obtener un rendimiento](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/default.aspx) sección de la [tutoriales de inicio rápido de ASP.NET 2.0](https://quickstarts.asp.net/QuickStartv20/aspnet/).

## <a name="step-1-creating-the-caching-web-pages"></a>Paso 1: Crear las páginas Web de almacenamiento en caché

Antes de empezar la exploración de las características de almacenamiento en caché de ObjectDataSource s, permiten s primero Tómese un momento para crear las páginas ASP.NET en el proyecto de sitio Web que se necesitará para este tutorial y los tres siguientes. Empiece por agregar una nueva carpeta denominada `Caching`. A continuación, agregue las siguientes páginas ASP.NET a la carpeta y asegúrese de que asociar cada página con el `Site.master` página maestra:

- `Default.aspx`
- `ObjectDataSource.aspx`
- `FromTheArchitecture.aspx`
- `AtApplicationStartup.aspx`
- `SqlCacheDependencies.aspx`


![Agregar las páginas ASP.NET para los tutoriales relacionados con el almacenamiento en caché](caching-data-with-the-objectdatasource-cs/_static/image1.png)

**Figura 1**: agregar las páginas ASP.NET para los tutoriales relacionados con el almacenamiento en caché


Al igual que en las otras carpetas, `Default.aspx` en el `Caching` carpeta enumerará los tutoriales en su sección. Recuerde que el `SectionLevelTutorialListing.ascx` Control de usuario proporciona esta funcionalidad. Por lo tanto, agregue este Control de usuario para `Default.aspx` arrastrándolo desde el Explorador de soluciones en la página s la vista Diseño.


[![Figura 2: Agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx](caching-data-with-the-objectdatasource-cs/_static/image3.png)](caching-data-with-the-objectdatasource-cs/_static/image2.png)

**Figura 2**: figura 2: agregar el `SectionLevelTutorialListing.ascx` Control de usuario a `Default.aspx` ([haga clic aquí para ver la imagen a tamaño completo](caching-data-with-the-objectdatasource-cs/_static/image4.png))


Por último, agregue estas páginas como entradas para el `Web.sitemap` archivo. En concreto, agregue el siguiente marcado después el trabajo con datos binarios `<siteMapNode>`:


[!code-xml[Main](caching-data-with-the-objectdatasource-cs/samples/sample1.xml)]

Después de actualizar `Web.sitemap`, tómese un momento para ver el sitio Web de tutoriales a través de un explorador. Ahora, el menú de la izquierda incluye elementos para los tutoriales de almacenamiento en caché.


![El mapa del sitio ahora incluye las entradas para los tutoriales de almacenamiento en caché](caching-data-with-the-objectdatasource-cs/_static/image5.png)

**Figura 3**: la asignación de sitio ahora incluye las entradas para los tutoriales de almacenamiento en caché


## <a name="step-2-displaying-a-list-of-products-in-a-web-page"></a>Paso 2: Mostrar una lista de productos en una página Web

Este tutorial trata sobre cómo utilizar las ObjectDataSource control s características integradas de almacenamiento en caché. Antes de que podemos observar estas características, sin embargo, primero es necesario trabajar desde una página. S permiten crear una página web que usa un control GridView para mostrar información de producto recuperado por un ObjectDataSource desde la `ProductsBLL` clase.

Comience abriendo la `ObjectDataSource.aspx` página en el `Caching` carpeta. Arrastre un control GridView en el cuadro de herramientas hasta el diseñador, establezca su `ID` propiedad `Products`y, en su etiqueta inteligente, elija para enlazar a un nuevo control ObjectDataSource denominado `ProductsDataSource`. Configurar el ObjectDataSource para trabajar con la `ProductsBLL` clase.


[![Configurar el ObjectDataSource para utilizar la clase ProductsBLL](caching-data-with-the-objectdatasource-cs/_static/image7.png)](caching-data-with-the-objectdatasource-cs/_static/image6.png)

**Figura 4**: configurar el ObjectDataSource que se utilizan los `ProductsBLL` clase ([haga clic aquí para ver la imagen a tamaño completo](caching-data-with-the-objectdatasource-cs/_static/image8.png))


Para esta página, permiten s crear un control GridView editable para que podemos examinar lo que ocurre cuando se modifican los datos almacenados en memoria caché en ObjectDataSource a través de la interfaz de s GridView. Dejar la lista desplegable en la ficha Seleccionar con su valor predeterminado, `GetProducts()`, pero cambie el elemento seleccionado en la ficha de actualización para la `UpdateProduct` sobrecarga que acepta `productName`, `unitPrice`, y `productID` como sus parámetros de entrada.


[![Establecer la lista desplegable de actualización pestaña s a la sobrecarga adecuada UpdateProduct](caching-data-with-the-objectdatasource-cs/_static/image10.png)](caching-data-with-the-objectdatasource-cs/_static/image9.png)

**Figura 5**: establece la lista desplegable de ficha actualización s en el adecuado `UpdateProduct` de sobrecarga ([haga clic aquí para ver la imagen a tamaño completo](caching-data-with-the-objectdatasource-cs/_static/image11.png))


Por último, establezca las listas desplegables en las pestañas INSERT y DELETE para (ninguno) y haga clic en Finalizar. Tras completar el Asistente para configurar orígenes de datos, Visual Studio establece la s ObjectDataSource `OldValuesParameterFormatString` propiedad `original_{0}`. Como se describe en el [una visión general de insertar, actualizar y eliminar datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial, esta propiedad debe quitarse de la sintaxis declarativa o vuelve a establecer en su valor predeterminado, `{0}`, en orden para que nuestro flujo de trabajo de actualización continuar sin errores.

Además, al finalizar el Asistente para Visual Studio agrega un campo a la GridView para cada uno de los campos de datos de producto. Quitar todos menos el `ProductName`, `CategoryName`, y `UnitPrice` BoundFields. A continuación, actualice el `HeaderText` propiedades de cada uno de estos BoundFields al producto, la categoría y el precio, respectivamente. Puesto que la `ProductName` campo es obligatorio, convertir el BoundField TemplateField y agregar un control RequiredFieldValidator a la `EditItemTemplate`. Asimismo, convertir el `UnitPrice` BoundField en TemplateField y agregue un control CompareValidator para asegurarse de que el valor especificado por el usuario es una moneda válida valor s mayor que o igual a cero. Además de estas modificaciones, no dude en realizar los cambios estéticos, como Alinear a la derecha el `UnitPrice` valor o especificar el formato de la `UnitPrice` texto en sus interfaces de solo lectura y edición.

Asegúrese de GridView editable activando la casilla de verificación Habilitar edición en la etiqueta inteligente de s de GridView. Compruebe también las casillas de verificación Habilitar paginación y habilitar la ordenación.

> [!NOTE]
> ¿Necesita una revisión de cómo personalizar la interfaz de edición de GridView s? Si es así, hacen referencia a la [personalizar la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) tutorial.


[![Habilitar la compatibilidad de GridView para editar, ordenar y paginar](caching-data-with-the-objectdatasource-cs/_static/image13.png)](caching-data-with-the-objectdatasource-cs/_static/image12.png)

**Figura 6**: habilitar la compatibilidad con GridView para editar, ordenar y paginar ([haga clic aquí para ver la imagen a tamaño completo](caching-data-with-the-objectdatasource-cs/_static/image14.png))


Después de realizar estas modificaciones GridView, el marcado declarativo de s GridView y ObjectDataSource debe ser similar al siguiente:


[!code-aspx[Main](caching-data-with-the-objectdatasource-cs/samples/sample2.aspx)]

Como se muestra en la figura 7, GridView editable muestra el nombre, la categoría y el precio de cada uno de los productos de la base de datos. Tómese un momento para probar el criterio de ordenación de la funcionalidad de página s los resultados, la página a través de ellos y editar un registro.


[![Cada producto es nombre, categoría y el precio se muestran en un Sortable, Pageable, GridView Editable](caching-data-with-the-objectdatasource-cs/_static/image16.png)](caching-data-with-the-objectdatasource-cs/_static/image15.png)

**Figura 7**: s nombre de cada producto, la categoría y el precio se muestra en un Sortable, Pageable, GridView Editable ([haga clic aquí para ver la imagen a tamaño completo](caching-data-with-the-objectdatasource-cs/_static/image17.png))


## <a name="step-3-examining-when-the-objectdatasource-is-requesting-data"></a>Paso 3: Examinar cuando ObjectDataSource el es solicitar datos

El `Products` GridView recupera sus datos para mostrar invocando la `Select` método de la `ProductsDataSource` ObjectDataSource. Este origen ObjectDataSource crea una instancia de la capa de lógica empresarial s `ProductsBLL` clase y llamadas a su `GetProducts()` método, que a su vez llama a la capa de acceso a datos s `ProductsTableAdapter` s `GetProducts()` método. El método de la capa DAL se conecta a la base de datos Northwind y emite el configurado `SELECT` consulta. Estos datos, a continuación, se devuelven a la capa DAL, que empaqueta en un `NorthwindDataTable`. Se devuelve el objeto DataTable a la capa BLL, que devuelve a la ObjectDataSource, que devuelve a la GridView. GridView, a continuación, crea un `GridViewRow` para cada objeto `DataRow` en la tabla de datos y cada uno de ellos `GridViewRow` finalmente se representa en el código HTML que se devuelve al cliente y se muestra en el Explorador de visitante s.

Esta secuencia de eventos se realiza cada vez que GridView debe enlazarse a los datos subyacentes. Esto ocurre cuando se visita la página en primer lugar, cuando se mueve de una página de datos a otro, al ordenar GridView, o al modificar los datos de GridView s a través de su integrado editen o eliminen interfaces. Si se deshabilita el estado de vista de GridView s, se puede Reenlazar la GridView en cada postback también. GridView puede también ser explícitamente vuelve a enlazar a sus datos mediante una llamada a su `DataBind()` método.

Para apreciar la frecuencia con la que se recuperan los datos de la base de datos, deje que s muestre un mensaje que indica si los datos se va a volver a recuperados. Agregar un control Web Label anteriormente GridView denominado `ODSEvents`. Borrar su `Text` propiedad y establezca su `EnableViewState` propiedad `false`. Debajo de la etiqueta, agregue un control Web de botón y establezca su `Text` propiedad a la devolución de datos.


[![Agregar una etiqueta y un botón a la página situada encima de GridView](caching-data-with-the-objectdatasource-cs/_static/image19.png)](caching-data-with-the-objectdatasource-cs/_static/image18.png)

**Figura 8**: agregar una etiqueta y un botón a la página encima de la GridView ([haga clic aquí para ver la imagen a tamaño completo](caching-data-with-the-objectdatasource-cs/_static/image20.png))


Durante el flujo de trabajo de acceso de datos, las operaciones de asignación ObjectDataSource `Selecting` evento se desencadena antes de crea el objeto subyacente y el método configurado que se invoca. Cree un controlador de eventos para este evento y agregue el código siguiente:


[!code-csharp[Main](caching-data-with-the-objectdatasource-cs/samples/sample3.cs)]

Cada vez que el ObjectDataSource realiza una solicitud a la arquitectura de datos, la etiqueta mostrará el evento de selección de texto que se desencadena.

Visite esta página en un explorador. Cuando primero se visita la página, se muestra el evento de selección de texto que se desencadena. Haga clic en el botón de devolución de datos y tenga en cuenta que el texto desaparece (suponiendo que las operaciones de asignación GridView `EnableViewState` propiedad está establecida en `true`, el valor predeterminado). Esto es porque, en la devolución de datos, se reconstruye GridView desde su estado de vista y, por tanto, t activar a ObjectDataSource para sus datos. Ordenación, paginación o modificar los datos, sin embargo, hace GridView a enlazarse a su origen de datos, y, por tanto, desencadena el evento de selección de texto vuelve a aparecer.


[![Cada vez que se vuelve a enlazar el control GridView a su origen de datos, se muestra si selecciona el evento](caching-data-with-the-objectdatasource-cs/_static/image22.png)](caching-data-with-the-objectdatasource-cs/_static/image21.png)

**Figura 9**: GridView el cada vez que se vuelve a enlazar a su origen de datos, se muestra si selecciona el evento ([haga clic aquí para ver la imagen a tamaño completo](caching-data-with-the-objectdatasource-cs/_static/image23.png))


[![Haga clic en el botón de devolución de datos hace GridView a se reconstruye a partir de su estado de vista](caching-data-with-the-objectdatasource-cs/_static/image25.png)](caching-data-with-the-objectdatasource-cs/_static/image24.png)

**Figura 10**: haga clic en el botón de devolución de datos que GridView a se reconstruye a partir de su estado de vista ([haga clic aquí para ver la imagen a tamaño completo](caching-data-with-the-objectdatasource-cs/_static/image26.png))


Puede parecer excesivo recuperar los datos de la base de datos cada vez que se paginar a través o se ordenan los datos. Después de todo, desde que se re use la paginación de manera predeterminada, el ObjectDataSource ha recuperado todos los registros cuando se muestra la primera página. Incluso aunque no proporcione GridView ordenar y paginar el soporte técnico, los datos deben obtenerse de la base de datos cada vez que primero se visita la página por cualquier usuario (y en cada devolución de datos, si el estado de vista está deshabilitado). Pero si GridView muestra los mismos datos para todos los usuarios, estas solicitudes de base de datos adicional superfluas. ¿Por qué no almacenar en caché los resultados devueltos por la `GetProducts()` método y se enlaza el control GridView a los almacenados en memoria caché los resultados?

## <a name="step-4-caching-the-data-using-the-objectdatasource"></a>Paso 4: Almacenamiento en caché los datos a través de ObjectDataSource

Basta con establecer algunas propiedades, ObjectDataSource puede configurarse para que se almacenan automáticamente en caché los datos recuperados en la caché de datos ASP.NET. En la lista siguiente se resume las propiedades relacionadas con la memoria caché de ObjectDataSource:

- [EnableCaching](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.enablecaching.aspx) debe establecerse en `true` para habilitar el almacenamiento en caché. De manera predeterminada, es `false`.
- [CacheDuration](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheduration.aspx) la cantidad de tiempo, en segundos, que se almacena en caché los datos. El valor predeterminado es 0. ObjectDataSource almacenará sólo en memoria caché datos si `EnableCaching` es `true` y `CacheDuration` se establece en un valor mayor que cero.
- [CacheExpirationPolicy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheexpirationpolicy.aspx) se puede establecer en `Absolute` o `Sliding`. Si `Absolute`, ObjectDataSource almacena en caché los datos recuperados para `CacheDuration` segundos; si `Sliding`, expiren los datos solo después de no se ha accedido durante `CacheDuration` segundos. De manera predeterminada, es `Absolute`.
- [CacheKeyDependency](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cachekeydependency.aspx) utilizar esta propiedad para asociar las entradas de caché de ObjectDataSource s con una dependencia de caché existente. Las entradas de datos de ObjectDataSource s se pueda expulsar prematuramente de la memoria caché mediante la expiración de su asociado `CacheKeyDependency`. Esta propiedad normalmente se usa para asociar una dependencia de caché SQL con la caché de s ObjectDataSource, un tema exploraremos en el futuro [utilizando las dependencias de caché de SQL](using-sql-cache-dependencies-cs.md) tutorial.

Permiten s configurar el `ProductsDataSource` ObjectDataSource en caché sus datos durante 30 segundos en escala absoluta. Establecer la s ObjectDataSource `EnableCaching` propiedad `true` y su `CacheDuration` propiedad a 30. Deje el `CacheExpirationPolicy` propiedad establecida en su valor predeterminado, `Absolute`.


[![Configurar el ObjectDataSource para almacenar sus datos en caché durante 30 segundos](caching-data-with-the-objectdatasource-cs/_static/image28.png)](caching-data-with-the-objectdatasource-cs/_static/image27.png)

**Figura 11**: configurar el ObjectDataSource para almacenar sus datos en caché durante 30 segundos ([haga clic aquí para ver la imagen a tamaño completo](caching-data-with-the-objectdatasource-cs/_static/image29.png))


Guarde los cambios y volver a visitar esta página en un explorador. El texto de evento que se activa si selecciona aparecerá cuando visite la página por primera vez como inicialmente los datos no están en la memoria caché. Pero no de postbacks posteriores desencadenados haciendo clic en el botón de devolución de datos, ordenación, paginación, o haga clic en los botones Editar o cancelar *no* texto que desencadena el evento si selecciona volver a mostrar. Esto es porque el `Selecting` evento sólo se desencadena cuando el ObjectDataSource obtiene sus datos de su objeto subyacente; el `Selecting` evento no se desencadena si los datos se extraen desde la caché de datos.

Después de 30 segundos, los datos se expulsen de la memoria caché. Los datos también se eliminan de la caché si las operaciones de asignación ObjectDataSource `Insert`, `Update`, o `Delete` los métodos se invocan. Por lo tanto, una vez transcurridos 30 segundos o el botón de actualización se ha hecho clic, ordenación, paginación, o al hacer clic en los botones Editar o cancelar hará que el ObjectDataSource obtener sus datos de su objeto subyacente, mostrando el evento Selecting desencadena texto cuando el `Selecting` desencadena el evento. Los resultados devueltos se colocan en la caché de datos.

> [!NOTE]
> Si ve el texto de evento que se activa si selecciona con frecuencia, incluso cuando se espera que el ObjectDataSource a trabajar con datos almacenados en caché, es posible debido a restricciones de memoria. Si no hay suficiente memoria libre, los datos agregados a la memoria caché por el ObjectDataSource pueden haber ha eliminación de registros obsoletos. Si t ObjectDataSource parece estar correctamente el almacenamiento en caché los datos o solo las memorias caché los datos de forma esporádica, cierre algunas aplicaciones para liberar memoria y vuelva a intentarlo.


Figura 12 muestra las operaciones de asignación ObjectDataSource almacenamiento en caché de flujo de trabajo. Cuando se desencadena el evento si selecciona el texto aparece en la pantalla, es porque los datos no estaba en la memoria caché y se tuvieron que se recupera del objeto subyacente. Cuando este texto falta, sin embargo, se s porque los datos estuvieran disponibles de la memoria caché. Cuando los datos se devuelven desde la memoria caché ahí s ejecuta ninguna llamada al objeto subyacente y, por lo tanto, ninguna consulta de base de datos.


![Los almacenes de ObjectDataSource y recupera sus datos de la caché de datos](caching-data-with-the-objectdatasource-cs/_static/image30.png)

**Figura 12**: ObjectDataSource almacena y recupera sus datos de la caché de datos


Cada aplicación de ASP.NET tiene su propia memoria caché de datos s comparten todas las páginas y los visitantes de la instancia. Esto significa que los datos almacenados en la caché de datos por el ObjectDataSource igualmente se comparten entre todos los usuarios que visitan la página. Para comprobarlo, abra el `ObjectDataSource.aspx` página en un explorador. Al primero visitar la página, se mostrará el texto de evento que se activa si selecciona (suponiendo que los datos agregados a la memoria caché por las pruebas anteriores se, hasta ahora, ha desalojado). Abra una segunda instancia del explorador y copiar y pegar la dirección URL de la primera instancia del explorador para el segundo. En la segunda instancia del explorador, no se muestra el texto de evento que se activa si selecciona porque lo s usando la misma en memoria caché datos que el primero.

Al insertar los datos recuperados en la memoria caché, ObjectDataSource utiliza un valor de clave de caché que incluye: el `CacheDuration` y `CacheExpirationPolicy` valores de propiedad; el tipo del objeto de negocios subyacente usándola ObjectDataSource, que se especifica a través de la [ `TypeName` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.typename.aspx) (`ProductsBLL`, en este ejemplo); el valor de la `SelectMethod` propiedad y el nombre y los valores de los parámetros en el `SelectParameters` colección; y los valores de sus `StartRowIndex`y `MaximumRows` propiedades, que se usan al implementar [paginación personalizada.](../paging-and-sorting/paging-and-sorting-report-data-cs.md)

Elaborar el valor de clave de caché como una combinación de estas propiedades garantiza una entrada de caché única, como cambian estos valores. Por ejemplo, en los tutoriales anteriores se ha examinado utilizando la `ProductsBLL` clase s. `GetProductsByCategoryID(categoryID)`, que devuelve todos los productos de la categoría especificada. Un usuario puede llegar a la página y vista de bebidas, que tiene un `CategoryID` de 1. Si el ObjectDataSource almacenado en caché los resultados sin tener en cuenta el `SelectParameters` valores, cuando otro usuario ha llegado a la página Ver condimentos mientras los productos de bebidas estuviera en la memoria caché, d verán los productos de bebidas en caché en lugar de condimentos. Mediante la variación de la clave de caché por estas propiedades, que incluyen los valores de la `SelectParameters`, ObjectDataSource mantiene una entrada de caché independiente para bebidas y condimentos.

## <a name="stale-data-concerns"></a>Problemas de datos obsoletos

ObjectDataSource expulsa automáticamente sus elementos de la memoria caché cuando cualquiera de sus `Insert`, `Update`, o `Delete` los métodos se invocan. Esto ayuda a protegerse contra datos obsoletos borrando las entradas de caché cuando se modifican los datos a través de la página. Sin embargo, es posible que un ObjectDataSource con almacenamiento en caché para seguirá mostrando datos obsoletos. En el caso más simple, puede ser debido a los datos de cambio directamente dentro de la base de datos. Quizás un administrador de base de datos ha ejecutado un script que modifica algunos de los registros en la base de datos.

También puede expandir este escenario de una manera más sutil. Aunque el ObjectDataSource expulsa sus elementos de la memoria caché cuando se llama a uno de sus métodos de modificación de datos, quitados los elementos almacenados en caché son para la combinación ObjectDataSource s valores de propiedad (`CacheDuration`, `TypeName`, `SelectMethod`, y así sucesivamente). Si tiene dos ObjectDataSources que usan diferentes `SelectMethods` o `SelectParameters`, pero todavía puede actualizar los mismos datos, a continuación, uno ObjectDataSource puede actualizar una fila e invalidar sus propias entradas de caché, pero la fila correspondiente de la segundo ObjectDataSource todavía se servirá desde las almacenadas en caché. Le animo a crear páginas para exhibir esta funcionalidad. Cree una página que muestra un control GridView editable que extrae los datos de un ObjectDataSource que usa la memoria caché y está configurado para obtener datos de la `ProductsBLL` clase s. `GetProducts()` método. Agregue otro GridView editables y ObjectDataSource a esta página (o de otro), pero para este segundo ObjectDataSource que use la `GetProductsByCategoryID(categoryID)` método. Desde el dos ObjectDataSources `SelectMethod` propiedades son diferentes, se ll cada tienen sus propios valores almacenados en caché. Si edita un producto en una cuadrícula, la próxima vez que enlazar los datos a la cuadrícula de otra (de paginación, ordenación etc.), se seguir atendiendo a los datos almacenados en caché anteriores y no reflejar el cambio que se realiza desde la cuadrícula de otra.

En resumen, utilice caducados basado en tiempo solo si está dispuesto a tienen el potencial de datos obsoletos y utilizar caducados más cortas para escenarios donde la actualización de los datos es importante. Si los datos obsoletos no están aceptables, ya sea renunciar el almacenamiento en caché o usar las dependencias de caché SQL (suponiendo que es la base de datos que se van a almacenamiento en caché). Analizaremos las dependencias de caché SQL en un tutorial posterior.

## <a name="summary"></a>Resumen

En este tutorial se examinan las capacidades de almacenamiento en caché de ObjectDataSource s integradas. Estableciendo simplemente algunas propiedades, se puede indicar a ObjectDataSource para almacenar en caché los resultados devueltos de los especificados `SelectMethod` en la caché de datos ASP.NET. El `CacheDuration` y `CacheExpirationPolicy` propiedades indican la duración se almacena en caché el elemento y si es absoluto o la expiración variable. El `CacheKeyDependency` propiedad todas las entradas de caché de ObjectDataSource s asocia una dependencia de caché existente. Esto puede utilizarse para expulsar las entradas de s ObjectDataSource desde la memoria caché antes de que se alcanza la expiración basada en el tiempo y se suele utilizar con las dependencias de caché SQL.

Puesto que el ObjectDataSource simplemente almacena en caché sus valores a la caché de datos, se pudo replicar la funcionalidad integrada de ObjectDataSource s mediante programación. Tiene sentido hacer esto en el nivel de presentación, ya que el ObjectDataSource ofrece esta funcionalidad de fábrica, pero podemos implementar capacidades de almacenamiento en caché en una capa independiente de la arquitectura. Para ello, necesitamos repetir la misma lógica utilizada por el ObjectDataSource. Exploraremos cómo trabajar mediante programación con la caché de datos desde dentro de la arquitectura en nuestro tutorial siguiente.

Feliz programación.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Almacenamiento en caché de ASP.NET: Técnicas y prácticas recomendadas](https://msdn.microsoft.com/library/aa478965.aspx)
- [Guía de arquitectura de almacenamiento en caché para aplicaciones de .NET Framework](https://msdn.microsoft.com/library/ee817645.aspx)
- [Caché de salida en ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Teresa Murphy. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](caching-data-in-the-architecture-cs.md)
