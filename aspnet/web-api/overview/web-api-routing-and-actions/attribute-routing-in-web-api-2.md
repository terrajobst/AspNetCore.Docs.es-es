---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Atributo de enrutamiento en ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: d16dcc618bf6c60714179601db14f4dd2a9e41ce
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912157"
---
<a name="attribute-routing-in-aspnet-web-api-2"></a>Enrutamiento mediante atributos en ASP.NET Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

*Enrutamiento* es cómo API Web coincide con un URI a una acción. Web API 2 es compatible con un nuevo tipo de enrutamiento, llamado *enrutamiento mediante atributos*. Como el nombre implica, el enrutamiento mediante atributos utiliza atributos para definir las rutas. Enrutamiento mediante atributos proporciona mayor control sobre los URI de la API web. Por ejemplo, puede crear fácilmente los identificadores URI que se describen las jerarquías de recursos.

El estilo anterior de enrutamiento, llamado basado en convenciones de enrutamiento, sigue siendo totalmente compatible. De hecho, puede combinar ambas técnicas en el mismo proyecto.

En este tema se muestra cómo habilitar el enrutamiento mediante atributos y se describe las diversas opciones para el enrutamiento mediante atributos. Para obtener un tutorial to-end que usa el enrutamiento mediante atributos, vea [crear una API de REST con enrutamiento mediante atributos en Web API 2](create-a-rest-api-with-attribute-routing.md).

## <a name="prerequisites"></a>Requisitos previos

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional o Enterprise edition

Como alternativa, use el Administrador de paquetes de NuGet para instalar los paquetes necesarios. Desde el **herramientas** menú en Visual Studio, seleccione **Administrador de paquetes de NuGet**, a continuación, seleccione **Package Manager Console**. Escriba el siguiente comando en la ventana de consola de administrador de paquetes:

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a>¿Por qué enrutamiento mediante atributos?

