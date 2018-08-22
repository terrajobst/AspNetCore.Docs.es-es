---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Llamar a un servicio de OData desde un cliente .NET (C#) | Microsoft Docs
author: MikeWasson
description: Este tutorial muestra cómo llamar a un servicio de OData desde una aplicación cliente de C#. Versiones de software que se usa en el tutorial Visual Studio 2013 (funciona con Visual S...
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 45d4d1304d2c628d2ab0d09925d2050bf6f58c60
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838732"
---
<a name="calling-an-odata-service-from-a-net-client-c"></a>Llamar a un servicio de OData desde un cliente .NET (C#)
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargue el proyecto completado](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Este tutorial muestra cómo llamar a un servicio de OData desde una aplicación cliente de C#.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (funciona con Visual Studio 2012)
> - [Biblioteca cliente de Servicios de datos de WCF](https://msdn.microsoft.com/library/cc668772.aspx)
> - Web API 2. (El servicio de OData de ejemplo se compila con Web API 2, pero la aplicación cliente no depende de la API Web).


En este tutorial, lo guiaré a través de la creación de una aplicación cliente que llama a un servicio de OData. El servicio de OData expone las siguientes entidades:

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

Los artículos siguientes describen cómo implementar el servicio de OData en Web API. (No necesita leerlos para entender este tutorial, sin embargo).

- [Creación de un extremo de OData en Web API 2](creating-an-odata-endpoint.md)
- [Relaciones de entidad de OData en Web API 2](working-with-entity-relations.md)
- [Acciones de OData en Web API 2](odata-actions.md)

## <a name="generate-the-service-proxy"></a>Generar al Proxy de servicio

El primer paso es generar a un proxy de servicio. El proxy de servicio es una clase .NET que define los métodos para acceder al servicio de OData. El proxy traduce llamadas de método en las solicitudes HTTP.

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

Comience abriendo el proyecto de servicio de OData en Visual Studio. Presione CTRL + F5 para ejecutar el servicio localmente en IIS Express. Tenga en cuenta la dirección local, incluido el número de puerto que Visual Studio asigna. Necesitará esta dirección cuando se crea el proxy.

A continuación, abra otra instancia de Visual Studio y cree un proyecto de aplicación de consola. La aplicación de consola será nuestra aplicación de cliente de OData. (También puede agregar el proyecto a la misma solución que el servicio.)

> [!NOTE]
> El proyecto de consola, consulte los pasos restantes.


En el Explorador de soluciones, haga clic en **referencias** y seleccione **Add Service Reference**.

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

En el **Add Service Reference** cuadro de diálogo, escriba la dirección del servicio OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

donde *puerto* es el número de puerto.

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

Para **Namespace**, escriba "ProductService". Esta opción define el espacio de nombres de la clase de proxy.

Haga clic en **Ir**. Visual Studio lee el documento de metadatos de OData para detectar las entidades en el servicio.

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

Haga clic en **Aceptar** para agregar la clase de proxy a su proyecto.

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a>Cree una instancia de la clase de Proxy de servicio

Dentro de su `Main` método, cree una nueva instancia de la clase de proxy, como se indica a continuación:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

De nuevo, use el número de puerto real donde se está ejecutando el servicio. Al implementar el servicio, usará el URI del servicio en directo. No es necesario actualizar al servidor proxy.

El código siguiente agrega un controlador de eventos que imprime los URI de solicitud a la ventana de consola. Este paso no es necesario, pero resulta interesante ver a los URI para cada consulta.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a>Consultar el servicio

El código siguiente obtiene la lista de productos desde el servicio de OData.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

Tenga en cuenta que no es necesario escribir ningún código para enviar la solicitud HTTP o analizar la respuesta. La clase de proxy encarga automáticamente al enumerar los `Container.Products` colección en el **foreach** bucle.

Al ejecutar la aplicación, el resultado debe ser similar al siguiente:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

Para obtener una entidad por identificador, use un `where` cláusula.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

Para el resto de este tema, que no se muestra todo el `Main` funcione, solo el código necesario para llamar al servicio.

## <a name="apply-query-options"></a>Aplicar las opciones de consulta

OData define [opciones de consulta](../supporting-odata-query-options.md) que se puede utilizar para filtrar, ordenar, paginar los datos y así sucesivamente. En el proxy de servicio, puede aplicar estas opciones mediante el uso de varias expresiones de LINQ.

En esta sección, le mostraré ejemplos breves. Para obtener más información, vea el tema [consideraciones sobre LINQ (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) en MSDN.

### <a name="filtering-filter"></a>Filtrado ($filter)

Para filtrar, use un `where` cláusula. El ejemplo siguiente se filtra por categoría de producto.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

Este código corresponde a la siguiente consulta de OData.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

Tenga en cuenta que el proxy convierte la `where` cláusula en una OData `$filter` expresión.

### <a name="sorting-orderby"></a>Ordenación ($orderby)

Para ordenar, use un `orderby` cláusula. El ejemplo siguiente se ordena por el precio de mayor a menor.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

Esta es la solicitud de OData correspondiente.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a>Paginación del cliente ($skip y $top)

Para los conjuntos de entidades de gran tamaño, el cliente podría limitar el número de resultados. Por ejemplo, un cliente podría mostrar 10 entradas a la vez. Esto se denomina *paginación del lado cliente*. (También hay [paginación del lado servidor](../supporting-odata-query-options.md#server-paging), donde el servidor limita el número de resultados.) Para realizar la paginación del lado cliente, use el LINQ **omitir** y **tomar** métodos. El ejemplo siguiente omite los primeros 40 resultados y toma los 10 siguientes.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

Esta es la solicitud de OData correspondiente:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a>Select ($select) y expandir ($expand)

Para incluir las entidades relacionadas, use el `DataServiceQuery<t>.Expand` método. Por ejemplo, para incluir la `Supplier` para cada `Product`:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

Esta es la solicitud de OData correspondiente:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

Para cambiar la forma de la respuesta, use el LINQ **seleccione** cláusula. El ejemplo siguiente obtiene solo el nombre de cada producto, con ninguna otra propiedad.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

Esta es la solicitud de OData correspondiente:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

Una cláusula select puede incluir las entidades relacionadas. En ese caso, no llame a **expandir**; el proxy incluye automáticamente la expansión en este caso. En el ejemplo siguiente se obtiene el nombre y el proveedor de cada producto.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

Esta es la solicitud de OData correspondiente. Tenga en cuenta que incluye la **$expand** opción.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

Para obtener más información acerca de $select y $expand, expanda, consulte [usar $select, $expand y $value en Web API 2](../using-select-expand-and-value.md).

## <a name="add-a-new-entity"></a>Agregar una nueva entidad

Para agregar una nueva entidad a un conjunto de entidades, llame `AddToEntitySet`, donde *EntitySet* es el nombre del conjunto de entidades. Por ejemplo, `AddToProducts` agrega un nuevo `Product` a la `Products` conjunto de entidades. Al generar el proxy, WCF Data Services crea automáticamente estos fuertemente tipado **AddTo** métodos.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

Para agregar un vínculo entre dos entidades, utilice el **AddLink** y **SetLink** métodos. El código siguiente agrega un nuevo proveedor y un nuevo producto y, a continuación, crea vínculos entre ellos.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

Use **AddLink** cuando la propiedad de navegación es una colección. En este ejemplo, estamos agregando un producto para el `Products` colección en el proveedor.

Use **SetLink** cuando la propiedad de navegación es una entidad única. En este ejemplo, estamos estableciendo el `Supplier` propiedad sobre el producto.

## <a name="update--patch"></a>Actualizar / aplicar revisiones

Para actualizar una entidad, llame a la **UpdateObject** método.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

La actualización se realiza cuando se llama a **SaveChanges**. De forma predeterminada, WCF envía una solicitud HTTP MERGE. El **PatchOnUpdate** opción indica a WCF para enviar un HTTP PATCH en su lugar.

> [!NOTE]
> ¿Por qué revisiones frente a la combinación? La especificación HTTP 1.1 original ([RCF 2616](http://tools.ietf.org/html/rfc2616)) no se definió ningún método HTTP con la semántica de "actualización parcial". Para admitir actualizaciones parciales, la especificación OData define el método MERGE. En 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) definido por el método de revisión para las actualizaciones parciales. Puede leer algunos del historial de este [entrada de blog](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) en el Blog de WCF Data Services. Hoy en día, la revisión es preferible a combinar. El controlador de OData creado por el scaffolding de Web API es compatible con ambos métodos.


Si desea reemplazar toda la entidad (semántica PUT), especifique el **ReplaceOnUpdate** opción. Esto hace que WCF enviar una solicitud HTTP PUT.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a>Eliminar una entidad

Para eliminar una entidad, llame a **DeleteObject**.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a>Invocar una acción de OData

En OData, [acciones](odata-actions.md) son una forma de agregar comportamientos de lado servidor que no se definen fácilmente como operaciones CRUD en entidades.

Aunque el documento de metadatos de OData describe las acciones, la clase de proxy no crea ningún método fuertemente tipado para ellos. Se puede invocar una acción de OData mediante el uso genérico **Execute** método. Sin embargo, deberá conocer los tipos de datos de los parámetros y el valor devuelto.

Por ejemplo, el `RateProduct` acción toma el parámetro denominado "Rating" de tipo `Int32` y devuelve un `double`. El código siguiente muestra cómo invocar esta acción.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

Para obtener más información, consulte[llamar a operaciones de servicio y las acciones](https://msdn.microsoft.com/library/hh230677.aspx).

Es una opción ampliar la **contenedor** clase para proporcionar un método fuertemente tipado que invoca la acción:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
