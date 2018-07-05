---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Compatibilidad con las opciones de consulta de OData en ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 02/04/2013
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 029a791074a5ab88f1c6d83311e88fc9f726fe6f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814946"
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a>Compatibilidad con las opciones de consulta de OData en ASP.NET Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

OData define los parámetros que pueden usarse para modificar una consulta de OData. El cliente envía estos parámetros en la cadena de consulta del URI de solicitud. Por ejemplo, para ordenar los resultados, un cliente usa el parámetro $orderby:

`http://localhost/Products?$orderby=Name`

La especificación OData llama a estos parámetros *opciones de consulta*. Puede habilitar las opciones de consulta de OData para cualquier controlador de API Web en el proyecto &#8212; el controlador no necesita ser un extremo de OData. Esto le ofrece una manera cómoda de agregar características como el filtrado y ordenación, en cualquier aplicación de API Web.

Antes de habilitar las opciones de consulta, lea el tema [OData Security Guidance](odata-security-guidance.md).

- [Habilitar las opciones de consulta de OData](#enable)
- [Consultas de ejemplo](#examples)
- [Paginación controlada por servidor](#server-paging)
- [Limitar las opciones de consulta](#limiting_query_options)
- [Invocar directamente las opciones de consulta](#ODataQueryOptions)
- [Validación de la consulta](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a>Habilitar las opciones de consulta de OData

Web API es compatible con las siguientes opciones de consulta de OData:

| Opción | Descripción |
| --- | --- |
| $expand | Se expande en línea de las entidades relacionadas. |
| $filter | Filtra los resultados, según una condición booleana. |
| $inlinecount | Indica al servidor que incluya el recuento total de las entidades coincidentes en la respuesta. (Útil para la paginación del lado servidor). |
| $orderby | Ordena los resultados. |
| $select | Selecciona las propiedades que se va a incluir en la respuesta. |
| $skip | Omite los primeros n resultados. |
| $top | Devuelve solo las primeras n los resultados. |

Para usar las opciones de consulta de OData, debe habilitarlas explícitamente. Puede habilitar de forma global para toda la aplicación o habilitarlas para determinados controladores o acciones específicas.

Para habilitar las opciones de consulta de OData de forma global, llame a **EnableQuerySupport** en el **HttpConfiguration** clase en el inicio:

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

El **EnableQuerySupport** método permite opciones de consulta globalmente para cualquier acción de controlador que devuelve un **IQueryable** tipo. Si no desea que las opciones de consulta habilitadas para toda la aplicación, puede habilitarlas para acciones de controlador específico agregando el **[Queryable]** atributo al método de acción.

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a>Consultas de ejemplo

Esta sección muestra los tipos de consultas que son posibles mediante las opciones de consulta de OData. Para obtener información específica sobre las opciones de consulta, consulte la documentación de OData en [www.odata.org](http://www.odata.org/).

Para obtener información acerca de $expanda y $select, consulte [usar $select, $expand y $value en ASP.NET Web API OData](using-select-expand-and-value.md).

**Paginación controlada por cliente**

Para los conjuntos de entidades de gran tamaño, el cliente podría limitar el número de resultados. Por ejemplo, un cliente podría mostrar 10 entradas a la vez, con vínculos "siguiente" para obtener la página siguiente de resultados. Para ello, el cliente usa las opciones de $top y $skip.

`http://localhost/Products?$top=10&$skip=20`

La opción $top proporciona el número máximo de entradas que se devuelven y la opción de $skip proporciona el número de entradas que se omitirán. El ejemplo anterior captura las entradas de 21 a 30.

**Filtrado**

La opción $filter permite filtrar los resultados aplicando una expresión booleana a un cliente. Las expresiones de filtro son bastante eficaces; se incluyen operadores aritméticos y lógicos, las funciones de cadena y funciones de fecha.

| Devolver todos los productos con categoría igual a "Toys". | `http://localhost/Products?$filter=Category` EQ 'Toys' |
| --- | --- |
| Devolver todos los productos con precio inferior a 10. | `http://localhost/Products?$filter=Price` lt 10 |
| Operadores lógicos: devolver todos los productos where precio > = 5 y el precio < = 15. | `http://localhost/Products?$filter=Price` GE 5 y el precio 15 |
| Funciones de cadena: devolver todos los productos con "zz" en el nombre. | `http://localhost/Products?$filter=substringof('zz',Name)` |
| Las funciones de fecha: devolver todos los productos con ReleaseDate después de 2005. | `http://localhost/Products?$filter=year(ReleaseDate)` gt 2005 |

**Ordenación**

Para ordenar los resultados, use el filtro $orderby.

| Ordenar por precio. | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| Ordenar por precio de manera descendente (de mayor a menor). | `http://localhost/Products?$orderby=Price desc` |
| Ordenar por categoría y, a continuación, ordenar por precio de manera descendente dentro de categorías. | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a>Paginación controlada por servidor

Si la base de datos contiene millones de registros, no desea enviarles todo en una carga. Para evitar esto, el servidor puede limitar el número de entradas que envía en una sola respuesta. Para habilitar la paginación en el servidor, establezca el **PageSize** propiedad en el **Queryable** atributo. El valor es el número máximo de entradas que se devolverá.

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

Si el controlador devuelve el formato OData, el cuerpo de respuesta contendrá un vínculo a la página siguiente de datos:

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

El cliente puede usar este vínculo para capturar la página siguiente. Para obtener información sobre el número total de entradas en el conjunto de resultados, el cliente puede establecer la opción de consulta $inlinecount con el valor "allpages".

`http://localhost/Products?$inlinecount=allpages`

El valor "allpages" indica al servidor que incluya el recuento total en la respuesta:

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> Vínculos de página siguiente y el recuento alineado requieren el formato OData. El motivo es que OData define campos especiales en el cuerpo de respuesta que contenga el vínculo y el recuento.


Para formatos que no sean OData, resulta todavía posible admitir el recuento de vínculos y en línea de la página siguiente, ajustando los resultados de consulta en un **PageResult&lt;T&gt;**  objeto. Sin embargo, requiere un poco más código. A continuación se muestra un ejemplo:

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

Este es un ejemplo de respuesta JSON:

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a>Limitar las opciones de consulta

Las opciones de consulta proporcionan al cliente de un gran control sobre la consulta que se ejecuta en el servidor. Es posible que en algunos casos, conviene que limite las opciones disponibles por motivos de seguridad o rendimiento. El **[Queryable]** atributo algunos incorpora las propiedades para este. Estos son algunos ejemplos:

Permitir solo $skip y $top, para admitir la paginación y nada más:

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

Permitir ordenación sólo con determinadas propiedades evitar la ordenación en las propiedades que no están indizadas en la base de datos:

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

Permitir la función lógica "eq" pero no hay otras funciones lógicas:

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

No se permiten a los operadores aritméticos:

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

Puede restringir las opciones de globalmente mediante la creación de un **QueryableAttribute** instancia y pasarlo a la **EnableQuerySupport** función:

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a>Invocar directamente las opciones de consulta

En lugar de usar el **[Queryable]** atributo, puede invocar las opciones de consulta directamente en el controlador. Para ello, agregue un **ODataQueryOptions** parámetro del método de controlador. En este caso, no es necesario el **[Queryable]** atributo.

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

API Web rellena la **ODataQueryOptions** desde el URI de cadena de consulta. Para aplicar la consulta, pase un **IQueryable** a la **ApplyTo** método. El método devuelve otro **IQueryable**.

Para escenarios avanzados, si no tiene un **IQueryable** proveedor de consultas, puede examinar el **ODataQueryOptions** y traducir las opciones de consulta a otra forma. (Por ejemplo, consulte Entrada de blog de RaghuRam Nadiminti [consultas traducir OData en HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), que también incluye un [ejemplo](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)

<a id="query-validation"></a>
## <a name="query-validation"></a>Validación de la consulta

El **[Queryable]** atributo valida la consulta antes de ejecutarlo. El paso de validación se realiza en el **QueryableAttribute.ValidateQuery** método. También puede personalizar el proceso de validación.

Consulte también [OData Security Guidance](odata-security-guidance.md).

En primer lugar, reemplazo una del validador de clases que es definido en el **Web.Http.OData.Query.Validators** espacio de nombres. Por ejemplo, la siguiente clase de validador deshabilita la opción 'desc' para la opción $orderby.

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

Subclase la **[Queryable]** atributo para invalidar el **validatequery de la** método.

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

A continuación, establezca el atributo personalizado ya sea globalmente o por controlador:

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

Si usas **ODataQueryOptions** establecer directamente, el validador en las opciones:

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
