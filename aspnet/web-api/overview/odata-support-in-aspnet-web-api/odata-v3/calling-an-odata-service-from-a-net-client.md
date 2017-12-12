---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Llamar a un servicio de OData desde un cliente .NET (C#) | Documentos de Microsoft
author: MikeWasson
description: "Este tutorial muestra cómo llamar a un servicio de OData desde una aplicación de cliente de C#. Versiones de software que se usa en el tutorial Visual Studio 2013 (funciona con Visual S..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: f6266045ebf55fb7ae691bfb55e9c90cd4edcc96
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="calling-an-odata-service-from-a-net-client-c"></a>Llamar a un servicio de OData desde un cliente .NET (C#)
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Este tutorial muestra cómo llamar a un servicio de OData desde una aplicación de cliente de C#.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (funciona con Visual Studio 2012)
> - [Biblioteca cliente de Servicios de datos de WCF](https://msdn.microsoft.com/en-us/library/cc668772.aspx)
> - Web API 2. (El servicio de OData de ejemplo se compila mediante la API Web 2, pero la aplicación cliente no depende de la API Web).


En este tutorial, le guían por la creación de una aplicación cliente que llama a un servicio de OData. El servicio de OData expone las siguientes entidades:

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

Los artículos siguientes describen cómo implementar el servicio de OData en Web API. (No necesita leyó para entender este tutorial, sin embargo).

- [Creación de un extremo de OData en Web API 2](creating-an-odata-endpoint.md)
- [Relaciones de entidad de OData en Web API 2](working-with-entity-relations.md)
- [Acciones de OData de Web API 2](odata-actions.md)

## <a name="generate-the-service-proxy"></a>Generar al Proxy de servicio

El primer paso consiste en generar a un proxy de servicio. El proxy de servicio es una clase .NET que define los métodos para tener acceso al servicio de OData. El proxy convierte las llamadas de método en las solicitudes HTTP.

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

Primer lugar, abra el proyecto de servicio de OData en Visual Studio. Presione CTRL + F5 para ejecutar el servicio localmente en IIS Express. Tenga en cuenta la dirección local, incluido el número de puerto que Visual Studio asigna. Necesitará esta dirección cuando se crea el proxy.

A continuación, abra otra instancia de Visual Studio y cree un proyecto de aplicación de consola. La aplicación de consola será nuestra aplicación de cliente de OData. (También puede agregar el proyecto a la misma solución que el servicio.)

> [!NOTE]
> El proyecto de consola, consulte los pasos restantes.


En el Explorador de soluciones, haga clic en **referencias** y seleccione **Agregar referencia de servicio**.

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

En el **Agregar referencia de servicio** cuadro de diálogo, escriba la dirección del servicio OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

donde *puerto* es el número de puerto.

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

Para **Namespace**, escriba "ProductService". Esta opción define el espacio de nombres de la clase de proxy.

Haga clic en **Ir**. Visual Studio lee el documento de metadatos de OData para detectar las entidades en el servicio.

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

Haga clic en **Aceptar** para agregar la clase de proxy para el proyecto.

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a>Cree una instancia de la clase de Proxy de servicio

Dentro de su `Main` (método), crear una nueva instancia de la clase de proxy, como se indica a continuación:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

De nuevo, use el número de puerto real donde se está ejecutando el servicio. Al implementar su servicio, usará el URI del servicio en vivo. No es necesario actualizar al servidor proxy.

El código siguiente agrega un controlador de eventos que imprime los URI de solicitud a la ventana de consola. Este paso no es necesario, pero es interesante ver a los URI para cada consulta.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a>Consultar al servicio

El código siguiente obtiene la lista de productos del servicio OData.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

Tenga en cuenta que no es necesario escribir ningún código para enviar la solicitud HTTP o analizar la respuesta. La clase de proxy encarga automáticamente al enumerar las `Container.Products` colección en la **foreach** bucle.

Al ejecutar la aplicación, el resultado debería ser similar al siguiente:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

Para obtener una entidad por identificador, use un `where` cláusula.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

Para el resto de este tema, no mostrar toda la matriz `Main` funcione, solo el código necesario para llamar al servicio.

## <a name="apply-query-options"></a>Aplicar opciones de consulta

OData define [opciones de consulta](../supporting-odata-query-options.md) que se puede utilizar para filtrar, ordenar, datos de la página y así sucesivamente. En el proxy de servicio, puede aplicar estas opciones mediante el uso de varias expresiones de LINQ.

En esta sección, voy a explicar ejemplos breves. Para obtener más información, vea el tema [consideraciones sobre LINQ (WCF Data Services)](https://msdn.microsoft.com/en-us/library/ee622463.aspx) en MSDN.

### <a name="filtering-filter"></a>Filtrado ($filter)

Para filtrar, use un `where` cláusula. El ejemplo siguiente se filtra por categoría de producto.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

Este código corresponde a la siguiente consulta de OData.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

Tenga en cuenta que el proxy convierte el `where` cláusula en una OData `$filter` expresión.

### <a name="sorting-orderby"></a>Ordenación ($orderby)

Para ordenar, use un `orderby` cláusula. En el ejemplo siguiente se ordenan por precio, de mayor a menor.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

Esta es la solicitud de OData correspondiente.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a>Paginación del cliente ($skip y $top)

Para los conjuntos de entidades de gran tamaño, el cliente desee limitar el número de resultados. Por ejemplo, un cliente podría mostrar 10 entradas a la vez. Esto se denomina *paginación del lado cliente*. (También hay [paginación del lado servidor](../supporting-odata-query-options.md#server-paging), donde el servidor limita el número de resultados.) Para llevar a cabo la paginación del lado cliente, usar LINQ **omitir** y **tomar** métodos. En el ejemplo siguiente se omite los primeros 40 resultados y toma los 10 siguientes.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

Esta es la solicitud de OData correspondiente:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a>Select ($select) y expandir ($expand)

Para incluir las entidades relacionadas, use la `DataServiceQuery<t>.Expand` método. Por ejemplo, para incluir la `Supplier` para cada `Product`:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

Esta es la solicitud de OData correspondiente:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

Para cambiar la forma de la respuesta, use LINQ **seleccione** cláusula. En el ejemplo siguiente se obtiene simplemente el nombre de cada producto, con ninguna otra propiedad.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

Esta es la solicitud de OData correspondiente:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

Una cláusula select puede incluir las entidades relacionadas. En ese caso, no llame a **expandir**; el proxy automáticamente incluye la expansión en este caso. En el ejemplo siguiente se obtiene el nombre y el proveedor de cada producto.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

Esta es la solicitud de OData correspondiente. Tenga en cuenta que incluye el **$expand** opción.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

Para obtener más información acerca de $select y $, expanda, consulte [con $select, $expand y $value en API Web 2](../using-select-expand-and-value.md).

## <a name="add-a-new-entity"></a>Agregar una nueva entidad

Para agregar una nueva entidad a un conjunto de entidades, llame `AddToEntitySet`, donde *EntitySet* es el nombre del conjunto de entidades. Por ejemplo, `AddToProducts` agrega un nuevo `Product` a la `Products` conjunto de entidades. Al generar el proxy, WCF Data Services crea automáticamente estos fuertemente tipado **AddTo** métodos.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

Para agregar un vínculo entre dos entidades, use la **AddLink** y **SetLink** métodos. El código siguiente agrega un nuevo proveedor y un nuevo producto y, a continuación, crea vínculos entre ellos.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

Use **AddLink** cuando la propiedad de navegación es una colección. En este ejemplo, vamos a agregar un producto a la `Products` colección en el proveedor.

Use **SetLink** cuando la propiedad de navegación es una entidad única. En este ejemplo, estamos estableciendo la `Supplier` propiedad en el producto.

## <a name="update--patch"></a>Actualizar / Patch

Para actualizar una entidad, llame a la **UpdateObject** método.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

La actualización se realiza cuando se llama a **SaveChanges**. De forma predeterminada, WCF envía una solicitud HTTP MERGE. El **PatchOnUpdate** opción indica a WCF para enviar una revisión de HTTP en su lugar.

> [!NOTE]
> ¿Por qué revisión frente a combinación? La especificación de HTTP 1.1 original ([RCF 2616](http://tools.ietf.org/html/rfc2616)) no se definió ningún método HTTP con la semántica de "actualización parcial". Para admitir actualizaciones parciales, la especificación OData define el método de mezcla. En 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) definido por el método de revisión para actualizaciones parciales. Puede leer parte del historial en este [entrada de blog](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) en el Blog de WCF Data Services. En la actualidad, revisión es preferible MERGE. El controlador de OData creado mediante el scaffolding de Web API es compatible con ambos métodos.


Si desea reemplazar toda la entidad (PUT semántica), especifique la **ReplaceOnUpdate** opción. Esto hace que WCF enviar una solicitud HTTP PUT.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a>Eliminar una entidad

Para eliminar una entidad, llame a **DeleteObject**.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a>Invocar una acción de OData

En OData, [acciones](odata-actions.md) son una forma de agregar los comportamientos de servidor que no se definen fácilmente como operaciones CRUD en entidades.

Aunque el documento de metadatos de OData describe las acciones, la clase de proxy no crea ningún método fuertemente tipado para ellos. Todavía se puede invocar una acción de OData mediante el uso de la interfaz genérica **Execute** método. Sin embargo, debe conocer los tipos de datos de los parámetros y el valor devuelto.

Por ejemplo, el `RateProduct` acción toma el parámetro denominado "Clasificación" de tipo `Int32` y devuelve un `double`. El código siguiente muestra cómo invocar esta acción.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

Para obtener más información, consulte[al llamar a operaciones de servicio y las acciones](https://msdn.microsoft.com/en-us/library/hh230677.aspx).

Es una opción ampliar la **contenedor** clase para proporcionar un método fuertemente tipado que invoca la acción:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
