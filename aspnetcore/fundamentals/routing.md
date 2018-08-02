---
title: Enrutamiento en ASP.NET Core
author: ardalis
description: Descubra la manera en que la funcionalidad de enrutamiento de ASP.NET Core se encarga de asignar una solicitud entrante a un controlador de ruta.
ms.author: riande
ms.date: 07/25/2018
uid: fundamentals/routing
ms.openlocfilehash: 19265ac4d19915462c50628061600b1fde04694b
ms.sourcegitcommit: c8e62aa766641aa55105f7db79cdf2b27a6e5977
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2018
ms.locfileid: "39254888"
---
# <a name="routing-in-aspnet-core"></a>Enrutamiento en ASP.NET Core

Por [Ryan Nowak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/) y [Rick Anderson](https://twitter.com/RickAndMSFT)

La funcionalidad de enrutamiento de ASP.NET Core se encarga de asignar una solicitud entrante a un controlador de ruta. Las rutas se definen en la aplicación ASP.NET y se configuran cuando se inicia la aplicación. Una ruta puede extraer opcionalmente valores de la dirección URL contenida en la solicitud, que se pueden usar para procesar las solicitudes. Con la información de ruta de la aplicación ASP.NET, la funcionalidad de enrutamiento también puede generar direcciones URL que se asignan a controladores de ruta. Por tanto, el enrutamiento puede buscar un controlador de ruta basado en una dirección URL o la dirección URL correspondiente a un controlador de ruta determinado en función de la información del controlador de ruta.

>[!IMPORTANT]
> En este documento se describe el enrutamiento de nivel bajo de ASP.NET Core. Para más información sobre el enrutamiento de ASP.NET Core MVC, vea [Enrutamiento a las acciones de controlador](../mvc/controllers/routing.md).

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="routing-basics"></a>Fundamentos del enrutamiento

El enrutamiento usa *rutas* (implementaciones de [IRouter](/dotnet/api/microsoft.aspnetcore.routing.irouter)) para:

* asignar las solicitudes entrantes a *controladores de ruta*

* generar direcciones URL que se usan en las respuestas

Por lo general, una aplicación tiene una sola colección de rutas. Cuando llega una solicitud, la colección de rutas se procesa en orden. La solicitud entrante busca una ruta que coincida con la dirección URL de la solicitud. Para ello, llama al método `RouteAsync` en cada ruta disponible de la colección de rutas. En cambio, una respuesta puede usar el enrutamiento para generar direcciones URL (por ejemplo, para el redireccionamiento o los vínculos) en función de la información de ruta. De este modo, evita tener que codificar de forma rígida las direcciones URL, lo que facilita el mantenimiento.

La clase `RouterMiddleware` conecta el enrutamiento a la canalización de [software intermedio](xref:fundamentals/middleware/index). [ASP.NET Core MVC](xref:mvc/overview) agrega enrutamiento a la canalización de software intermedio como parte de su configuración. Para obtener información sobre el uso del enrutamiento como componente independiente, vea [Uso de software intermedio de enrutamiento](#using-routing-middleware).

<a name="url-matching-ref"></a>

### <a name="url-matching"></a>Coincidencia de dirección URL

La coincidencia de dirección URL es el proceso por el cual el enrutamiento envía una solicitud entrante a un *controlador*. Este proceso suele basarse en datos de la ruta de dirección URL, pero se puede ampliar para tener en cuenta los datos de la solicitud. La capacidad de enviar solicitudes a controladores independientes es clave para escalar el tamaño y la complejidad de una aplicación.

Las solicitudes entrantes especifican la clase `RouterMiddleware`, que llama al método `RouteAsync` en cada ruta de la secuencia. La instancia de `IRouter` decide si *controla* la solicitud mediante el establecimiento de `RouteContext.Handler` en un `RequestDelegate` no nulo. Si una ruta establece un controlador para la solicitud, el procesamiento de rutas se detiene y se invoca el controlador para procesar la solicitud. Si se prueban todas las rutas y no se encuentra ningún controlador para la solicitud, el software intermedio llama al *siguiente* y se invoca el siguiente software intermedio de la canalización de solicitudes.

La entrada principal a `RouteAsync` es el `RouteContext.HttpContext` asociado a la solicitud actual. `RouteContext.Handler` y `RouteContext.RouteData` son salidas que se establecerán cuando una ruta coincida.

Una coincidencia durante `RouteAsync` también establecerá las propiedades de `RouteContext.RouteData` en los valores adecuados en función del procesamiento de solicitudes realizado hasta el momento. Si una ruta coincide con una solicitud, `RouteContext.RouteData` contendrá información de estado importante sobre el *resultado*.

`RouteData.Values` es un diccionario con los *valores de ruta* generados desde la ruta. Estos valores suelen determinarse mediante la conversión en tokens de la dirección URL, y pueden usarse para aceptar la entrada del usuario o para tomar otras decisiones sobre el envío dentro de la aplicación.

`RouteData.DataTokens` es un contenedor de propiedades de datos adicionales relacionados con la ruta coincidente. Se proporcionan `DataTokens` para permitir la asociación de datos de estado con cada ruta, de modo que la aplicación pueda tomar decisiones más adelante en función de las rutas que han coincidido. Estos valores los define el desarrollador y **no** afectan de ninguna manera al comportamiento del enrutamiento. Además, los valores que se guardan provisionalmente en tokens de datos pueden ser de cualquier tipo, a diferencia de los valores de ruta, que deben poder convertirse fácilmente en cadenas y a partir de estas.

`RouteData.Routers` es una lista de las rutas que han participado en encontrar una coincidencia correcta con la solicitud. Las rutas se pueden anidar unas dentro de otras y la propiedad `Routers` refleja la ruta de acceso del árbol lógico de rutas que han tenido como resultado una coincidencia. Por lo general, el primer elemento de `Routers` es la colección de rutas y se debe usar para la generación de direcciones URL. El último elemento de `Routers` es el controlador de ruta que ha coincidido.

### <a name="url-generation"></a>Generación de dirección URL

La generación de dirección URL es el proceso por el cual el enrutamiento puede crear una ruta de dirección URL basada en un conjunto de valores de ruta. Esto permite una separación lógica entre los controladores y las direcciones URL que tienen acceso a ellos.

La generación de dirección URL sigue un proceso iterativo similar, pero se inicia cuando el código de usuario o de marco de trabajo llama al método `GetVirtualPath` de la colección de rutas. Se llamará en secuencia al método `GetVirtualPath` de cada *ruta* hasta que se devuelva un valor `VirtualPathData` no nulo.

La principal entradas de `GetVirtualPath` son:

* `VirtualPathContext.HttpContext`

* `VirtualPathContext.Values`

* `VirtualPathContext.AmbientValues`

Las rutas usan principalmente los valores de ruta proporcionados por `Values` y `AmbientValues` para decidir dónde se puede generar una dirección URL y qué valores se deben incluir. `AmbientValues` son el conjunto de valores de ruta producidos cuando la solicitud actual coincide con el sistema de enrutamiento. En cambio, `Values` son los valores de ruta que especifican cómo se genera la dirección URL deseada para la operación actual. Se proporciona `HttpContext` por si una ruta necesita obtener servicios o datos adicionales asociados con el contexto actual.

Sugerencia: Considere `Values` como un conjunto de invalidaciones de `AmbientValues`. La generación de dirección URL intenta reutilizar los valores de ruta de la solicitud actual para que sea más fácil generar direcciones URL para los vínculos con la misma ruta o valores de ruta.

La salida de `GetVirtualPath` es `VirtualPathData`. `VirtualPathData` es un valor paralelo de `RouteData`; contiene el valor `VirtualPath` de la dirección URL de salida y algunas propiedades más que la ruta debe establecer.

La propiedad `VirtualPathData.VirtualPath` contiene la *ruta de acceso virtual* generada por la ruta. Es posible que deba procesar aún más la ruta de acceso, según sus necesidades. Por ejemplo, si quiere representar la dirección URL generada en HTML debe anteponer la ruta de acceso base de la aplicación.

`VirtualPathData.Router` es una referencia a la ruta que ha generado correctamente la dirección URL.

La propiedad `VirtualPathData.DataTokens` es un diccionario de datos adicionales relacionados con la ruta que ha generado la dirección URL. Se trata del valor paralelo de `RouteData.DataTokens`.

### <a name="creating-routes"></a>Creación de rutas

El enrutamiento proporciona la clase `Route` como implementación estándar de `IRouter`. `Route` usa la sintaxis de *plantilla de ruta* para definir los patrones que se harán coincidir con la ruta de dirección URL cuando se llame a `RouteAsync`. `Route` usará la misma plantilla de ruta para generar una dirección URL cuando se llame a `GetVirtualPath`.

La mayoría de las aplicaciones creará rutas mediante una llamada a `MapRoute` o a uno de los métodos de extensión similares definidos en `IRouteBuilder`. Todos estos métodos crearán una instancia de `Route` y la agregarán a la colección de rutas.

Nota: `MapRoute` no toma ningún parámetro de controlador de ruta, solo agrega las rutas que procesará `DefaultHandler`. Dado que el controlador predeterminado es `IRouter`, es posible que decida no controlar la solicitud. Por ejemplo, ASP.NET MVC suele configurarse como un controlador predeterminado que solo controla las solicitudes que coinciden con un controlador y una acción disponibles. Para más información sobre el enrutamiento a MVC, vea [Enrutamiento a las acciones de controlador](../mvc/controllers/routing.md).

Este es un ejemplo de una llamada a `MapRoute` usada por una definición de ruta típica de ASP.NET MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Esta plantilla coincidirá con una ruta de dirección URL como `/Products/Details/17` y extraerá los valores de ruta `{ controller = Products, action = Details, id = 17 }`. Los valores de ruta se determinan al dividir la ruta de dirección URL en segmentos y hacer coincidir cada segmento con el nombre del *parámetro de ruta* de la plantilla de ruta. Los parámetros de ruta tienen un nombre asignado. Para definirlos, el nombre del parámetro se incluye entre llaves `{ }`.

La plantilla anterior también podría coincidir con la ruta de dirección URL `/` y generaría los valores `{ controller = Home, action = Index }`. Esto se debe a que los parámetros de ruta `{controller}` y `{action}` tienen valores predeterminados y el parámetro de ruta `id` es opcional. Un signo igual `=` seguido de un valor después del nombre del parámetro de ruta define un valor predeterminado para el parámetro. Un signo de interrogación `?` después del nombre del parámetro de ruta define el parámetro como opcional. Los parámetros de ruta con un valor predeterminado *siempre* generan un valor de ruta cuando la ruta coincide, mientras que los parámetros opcionales no generarán un valor de ruta si no hay ningún segmento de ruta de dirección URL correspondiente.

Vea en [Referencia de plantilla de ruta](#route-template-reference) una descripción detallada de las características y la sintaxis de la plantilla de ruta.

En este ejemplo se incluye una *restricción de ruta*:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Esta plantilla coincidirá con una ruta de dirección URL como `/Products/Details/17`, pero no con `/Products/Details/Apples`. La definición de parámetro de ruta `{id:int}` define una *restricción de ruta* para el parámetro de ruta `id`. Las restricciones de ruta implementan `IRouteConstraint` e inspeccionan los valores de ruta para comprobarlos. En este ejemplo, el valor de ruta `id` debe poder convertirse en un entero. Vea en [Referencia de restricción de ruta](#route-constraint-reference) una explicación más detallada de las restricciones de ruta que se proporcionan con el marco de trabajo.

Las sobrecargas adicionales de `MapRoute` aceptan valores para `constraints`, `dataTokens` y `defaults`. Estos parámetros adicionales de `MapRoute` se definen como un tipo `object`. Estos parámetros suelen usarse para pasar un objeto de tipo anónimo, donde los nombres de propiedad del tipo anónimo coinciden con los nombres de los parámetros de ruta.

En los dos ejemplos siguientes se crean rutas equivalentes:

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Sugerencia: La sintaxis insertada para definir las restricciones y los valores predeterminados puede ser más conveniente para rutas simples, pero hay algunas características como los tokens de datos que no son compatibles con esta sintaxis.

En este ejemplo se muestran otras características:

```csharp
routes.MapRoute(
  name: "blog",
  template: "Blog/{*article}",
  defaults: new { controller = "Blog", action = "ReadArticle" });
```

Esta plantilla coincidirá con una ruta de dirección URL como `/Blog/All-About-Routing/Introduction` y extraerá los valores `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. La ruta genera los valores de ruta predeterminados para `controller` y `action`, incluso si no hay parámetros de ruta correspondientes en la plantilla. Los valores predeterminados pueden especificarse en la plantilla de ruta. El parámetro de ruta `article` se define como un *comodín* por la aparición de un asterisco `*` antes del nombre del parámetro de ruta. Los parámetros de ruta comodín capturan el resto de la ruta de dirección URL y también pueden coincidir con la cadena vacía.

En este ejemplo se agregan restricciones de ruta y tokens de datos:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Esta plantilla coincide con una ruta de dirección URL como `/en-US/Products/5` y extrae los valores `{ controller = Products, action = Details, id = 5 }` y los tokens de datos `{ locale = en-US }`.

![Tokens de Windows de variables locales](routing/_static/tokens.png)

<a name="id1"></a>

### <a name="url-generation"></a>Generación de dirección URL

La clase `Route` también puede llevar a cabo la generación de dirección URL mediante la combinación de un conjunto de valores de ruta con su plantilla de ruta. Se trata lógicamente del proceso inverso de hacer coincidir la ruta de dirección URL.

Sugerencia: Para entender mejor la generación de dirección URL, imagine qué dirección URL quiere generar y, después, piense cómo coincidiría una plantilla de ruta con esa dirección URL. ¿Qué valores se generarían? Este es un equivalente aproximado de cómo funciona la generación de dirección URL en la clase `Route`.

En este ejemplo se usa una ruta de estilo básica de ASP.NET MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Con los valores de ruta `{ controller = Products, action = List }`, esta ruta generará la dirección URL `/Products/List`. Los valores de ruta se sustituyen por los parámetros de ruta correspondientes para formar la ruta de dirección URL. Dado que `id` es un parámetro de ruta opcional, no supone ningún problema que no tenga un valor.

Con los valores de ruta `{ controller = Home, action = Index }`, esta ruta generará la dirección URL `/`. Los valores de ruta proporcionados coinciden con los valores predeterminados, por lo que los segmentos correspondientes con esos valores se pueden omitir sin ningún riesgo. Tenga en cuenta que ambas direcciones URL generadas harían un recorrido de ida y vuelta con esta definición de ruta y generarían los mismos valores de ruta que se han usado para generar la dirección URL.

Sugerencia: Las aplicaciones con ASP.NET MVC deben usar `UrlHelper` para generar direcciones URL en lugar de llamar al enrutamiento directamente.

Para obtener más detalles sobre el proceso de generación de dirección URL, vea [Referencia de generación de dirección URL](#url-generation-reference).

## <a name="using-routing-middleware"></a>Uso de software intermedio de enrutamiento

Agregue el paquete NuGet "Microsoft.AspNetCore.Routing".

Agregue enrutamiento al contenedor de servicios en *Startup.cs*:

[!code-csharp[](../fundamentals/routing/sample/RoutingSample/Startup.cs?highlight=3&start=11&end=14)]

Las rutas deben configurarse en el método `Configure` en la clase `Startup`. En el ejemplo siguiente se usan estas API:

* `RouteBuilder`
* `Build`
* `MapGet` coincide solamente con solicitudes HTTP GET
* `UseRouter`

```csharp
public void Configure(IApplicationBuilder app, ILoggerFactory loggerFactory)
{
    var trackPackageRouteHandler = new RouteHandler(context =>
    {
        var routeValues = context.GetRouteData().Values;
        return context.Response.WriteAsync(
            $"Hello! Route values: {string.Join(", ", routeValues)}");
    });

    var routeBuilder = new RouteBuilder(app, trackPackageRouteHandler);

    routeBuilder.MapRoute(
        "Track Package Route",
        "package/{operation:regex(^(track|create|detonate)$)}/{id:int}");

    routeBuilder.MapGet("hello/{name}", context =>
    {
        var name = context.GetRouteValue("name");
        // This is the route handler when HTTP GET "hello/<anything>"  matches
        // To match HTTP GET "hello/<anything>/<anything>,
        // use routeBuilder.MapGet("hello/{*name}"
        return context.Response.WriteAsync($"Hi, {name}!");
    });

    var routes = routeBuilder.Build();
    app.UseRouter(routes);
}
```

En la tabla siguiente se muestran las respuestas con los URI especificados.

| Identificador URI | Respuesta  |
| ------- | -------- |
| /package/create/3  | Hello! Valores de ruta: [operation, create], [id, 3] |
| /package/track/-3  | Hello! Valores de ruta: [operation, track], [id, -3] |
| /package/track/-3/ | Hello! Valores de ruta: [operation, track], [id, -3]  |
| /package/track/ | \<Pasar explícitamente, ninguna coincidencia> |
| GET /hello/Joe | Hi, Joe! |
| POST /hello/Joe | \<Pasar explícitamente, solo coincide con HTTP GET> |
| GET /hello/Joe/Smith | \<Pasar explícitamente, ninguna coincidencia> |

Si va a configurar una única ruta, pase una instancia de `IRouter` para llamar a `app.UseRouter`. No tendrá que llamar a `RouteBuilder`.

El marco de trabajo proporciona un conjunto de métodos de extensión para crear rutas, como los siguientes:

* `MapRoute`
* `MapGet`
* `MapPost`
* `MapPut`
* `MapDelete`
* `MapVerb`

Algunos de estos métodos, como `MapGet`, requieren que se proporcione un valor `RequestDelegate`. El valor `RequestDelegate` se usará como *controlador de ruta* cuando la ruta coincida. Otros métodos de esta familia permiten configurar una canalización de software intermedio que se usará como controlador de ruta. Si el método *Map* no acepta un controlador, como `MapRoute`, usará `DefaultHandler`.

Los métodos `Map[Verb]` usan restricciones para limitar la ruta al verbo HTTP en el nombre del método. Por ejemplo, vea [MapGet](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L85-L88) y [MapVerb](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L156-L180).

## <a name="route-template-reference"></a>Referencia de plantilla de ruta

Los tokens entre llaves (`{ }`) definen *parámetros de ruta* que se enlazarán si se encuentran coincidencias con la ruta. Puede definir más de un parámetro de ruta en un segmento de ruta, pero deben estar separados por un valor literal. Por ejemplo, `{controller=Home}{action=Index}` no sería una ruta válida, ya que no hay ningún valor literal entre `{controller}` y `{action}`. Estos parámetros de ruta deben tener un nombre y, opcionalmente, atributos adicionales especificados.

El texto literal diferente de los parámetros de ruta (por ejemplo, `{id}`) y el separador de ruta `/` deben coincidir con el texto de la dirección URL. La coincidencia de texto no distingue mayúsculas de minúsculas y se basa en la representación descodificada de la ruta de las direcciones URL. Para que el delimitador de parámetro de ruta literal `{` o `}` coincida, repita el carácter (`{{` o `}}`) para usarlo como secuencia de escape.

Es necesario tener en cuenta otras consideraciones en el caso de los patrones de dirección URL que intentan capturar un nombre de archivo con una extensión de archivo opcional. Por ejemplo, cuando se usa la plantilla `files/{filename}.{ext?}`, si existen `filename` y `ext` se rellenarán ambos valores. Si solo existe `filename` en la dirección URL, la ruta coincide porque el punto final `.` es opcional. Las direcciones URL siguientes coincidirían con esta ruta:

* `/files/myFile.txt`
* `/files/myFile`

Puede usar el carácter `*` como prefijo de un parámetro de ruta para enlazar con el resto del URI; es lo que se denomina un parámetro *comodín*. Por ejemplo, `blog/{*slug}` coincidirá con cualquier URI que empiece por `/blog` y que vaya seguido por cualquier valor (que se asignaría al valor de ruta `slug`). Los parámetros comodín también pueden coincidir con una cadena vacía.

Los parámetros de ruta pueden tener *valores predeterminados*. Para designar un valor predeterminado, se especifica después del nombre de parámetro, separado por un signo `=`. Por ejemplo, `{controller=Home}` definiría `Home` como el valor predeterminado de `controller`. El valor predeterminado se usa si no hay ningún valor en la dirección URL para el parámetro. Además de los valores predeterminados, los parámetros de ruta pueden ser opcionales (para especificarlos, se anexa un signo `?` al final del nombre del parámetro, como en `id?`). La diferencia entre ser opcional y tener un valor predeterminado es que un parámetro de ruta con un valor predeterminado siempre genera un valor, mientras que un parámetro opcional tiene un valor solo cuando se proporciona uno.

Los parámetros de ruta también pueden tener restricciones, que deben coincidir con el valor de ruta enlazado desde la dirección URL. Si se agrega un carácter de dos puntos `:` y un nombre de restricción después del nombre del parámetro de ruta se especifica una *restricción insertada* en un parámetro de ruta. Si la restricción requiere argumentos, se proporcionan entre paréntesis `( )` después del nombre de restricción. Se pueden especificar varias restricciones insertadas si se anexa otro carácter de dos puntos `:` y un nombre de restricción. El nombre de restricción se pasa al servicio `IInlineConstraintResolver` para crear una instancia de `IRouteConstraint` para su uso en el procesamiento de direcciones URL. Por ejemplo, la plantilla de ruta `blog/{article:minlength(10)}` especifica la restricción `minlength` con el argumento `10`. Para obtener una descripción más detalladas de las restricciones de ruta y una lista de las restricciones proporcionadas por el marco de trabajo, vea [Referencia de restricción de ruta](#route-constraint-reference).

En la tabla siguiente se muestran algunas plantillas de ruta y su comportamiento.

| Plantilla de ruta | Dirección URL coincidente de ejemplo | Notas |
| -------- | -------- | ------- |
| hello  | /hello  | Solo coincide con la ruta de acceso única `/hello` |
| {Page=Home} | / | Coincide y establece `Page` en `Home` |
| {Page=Home}  | /Contact  | Coincide y establece `Page` en `Contact` |
| {controller}/{action}/{id?} | /Products/List | Se asigna al controlador `Products` y la acción `List` |
| {controller}/{action}/{id?} | /Products/Details/123  |  Se asigna al controlador `Products` y la acción `Details`.  `id` se establece en 123 |
| {controller=Home}/{action=Index}/{id?} | /  |  Se asigna al controlador `Home` y el método `Index`; `id` se omite |

El uso de una plantilla suele ser el método de enrutamiento más sencillo. Las restricciones y los valores predeterminados también se pueden especificar fuera de la plantilla de ruta.

Sugerencia: Habilite el [registro](xref:fundamentals/logging/index) para ver de qué forma las implementaciones de enrutamiento integradas, como `Route`, coinciden con las solicitudes.

## <a name="reserved-routing-names"></a>Nombres de enrutamientos reservados

Las siguientes palabras clave son nombres reservados y no se pueden usar como nombres de ruta o parámetros:

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Referencia de restricción de ruta

Las restricciones de ruta se ejecutan cuando un valor `Route` ha coincidido con la sintaxis de la dirección URL entrante y ha convertido la ruta de dirección URL en valores de ruta. En general, las restricciones de ruta inspeccionan el valor de ruta asociado a través de la plantilla de ruta y deciden si el valor es aceptable o no. Algunas restricciones de ruta usan datos ajenos al valor de ruta para decidir si la solicitud se puede enrutar. Por ejemplo, `HttpMethodRouteConstraint` puede aceptar o rechazar una solicitud basada en su verbo HTTP.

>[!WARNING]
> Evite el uso de restricciones para la **validación de entradas**, porque si lo hace, la entrada no válida producirá un error "404 No encontrado" en lugar de un error 400 con el mensaje de error adecuado. Las restricciones de ruta deben usarse para **eliminar la ambigüedad** entre rutas similares, no para validar las entradas de una ruta determinada.

En la tabla siguiente se muestran algunas restricciones de ruta y su comportamiento esperado.

| restricción | Ejemplo | Coincidencias de ejemplo | Notas |
| --------   | ------- | ------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789`  | Coincide con cualquier entero |
| `bool`  | `{active:bool}` | `true`, `FALSE` | Coincide con `true` o `false` (no distingue mayúsculas de minúsculas) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm`  | Coincide con un valor `DateTime` válido (en la referencia cultural invariable; vea la advertencia) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Coincide con un valor `decimal` válido (en la referencia cultural invariable; vea la advertencia) |
| `double`  | `{weight:double}` | `1.234`, `-1,001.01e8` | Coincide con un valor `double` válido (en la referencia cultural invariable; vea la advertencia) |
| `float`  | `{weight:float}` | `1.234`, `-1,001.01e8` | Coincide con un valor `float` válido (en la referencia cultural invariable; vea la advertencia) |
| `guid`  | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Coincide con un valor `Guid` válido |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Coincide con un valor `long` válido |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | La cadena debe tener al menos cuatro caracteres |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | La cadena no debe tener más de ocho caracteres |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | La cadena debe tener una longitud de exactamente 12 caracteres |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | La cadena debe tener una longitud como mínimo de ocho caracteres y como máximo de 16 |
| `min(value)` | `{age:min(18)}` | `19` | El valor entero debe ser como mínimo 18 |
| `max(value)` | `{age:max(120)}` |  `91` | El valor entero debe ser como máximo 120 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | El valor entero debe ser como mínimo 18 y máximo 120 |
| `alpha` | `{name:alpha}` | `Rick` | La cadena debe constar de uno o más caracteres alfabéticos (`a`-`z`, no distingue mayúsculas de minúsculas) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | La cadena debe coincidir con la expresión regular (vea las sugerencias sobre cómo definir una expresión regular) |
| `required`  | `{name:required}` | `Rick` |  Se usa para exigir que un valor que no es de parámetro esté presente durante la generación de dirección URL |

Es posible aplicar varias restricciones delimitadas por dos puntos a un único parámetro. Por ejemplo, la siguiente restricción permite limitar un parámetro a un valor entero de 1 o superior:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

>[!WARNING]
> Las restricciones de ruta que comprueban que la dirección URL se puede convertir en un tipo CLR (como `int` o `DateTime`) usan siempre la referencia cultural invariable, es decir, dan por supuesto que la dirección URL no es localizable. Las restricciones de ruta proporcionadas por el marco de trabajo no modifican los valores almacenados en los valores de ruta. Todos los valores de ruta analizados desde la dirección URL se almacenarán como cadenas. Por ejemplo, la [restricción de ruta Float](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/Constraints/FloatRouteConstraint.cs#L44-L60) intentará convertir el valor de ruta en un valor Float, pero el valor convertido se usa exclusivamente para comprobar que se puede convertir en Float.

## <a name="regular-expressions"></a>Expresiones regulares

El marco de trabajo de ASP.NET Core agrega `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` al constructor de expresiones regulares. En [RegexOptions Enumeration](/dotnet/api/system.text.regularexpressions.regexoptions) (Enumeración RegexOptions) puede ver una descripción de estos miembros.

Las expresiones regulares usan delimitadores y tokens similares a los que usan el enrutamiento y el lenguaje C#. Es necesario usar secuencias de escape con los tokens de expresiones regulares. Por ejemplo, para usar la expresión regular `^\d{3}-\d{2}-\d{4}$` en el enrutamiento, es necesario escribir los caracteres `\` como `\\` en el archivo de código fuente de C# para que el carácter de escape de cadena `\` tenga una secuencia de escape (a menos que use [literales de cadena textuales](/dotnet/csharp/language-reference/keywords/string). Es necesario incluir una secuencia de escape en los caracteres `{`, `}`, "[" y "]". Para ello, duplíquelos a fin de incluir una secuencia de escape en los caracteres delimitadores del parámetro de enrutamiento.  En la tabla siguiente se muestra una expresión regular y la versión con una secuencia de escape.

| Expresión               | Nota |
| ----------------- | ------------ |
| `^\d{3}-\d{2}-\d{4}$` | Expresión regular |
| `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` | Con secuencia de escape  |
| `^[a-z]{2}$` | Expresión regular |
| `^[[a-z]]{{2}}$` | Con secuencia de escape  |

Las expresiones regulares usadas en el enrutamiento suelen empezar con el carácter `^` (coincidencia con la posición inicial de la cadena) y acabar con el carácter `$` (coincidencia con la posición final de la cadena). Los caracteres `^` y `$` garantizan que la expresión regular coincide con el valor completo del parámetro de ruta. Sin los caracteres `^` y `$`, la expresión regular coincidirá con cualquier subcadena de la cadena, algo que normalmente no es lo que se busca conseguir. En la tabla siguiente se muestran algunos ejemplos y se explica por qué que coinciden o no.

| Expresión               | String | Coincidir con | Comentario |
| ----------------- | ------------ |  ------------ |  ------------ |
| `[a-z]{2}` | hello | sí | coincidencias de subcadenas |
| `[a-z]{2}` | 123abc456 | sí | coincidencias de subcadenas |
| `[a-z]{2}` | mz | sí | coincide con la expresión |
| `[a-z]{2}` | MZ | sí | no distingue mayúsculas de minúsculas |
| `^[a-z]{2}$` |  hello | No | vea `^` y `$` más arriba |
| `^[a-z]{2}$` |  123abc456 | No | vea `^` y `$` más arriba |

Consulte las [expresiones regulares de .NET Framework](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) para obtener más información sobre la sintaxis de expresiones regulares.

Para restringir un parámetro a un conjunto conocido de valores posibles, use una expresión regular. Por ejemplo, `{action:regex(^(list|get|create)$)}` solo hace coincidir el valor de ruta `action` con `list`, `get` o `create`. Si se pasa al diccionario de restricciones, la cadena "^(list|get|create)$" sería equivalente. Las restricciones que se pasan al diccionario de restricciones (no insertado en una plantilla) que no coinciden con una de las restricciones conocidas también se tratan como expresiones regulares.

## <a name="url-generation-reference"></a>Referencia de generación de dirección URL

En el ejemplo siguiente se muestra cómo se genera un vínculo a una ruta, dado un diccionario de valores de ruta y un valor `RouteCollection`.

[!code-csharp[](../fundamentals/routing/sample/RoutingSample/Startup.cs?range=45-59)]

El valor `VirtualPath` generado al final del ejemplo anterior es `/package/create/123`.

El segundo parámetro del constructor `VirtualPathContext` es una colección de *valores de ambiente*. Los valores de ambiente aportan comodidad al limitar el número de valores que el desarrollador debe especificar dentro de un determinado contexto de solicitud. Los valores de ruta actuales de la solicitud actual se consideran valores de ambiente para la generación de vínculos. Por ejemplo, si en una aplicación ASP.NET MVC se encuentra en la acción `About` de `HomeController`, no es necesario especificar el valor de ruta de controlador para vincular a la acción `Index` (se usará el valor de ambiente `Home`).

Los valores de ambiente se omiten cuando no coinciden con un parámetro y cuando un valor proporcionado explícitamente lo invalida al ir de izquierda a derecha en la dirección URL.

Los valores que se proporcionan explícitamente pero que no coinciden con nada se agregan a la cadena de consulta. En la tabla siguiente se muestra el resultado cuando se usa la plantilla de ruta `{controller}/{action}/{id?}`.

| Valores de ambiente | Valores explícitos | Resultado |
| -------------   | -------------- | ------ |
| controller="Home" | action="About" | `/Home/About` |
| controller="Home" | controller="Order",action="About" | `/Order/About` |
| controller="Home",color="Red" | action="About" | `/Home/About` |
| controller="Home" | action="About",color="Red" | `/Home/About?color=Red`

Si una ruta tiene un valor predeterminado que no se corresponde con un parámetro y ese valor se proporciona explícitamente, debe coincidir con el valor predeterminado. Por ejemplo:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
  defaults: new { controller = "Blog", action = "ReadPost" });
```

La generación de vínculos solo generará un vínculo para esta ruta si se proporcionan los valores coincidentes para el controlador y la acción.
