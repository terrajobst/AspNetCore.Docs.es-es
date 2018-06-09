---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Acciones y funciones de OData v4 con ASP.NET Web API 2.2 | Documentos de Microsoft
author: MikeWasson
description: En OData, acciones y funciones son una forma de agregar los comportamientos de servidor que no se definen fácilmente como operaciones CRUD en entidades. Este tutorial se muestra cómo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 532362f0c0faaaf0cb0c04726856f0497e5261b5
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "26508234"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>Acciones y funciones de OData v4 con ASP.NET Web API 2.2
====================
por [Mike Wasson](https://github.com/MikeWasson)

> En OData, acciones y funciones son una forma de agregar los comportamientos de servidor que no se definen fácilmente como operaciones CRUD en entidades. Este tutorial muestra cómo agregar funciones y acciones para un extremo OData v4, mediante Web API 2.2. El tutorial se basa en el tutorial [crear OData v4 extremo mediante ASP.NET Web API 2](create-an-odata-v4-endpoint.md)
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - 2.2 API Web
> - OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Versiones de tutoriales
> 
> Para la versión 3 de OData, vea [las acciones de OData en ASP.NET Web API 2](../odata-v3/odata-actions.md).


La diferencia entre *acciones* y *funciones* es que las acciones pueden tener efectos secundarios y funciones no tienen que serlo. Acciones y funciones pueden devolver datos. Algunos usos de las acciones son:

- Transacciones complejas.
- La manipulación de varias entidades a la vez.
- Permitir actualizaciones solo a determinadas propiedades de una entidad.
- Envío de datos que no es una entidad.

Las funciones son útiles para devolver información que no se corresponde directamente a una entidad o colección.

Una acción (o función) puede tener como destino una sola entidad o una colección. En la terminología de OData, esto es el *enlace*. También puede hacer que &quot;sin enlazar&quot; las acciones o funciones, que se conocen como operaciones estáticas en el servicio.

## <a name="example-adding-an-action"></a>Ejemplo: Agregar una acción

Vamos a definir una acción para valorar un producto.

> [!NOTE]
> Este tutorial se basa en el tutorial [crear OData v4 extremo mediante ASP.NET Web API 2](create-an-odata-v4-endpoint.md)


En primer lugar, agregue un `ProductRating` modelo para representar las clasificaciones.

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

Agregar un **DbSet** a la `ProductsContext` de la clase, por lo que EF creará una tabla de clasificación en la base de datos.

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>Agregar la acción a EDM

En WebApiConfig.cs, agregue el código siguiente:

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

El **EntityTypeConfiguration.Action** método agrega una acción para el entity data model (EDM). El **parámetro** método especifica un parámetro con tipo para la acción.

Este código también establece el espacio de nombres para el modelo EDM. El espacio de nombres es importante porque el URI para la acción incluye el nombre de acción completo:

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> En una configuración típica de IIS, el punto en esta dirección URL hará que IIS devuelva el error 404. Para resolver este problema agregando la siguiente sección al archivo Web.Config:

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>Agregue un método de controlador para la acción

Para habilitar la &quot;velocidad&quot; acción, agregue el método siguiente a `ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

Tenga en cuenta que el nombre del método coincide con el nombre de acción. El **[HttpPost]** atributo especifica el método es un método POST de HTTP.

Para invocar la acción, el cliente envía una solicitud HTTP POST similar al siguiente:

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

El &quot;velocidad&quot; acción está enlazada a instancias de producto, por lo que el URI para la acción es el nombre de acción completo que se anexa a la entidad de URI. (Recuerde que se debe establecer el espacio de nombres EDM &quot;ProductService&quot;, por lo que es el nombre de acción completo &quot;ProductService.Rate&quot;.)

El cuerpo de la solicitud contiene los parámetros de acción como una carga JSON. API Web convierte automáticamente la carga de JSON para un **ODataActionParameters** objeto, que es simplemente un diccionario de valores de parámetro. Utilice este diccionario para tener acceso a los parámetros del método de controlador.

Si el cliente envía los parámetros de acción en el error de formato, el valor de **ModelState.IsValid** es false. Comprobar esta marca en su método de controlador y devolver un error si **IsValid** es false.

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>Ejemplo: Agregar una función

Ahora vamos a agregar una función de OData que devuelve el producto más caro. Como antes, el primer paso consiste en agregar la función en el EDM. En WebApiConfig.cs, agregue el código siguiente.

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

En este caso, la función está enlazada a la colección de productos, en lugar de las instancias individuales de producto. Los clientes invocación la función mediante el envío de una solicitud GET:

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

Éste es el método de controlador para esta función:

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

Tenga en cuenta que el nombre del método coincide con el nombre de función. El **[HttpGet]** atributo especifica el método es un método GET de HTTP.

Aquí está la respuesta HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>Ejemplo: Agregar una función sin enlazar

El ejemplo anterior era una función enlazada a una colección. En el ejemplo siguiente, crearemos un *sin enlazar* función. Funciones sin enlazar se conocen como operaciones estáticas en el servicio. La función en este ejemplo devuelve el impuesto para un código postal determinado.

En el archivo WebApiConfig, agregue la función a EDM:

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

Tenga en cuenta que estamos llamando a **función** directamente en el **ODataModelBuilder**, en lugar del tipo de entidad o colección. Esto indica que el generador de modelos que la función es independiente.

Éste es el método de controlador que implementa la función:

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

No importa qué controlador de Web API colocar este método en. Se podría poner `ProductsController`, o definir un controlador independiente. El **[ODataRoute]** atributo define la plantilla URI para la función.

Esta es una solicitud de cliente de ejemplo:

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

La respuesta HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
