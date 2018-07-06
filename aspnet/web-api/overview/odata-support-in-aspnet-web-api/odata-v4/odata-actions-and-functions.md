---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Acciones y funciones en OData v4 con ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: En OData, acciones y funciones son una manera de agregar comportamientos de lado servidor que no se definen fácilmente como operaciones CRUD en entidades. Este tutorial se muestra cómo...
ms.author: aspnetcontent
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: a34361a2b298a6cb1efaa386d0a60011f4a578fa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834979"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>Acciones y funciones en OData v4 con ASP.NET Web API 2.2
====================
por [Mike Wasson](https://github.com/MikeWasson)

> En OData, acciones y funciones son una manera de agregar comportamientos de lado servidor que no se definen fácilmente como operaciones CRUD en entidades. Este tutorial muestra cómo agregar funciones y acciones a un extremo OData v4, con Web API 2.2. El tutorial se basa en el tutorial [crear OData v4 extremo mediante ASP.NET Web API 2](create-an-odata-v4-endpoint.md)
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - Web API 2.2
> - OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Versiones de tutoriales
> 
> Para OData versión 3, vea [acciones de OData en ASP.NET Web API 2](../odata-v3/odata-actions.md).


La diferencia entre *acciones* y *funciones* es que las acciones pueden tener efectos secundarios y las funciones no lo hacen. Acciones y funciones pueden devolver datos. Algunos usos de las acciones incluyen:

- Transacciones complejas.
- Manipular varias entidades a la vez.
- Permitir actualizaciones únicamente a determinadas propiedades de una entidad.
- Envío de datos que no es una entidad.

Las funciones son útiles para devolver información que no corresponden directamente a una entidad o colección.

Una acción (o función) puede tener como destino una sola entidad o una colección. En la terminología de OData, se trata la *enlace*. También puede tener &quot;independiente&quot; las acciones o funciones, que se conocen como operaciones estáticas en el servicio.

## <a name="example-adding-an-action"></a>Ejemplo: Agregar una acción

Vamos a definir una acción para valorar un producto.

> [!NOTE]
> Este tutorial se basa en el tutorial [crear OData v4 extremo mediante ASP.NET Web API 2](create-an-odata-v4-endpoint.md)


En primer lugar, agregue un `ProductRating` modelo para representar las clasificaciones.

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

Agregue también un **DbSet** a la `ProductsContext` clase, por lo que EF creará una tabla de clasificación en la base de datos.

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>Agregar la acción al EDM

En WebApiConfig.cs, agregue el código siguiente:

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

El **EntityTypeConfiguration.Action** método agrega una acción para el entity data model (EDM). El **parámetro** método especifica un parámetro con tipo para la acción.

Este código también establece el espacio de nombres para el EDM. El espacio de nombres es importante porque el URI para la acción incluye el nombre de acción completa:

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> En una configuración típica de IIS, el punto en esta dirección URL hará que IIS devuelva el error 404. Para resolver este problema agregando la siguiente sección al archivo Web.Config:

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>Agregar un método de controlador para la acción

Para habilitar el &quot;tasa&quot; acción, agregue el siguiente método a `ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

Tenga en cuenta que el nombre del método coincide con el nombre de acción. El **[HttpPost]** atributo especifica el método es un método HTTP POST.

Para invocar la acción, el cliente envía una solicitud HTTP POST similar al siguiente:

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

El &quot;tasa&quot; acción está enlazada a las instancias de Product, por lo que el URI para la acción es el nombre de acción completa anexado a la URI de la entidad. (Recuerde que configuramos el espacio de nombres EDM &quot;ProductService&quot;, por lo que es el nombre de acción completa &quot;ProductService.Rate&quot;.)

El cuerpo de la solicitud contiene los parámetros de acción como una carga JSON. API Web convierte automáticamente la carga de JSON para un **ODataActionParameters** objeto, que es más que un diccionario de valores de parámetro. Utilice este diccionario para tener acceso a los parámetros del método de controlador.

Si el cliente envía los parámetros de acción en el problema de formato, el valor de **ModelState.IsValid** es false. Active esta marca en el método de controlador y devolverá un error si **IsValid** es false.

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>Ejemplo: Agregar una función

Ahora vamos a agregar una función de OData que devuelve el producto más costoso. Como antes, el primer paso es agregar la función en el EDM. En WebApiConfig.cs, agregue el código siguiente.

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

En este caso, la función está enlazada a la colección de productos, en lugar de las instancias individuales de producto. Los clientes de invocan la función mediante el envío de una solicitud GET:

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

Este es el método de controlador para esta función:

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

Tenga en cuenta que el nombre del método coincide con el nombre de función. El **[HttpGet]** atributo especifica el método es un método HTTP GET.

Esta es la respuesta HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>Ejemplo: Agregar una función sin enlazar

El ejemplo anterior era una función enlazada a una colección. En el ejemplo siguiente, crearemos un *independiente* función. Las funciones no enlazadas se denominan como operaciones estáticas en el servicio. La función en este ejemplo devolverá los impuestos por un código postal determinado.

En el archivo WebApiConfig, agregue la función en el EDM:

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

Tenga en cuenta que estamos llamando a **función** directamente en el **ODataModelBuilder**, en lugar del tipo de entidad o colección. Esto indica que el generador de modelos que la función es independiente.

Este es el método de controlador que implementa la función:

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

No importa qué controlador de API Web Coloque este método. Podría poner en `ProductsController`, o definir un controlador aparte. El **[ODataRoute]** atributo define la plantilla URI para la función.

Este es un ejemplo de solicitud de cliente:

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

La respuesta HTTP:

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
