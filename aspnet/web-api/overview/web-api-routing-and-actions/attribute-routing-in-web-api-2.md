---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Atributo de enrutamiento en ASP.NET Web API 2 | Documentos de Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: ad44ee525601f308498967159e964aa41a2ce00c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="attribute-routing-in-aspnet-web-api-2"></a>Ruta de atributo en ASP.NET Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

*Enrutamiento* es la API Web coincide con un URI para una acción. Web API 2 es compatible con un nuevo tipo de enrutamiento, denominado *atributo enrutamiento*. Como su nombre indica, enrutamiento de atributo usa atributos para definir las rutas. Ruta de atributo proporciona mayor control sobre los URI de la API web. Por ejemplo, puede crear fácilmente los identificadores URI que describen las jerarquías de recursos.

El estilo anterior de enrutamiento, denominado basada en convenciones de enrutamiento, sigue siendo totalmente compatible. De hecho, puede combinar ambas técnicas en el mismo proyecto.

En este tema se muestra cómo habilitar el enrutamiento de atributo y describe las distintas opciones para el enrutamiento de atributo. Para ver un tutorial to-end que utiliza el enrutamiento de atributo, vea [crear una API de REST con el atributo de enrutamiento de Web API 2](create-a-rest-api-with-attribute-routing.md).


## <a name="prerequisites"></a>Requisitos previos

