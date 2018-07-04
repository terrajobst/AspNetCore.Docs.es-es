---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: Novedades de ASP.NET Web API 2.2 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 51b01c4b9ee8d12e4e3925193e308d3ca6c5f2b6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377446"
---
<a name="whats-new-in-aspnet-web-api-22"></a>Novedades de ASP.NET Web API 2.2
====================
por [Microsoft](https://github.com/microsoft)

Este tema describe cuáles son las novedades para ASP.NET Web API 2.2.

- [Descarga](#download)
- [Documentación](#documentation)
- [Nuevas características en ASP.NET Web API 2.2](#newf)

    - [OData v4](#OData)
    - [Mejoras de enrutamiento de atributo](#ARI)
    - [Soporte de cliente de API Web para Windows Phone 8.1](#phone)
- [Problemas conocidos y cambios importantes](#known-issues)
- [Correcciones de errores](#bug-fixes)
- [Microsoft.AspNet.OData 5.2.1](#odata521)
- [Microsoft.AspNet.WebAPI 5.2.2](#522RC)
- [Microsoft.AspNet.WebAPI 5.2.3 Beta](#523)

<a id="download"></a>
## <a name="download"></a>Descargar

Las características en tiempo de ejecución se publican como paquetes de NuGet en la Galería de NuGet. Siguen todos los paquetes en tiempo de ejecución el [Versionamiento semántico](http://semver.org/) especificación. El paquete más reciente de ASP.NET Web API 2.2 tiene la siguiente versión: "5.2.0". Puede instalar o actualizar estos paquetes a través de [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). La versión también incluye los correspondientes paquetes localizados en NuGet.

Puede instalar o actualizar a los paquetes de NuGet publicados mediante la consola de administrador de paquetes de NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Documentación

Tutoriales y otra información sobre ASP.NET Web API 2.2 están disponibles desde el sitio web ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a>Nuevas características en ASP.NET Web API 2.2

<a id="OData"></a>
### <a name="odata-v4"></a>OData v4

Esta versión agrega compatibilidad con el Protocolo OData v4. Para obtener más información, consulte el [documentación de Web API OData v4.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)

Estas son algunas de las características clave y los cambios de OData v4:

- [Compatibilidad con las propiedades de alias en el modelo de OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [Compatibilidad con ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute y ConcurrencyCheckAttribute en ODataConventionModelBuilder](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [Proporcionar la capacidad de proporcionar un título descriptivo para las acciones](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- Integrar con ODL UriParser
- Compatibilidad con [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [contención](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) y [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)
- Compatibilidad con conversión de tipos primitivos
- [Se ha agregado compatibilidad de la función de OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Alias de parámetro de soporte técnico para las llamadas de función](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Admite la convención de nomenclatura de mayúsculas y minúsculas en el modelo](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- Compatibilidad con cast() en $filter
- Soporte técnico para el tipo complejo abierto
- Quitado EntitySetController y AsyncEntitySetController
- [$Link modificadas a $ref](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [Se ha agregado compatibilidad con enrutamiento del atributo](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- Usa las bibliotecas de OData Core 6.4.0

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a>Mejoras de enrutamiento de atributo

Enrutamiento mediante atributos ahora proporciona un punto de extensibilidad llamado IDirectRouteProvider, lo que permite un control total sobre cómo se detectan y configurar las rutas de atributo. Un IDirectRouteProvider es responsable de proporcionar una lista de acciones y los controladores, junto con información de ruta asociada para especificar exactamente qué configuración de enrutamiento se desea para esas acciones. Una implementación IDirectRouteProvider puede especificarse cuando se llama a MapAttributes/MapHttpAttributeRoutes.

Personalizar IDirectRouteProvider será más fácil extendiendo nuestra implementación predeterminada, DefaultDirectRouteProvider. Esta clase proporciona métodos virtuales que se puede invalidar independientes para cambiar la lógica para detectar atributos, crear entradas de ruta y detección de prefijo de ruta y el prefijo de área.

Estos son algunos ejemplos en lo que podría hacer con este nuevo punto de extensibilidad:

1. Admite la herencia de atributos de ruta

    Ejemplo:

    Aquí una solicitud like "/ api/10/valores" devolvería correctamente "Éxito: 10"

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. Proporcione un nombre de ruta predeterminada para las rutas de atributo siguiendo alguna convención que desee. De forma predeterminada, el enrutamiento mediante atributos no crea automáticamente los nombres de las rutas de atributo.
3. Modificar plantilla de ruta de las rutas de atributo en un lugar central antes de acabar en la tabla de rutas.

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a>Compatibilidad de cliente de la API de Web para Windows Phone 8.1

Ahora puede usar el paquete NuGet de Web API cliente para implementar la lógica de cliente de API Web cuando el destino es Windows Phone 8.1 o desde dentro de una aplicación Universal.

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conocidos y cambios importantes

Esta sección describen problemas conocidos y cambios importantes en la ASP.NET Web API 2.2.

### <a name="odata-v4"></a>OData v4

#### <a name="model-builder"></a>Generador de modelos

Problema: No se pudieran exponer funciones sobrecargadas como FunctionImport

Si hay 2 funciones sobrecargadas y también son FunctionImport tal como se muestra a continuación, a continuación, solicitar resultados ~/GetAllConventionCustomers(CustomerName={customerName}) en System.InvalidOperationException.

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

Solución alternativa: La solución alternativa para este problema consiste en agregar ambos las sobrecargas de función como FunctionImports.

#### <a name="odata-routing"></a>Enrutamiento de OData

Literales de cadena que incluyen la dirección URL codifican de barra diagonal (% 2F) y backslash(%5C) generar un error 404 cuando se usan en las rutas de acceso del recurso de OData.

Por ejemplo, los literales de cadena se pueden usar en las rutas de acceso del recurso de OData como parámetros de funciones o valores de clave de conjuntos de entidades.

/Employees/total.GetCount(Name='Name%2F')

/Employees('Name%5C')

Cuando dichas solicitudes el escape de Naciones Unidas de voluntad de hosts de recepción de servicios las secuencias de escape de antes de pasarlas al tiempo de ejecución de API Web. Esto protege contra ataques similar al siguiente:  
  
`http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:`

Esto hace que la pila de OData de Web API devolver un error 404 (no encontrado). Para evitar este error, el cliente debería utilizar las secuencias de escape doble barra diagonal (% 252F) y barra diagonal inversa (% C de 255). Esto no ocurre para las cadenas de consulta como /Employees? $filter = nombre eq 'Nombre % 2F'

**Tenga en cuenta las barras diagonales sin escape ('/') y barras diagonales inversas (") no son válidas en literales de cadena de ruta de acceso de recurso de OData. Las barras diagonales deben aparecer sólo como separadores de ruta de acceso y barras diagonales inversas no deben aparecer en la ruta de acceso del recurso de OData en absoluto. (Ambos son utilizables en algunas partes de una cadena de consulta de OData).**

Solución alternativa: Podría invalidar el método Parse de DefaultODataPathHandler a la barra diagonal y una barra diagonal inversa en literales de cadena de escape antes de analizarlos realmente. Puede encontrar un ejemplo de este enfoque aquí.

### <a name="odata-v3"></a>OData v3

#### <a name="queryable"></a>[Queryable]

El atributo [Queryable] está desusado. Deben usar nuevas aplicaciones de OData v3 **System.Web.Http.OData.EnableQueryAttribute**.

El **ODataHttpConfigurationExtensions.EnableQuerySupport** método de extensión se agrega ahora un **EnableQueryAttribute** a la colección de filtros globales. Si tiene alguno de los controladores del **[Queryable]** de atributo, una llamada a `config.EnableQuerySupport()` hará que el **[Queryable]** atributo a un error

La manera recomendada para resolver este problema es reemplazar todas las instancias de **QueryableAttribute** con **System.Web.Http.OData.EnableQueryAttribute**.

Una solución alternativa es usar el código siguiente en la configuración de la API Web:

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a>Enrutamiento mediante atributos

Problema: Enlace de modelos del tipo complejo que se decora con el atributo FromUri tiene un comportamiento diferente cuando se usa el enrutamiento mediante atributos.

Vínculo siguiente está realizando un seguimiento del problema y también incluye detalles sobre una solución alternativa.  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

Problema: Scaffolding MVC o API Web en un proyecto sin 5.2.0 los resultados de los paquetes en 5.1.2 paquetes para las que aún no existen en el proyecto

Actualizar paquetes de NuGet para ASP.NET MVC 5.2 no actualiza las herramientas de Visual Studio como el scaffolding de ASP.NET o la plantilla de proyecto de aplicación Web ASP.NET. Usan la versión anterior de los paquetes en tiempo de ejecución ASP.NET (por ejemplo, 5.1.2 en Update 2). Como resultado, el scaffolding de ASP.NET se instalará la versión anterior (por ejemplo, 5.1.2 en Update 2) de los paquetes necesarios, si aún no están disponibles en los proyectos. Sin embargo, el scaffolding de ASP.NET en Visual Studio 2013 RTM o Update 1 no sobrescribe los paquetes más recientes en sus proyectos. Si usa scaffolding de ASP.NET después de actualizar los paquetes de los proyectos de Web API 2.2 o ASP.NET MVC 5.2, asegúrese de que las versiones de API Web y ASP.NET MVC son coherentes.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Correcciones de errores y las actualizaciones de características secundarias

Esta versión también incluye varias correcciones de errores y una característica secundaria actualizaciones. Puede encontrar una lista completa aquí:

- [paquete 5.2](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a>Microsoft.AspNet.OData 5.2.1

El paquete Microsoft.AspNet.OData 5.2.1 contiene actualizaciones de dependencias de NuGet, pero no hay correcciones de errores. Con esta actualización, ya no hay una dependencia estricta en Microsoft.OData.Core 6.4.0, pero uno puede actualizar a cualquier versión entre 6.4.0 y 7.0.0.

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a>Microsoft.AspNet.WebAPI 5.2.2

En esta versión hemos creado una dependencia de cambio para `Json.Net 6.0.4`. Para obtener más información sobre lo que es nuevo en esta versión de `Json.NET`, consulte [Json.NET 6.0 Release 4 - JSON Merge, inserción de dependencias](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection). Esta versión no tiene otras características nuevas ni correcciones de errores en la API Web. Hemos actualizado posteriormente todos los demás paquetes dependientes que poseemos para depender de esta nueva versión de API Web.

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a>Microsoft.AspNet.WebAPI 5.2.3 Beta

Puede leer sobre la versión [aquí](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Esta versión contiene correcciones de errores solo. Puede usar [esta consulta](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) para ver la lista de los problemas corregidos en esta versión.
