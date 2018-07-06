---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: Convenciones de enrutamiento en ASP.NET Web API 2 Odata | Microsoft Docs
author: MikeWasson
description: En este artículo se describe las convenciones de enrutamiento utiliza API de Web para puntos de conexión de OData.
ms.author: aspnetcontent
ms.date: 07/31/2013
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: 645e820d3b03d1e3d2ac088973f6296efa5c2f40
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818270"
---
<a name="routing-conventions-in-aspnet-web-api-2-odata"></a>Convenciones de enrutamiento en ASP.NET Web API 2 Odata
====================
por [Mike Wasson](https://github.com/MikeWasson)

> En este artículo se describe las convenciones de enrutamiento utiliza API de Web para puntos de conexión de OData.


Cuando la API Web obtiene una solicitud de OData, asigna la solicitud para un nombre de controlador y un nombre de acción. La asignación se basa en el método HTTP y el URI. Por ejemplo, `GET /odata/Products(1)` se asigna a `ProductsController.GetProduct`.

En la parte 1 de este artículo, describen las convenciones de enrutamiento de OData integradas. Estas convenciones están diseñadas específicamente para los puntos de conexión de OData y reemplazar el sistema de enrutamiento de Web API predeterminada. (El reemplazo se produce cuando se llama a **MapODataRoute**.)

En la parte 2, muestra cómo agregar convenciones de enrutamientos personalizadas. Actualmente las convenciones integradas no cubren la gama completa de OData URIs, pero puede ampliarlos para controlar casos adicionales.

- [Convenciones de enrutamientos integradas](#conventions)
- [Convenciones de enrutamientos personalizadas](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a>Convenciones de enrutamientos integradas

Antes de que describen las convenciones de enrutamiento de OData en Web API, resulta útil comprender a OData URIs. Un [URI de OData](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/) consta de:

- La raíz del servicio
- La ruta de acceso del recurso
- Opciones de consulta

![](odata-routing-conventions/_static/image1.png)

Para el enrutamiento, lo importante es la ruta de acceso del recurso. La ruta de acceso de recursos se divide en segmentos. Por ejemplo, `/Products(1)/Supplier` tiene tres segmentos:

- `Products` hace referencia a un conjunto de entidades denominado "Productos".
- `1` es una clave de entidad, seleccionar una sola entidad del conjunto.
- `Supplier` es una propiedad de navegación que selecciona una entidad relacionada.

Por lo que esta ruta de acceso recoge el proveedor del producto 1.

> [!NOTE]
> Segmentos de ruta de acceso de OData no siempre se corresponde con segmentos de URI. Por ejemplo, "1" se considera un segmento de ruta de acceso.


**Nombres de controlador.** El nombre del controlador siempre se deriva de la entidad establecidos en la raíz de la ruta de acceso del recurso. Por ejemplo, si la ruta de acceso del recurso es `/Products(1)/Supplier`, Web API busca un controlador denominado `ProductsController`.

**Nombres de acción.** Los nombres de acción se derivan de entity data model (EDM), además de los segmentos de ruta de acceso, como se muestra en las tablas siguientes. En algunos casos, tiene dos opciones para el nombre de acción. Por ejemplo, "Get" o &quot;GetProducts&quot;.

**Consulta de entidades**

| Solicitud | URI de ejemplo | Nombre de acción | Acción de ejemplo |
| --- | --- | --- | --- |
| OBTENER /entityset | / Products | GetEntitySet o Get | GetProducts |
| OBTENER /entityset(key) | /Products(1) | GetEntityType o Get | GetProduct |
| OBTENER /entityset (clave) / conversión | / /Models.Book products (1) | GetEntityType o Get | GetBook |

Para obtener más información, consulte [crear un extremo de OData de solo lectura](odata-v3/creating-an-odata-endpoint.md).

**Crear, actualizar y eliminar entidades**

| Solicitud | URI de ejemplo | Nombre de acción | Acción de ejemplo |
| --- | --- | --- | --- |
| REGISTRAR /entityset | / Products | PostEntityType o Post | PostProduct |
| COLOCAR /entityset(key) | /Products(1) | PutEntityType o Put | PutProduct |
| COLOCAR /entityset (clave) / conversión | / /Models.Book products (1) | PutEntityType o Put | PutBook |
| REVISIÓN /entityset(key) | /Products(1) | PatchEntityType o revisión | PatchProduct |
| PATCH /entityset (clave) / conversión | / /Models.Book products (1) | PatchEntityType o revisión | PatchBook |
| ELIMINAR /entityset(key) | /Products(1) | DeleteEntityType o Delete | DeleteProduct |
| ELIMINAR /entityset (clave) / conversión | / /Models.Book products (1) | DeleteEntityType o Delete | DeleteBook |

**Consultar una propiedad de navegación**

| Solicitud | URI de ejemplo | Nombre de acción | Acción de ejemplo |
| --- | --- | --- | --- |
| GET /entityset (clave) y navegación | / Products (1) / proveedor | GetNavigationFromEntityType o GetNavigation | GetSupplierFromProduct |
| OBTENER navegación/cast//entityset (clave) | / /Models.Book/Author products (1) | GetNavigationFromEntityType o GetNavigation | GetAuthorFromBook |

Para obtener más información, consulte [trabajar con relaciones de entidad](odata-v3/working-with-entity-relations.md).

**Creación y eliminación de vínculos**

| Solicitud | URI de ejemplo | Nombre de acción |
| --- | --- | --- |
| POST /entityset (clave) / $links/navegación | / Vínculos de (1) / $ products/proveedor | CreateLink |
| PUT /entityset (clave) / $links/navegación | / Vínculos de (1) / $ products/proveedor | CreateLink |
| ELIMINAR /entityset (clave) / $links/navegación | / Vínculos de (1) / $ products/proveedor | DeleteLink |
| ELIMINAR /entityset(key)/$links/navigation(relatedKey) | /Products/(1)/$Links/Suppliers(1) | DeleteLink |

Para obtener más información, consulte [trabajar con relaciones de entidad](odata-v3/working-with-entity-relations.md).

**Propiedades**

*Requiere que Web API 2*

| Solicitud | URI de ejemplo | Nombre de acción | Acción de ejemplo |
| --- | --- | --- | --- |
| GET /entityset (clave) o propiedad | / Products (1) / nombre | GetPropertyFromEntityType o GetProperty | GetNameFromProduct |
| OBTENER la propiedad/cast//entityset (clave) | / /Models.Book/Author products (1) | GetPropertyFromEntityType o GetProperty | GetTitleFromBook |

**Acciones**

| Solicitud | URI de ejemplo | Nombre de acción | Acción de ejemplo |
| --- | --- | --- | --- |
| POST /entityset (clave) / acción | / Products (1) / velocidad | ActionNameOnEntityType o ActionName | RateOnProduct |
| REGISTRAR acciones/conversión o /entityset (clave) | / /Models.Book/CheckOut products (1) | ActionNameOnEntityType o ActionName | CheckOutOnBook |

Para obtener más información, consulte [acciones OData](odata-v3/odata-actions.md).

**Firmas de método**

Estas son algunas reglas para las firmas de método:

- Si la ruta de acceso contiene una clave, la acción debe tener un parámetro denominado *clave*.
- Si la ruta de acceso contiene una clave en una propiedad de navegación, la acción debe tener un parámetro denominado *relatedKey*.
- Decorar *clave* y *relatedKey* parámetros con el **[FromODataUri]** parámetro.
- POST y las solicitudes PUT toman un parámetro de tipo de entidad.
- Las solicitudes PATCH toman un parámetro de tipo **Delta&lt;T&gt;**, donde *T* es el tipo de entidad.

Como referencia, este es un ejemplo que muestra las firmas de método para cada convención de enrutamiento de OData integrado.

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a>Convenciones de enrutamientos personalizadas

Actualmente las convenciones integradas no cubren a todas OData URIs posibles. Puede agregar nuevas convenciones implementando la **IODataRoutingConvention** interfaz. Esta interfaz tiene dos métodos:

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- **SelectController** devuelve el nombre del controlador.
- **SelectAction** devuelve el nombre de la acción.

Para ambos métodos, si la convención no es aplicable a esa solicitud, el método debe devolver null.

El **ODataPath** parámetro representa la ruta de acceso de recurso de OData analizado. Contiene una lista de **[ODataPathSegment](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatapathsegment.aspx)** instancias, uno para cada segmento de la ruta de acceso del recurso. **ODataPathSegment** es una clase abstracta; cada tipo de segmento se representa mediante una clase que derive de **ODataPathSegment**.

El **ODataPath.TemplatePath** propiedad es una cadena que representa la concatenación de todos los segmentos de ruta de acceso. Por ejemplo, si el URI es `/Products(1)/Supplier`, la plantilla de ruta de acceso es &quot;~/entityset/key/navigation&quot;. Tenga en cuenta que los segmentos no corresponden directamente a segmentos de URI. Por ejemplo, la clave de entidad (1) se representa como su propio **ODataPathSegment**.

Normalmente, una implementación de **IODataRoutingConvention** hace lo siguiente:

1. Compare la plantilla de ruta de acceso para ver si esta convención se aplica a la solicitud actual. Si no es aplicable, devuelve null.
2. Si se aplica la convención, use las propiedades de la **ODataPathSegment** instancias para derivar los nombres de acción y controlador.
3. Para las acciones, agregue los valores para el diccionario de ruta que se debe enlazar a los parámetros de acción (normalmente, las claves de entidad).

Veamos un ejemplo concreto. Las convenciones de enrutamiento integradas no admiten la indexación en una colección de navegación. En otras palabras, no hay ninguna convención de URI similar al siguiente:

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

Aquí es una convención de enrutamiento personalizada para controlar este tipo de consulta.

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

Notas:

1. Realizo la derivación desde **EntitySetRoutingConvention**, porque el **SelectController** método en esa clase es adecuado para esta nueva convención de enrutamiento. Esto significa que no es necesario volver a implementar **SelectController**.
2. La convención se aplica únicamente a las solicitudes GET, y solo cuando la plantilla de ruta de acceso es &quot;~/entityset/key/navigation/key&quot;.
3. Es el nombre de acción &quot;obtener {EntityType}&quot;, donde *{EntityType}* es el tipo de la colección de navegación. Por ejemplo, &quot;GetSupplier&quot;. Puede usar cualquier convención de nomenclatura que le guste &#8212; simplemente asegúrese de que sus acciones de controlador coinciden.
4. La acción toma dos parámetros denominados *clave* y *relatedKey*. (Para obtener una lista de algunos nombres de parámetro predefinidos, vea [ODataRouteConstants](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatarouteconstants.aspx).)

El siguiente paso es agregar la nueva convención a la lista de convenciones de enrutamiento. Esto sucede durante la configuración, tal como se muestra en el código siguiente:

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

Estos son algunos otros ejemplo convenciones de enrutamiento que ser útil estudiar:

- [CompositeKeyRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [CustomNavigationRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [NonBindableActionRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [ODataVersionRouteConstraint](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

Y por supuesto propia API Web está abierto, para que pueda ver el [código fuente](http://aspnetwebstack.codeplex.com/) para las convenciones de enrutamiento integradas. Se definen en el **System.Web.Http.OData.Routing.Conventions** espacio de nombres.
