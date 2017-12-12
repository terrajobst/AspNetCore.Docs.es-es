---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Herencia de tipo complejo de OData v4 con ASP.NET Web API | Documentos de Microsoft
author: microsoft
description: "Según la especificación de OData v4, un tipo complejo puede heredar de otro tipo complejo. (Un tipo complejo es un tipo estructurado sin una clave). API de Web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: be2dbfa82b99b6c48928e4e767716852c14a463b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>Herencia de tipo complejo de OData v4 con ASP.NET Web API
====================
por [Microsoft](https://github.com/microsoft)

> Función de OData v4 [especificación](http://www.odata.org/documentation/odata-version-4-0/), un tipo complejo puede heredar de otro tipo complejo. (Un *complejo* es un tipo estructurado sin una clave.) Web API OData 5.3 admite la herencia de tipo complejo.
> 
> Este tema muestra cómo crear un entity data model (EDM) con tipos complejos de herencia. Para el código fuente completo, vea [ejemplo de herencia de tipo complejo de OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - Web API OData 5.3
> - OData v4


## <a name="model-hierarchy"></a>Jerarquía del modelo

Para ilustrar la herencia de tipo complejo, vamos a usar la siguiente jerarquía de clases.

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape`es un tipo complejo abstracto. `Rectangle`, `Triangle`, y `Circle` se derivan de tipos complejos `Shape`, y `RoundRectangle` deriva de `Rectangle`. `Window`es un tipo de entidad y contiene un `Shape` instancia.

Estos son las clases CLR que definen estos tipos.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>Generar el modelo EDM

Para crear el EDM, puede usar **ODataConventionModelBuilder**, lo que deduce las relaciones de herencia entre los tipos CLR.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

También puede compilar EDM explícitamente, mediante **ODataModelBuilder**. Esto tiene más código, pero ofrece un mayor control sobre el modelo EDM.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

Estos dos ejemplos crea el mismo esquema EDM.

## <a name="metadata-document"></a>Documento de metadatos

Este es el documento de metadatos de OData, que muestra herencia de tipo complejo.

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

En el documento de metadatos, puede ver:

- El `Shape` tipo complejo es abstracta.
- El `Rectangle`, `Triangle`, y `Circle` tipo complejo tiene el tipo base `Shape`.
- El `RoundRectangle` tipo tiene el tipo base `Rectangle`.

## <a name="casting-complex-types"></a>Conversión de tipos complejos

Ahora se admite la conversión se realiza en tipos complejos. Por ejemplo, la siguiente consulta convierte un `Shape` a una `Rectangle`.

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

Esta es la carga de respuesta:

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
