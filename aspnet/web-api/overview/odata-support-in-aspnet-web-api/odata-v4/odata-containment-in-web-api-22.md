---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Contención en OData v4 usar Web API 2.2 | Microsoft Docs
author: rick-anderson
description: Tradicionalmente, una entidad solo se puede acceder si se encapsule dentro de un conjunto de entidades. Pero OData v4 proporciona dos opciones adicionales, Singleton y Con...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 33ff49f69d70dd3a8179445d2895c418d2185e49
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365612"
---
<a name="containment-in-odata-v4-using-web-api-22"></a>Usar Web API 2.2 la contención en OData v4
====================
por Tan Jinfu

> Tradicionalmente, una entidad solo se puede acceder si se encapsule dentro de un conjunto de entidades. Pero OData v4 proporciona dos opciones adicionales, Singleton y contención, ambos de los cuales admite WebAPI 2.2.


En este tema se muestra cómo definir una contención en un extremo de OData en WebApi 2.2. Para obtener más información acerca de la contención, vea [contención viene con OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx). Para crear un punto de conexión de OData V4 en Web API, consulte [crear una mediante ASP.NET Web API 2.2 de OData v4 extremo](create-an-odata-v4-endpoint.md).

En primer lugar, vamos a crear un modelo de dominio de la contención en el servicio de OData, con este modelo de datos:

![Modelo de datos](odata-containment-in-web-api-22/_static/image1.png)

Una cuenta contiene muchas PaymentInstruments (PI), pero no definimos un conjunto de entidades de una instrucción de procesamiento. En su lugar, solo se pueden acceder a la PI a través de una cuenta.

Puede descargar la solución emplea en este tema de [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).

## <a name="defining-the-data-model"></a>Definir el modelo de datos

1. Definir los tipos de CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    El `Contained` atributo se utiliza para las propiedades de navegación de contención.
2. Generar el modelo EDM a partir de los tipos CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    El `ODataConventionModelBuilder` controlará la creación del modelo EDM, si la `Contained` atributo se agrega a la propiedad de navegación correspondiente. Si la propiedad es un tipo de colección, un `GetCount(string NameContains)` también se creará la función.

    Los metadatos generados tendrá un aspecto similar al siguiente:

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    El `ContainsTarget` atributo indica que la propiedad de navegación es una contención.

## <a name="define-the-containing-entity-set-controller"></a>Definir el controlador de conjunto de entidades que contiene

Entidades independientes no tienen su propio controlador; la acción se define en el controlador de conjunto de entidades que contiene. En este ejemplo, hay un AccountsController, pero no PaymentInstrumentsController.

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

Si la ruta de acceso de OData es 4 o más segmentos, atributo de sólo funciona el enrutamiento, como `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` en el controlador anterior. En caso contrario, atributo y el enrutamiento convencional funciona: por ejemplo, `GetPayInPIs(int key)` coincide con `GET ~/Accounts(1)/PayinPIs`.

*Gracias a Leo Hu para el contenido original de este artículo.*
