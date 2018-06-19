---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Contención de OData v4 usar Web API 2.2 | Documentos de Microsoft
author: rick-anderson
description: Tradicionalmente, una entidad puede obtenerse solo si se encapsula dentro de un conjunto de entidades. Pero con OData v4 proporciona dos opciones adicionales, Singleton y Con...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 7d3c81bf3d2a43faa3e71155637e031f81143782
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508004"
---
<a name="containment-in-odata-v4-using-web-api-22"></a>Contención de OData v4 usar Web API 2.2
====================
por Tan Jinfu

> Tradicionalmente, una entidad puede obtenerse solo si se encapsula dentro de un conjunto de entidades. Pero con OData v4 proporciona dos opciones adicionales, Singleton y contención, ambos de los cuales admite WebAPI 2.2.


Este tema muestra cómo definir la contención en un extremo de OData en WebApi 2.2. Para obtener más información acerca de la contención, vea [contención proviene con OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx). Para crear un extremo de OData V4 en Web API, consulte [crear OData v4 extremo mediante ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).

En primer lugar, vamos a crear un modelo de dominio de contención en el servicio de OData, con este modelo de datos:

![Modelo de datos](odata-containment-in-web-api-22/_static/image1.png)

Una cuenta contiene muchas PaymentInstruments (PI), pero no definimos un conjunto de entidades de una instrucción de procesamiento. En su lugar, el PI solo puede tener acceso a través de una cuenta.

Puede descargar la solución empleada en este tema de [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).

## <a name="defining-the-data-model"></a>Definir el modelo de datos

1. Definir los tipos CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    El `Contained` atributo se usa para las propiedades de navegación de contención.
2. Generar el modelo EDM a partir de los tipos CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    El `ODataConventionModelBuilder` controlará la generación del modelo EDM si el `Contained` atributo se agrega a la propiedad de navegación correspondiente. Si la propiedad es un tipo de colección, un `GetCount(string NameContains)` también se creará la función.

    Los metadatos generados tendrá un aspecto similar al siguiente:

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    El `ContainsTarget` atributo indica que la propiedad de navegación es la contención.

## <a name="define-the-containing-entity-set-controller"></a>Definir el controlador de conjunto de entidades que contiene

Entidades independientes no tienen su propio controlador; la acción se define en el controlador de conjunto de entidades que lo contiene. En este ejemplo, hay un AccountsController, pero no PaymentInstrumentsController.

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

Si la ruta de acceso de OData es 4 o más segmentos, atributo de sólo funciona el enrutamiento, como `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` en el controlador anterior. En caso contrario, los atributos y el enrutamiento convencional funciona: por ejemplo, `GetPayInPIs(int key)` coincide con `GET ~/Accounts(1)/PayinPIs`.

*Gracias a Leo Hu para el contenido original de este artículo.*
