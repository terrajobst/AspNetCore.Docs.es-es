---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Guía de seguridad para ASP.NET Web API 2 OData | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/06/2013
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 2a5b776a81cb3e3cf809dd3c4229448988086a32
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823992"
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a>Guía de seguridad para ASP.NET Web API 2 OData
====================
por [Mike Wasson](https://github.com/MikeWasson)

En este tema se describe algunos de los problemas de seguridad que se deben considerar al exponer un conjunto de datos a través de OData.

## <a name="edm-security"></a>Seguridad EDM

La semántica de consulta se basa en el entity data model (EDM), no los tipos del modelo subyacente. Puede excluir una propiedad de EDM y no será visible para la consulta. Por ejemplo, suponga que el modelo incluye un tipo de empleado con una propiedad de sueldo. Es posible que desee excluir de esta propiedad de EDM para ocultarlo de los clientes.

Hay dos maneras de excluye una propiedad de EDM. Puede establecer el **[IgnoreDataMember]** atributo en la propiedad de la clase del modelo:

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

También puede quitar mediante programación la propiedad de EDM:

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a>Seguridad de la consulta

Un cliente malintencionado o naive puede ser capaz de crear una consulta que tarda mucho en ejecutar. En el peor de los casos esto puede interrumpir el acceso a su servicio.

El **[Queryable]** atributo es un filtro de acción que analiza, valida y aplica la consulta. El filtro convierte las opciones de consulta en una expresión LINQ. Cuando se devuelve el controlador de OData un **IQueryable** tipo, el **IQueryable** proveedor LINQ convierte la expresión LINQ en una consulta. Por lo tanto, el rendimiento depende en el proveedor LINQ que se usa y también en las características específicas de su esquema de conjunto de datos o base de datos.

Para obtener más información sobre el uso de las opciones de consulta de OData en ASP.NET Web API, consulte [que admiten opciones de consulta de OData](supporting-odata-query-options.md).

Si sabe que todos los clientes son de confianza (por ejemplo, en un entorno empresarial), o si el conjunto de datos es pequeño, rendimiento de las consultas no puede ser un problema. De lo contrario, considere las siguientes recomendaciones.

- Probar el servicio con varias consultas y generar perfiles de la base de datos.
- Habilitar la paginación controlada por servidor evitar la devolución de un gran conjunto de datos en una consulta. Para obtener más información, consulte [Server-Driven paginación](supporting-odata-query-options.md#server-paging). 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- ¿Necesita con $filter y $orderby? Algunas aplicaciones podrían permitir que el cliente de paginación, usando $top y $skip, pero deshabilitar las otras opciones de consulta. 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- Considere la posibilidad de restringir $orderby a las propiedades de un índice agrupado. Ordenar datos de gran tamaño sin un índice agrupado es lento. 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- Número máximo de nodos: el **MaxNodeCount** propiedad **[Queryable]** establece los nodos de número máximos permitidos en el árbol de sintaxis $filter. El valor predeterminado es 100, pero es posible que desea establecer un valor inferior, porque un gran número de nodos puede ser lento para compilar. Esto es especialmente cierto si está utilizando LINQ to Objects (es decir, las consultas LINQ en una colección en memoria sin el uso de un proveedor LINQ intermedio). 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- Considere la posibilidad de deshabilitar las funciones any() y all(), como pueden ser lentas. 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- Si las propiedades de cadena contienen cadenas grandes & #8212for ejemplo, una descripción de producto o una entrada de blog & 8212consider # deshabilitar las funciones de cadena. 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- Considere la posibilidad de no permitir el filtrado en las propiedades de navegación. Filtrado en las propiedades de navegación puede dar lugar a una combinación, que podría ser lenta, dependiendo de su esquema de base de datos. El código siguiente muestra un validador de consulta que impide que el filtrado en las propiedades de navegación. Para obtener más información acerca de los validadores de consulta, vea [validación de la consulta](supporting-odata-query-options.md#query-validation). 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- Considere la posibilidad de restringir las consultas $filter escribiendo un validador que se personaliza para la base de datos. Por ejemplo, tenga en cuenta estas dos consultas: 

  - Todas las películas con actores cuyo apellido empieza por 'A'.
  - Todas las películas publicadas en 1994.

    A menos que se indizan películas por actores, la primera consulta puede requerir que el motor de base de datos para analizar toda la lista de películas. Mientras que la segunda consulta podría ser aceptable, películas; se supone que se indiza por año de lanzamiento.

    El código siguiente muestra un validador que permite el filtrado en las propiedades "ReleaseYear" y "Title", pero ninguna otra propiedad.

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- En general, considere la posibilidad de que las funciones de $filter que necesita. Si los clientes no necesitan la expresividad de $filter completa, puede limitar las funciones permitidas.
