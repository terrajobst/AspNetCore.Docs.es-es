---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Compatibilidad con las opciones de consulta de OData en ASP.NET Web API 2 | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/04/2013
ms.topic: article
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 004c029db6f01627f7cadff26aaf5554ce2b93a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a>Compatibilidad con opciones de consulta de OData en ASP.NET Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

OData define los parámetros que pueden utilizarse para modificar una consulta de OData. El cliente envía estos parámetros en la cadena de consulta de la URI de solicitud. Por ejemplo, para ordenar los resultados, un cliente usa el parámetro $orderby:

`http://localhost/Products?$orderby=Name`

La especificación OData llama a estos parámetros *opciones de consulta*. Puede habilitar las opciones de consulta de OData para cualquier controlador de Web API en su proyecto & #8212; el controlador no deben ser un extremo de OData. Esto proporciona una manera cómoda de agregar características como el filtrado y ordenación, en cualquier aplicación de API Web.

Antes de habilitar las opciones de consulta, lea el tema [Guía de seguridad de OData](odata-security-guidance.md).

- [Habilitar las opciones de consulta de OData](#enable)
- [Consultas de ejemplo](#examples)
- [Paginación controlada por el servidor](#server-paging)
- [Limitar las opciones de consulta](#limiting_query_options)
- [Invocar directamente las opciones de consulta](#ODataQueryOptions)
- [Validación de consulta](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a>Habilitar las opciones de consulta de OData

Web API es compatible con las siguientes opciones de consulta de OData:

| Opción | Descripción |
| --- | --- |
| $expand | Se expande en línea de las entidades relacionadas. |
| $filter | Filtra los resultados, en función de una condición booleana. |
| $inlinecount | Indica al servidor que incluya el recuento total de entidades coincidentes en la respuesta. (Útil para la paginación del lado servidor). |
| $orderby | Ordena los resultados. |
| $select | Selecciona las propiedades que desea incluir en la respuesta. |
| $skip | Omite los n primeros resultados. |
| $top | Devuelve solo lo primeros n los resultados. |

Para utilizar las opciones de consulta de OData, debe habilitarlas explícitamente. Puede habilitarlas globalmente para toda la aplicación o habilitarlas para determinados controladores o acciones específicas.

Para habilitar las opciones de consulta de OData globalmente, llame a **EnableQuerySupport** en el **HttpConfiguration** clase durante el inicio:

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

El **EnableQuerySupport** método habilita opciones de consulta global para cualquier acción de controlador que devuelve un **IQueryable** tipo. Si no desea que las opciones de consulta habilitadas para toda la aplicación, puede habilitarlas para las acciones de un controlador específico agregando el **[Queryable]** atributo al método de acción.

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a>Consultas de ejemplo

Esta sección muestra los tipos de consultas que son posibles mediante las opciones de consulta de OData. Para obtener detalles sobre las opciones de consulta, consulte la documentación de OData en [www.odata.org](http://www.odata.org/).

Para obtener información acerca de $expanda y $select, consulte [con $select, $expand y $value de OData de ASP.NET Web API](using-select-expand-and-value.md).

**Paginación controlada por el cliente**

Para los conjuntos de entidades de gran tamaño, el cliente desee limitar el número de resultados. Por ejemplo, un cliente podría mostrar 10 entradas a la vez, con vínculos "siguiente" para obtener la siguiente página de resultados. Para ello, el cliente utiliza las opciones $top y $skip.

`http://localhost/Products?$top=10&$skip=20`

La opción $top indica el número máximo de entradas que se devolverá, y la opción $skip proporciona el número de entradas que se omitirán. El ejemplo anterior captura las entradas de 21 a 30.

**Filtrado**

La opción $filter permite que un cliente filtrar los resultados mediante la aplicación de una expresión booleana. Las expresiones de filtro son muy eficaces; incluyen operadores lógicos y aritméticos, funciones de cadena y funciones de fecha.

| Devolver todos los productos con categoría igual a "Toys". | `http://localhost/Products?$filter=Category`EQ 'Toys' |
| --- | --- |
| Devolver todos los productos con precio inferior a 10. | `http://localhost/Products?$filter=Price`lt 10 |
| Operadores lógicos: devolver todos los productos where precio > = 5 y el precio < = 15. | `http://localhost/Products?$filter=Price`GE 5 y precio le 15 |
| Funciones de cadena: devolver todos los productos con "zz" en el nombre. | `http://localhost/Products?$filter=substringof('zz',Name)` |
| Funciones de fecha: devolver todos los productos con ReleaseDate después 2005. | `http://localhost/Products?$filter=year(ReleaseDate)`gt 2005 |

**Ordenar**

Para ordenar los resultados, use el filtro de $orderby.

| Ordenar por precio. | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| Ordenar por precio de forma descendente (de mayor a menor). | `http://localhost/Products?$orderby=Price desc` |
| Ordenar por categoría y, a continuación, ordenar por precio de forma descendente dentro de las categorías. | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a>Paginación controlada por el servidor

Si la base de datos contiene millones de registros, que no desea enviarlos en una carga. Para evitar esto, el servidor puede limitar el número de entradas que envía en una única respuesta. Para habilitar la paginación en el servidor, establezca el **PageSize** propiedad en el **Queryable** atributo. El valor es el número máximo de entradas que se devolverá.

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

Si el controlador devuelve el formato OData, el cuerpo de respuesta contendrá un vínculo a la página siguiente de datos:

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

El cliente puede usar este vínculo para obtener la página siguiente. Para obtener información sobre el número total de entradas en el conjunto de resultados, el cliente puede establecer la opción de consulta $inlinecount con el valor "allpages".

`http://localhost/Products?$inlinecount=allpages`

El valor "allpages" indica al servidor que incluya el recuento total de la respuesta:

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> Vínculos de página siguiente y el recuento alineado requieren un formato OData. La razón es que OData define campos especiales en el cuerpo de respuesta para mantener el vínculo y el recuento.


Para conocer los formatos no OData, todavía es posible admitir el número de vínculos y en línea de la página siguiente, ajustando los resultados de consulta en un **PageResult&lt;T&gt;**  objeto. Sin embargo, requiere un poco más código. A continuación se muestra un ejemplo:

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

Este es un respuesta JSON de ejemplo:

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a>Limitar las opciones de consulta

Las opciones de consulta proporcionan al cliente un gran control sobre la consulta que se ejecuta en el servidor. En algunos casos, puede limitar las opciones disponibles por motivos de seguridad o de rendimiento. El **[Queryable]** atributo ha algunos creados en las propiedades para este. Estos son algunos ejemplos:

Permitir sólo $skip y $top, para admitir la paginación y nada más:

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

Permite ordenar solo por determinadas propiedades evitar la ordenación en las propiedades que no están indizadas en la base de datos:

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

Permitir la función lógica "eq" pero no otras funciones lógicas:

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

No se permiten a los operadores aritméticos:

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

Puede restringir opciones globalmente mediante la creación de un **QueryableAttribute** instancia y pasarlo a la **EnableQuerySupport** función:

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a>Invocar directamente las opciones de consulta

En lugar de utilizar el **[Queryable]** atributo, puede invocar las opciones de consulta directamente en el controlador. Para ello, agregue un **ODataQueryOptions** parámetro para el método del controlador. En este caso, no es necesario el **[Queryable]** atributo.

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

API Web rellena la **ODataQueryOptions** del URI de cadena de consulta. Para aplicar la consulta, pase una **IQueryable** a la **ApplyTo** método. El método devuelve otro **IQueryable**.

Para escenarios avanzados, si no tiene un **IQueryable** consultar al proveedor, puede examinar la **ODataQueryOptions** y traducir las opciones de consulta a otra forma. (Por ejemplo, consulte Entrada de blog de RaghuRam Nadiminti [consultas traducir OData en HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), que también incluye una [ejemplo](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)

<a id="query-validation"></a>
## <a name="query-validation"></a>Validación de consulta

El **[Queryable]** atributo valida la consulta antes de ejecutarlo. El paso de validación se realiza en el **QueryableAttribute.ValidateQuery** método. También puede personalizar el proceso de validación.

Consulte también [Guía de seguridad de OData](odata-security-guidance.md).

En primer lugar, invalidación uno del validador clases que es definido en el **Web.Http.OData.Query.Validators** espacio de nombres. Por ejemplo, la siguiente clase de validador deshabilita la opción "desc" para la opción $orderby.

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

Subclase la **[Queryable]** atributo para invalidar la **ValidateQuery** método.

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

A continuación, establezca el atributo personalizado ya sea globalmente o por el controlador:

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

Si utilizas **ODataQueryOptions** establecer directamente, el validador en las opciones:

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