La primera versión de API Web usa *basado en convenciones* enrutamiento. En ese tipo de enrutamiento, puede definir uno o más plantillas de ruta, que básicamente son cadenas de parámetros. Cuando el marco de trabajo recibe una solicitud, coincide con el URI en la plantilla de ruta. (Para obtener más información sobre el enrutamiento basado en convenciones, vea [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).

Una ventaja del enrutamiento basado en convenciones es que las plantillas se definen en un solo lugar y las reglas de enrutamiento se aplican de forma coherente en todos los controladores. Por desgracia, enrutamiento basado en convenciones hace más difícil admitir determinados patrones URI que son comunes en las API de RESTful. Por ejemplo, los recursos a menudo contienen recursos secundarios: los clientes tienen pedidos, las películas tienen los actores, tienen los autores de libros y así sucesivamente. Es natural para crear a los URI que reflejan estas relaciones:

`/customers/1/orders`

Este tipo de URI es difícil crear mediante enrutamiento basado en convenciones. Aunque es posible, los resultados no se escalan bien si tiene muchos controladores o tipos de recursos.

Con el enrutamiento mediante atributos, es muy fácil de definir una ruta para este URI. Simplemente agregue un atributo a la acción de controlador:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

Estos son algunos otros patrones de ese atributo enrutamiento hace fácil.

**Versiones de API**

En este ejemplo, "/ api/v1/products" sería enrutado a un controlador diferente que "/ v2/api/products".

`/api/v1/products`
`/api/v2/products`

**Segmentos URI sobrecargados**

En este ejemplo, "1" es un número de pedido, pero "pendiente" se asigna a una colección.

`/orders/1`
`/orders/pending`

**Varios tipos de parámetro**

En este ejemplo, "1" es un número de pedido, pero "2013/06/16" especifica una fecha.

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a>Habilitar el enrutamiento mediante atributos

Para habilitar el enrutamiento mediante atributos, llame a **MapHttpAttributeRoutes** durante la configuración. Este método de extensión se define en el **System.Web.Http.HttpConfigurationExtensions** clase.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

Enrutamiento mediante atributos se puede combinar con [basado en convenciones](routing-in-aspnet-web-api.md) enrutamiento. Para definir las rutas basado en convenciones, llame a la **MapHttpRoute** método.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

Para obtener más información sobre la configuración de Web API, consulte [configuración de ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a>Nota: Migración de Web API 1

Antes de Web API 2, las plantillas de proyecto Web API generan código similar al siguiente:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

Si está habilitado el enrutamiento mediante atributos, este código producirá una excepción. Si actualiza un proyecto de API Web existente para usar el enrutamiento mediante atributos, asegúrese de actualizar este código de configuración a la siguiente:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> Para obtener más información, consulte [configurar Web API con hospedaje de ASP.NET](../advanced/configuring-aspnet-web-api.md#webhost).


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a>Agregar atributos de ruta

Este es un ejemplo de una ruta definida mediante un atributo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

La cadena &quot;clientes / {customerId} / pedidos&quot; es la plantilla URI para la ruta. API Web intenta hacer coincidir el URI de solicitud a la plantilla. En este ejemplo, "clientes" y "orders" son los segmentos literales y "{customerId}" es un parámetro de variable. Los URI siguientes coincidirían con esta plantilla:

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

Puede restringir la búsqueda de coincidencias utilizando [restricciones](#constraints), que se describen más adelante en este tema.

Tenga en cuenta que el &quot;{customerId}&quot; parámetro en la plantilla de ruta coincide con el nombre de la *customerId* parámetro del método. Cuando la API Web invoca la acción de controlador, intenta enlazar los parámetros de ruta. Por ejemplo, si el URI es `http://example.com/customers/1/orders`, API Web intenta enlazar el valor "1" para el *customerId* parámetro en la acción.

Una plantilla URI puede tener varios parámetros:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

Los métodos de controlador que no tienen un atributo de ruta usan enrutamiento basado en convenciones. De este modo, puede combinar ambos tipos de enrutamiento en el mismo proyecto.

## <a name="http-methods"></a>Métodos HTTP

API Web también selecciona las acciones según el método HTTP de la solicitud (GET, POST, etcetera). De forma predeterminada, la API Web busca una coincidencia entre mayúsculas y minúsculas, con el inicio del nombre del método de controlador. Por ejemplo, un método de controlador denominado `PutCustomers` coincide con una solicitud HTTP PUT.

Puede invalidar esta convención decorando el método con cualquiera de los siguientes atributos:

- **[HttpDelete]**
- **[HttpGet]**
- **[HttpHead]**
- **[HttpOptions]**
- **[HttpPatch]**
- **[HttpPost]**
- **[HttpPut]**

El ejemplo siguiente asigna el método CreateBook a las solicitudes HTTP POST.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

Para todos los demás métodos HTTP, incluidos los métodos no estándares, usan el **AcceptVerbs** atributo, que toma una lista de métodos HTTP.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a>Prefijos de rutas

A menudo, las rutas en un controlador todas comienzan por el mismo prefijo. Por ejemplo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

Puede establecer un prefijo común para todo un controlador mediante la **[RoutePrefix]** atributo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

Use una tilde (~) en el atributo del método para invalidar el prefijo de ruta:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

El prefijo de ruta puede incluir parámetros:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a>Restricciones de ruta

Las restricciones de ruta le permiten restringir cómo coinciden los parámetros en la plantilla de ruta. La sintaxis general es &quot;{: restricción de parámetro}&quot;. Por ejemplo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

En este caso, la primera ruta sólo se puede seleccionar si el &quot;id&quot; segmento del URI es un entero. En caso contrario, se elegirá la segunda ruta.

En la tabla siguiente se enumera las restricciones que se admiten.

| Restricción | Descripción | Ejemplo |
| --- | --- | --- |
| alfa | Coincidencias de mayúscula o minúscula de caracteres del alfabeto latino (, a-z, A-z) | {x: alfa} |
| bool | Coincide con un valor booleano. | {x: bool} |
| datetime | Coincide con un **DateTime** valor. | {x: datetime} |
| decimal | Coincide con un valor decimal. | {x: decimal} |
| double | Coincide con un valor de punto flotante de 64 bits. | {x: double} |
| float | Coincide con un valor de punto flotante de 32 bits. | {x: float} |
| guid | Coincide con un valor GUID. | {x: guid} |
| int | Coincide con un valor entero de 32 bits. | {x: int} |
| longitud | Busca una cadena con la longitud especificada o en un intervalo especificado de longitudes. | {x: length(6)} {x: length(1,20)} |
| long | Coincide con un valor entero de 64 bits. | {x: long} |
| max | Coincide con un entero con un valor máximo. | {x: max(10)} |
| MaxLength | Busca una cadena con una longitud máxima. | {x: maxlength(10)} |
| min | Coincide con un entero con un valor mínimo. | {x: min(10)} |
| minLength | Busca una cadena con una longitud mínima. | {x: minlength(10)} |
| range | Coincide con un número entero en un intervalo de valores. | {x: range(10,50)} |
| regex | Coincide con una expresión regular. | {x: regex(^\d{3}-\d{3}-\d{4}$)} |

Tenga en cuenta que algunas de las restricciones, como &quot;min&quot;, toman argumentos entre paréntesis. Puede aplicar varias restricciones a un parámetro, separado por puntos.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a>Restricciones de ruta personalizados

Puede crear las restricciones de ruta personalizada implementando la **IHttpRouteConstraint** interfaz. Por ejemplo, la siguiente restricción restringe un parámetro a un valor entero distinto de cero.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

El código siguiente muestra cómo registrar la restricción:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

Ahora puede aplicar la restricción en sus rutas:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

También puede reemplazar toda la **DefaultInlineConstraintResolver** clase implementando la **IInlineConstraintResolver** interfaz. Si lo hace, reemplazará todas las restricciones integradas, a menos que la implementación de **IInlineConstraintResolver** agrega específicamente.

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a>Los parámetros de URI opcional y los valores predeterminados

Puede hacer que un parámetro de URI opcional mediante la adición de un signo de interrogación para el parámetro de ruta. Si un parámetro de ruta es opcional, debe definir un valor predeterminado para el parámetro de método.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

En este ejemplo, `/api/books/locale/1033` y `/api/books/locale` devuelven el mismo recurso.

Como alternativa, puede especificar un valor predeterminado dentro de la plantilla de ruta, como sigue:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

Esto es prácticamente el mismo que el ejemplo anterior, pero hay una ligera diferencia de comportamiento cuando se aplica el valor predeterminado.

- En el primer ejemplo ("{lcid?}"), el valor predeterminado de 1033 se asigna directamente al parámetro de método, por lo que el parámetro tendrá este valor exacto.
- En el segundo ejemplo ("{lcid = 1033}"), el valor predeterminado de "1033" recorre el proceso de enlace de modelos. El enlazador de modelos de forma predeterminada, se convertirá "1033" al valor numérico 1033. Sin embargo, es posible insertar un enlazador de modelos personalizado, que podría hacer algo diferente.

(En la mayoría de los casos, a menos que tenga los enlazadores de modelos personalizados en la canalización, los dos formularios será equivalente.)

<a id="route-names"></a>
## <a name="route-names"></a>Nombres de ruta

En la API Web, cada ruta tiene un nombre. Los nombres de ruta son útiles para generar vínculos, por lo que puede incluir un vínculo en una respuesta HTTP.

Para especificar el nombre de ruta, establezca la **nombre** propiedad del atributo. El ejemplo siguiente muestra cómo establecer el nombre de ruta y también cómo usar el nombre de ruta cuando se genera un vínculo.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a>Orden de la ruta

Cuando el marco de trabajo intenta hacer coincidir un URI con una ruta, evalúa las rutas en un orden concreto. Para especificar el orden, establezca el **RouteOrder** propiedad del atributo de ruta. Los valores más bajos se evalúan primero. El valor de orden predeterminado es cero.

Aquí es cómo se determina el orden total:

1. Comparar el **RouteOrder** propiedad del atributo de ruta.
2. Examine cada segmento URI de la plantilla de ruta. Para cada segmento, pedido como sigue:

    1. Segmentos literales.
    2. Parámetros de ruta con restricciones.
    3. Parámetros de ruta sin restricciones.
    4. Segmentos de parámetro de carácter comodín con restricciones.
    5. Segmentos de carácter comodín parámetro sin restricciones.
3. En el caso de empate, las rutas se ordenan por una comparación de cadenas ordinales entre mayúsculas y minúsculas ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) de la plantilla de ruta.

A continuación se muestra un ejemplo. Supongamos que define el controlador siguiente:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

Estas rutas se ordenan como sigue.

1. pedidos y detalles
2. pedidos / {id}
3. pedidos / {customerName}
4. pedidos / {\*fecha}
5. pedidos / pendiente

Tenga en cuenta que "Detalles" es un segmento literal y aparece antes que "{id}", pero "pendiente" aparece por última vez porque el **RouteOrder** propiedad es 1. (En este ejemplo se supone que hay es ningún cliente denominado "Detalles" o "pendiente". En general, intente evitar rutas ambiguas. En este ejemplo, una plantilla de ruta mejor para `GetByCustomer` es "customers / {customerName}")
