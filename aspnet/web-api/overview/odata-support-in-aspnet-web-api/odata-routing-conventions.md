---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: Convenciones de enrutamiento en ASP.NET Web API 2 Odata | Documentos de Microsoft
author: MikeWasson
description: "Este artículo describen las convenciones de enrutamiento utiliza API de Web para puntos de conexión de OData."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/31/2013
ms.topic: article
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: 0ab99dd443040b90ffefd2f5b9261a63b91e9463
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="routing-conventions-in-aspnet-web-api-2-odata"></a>Convenciones de enrutamiento en ASP.NET Web API 2 Odata
====================
por [Mike Wasson](https://github.com/MikeWasson)

> Este artículo describen las convenciones de enrutamiento utiliza API de Web para puntos de conexión de OData.


Cuando la API Web recibe una solicitud de OData, asigna la solicitud para un nombre de controlador y un nombre de acción. La asignación se basa en el método HTTP y el URI. Por ejemplo, `GET /odata/Products(1)` se asigna a `ProductsController.GetProduct`.

En la parte 1 de este artículo, describen las convenciones de enrutamiento de OData integradas. Estas convenciones están diseñadas específicamente para los puntos de conexión de OData y reemplazar el sistema de enrutamiento de API Web de manera predeterminada. (El reemplazo se produce cuando se llama a **MapODataRoute**.)

En la parte 2, muestra cómo agregar convenciones de enrutamientos personalizadas. Actualmente las convenciones integradas no se tratan el intervalo completo de OData URIs, pero puede ampliarlas para abordar los casos adicionales.

- [Convenciones de enrutamiento integradas](#conventions)
- [Convenciones de enrutamientos personalizadas](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a>Convenciones de enrutamiento integradas

Antes de que describen las convenciones de enrutamiento de OData en API Web, resulta útil comprender a OData URIs. Un [URI de OData](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/) consta de:

- La raíz del servicio
- La ruta de acceso de recursos
- Opciones de consulta

![](odata-routing-conventions/_static/image1.png)

Para el enrutamiento, lo importante es la ruta de acceso del recurso. La ruta de acceso de recursos se divide en segmentos. Por ejemplo, `/Products(1)/Supplier` tiene tres segmentos:

- `Products`hace referencia a un conjunto de entidades con nombre "Productos".
- `1`es una clave de entidad, al seleccionar una sola entidad del conjunto.
- `Supplier`es una propiedad de navegación que selecciona una entidad relacionada.

Por lo que esta ruta de acceso recoge el proveedor del producto 1.

> [!NOTE]
> Segmentos de ruta de acceso de OData no siempre corresponden a segmentos URI. Por ejemplo, "1" se considera un segmento de ruta de acceso.


**Nombres de controlador.** El nombre del controlador siempre se deriva de la entidad establecida en la raíz de la ruta de acceso del recurso. Por ejemplo, si la ruta de acceso de recurso es `/Products(1)/Supplier`, API Web busca un controlador denominado `ProductsController`.

**Nombres de acción.** Los nombres de acción se derivan de los segmentos de ruta de acceso más el entity data model (EDM), como se muestra en las tablas siguientes. En algunos casos, tendrá dos opciones para el nombre de acción. Por ejemplo, "Get" o &quot;GetProducts&quot;.

**Consulta de entidades**

| Solicitud | URI de ejemplo | Nombre de acción | Acción de ejemplo |
| --- | --- | --- | --- |
| OBTENER /entityset | / Products | GetEntitySet o Get | GetProducts |
| GET /entityset(key) | /Products(1) | GetEntityType o Get | GetProduct |
| GET /entityset(key)/cast | /Products(1)/Models.Book | GetEntityType o Get | GetBook |

Para obtener más información, consulte [crear un extremo de OData de solo lectura](odata-v3/creating-an-odata-endpoint.md).

**Crear, actualizar y eliminar entidades**

| Solicitud | URI de ejemplo | Nombre de acción | Acción de ejemplo |
| --- | --- | --- | --- |
| REGISTRAR /entityset | / Products | PostEntityType o posterior | PostProduct |
| PUT /entityset(key) | /Products(1) | PutEntityType o Put | PutProduct |
| PUT /entityset(key)/cast | /Products(1)/Models.Book | PutEntityType o Put | PutBook |
| REVISIÓN /entityset(key) | /Products(1) | PatchEntityType o revisión | PatchProduct |
| Aplicar la revisión /entityset (clave) / convierte | /Products(1)/Models.Book | PatchEntityType o revisión | PatchBook |
| ELIMINAR /entityset(key) | /Products(1) | DeleteEntityType o Delete | DeleteProduct |
| ELIMINAR /entityset (clave) / convierte | /Products(1)/Models.Book | DeleteEntityType o Delete | DeleteBook |

**Consultar una propiedad de navegación**

| Solicitud | URI de ejemplo | Nombre de acción | Acción de ejemplo |
| --- | --- | --- | --- |
| GET /entityset(key)/navigation | / Products (1) / proveedor | GetNavigationFromEntityType o GetNavigation | GetSupplierFromProduct |
| GET /entityset(key)/cast/navigation | /Products(1)/Models.Book/Author | GetNavigationFromEntityType o GetNavigation | GetAuthorFromBook |

Para obtener más información, consulte [trabajar con relaciones de entidad](odata-v3/working-with-entity-relations.md).

**Crear y eliminar vínculos**

| Solicitud | URI de ejemplo | Nombre de acción |
| --- | --- | --- |
| POST /entityset(key)/$links/navigation | /Products(1)/$links/Supplier | CreateLink |
| PUT /entityset(key)/$links/navigation | /Products(1)/$links/Supplier | CreateLink |
| DELETE /entityset(key)/$links/navigation | /Products(1)/$links/Supplier | DeleteLink |
| DELETE /entityset(key)/$links/navigation(relatedKey) | /Products/(1)/$links/Suppliers(1) | DeleteLink |

Para obtener más información, consulte [trabajar con relaciones de entidad](odata-v3/working-with-entity-relations.md).

**Propiedades**

*Requiere Web API 2*

| Solicitud | URI de ejemplo | Nombre de acción | Acción de ejemplo |
| --- | --- | --- | --- |
| GET /entityset(key)/property | / Products (1) / nombre | GetPropertyFromEntityType o GetProperty | GetNameFromProduct |
| GET /entityset(key)/cast/property | /Products(1)/Models.Book/Author | GetPropertyFromEntityType o GetProperty | GetTitleFromBook |

**Acciones**

| Solicitud | URI de ejemplo | Nombre de acción | Acción de ejemplo |
| --- | --- | --- | --- |
| POST /entityset (clave) / acción | / Products (1) / velocidad | ActionNameOnEntityType o ActionName | RateOnProduct |
| /Entityset (clave) / cast/acción posterior a la | /Products(1)/Models.Book/CheckOut | ActionNameOnEntityType o ActionName | CheckOutOnBook |

Para obtener más información, consulte [las acciones de OData](odata-v3/odata-actions.md).

**Firmas de método**

Estas son algunas reglas para las firmas de método:

- Si la ruta de acceso contiene una clave, la acción debe tener un parámetro denominado *clave*.
- Si la ruta de acceso contiene una clave en una propiedad de navegación, la acción debe tener un parámetro denominado *relatedKey*.
- Decorar *clave* y *relatedKey* parámetros con el **[FromODataUri]** parámetro.
- POST y PUT solicitudes toman un parámetro de tipo de entidad.
- Las solicitudes de revisión toman un parámetro de tipo **Delta&lt;T&gt;**, donde *T* es el tipo de entidad.

Como referencia, mostramos un ejemplo que muestra las firmas de método para cada convención de enrutamiento de OData integrado.

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a>Convenciones de enrutamientos personalizadas

Actualmente las convenciones integradas no tratan a todos los OData URIs posibles. Puede agregar nuevas convenciones implementando la **IODataRoutingConvention** interfaz. Esta interfaz tiene dos métodos:

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- **SelectController** devuelve el nombre del controlador.
- **SelectAction** devuelve el nombre de la acción.

Para ambos métodos, si la convención no se aplica a esa solicitud, el método debe devolver null.

El **ODataPath** parámetro representa la ruta de acceso de recurso de OData analizado. Contiene una lista de  **[ODataPathSegment](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatapathsegment.aspx)**  instancias, uno para cada segmento de la ruta de acceso del recurso. **ODataPathSegment** es una clase abstracta; cada tipo de segmento se representa mediante una clase que deriva de **ODataPathSegment**.

El **ODataPath.TemplatePath** propiedad es una cadena que representa la concatenación de todos los segmentos de ruta de acceso. Por ejemplo, si el identificador URI es `/Products(1)/Supplier`, la plantilla de ruta de acceso es &quot;~/entityset/key/navigation&quot;. Tenga en cuenta que los segmentos no corresponden directamente a segmentos URI. Por ejemplo, la clave de entidad (1) se representa como su propio **ODataPathSegment**.

Normalmente, una implementación de **IODataRoutingConvention** hace lo siguiente:

1. Comparar la plantilla de ruta de acceso para ver si esta convención se aplica a la solicitud actual. Si no se aplica, devuelve null.
2. Si se aplica la convención, use las propiedades de la **ODataPathSegment** instancias para derivar los nombres de acción y controlador.
3. Para las acciones, agregue los valores para el diccionario de ruta que se debe enlazar a los parámetros de acción (normalmente las claves de entidad).

Veamos un ejemplo concreto. Las convenciones de enrutamiento integradas no son compatibles con la indización en una colección de navegación. En otras palabras, no hay ninguna convención para los identificadores URI similar al siguiente:

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

Aquí es una convención de enrutamiento personalizada para controlar este tipo de consulta.

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

Notas:

1. Derivan de **EntitySetRoutingConvention**, porque la **SelectController** método de esa clase es el adecuado para esta nueva convención de enrutamiento. Esto significa que no es necesario volver a implementar **SelectController**.
2. La convención se aplica únicamente a las solicitudes GET, y solo cuando la plantilla de ruta de acceso es &quot;~/entityset/key/navigation/key&quot;.
3. Es el nombre de acción &quot;obtener {EntityType}&quot;, donde *{EntityType}* es el tipo de la colección de navegación. Por ejemplo, &quot;GetSupplier&quot;. Puede usar cualquier convención de nomenclatura que le guste &#8212; Asegúrese de que las acciones de controlador coincide con.
4. La acción toma dos parámetros denominados *clave* y *relatedKey*. (Para obtener una lista de algunos nombres de parámetro predefinidos, vea [ODataRouteConstants](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatarouteconstants.aspx).)

El paso siguiente consiste en agregar la convención de nuevo a la lista de convenciones de enrutamiento. Esto sucede durante la configuración, tal como se muestra en el código siguiente:

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

Estos son algunos otros ejemplo convenciones de enrutamiento que ser útil para estudiar:

- [CompositeKeyRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [CustomNavigationRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [NonBindableActionRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [ODataVersionRouteConstraint](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

Y por supuesto propia API Web es código abierto, para que pueda ver el [código fuente](http://aspnetwebstack.codeplex.com/) para las convenciones de enrutamiento integradas. Se definen en el **System.Web.Http.OData.Routing.Conventions** espacio de nombres.
