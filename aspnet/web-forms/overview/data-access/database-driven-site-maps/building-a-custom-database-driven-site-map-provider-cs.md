---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
title: Creación de un proveedor del mapa del sitio controlado por base de datos personalizados (C#) | Microsoft Docs
author: rick-anderson
description: El proveedor de mapa del sitio predeterminado en ASP.NET 2.0 recupera sus datos desde un archivo XML estático. Mientras que el proveedor basado en XML es adecuado para muchas pequeño y mediano ventana...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: 04b7591d-106f-4f05-87e9-d416cb65a8a6
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
msc.type: authoredcontent
ms.openlocfilehash: 4db59ed13aef81e94feba61299e710bfddd78a76
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838898"
---
<a name="building-a-custom-database-driven-site-map-provider-c"></a>Creación de un proveedor del mapa del sitio controlado por base de datos personalizados (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_CS.zip) o [descargar PDF](building-a-custom-database-driven-site-map-provider-cs/_static/datatutorial62cs1.pdf)

> El proveedor de mapa del sitio predeterminado en ASP.NET 2.0 recupera sus datos desde un archivo XML estático. Aunque el proveedor basado en XML es adecuado para muchos sitios Web de pequeños y medianas, aplicaciones Web más grandes requieren un mapa del sitio más dinámico. En este tutorial que vamos a crear un proveedor del mapa del sitio personalizada que recupera sus datos de la capa de lógica empresarial, que a su vez recupera datos de la base de datos.


## <a name="introduction"></a>Introducción

ASP.NET 2.0 habilita la característica de mapa del sitio s un desarrollador de páginas definir un mapa del sitio web application s en algún medio persistente, como en un archivo XML. Una vez definido, los datos del mapa del sitio pueden obtenerse mediante programación a través del [ `SiteMap` clase](https://msdn.microsoft.com/library/system.web.sitemap.aspx) en el [ `System.Web` espacio de nombres](https://msdn.microsoft.com/library/system.web.aspx) o a través de una variedad de exploración Web controles, como el Controles SiteMapPath, Menu y TreeView. El sistema de sitio usa la [modelo de proveedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) para que se pueden crear implementaciones de serialización de mapa de sitio diferente y conectadas a una aplicación web. El proveedor del mapa de sitio predeterminado que se incluye con ASP.NET 2.0 conserva la estructura de mapa del sitio en un archivo XML. En el [páginas maestras y navegación del sitio](../introduction/master-pages-and-site-navigation-cs.md) tutorial, creamos un archivo denominado `Web.sitemap` que contiene esta estructura y estado actualizando su XML con cada nueva sección del tutorial.

El proveedor del mapa del sitio basado en XML predeterminada funciona bien si la estructura s mapa del sitio es bastante estática, como estos tutoriales. En muchos escenarios, sin embargo, es necesario un mapa del sitio más dinámico. Tenga en cuenta el mapa del sitio que se muestra en la figura 1, donde cada categoría y producto aparecen como secciones de la estructura del sitio Web. Con esta asignación de sitio, visite la página web correspondiente al nodo raíz podría enumerar todas las categorías, mientras que visitar una página web de categoría en particular s haría una lista de los productos de esa categoría s y ver una página web de producto en particular s mostraría ese producto detalles s.


[![Las categorías y productos composición la estructura del mapa s sitio](building-a-custom-database-driven-site-map-provider-cs/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image1.png)

**Figura 1**: las categorías y composición de los productos de la estructura de mapa del sitio s ([haga clic aquí para ver imagen en tamaño completo](building-a-custom-database-driven-site-map-provider-cs/_static/image2.png))


Aunque esta estructura en función de categoría y producto podría ser codificados de forma rígida en el `Web.sitemap` archivo, el archivo deberá actualizarse cada vez que una categoría o producto se ha agregado, quitado o cambiado de nombre. Por lo tanto, si se ha recuperado su estructura de la base de datos o, lo ideal es que, desde la capa de lógica empresarial de la arquitectura de aplicación s sería simplificado de enormemente el mantenimiento de la asignación de sitio. De este modo, como categorías y productos se han agregado, cambiado de nombre o eliminado, el mapa del sitio se actualizaría automáticamente para reflejar estos cambios.

Dado que la serialización de mapa de sitio de ASP.NET 2.0 s se basa en el modelo de proveedor, podemos crear nuestro propio proveedor del mapa del sitio personalizada que toma sus datos desde un almacén de datos alternativo, como la base de datos o la arquitectura. En este tutorial vamos a crear un proveedor personalizado que recupera sus datos de la capa BLL. Introducción s Let!

> [!NOTE]
> El proveedor del mapa del sitio personalizado creado en este tutorial está estrechamente asociado a los modelos de arquitectura y datos de s de aplicación. Jeff Prosise s [almacenar mapas del sitio en SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) y [el proveedor del mapa del sitio SQL ha estado esperando](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx) artículos examine un enfoque generalizado para almacenar los datos del mapa del sitio en SQL Server.


## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>Paso 1: Creación de las páginas Web de sitio personalizada mapa proveedor

Antes de empezar a crear un proveedor del mapa del sitio personalizado, permiten s agregar primero las páginas ASP.NET que necesitaremos para este tutorial. Empiece por agregar una nueva carpeta denominada `SiteMapProvider`. A continuación, agregue las siguientes páginas ASP.NET a la carpeta y asegúrese de asociar cada página con el `Site.master` página maestra:

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

Agregue también un `CustomProviders` subcarpeta para el `App_Code` carpeta.


![Agregar las páginas ASP.NET para los tutoriales relacionados con el proveedor de mapa de sitio](building-a-custom-database-driven-site-map-provider-cs/_static/image2.gif)

**Figura 2**: agregar tutoriales relacionados con el proveedor asignan las páginas ASP.NET para el sitio


Dado que hay solo un tutorial de esta sección, no queremos t necesidad `Default.aspx` para enumerar los tutoriales de sección s. En su lugar, `Default.aspx` mostrará las categorías en un control GridView. Trataremos esto en el paso 2.

A continuación, actualice `Web.sitemap` para incluir una referencia a la `Default.aspx` página. En concreto, agregue el siguiente marcado después el servicio Caching `<siteMapNode>`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample1.xml)]

Después de actualizar `Web.sitemap`, dedique un momento para ver el sitio Web de tutoriales a través de un explorador. El menú de la izquierda incluye ahora un elemento para el tutorial de proveedor del mapa de sitio único.


![El mapa del sitio incluye ahora una entrada para el Tutorial de proveedor del mapa de sitio](building-a-custom-database-driven-site-map-provider-cs/_static/image3.gif)

**Figura 3**: el mapa del sitio incluye ahora una entrada para el Tutorial de proveedor del mapa de sitio


Este enfoque principal s tutorial es ilustrar la creación de un proveedor del mapa del sitio personalizado y configurar una aplicación web para usar dicho proveedor. En concreto, vamos a crear un proveedor que devuelve un mapa del sitio que incluye un nodo raíz junto con un nodo para cada categoría y producto, como se muestra en la figura 1. En general, cada nodo del mapa del sitio puede especificar una dirección URL. Para nuestro mapa del sitio, será la dirección URL raíz del nodo s `~/SiteMapProvider/Default.aspx`, que mostrará una lista de todas las categorías en la base de datos. Cada nodo de categoría en el mapa del sitio tendrá una dirección URL que apunta a `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`, que mostrará una lista de todos los productos en la instancia especificada de *categoryID*. Por último, cada nodo del mapa del sitio del producto apuntará a `~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`, que mostrará los detalles de producto específico s.

Para empezar debemos crear la `Default.aspx`, `ProductsByCategory.aspx`, y `ProductDetails.aspx` páginas. Estas páginas se completan en los pasos 2, 3 y 4, respectivamente. Puesto que el valor de este tutorial es la de los proveedores del mapa del sitio, y puesto que los tutoriales anteriores han tratado de crear informes de estos tipos de maestro y detalles de varias páginas, se le Dese prisa a través de los pasos 2 a 4. Si necesita un actualizador sobre cómo crear informes de maestro y detalles que abarcan varias páginas, vuelva a consultar el [filtrado de maestro y detalles en dos páginas](../masterdetail/master-detail-filtering-across-two-pages-cs.md) tutorial.

## <a name="step-2-displaying-a-list-of-categories"></a>Paso 2: Mostrar una lista de categorías

Abra el `Default.aspx` página en el `SiteMapProvider` carpeta y arrastre un control GridView del cuadro de herramientas hasta el diseñador, establecer su `ID` a `Categories`. En la etiqueta inteligente s GridView, enlazarlo a un nuevo origen ObjectDataSource denominado `CategoriesDataSource` y configúrelo para que recupera sus datos mediante el `CategoriesBLL` clase s `GetCategories` método. Puesto que este GridView sólo muestra las categorías y no proporciona capacidades de modificación de datos, establezca las listas desplegables en la actualización, INSERCIÓN y eliminar pestañas en (None).


[![Configurar el origen ObjectDataSource para devolver las categorías mediante el método GetCategories](building-a-custom-database-driven-site-map-provider-cs/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image3.png)

**Figura 4**: configurar el origen ObjectDataSource a devolver mediante categorías el `GetCategories` método ([haga clic aquí para ver imagen en tamaño completo](building-a-custom-database-driven-site-map-provider-cs/_static/image4.png))


[![Establecer las listas desplegables en la actualización, INSERCIÓN y eliminar las fichas en (None)](building-a-custom-database-driven-site-map-provider-cs/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image5.png)

**Figura 5**: establecer la lista desplegable se enumeran en la actualización, INSERCIÓN y eliminar pestañas en (None) ([haga clic aquí para ver imagen en tamaño completo](building-a-custom-database-driven-site-map-provider-cs/_static/image6.png))


Después de completar el Asistente para configurar orígenes de datos, Visual Studio agregará un BoundField para `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, y `BrochurePath`. Editar el control GridView para que solo contenga el `CategoryName` y `Description` BoundFields y actualizar la `CategoryName` BoundField s `HeaderText` propiedad a la categoría.

A continuación, agregue un campo Hyperlink y colóquelo lo que TI s el campo más a la izquierda. Establecer el `DataNavigateUrlFields` propiedad `CategoryID` y el `DataNavigateUrlFormatString` propiedad `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`. Establecer el `Text` propiedad para ver los productos.


![Agregar un campo Hyperlink a la GridView de categorías](building-a-custom-database-driven-site-map-provider-cs/_static/image6.gif)

**Figura 6**: agregar un campo Hyperlink a la `Categories` GridView


Después de crear el origen ObjectDataSource y personalizar los campos de s GridView, el marcado declarativo de dos controles tendrá un aspecto similar al siguiente:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample2.aspx)]

La figura 7 muestra `Default.aspx` cuando se ve mediante un explorador. Una categoría s ver productos vínculo le llevará al `ProductsByCategory.aspx?CategoryID=categoryID`, que creamos en el paso 3.


[![Cada categoría es aparece junto con un vínculo de productos de vista](building-a-custom-database-driven-site-map-provider-cs/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image7.png)

**Figura 7**: es cada categoría aparece junto con un vínculo de productos de vista ([haga clic aquí para ver imagen en tamaño completo](building-a-custom-database-driven-site-map-provider-cs/_static/image8.png))


## <a name="step-3-listing-the-selected-category-s-products"></a>Paso 3: Lista de los productos de s de la categoría seleccionada

Abra el `ProductsByCategory.aspx` página y agregue un control GridView, asígnele el nombre `ProductsByCategory`. En la etiqueta inteligente, de enlazar el control GridView a un nuevo origen ObjectDataSource denominado `ProductsByCategoryDataSource`. Configurar el origen ObjectDataSource para usar el `ProductsBLL` clase s `GetProductsByCategoryID(categoryID)` método y establezca la lista desplegable se enumera en (None) en las pestañas UPDATE, INSERT y DELETE.


[![Utilice el método de clase ProductsBLL s GetProductsByCategoryID(categoryID)](building-a-custom-database-driven-site-map-provider-cs/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image9.png)

**Figura 8**: Use el `ProductsBLL` clase s `GetProductsByCategoryID(categoryID)` método ([haga clic aquí para ver imagen en tamaño completo](building-a-custom-database-driven-site-map-provider-cs/_static/image10.png))


Solicita el último paso en el Asistente para configurar orígenes de datos para un origen de parámetro *categoryID*. Puesto que esta información se pasa a través del campo de cadena de consulta `CategoryID`, seleccione la cadena de consulta en la lista desplegable y escriba CategoryID en el cuadro de texto QueryStringField tal como se muestra en la figura 9. Haga clic en Finalizar para completar al asistente.


[![Utilice el campo de cadena de consulta CategoryID para el parámetro categoryID](building-a-custom-database-driven-site-map-provider-cs/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image11.png)

**Figura 9**: Use el `CategoryID` Querystring Field de la *categoryID* parámetro ([haga clic aquí para ver imagen en tamaño completo](building-a-custom-database-driven-site-map-provider-cs/_static/image12.png))


Después de completar al asistente, Visual Studio agregará BoundFields correspondiente y un CampoCasillaVerificación en GridView para los campos de datos del producto. Quite todos menos a los `ProductName`, `UnitPrice`, y `SupplierName` BoundFields. Personalizar estos tres BoundFields `HeaderText` propiedades para leer el producto, precio y proveedor, respectivamente. Formato del `UnitPrice` BoundField como una moneda.

A continuación, agregue un campo Hyperlink y muévalo a la posición más a la izquierda. Establecer su `Text` propiedad para ver los detalles, su `DataNavigateUrlFields` propiedad `ProductID`y su `DataNavigateUrlFormatString` propiedad `~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`.


![Agregar un campo de detalles de la vista a Hyperlink que apunta a ProductDetails.aspx](building-a-custom-database-driven-site-map-provider-cs/_static/image10.gif)

**Figura 10**: Agregar campo Hyperlink que señala de una vista de detalles `ProductDetails.aspx`


Después de realizar estas personalizaciones, el marcado declarativo de s GridView y ObjectDataSource debería parecerse al siguiente:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample3.aspx)]

Volver a ver `Default.aspx` a través de un explorador y haga clic en los productos de la vista de vincular las bebidas. Esto le llevará a `ProductsByCategory.aspx?CategoryID=1`, mostrar los nombres, los precios y proveedores de los productos en la base de datos de Northwind que pertenecen a la categoría bebidas (consulte la figura 11). No dude en seguir mejorando esta página para incluir un vínculo para volver a los usuarios a la página de listado de categoría (`Default.aspx`) y un control DetailsView o FormView que muestra el nombre de la categoría seleccionada s y la descripción.


[![Se muestran los nombres de bebidas, precios y proveedores](building-a-custom-database-driven-site-map-provider-cs/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image13.png)

**Figura 11**: se muestran los nombres de bebidas, precios y proveedores ([haga clic aquí para ver imagen en tamaño completo](building-a-custom-database-driven-site-map-provider-cs/_static/image14.png))


## <a name="step-4-showing-a-product-s-details"></a>Paso 4: Mostrar un detalles del producto s

La página final, `ProductDetails.aspx`, muestra los detalles de los productos seleccionados. Abra `ProductDetails.aspx` y arrastre un DetailsView desde el cuadro de herramientas hasta el diseñador. Establezca la s DetailsView `ID` propiedad `ProductInfo` y borre su `Height` y `Width` los valores de propiedad. En la etiqueta inteligente, de enlazar DetailsView a un nuevo origen ObjectDataSource denominado `ProductDataSource`, configurar el origen ObjectDataSource para extraer sus datos de la `ProductsBLL` clase s `GetProductByProductID(productID)` método. Al igual que con las páginas web anteriores creados en los pasos 2 y 3, establecer las listas desplegables en la actualización, INSERCIÓN y eliminar pestañas en (None).


[![Configurar el origen ObjectDataSource para usar el método GetProductByProductID(productID)](building-a-custom-database-driven-site-map-provider-cs/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.png)

**Figura 12**: configurar el origen ObjectDataSource que se usarán el `GetProductByProductID(productID)` método ([haga clic aquí para ver imagen en tamaño completo](building-a-custom-database-driven-site-map-provider-cs/_static/image16.png))


Solicita el último paso del Asistente para configurar origen de datos para el origen de la *productID* parámetro. Puesto que estos datos se presentan a través del campo de cadena de consulta `ProductID`, Establece la lista desplegable en la cadena de consulta y el cuadro de texto QueryStringField en ProductID. Por último, haga clic en el botón Finalizar para completar al asistente.


[![Configurar el parámetro para extraer el valor del campo de cadena de consulta ProductID productID](building-a-custom-database-driven-site-map-provider-cs/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image17.png)

**Figura 13**: configurar el *productID* parámetro para extraer su valor desde el `ProductID` Querystring Field ([haga clic aquí para ver imagen en tamaño completo](building-a-custom-database-driven-site-map-provider-cs/_static/image18.png))


Después de completar al Asistente para configurar orígenes de datos, Visual Studio creará BoundFields correspondiente y un CampoCasillaVerificación en DetailsView para los campos de datos del producto. Quitar el `ProductID`, `SupplierID`, y `CategoryID` BoundFields y configure los campos restantes según estime oportuno. Después de una serie de configuraciones estéticas, mi marcado declarativo de s DetailsView y ObjectDataSource tenía un aspecto similar al siguiente:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample4.aspx)]

Para probar esta página, volver a `Default.aspx` y haga clic en Ver productos para la categoría Bebidas. En la lista de productos de bebidas, haga clic en el vínculo Ver detalles de té Chai. Esto le llevará a `ProductDetails.aspx?ProductID=1`, que muestra un s té Chai obtener información detallada (vea la figura 14).


[![Se muestra el té Chai s proveedor, categoría, precios y otra información](building-a-custom-database-driven-site-map-provider-cs/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image19.png)

**Figura 14**: té Chai s proveedor, categoría, precios y otra información se muestra ([haga clic aquí para ver imagen en tamaño completo](building-a-custom-database-driven-site-map-provider-cs/_static/image20.png))


## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>Paso 5: Comprender el funcionamiento interno de un proveedor del mapa del sitio

El mapa del sitio se representa en la memoria de servidor s web como una colección de `SiteMapNode` instancias que forman una jerarquía. Debe haber exactamente una raíz, todos los nodos no raíz deben tener exactamente un nodo primario y todos los nodos pueden tener un número arbitrario de elementos secundarios. Cada `SiteMapNode` objeto representa una sección de la estructura del sitio Web s; estas secciones tienen normalmente una página web correspondiente. Por lo tanto, el [ `SiteMapNode` clase](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) tiene propiedades como `Title`, `Url`, y `Description`, que proporcionan información de la sección la `SiteMapNode` representa. También hay un `Key` propiedad que identifica de forma única cada `SiteMapNode` en la jerarquía, así como las propiedades utilizadas para establecer esta jerarquía `ChildNodes`, `ParentNode`, `NextSibling`, `PreviousSibling`, y así sucesivamente.

Figura 15 muestra la estructura de mapa del sitio general de la figura 1, pero con los detalles de implementación esbozados con más detalle.


[![Cada SiteMapNode tiene propiedades, como título, dirección Url, clave etc.](building-a-custom-database-driven-site-map-provider-cs/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.gif)

**Figura 15**: cada `SiteMapNode` tiene propiedades como `Title`, `Url`, `Key`, y así sucesivamente ([haga clic aquí para ver imagen en tamaño completo](building-a-custom-database-driven-site-map-provider-cs/_static/image17.gif))


El mapa del sitio es accesible a través de la [ `SiteMap` clase](https://msdn.microsoft.com/library/system.web.sitemap.aspx) en el [ `System.Web` espacio de nombres](https://msdn.microsoft.com/library/system.web.aspx). Esta clase s `RootNode` propiedad devuelve la raíz del mapa s sitio `SiteMapNode` ejemplo; `CurrentNode` devuelve el `SiteMapNode` cuyo `Url` propiedad coincide con la dirección URL de la página solicitada actualmente. Esta clase se usa internamente los controles Web de ASP.NET 2.0 s navegación.

Cuando el `SiteMap` se tiene acceso a las propiedades de la clase s, debe serializar la estructura de mapa del sitio de algún medio persistente en la memoria. Sin embargo, la lógica de serialización de mapa de sitio es modificable en la `SiteMap` clase. En su lugar, en tiempo de ejecución el `SiteMap` clase determina qué mapa del sitio *proveedor* a utilizar para la serialización. De forma predeterminada, el [ `XmlSiteMapProvider` clase](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx) se usa, que lee la estructura del mapa s sitio desde un archivo XML de formato correcto. Sin embargo, podemos crear nuestro propio proveedor del mapa del sitio personalizado con un poco de trabajo.

Todos los proveedores del mapa del sitio deben derivarse de la [ `SiteMapProvider` clase](https://msdn.microsoft.com/library/system.web.sitemapprovider.aspx), que incluye los métodos esenciales y propiedades necesarias para el sitio de proveedores del mapa, pero omite muchos de los detalles de implementación. Una segunda clase, [ `StaticSiteMapProvider` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.aspx), amplía el `SiteMapProvider` clase y contiene una implementación más sólida de la funcionalidad necesaria. Internamente, el `StaticSiteMapProvider` almacena el `SiteMapNode` instancias del sitio se asignan en un `Hashtable` y proporciona métodos como `AddNode(child, parent)`, `RemoveNode(siteMapNode),` y `Clear()` que agreguen y quiten `SiteMapNode` s a interno `Hashtable`. La clase `XmlSiteMapProvider` se deriva de la clase `StaticSiteMapProvider`.

Al crear un proveedor del mapa del sitio personalizado que extiende `StaticSiteMapProvider`, hay dos métodos abstractos que deben invalidarse: [ `BuildSiteMap` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.buildsitemap.aspx) y [ `GetRootNodeCore` ](https://msdn.microsoft.com/library/system.web.sitemapprovider.getrootnodecore.aspx). `BuildSiteMap`, como su nombre implica, es responsable de cargar la estructura de mapa del sitio desde el almacenamiento persistente y construirlo en memoria. `GetRootNodeCore` Devuelve el nodo raíz del mapa del sitio.

Antes de una web de aplicación puede utilizar un proveedor del mapa del sitio que se debe registrar en la configuración de aplicación s. De forma predeterminada, el `XmlSiteMapProvider` se registre con el nombre `AspNetXmlSiteMapProvider`. Para registrar los proveedores del mapa de sitio adicionales, agregue el siguiente marcado para `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample5.xml)]

El *nombre* valor asigna un nombre legible para el proveedor al *tipo* especifica el nombre de tipo completo del proveedor del mapa del sitio. Exploraremos valores concretos para el *nombre* y *tipo* valores en el paso 7, después se ve creado nuestro proveedor del mapa del sitio personalizado.

La clase de proveedor del mapa de sitio se crea una instancia de la primera vez que se accede desde el `SiteMap` clase y permanece en memoria durante la vigencia de la aplicación web. Dado que hay solo una instancia del proveedor del mapa del sitio que se puede invocar desde varios, los visitantes del sitio web simultáneas, es imperativo que los métodos del proveedor de s sea *subprocesos*.

Para razones de rendimiento y escalabilidad, lo importante que se almacenará en caché el sitio en la memoria de asignar la estructura y devolver esto almacenar en caché de estructura, en lugar de volver a crear cada vez el `BuildSiteMap` se invoca el método. `BuildSiteMap` puede llamarse varias veces por cada solicitud de página por usuario, dependiendo de los controles de navegación en uso en la página y la profundidad de la estructura de mapa del sitio. En cualquier caso, si la estructura de mapa del sitio se almacenará en caché no `BuildSiteMap` , a continuación, cada vez que se invoque sería necesario volver a recuperar la información de producto y categoría de la arquitectura (lo que daría como resultado de una consulta a la base de datos). Como se explicó en los tutoriales anteriores de almacenamiento en caché, datos almacenados en caché pueden quedar obsoletos. Para combatir esta situación, podemos usar tiempo o expirados de dependencia de caché SQL.

> [!NOTE]
> Un proveedor del mapa del sitio si lo desea puede invalidar el [ `Initialize` método](https://msdn.microsoft.com/library/system.web.sitemapprovider.initialize.aspx). `Initialize` se invoca cuando el proveedor del mapa del sitio, primero se crea una instancia y se pasa a los atributos personalizados asignados al proveedor en `Web.config` en el `<add>` elemento, como: `<add name="name" type="type" customAttribute="value" />`. Resulta útil si desea permitir que un desarrollador de páginas especificar la configuración de relacionados con el proveedor de mapa de sitio distintos sin tener que modificar el código s del proveedor. Por ejemplo, si nos estábamos leer los datos de categoría y productos directamente desde la base de datos en lugar de hacerlo a través de la arquitectura, d, probablemente desee permiten a los programadores de la página Especificar la cadena de conexión de base de datos a través de `Web.config` en lugar de usar un codificado valor en el código del proveedor s. El proveedor del mapa del sitio personalizado vamos a crear en el paso 6 no reemplazar esto `Initialize` método. Para obtener un ejemplo del uso de la `Initialize` método, consulte [Jeff Prosise](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) s [almacenar mapas del sitio en SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) artículo.


## <a name="step-6-creating-the-custom-site-map-provider"></a>Paso 6: Crear el proveedor del mapa del sitio personalizada

Para crear un proveedor del mapa del sitio personalizado que se basa el mapa del sitio de las categorías y productos en la base de datos Northwind, necesitamos crear una clase que extiende `StaticSiteMapProvider`. En el paso 1 ha pedido que agregue un `CustomProviders` carpeta en el `App_Code` carpeta - agregar una nueva clase a esta carpeta denominada `NorthwindSiteMapProvider`. Agregue el código siguiente a la clase `NorthwindSiteMapProvider` :


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample6.cs)]

