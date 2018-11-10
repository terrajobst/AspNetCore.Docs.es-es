---
title: Enrutamiento en ASP.NET Core
author: ardalis
description: Descubra la manera en que la funcionalidad de enrutamiento de ASP.NET Core se encarga de asignar una solicitud entrante a un controlador de ruta.
ms.author: riande
ms.custom: mvc
ms.date: 10/01/2018
uid: fundamentals/routing
ms.openlocfilehash: a014782ba503bc8bd0fdefb4cb4f382aa8fde4cd
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244975"
---
# <a name="routing-in-aspnet-core"></a>Enrutamiento en ASP.NET Core

Por [Ryan Nowak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/) y [Rick Anderson](https://twitter.com/RickAndMSFT)

La funcionalidad de enrutamiento de ASP.NET Core se encarga de asignar una solicitud entrante a un controlador de ruta. Las rutas se definen en la aplicación y se configuran cuando se inicia la aplicación. Una ruta puede extraer opcionalmente valores de la dirección URL contenida en la solicitud, que se pueden usar para procesar las solicitudes. Con la información de ruta de la aplicación, la funcionalidad de enrutamiento también puede generar direcciones URL que se asignan a controladores de ruta. Por tanto, el enrutamiento puede buscar un controlador de ruta a partir de una dirección URL, o bien encontrar la dirección URL correspondiente a un controlador de ruta determinado en función de la información del controlador de ruta.

La mayoría de las aplicaciones deben elegir un esquema de enrutamiento básico y descriptivo para que las direcciones URL sean legibles y significativas. La ruta convencional predeterminada `{controller=Home}/{action=Index}/{id?}`:

* Admite un esquema de enrutamiento básico y descriptivo:
* Es un buen punto de partida para las aplicaciones web destinadas a usarse en los exploradores.

Es habitual agregar rutas breves adicionales a áreas de mucho tráfico de la aplicación en situaciones especializadas (por ejemplo, blogs o comercio electrónico) con el [enrutamiento mediante atributos](xref:mvc/controllers/routing#attribute-routing) o rutas convencionales dedicadas.

Las API web deben usar el enrutamiento mediante atributos para modelar la funcionalidad de la aplicación como un conjunto de recursos donde las operaciones se representan mediante verbos HTTP. Esto significa que muchas operaciones (por ejemplo, GET y POST) del mismo recurso lógico usarán la misma dirección URL. El enrutamiento mediante atributos ofrece un nivel de control que resulta necesario para diseñar cuidadosamente un espacio de direcciones URL de la API.

La compatibilidad de la generación de direcciones URL de MVC permite desarrollar la aplicación sin necesidad de codificar de forma rígida las direcciones URL para vincular la aplicación. Esto permite partir con una configuración de enrutamiento básica y modificar las rutas una vez determinada la forma de la aplicación.

> [!IMPORTANT]
> En este documento se describe el enrutamiento de ASP.NET Core de bajo nivel. Para obtener información sobre el enrutamiento de ASP.NET Core MVC, vea <xref:mvc/controllers/routing>.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="routing-basics"></a>Fundamentos del enrutamiento

El enrutamiento usa *rutas* (implementaciones de <xref:Microsoft.AspNetCore.Routing.IRouter>) para:

* Asignar las solicitudes entrantes a *controladores de ruta*.
* Generar direcciones URL que se usan en las respuestas.

Por lo general, una aplicación tiene una sola colección de rutas. Cuando llega una solicitud, la colección de rutas se procesa en orden. La solicitud entrante busca una ruta que coincida con la dirección URL de la solicitud. Para ello, llama al método <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> en cada ruta disponible de la colección de rutas. En cambio, una respuesta puede usar el enrutamiento para generar direcciones URL (por ejemplo, para el redireccionamiento o los vínculos) en función de la información de ruta. De este modo, se evita tener que codificar de forma rígida las direcciones URL, lo que facilita el mantenimiento.

La clase <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> conecta el enrutamiento a la canalización de [software intermedio](xref:fundamentals/middleware/index). [ASP.NET Core MVC](xref:mvc/overview) agrega enrutamiento a la canalización de software intermedio como parte de su configuración. Para obtener información sobre el uso del enrutamiento como componente independiente, vea la sección [Uso de software intermedio de enrutamiento](#use-routing-middleware).

### <a name="url-matching"></a>Coincidencia de dirección URL

La coincidencia de dirección URL es el proceso por el cual el enrutamiento envía una solicitud entrante a un *controlador*. Este proceso se basa en datos de la ruta de dirección URL, pero se puede ampliar para tener en cuenta cualquier dato de la solicitud. La capacidad de enviar solicitudes a controladores independientes es clave para escalar el tamaño y la complejidad de una aplicación.

Las solicitudes entrantes especifican la clase `RouterMiddleware`, que llama al método <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> en cada ruta de la secuencia. La instancia de <xref:Microsoft.AspNetCore.Routing.IRouter> decide si *controla* la solicitud mediante el establecimiento de [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) en un <xref:Microsoft.AspNetCore.Http.RequestDelegate> que no sea NULL. Si una ruta establece un controlador para la solicitud, el procesamiento de rutas se detiene y se invoca el controlador para procesar la solicitud. Si se prueban todas las rutas y no se encuentra ningún controlador para la solicitud, el software intermedio llama a *next* y se invoca el software intermedio siguiente de la canalización de solicitudes.

La entrada principal para `RouteAsync` es el [RouteContext.HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*) asociado a la solicitud actual. `RouteContext.Handler` y [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) son salidas que se establecen después de que una ruta coincida.

Una coincidencia durante `RouteAsync` también establece las propiedades de `RouteContext.RouteData` en los valores adecuados en función del procesamiento de solicitudes realizado hasta el momento. Si una ruta coincide con una solicitud, `RouteContext.RouteData` contiene información de estado importante sobre el *resultado*.

[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) es un diccionario de los *valores de ruta* generados desde la ruta. Estos valores se suelen determinar mediante la conversión en tokens de la dirección URL, y se pueden usar para aceptar la entrada del usuario o para tomar otras decisiones sobre el envío dentro de la aplicación.

[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) es un contenedor de propiedades de datos adicionales relacionados con la ruta coincidente. Se proporcionan `DataTokens` para permitir la asociación de datos de estado con cada ruta, de modo que la aplicación pueda tomar decisiones más adelante en función de las rutas que han coincidido. Estos valores los define el desarrollador y **no** afectan de ninguna manera al comportamiento del enrutamiento. Además, los valores que se guardan provisionalmente en `RouteData.DataTokens` pueden ser de cualquier tipo, a diferencia de `RouteData.Values`, que deben poder convertirse fácilmente en cadenas y a partir de estas.

[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers*) es una lista de las rutas que han participado en encontrar una coincidencia correcta con la solicitud. Las rutas se pueden anidar unas dentro de otras. La propiedad `Routers` refleja la ruta de acceso del árbol lógico de rutas que han tenido como resultado una coincidencia. Por lo general, el primer elemento de `Routers` es la colección de rutas y se debe usar para la generación de direcciones URL. El último elemento de `Routers` es el controlador de ruta que ha coincidido.

### <a name="url-generation"></a>Generación de dirección URL

La generación de dirección URL es el proceso por el cual el enrutamiento puede crear una ruta de dirección URL basada en un conjunto de valores de ruta. Esto permite una separación lógica entre los controladores y las direcciones URL que tienen acceso a ellos.

La generación de direcciones URL sigue un proceso iterativo similar, pero se inicia cuando el código de usuario o de marco de trabajo llama al método <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> de la colección de rutas. Después, se llama en secuencia al método `GetVirtualPath` de cada *ruta* hasta que se devuelva un valor <xref:Microsoft.AspNetCore.Routing.VirtualPathData> distinto de NULL.

La principal entradas de `GetVirtualPath` son:

* [VirtualPathContext.HttpContext](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext*)
* [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*)
* [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*)

Las rutas usan principalmente los valores de ruta proporcionados por `Values` y `AmbientValues` para decidir si es posible generar una dirección URL y qué valores se deben incluir. `AmbientValues` son el conjunto de valores de ruta producidos cuando la solicitud actual coincide con el sistema de enrutamiento. En cambio, `Values` son los valores de ruta que especifican cómo se genera la dirección URL deseada para la operación actual. Se proporciona `HttpContext` por si una ruta necesita obtener servicios o datos adicionales asociados con el contexto actual.

> [!TIP]
> Piense en [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) como un conjunto de invalidaciones para [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*). La generación de direcciones URL intenta reutilizar los valores de ruta de la solicitud actual para que sea más fácil generar direcciones URL para los vínculos con la misma ruta o valores de ruta.

La salida de `GetVirtualPath` es `VirtualPathData`. `VirtualPathData` es un valor paralelo de `RouteData`. `VirtualPathData` contiene el valor `VirtualPath` de la dirección URL de salida y algunas propiedades más que la ruta debe establecer.

La propiedad [VirtualPathData.VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) contiene la *ruta de acceso virtual* generada por la ruta. Es posible que deba procesar aún más la ruta de acceso, según sus necesidades. Si quiere representar la dirección URL generada en HTML, anteponga la ruta de acceso base de la aplicación.

[VirtualPathData.Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) es una referencia a la ruta que ha generado correctamente la dirección URL.

La propiedad [VirtualPathData.DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) es un diccionario de datos adicionales relacionados con la ruta que ha generado la dirección URL. Se trata del valor paralelo de [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*).

### <a name="creating-routes"></a>Creación de rutas

El enrutamiento proporciona la clase <xref:Microsoft.AspNetCore.Routing.Route> como implementación estándar de <xref:Microsoft.AspNetCore.Routing.IRouter>. `Route` usa la sintaxis de *plantilla de ruta* para definir los patrones que se hacen coincidir con la ruta de dirección URL cuando se llama a <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*>. `Route` usa la misma plantilla de ruta para generar una dirección URL cuando se llama a `GetVirtualPath`.

La mayoría de las aplicaciones crea rutas mediante una llamada a <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> o a uno de los métodos de extensión similares definidos en <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>. Todos estos métodos crean una instancia de <xref:Microsoft.AspNetCore.Routing.Route> y la agregan a la colección de rutas.

`MapRoute` no toma un parámetro de controlador de ruta. `MapRoute` solo agrega las rutas que se controlan mediante <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>. Dado que el controlador predeterminado es `IRouter`, es posible que decida no controlar la solicitud. Por ejemplo, ASP.NET Core MVC se suele configurar como un controlador predeterminado que solo controla las solicitudes que coinciden con un controlador y una acción disponibles. Para obtener más información sobre el enrutamiento a MVC, vea <xref:mvc/controllers/routing>.

El código siguiente es un ejemplo de una llamada a `MapRoute` usada por una definición de ruta típica de ASP.NET Core MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Esta plantilla coincide con una ruta de dirección URL como `/Products/Details/17` y extrae los valores de ruta `{ controller = Products, action = Details, id = 17 }`. Los valores de ruta se determinan al dividir la ruta de dirección URL en segmentos y hacer coincidir cada segmento con el nombre del *parámetro de ruta* en la plantilla de ruta. Los parámetros de ruta tienen un nombre asignado. Para definirlos, el nombre del parámetro se incluye entre llaves `{ ... }`.

La plantilla anterior también podría coincidir con la ruta de dirección URL `/` y generaría los valores `{ controller = Home, action = Index }`. Esto se debe a que los parámetros de ruta `{controller}` y `{action}` tienen valores predeterminados y el parámetro de ruta `id` es opcional. Un signo igual `=` seguido de un valor después del nombre del parámetro de ruta define un valor predeterminado para el parámetro. Un signo de interrogación `?` después del nombre del parámetro de ruta define el parámetro como opcional. Los parámetros de ruta con un valor predeterminado *siempre* generan un valor de ruta cuando la ruta coincide. Los parámetros opcionales no generan un valor de ruta si no existió el segmento de ruta de dirección URL correspondiente.

Vea en [Referencia de plantilla de ruta](#route-template-reference) una descripción detallada de las características y la sintaxis de la plantilla de ruta.

En este ejemplo se incluye una *restricción de ruta*:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Esta plantilla coincide con una ruta de dirección URL como `/Products/Details/17`, pero no con `/Products/Details/Apples`. La definición de parámetro de ruta `{id:int}` define una [restricción de ruta](#route-constraint-reference) para el parámetro de ruta `id`. Las restricciones de ruta implementan <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> e inspeccionan los valores de ruta para comprobarlos. En este ejemplo, el valor de ruta `id` debe poder convertirse en un entero. Vea en [Referencia de restricción de ruta](#route-constraint-reference) una explicación más detallada de las restricciones de ruta que se proporcionan con el marco de trabajo.

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

> [!TIP]
> La sintaxis insertada para definir las restricciones y los valores predeterminados puede ser conveniente para las rutas simples. Pero hay algunas características, como los tokens de datos, que no son compatibles con esta sintaxis.

En el ejemplo siguiente se muestran algunos escenarios más:

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{*article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

Esta plantilla coincide con una ruta de dirección URL como `/Blog/All-About-Routing/Introduction` y extrae los valores `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. La ruta genera los valores de ruta predeterminados para `controller` y `action`, incluso si no hay parámetros de ruta correspondientes en la plantilla. Los valores predeterminados pueden especificarse en la plantilla de ruta. El parámetro de ruta `article` se define como un *comodín* por la aparición de un asterisco `*` antes del nombre del parámetro de ruta. Los parámetros de ruta comodín capturan el resto de la ruta de dirección URL y también pueden coincidir con la cadena vacía.

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

### <a name="url-generation"></a>Generación de dirección URL

La clase `Route` también puede llevar a cabo la generación de dirección URL mediante la combinación de un conjunto de valores de ruta con su plantilla de ruta. Se trata lógicamente del proceso inverso de hacer coincidir la ruta de dirección URL.

> [!TIP]
> Para entender mejor la generación de direcciones URL, imagine qué dirección URL quiere generar y, después, piense cómo coincidiría una plantilla de ruta con esa dirección URL. ¿Qué valores se generarían? Este es un equivalente aproximado de cómo funciona la generación de dirección URL en la clase `Route`.

En este ejemplo se usa una ruta básica de estilo de ASP.NET Core MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Con los valores de ruta `{ controller = Products, action = List }`, esta ruta genera la dirección URL `/Products/List`. Los valores de ruta se sustituyen por los parámetros de ruta correspondientes para formar la ruta de dirección URL. Dado que `id` es un parámetro de ruta opcional, no supone ningún problema que no tenga un valor.

Con los valores de ruta `{ controller = Home, action = Index }`, esta ruta genera la dirección URL `/`. Los valores de ruta que se proporcionaron coinciden con los valores predeterminados, por lo que los segmentos correspondientes con esos valores se pueden omitir sin ningún riesgo. Ambas direcciones URL generadas realizan un recorrido de ida y vuelta con esta definición de ruta y generan los mismos valores de ruta que se usaron para generar la dirección URL.

> [!TIP]
> Las aplicaciones con ASP.NET Core MVC deben usar <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> para generar direcciones URL en lugar de llamar directamente al enrutamiento.

Para obtener más información sobre la generación de direcciones URL, vea [url-generation-reference](#url-generation-reference).

## <a name="use-routing-middleware"></a>Uso de software intermedio de enrutamiento

::: moniker range=">= aspnetcore-2.1"

Haga referencia al [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) en el archivo de proyecto de la aplicación.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Haga referencia al [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage) en el archivo de proyecto de la aplicación.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Haga referencia a [Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/) en el archivo de proyecto de la aplicación.

::: moniker-end

Agregue enrutamiento al contenedor de servicios en `Startup.ConfigureServices`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](routing/samples/1.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

::: moniker-end

Las rutas se deben configurar en el método `Startup.Configure`. En la aplicación de ejemplo se usan estas API:

* `RouteBuilder`
* `Build`
* `MapGet` &ndash; solo coincide con solicitudes HTTP GET.
* `UseRouter`

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](routing/samples/1.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

::: moniker-end

En la tabla siguiente se muestran las respuestas con los URI especificados.

| Identificador URI                  | Respuesta                                          |
| -------------------- | ------------------------------------------------- |
| /package/create/3    | Hello! Valores de ruta: [operation, create], [id, 3] |
| /package/track/-3    | Hello! Valores de ruta: [operation, track], [id, -3] |
| /package/track/-3/   | Hello! Valores de ruta: [operation, track], [id, -3] |
| /package/track/      | &lt;Pasar explícitamente, ninguna coincidencia&gt;                    |
| GET /hello/Joe       | Hi, Joe!                                          |
| POST /hello/Joe      | &lt;Pasar explícitamente, solo coincide con HTTP GET&gt;       |
| GET /hello/Joe/Smith | &lt;Pasar explícitamente, ninguna coincidencia&gt;                    |

Si va a configurar una única ruta, pase una instancia de `IRouter` para llamar a <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>. No tendrá que usar <xref:Microsoft.AspNetCore.Routing.RouteBuilder>.

El marco de trabajo proporciona un conjunto de métodos de extensión para crear rutas, como los siguientes:

* `MapRoute`
* `MapGet`
* `MapPost`
* `MapPut`
* `MapDelete`
* `MapVerb`

Algunos de estos métodos, como `MapGet`, requieren que se proporcione un valor `RequestDelegate`. El valor `RequestDelegate` se usa como *controlador de ruta* cuando la ruta coincida. Otros métodos de esta familia permiten configurar una canalización de software intermedio para usarla como controlador de ruta. Si el método *Map* no acepta un controlador, como `MapRoute`, usa <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.

Los métodos `Map[Verb]` usan restricciones para limitar la ruta al verbo HTTP en el nombre del método. Por ejemplo, vea <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> y <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.

## <a name="route-template-reference"></a>Referencia de plantilla de ruta

Los tokens entre llaves (`{ ... }`) definen *parámetros de ruta* que se enlazan si se encuentran coincidencias con la ruta. Puede definir más de un parámetro de ruta en un segmento de ruta, pero deben estar separados por un valor literal. Por ejemplo, `{controller=Home}{action=Index}` no es una ruta válida, ya que no hay ningún valor literal entre `{controller}` y `{action}`. Estos parámetros de ruta deben tener un nombre y, opcionalmente, atributos adicionales especificados.

El texto literal diferente de los parámetros de ruta (por ejemplo, `{id}`) y el separador de ruta `/` deben coincidir con el texto de la dirección URL. La coincidencia de texto no distingue mayúsculas de minúsculas y se basa en la representación descodificada de la ruta de las direcciones URL. Para que el delimitador de parámetro de ruta literal `{` o `}` coincida, repita el carácter (`{{` o `}}`) para usarlo como secuencia de escape.

Es necesario tener en cuenta otras consideraciones en el caso de los patrones de dirección URL que intentan capturar un nombre de archivo con una extensión de archivo opcional. Por ejemplo, considere la plantilla `files/{filename}.{ext?}`. Cuando existen `filename` y `ext`, ambos valores se rellenan. Si solo existe `filename` en la dirección URL, la ruta coincide porque el punto final `.` es opcional. Las direcciones URL siguientes coinciden con esta ruta:

* `/files/myFile.txt`
* `/files/myFile`

Se puede usar el carácter `*` como prefijo de un parámetro de ruta para enlazar con el resto del URI. Es lo que se denomina un parámetro *comodín*. Por ejemplo, `blog/{*slug}` coincide con cualquier URI que empiece por `/blog` y que vaya seguido de cualquier valor (que se asigna al valor de ruta `slug`). Los parámetros comodín también pueden coincidir con una cadena vacía.

::: moniker range=">= aspnetcore-2.2"

El parámetro catch-all inserta los caracteres de escape correspondientes cuando se usa la ruta para generar una dirección URL, incluidos caracteres de separación de ruta de acceso (`/`). Por ejemplo, la ruta `foo/{*path}` con valores de ruta `{ path = "my/path" }` genera `foo/my%2Fpath`. Tenga en cuenta la barra diagonal de escape. Para los caracteres separadores de ruta de acceso de ida y vuelta, use el prefijo de parámetro de ruta `**`. La ruta `foo/{**path}` con `{ path = "my/path" }` genera `foo/my/path`.

::: moniker-end

Los parámetros de ruta pueden tener *valores predeterminados*. Para designar un valor predeterminado, se especifica después del nombre de parámetro, separado por un signo igual (`=`). Por ejemplo, `{controller=Home}` define `Home` como el valor predeterminado de `controller`. El valor predeterminado se usa si no hay ningún valor en la dirección URL para el parámetro. Además de los valores predeterminados, los parámetros de ruta pueden ser opcionales (para especificarlos, se anexa un signo de interrogación (`?`) al final del nombre del parámetro, como en `id?`). La diferencia entre los valores opcionales y los parámetros de ruta predeterminados es que un parámetro de ruta con un valor predeterminado siempre genera un valor, mientras que un parámetro opcional tiene un valor solo cuando la dirección URL de solicitud le proporciona uno.

::: moniker range=">= aspnetcore-2.2"

Los parámetros de ruta pueden tener restricciones, que deben coincidir con el valor de ruta enlazado desde la dirección URL. Si se agrega un carácter de dos puntos (`:`) y un nombre de restricción después del nombre del parámetro de ruta, se especifica una *restricción insertada* en un parámetro de ruta. Si la restricción requiere argumentos, se incluyen entre paréntesis `( )` después del nombre de restricción. Se pueden especificar varias restricciones insertadas si se anexa otro carácter de dos puntos (`:`) y un nombre de restricción. El nombre de restricción y los argumentos se pasan al servicio <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> para crear una instancia de <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> para su uso en el procesamiento de direcciones URL. Si el constructor de restricción requiere servicios, se resuelven a partir de los servicios de aplicación de la inserción de dependencias. Por ejemplo, la plantilla de ruta `blog/{article:minlength(10)}` especifica una restricción `minlength` con el argumento `10`. Para obtener más información sobre las restricciones de ruta y una lista de las restricciones proporcionadas por el marco de trabajo, vea la sección [Referencia de restricciones de ruta](#route-constraint-reference).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Los parámetros de ruta pueden tener restricciones, que deben coincidir con el valor de ruta enlazado desde la dirección URL. Si se agrega un carácter de dos puntos (`:`) y un nombre de restricción después del nombre del parámetro de ruta, se especifica una *restricción insertada* en un parámetro de ruta. Si la restricción requiere argumentos, se incluyen entre paréntesis `( )` después del nombre de restricción. Se pueden especificar varias restricciones insertadas si se anexa otro carácter de dos puntos (`:`) y un nombre de restricción. El nombre de restricción y los argumentos se pasan al servicio <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> para crear una instancia de <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> para su uso en el procesamiento de direcciones URL. Por ejemplo, la plantilla de ruta `blog/{article:minlength(10)}` especifica una restricción `minlength` con el argumento `10`. Para obtener más información sobre las restricciones de ruta y una lista de las restricciones proporcionadas por el marco de trabajo, vea la sección [Referencia de restricciones de ruta](#route-constraint-reference).

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Los parámetros de ruta también pueden tener transformadores de parámetros, que transforman el valor de un parámetro al generar vínculos y acciones y páginas coincidentes para los URI. Al igual que las restricciones, los transformadores de parámetros se pueden agregar en línea a un parámetro de ruta al incorporar un carácter de dos puntos (`:`) y un nombre de transformador después del nombre del parámetro de ruta. Por ejemplo, la plantilla de ruta `blog/{article:slugify}` especifica un transformador `slugify`.

::: moniker-end

En la tabla siguiente se muestran algunas plantillas de ruta y su comportamiento.

| Plantilla de ruta                         | Dirección URL coincidente de ejemplo  | Notas                                                                  |
| -------------------------------------- | --------------------- | ---------------------------------------------------------------------- |
| hello                                  | /hello                | Solo coincide con la ruta de acceso única `/hello`                                  |
| {Page=Home}                            | /                     | Coincide y establece `Page` en `Home`                                      |
| {Page=Home}                            | /Contact              | Coincide y establece `Page` en `Contact`                                   |
| {controller}/{action}/{id?}            | /Products/List        | Se asigna al controlador `Products` y la acción `List`                       |
| {controller}/{action}/{id?}            | /Products/Details/123 |  Se asigna al controlador `Products` y la acción `Details`.  `id` se establece en 123 |
| {controller=Home}/{action=Index}/{id?} | /                     |  Se asigna al controlador `Home` y el método `Index`; `id` se omite       |

El uso de una plantilla suele ser el método de enrutamiento más sencillo. Las restricciones y los valores predeterminados también se pueden especificar fuera de la plantilla de ruta.

> [!TIP]
> Habilite el [registro](xref:fundamentals/logging/index) para ver de qué forma las implementaciones de enrutamiento integradas, como `Route`, coinciden con las solicitudes.

## <a name="reserved-routing-names"></a>Nombres de enrutamientos reservados

Las siguientes palabras clave son nombres reservados y no se pueden usar como nombres de ruta o parámetros:

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Referencia de restricción de ruta

Las restricciones de ruta se ejecutan cuando un valor `Route` ha coincidido con la sintaxis de la dirección URL entrante y ha convertido la ruta de dirección URL en valores de ruta. En general, las restricciones de ruta inspeccionan el valor de ruta asociado a través de la plantilla de ruta y deciden si el valor es aceptable o no. Algunas restricciones de ruta usan datos ajenos al valor de ruta para decidir si la solicitud se puede enrutar. Por ejemplo, <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> puede aceptar o rechazar una solicitud basada en su verbo HTTP.

> [!WARNING]
> Evite el uso de restricciones para la **validación de entradas**, porque si lo hace, la entrada no válida producirá un error *404 - No encontrado* en lugar de un error *400 - Solicitud incorrecta* con el mensaje de error adecuado. Las restricciones de ruta se usan para **eliminar la ambigüedad** entre rutas similares, no para validar las entradas de una ruta determinada.

En la tabla siguiente se muestran algunas restricciones de ruta y su comportamiento esperado.

| restricción | Ejemplo | Coincidencias de ejemplo | Notas |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789`  | Coincide con cualquier entero |
| `bool` | `{active:bool}` | `true`, `FALSE` | Coincide con `true` o `false` (no distingue mayúsculas de minúsculas) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm`  | Coincide con un valor `DateTime` válido (en la referencia cultural invariable; vea la advertencia) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Coincide con un valor `decimal` válido (en la referencia cultural invariable; vea la advertencia) |
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | Coincide con un valor `double` válido (en la referencia cultural invariable; vea la advertencia) |
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | Coincide con un valor `float` válido (en la referencia cultural invariable; vea la advertencia) |
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Coincide con un valor `Guid` válido |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Coincide con un valor `long` válido |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | La cadena debe tener al menos cuatro caracteres |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | La cadena no debe tener más de ocho caracteres |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | La cadena debe tener una longitud de exactamente 12 caracteres |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | La cadena debe tener una longitud como mínimo de ocho caracteres y como máximo de 16 |
| `min(value)` | `{age:min(18)}` | `19` | El valor entero debe ser como mínimo 18 |
| `max(value)` | `{age:max(120)}` | `91` | El valor entero debe ser como máximo 120 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | El valor entero debe ser como mínimo 18 y máximo 120 |
| `alpha` | `{name:alpha}` | `Rick` | La cadena debe constar de uno o más caracteres alfabéticos (`a`-`z`, no distingue mayúsculas de minúsculas) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | La cadena debe coincidir con la expresión regular (vea las sugerencias sobre cómo definir una expresión regular) |
| `required` | `{name:required}` | `Rick` |  Se usa para exigir que un valor que no es de parámetro esté presente durante la generación de dirección URL |

Es posible aplicar varias restricciones delimitadas por dos puntos a un único parámetro. Por ejemplo, la siguiente restricción permite limitar un parámetro a un valor entero de 1 o superior:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> Las restricciones de ruta que comprueban la dirección URL y que se convierten en un tipo CLR (como `int` o `DateTime`) usan siempre la referencia cultural invariable. Estas restricciones dan por supuesto que la dirección URL no es localizable. Las restricciones de ruta proporcionadas por el marco de trabajo no modifican los valores almacenados en los valores de ruta. Todos los valores de ruta analizados desde la dirección URL se almacenan como cadenas. Por ejemplo, la restricción `float` intenta convertir el valor de ruta en un valor Float, pero el valor convertido se usa exclusivamente para comprobar que se puede convertir en Float.

## <a name="regular-expressions"></a>Expresiones regulares

El marco de trabajo de ASP.NET Core agrega `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` al constructor de expresiones regulares. Vea <xref:System.Text.RegularExpressions.RegexOptions> para obtener una descripción de estos miembros.

Las expresiones regulares usan delimitadores y tokens similares a los que usan el enrutamiento y el lenguaje C#. Es necesario usar secuencias de escape con los tokens de expresiones regulares. Para usar la expresión regular `^\d{3}-\d{2}-\d{4}$` en el enrutamiento, es necesario escribir los caracteres `\` de la expresión como `\\` en el archivo de código fuente de C# para que el carácter de escape de cadena `\` tenga una secuencia de escape (a menos que se usen [literales de cadena textuales](/dotnet/csharp/language-reference/keywords/string)). Es necesario incluir una secuencia de escape en los caracteres `{`, `}`, `[` y `]`. Para ello, duplíquelos a fin de incluir una secuencia de escape en los caracteres delimitadores del parámetro de enrutamiento. En la tabla siguiente se muestra una expresión regular y la versión con una secuencia de escape.

| Expresión            | Con secuencia de escape                        |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Las expresiones regulares que se usan en el enrutamiento suelen empezar con el carácter `^` (coincidencia con la posición inicial de la cadena) y terminar con el carácter `$` (coincidencia con la posición final de la cadena). Los caracteres `^` y `$` garantizan que la expresión regular coincide con el valor completo del parámetro de ruta. Sin los caracteres `^` y `$`, la expresión regular coincide con cualquier subcadena de la cadena, algo que normalmente no es deseable. En la tabla siguiente se muestran algunos ejemplos y se explica por qué que coinciden o no.

| Expresión   | String    | Coincidir con | Comentario               |
| ------------ | --------- |  ---- |  -------------------- |
| `[a-z]{2}`   | hello     | Sí   | Coincidencias de subcadenas     |
| `[a-z]{2}`   | 123abc456 | Sí   | Coincidencias de subcadenas     |
| `[a-z]{2}`   | mz        | Sí   | Coincide con la expresión    |
| `[a-z]{2}`   | MZ        | Sí   | No distingue mayúsculas de minúsculas    |
| `^[a-z]{2}$` |  hello    | No    | Vea `^` y `$` más arriba |
| `^[a-z]{2}$` | 123abc456 | No    | Vea `^` y `$` más arriba |

Para obtener más información sobre la sintaxis de expresiones regulares, vea [Expresiones regulares de .NET Framework](/dotnet/standard/base-types/regular-expression-language-quick-reference).

Para restringir un parámetro a un conjunto conocido de valores posibles, use una expresión regular. Por ejemplo, `{action:regex(^(list|get|create)$)}` solo hace coincidir el valor de ruta `action` con `list`, `get` o `create`. Si se pasa al diccionario de restricciones, la cadena `^(list|get|create)$` es equivalente. Las restricciones que se pasan al diccionario de restricciones (no insertado en una plantilla) que no coinciden con una de las restricciones conocidas también se tratan como expresiones regulares.

::: moniker range=">= aspnetcore-2.2"

## <a name="parameter-transformer-reference"></a>Referencia de transformadores de parámetros

Transformadores de parámetros:

* Se ejecutan al generar un vínculo para `Route`.
* Implemente `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`.
* Se configuran con <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>.
* Toman el valor de ruta del parámetro y lo transforman en un nuevo valor de cadena.
* El valor transformado se usa en el vínculo generado.

Por ejemplo, un transformador de parámetros personalizado `slugify` en el patrón de ruta `blog\{article:slugify}` con `Url.Action(new { article = "MyTestArticle" })` genera `blog\my-test-article`.

Los marcos también usan transformadores de parámetros para transformar el URI en el que se resuelve un punto de conexión. Por ejemplo, ASP.NET Core MVC usa transformadores de parámetros para transformar el valor de ruta usado para hacer coincidir elementos `area`, `controller`, `action` y `page`.

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home:slugify}/{action=Index:slugify}/{id?}");
```

Con la ruta anterior, la acción `SubscriptionManagementController.GetAll()` coincide con el URI `/subscription-management/get-all`. Un transformador de parámetros no cambia los valores de ruta usados para generar un vínculo. `Url.Action("GetAll", "SubscriptionManagement")` genera `/subscription-management/get-all`.

ASP.NET Core proporciona las convenciones de API para usar transformadores de parámetros con rutas generadas:

* ASP.NET Core MVC tiene la convención de API `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention`. Esta convención aplica un transformador de parámetro especificado a todas las rutas de atributo de la aplicación. El transformador de parámetro transforma los tokens de rutas de atributo a medida que se reemplazan. Para más información, consulte [Usar un transformador de parámetro para personalizar el reemplazo de tokens](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).
* Razor Pages tiene la convención de API `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention`. Esta convención aplica un transformador de parámetro especificado a todas las instancias de Razor Pages descubiertas automáticamente. El transformador de parámetro transforma los segmentos nombre de archivo y carpeta de las rutas de Razor Pages. Para más información, consulte el artículo sobre cómo [usar un transformador de parámetro para personalizar rutas de Razor Pages](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).

::: moniker-end

## <a name="url-generation-reference"></a>Referencia de generación de direcciones URL

En el ejemplo siguiente se muestra cómo se genera un vínculo a una ruta, dado un diccionario de valores de ruta y un valor <xref:Microsoft.AspNetCore.Routing.RouteCollection>.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](routing/samples/1.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

::: moniker-end

El valor `VirtualPath` generado al final del ejemplo anterior es `/package/create/123`. El diccionario proporciona los valores de ruta `operation` e `id` de la plantilla "Ruta de paquete de seguimiento", `package/{operation}/{id}`. Para obtener más información, vea el código de ejemplo de la sección [Uso de software intermedio de enrutamiento](#use-routing-middleware) o la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/samples).

El segundo parámetro del constructor `VirtualPathContext` es una colección de *valores de ambiente*. Los valores de ambiente aportan comodidad al limitar el número de valores que el desarrollador debe especificar dentro de un determinado contexto de solicitud. Los valores de ruta actuales de la solicitud actual se consideran valores de ambiente para la generación de vínculos. Si en una aplicación ASP.NET Core MVC se encuentra en la acción `About` de `HomeController`, no es necesario especificar el valor de ruta de controlador para vincular a la acción `Index` (se usará el valor de ambiente `Home`).

Los valores de ambiente se omiten cuando no coinciden con un parámetro y cuando un valor proporcionado explícitamente lo invalida al ir de izquierda a derecha en la dirección URL.

Los valores que se proporcionan explícitamente pero que no coinciden con nada se agregan a la cadena de consulta. En la tabla siguiente se muestra el resultado cuando se usa la plantilla de ruta `{controller}/{action}/{id?}`.

| Valores de ambiente                | Valores explícitos                   | Resultado                  |
| ----------------------------- | --------------------------------- | ----------------------- |
| controller="Home"             | action="About"                    | `/Home/About`           |
| controller="Home"             | controller="Order",action="About" | `/Order/About`          |
| controller="Home",color="Red" | action="About"                    | `/Home/About`           |
| controller="Home"             | action="About",color="Red"        | `/Home/About?color=Red` |

Si una ruta tiene un valor predeterminado que no se corresponde con un parámetro y ese valor se proporciona de forma explícita, debe coincidir con el valor predeterminado:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

La generación de vínculos solo genera un vínculo para esta ruta si se proporcionan los valores coincidentes para el controlador y la acción.
