---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Herencia de tipo complejo en OData v4 con ASP.NET Web API | Microsoft Docs
author: microsoft
description: Según la especificación de OData v4, un tipo complejo puede heredar de otro tipo complejo. (Un tipo complejo es un tipo estructurado sin una clave). API de Web...
ms.author: riande
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 8dbf7dc4cfc70e1ea1ed6f72ffc0751a56809c09
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835736"
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>Herencia de tipo complejo en OData v4 con ASP.NET Web API
====================
por [Microsoft](https://github.com/microsoft)

> Según el OData v4 [especificación](http://www.odata.org/documentation/odata-version-4-0/), un tipo complejo puede heredar de otro tipo complejo. (Un *complejos* tipo es un tipo estructurado sin una clave.) Web API OData 5.3 admite la herencia de tipo complejo.
> 
> En este tema se muestra cómo crear un entity data model (EDM) con tipos de herencia compleja. Para el código fuente completo, vea [ejemplos de herencia de tipos complejos de OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - Web API OData 5.3
> - OData v4


## <a name="model-hierarchy"></a>Jerarquía del modelo

Para ilustrar la herencia de tipos complejos, vamos a usar la siguiente jerarquía de clases.

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape` es un tipo complejo abstracto. `Rectangle`, `Triangle`, y `Circle` se derivan de tipos complejos `Shape`, y `RoundRectangle` deriva `Rectangle`. `Window` es un tipo de entidad y contiene un `Shape` instancia.

Estas son las clases CLR que definen estos tipos.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>Generar el modelo EDM

Para crear el EDM, puede usar **ODataConventionModelBuilder**, que infiere las relaciones de herencia entre los tipos CLR.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

También puede generar el EDM explícitamente, mediante **ODataModelBuilder**. Esto toma más código, pero le ofrece más control sobre el modelo EDM.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

Estos dos ejemplos crea el mismo esquema EDM.

## <a name="metadata-document"></a>Documento de metadatos

Este es el documento de metadatos de OData, que muestra herencia de tipo complejo.

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

En el documento de metadatos, puede ver:

- El `Shape` tipo complejo es abstracto.
- El `Rectangle`, `Triangle`, y `Circle` tipo complejo tiene el tipo base `Shape`.
- El `RoundRectangle` tipo tiene el tipo base `Rectangle`.

## <a name="casting-complex-types"></a>Conversión de tipos complejos

Ahora se admite la conversión de tipos complejos. Por ejemplo, la siguiente consulta convierte un `Shape` a un `Rectangle`.

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

Esta es la carga de respuesta:

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
