---
title: Enrutamiento de ASP.NET Core
author: ardalis
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: bbbcf9e4-3c4c-4f50-b91e-175fe9cae4e2
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/routing
ms.openlocfilehash: 98756e2c5b336aabcf5155d929160b616baaf2ee
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="routing-in-aspnet-core"></a>Enrutamiento de ASP.NET Core

Por [Ryan Nowak](https://github.com/rynowak), [Steve Smith](http://ardalis.com), y [Rick Anderson](https://twitter.com/RickAndMSFT)

Función de enrutamiento es responsable de asignar una solicitud entrante a un controlador de ruta. Las rutas se definen en la aplicación ASP.NET y configuradas cuando se inicia la aplicación. Una ruta, opcionalmente, puede extraer valores de la dirección URL contenida en la solicitud y, a continuación, se pueden utilizar estos valores para procesar las solicitudes. Con información de ruta de la aplicación ASP.NET, la funcionalidad de enrutamiento también es capaz de generar direcciones URL que se asignan a los controladores de ruta. Por lo tanto, el enrutamiento puede encontrar un controlador de ruta basado en una dirección URL o la dirección URL correspondiente a un controlador de ruta determinado en función de la información de controlador de ruta.

>[!IMPORTANT]
> Este documento cubre el núcleo de ASP.NET de nivel bajo enrutamiento. Para el enrutamiento MVC de ASP.NET Core, vea [enrutamiento a las acciones del controlador](../mvc/controllers/routing.md)

[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/sample)

## <a name="routing-basics"></a>Fundamentos del enrutamiento

Usa enrutamiento *rutas* (implementaciones de [IRouter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.routing.irouter)) en:

* asignar las solicitudes entrantes a *enrutar controladores*

* generar direcciones URL que se utilizan en las respuestas

Por lo general, una aplicación tiene una sola colección de rutas. Cuando llega una solicitud, la colección de rutas se procesa en orden. La solicitud entrante busca una ruta que coincida con la dirección URL de la solicitud mediante una llamada a la `RouteAsync` método en cada ruta disponible en la colección de rutas. Por el contrario, una respuesta puede utilizar el enrutamiento para generar direcciones URL (por ejemplo, para la redirección o vínculos) basándose en la información de ruta y, por tanto, evitar tener que codificar de forma rígida direcciones URL, lo que facilita el mantenimiento.

Enrutamiento está conectado a la [middleware](middleware.md) de canalización por la `RouterMiddleware` clase. [ASP.NET MVC](../mvc/overview.md) agrega enrutamiento a la canalización de middleware como parte de su configuración. Para obtener información sobre el uso de enrutamiento como un componente independiente, consulte [utilizando enrutamiento-software intermedio](#using-routing-middleware).

<a name=url-matching-ref></a>

### <a name="url-matching"></a>Coincidencia de dirección URL

Coincidencia de dirección URL es el proceso por qué enrutamiento envía una solicitud entrante a un *controlador*. Este proceso se suele basar en datos en la ruta de acceso de dirección URL, pero se puede extender para tener en cuenta los datos de la solicitud. La capacidad de distribuir las solicitudes para separar los controladores es clave para escalar el tamaño y la complejidad de una aplicación.

Escriba las solicitudes entrantes el `RouterMiddleware`, que llama el `RouteAsync` método en cada ruta en secuencia. El `IRouter` instancia elige si *controlar* la solicitud estableciendo la `RouteContext` `Handler` a un valor no nulo `RequestDelegate`. Si una ruta establece un controlador para la solicitud, se invocará el procesamiento se detiene y el controlador de ruta para procesar la solicitud. Si todas las rutas se prueban y se encuentra ningún controlador para la solicitud, el middleware invoca *siguiente* y se invoca al siguiente middleware en la canalización de solicitudes.

La entrada principal a `RouteAsync` es el `RouteContext` `HttpContext` asociado a la solicitud actual. El `RouteContext.Handler` y `RouteContext` `RouteData` son salidas que se establecerán después de que coincide con una ruta.

Una coincidencia durante la `RouteAsync` también establecerá las propiedades de la `RouteContext.RouteData` en los valores adecuados según el procesamiento de la solicitud hecho hasta ahora. Si una ruta coincide con una solicitud, el `RouteContext.RouteData` contendrá información de estado importante sobre la *resultado*.

`RouteData``Values` es un diccionario de *valores de ruta* producidos a partir de la ruta. Estos valores se determinan normalmente por la dirección URL de la división de tokens y pueden utilizarse para aceptar proporcionados por el usuario, o para tomar decisiones más distribución dentro de la aplicación.

`RouteData``DataTokens` es un contenedor de propiedades de datos adicionales relacionados con la ruta coincidente. `DataTokens`se proporcionan para admitir el estado de asociar datos con las rutas para que la aplicación pueda tomar decisiones más adelante en función de qué ruta coinciden. Estos valores son definidos por el desarrollador y **no** afectan al comportamiento del enrutamiento de ninguna manera. Además, los valores que se esconde en tokens de datos pueden ser de cualquier tipo, a diferencia de los valores de ruta, que debe ser fácilmente convertible a y desde cadenas.

`RouteData``Routers` es una lista de las rutas que participaron en una coincidencia correcta con la solicitud. Las rutas se pueden anidar dentro de otra y el `Routers` propiedad refleja la ruta de acceso a través del árbol lógico de rutas que dan como resultado una coincidencia. Por lo general, el primer elemento de `Routers` es la colección de rutas y se debe usar para la generación de direcciones URL. El último elemento de `Routers` es el controlador de ruta que coincide.

### <a name="url-generation"></a>Generación de direcciones URL

Generación de direcciones URL es el proceso por el que el enrutamiento, puede crear una ruta de acceso de dirección URL basada en un conjunto de valores de ruta. Esto permite una separación lógica entre los controladores y las direcciones URL que tienen acceso a ellos.

Generación de direcciones URL sigue un proceso iterativo similar, pero se inicia con el código de usuario o el marco de trabajo llamar a la `GetVirtualPath` método de la colección de rutas. Cada *ruta* , tendrá su `GetVirtualPath` llama al método en secuencia hasta que un valor no nulo `VirtualPathData` se devuelve.

La principal entradas al `GetVirtualPath` son:

* `VirtualPathContext` `HttpContext`

* `VirtualPathContext` `Values`

* `VirtualPathContext` `AmbientValues`

Las rutas usan principalmente los valores de ruta proporcionados por el `Values` y `AmbientValues` para decidir donde es posible generar una dirección URL y qué valores se deben para incluir. El `AmbientValues` son el conjunto de valores de ruta que se han producido en la coincidencia de la solicitud actual con el sistema de enrutamiento. En cambio, `Values` son los valores de ruta que especifica cómo generar la dirección URL deseada para la operación actual. El `HttpContext` se proporciona en caso de una ruta necesita obtener servicios o datos adicionales asociados con el contexto actual.

Sugerencia: Reflexión de `Values` como un conjunto de valores de reemplazo de la `AmbientValues`. Generación de direcciones URL intenta volver a usar valores de ruta de la solicitud actual para que sea fácil generar direcciones URL para los vínculos con la misma ruta o los valores de ruta.

La salida de `GetVirtualPath` es una `VirtualPathData`. `VirtualPathData`es un paralelo de `RouteData`; contiene el `VirtualPath` para la dirección URL de salida, así como las algunas propiedades adicionales que se deben establecer la ruta.

El `VirtualPathData` `VirtualPath` propiedad contiene el *ruta de acceso virtual* generados por la ruta. Dependiendo de sus necesidades debe procesar aún más la ruta de acceso. Por ejemplo, si desea representar la dirección URL generada en HTML debe anteponer la ruta de acceso base de la aplicación.

El `VirtualPathData` `Router` es una referencia a la ruta que se generó correctamente la dirección URL.

El `VirtualPathData` `DataTokens` propiedades es un diccionario de datos adicionales relacionados con la ruta que genera la dirección URL. Se trata el paralelo de `RouteData.DataTokens`.

### <a name="creating-routes"></a>Crear rutas

El enrutamiento proporciona la `Route` clase como la implementación estándar de `IRouter`. `Route`usa el *plantilla de ruta* sintaxis para definir patrones que se hará coincidir con la ruta de acceso de dirección URL cuando `RouteAsync` se llama. `Route`usará la misma plantilla de ruta para generar una dirección URL cuando `GetVirtualPath` se llama.

Mayoría de las aplicaciones creará rutas mediante una llamada a `MapRoute` o uno de los métodos de extensión similares definidos en `IRouteBuilder`. Todos estos métodos creará una instancia de `Route` y lo agrega a la colección de rutas.

Nota: `MapRoute` no toma un parámetro del controlador de ruta - solo agrega las rutas que será procesadas por el `DefaultHandler`. Puesto que el controlador predeterminado es una `IRouter`, es posible que decida no atender la solicitud. Por ejemplo, ASP.NET MVC normalmente se configura como un controlador predeterminado que solo administra las solicitudes que coinciden con una acción y controlador disponible. Para más información sobre el enrutamiento de MVC, consulte [enrutamiento a las acciones del controlador](../mvc/controllers/routing.md).

Este es un ejemplo de un `MapRoute` llamada que se usa una definición de ruta de ASP.NET MVC típica:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Esta plantilla coincidirá con una ruta de acceso de dirección URL como `/Products/Details/17` y extraer los valores de ruta `{ controller = Products, action = Details, id = 17 }`. Los valores de ruta se determinan por dividir la ruta de acceso de dirección URL en segmentos y coincidencia de cada segmento de la *enrutar parámetro* nombre en la plantilla de ruta. Se denominan parámetros de ruta. Se definen por escriba el nombre del parámetro entre llaves `{ }`.

La plantilla anterior también podría coincidir con la ruta de acceso de dirección URL `/` y generaría los valores `{ controller = Home, action = Index }`. Esto sucede porque el `{controller}` y `{action}` ruta parámetros tienen valores predeterminados y el `id` parámetro de ruta es opcional. Es igual a `=` inicio de sesión seguido por un valor después de que el nombre de parámetro de ruta define un valor predeterminado para el parámetro. Un signo de interrogación `?` después de que el nombre de parámetro de ruta define el parámetro como opcional. Parámetros con un valor predeterminado de ruta *siempre* producen un valor de ruta cuando coincide con la ruta: parámetros opcionales no producirá un valor de ruta si no hay ningún segmento de ruta de acceso de dirección URL correspondiente.

Vea [referencia de plantillas de ruta](#route-template-reference) para obtener una descripción detallada de la sintaxis y características de la plantilla de ruta.

Este ejemplo incluye una *enrutar restricción*:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Esta plantilla coincidirá con una ruta de acceso de dirección URL como `/Products/Details/17`, pero no `/Products/Details/Apples`. La definición de parámetro de ruta `{id:int}` define un *enrutar restricción* para el `id` parámetro de ruta. Implementan restricciones de la ruta `IRouteConstraint` e inspeccionar los valores de ruta para comprobarlos. En este ejemplo, el valor de ruta `id` debe poder convertirse en un entero. Vea [referencia de restricción de ruta](#route-constraint-reference) para obtener una explicación más detallada de las restricciones de ruta que se proporcionan con el marco de trabajo.

Las sobrecargas adicionales de `MapRoute` acepte valores para `constraints`, `dataTokens`, y `defaults`. Estos parámetros adicionales de `MapRoute` se define como tipo `object`. El uso habitual de estos parámetros consiste en pasar un objeto con tipo anónimo, donde los nombres de propiedad de la coincidencia de tipo anónimo enrutan nombres de parámetro.

Los dos ejemplos siguientes crean rutas equivalente:

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Sugerencia: La sintaxis en línea para definir las restricciones y los valores predeterminados puede ser más conveniente para rutas simples. Sin embargo, existen características como tokens de datos que no son compatibles con la sintaxis en línea.

Este ejemplo muestra algunas de las características más:

```csharp
routes.MapRoute(
  name: "blog",
  template: "Blog/{*article}",
  defaults: new { controller = "Blog", action = "ReadArticle" });
```

Esta plantilla coincidirá con una ruta de acceso de dirección URL como `/Blog/All-About-Routing/Introduction` y va a extraer los valores `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. Los valores de la ruta predeterminada para `controller` y `action` son generadas por la ruta, incluso si no hay ningún parámetro de ruta correspondiente en la plantilla. Pueden especificar valores predeterminados en la plantilla de ruta. El `article` parámetro de ruta se define como un *general* por la apariencia de un asterisco `*` antes del nombre de parámetro de ruta. Parámetros de ruta comodín capturar el resto de la ruta de acceso de dirección URL y también pueden coincidir con la cadena vacía.

Este ejemplo agrega los tokens de datos y las restricciones de ruta:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Esta plantilla coincidirá con una ruta de acceso de dirección URL como `/Products/5` y va a extraer los valores `{ controller = Products, action = Details, id = 5 }` y los tokens de datos `{ locale = en-US }`.

![Tokens de Windows de variables locales](routing/_static/tokens.png)

<a name=id1></a>

### <a name="url-generation"></a>Generación de direcciones URL

La `Route` clase también puede realizar la generación de direcciones URL mediante la combinación de un conjunto de valores de ruta con su plantilla de ruta. Esto es, lógicamente, el proceso inverso de la ruta de acceso de dirección URL de búsqueda de coincidencias.

Sugerencia: Para entender mejor generación de direcciones URL, imagine qué dirección URL que desea generar y, a continuación, piense en cómo una plantilla de ruta coincidiría con esa dirección URL. ¿Qué valores se generaría? Éste es el equivalente aproximado de cómo funciona la generación de direcciones URL en la `Route` clase.

Este ejemplo utiliza una ruta de estilo de ASP.NET MVC básica:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Con los valores de ruta `{ controller = Products, action = List }`, esta ruta generará la dirección URL `/Products/List`. Los valores de ruta se sustituyen por los parámetros de ruta correspondiente formar la ruta de acceso de dirección URL. Puesto que `id` es una función opcional parámetro de ruta, no se trata de ningún problema que no tiene un valor.

Con los valores de ruta `{ controller = Home, action = Index }`, esta ruta generará la dirección URL `/`. Los valores de ruta que se proporcionaron coincide con los valores predeterminados para que los segmentos correspondientes a esos valores se pueden omitir sin ningún riesgo. Tenga en cuenta que ambas direcciones URL generado podría ida y vuelta con esta definición de ruta y generar los mismos valores de ruta que se usaron para generar la dirección URL.

Sugerencia: Debe usar una aplicación con ASP.NET MVC `UrlHelper` para generar direcciones URL en lugar de llamar a enrutamiento directamente.

Para obtener más detalles sobre el proceso de generación de dirección URL, vea [referencia de generación de url](#url-generation-reference).

## <a name="using-routing-middleware"></a>Uso de Middleware de enrutamiento

Agregue el paquete NuGet "Microsoft.AspNetCore.Routing".

Agregar al contenedor de servicios en el enrutamiento *Startup.cs*:

[!code-csharp[Main](../fundamentals/routing/sample/RoutingSample/Startup.cs?highlight=3&start=11&end=14)]

Las rutas deben configurarse en el `Configure` método en la `Startup` clase. El ejemplo siguiente utiliza estas API:

* `RouteBuilder`
* `Build`
* `MapGet`Coincide con solamente las solicitudes HTTP GET
* `UseRouter`

<!-- literal_block {"xml:space": "preserve", "source": "fundamentals/routing/sample/RoutingSample/Startup.cs", "ids": [], "linenos": false, "highlight_args": {"linenostart": 1}} -->

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

La siguiente tabla muestra las respuestas con el URI especificado.

| Identificador URI | Respuesta  |
| ------- | -------- |
| /Package/Create/3  | Hello! Valores de ruta: [operación, crear], [Id. de 3] |
| / paquetes/seguimiento /-3  | Hello! Valores de ruta: [operación, pista], [Id. de -3] |
| / paquetes/seguimiento/3 / | Hello! Valores de ruta: [operación, pista], [Id. de -3]  |
| /Package/seguimiento / | \<Pasar ninguna coincidencia explícitamente > |
| OBTENER /hello/Joe | Hola, Juan. |
| REGISTRAR /hello/Joe | \<Pasar explícitamente, solo las coincidencias HTTP GET > |
| OBTENER /hello/Joe/Smith | \<Pasar ninguna coincidencia explícitamente > |

Si va a configurar una única ruta, llame a `app.UseRouter` pasando un `IRouter` instancia. No tendrá que llamar a `RouteBuilder`.

El marco de trabajo proporciona un conjunto de métodos de extensión para crear rutas, como:

* `MapRoute`
* `MapGet`
* `MapPost`
* `MapPut`
* `MapDelete`
* `MapVerb`

Algunos de estos métodos como `MapGet` requieren un `RequestDelegate` proporcionarse. El `RequestDelegate` se usará como el *controlador de ruta* cuando coincide con la ruta. Otros métodos de esta familia permiten configurar una canalización de middleware que se usará como controlador de ruta. Si el *mapa* método no acepta un controlador, como `MapRoute`, entonces va a usar el `DefaultHandler`.

El `Map[Verb]` métodos usan restricciones para limitar la ruta para el verbo HTTP en el nombre del método. Por ejemplo, vea [MapGet](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L85-L88) y [MapVerb](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L156-L180).

## <a name="route-template-reference"></a>Referencia de plantilla de ruta

Tokens entre llaves (`{ }`) definir *enrutar parámetros* que se enlazará si la ruta se encuentran coincidencias. Puede definir más de un parámetro de ruta en un segmento de ruta, pero deben estar separados por un valor literal. Por ejemplo `{controller=Home}{action=Index}` no sería una ruta válida, porque no hay ningún valor literal entre `{controller}` y `{action}`. Estos parámetros de ruta deben tener un nombre y atributos adicionales pueden especificar.

Texto literal distintos de los parámetros de ruta (por ejemplo, `{id}`) y el separador de ruta de acceso `/` debe coincidir con el texto en la dirección URL. La coincidencia de texto distingue entre mayúsculas y minúsculas y se basa en la representación de la ruta de acceso de direcciones URL descodificada. Para que coincida con el delimitador de parámetro de ruta literal `{` o `}`, escape repitiendo el carácter (`{{` o `}}`).

Patrones de dirección URL que intentan capturar un nombre de archivo con una extensión de archivo opcional tienen consideraciones adicionales. Por ejemplo, mediante la plantilla `files/{filename}.{ext?}` : si ambos `filename` y `ext` existe, ambos valores se rellenará. Si solo `filename` existe en la dirección URL, las coincidencias de la ruta porque el punto final `.` es opcional. Las direcciones URL siguientes coincidiría con esta ruta:

* `/files/myFile.txt`
* `/files/myFile.`
* `/files/myFile`

Puede usar el `*` caracteres como prefijo para un parámetro de ruta para enlazar con el resto del URI: Esto se denomina una *general* parámetro. Por ejemplo, `blog/{*slug}` coincidiría con cualquier URI que se inició con `/blog` y tenía ningún valor siguiente (que se asignarían a la `slug` enrutar valor). Detección de todos los parámetros también pueden coincidir con la cadena vacía.

Pueden tener parámetros de ruta *valores predeterminados*, designada especificando el valor predeterminado después del nombre de parámetro, separado por un `=`. Por ejemplo, `{controller=Home}` tendría que definir `Home` como el valor predeterminado de `controller`. Si ningún valor se encuentra en la dirección URL para el parámetro, se utiliza el valor predeterminado. Además de los valores predeterminados, pueden ser opcionales de parámetros de ruta (especificado mediante la anexión de un `?` hasta el final del nombre del parámetro, como en `id?`). La diferencia entre opcional y "tiene un valor predeterminado" es que un parámetro de ruta con un valor predeterminado siempre genera un valor; un parámetro opcional tiene un valor sólo cuando se proporciona uno.

Parámetros de ruta también pueden tener restricciones, que deben coincidir con el valor de ruta enlazado desde la dirección URL. Agregar un signo de dos puntos `:` y nombre de la restricción después de que el nombre de parámetro de ruta especifica un *restricción alineada* en un parámetro de ruta. Si la restricción requiere argumentos de los que se proporcionan entre paréntesis `( )` después del nombre de restricción. Se pueden especificar varias restricciones alineadas anexando otra coma `:` y nombre de la restricción. El nombre de la restricción se pasa a la `IInlineConstraintResolver` servicio para crear una instancia de `IRouteConstraint` a utilizar en el procesamiento de la dirección URL. Por ejemplo, la plantilla de ruta `blog/{article:minlength(10)}` especifica la `minlength` restricción con el argumento `10`. Para obtener más restricciones de la ruta de descripción y una lista de las restricciones proporcionada por el marco de trabajo, consulte [referencia de restricción de ruta](#route-constraint-reference).

La tabla siguiente muestran algunas plantillas de ruta y su comportamiento.

| Plantilla de ruta | Dirección URL de búsqueda de coincidencias de ejemplo | Notas |
| -------- | -------- | ------- |
| hello  | sería  | Solo coincide con la ruta de acceso único`/hello` |
| {Página = Home} | / | Coincide con y establece `Page` a`Home` |
| {Página = Home}  | O póngase en contacto con  | Coincide con y establece `Page` a`Contact` |
| ¿{controller} / {action} / {id}? | / Productos/lista | Se asigna a `Products` controlador y `List` acción |
| ¿{controller} / {action} / {id}? | / Productos/detalles/123  |  Se asigna a `Products` controlador y `Details` acción.  `id`establézcalo 123 |
| ¿{controlador = Home} / {acción = índice} / {id}? | /  |  Se asigna a `Home` controlador y `Index` método; `id` se omite. |

Mediante una plantilla suele ser el método más sencillo para el enrutamiento. Las restricciones y los valores predeterminados pueden especificarse también fuera de la plantilla de ruta.

Sugerencia: Habilitar [registro](logging.md) para ver cómo la basadas en implementaciones de enrutamientos, como `Route`, coincide con las solicitudes.

## <a name="route-constraint-reference"></a>Referencia de restricción de ruta

Restricciones de la ruta cuando ejecutan un `Route` tiene coincide con la sintaxis de la dirección URL entrante y acorta la ruta de acceso de dirección URL en valores de ruta. En general, las restricciones de ruta inspeccionar el valor de ruta asociado a través de la plantilla de ruta y realizar un simple sí/no de decisión sobre si es o no el valor aceptable. Algunas restricciones de la ruta utilizan datos que superan el valor de ruta para tener en cuenta si se puede enrutar la solicitud. Por ejemplo, el `HttpMethodRouteConstraint` puede aceptar o rechazar una solicitud basada en su verbo HTTP.

>[!WARNING]
> Evite el uso de restricciones para **la validación de entrada**, porque esa entrada no válida de este modo, producirá un error 404 (no encontrado) en lugar de un error 400 con un mensaje de error adecuado. Restricciones de la ruta deben usarse para **eliminar la ambigüedad de** entre las rutas similares, no para validar las entradas para una ruta determinada.

La tabla siguiente muestran algunas restricciones de la ruta y el comportamiento esperado.

| restricción | Ejemplo | Coincidencias de ejemplo | Notas |
| --------   | ------- | ------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789`  | Hace coincidir cualquier número entero |
| `bool`  | `{active:bool}` | `true`, `FALSE` | Coincidencias `true` o `false` (entre mayúsculas y minúsculas) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm`  | Coincide con un válido `DateTime` valor (en la referencia cultural invariable - ver advertencia) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Coincide con un válido `decimal` valor (en la referencia cultural invariable - ver advertencia) |
| `double`  | `{weight:double}` | `1.234`, `-1,001.01e8` | Coincide con un válido `double` valor (en la referencia cultural invariable - ver advertencia) |
| `float`  | `{weight:float}` | `1.234`, `-1,001.01e8` | Coincide con un válido `float` valor (en la referencia cultural invariable - ver advertencia) |
| `guid`  | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Coincide con un válido `Guid` valor |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Coincide con un válido `long` valor |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | Cadena debe tener al menos 4 caracteres |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | Cadena debe ser no más de 8 caracteres |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | Cadena debe tener exactamente 12 caracteres |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | Cadena debe tener al menos 8 y no más de 16 caracteres |
| `min(value)` | `{age:min(18)}` | `19` | Valor entero debe ser al menos 18 |
| `max(value)` | `{age:max(120)}` |  `91` | Valor entero debe ser no más de 120 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | Valor entero debe ser al menos 18 pero no más de 120 |
| `alpha` | `{name:alpha}` | `Rick` | Cadena debe constar de uno o más caracteres alfabéticos (`a`-`z`, entre mayúsculas y minúsculas) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | Cadena debe coincidir con la expresión regular (Véase sugerencias acerca de cómo definir una expresión regular) |
| `required`  | `{name:required}` | `Rick` |  Utiliza para exigir que un valor de parámetro no está presente durante la generación de direcciones URL |

>[!WARNING]
> Restricciones de la ruta que compruebe la dirección URL se pueden convertir en un tipo CLR (como `int` o `DateTime`) utilizar siempre la referencia cultural invariable: adoptan la dirección URL no es localizable. Las restricciones de ruta proporcionado por el marco de trabajo no modifique los valores almacenados en los valores de ruta. Todos los valores de ruta analizados desde la dirección URL se almacenarán como cadenas. Por ejemplo, el [restricción de ruta Float](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/Constraints/FloatRouteConstraint.cs#L44-L60) intentará convertir el valor de ruta en un valor flotante, pero el valor convertido se usa exclusivamente para comprobar que se pueda convertir en flotante.

## <a name="regular-expressions"></a>Expresiones regulares 

El marco de trabajo de ASP.NET Core agrega `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` al constructor de expresión regular. Vea [enumeración RegexOptions](https://msdn.microsoft.com/library/system.text.regularexpressions.regexoptions(v=vs.110).aspx) para obtener una descripción de estos miembros.

Expresiones regulares utilizan delimitadores y símbolos (tokens) similares a aquellos utilizados con enrutamiento y el lenguaje C#. Deben convertirse los tokens de expresión regular. Por ejemplo, para usar la expresión regular `^\d{3}-\d{2}-\d{4}$` de enrutamiento, debe tener la `\` los caracteres escritos en como `\\` en el archivo de código fuente de C# para escapar el `\` carácter de escape de cadena (a menos que utilice [literalmente literales de cadena](https://msdn.microsoft.com/library/aa691090(v=vs.71).aspx)). El `{` , `}` , ' [' y ']' caracteres tienen que evitarse duplicándolas escape los caracteres delimitadores de parámetro de enrutamiento.  En la tabla siguiente se muestra una expresión regular y la versión de la secuencia de escape.

| Expresión               | Nota |
| ----------------- | ------------ | 
| `^\d{3}-\d{2}-\d{4}$` | Expresión regular |
| `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` | Caracteres de escape  |
| `^[a-z]{2}$` | Expresión regular |
| `^[[a-z]]{{2}}$` | Caracteres de escape  |

Expresiones regulares utilizadas en el enrutamiento a menudo se iniciará con la `^` caracteres (posición inicial de la cadena de coincidencia) y terminan con la `$` caracteres (coincidencia de una posición de la cadena final). El `^` y `$` caracteres Asegúrese de que la coincidencia de expresión regular el valor del parámetro de ruta completa. Sin el `^` y `$` caracteres de la expresión regular coincidirá con cualquier cadena subcarpetas dentro de la cadena, que a menudo no es lo desea. En la tabla siguiente se muestra algunos ejemplos y explica por qué que coinciden o no coinciden.

| Expresión               | Cadena | Coincidir con | Comentario |
| ----------------- | ------------ |  ------------ |  ------------ | 
| `[a-z]{2}` | hello | sí | coincidencia de subcadena |
| `[a-z]{2}` | 123abc456 | sí | coincidencia de subcadena |
| `[a-z]{2}` | MZ | sí | coincide con la expresión |
| `[a-z]{2}` | MZ | sí | no distinguir mayúsculas de minúsculas |
| `^[a-z]{2}$` |  hello | No | vea `^` y `$` anteriormente |
| `^[a-z]{2}$` |  123abc456 | No | vea `^` y `$` anteriormente |

Hacer referencia a [expresiones regulares de .NET Framework](https://msdn.microsoft.com/library/hs600312(v=vs.110).aspx) para obtener más información acerca de la sintaxis de expresión regular.

Para restringir un parámetro con un conjunto conocido de posibles valores, utilice una expresión regular. Por ejemplo `{action:regex(^(list|get|create)$)}` solo coincide con el `action` enrutar valor a `list`, `get`, o `create`. Si se pasa en el diccionario de restricciones, la cadena "^ (lista | get | crear) $" sería equivalente. Las restricciones que se pasan en el diccionario de restricciones (no alineado dentro de una plantilla) que no coinciden con una de las restricciones conocidas también se tratan como expresiones regulares.

## <a name="url-generation-reference"></a>Referencia de generación de dirección URL

El ejemplo siguiente muestra cómo generar un vínculo a una ruta tiene un diccionario de valores de ruta y un `RouteCollection`.

[!code-csharp[Main](../fundamentals/routing/sample/RoutingSample/Startup.cs?range=45-59)]

El `VirtualPath` generado al final del ejemplo anterior es `/package/create/123`.

El segundo parámetro a la `VirtualPathContext` constructor es una colección de *valores ambiente*. Valores de ambiente aportar comodidad limitando el número de valores que se debe especificar un programador dentro de un determinado contexto de solicitud. Los valores actuales de la ruta de la solicitud actual se consideran valores de ambiente para la generación de vínculo. Por ejemplo, en una aplicación de MVC de ASP.NET si se encuentra en la `About` acción de la `HomeController`, no es necesario especificar el valor de la ruta de controlador para vincular a la `Index` acción (el valor ambiente de `Home` usará).

Se omiten los valores de ambiente que no coincide con un parámetro, y también se omiten los valores de ambiente cuando un valor explícitamente proporcionados, invalida van de izquierda a derecha en la dirección URL.

Los valores que se proporcionan de forma explícita, pero que no coincide con nada se agregan a la cadena de consulta. En la tabla siguiente se muestra el resultado cuando se usa la plantilla de ruta `{controller}/{action}/{id?}`.

| Valores de ambiente | Valores explícitos | Resultado |
| -------------   | -------------- | ------ |
| controlador = "Inicio" | acción = "About" | `/Home/About` |
| controlador = "Inicio" | controlador = "Order", acción = "About" | `/Order/About` |
| controlador = "Home", color = "Red" | acción = "About" | `/Home/About` |
| controlador = "Inicio" | acción = "About" de color = "Red" | `/Home/About?color=Red`

Si una ruta tiene un valor predeterminado que no se corresponde con un parámetro y se proporciona explícitamente ese valor, debe coincidir con el valor predeterminado. Por ejemplo:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
  defaults: new { controller = "Blog", action = "ReadPost" });
```

Generación de vínculo solo generaría un vínculo para esta ruta cuando se proporcionan los valores de búsqueda de coincidencias de acción y controlador.
