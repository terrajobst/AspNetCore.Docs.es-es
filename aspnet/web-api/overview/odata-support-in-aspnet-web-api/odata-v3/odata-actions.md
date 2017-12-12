---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Compatibilidad con las acciones de OData en ASP.NET Web API 2 | Documentos de Microsoft
author: MikeWasson
description: "En OData, las acciones son una manera de agregar los comportamientos de servidor que no se definen fácilmente como operaciones CRUD en entidades. Algunos usos de las acciones son: implemente..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: acb369ca8f1bab8d7cad14c15f46cfd44beb9fdd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a>Compatibilidad con las acciones de OData en ASP.NET Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> En OData, *acciones* son una forma de agregar los comportamientos de servidor que no se definen fácilmente como operaciones CRUD en entidades. Algunos usos de las acciones son:
> 
> - Implementación de las transacciones complejas.
> - La manipulación de varias entidades a la vez.
> - Permitir actualizaciones solo a determinadas propiedades de una entidad.
> - Enviar información al servidor que no está definido en una entidad.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - Web API 2
> - Versión 3 de OData
> - Entity Framework 6


## <a name="example-rating-a-product"></a>Ejemplo: Clasificación de un producto

En este ejemplo, queremos que los usuarios puedan clasificar los productos y, a continuación, exponer el promedio de clasificaciones para cada producto. En la base de datos, se almacenará una lista de clasificaciones, organizados en productos.

Este es el modelo que podríamos utilizar para representar las clasificaciones en Entity Framework:

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

Pero no queremos que los clientes para registrar un `ProductRating` objeto a una colección de "Clasificaciones". De manera intuitiva, la clasificación está asociada a la colección de productos y el cliente solo se tiene que registrar el valor de clasificación.

Por lo tanto, en lugar de utilizar las operaciones CRUD normales, definimos una acción que un cliente puede invocar en un producto. En la terminología de OData, la acción es *enlazados* a entidades de producto.

>Las acciones tienen efectos secundarios en el servidor. Por este motivo, se invocan mediante solicitudes HTTP POST. Las acciones pueden tener parámetros y tipos de valor devuelto, que se describen en los metadatos del servicio. El cliente envía los parámetros en el cuerpo de solicitud y el servidor envía el valor devuelto en el cuerpo de respuesta. Para invocar la acción "Velocidad producto", el cliente envía una operación POST a un URI similar al siguiente:

[!code-console[Main](odata-actions/samples/sample2.cmd)]

Los datos de la solicitud POST están simplemente la clasificación de producto:

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a>Declarar la acción en el Entity Data Model

En la configuración de la API Web, agregue la acción para el entity data model (EDM):

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

Este código define "RateProduct" como una acción que se pueden realizar en entidades de producto. También declara que la acción que emprende una **int** parámetro denominado "Clasificación" y devuelve un **int** valor.

## <a name="add-the-action-to-the-controller"></a>Agregar la acción para el controlador

La acción "RateProduct" se enlaza a las entidades de producto. Para implementar la acción, agregue un método denominado `RateProduct` al controlador de productos:

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

Tenga en cuenta que el nombre del método coincide con el nombre de la acción en el EDM. El método tiene dos parámetros:

- *clave*: la clave del producto a velocidad.
- *parámetros*: un diccionario de valores de parámetro de acción.

Si usa las convenciones de enrutamiento de forma predeterminada, el parámetro de clave debe denominarse "clave". También es importante incluir la **[FromOdataUri]** de atributo, como se muestra. Este atributo indica a API Web para usar las reglas de sintaxis de OData cuando analiza la clave de URI de la solicitud.

Use la *parámetros* diccionario para obtener los parámetros de acción:

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

Si el cliente envía los parámetros de acción en el valor correcto de formato, el valor de **ModelState.IsValid** es true. En ese caso, puede usar el **ODataActionParameters** diccionario para obtener los valores de parámetro. En este ejemplo, el `RateProduct` acción toma un parámetro único denominado "Clasificación".

## <a name="action-metadata"></a>Metadatos de acción