Permiten s comenzará por explorar esta clase s `BuildSiteMap` método, que comienza con un [ `lock` instrucción](https://msdn.microsoft.com/library/c5kehkcz.aspx). El `lock` instrucción solo permite un subproceso cada vez que escriba, con lo que se serializa el acceso a su código y evitando que dos subprocesos simultáneos de ejecución paso a paso en código era de s entre sí.

El nivel de clase `SiteMapNode` variable `root` se usa para almacenar en caché de la estructura de mapa del sitio. Cuando se construye el mapa del sitio por primera vez, o por primera vez después de que se ha modificado los datos subyacentes, `root` será `null` y se construirá la estructura de mapa del sitio. El nodo del mapa s raíz del sitio se asigna a `root` durante la construcción para que la próxima vez que este método se denomina proceso, `root` no será `null`. Por lo tanto, siempre y cuando `root` no `null` la estructura de mapa del sitio se devolverá al llamador sin tener que volver a crearla.

Si es la raíz `null`, se crea la estructura de mapa del sitio de la información de producto y categoría. El mapa del sitio se compila mediante la creación de la `SiteMapNode` instancias y, a continuación, que forman la jerarquía a través de las llamadas a la `StaticSiteMapProvider` clase s `AddNode` método. `AddNode` realiza la contabilización interna, almacenar las diversas `SiteMapNode` instancias en un `Hashtable`. Antes de empezar a construir la jerarquía, comenzamos llamando el `Clear` método, que borra los elementos de interno `Hashtable`. A continuación, el `ProductsBLL` clase s `GetProducts` método y resultante `ProductsDataTable` se almacenan en variables locales.

La construcción de mapa s sitio comienza por crear el nodo raíz y asignarlo a `root`. La sobrecarga de la [ `SiteMapNode` constructor s](https://msdn.microsoft.com/library/system.web.sitemapnode.sitemapnode.aspx) utilizado aquí y en todo esto `BuildSiteMap` se pasa la información siguiente:

- Una referencia al proveedor del mapa del sitio (`this`).
- El `SiteMapNode` s `Key`. Esto requiere el valor debe ser único para cada `SiteMapNode`.
- El `SiteMapNode` s `Url`. `Url` es opcional, pero si se proporciona, cada uno de ellos `SiteMapNode` s `Url` valor debe ser único.
- El `SiteMapNode` s `Title`, lo cual es necesario.

El `AddNode(root)` llamada al método agrega el `SiteMapNode` `root` al mapa del sitio como raíz. A continuación, cada `ProductRow` en el `ProductsDataTable` se enumera. Si ya existe un `SiteMapNode` para la categoría de producto s actual, se hace referencia. En caso contrario, un nuevo `SiteMapNode` para la categoría se crea y se agrega como elemento secundario de la `SiteMapNode``root` a través de la `AddNode(categoryNode, root)` llamada al método. Después de la categoría correspondiente `SiteMapNode` nodo se han encontrado o creado, un `SiteMapNode` se creó para el producto actual y se agrega como elemento secundario de la categoría `SiteMapNode` a través de `AddNode(productNode, categoryNode)`. Tenga en cuenta que la categoría `SiteMapNode` s `Url` es el valor de propiedad `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID` mientras que el producto `SiteMapNode` s `Url` se asigna la propiedad `~/SiteMapNode/ProductDetails.aspx?ProductID=productID`.

> [!NOTE]
> Aquellos productos que tienen una base de datos `NULL` valor para sus `CategoryID` se agrupan en una categoría `SiteMapNode` cuyo `Title` propiedad está establecida en None y cuyo `Url` propiedad está establecida en una cadena vacía. He decidido establecer `Url` en una cadena vacía desde el `ProductBLL` clase s `GetProductsByCategory(categoryID)` método actualmente no tiene la capacidad de devolver los productos con un `NULL` `CategoryID` valor. Además, yo quería demostrar cómo se representan los controles de navegación una `SiteMapNode` que no tiene un valor para su `Url` propiedad. Le animo a ampliar este tutorial para que ningún `SiteMapNode` s `Url` propiedad apunta a `ProductsByCategory.aspx`, aún muestra sólo los productos con `NULL` `CategoryID` valores.


Después de crear el mapa del sitio, se agrega un objeto arbitrario a la caché de datos con una dependencia de caché SQL en el `Categories` y `Products` tablas a través de un `AggregateCacheDependency` objeto. Exploramos mediante dependencias de caché de SQL en el tutorial anterior, *utilizando las dependencias de caché de SQL*. El proveedor del mapa del sitio personalizado, sin embargo, usa una sobrecarga de la caché de datos s `Insert` método que se ve aún para explorar. Esta sobrecarga acepta como parámetro de entrada final un delegado que se llama cuando se quita el objeto de la memoria caché. En concreto, se pasa en un nuevo [ `CacheItemRemovedCallback` delegar](https://msdn.microsoft.com/library/system.web.caching.cacheitemremovedcallback.aspx) que apunta a la `OnSiteMapChanged` método definido más adelante en el `NorthwindSiteMapProvider` clase.

> [!NOTE]
> La representación en memoria del mapa del sitio se almacena en caché a través de la variable de nivel de clase `root`. Dado que hay solo una instancia de la clase de proveedor del mapa de sitio personalizada y, puesto que dicha instancia se comparte entre todos los subprocesos en la aplicación web, esta variable de clase actúa como una memoria caché. El `BuildSiteMap` método también utiliza la caché de datos, pero solo como un medio para recibir una notificación cuando la base de datos en la base de datos la `Categories` o `Products` las tablas de cambios. Tenga en cuenta que el valor que se colocan en la caché de datos es simplemente la fecha y hora actuales. Los datos del mapa del sitio real están *no* poner en la caché de datos.


El `BuildSiteMap` se completa el método devolviendo el nodo raíz del mapa del sitio.

Los métodos restantes son bastante sencillos. `GetRootNodeCore` es responsable de devolver el nodo raíz. Puesto que `BuildSiteMap` devuelve la raíz, `GetRootNodeCore` simplemente devuelve `BuildSiteMap` s valor devuelto. El `OnSiteMapChanged` método establece `root` a `null` cuando se quita el elemento en caché. Con raíz vuelve a establecer en `null`, la próxima vez `BuildSiteMap` es invoca, la estructura de mapa del sitio se volverá a generar. Por último, el `CachedDate` propiedad devuelve el valor de fecha y hora almacenado en la caché de datos, si existe dicho valor. Esta propiedad se puede usar un desarrollador de páginas para determinar cuándo los datos del mapa del sitio se última almacenan en caché.

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>Paso 7: Registrar el`NorthwindSiteMapProvider`

En orden para nuestra aplicación web para usar el `NorthwindSiteMapProvider` proveedor del mapa del sitio creado en el paso 6, es necesario registrarlo en el `<siteMap>` sección de `Web.config`. En concreto, agregue el siguiente marcado dentro de la `<system.web>` elemento `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample7.xml)]

Este marcado hace dos cosas: en primer lugar, indica que la integrada `AspNetXmlSiteMapProvider` es el proveedor de mapa del sitio predeterminado; en segundo lugar, registra el proveedor del mapa del sitio personalizado creado en el paso 6 con el nombre fácil de usar Northwind.

> [!NOTE]
> Para los proveedores del mapa del sitio ubicados en la aplicación s `App_Code` carpeta, el valor de la `type` atributo es simplemente el nombre de clase. Como alternativa, el proveedor del mapa del sitio personalizado podría haber creado en un proyecto de biblioteca de clases independiente con el ensamblado compilado que se colocan en la s de la aplicación web `/Bin` directory. En ese caso, el `type` sería el valor del atributo *Namespace*. *ClassName*, *AssemblyName* .


Después de actualizar `Web.config`, tómese un momento para ver cualquier página de los tutoriales en un explorador. Tenga en cuenta que la interfaz de navegación de la izquierda aún muestra las secciones y tutoriales que se definen en `Web.sitemap`. Esto es porque hemos dejado `AspNetXmlSiteMapProvider` como el proveedor predeterminado. Para crear un elemento de interfaz de usuario de navegación que usa el `NorthwindSiteMapProvider`, necesitamos especificar explícitamente que se debe usar el proveedor del mapa del sitio Northwind. Veremos cómo realizar esta tarea en el paso 8.

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>Paso 8: Mostrar información del mapa del sitio mediante el proveedor del mapa del sitio personalizada

Con el sitio personalizado proveedor del mapa creado y registrado en `Web.config`, que está listo para agregar controles de navegación a la `Default.aspx`, `ProductsByCategory.aspx`, y `ProductDetails.aspx` páginas en el `SiteMapProvider` carpeta. Comience abriendo la `Default.aspx` página y arrastre un `SiteMapPath` desde el cuadro de herramientas hasta el diseñador. El control SiteMapPath se encuentra en la sección de navegación del cuadro de herramientas.


[![Agregar un SiteMapPath a Default.aspx](building-a-custom-database-driven-site-map-provider-cs/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image18.gif)

**Figura 16**: agregar un SiteMapPath a `Default.aspx` ([haga clic aquí para ver imagen en tamaño completo](building-a-custom-database-driven-site-map-provider-cs/_static/image20.gif))


El control SiteMapPath muestra una ruta de navegación, que indica la ubicación actual de s página dentro del mapa del sitio. Se ha agregado un SiteMapPath a la parte superior de la página maestra en el *páginas maestras y navegación del sitio* tutorial.

Dedique un momento para ver esta página a través de un explorador. Agregado en la figura 16 SiteMapPath usa el proveedor de mapa del sitio predeterminado, extraer sus datos `Web.sitemap`. Por lo tanto, el elemento breadcrumb muestra inicio &gt; personalizar el mapa del sitio, al igual que la ruta de navegación en la esquina superior derecha.


[![La ruta de navegación usa el proveedor del mapa del sitio predeterminado](building-a-custom-database-driven-site-map-provider-cs/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.gif)

**Figura 17**: la ruta de navegación usa el proveedor del mapa del sitio predeterminado ([haga clic aquí para ver imagen en tamaño completo](building-a-custom-database-driven-site-map-provider-cs/_static/image23.gif))


Para hacer el SiteMapPath agregado en la figura 16 usar el proveedor del mapa del sitio personalizado se creó en el paso 6, establezca su [ `SiteMapProvider` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx) a Northwind, el nombre se asigna a la `NorthwindSiteMapProvider` en `Web.config`. Desafortunadamente, el diseñador continúa usando el proveedor del mapa del sitio predeterminado, pero si visita la página a través del explorador después de realizar este cambio de propiedad verá que la ruta de navegación ahora usa el proveedor del mapa del sitio personalizado.


[![La ruta de navegación usa ahora la NorthwindSiteMapProvider de proveedor del mapa de sitio personalizado](building-a-custom-database-driven-site-map-provider-cs/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image24.gif)

**Figura 18**: la ruta de navegación ahora usa el proveedor del mapa del sitio personalizado `NorthwindSiteMapProvider` ([haga clic aquí para ver imagen en tamaño completo](building-a-custom-database-driven-site-map-provider-cs/_static/image26.gif))


El control SiteMapPath muestra una interfaz de usuario más funcional en el `ProductsByCategory.aspx` y `ProductDetails.aspx` páginas. Agregar un SiteMapPath a estas páginas, establecer el `SiteMapProvider` propiedad en ambos a Northwind. Desde `Default.aspx` haga clic en el vínculo Ver productos de bebidas y, a continuación, en el vínculo Ver detalles de té Chai. Como se muestra en la figura 19, la ruta de navegación incluye la sección de mapa del sitio actual (té Chai) y sus antecesores: bebidas y todas las categorías.


[![La ruta de navegación usa ahora la NorthwindSiteMapProvider de proveedor del mapa de sitio personalizado](building-a-custom-database-driven-site-map-provider-cs/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.png)

**Figura 19**: la ruta de navegación ahora usa el proveedor del mapa del sitio personalizado `NorthwindSiteMapProvider` ([haga clic aquí para ver imagen en tamaño completo](building-a-custom-database-driven-site-map-provider-cs/_static/image22.png))


Otros elementos de interfaz de usuario de navegación se pueden usar además SiteMapPath, como los controles Menu y TreeView. El `Default.aspx`, `ProductsByCategory.aspx`, y `ProductDetails.aspx` páginas en la descarga para este tutorial, por ejemplo, todos incluyen controles de menú (Véase la figura 20). Vea [s de examen de ASP.NET 2.0 Site Navigation Features](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) y [mediante controles de navegación del sitio](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx) sección de la [inicios rápidos de ASP.NET 2.0](https://quickstarts.asp.net/QuickStartv20/aspnet/) para una información más detallada sobre el los controles de navegación y el sistema de asignación de sitio en ASP.NET 2.0.


[![El Control de menú muestra cada una de las categorías y productos](building-a-custom-database-driven-site-map-provider-cs/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image28.gif)

**Figura 20**: el menú Control enumera cada uno de las categorías y productos ([haga clic aquí para ver imagen en tamaño completo](building-a-custom-database-driven-site-map-provider-cs/_static/image30.gif))


Como se mencionó anteriormente en este tutorial, la estructura de mapa del sitio puede tener acceso mediante programación a través del `SiteMap` clase. El código siguiente devuelve la raíz `SiteMapNode` del proveedor predeterminado:


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample8.cs)]

Puesto que la `AspNetXmlSiteMapProvider` es el proveedor predeterminado para nuestra aplicación, el código anterior devolverá el nodo raíz definido en `Web.sitemap`. Para hacer referencia a un proveedor del mapa del sitio distinto del predeterminado, utilice el `SiteMap` clase s [ `Providers` propiedad](https://msdn.microsoft.com/library/system.web.sitemap.providers.aspx) así:


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample9.cs)]

Donde *nombre* es el nombre del proveedor del mapa del sitio personalizado (para nuestra aplicación web, Northwind).

Para obtener acceso a un miembro específico para un proveedor del mapa del sitio, use `SiteMap.Providers["name"]` para recuperar la instancia del proveedor y, a continuación, convertirlo al tipo adecuado. Por ejemplo, para mostrar el `NorthwindSiteMapProvider` s `CachedDate` propiedad en una página ASP.NET, utilice el código siguiente:


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample10.cs)]

> [!NOTE]
> No olvide probar la característica de dependencias de caché SQL. Después de visitar el `Default.aspx`, `ProductsByCategory.aspx`, y `ProductDetails.aspx` páginas, vaya a uno de los tutoriales en la edición, inserción y eliminación de la sección y cambie el nombre de un producto o categoría. A continuación, volver a una de las páginas de la `SiteMapProvider` carpeta. Suponiendo que ha transcurrido tiempo suficiente para que el mecanismo de sondeo a tener en cuenta el cambio en la base de datos subyacente, se debe actualizar el mapa del sitio para mostrar el nuevo producto o el nombre de categoría.


## <a name="summary"></a>Resumen

ASP.NET 2.0 incluye características de mapa del sitio s un `SiteMap` (clase), un número de navegación integrada Web controles y un proveedor del mapa de sitio predeterminado que se espera que la información de mapa del sitio se almacena en un archivo XML. Para poder usar la información del mapa del sitio de algún otro origen, como desde una base de datos, la arquitectura de s de la aplicación o un servicio Web remoto que es necesario crear un proveedor del mapa del sitio personalizado. Esto implica la creación de una clase que deriva, directa o indirectamente, de la `SiteMapProvider` clase.

En este tutorial hemos visto cómo crear un proveedor del mapa del sitio personalizado que según la información de producto y categoría llamada desde la arquitectura de aplicación en el mapa del sitio. Nuestro proveedor extendido el `StaticSiteMapProvider` clase y crear conlleva un `BuildSiteMap` método que recupera los datos, construye la jerarquía del mapa del sitio y almacena en caché la estructura resultante en una variable de nivel de clase. Usamos una dependencia de caché SQL con una función de devolución de llamada para invalidar el almacenado en caché cuando estructura subyacente `Categories` o `Products` se modifican los datos.

Feliz programación.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Almacenar mapas de sitio en SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) y [el proveedor del mapa del sitio SQL ha estado esperando](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)
- [Modelo de proveedor de s de un vistazo a ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [El Kit de herramientas de proveedor](https://msdn.microsoft.com/asp.net/aa336558.aspx)
- [Examen de ASP.NET 2.0 s Site Navigation Features](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Dave Gardner, Zack Jones, Teresa Murphy y Bernadette Leigh. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](building-a-custom-database-driven-site-map-provider-vb.md)
