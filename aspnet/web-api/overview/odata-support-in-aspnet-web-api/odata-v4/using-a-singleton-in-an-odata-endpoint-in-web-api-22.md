---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Crear un Singleton en OData v4 usar Web API 2.2 | Microsoft Docs
author: rick-anderson
description: En este tema se muestra cómo definir un singleton en un extremo de OData en Web API 2.2.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 3ae9b23fae356e387a011a190119d760dc46d022
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390932"
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a>Crear un Singleton en OData v4 usar Web API 2.2
====================
por Zoe Luo

> Tradicionalmente, una entidad solo se puede acceder si se encapsule dentro de un conjunto de entidades. Pero OData v4 proporciona dos opciones adicionales, Singleton y contención, ambos de los cuales admite WebAPI 2.2.


En este artículo se muestra cómo definir un singleton en un extremo de OData en Web API 2.2. Para obtener información sobre un singleton de qué es y cómo puede beneficiarse de usarlo, consulte [mediante un singleton para definir la entidad especial](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx). Para crear un punto de conexión de OData V4 en Web API, consulte [crear una mediante ASP.NET Web API 2.2 de OData v4 extremo](create-an-odata-v4-endpoint.md). 

Vamos a crear un singleton en el proyecto de API Web mediante el modelo de datos siguientes:

![Modelo de datos](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

Un valor singleton denominado `Umbrella` se definirán según tipo `Company`y un conjunto con nombre de entidades `Employees` se definirán según tipo `Employee`.

La solución que se usa en este tutorial se puede descargar desde [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).

## <a name="define-the-data-model"></a>Definir el modelo de datos

1. Definir los tipos de CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. Generar el modelo EDM a partir de los tipos CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    En este caso, `builder.Singleton<Company>("Umbrella")` indica el generador de modelos para crear un valor singleton denominado `Umbrella` en el modelo EDM.

    Los metadatos generados tendrá un aspecto similar al siguiente:

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    Desde los metadatos, vemos que la propiedad de navegación `Company` en el `Employees` está enlazado el conjunto de entidades para el singleton `Umbrella`. El enlace se realiza automáticamente `ODataConventionModelBuilder`, puesto que sólo `Umbrella` tiene la `Company` tipo. Si hay alguna ambigüedad en el modelo, puede usar `HasSingletonBinding` para enlazar explícitamente una propiedad de navegación en un singleton; `HasSingletonBinding` tiene el mismo efecto que usar el `Singleton` atributo en la definición de tipo CLR:

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a>Definir el controlador de singleton

Al igual que el controlador de EntitySet, el controlador de singleton se hereda de `ODataController`, y debe ser el nombre del controlador singleton `[singletonName]Controller`.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

Con el fin de controlar los distintos tipos de solicitudes, acciones deben estar previamente definido en el controlador. **Enrutamiento mediante atributos** está habilitada de forma predeterminada en WebApi 2.2. Por ejemplo, para definir una acción para controlar consultar `Revenue` desde `Company` mediante el enrutamiento mediante atributos, use lo siguiente:

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

Si no estás dispuesto a definir los atributos de cada acción, simplemente definen las acciones siguientes [convenciones de enrutamiento de OData](../odata-routing-conventions.md). Dado que una clave no es necesaria para realizar consultas en un singleton, las acciones definidas en el controlador de singleton son ligeramente diferentes de las acciones definidos en el controlador de entityset.

Como referencia, las firmas de método para cada definición de la acción en el controlador de singleton se enumeran a continuación.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

Básicamente, esto es todo lo que necesita hacer en el lado del servicio. El [proyecto de ejemplo](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contiene todo el código para la solución y el cliente de OData que se muestra cómo usar el singleton. El cliente se crea siguiendo los pasos descritos en [crear una aplicación de cliente de OData v4](create-an-odata-v4-client-app.md).

. 

*Gracias a Leo Hu para el contenido original de este artículo.*
