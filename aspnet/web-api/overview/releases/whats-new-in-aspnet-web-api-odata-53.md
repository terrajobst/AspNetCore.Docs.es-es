---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: Novedades de ASP.NET Web API OData 5.3 | Documentos de Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: e918f86ebd813f31ad0c21566e617482fc7dd963
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508114"
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a>Novedades de ASP.NET Web API OData 5.3
====================
por [Microsoft](https://github.com/microsoft)

Este tema describe lo que es nuevo en ASP.NET Web API OData 5.3.

- [Descarga](#download)
- [Documentación](#documentation)
- [Bibliotecas principales de OData](#corelib)
- [Nuevas características](#newf)
- [Problemas conocidos y los cambios recientes](#known-issues)
- [Correcciones de errores](#bug-fixes)
- [ASP.NET Web API OData 5.3.1](#OD)

<a id="download"></a>
## <a name="download"></a>Descargar

Las características en tiempo de ejecución se publican como paquetes de NuGet en la Galería de NuGet. Puede instalar o actualizar a los paquetes de NuGet publicados mediante la consola de administrador de paquetes de NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Documentación

Encontrará tutoriales y otra documentación sobre ASP.NET Web API OData en la [sitio web de ASP.NET](../odata-support-in-aspnet-web-api/index.md).

<a id="corelib"></a>
## <a name="odata-core-libraries"></a>Bibliotecas principales de OData

Para OData v4, Web API ahora usa ODataLib versión 6.5.0

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a>Nuevas características de ASP.NET Web API OData 5.3

### <a name="support-for-levels-in-expand"></a>Expanda la compatibilidad con $levels en $

Puede usar el $levels consulta opción en $ampliar las consultas. Por ejemplo:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

Esta consulta equivale a:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a>Compatibilidad con tipos de entidad abierto

Un *abrir tipo* es un tipo de stuctured que contiene las propiedades dinámicas, además de todas las propiedades que se declaran en la definición de tipo. Tipos abiertos, podrá agregar flexibilidad a los modelos de datos. Para obtener más información, vea xxxx.

### <a name="support-for-dynamic-collection-properties-in-open-types"></a>Compatibilidad con las propiedades de colección dinámica de tipos abiertos

Una propiedad dinámica que tenía anteriormente, debe ser un valor único. En 5.3, las propiedades dinámicas pueden tener valores de la colección. Por ejemplo, en la siguiente carga de JSON, el `Emails` propiedad es una propiedad dinámica y de la colección de tipo de cadena:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a>Compatibilidad con la herencia para los tipos complejos

Ahora los tipos complejos pueden heredar de un tipo base. Por ejemplo, un servicio OData podría definir los siguientes tipos complejos:

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

Este es el modelo EDM para este ejemplo:

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

Para obtener más información, consulte [ejemplo de herencia de tipo complejo de OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conocidos y los cambios recientes

Esta sección describen problemas conocidos y cambios importantes en 5.3 de OData de ASP.NET Web API.

### <a name="odata-v4"></a>OData v4

#### <a name="query-options"></a>Opciones de consulta

Problema: $ Anidada expanda con $levels = max da como resultado una profundidad de expansión incorrecto.

Por ejemplo, dada la siguiente solicitud:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

Si `MaxExpansionDepth` es 5, esta consulta, se crearán una profundidad de expansión de 6.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Correcciones de errores y actualizaciones de características secundarias

Esta versión también incluye varias correcciones de errores y una característica secundaria actualizaciones. Puede encontrar la lista completa aquí:

- [Correcciones de errores](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a>ASP.NET Web API OData 5.3.1

En esta versión hemos realizado una [corrección](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) a algunas de las enumeraciones de AllowedFunctions. Esta versión no tiene ningún otro correcciones de errores o característica nueva.