Para ver los metadatos del servicio, envía una solicitud GET a los metadatos de /odata/$. Esta es la parte de los metadatos que declara el `RateProduct` acción:

[!code-xml[Main](odata-actions/samples/sample7.xml)]

El **FunctionImport** elemento declara la acción. La mayoría de los campos es autoexplicativo, pero dos son cabe destacar:

- **IsBindable** significa que la acción se puede invocar en la entidad de destino, al menos parte del tiempo.
- **IsAlwaysBindable** significa que siempre se puede invocar la acción en la entidad de destino.

La diferencia es que algunas acciones siempre están disponibles para los clientes, pero otras acciones podrían depender del estado de la entidad. Por ejemplo, supongamos que define una acción de "Compra". Solo puede adquirir un elemento que está en el almacén. Si el elemento está agotado, un cliente no puede invocar la acción.

Al definir el EDM, el **acción** método crea una acción siempre puede enlazar:

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

Explicaré acciones no siempre enlazable (también denominada *transitorio* acciones) más adelante en este tema.

## <a name="invoking-the-action"></a>Invocar la acción

Ahora veamos cómo un cliente invocaría esta acción. Suponga que el cliente desea proporcionar una clasificación de 2 para el producto con el identificador = 4. Este es un mensaje de solicitud de ejemplo, con formato JSON para el cuerpo de solicitud:

[!code-console[Main](odata-actions/samples/sample9.cmd)]

Este es el mensaje de respuesta:

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a>Enlazar una acción a un conjunto de entidades

En el ejemplo anterior, la acción está enlazada a una entidad individual: el cliente clasifica un único producto. También puede enlazar una acción a una colección de entidades. Solo realice los cambios siguientes:

En el EDM, agregue la acción a la entidad **colección** propiedad.

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

En el método del controlador, omitir el *clave* parámetro.

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

Ahora el cliente invoca la acción en el conjunto de entidades de productos:

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a>Acciones con los parámetros de recopilación

Las acciones pueden tener parámetros que toman una colección de valores. En el EDM, use **CollectionParameter&lt;T&gt;**  para declarar el parámetro.

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

Esto declara un parámetro denominado "Clasificaciones" que toma una colección de **int** valores. En el método del controlador, obtendrá aún el valor del parámetro de la **ODataActionParameters** objeto, pero ahora el valor es un **ICollection&lt;int&gt;**  valor:

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a>Acciones transitorias

En el ejemplo "RateProduct", los usuarios siempre pueden clasificar un producto, por lo que la acción siempre está disponible. Pero algunas acciones dependen del estado de la entidad. Por ejemplo, en un servicio de alquiler de vídeo, la acción de "Extracción" no está siempre disponible. (Depende si hay disponible una copia de dicho vídeo.) Este tipo de acción se denomina un *transitorio* acción.

En los metadatos del servicio, tiene una acción transitoria **IsAlwaysBindable** igual a false. Que es realmente el valor predeterminado, por lo que los metadatos tendrá un aspecto similar al siguiente:

[!code-xml[Main](odata-actions/samples/sample16.xml)]

Aquí por qué esto es importante: si una acción es transitoria, el servidor debe indicar al cliente cuando la acción está disponible. Para ello incluye un vínculo a la acción de la entidad. Este es un ejemplo de una entidad de película:

[!code-console[Main](odata-actions/samples/sample17.cmd)]

La propiedad "#CheckOut" contiene un vínculo a la acción de extracción del repositorio. Si la acción no está disponible, el servidor omite el vínculo.

Para declarar una acción transitoria en el EDM, llame a la **TransientAction** método:

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

Además, debe proporcionar una función que devuelve un vínculo de acción para una entidad dada. Establezca esta función mediante una llamada a **HasActionLink**. Puede escribir la función como una expresión lambda:

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

Si la acción está disponible, la expresión lambda devuelve un vínculo a la acción. El serializador de OData incluye este vínculo cuando serializa la entidad. Cuando la acción no está disponible, la función devuelve `null`.

## <a name="additional-resources"></a>Recursos adicionales

[Ejemplo de acciones de OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