[Visual Studio de 2017](https://www.visualstudio.com/vs/) Community, Professional o Enterprise Edition

Como alternativa, use el Administrador de paquetes de NuGet para instalar los paquetes necesarios. Desde el **herramientas** menú en Visual Studio, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**. Escriba el siguiente comando en la ventana de la consola de administrador de paquetes:

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a>¿Por qué atributo enrutamiento?

La primera versión de API Web que usan *basada en convenciones* enrutamiento. En ese tipo de enrutamiento, puede definir uno o más plantillas de ruta, que básicamente son cadenas de parámetros. Cuando el marco de trabajo recibe una solicitud, coincide con el URI en la plantilla de ruta. (Para obtener más información sobre el enrutamiento basado en convenciones, vea [enrutamiento de ASP.NET Web API](routing-in-aspnet-web-api.md).

Una ventaja de enrutamiento basado en la convención es que las plantillas se definen en un único lugar y las reglas de enrutamiento se aplican de forma coherente en todos los controladores. Por desgracia, enrutamiento basado en la convención hace más difícil admitir determinados patrones URI que son comunes en las API de REST. Por ejemplo, los recursos a menudo contienen recursos secundarios: los clientes tienen pedidos, las películas tienen actores, tienen los autores de libros y así sucesivamente. Es natural para crear a URI que refleja estas relaciones:

`/customers/1/orders`

Este tipo de URI es difícil crear mediante el enrutamiento basado en la convención. Aunque es posible, los resultados no admiten una ampliación también si tiene muchos controladores o tipos de recursos.

Con el enrutamiento de atributo, es trivial para definir una ruta para este URI. Simplemente agregue un atributo a la acción de controlador:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

Estos son algunos otros patrones de ese atributo enrutamiento hace fácil.

**Control de versiones de API**

En este ejemplo, "/ api/v1/products" sería enrutado a un controlador diferente que "/ v2/api/productos".

`/api/v1/products`  
`/api/v2/products`

**Segmentos URI sobrecargados**

En este ejemplo, "1" es un número de pedido, pero "pendiente" se asigna a una colección.

`/orders/1`  
`/orders/pending`

**Varios tipos de parámetros**

En este ejemplo, "1" es un número de pedido, pero "2013/06/16" especifica una fecha.

`/orders/1`  
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a>Habilitar el enrutamiento de atributo

Para habilitar el enrutamiento de atributo, llame a **MapHttpAttributeRoutes** durante la configuración. Este método de extensión se define en el **System.Web.Http.HttpConfigurationExtensions** clase.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

Ruta de atributo se puede combinar con [basada en convenciones](routing-in-aspnet-web-api.md) enrutamiento. Para definir las rutas basada en convenciones, llame a la **MapHttpRoute** método.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

Para obtener más información sobre la configuración de Web API, consulte [configurar ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a>Nota: La migración desde la API Web 1

Antes de la API Web 2, las plantillas de proyecto de Web API generan código similar al siguiente:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

Si está habilitado el enrutamiento de atributo, este código producirá una excepción. Si actualiza un proyecto de API Web existente para usar el enrutamiento de atributo, asegúrese de actualizar este código de configuración al siguiente:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> Para obtener más información, consulte [configuración de Web API con hospedaje de ASP.NET](../advanced/configuring-aspnet-web-api.md#webhost).


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a>Agregar atributos de ruta

Este es un ejemplo de una ruta que se definen mediante un atributo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

La cadena &quot;clientes / {customerId} / ordena&quot; es la plantilla URI para la ruta. API Web intenta hacer coincidir el URI de solicitud a la plantilla. En este ejemplo, "customers" y "orders" son literales segmentos y "{customerId}" es un parámetro de la variable. Los URI siguientes coincidiría con esta plantilla:

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

Puede restringir la búsqueda de coincidencias utilizando [restricciones](#constraints), que se describen más adelante en este tema.

Tenga en cuenta que la &quot;{customerId}&quot; parámetro en la plantilla de ruta coincide con el nombre de la *customerId* parámetro del método. Cuando la API Web, se invoca la acción de controlador, intenta enlazar los parámetros de ruta. Por ejemplo, si el identificador URI es `http://example.com/customers/1/orders`, API Web intenta enlazar el valor "1" para el *customerId* parámetro en la acción.

Una plantilla URI puede tener varios parámetros:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

Los métodos de controlador que no tiene un atributo de ruta usan enrutamiento basado en la convención. De este modo, puede combinar ambos tipos de enrutamiento en el mismo proyecto.

## <a name="http-methods"></a>Métodos HTTP

API Web también selecciona acciones basadas en el método HTTP de la solicitud (GET, POST, etcetera). De forma predeterminada, la API Web busca una coincidencia entre mayúsculas y minúsculas con el inicio del nombre del método de controlador. Por ejemplo, un método de controlador denominado `PutCustomers` coincide con una solicitud PUT de HTTP.

Puede invalidar esta convención decorando lo (método) con cualquiera de los siguientes atributos:

- **[HttpDelete]**
- **[HttpGet]**
- **[HttpHead]**
- **[HttpOptions]**
- **[HttpPatch]**
- **[HttpPost]**
- **[HttpPut]**

En el ejemplo siguiente se asigna el método CreateBook a solicitudes HTTP POST.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

Para todos los demás métodos HTTP, incluidos los métodos no estándares, utilicen la **AcceptVerbs** atributo, que toma una lista de métodos HTTP.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a>Prefijos de rutas

A menudo, las rutas en un controlador todas comienzan por el mismo prefijo. Por ejemplo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

Puede establecer un prefijo común para un controlador de todo mediante el **[RoutePrefix]** atributo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

Usar una tilde (~) en el atributo de método para invalidar el prefijo de ruta:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

El prefijo de ruta puede incluir parámetros:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a>Restricciones de la ruta

Restricciones de la ruta le permiten restringir cómo coinciden los parámetros en la plantilla de ruta. La sintaxis general es &quot;{parámetro: restricción}&quot;. Por ejemplo:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

En este caso, la primera ruta sólo se puede seleccionar si el &quot;identificador&quot; segmento del URI es un entero. En caso contrario, se elegirá la segunda ruta.

En la tabla siguiente enumera las restricciones que se admiten.

| Restricción | Descripción | Ejemplo |
| --- | --- | --- |
| Alfa | Coincidencias en mayúsculas o minúsculas caracteres del alfabeto latino (a – z, a Z) | {x: alfa} |
| bool | Coincide con un valor booleano. | {x: bool} |
| datetime | Coincide con un **DateTime** valor. | {x: fecha y hora} |
| decimal | Coincide con un valor decimal. | {x: decimal} |
| double | Coincide con un valor de punto flotante de 64 bits. | {x: double} |
| flotante | Coincide con un valor de punto flotante de 32 bits. | {x: float} |
| guid | Coincide con un valor GUID. | {x: guid} |
| int | Coincide con un valor entero de 32 bits. | {x: int} |
| longitud | Coincide con una cadena con la longitud especificada o dentro de un intervalo especificado de longitudes. | {x: length(6)} {x: length(1,20)} |
| long | Coincide con un valor entero de 64 bits. | {x: long} |
| max | Devuelve un entero con un valor máximo. | {x: max(10)} |
| MaxLength | Coincide con una cadena con una longitud máxima. | {x: maxlength(10)} |
| min | Devuelve un entero con un valor mínimo. | {x: min(10)} |
| minLength | Coincide con una cadena con una longitud mínima. | {x: minlength(10)} |
| range | Devuelve un número entero dentro de un intervalo de valores. | {x: range(10,50)} |
| regex | Coincide con una expresión regular. | {x: regex(^\d{3}-\d{3}-\d{4}$)} |

Observe que algunas de las restricciones, como &quot;min&quot;, toman argumentos entre paréntesis. Puede aplicar varias restricciones a un parámetro, separado por un coma.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a>Restricciones de la ruta personalizada

Puede crear restricciones de la ruta personalizada mediante la implementación de la **IHttpRouteConstraint** interfaz. Por ejemplo, la restricción siguiente restringe un parámetro a un valor entero distinto de cero.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

El código siguiente muestra cómo registrar la restricción:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

Ahora puede aplicar la restricción en sus rutas:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

También puede reemplazar toda la **DefaultInlineConstraintResolver** clase implementando la **IInlineConstraintResolver** interfaz. Si lo hace, reemplazará todas las restricciones integradas, a menos que la implementación de **IInlineConstraintResolver** agrega específicamente.

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a>Parámetros de URI opcional y los valores predeterminados

Puede hacer que un parámetro URI opcional mediante la adición de un signo de interrogación para el parámetro de ruta. Si un parámetro de ruta es opcional, debe definir un valor predeterminado para el parámetro de método.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

En este ejemplo, `/api/books/locale/1033` y `/api/books/locale` devuelven el mismo recurso.

Como alternativa, puede especificar un valor predeterminado dentro de la plantilla de ruta, como se indica a continuación:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

Esto es prácticamente el mismo que el ejemplo anterior, pero hay una ligera diferencia de comportamiento cuando se aplica el valor predeterminado.

- En el primer ejemplo ("{lcid?}"), el valor predeterminado de 1033 se asigna directamente al parámetro de método, por lo que el parámetro tendrán este valor exacto.
- En el segundo ejemplo ("{lcid = 1033}"), el valor predeterminado de "1033" lleva a cabo el proceso de enlace de modelos. El enlazador de modelos predeterminado convertirá "1033" en el valor numérico 1033. Sin embargo, puede conectar un enlazador de modelos personalizado, que puede hacer algo diferente.

(En la mayoría de los casos, a menos que tenga los enlazadores de modelos personalizados en la canalización, los dos formularios será equivalente.)

<a id="route-names"></a>
## <a name="route-names"></a>Nombres de ruta

En la API de Web, cada ruta tiene un nombre. Los nombres de ruta son útiles para generar vínculos, por lo que puede incluir un vínculo en una respuesta HTTP.

Para especificar el nombre de ruta, establezca la **nombre** propiedad del atributo. En el ejemplo siguiente se muestra cómo establecer el nombre de ruta y también cómo usar el nombre de ruta cuando se genera un vínculo.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a>Orden de la ruta

Cuando el marco de trabajo intenta hacer coincidir un URI con una ruta, evalúa las rutas en un orden concreto. Para especificar el orden, establezca el **RouteOrder** propiedad en el atributo de ruta. Los valores más bajos se evalúan primero. El valor de orden predeterminado es cero.

Aquí es cómo se determina el orden total:

1. Comparar el **RouteOrder** propiedad del atributo de ruta.
2. Mire cada segmento URI de la plantilla de ruta. Para cada segmento, ordenar como sigue: 

    1. Segmentos de literales.
    2. Parámetros de ruta con restricciones.
    3. Parámetros de ruta sin restricciones.
    4. Segmentos de parámetro de carácter comodín con restricciones.
    5. Segmentos de parámetro de carácter comodín sin restricciones.
3. En el caso de empate, las rutas se ordenan por una comparación de cadenas ordinales entre mayúsculas y minúsculas ([OrdinalIgnoreCase](https://msdn.microsoft.com/en-us/library/system.stringcomparer.ordinalignorecase.aspx)) de la plantilla de ruta.

A continuación se muestra un ejemplo. Supongamos que define el siguiente controlador:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

Estas rutas se ordenan como se indica a continuación.

1. pedidos y detalles
2. pedidos / {id}
3. pedidos / {NombreCliente}
4. pedidos / {\*fecha}
5. pedidos / pendiente

Observe que "Detalles" es un segmento literal y aparece antes que "{id}", pero "pendiente" aparece última porque la **RouteOrder** propiedad es 1. (En este ejemplo se supone que hay es ningún cliente denominada "Detalles" o "pendiente". En general, intente evitar las rutas ambiguas. En este ejemplo, una plantilla de ruta mejor para `GetByCustomer` es "clientes / {NombreCliente}")
