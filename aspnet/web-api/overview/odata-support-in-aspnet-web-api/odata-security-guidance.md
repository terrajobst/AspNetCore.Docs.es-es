---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Directrices de seguridad para ASP.NET Web API 2 OData | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/06/2013
ms.topic: article
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 41b05f2a2f8247853d8358e6cc1246c8b438a6db
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a>Directrices de seguridad para ASP.NET Web API 2 OData
====================
por [Mike Wasson](https://github.com/MikeWasson)

En este tema se describe algunos de los problemas de seguridad que se deben considerar al exponer un conjunto de datos a través de OData.

## <a name="edm-security"></a>Seguridad EDM

La semántica de consulta se basa en entity data model (EDM), no los tipos del modelo subyacente. Puede excluir una propiedad de EDM y no serán visible para la consulta. Por ejemplo, suponga que el modelo incluye un tipo de empleado con una propiedad de sueldo. Puede excluir esta propiedad del EDM para ocultarlo de los clientes.

Hay dos maneras de excluye una propiedad de EDM. Puede establecer la **[IgnoreDataMember]** atributo en la propiedad de la clase del modelo:

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

También puede quitar mediante programación la propiedad de EDM:

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a>Seguridad de la consulta

Un cliente malintencionado o naïve posible que pueda crear una consulta que tarda mucho en ejecutar. En el peor de los casos esto puede interrumpir el acceso a su servicio.

El **[Queryable]** atributo es un filtro de acción que analiza, valida y aplica la consulta. El filtro convierte las opciones de consulta en una expresión LINQ. Cuando el controlador de OData devuelve un **IQueryable** tipo, el **IQueryable** proveedor LINQ convierte la expresión LINQ en una consulta. Por lo tanto, el rendimiento depende en el proveedor LINQ que se utiliza y también en las características específicas de su esquema de conjunto de datos o base de datos.

Para obtener más información sobre el uso de las opciones de consulta de OData en ASP.NET Web API, consulte [admiten opciones de consulta de OData](supporting-odata-query-options.md).

Si sabe que todos los clientes son de confianza (por ejemplo, en un entorno empresarial), o si el conjunto de datos es pequeño, el rendimiento de las consultas no sea un problema. En caso contrario, debe tener en cuenta las siguientes recomendaciones.

- Probar el servicio con varias consultas y generar perfiles de la base de datos.
- Habilitar la paginación controlada por el servidor evitar devolver un conjunto de datos grande en una consulta. Para obtener más información, consulte [Server-Driven paginación](supporting-odata-query-options.md#server-paging). 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- ¿Necesita $filter y $orderby? Algunas aplicaciones podrían permitir el cliente de paginación, el uso de $top y $skip y deshabilitar las otras opciones de consulta. 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- Considere la posibilidad de restringir $orderby a las propiedades de un índice agrupado. Ordenar datos de gran tamaño sin un índice agrupado es lenta. 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- El número de nodos máximo: el **MaxNodeCount** propiedad **[Queryable]** establece los nodos de número máximos permitidos en el árbol de sintaxis $filter. El valor predeterminado es 100, pero puede que desee establecer un valor inferior, porque puede ser lento compilar un gran número de nodos. Esto es especialmente cierto si está utilizando LINQ to Objects (es decir, las consultas LINQ en una colección en memoria, sin el uso de un proveedor LINQ intermedio). 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- Considere la posibilidad de deshabilitar las funciones any() y all(), como pueden ser lenta. 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- Si las propiedades de cadena contienen cadenas largas & #8212for ejemplo, una descripción del producto o una entrada de blog y 8212consider # deshabilitar las funciones de cadena. 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- Considere la posibilidad de impedir el filtrado en las propiedades de navegación. Filtrar según propiedades de navegación puede dar lugar a una combinación, que puede ser lenta, dependiendo de su esquema de base de datos. El código siguiente muestra un validador de consulta que impide que el filtrado en las propiedades de navegación. Para obtener más información acerca de los validadores de consulta, vea [consulta validación](supporting-odata-query-options.md#query-validation). 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- Considere la posibilidad de restringir las consultas $filter escribiendo un validador que se personaliza para la base de datos. Por ejemplo, tenga en cuenta estas dos consultas: 

  - Todas las películas con actores cuyo nombre empieza con 'A'.
  - Todas las películas publicadas en 1994.

    A menos que se indizan películas por agentes, la primera consulta puede requerir el motor de base de datos para examinar toda la lista de películas. Mientras que la segunda consulta podría ser aceptable, películas suponiendo que se indizan por año de lanzamiento.

    El código siguiente muestra un validador que permite filtrar en las propiedades "ReleaseYear" y "Title", pero ninguna otra propiedad.

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- En general, tenga en cuenta qué funciones $filter que necesita. Si los clientes no necesitan la expresividad completa de $filter, puede limitar las funciones permitidas.
