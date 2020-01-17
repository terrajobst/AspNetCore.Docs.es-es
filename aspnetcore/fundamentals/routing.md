---
title: Enrutamiento en ASP.NET Core
author: rick-anderson
description: Descubra cómo el enrutamiento de ASP.NET Core es responsable de asignar URI de solicitud a los selectores de punto de conexión y de distribuir las solicitudes entrantes a los puntos de conexión.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/13/2019
uid: fundamentals/routing
ms.openlocfilehash: 9780183f8f9bc322f73d058b3cab7f8c10f7cd5f
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75354740"
---
# <a name="routing-in-aspnet-core"></a>Enrutamiento en ASP.NET Core

Por [Ryan Nowak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/) y [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

El enrutamiento es responsable de asignar URI de solicitud a los puntos de conexión y de distribuir las solicitudes entrantes a esos puntos de conexión. Las rutas se definen en la aplicación y se configuran cuando se inicia la aplicación. Una ruta puede extraer opcionalmente valores de la dirección URL contenida en la solicitud, que se pueden usar para procesar las solicitudes. Con la información de ruta de la aplicación, el enrutamiento también puede generar direcciones URL que se asignan a puntos de conexión. Muchas aplicaciones no necesitan agregar rutas más allá de lo que proporcionan las plantillas. Las plantillas de ASP.NET Core para controladores y Razor Pages configuran puntos de conexión de ruta. Si necesita agregar puntos de conexión de ruta personalizados, los puntos de conexión personalizados se pueden configurar junto con los puntos de conexión de ruta generados por la plantilla.

> [!IMPORTANT]
> En este documento se describe el enrutamiento de ASP.NET Core de bajo nivel. Para obtener información sobre el enrutamiento de ASP.NET Core MVC, vea <xref:mvc/controllers/routing>. Para obtener más información sobre las convenciones de enrutamiento en Razor Pages, consulte <xref:razor-pages/razor-pages-conventions>.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="routing-basics"></a>Fundamentos del enrutamiento

La mayoría de las aplicaciones deben elegir un esquema de enrutamiento básico y descriptivo para que las direcciones URL sean legibles y significativas. La ruta convencional predeterminada `{controller=Home}/{action=Index}/{id?}`:

* Admite un esquema de enrutamiento básico y descriptivo.
* Se trata de un punto de partida útil para las aplicaciones basadas en la interfaz de usuario.

Los desarrolladores suelen agregar rutas breves adicionales a áreas de mucho tráfico de una aplicación en situaciones especializadas (por ejemplo, puntos de conexión de blog y comercio electrónico) con el [enrutamiento mediante atributos](xref:mvc/controllers/routing#attribute-routing) o rutas convencionales dedicadas.

Las API web deben usar el enrutamiento mediante atributos para modelar la funcionalidad de la aplicación como un conjunto de recursos donde las operaciones se representan mediante verbos HTTP. Esto significa que muchas operaciones (por ejemplo, GET y POST) del mismo recurso lógico usarán la misma dirección URL. El enrutamiento mediante atributos proporciona un nivel de control que es necesario para diseñar cuidadosamente un diseño de puntos de conexión públicos de la API.

En las aplicaciones de Razor Pages se usa el enrutamiento convencional predeterminado para proporcionar recursos con nombre en la carpeta *Páginas* de una aplicación. Existen convenciones adicionales que permiten personalizar el comportamiento de enrutamiento de Razor Pages. Para obtener más información, vea <xref:razor-pages/index> y <xref:razor-pages/razor-pages-conventions>.

La compatibilidad de la generación de direcciones URL permite desarrollar la aplicación sin codificar de forma rígida las direcciones URL para vincular la aplicación. Esta compatibilidad permite empezar con una configuración de enrutamiento básica y modificar las rutas una vez determinado el diseño de los recursos de la aplicación.

El enrutamiento usa *puntos de conexión* (`Endpoint`) para representar los puntos de conexión lógicos en una aplicación.

Un punto de conexión define un delegado para procesar las solicitudes y una colección de metadatos arbitrarios. Los metadatos se usan para implementar cuestiones transversales según las directivas y la configuración asociada a cada punto de conexión.

El sistema de enrutamiento tiene las características siguientes:

* La sintaxis de plantilla de ruta se usa para definir las rutas con parámetros de ruta con tokens.
* Se permite la configuración de puntos de conexión de estilo convencional y de estilo de atributo.
* Se usa <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> para determinar si un parámetro de dirección URL contiene un valor válido para una restricción de punto de conexión determinada.
* Los modelos de aplicación, como MVC y Razor Pages, registran todos sus puntos de conexión, que presentan una implementación predecible de los escenarios de enrutamiento.
* La implementación de enrutamiento toma decisiones relativas al enrutamiento siempre que sea lo deseado en la canalización de middleware.
* El middleware que aparece después de un middleware de enrutamiento puede inspeccionar el resultado de la decisión del punto de conexión del middleware de enrutamiento para un URI de solicitud determinado.
* Se pueden enumerar todos los puntos de conexión de la aplicación en cualquier parte de la canalización de middleware.
* Una aplicación puede usar el enrutamiento para generar direcciones URL (por ejemplo, para el redireccionamiento o los vínculos) en función de la información del punto de conexión. De este modo, se evita codificar de forma rígida las direcciones URL, lo que facilita el mantenimiento.
* La generación de direcciones URL se basa en direcciones, que admiten la extensibilidad arbitraria:

  * La API del generador de vínculos (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>) se puede resolver en cualquier lugar mediante la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection) para generar direcciones URL.
  * Cuando la API del generador de vínculos no está disponible a través de DI, <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> ofrece métodos para generar direcciones URL.

> [!NOTE]
> La vinculación de puntos de conexión está limitada a las acciones y las páginas de MVC y Razor Pages. Las expansiones de las funciones de vinculación de punto de conexión están previstas para próximas versiones.

La clase <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> conecta el enrutamiento a la canalización de [software intermedio](xref:fundamentals/middleware/index). [ASP.NET Core MVC](xref:mvc/overview) agrega enrutamiento a la canalización de middleware como parte de su configuración y controla el enrutamiento en las aplicaciones de MVC y Razor Pages. Para obtener información sobre cómo usar el enrutamiento como componente independiente, vea la sección [Uso de software intermedio de enrutamiento](#use-routing-middleware).

### <a name="url-matching"></a>Coincidencia de dirección URL

La coincidencia de dirección URL es el proceso por el cual el enrutamiento envía una solicitud entrante a un *punto de conexión*. Este proceso se basa en datos de la ruta de dirección URL, pero se puede ampliar para tener en cuenta cualquier dato de la solicitud. La capacidad de enviar solicitudes a controladores independientes es clave para escalar el tamaño y la complejidad de una aplicación.

Cuando se ejecuta un middleware de enrutamiento, se establece un punto de conexión (`Endpoint`) y se enrutan los valores a una característica de <xref:Microsoft.AspNetCore.Http.HttpContext>. Para la solicitud actual:

* Al llamar a `HttpContext.GetEndpoint` se obtiene el punto de conexión.
* `HttpRequest.RouteValues` obtiene la colección de valores de ruta.

El middleware que se ejecuta después del middleware de enrutamiento puede ver el punto de conexión y tomar medidas. Por ejemplo, un middleware de autorización puede consultar la colección de metadatos del punto de conexión de una directiva de autorización. Después de que se ejecuta todo el middleware en la canalización de procesamiento de solicitudes, se invoca al delegado del punto de conexión seleccionado.

El sistema de enrutamiento en el enrutamiento de punto de conexión es responsable de todas las decisiones relativas al envío. Como el middleware aplica las directivas en función del punto de conexión seleccionado, es importante que cualquier decisión que pueda afectar a la distribución o la aplicación de directivas de seguridad se realice dentro del sistema de enrutamiento.

Cuando se ejecuta el delegado del punto de conexión, las propiedades de [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) se establecen en los valores adecuados en función del procesamiento de solicitudes realizado hasta el momento.

[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) es un diccionario de los *valores de ruta* generados desde la ruta. Estos valores se suelen determinar mediante la conversión en tokens de la dirección URL, y se pueden usar para aceptar la entrada del usuario o para tomar otras decisiones sobre el envío dentro de la aplicación.

[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) es un contenedor de propiedades de datos adicionales relacionados con la ruta coincidente. Se proporcionan <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> para permitir la asociación de datos de estado con cada ruta, de modo que la aplicación pueda tomar decisiones en función de las rutas que han coincidido. Estos valores los define el desarrollador y **no** afectan de ninguna manera al comportamiento del enrutamiento. Además, los valores que se guardan provisionalmente en [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) pueden ser de cualquier tipo, a diferencia de [RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values), que deben poder convertirse fácilmente en cadenas y a partir de estas.

[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) es una lista de las rutas que han participado en encontrar una coincidencia correcta con la solicitud. Las rutas se pueden anidar unas dentro de otras. La propiedad <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> refleja la ruta de acceso del árbol lógico de rutas que han tenido como resultado una coincidencia. Por lo general, el primer elemento de <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> es la colección de rutas y se debe usar para la generación de direcciones URL. El último elemento de <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> es el controlador de ruta que ha coincidido.

<a name="lg"></a>

### <a name="url-generation-with-linkgenerator"></a>Generación de direcciones URL con LinkGenerator

La generación de dirección URL es el proceso por el cual el enrutamiento puede crear una ruta de dirección URL basada en un conjunto de valores de ruta. Esto permite una separación lógica entre los puntos de conexión y las direcciones URL que tienen acceso a ellos.

El enrutamiento de punto de conexión incluye la API del generador de vínculos (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>). <xref:Microsoft.AspNetCore.Routing.LinkGenerator> es un servicio singleton que se puede recuperar a partir de la DI. La API se puede usar fuera del contexto de una solicitud en ejecución. <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> de MVC y los escenarios que dependen de <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>, como los [asistentes de etiquetas](xref:mvc/views/tag-helpers/intro), los de HTML y [los resultados de acción](xref:mvc/controllers/actions), usan el generador de vínculos para proporcionar funciones de generación de vínculos.

El generador de vínculos está respaldado por el concepto de una *dirección* y *esquemas de direcciones*. Un esquema de direcciones es una manera de determinar los puntos de conexión que se deben tener en cuenta para la generación de vínculos. Por ejemplo, los escenarios de nombre y valores de ruta de Razor Pages y MVC con los que muchos usuarios están familiarizados se implementan como un esquema de direcciones.

El generador de vínculos puede vincular a acciones y páginas de Razor Pages y MVC a través de los métodos de extensión siguientes:

* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*>
* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetPathByPage*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetUriByPage*>

Una sobrecarga de estos métodos acepta argumentos que incluyan `HttpContext`. Estos métodos son equivalentes funcionalmente a `Url.Action` y `Url.Page`, pero ofrecen flexibilidad y opciones adicionales.

Los métodos `GetPath*` son más similares a `Url.Action` y `Url.Page`, dado que generan un URI que contiene una ruta de acceso absoluta. Los métodos `GetUri*` siempre generan un URI absoluto que contiene un esquema y un host. Los métodos que aceptan `HttpContext` generan un URI en el contexto de la solicitud que se ejecuta. A menos que se reemplacen, se usan los valores de ruta de ambiente, la ruta de acceso base de la dirección URL, el esquema y el host de la solicitud que se ejecuta.

Se llama a <xref:Microsoft.AspNetCore.Routing.LinkGenerator> con una dirección. La generación de un URI se produce en dos pasos:

1. Se enlaza una dirección a una lista de puntos de conexión que coincidan con la dirección.
1. Se evalúa el elemento `RoutePattern` de cada punto de conexión hasta que se encuentra un patrón de ruta que coincida con los valores proporcionados. La salida resultante se combina con otras partes del URI proporcionadas al generador de vínculos y devueltas.

Los métodos proporcionados por <xref:Microsoft.AspNetCore.Routing.LinkGenerator> admiten funciones estándar de generación de vínculos para cualquier tipo de dirección. La forma más útil de usar el generador de vínculos es a través de métodos de extensión que realicen operaciones para un tipo de dirección específica.

| Método de extensión | Descripción |
| ---------------- | ----------- |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*> | Genera un URI con una ruta de acceso absoluta en función de los valores proporcionados. |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetUriByAddress*> | Genera un URI absoluto en función de los valores proporcionados.             |

> [!WARNING]
> Preste atención a las consecuencias siguientes de llamar a los métodos <xref:Microsoft.AspNetCore.Routing.LinkGenerator>:
>
> * Use los métodos de extensión `GetUri*` con precaución en una configuración de aplicación en la que no se valide el encabezado `Host` de las solicitudes entrantes. Si no se valida el encabezado `Host` de las solicitudes entrantes, la entrada de la solicitud que no sea de confianza se puede devolver al cliente en los URI de una página o vista. Se recomienda que todas las aplicaciones de producción configuren su servidor para validar el encabezado `Host` en función de valores válidos conocidos.
>
> * Use <xref:Microsoft.AspNetCore.Routing.LinkGenerator> con precaución en el middleware junto con `Map` o `MapWhen`. `Map*` cambia la ruta de acceso base de la solicitud que se ejecuta, lo que afecta a la salida de la generación de vínculos. Todas las API de <xref:Microsoft.AspNetCore.Routing.LinkGenerator> permiten especificar una ruta de acceso base. Especifique siempre una ruta de acceso base vacía para deshacer el efecto de `Map*` en la generación de vínculos.

## <a name="endpoint-routing"></a>Enrutamiento de puntos de conexión

* Un punto de conexión de ruta tiene una plantilla, metadatos y un delegado de solicitud que sirve a la respuesta del punto de conexión. Los metadatos se usan para implementar cuestiones transversales según las directivas y la configuración asociada a cada punto de conexión. Por ejemplo, un middleware de autorización puede consultar la colección de metadatos del punto de conexión de una [directiva de autorización](xref:security/authorization/policies#applying-policies-to-mvc-controllers).
* El enrutamiento de puntos de conexión se integra con middleware mediante dos métodos de extensión:
  * [UseRouting](xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*) agrega coincidencia de rutas a la canalización de middleware. Debe ser anterior a cualquier middleware que tenga en cuenta la ruta, como la autorización, la ejecución del punto de conexión, etc.
  * [UseEndpoints](xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*) agrega la ejecución del punto de conexión a la canalización de middleware. Ejecuta el delegado de solicitud que sirve a la respuesta del punto de conexión.
  `UseEndpoints` también es donde se configuran los puntos de conexión de ruta que la aplicación puede ejecutar y hacer coincidir. Por ejemplo, <xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapRazorPages*>, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*>, <xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapGet*> y <xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapPost*>.
* Las aplicaciones usan los métodos auxiliares de ASP.NET Core para configurar sus rutas. Los marcos de ASP.NET Core proporcionan métodos auxiliares como <xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapRazorPages*>, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> y `MapHub<THub>`. También hay métodos auxiliares para configurar sus propios puntos de conexión de ruta personalizados: <xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapGet*>, <xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapPost*> y [MapVerb](xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions). 
* El enrutamiento de puntos de conexión también admite puntos de conexión que cambian después de que se haya iniciado una aplicación. Para que esto sea compatible con la aplicación o con el marco de ASP.NET Core, se debe crear y registrar un <xref:Microsoft.AspNetCore.Routing.EndpointDataSource> personalizado. Se trata de una característica avanzada y, por lo general, no es necesaria. Habitualmente, los puntos de conexión se configuran en el inicio y son estáticos durante la vigencia de la aplicación. La carga de la configuración de ruta desde un archivo o base de datos en el inicio no es dinámica.

En el código siguiente se muestra un ejemplo básico de enrutamiento de punto de conexión:

[!code-csharp[](routing/samples/3.x/Startup.cs?name=snippet)]

Consulte [Coincidencia de dirección URL](#url-matching) en este documento para más información sobre el enrutamiento de puntos de conexión.

## <a name="endpoint-routing-differences-from-earlier-versions-of-routing"></a>Diferencias de enrutamiento de puntos de conexión con respecto a versiones anteriores del enrutamiento

Existen algunas diferencias entre el enrutamiento de punto de conexión y las versiones anteriores del enrutamiento anteriores a ASP.NET Core 2.2:

* El sistema de enrutamiento de punto de conexión no es compatible con la extensibilidad basada en <xref:Microsoft.AspNetCore.Routing.IRouter>, incluida la herencia de <xref:Microsoft.AspNetCore.Routing.Route>.

* El enrutamiento de punto de conexión no es compatible con [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim). Utilice la [versión compatibilidad](xref:mvc/compatibility-version) 2.1 (`.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)`) para seguir usando la corrección de compatibilidad.

* El enrutamiento de punto de conexión tiene un comportamiento diferente para el uso de mayúsculas y minúsculas en los URI generados al usar rutas convencionales.

  Tenga en cuenta la plantilla de ruta predeterminada siguiente:

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  Supongamos que genera un vínculo a una acción mediante la ruta siguiente:

  ```csharp
  var link = Url.Action("ReadPost", "blog", new { id = 17, });
  ```

  Con el enrutamiento basado en <xref:Microsoft.AspNetCore.Routing.IRouter>, este código genera un URI de `/blog/ReadPost/17`, que respeta las mayúsculas y minúsculas del valor de ruta proporcionado. El enrutamiento de punto de conexión de ASP.NET Core 2.2 o versiones posteriores genera `/Blog/ReadPost/17` ("Blog" se pone mayúscula). El enrutamiento de punto de conexión proporciona la interfaz `IOutboundParameterTransformer`, que se puede usar para personalizar este comportamiento de forma global, o bien para aplicar otras convenciones para la asignación de direcciones URL.

  Para obtener más información, vea la sección [Referencia de transformadores de parámetros](#parameter-transformer-reference).

* La generación de vínculos que se usa en Razor Pages y MVC con las rutas convencionales tiene un comportamiento diferente al intentar vincularlo a un controlador o una acción, o a una página que no existe.

  Tenga en cuenta la plantilla de ruta predeterminada siguiente:

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  Supongamos que genera un vínculo a una acción mediante la plantilla predeterminada con lo siguiente:

  ```csharp
  var link = Url.Action("ReadPost", "Blog", new { id = 17, });
  ```

  Con el enrutamiento basado en `IRouter`, el resultado siempre es `/Blog/ReadPost/17`, incluso aunque `BlogController` no exista o no tenga un método de acción `ReadPost`. Como se esperaba, si el método de acción existe, el enrutamiento de punto de conexión de ASP.NET Core 2.2 o versiones posteriores genera `/Blog/ReadPost/17`. *Sin embargo, si la acción no existe, el enrutamiento de punto de conexión genera una cadena vacía.* Conceptualmente, el enrutamiento de punto de conexión no supone que, si la acción no existe, sí que exista el punto de conexión.

* El *algoritmo de invalidación del valor de ambiente* de la generación de vínculos tiene otro comportamiento al usarlo con el enrutamiento de punto de conexión.

  La *invalidación del valor de ambiente* es el algoritmo que decide qué valores de ruta de la solicitud que se ejecuta actualmente (los valores de ambiente) se pueden usar en las operaciones de generación de vínculos. El enrutamiento convencional siempre invalida los valores de ruta adicionales al vincular a otra acción. El enrutamiento mediante atributos no tenía este comportamiento antes del lanzamiento de ASP.NET Core 2.2. En versiones anteriores de ASP.NET Core, los vínculos a otra acción que usara los mismos nombres de parámetro de ruta producían errores de generación de vínculo. En ASP.NET Core 2.2 o versiones posteriores, las dos formas de enrutamiento invalidan los valores cuando se vincula a otra acción.

  Considere el ejemplo siguiente de ASP.NET Core 2.1 o una versión anterior. Al vincular a otra acción (o a otra página), los valores de ruta se pueden reutilizar de formas no deseadas.

  En */Pages/Store/Product.cshtml*:

  ```cshtml
  @page "{id}"
  @Url.Page("/Login")
  ```

  En */Pages/Login.cshtml*:

  ```cshtml
  @page "{id?}"
  ```

  Si el URI es `/Store/Product/18` en ASP.NET Core 2.1 o versiones anteriores, el vínculo que genera `@Url.Page("/Login")` en la página Store/Info es `/Login/18`. Se reutiliza el valor `id` de 18, aunque el destino del vínculo sea otro elemento totalmente distinto de la aplicación. El valor de ruta `id` en el contexto de la página `/Login` probablemente sea un valor de id. de usuario, no un valor de id. de producto de tienda.

  En el enrutamiento de punto de conexión con ASP.NET Core 2.2 o versiones posteriores, el resultado es `/Login`. Los valores de ambiente no se reutilizan cuando el destino vinculado es otra acción o página.

* Sintaxis de parámetro de ruta de ida y vuelta: las barras diagonales no se codifican cuando se usa una sintaxis de parámetro comodín de doble asterisco (`**`).

  Durante la generación de vínculos, el sistema de enrutamiento codifica el valor capturado en un parámetro comodín de doble asterisco (`**`; por ejemplo, `{**myparametername}`), excepto las barras diagonales. El comodín de doble asterisco es compatible con el enrutamiento basado en `IRouter` en ASP.NET Core 2.2 o versiones posteriores.

  La sintaxis de parámetro comodín de un único asterisco en versiones anteriores de ASP.NET Core (`{*myparametername}`) sigue siendo compatible y las barras diagonales se codifican.

  | Ruta              | Vínculo generado con<br>`Url.Action(new { category = "admin/products" })`&hellip; |
  | ------------------ | --------------------------------------------------------------------- |
  | `/search/{*page}`  | `/search/admin%2Fproducts` (la barra diagonal se codifica)             |
  | `/search/{**page}` | `/search/admin/products`                                              |

### <a name="middleware-example"></a>Ejemplo de middleware

En el ejemplo siguiente, un middleware usa la API <xref:Microsoft.AspNetCore.Routing.LinkGenerator> para crear el vínculo a un método de acción que enumera los productos de la tienda. El uso del generador de vínculos mediante su inserción en una clase y la llamada a `GenerateLink` está disponible para cualquier clase de una aplicación.

```csharp
using Microsoft.AspNetCore.Routing;

public class ProductsLinkMiddleware
{
    private readonly LinkGenerator _linkGenerator;

    public ProductsLinkMiddleware(RequestDelegate next, LinkGenerator linkGenerator)
    {
        _linkGenerator = linkGenerator;
    }

    public async Task InvokeAsync(HttpContext httpContext)
    {
        var url = _linkGenerator.GetPathByAction("ListProducts", "Store");

        httpContext.Response.ContentType = "text/plain";

        await httpContext.Response.WriteAsync($"Go to {url} to see our products.");
    }
}
```

### <a name="create-routes"></a>Creación de rutas

La mayoría de las aplicaciones crea rutas mediante una llamada a <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> o a uno de los métodos de extensión similares definidos en <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>. Todos los métodos de extensión <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> crean una instancia de <xref:Microsoft.AspNetCore.Routing.Route> y la agregan a la colección de rutas.

<xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> no acepta un parámetro de controlador de ruta. <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> solo agrega las rutas que se controlan mediante <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>. Para obtener más información sobre el enrutamiento en MVC, vea <xref:mvc/controllers/routing>.

El código siguiente es un ejemplo de una llamada a <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> usada por una definición de ruta típica de ASP.NET Core MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Esta plantilla coincide con una ruta de dirección URL y extrae los valores de ruta. Por ejemplo, la ruta de acceso `/Products/Details/17` genera los valores de ruta siguientes: `{ controller = Products, action = Details, id = 17 }`.

Los valores de ruta se determinan al dividir la ruta de dirección URL en segmentos y hacer coincidir cada segmento con el nombre del *parámetro de ruta* en la plantilla de ruta. Los parámetros de ruta tienen un nombre asignado. Para definir los parámetros, el nombre del parámetro se incluye entre llaves `{ ... }`.

La plantilla anterior también podría coincidir con la ruta de dirección URL `/` y generar los valores `{ controller = Home, action = Index }`. Esto se debe a que los parámetros de ruta `{controller}` y `{action}` tienen valores predeterminados y el parámetro de ruta `id` es opcional. Un signo igual (`=`) seguido de un valor después del nombre del parámetro de ruta define un valor predeterminado para el parámetro. Un signo de interrogación (`?`) después del nombre del parámetro de ruta define un parámetro opcional.

Los parámetros de ruta con un valor predeterminado *siempre* generan un valor de ruta cuando la ruta coincide. Los parámetros opcionales no generan un valor de ruta si no existió el segmento de ruta de dirección URL correspondiente. Vea la sección [Referencia de plantilla de ruta](#route-template-reference) para obtener una descripción detallada de los escenarios y la sintaxis de la plantilla de ruta.

En el ejemplo siguiente, la definición de parámetro de ruta `{id:int}` define una [restricción de ruta](#route-constraint-reference) para el parámetro de ruta `id`:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Esta plantilla coincide con una ruta de dirección URL como `/Products/Details/17`, pero no con `/Products/Details/Apples`. Las restricciones de ruta implementan <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> e inspeccionan los valores de ruta para comprobarlos. En este ejemplo, el valor de ruta `id` debe poder convertirse en un entero. Vea la [Referencia de restricción de ruta](#route-constraint-reference) para obtener una explicación de las restricciones de ruta proporcionadas por el marco de trabajo.

Las sobrecargas adicionales de <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> aceptan valores para `constraints`, `dataTokens` y `defaults`. Estos parámetros suelen usarse para pasar un objeto de tipo anónimo, donde los nombres de propiedad del tipo anónimo coinciden con los nombres de los parámetros de ruta.

En los ejemplos <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> siguientes se crean rutas equivalentes:

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
> La sintaxis insertada para definir las restricciones y los valores predeterminados puede ser conveniente para las rutas simples. Pero hay algunos escenarios, como los tokens de datos, que no son compatibles con la sintaxis insertada.

En el ejemplo siguiente se muestran algunos escenarios más:

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{**article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

La plantilla anterior coincide con una ruta de dirección URL como `/Blog/All-About-Routing/Introduction` y extrae los valores `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. La ruta genera los valores de ruta predeterminados para `controller` y `action`, incluso si no hay parámetros de ruta correspondientes en la plantilla. Los valores predeterminados pueden especificarse en la plantilla de ruta. El parámetro de ruta `article` se define como un *comodín* por la aparición de un asterisco doble (`**`) antes del nombre del parámetro de ruta. Los parámetros de ruta comodín capturan el resto de la ruta de dirección URL y también pueden coincidir con la cadena vacía.

En el ejemplo siguiente se agregan restricciones de ruta y tokens de datos:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

La plantilla anterior coincide con una ruta de dirección URL como `/en-US/Products/5` y extrae los valores `{ controller = Products, action = Details, id = 5 }` y los tokens de datos `{ locale = en-US }`.

![Tokens de Windows de variables locales](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a>Generación de direcciones URL de clase de ruta

La clase <xref:Microsoft.AspNetCore.Routing.Route> también puede llevar a cabo la generación de dirección URL mediante la combinación de un conjunto de valores de ruta con su plantilla de ruta. Se trata lógicamente del proceso inverso de hacer coincidir la ruta de dirección URL.

> [!TIP]
> Para entender mejor la generación de direcciones URL, imagine qué dirección URL quiere generar y, después, piense cómo coincidiría una plantilla de ruta con esa dirección URL. ¿Qué valores se generarían? Este es un equivalente aproximado de cómo funciona la generación de dirección URL en la clase <xref:Microsoft.AspNetCore.Routing.Route>.

En el ejemplo siguiente se usa una ruta predeterminada de ASP.NET Core MVC general:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Con los valores de ruta `{ controller = Products, action = List }`, se genera la dirección URL `/Products/List`. Los valores de ruta se sustituyen por los parámetros de ruta correspondientes para formar la ruta de dirección URL. Como `id` es un parámetro de ruta opcional, la dirección URL se ha generado correctamente sin ningún valor para `id`.

Con los valores de ruta `{ controller = Home, action = Index }`, se genera la dirección URL `/`. Los valores de ruta proporcionados coinciden con los valores predeterminados, y los segmentos correspondientes a los valores predeterminados se omiten sin ningún riesgo.

Ambas direcciones URL generadas realizan un recorrido de ida y vuelta con la definición de ruta siguiente (`/Home/Index` y `/`) y generan los mismos valores de ruta que se usaron para generar la dirección URL.

> [!NOTE]
> Las aplicaciones con ASP.NET Core MVC deben usar <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> para generar direcciones URL en lugar de llamar directamente al enrutamiento.

Para obtener más información sobre la generación de direcciones URL, vea la sección [Referencia de generación de direcciones URL](#url-generation-reference).

## <a name="use-routing-middleware"></a>Uso de software intermedio de enrutamiento

Haga referencia al [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) en el archivo de proyecto de la aplicación.

Agregue enrutamiento al contenedor de servicios en `Startup.ConfigureServices`:

[!code-csharp[](routing/samples/3.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Las rutas se deben configurar en el método `Startup.Configure`. En la aplicación de ejemplo se usan las API siguientes:

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; solo coincide con solicitudes HTTP GET.
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/3.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

En la tabla siguiente se muestran las respuestas con los URI especificados.

| Identificador URI                    | Respuesta                                          |
| ---------------------- | ------------------------------------------------- |
| `/package/create/3`    | Hello! Valores de ruta: [operation, create], [id, 3] |
| `/package/track/-3`    | Hello! Valores de ruta: [operation, track], [id, -3] |
| `/package/track/-3/`   | Hello! Valores de ruta: [operation, track], [id, -3] |
| `/package/track/`      | La solicitud pasa; no hay ninguna coincidencia.              |
| `GET /hello/Joe`       | Hi, Joe!                                          |
| `POST /hello/Joe`      | La solicitud pasa; solo coincide con HTTP GET. |
| `GET /hello/Joe/Smith` | La solicitud pasa; no hay ninguna coincidencia.              |

El marco de trabajo proporciona un conjunto de métodos de extensión para crear rutas (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>):

* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareVerb*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>

Los métodos `Map[Verb]` usan restricciones para limitar la ruta al verbo HTTP en el nombre del método. Por ejemplo, vea <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> y <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.

## <a name="route-template-reference"></a>Referencia de plantilla de ruta

Los tokens entre llaves (`{ ... }`) definen *parámetros de ruta* que se enlazan si se encuentran coincidencias con la ruta. Puede definir más de un parámetro de ruta en un segmento de ruta, pero deben estar separados por un valor literal. Por ejemplo, `{controller=Home}{action=Index}` no es una ruta válida, ya que no hay ningún valor literal entre `{controller}` y `{action}`. Estos parámetros de ruta deben tener un nombre y, opcionalmente, atributos adicionales especificados.

El texto literal diferente de los parámetros de ruta (por ejemplo, `{id}`) y el separador de ruta `/` deben coincidir con el texto de la dirección URL. La coincidencia de texto no distingue mayúsculas de minúsculas y se basa en la representación descodificada de la ruta de las direcciones URL. Para que el delimitador de parámetro de ruta literal coincida (`{` o `}`), repita el carácter (`{{` o `}}`) para usarlo como secuencia de escape.

Los patrones de dirección URL que intentan capturar un nombre de archivo con una extensión de archivo opcional tienen consideraciones adicionales. Por ejemplo, considere la plantilla `files/{filename}.{ext?}`. Cuando existen valores para `filename` y `ext`, los dos valores se rellenan. Si solo existe un valor para `filename` en la dirección URL, la ruta coincide porque el punto final (`.`) es opcional. Las direcciones URL siguientes coinciden con esta ruta:

* `/files/myFile.txt`
* `/files/myFile`

Se puede usar un asterisco (`*`) o un asterisco doble (`**`) como prefijo de un parámetro de ruta para enlazar con el resto del URI. Se denominan parámetros *comodín*. Por ejemplo, `blog/{**slug}` coincide con cualquier URI que empiece por `/blog` y que vaya seguido de cualquier valor, que se asigna al valor de ruta `slug`. Los parámetros comodín también pueden coincidir con una cadena vacía.

El parámetro catch-all inserta los caracteres de escape correspondientes cuando se usa la ruta para generar una dirección URL, incluidos caracteres de separación de ruta de acceso (`/`). Por ejemplo, la ruta `foo/{*path}` con valores de ruta `{ path = "my/path" }` genera `foo/my%2Fpath`. Tenga en cuenta la barra diagonal de escape. Para los caracteres separadores de ruta de acceso de ida y vuelta, use el prefijo de parámetro de ruta `**`. La ruta `foo/{**path}` con `{ path = "my/path" }` genera `foo/my/path`.

Los parámetros de ruta pueden tener *valores predeterminados* designados mediante la especificación del valor predeterminado después del nombre de parámetro, separado por un signo igual (`=`). Por ejemplo, `{controller=Home}` define `Home` como el valor predeterminado de `controller`. El valor predeterminado se usa si no hay ningún valor en la dirección URL para el parámetro. Los parámetros de ruta se pueden convertir en opcionales si se anexa un signo de interrogación (`?`) al final del nombre del parámetro, como en `id?`. La diferencia entre los valores opcionales y los parámetros de ruta predeterminados es que un parámetro de ruta con un valor predeterminado siempre genera un valor, mientras que un parámetro opcional tiene un valor solo cuando la dirección URL de solicitud lo proporciona.

Los parámetros de ruta pueden tener restricciones que deben coincidir con el valor de ruta enlazado desde la dirección URL. Si se agrega un carácter de dos puntos (`:`) y un nombre de restricción después del nombre del parámetro de ruta, se especifica una *restricción insertada* en un parámetro de ruta. Si la restricción requiere argumentos, se incluyen entre paréntesis (`(...)`) después del nombre de restricción. Se pueden especificar varias restricciones insertadas si se anexa otro carácter de dos puntos (`:`) y un nombre de restricción.

El nombre de restricción y los argumentos se pasan al servicio <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> para crear una instancia de <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> para su uso en el procesamiento de direcciones URL. Por ejemplo, la plantilla de ruta `blog/{article:minlength(10)}` especifica una restricción `minlength` con el argumento `10`. Para obtener más información sobre las restricciones de ruta y una lista de las restricciones proporcionadas por el marco de trabajo, vea la sección [Referencia de restricciones de ruta](#route-constraint-reference).

Los parámetros de ruta también pueden tener transformadores de parámetros, que transforman el valor de un parámetro al generar vínculos y acciones y páginas coincidentes para las direcciones URL. Al igual que las restricciones, los transformadores de parámetros se pueden agregar en línea a un parámetro de ruta al incorporar un carácter de dos puntos (`:`) y un nombre de transformador después del nombre del parámetro de ruta. Por ejemplo, la plantilla de ruta `blog/{article:slugify}` especifica un transformador `slugify`. Para obtener más información sobre los transformadores de parámetros, vea la sección [Referencia de transformadores de parámetros](#parameter-transformer-reference).

En la tabla siguiente se muestran algunas plantillas de ruta de ejemplo y su comportamiento.

| Plantilla de ruta                           | URI coincidente de ejemplo    | El URI de la solicitud&hellip;                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | Solo coincide con la ruta de acceso única `/hello`.                                     |
| `{Page=Home}`                            | `/`                     | Coincide y establece `Page` en `Home`.                                         |
| `{Page=Home}`                            | `/Contact`              | Coincide y establece `Page` en `Contact`.                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | Se asigna al controlador `Products` y la acción `List`.                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | Se asigna al controlador `Products` y la acción `Details` (`id` se establece en 123). |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | Se asigna al controlador `Home` y al método `Index` (`id` se omite).        |

El uso de una plantilla suele ser el método de enrutamiento más sencillo. Las restricciones y los valores predeterminados también se pueden especificar fuera de la plantilla de ruta.

> [!TIP]
> Habilite el [registro](xref:fundamentals/logging/index) para ver de qué forma las implementaciones de enrutamiento integradas, como <xref:Microsoft.AspNetCore.Routing.Route>, coinciden con las solicitudes.

## <a name="reserved-routing-names"></a>Nombres de enrutamientos reservados

Las siguientes palabras clave son nombres reservados y no se pueden usar como nombres de ruta o parámetros:

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Referencia de restricción de ruta

Las restricciones de ruta se ejecutan cuando se ha producido una coincidencia con la dirección URL entrante y la ruta de dirección URL se convierte en tokens en valores de ruta. En general, las restricciones de ruta inspeccionan el valor de ruta asociado a través de la plantilla de ruta y deciden si el valor es aceptable o no. Algunas restricciones de ruta usan datos ajenos al valor de ruta para decidir si la solicitud se puede enrutar. Por ejemplo, <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> puede aceptar o rechazar una solicitud basada en su verbo HTTP. Las restricciones se usan en las solicitudes de enrutamiento y la generación de vínculos.

> [!WARNING]
> No use las restricciones para las **validación de entrada**. Si las restricciones se usan para la **validación de entrada**, los resultados de entrada no válidos producirán un error *404 - No encontrado* en lugar de un error *400 - Solicitud incorrecta* con un mensaje de error adecuado. Las restricciones de ruta se usan para **eliminar la ambigüedad** entre rutas similares, no para validar las entradas de una ruta determinada.

En la tabla siguiente se muestran algunas restricciones de ruta de ejemplo y su comportamiento esperado.

| restricción | Ejemplo | Coincidencias de ejemplo | Notas |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789` | Coincide con cualquier entero |
| `bool` | `{active:bool}` | `true`, `FALSE` | Coincide con `true` o `false` (no distingue mayúsculas de minúsculas) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | Coincide con un valor `DateTime` válido (en la referencia cultural invariable; vea la advertencia) |
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
| `required` | `{name:required}` | `Rick` | Se usa para exigir que un valor que no es de parámetro esté presente durante la generación de dirección URL |

Es posible aplicar varias restricciones delimitadas por dos puntos a un único parámetro. Por ejemplo, la siguiente restricción permite limitar un parámetro a un valor entero de 1 o superior:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> Las restricciones de ruta que comprueban la dirección URL y que se convierten en un tipo CLR (como `int` o `DateTime`) usan siempre la referencia cultural invariable. Estas restricciones dan por supuesto que la dirección URL no es localizable. Las restricciones de ruta proporcionadas por el marco de trabajo no modifican los valores almacenados en los valores de ruta. Todos los valores de ruta analizados desde la dirección URL se almacenan como cadenas. Por ejemplo, la restricción `float` intenta convertir el valor de ruta en un valor Float, pero el valor convertido se usa exclusivamente para comprobar que se puede convertir en Float.

## <a name="regular-expressions"></a>Expresiones regulares

El marco de trabajo de ASP.NET Core agrega `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` al constructor de expresiones regulares. Vea <xref:System.Text.RegularExpressions.RegexOptions> para obtener una descripción de estos miembros.

Las expresiones regulares usan delimitadores y tokens similares a los que usan el enrutamiento y el lenguaje C#. Es necesario usar secuencias de escape con los tokens de expresiones regulares. Para usar la expresión regular `^\d{3}-\d{2}-\d{4}$` en el enrutamiento, los caracteres `\` (una barra diagonal inversa) de la cadena se deben proporcionar como caracteres `\\` (barra diagonal inversa doble) en el archivo de código fuente de C# para que el carácter de escape de cadena `\` tenga una secuencia de escape (a menos que se usen [literales de cadena textuales](/dotnet/csharp/language-reference/keywords/string)). Para aplicar secuencias de escape a los caracteres delimitadores de parámetro de enrutamiento (`{`, `}`, `[` y `]`), duplique los caracteres en la expresión (`{{`, `}`, `[[` y `]]`). En la tabla siguiente se muestra una expresión regular y la versión con la secuencia de escape.

| Expresión regular    | Expresión regular con secuencia de escape     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Las expresiones regulares que se usan en el enrutamiento suelen empezar con el carácter de acento circunflejo (`^`) y coinciden con la posición inicial de la cadena. Las expresiones suelen terminar con el carácter del signo de dólar (`$`) y coincidir con el final de la cadena. Los caracteres `^` y `$` garantizan que la expresión regular coincide con el valor completo del parámetro de ruta. Sin los caracteres `^` y `$`, la expresión regular coincide con cualquier subcadena de la cadena, algo que normalmente no es deseable. En la tabla siguiente se proporcionan ejemplos y se explica por qué coinciden o no.

| Expresión   | String    | Coincidir con | Comentario               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | hello     | Sí   | Coincidencias de subcadenas     |
| `[a-z]{2}`   | 123abc456 | Sí   | Coincidencias de subcadenas     |
| `[a-z]{2}`   | mz        | Sí   | Coincide con la expresión    |
| `[a-z]{2}`   | MZ        | Sí   | No distingue mayúsculas de minúsculas    |
| `^[a-z]{2}$` | hello     | No    | Vea `^` y `$` más arriba |
| `^[a-z]{2}$` | 123abc456 | No    | Vea `^` y `$` más arriba |

Para obtener más información sobre la sintaxis de expresiones regulares, vea [Expresiones regulares de .NET Framework](/dotnet/standard/base-types/regular-expression-language-quick-reference).

Para restringir un parámetro a un conjunto conocido de valores posibles, use una expresión regular. Por ejemplo, `{action:regex(^(list|get|create)$)}` solo hace coincidir el valor de ruta `action` con `list`, `get` o `create`. Si se pasa al diccionario de restricciones, la cadena `^(list|get|create)$` es equivalente. Las restricciones que se pasan al diccionario de restricciones (no insertado en una plantilla) que no coinciden con una de las restricciones conocidas también se tratan como expresiones regulares.

## <a name="custom-route-constraints"></a>Restricciones de ruta personalizadas

Además de las restricciones de ruta integradas, se pueden crear restricciones de ruta personalizadas implementando la interfaz <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>. La interfaz <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> contiene un único método, `Match`, que devuelve `true` si se cumple la restricción, y `false` en caso contrario.

Para utilizar una restricción <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> personalizada, el tipo de restricción de ruta debe registrarse con el parámetro <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> de la aplicación en el contenedor de servicios de la aplicación. <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> es un diccionario que asigna claves de restricciones de ruta a implementaciones de <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> que validen esas restricciones. El parámetro <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> de una aplicación puede actualizarse en `Startup.ConfigureServices` como parte de una llamada a [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) o configurando <xref:Microsoft.AspNetCore.Routing.RouteOptions> directamente con `services.Configure<RouteOptions>`. Por ejemplo:

```csharp
services.AddRouting(options =>
{
    options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
});
```

La restricción puede aplicarse a las rutas de la manera habitual, usando el nombre especificado al registrar el tipo de restricción. Por ejemplo:

```csharp
[HttpGet("{id:customName}")]
public ActionResult<string> Get(string id)
```

## <a name="parameter-transformer-reference"></a>Referencia de transformadores de parámetros

Transformadores de parámetros:

* Se ejecutan al generar un vínculo para <xref:Microsoft.AspNetCore.Routing.Route>.
* Implemente `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`.
* Se configuran con <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>.
* Toman el valor de ruta del parámetro y lo transforman en un nuevo valor de cadena.
* Como resultado, el valor transformado se usa en el vínculo generado.

Por ejemplo, un transformador de parámetros personalizado `slugify` en el patrón de ruta `blog\{article:slugify}` con `Url.Action(new { article = "MyTestArticle" })` genera `blog\my-test-article`.

Para usar un transformador de parámetros en un patrón de ruta, configúrelo primero con <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> en `Startup.ConfigureServices`:

```csharp
services.AddRouting(options =>
{
    // Replace the type and the name used to refer to it with your own
    // IOutboundParameterTransformer implementation
    options.ConstraintMap["slugify"] = typeof(SlugifyParameterTransformer);
});
```

El marco usa los transformadores de parámetros para transformar el URI en el que se resuelve un punto de conexión. Por ejemplo, ASP.NET Core MVC usa transformadores de parámetros para transformar el valor de ruta usado para hacer coincidir elementos `area`, `controller`, `action` y `page`.

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller:slugify=Home}/{action:slugify=Index}/{id?}");
```

Con la ruta anterior, la acción `SubscriptionManagementController.GetAll()` coincide con el URI `/subscription-management/get-all`. Un transformador de parámetros no cambia los valores de ruta usados para generar un vínculo. Por ejemplo, `Url.Action("GetAll", "SubscriptionManagement")` genera `/subscription-management/get-all`.

ASP.NET Core proporciona las convenciones de API para usar transformadores de parámetros con rutas generadas:

* ASP.NET Core MVC tiene la convención de API `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention`. Esta convención aplica un transformador de parámetro especificado a todas las rutas de atributo de la aplicación. El transformador de parámetro transforma los tokens de rutas de atributo a medida que se reemplazan. Para más información, consulte [Usar un transformador de parámetro para personalizar el reemplazo de tokens](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).
* Razor Pages tiene la convención de API `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention`. Esta convención aplica un transformador de parámetros especificado a todas las instancias de Razor Pages detectadas de forma automática. El transformador de parámetros transforma los segmentos de nombre de archivo y carpeta de las rutas de Razor Pages. Para más información, consulte el artículo sobre cómo [usar un transformador de parámetro para personalizar rutas de Razor Pages](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).

## <a name="url-generation-reference"></a>Referencia de generación de direcciones URL

En el ejemplo siguiente se muestra cómo se genera un vínculo a una ruta, dado un diccionario de valores de ruta y un valor <xref:Microsoft.AspNetCore.Routing.RouteCollection>.

[!code-csharp[](routing/samples/3.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

El valor <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> generado al final del ejemplo anterior es `/package/create/123`. El diccionario proporciona los valores de ruta `operation` e `id` de la plantilla "Ruta de paquete de seguimiento", `package/{operation}/{id}`. Para obtener más información, vea el código de ejemplo de la sección [Uso de software intermedio de enrutamiento](#use-routing-middleware) o la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples).

El segundo parámetro del constructor <xref:Microsoft.AspNetCore.Routing.VirtualPathContext> es una colección de *valores de ambiente*. Los valores de ambiente son adecuados porque limitan el número de valores que el desarrollador debe especificar dentro de un contexto de solicitud. Los valores de ruta actuales de la solicitud actual se consideran valores de ambiente para la generación de vínculos. En la acción `About` de `HomeController` de una aplicación ASP.NET Core MVC, no es necesario especificar el valor de ruta de controlador para vincular a la acción `Index` (se usará el valor de ambiente `Home`).

Los valores de ambiente que no coincidan con un parámetro se omiten. También se omiten los valores de ambiente cuando un valor proporcionado de forma explícita invalida el valor de ambiente. La coincidencia se produce de izquierda a derecha en la dirección URL.

Los valores que se proporcionan de forma explícita pero que no coinciden con un segmento de la ruta se agregan a la cadena de consulta. En la tabla siguiente se muestra el resultado cuando se usa la plantilla de ruta `{controller}/{action}/{id?}`.

| Valores de ambiente                     | Valores explícitos                        | Resultado                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| controller = "Home"                | action = "About"                       | `/Home/About`           |
| controller = "Home"                | controller = "Order", action = "About" | `/Order/About`          |
| controller = "Home", color = "Red" | action = "About"                       | `/Home/About`           |
| controller = "Home"                | action = "About", color = "Red"        | `/Home/About?color=Red` |

Si una ruta tiene un valor predeterminado que no se corresponde con un parámetro y ese valor se proporciona de forma explícita, debe coincidir con el valor predeterminado:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

La generación de vínculos solo genera un vínculo para esta ruta si se proporcionan los valores coincidentes para `controller` y `action`.

## <a name="complex-segments"></a>Segmentos complejos

Los segmentos complejos (por ejemplo, `[Route("/x{token}y")]`), se procesan buscando coincidencias de literales de derecha a izquierda de un modo no expansivo. Consulte [este código](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) para obtener una explicación detallada de cómo se comparan los segmentos complejos. El [código de ejemplo](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) no se usa en ASP.NET Core, pero proporciona una buena explicación de los segmentos complejos.
<!-- While that code is no longer used by ASP.NET Core for complex segment matching, it provides a good match to the current algorithm. The [current code](https://github.com/aspnet/AspNetCore/blob/91514c9af7e0f4c44029b51f05a01c6fe4c96e4c/src/Http/Routing/src/Matching/DfaMatcherBuilder.cs#L227-L244) is too abstracted from matching to be useful for understanding complex segment matching.
-->

## <a name="configuring-endpoint-metadata"></a>Configuración de metadatos de punto de conexión

Los vínculos siguientes proporcionan información sobre la configuración de metadatos de punto de conexión:

* [Habilitación de CORS con enrutamiento de punto de conexión](xref:security/cors#enable-cors-with-endpoint-routing)
* [Ejemplo de IAuthorizationPolicyProvider](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider) con un atributo `[MinimumAgeAuthorize]` personalizado
* [Autenticación de pruebas con el atributo [Authorize]](xref:security/authentication/identity#test-identity)
* <xref:Microsoft.AspNetCore.Builder.AuthorizationEndpointConventionBuilderExtensions.RequireAuthorization*>
* [Selección del esquema con el atributo [Authorize]](xref:security/authorization/limitingidentitybyscheme#selecting-the-scheme-with-the-authorize-attribute)
* [Aplicación de directivas con el atributo [Authorize]](xref:security/authorization/policies#applying-policies-to-mvc-controllers)
* <xref:security/authorization/roles>

<a name="hostmatch"></a>

## <a name="host-matching-in-routes-with-requirehost"></a>Comparación de host en rutas con RequireHost

`RequireHost` aplica una restricción a la ruta que requiere el host especificado. Los parámetros `RequireHost` o `[Host]` pueden ser:

* Host: `www.domain.com` (compara `www.domain.com` con cualquier puerto)
* Host con carácter comodín: `*.domain.com` (coincide con `www.domain.com`, `subdomain.domain.com` o `www.subdomain.domain.com` en cualquier puerto)
* Puerto: `*:5000` (coincide con el puerto 5000 con cualquier host)
* Host y puerto: `www.domain.com:5000`, `*.domain.com:5000` (coincide con el host y el puerto)

Se pueden especificar varios parámetros mediante `RequireHost` o `[Host]`. La restricción coincidirá con los hosts válidos para cualquiera de los parámetros. Por ejemplo, `[Host("domain.com", "*.domain.com")]` coincidirá con `domain.com`, `www.domain.com` o `subdomain.domain.com`.

En el código siguiente se usa `RequireHost` para requerir el host especificado en la ruta:

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapGet("/", context => context.Response.WriteAsync("Hi Contoso!"))
            .RequireHost("contoso.com");
        endpoints.MapGet("/", context => context.Response.WriteAsync("Hi AdventureWorks!"))
            .RequireHost("adventure-works.com");
        endpoints.MapHealthChecks("/healthz").RequireHost("*:8080");
    });
}
```

En el código siguiente se usa el atributo `[Host]` para requerir el host especificado en el controlador:

```csharp
[Host("contoso.com", "adventure-works.com")]
public class HomeController : Controller
{
    private readonly ILogger<HomeController> _logger;

    public HomeController(ILogger<HomeController> logger)
    {
        _logger = logger;
    }

    public IActionResult Index()
    {
        return View();
    }

    [Host("example.com:8080")]
    public IActionResult Privacy()
    {
        return View();
    }

}
```

Cuando el atributo `[Host]` se aplica al método de acción y al controlador:

* Se usa el atributo en la acción.
* Se omite el atributo del controlador.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

El enrutamiento es responsable de asignar URI de solicitud a los puntos de conexión y de distribuir las solicitudes entrantes a esos puntos de conexión. Las rutas se definen en la aplicación y se configuran cuando se inicia la aplicación. Una ruta puede extraer opcionalmente valores de la dirección URL contenida en la solicitud, que se pueden usar para procesar las solicitudes. Con la información de ruta de la aplicación, el enrutamiento también puede generar direcciones URL que se asignan a puntos de conexión.

Para usar los escenarios de enrutamiento más recientes de ASP.NET Core 2.2, especifique la [versión de compatibilidad](xref:mvc/compatibility-version) en el registro de servicios de MVC en `Startup.ConfigureServices`:

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

La opción <xref:Microsoft.AspNetCore.Mvc.MvcOptions.EnableEndpointRouting> determina si el enrutamiento debe usar de forma interna la lógica basada en el punto de conexión o la lógica basada en <xref:Microsoft.AspNetCore.Routing.IRouter> de ASP.NET Core 2.1 o una versión anterior. Cuando la versión de compatibilidad se establece en 2.2 o una versión posterior, el valor predeterminado es `true`. Establezca el valor en `false` para usar la lógica de enrutamiento anterior:

```csharp
// Use the routing logic of ASP.NET Core 2.1 or earlier:
services.AddMvc(options => options.EnableEndpointRouting = false)
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

Para obtener más información sobre el enrutamiento basado en <xref:Microsoft.AspNetCore.Routing.IRouter>, vea la [versión para ASP.NET Core 2.1 de este tema](/aspnet/core/fundamentals/routing?view=aspnetcore-2.1).

> [!IMPORTANT]
> En este documento se describe el enrutamiento de ASP.NET Core de bajo nivel. Para obtener información sobre el enrutamiento de ASP.NET Core MVC, vea <xref:mvc/controllers/routing>. Para obtener más información sobre las convenciones de enrutamiento en Razor Pages, consulte <xref:razor-pages/razor-pages-conventions>.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="routing-basics"></a>Fundamentos del enrutamiento

La mayoría de las aplicaciones deben elegir un esquema de enrutamiento básico y descriptivo para que las direcciones URL sean legibles y significativas. La ruta convencional predeterminada `{controller=Home}/{action=Index}/{id?}`:

* Admite un esquema de enrutamiento básico y descriptivo.
* Se trata de un punto de partida útil para las aplicaciones basadas en la interfaz de usuario.

Los desarrolladores suelen agregar rutas breves adicionales a áreas de mucho tráfico de una aplicación en situaciones especializadas (por ejemplo, puntos de conexión de blog y comercio electrónico) con el [enrutamiento mediante atributos](xref:mvc/controllers/routing#attribute-routing) o rutas convencionales dedicadas.

Las API web deben usar el enrutamiento mediante atributos para modelar la funcionalidad de la aplicación como un conjunto de recursos donde las operaciones se representan mediante verbos HTTP. Esto significa que muchas operaciones (por ejemplo, GET y POST) del mismo recurso lógico usarán la misma dirección URL. El enrutamiento mediante atributos proporciona un nivel de control que es necesario para diseñar cuidadosamente un diseño de puntos de conexión públicos de la API.

En las aplicaciones de Razor Pages se usa el enrutamiento convencional predeterminado para proporcionar recursos con nombre en la carpeta *Páginas* de una aplicación. Existen convenciones adicionales que permiten personalizar el comportamiento de enrutamiento de Razor Pages. Para obtener más información, vea <xref:razor-pages/index> y <xref:razor-pages/razor-pages-conventions>.

La compatibilidad de la generación de direcciones URL permite desarrollar la aplicación sin codificar de forma rígida las direcciones URL para vincular la aplicación. Esta compatibilidad permite empezar con una configuración de enrutamiento básica y modificar las rutas una vez determinado el diseño de los recursos de la aplicación.

El enrutamiento usa *puntos de conexión* (`Endpoint`) para representar los puntos de conexión lógicos en una aplicación.

Un punto de conexión define un delegado para procesar las solicitudes y una colección de metadatos arbitrarios. Los metadatos se usan para implementar cuestiones transversales según las directivas y la configuración asociada a cada punto de conexión.

El sistema de enrutamiento tiene las características siguientes:

* La sintaxis de plantilla de ruta se usa para definir las rutas con parámetros de ruta con tokens.
* Se permite la configuración de puntos de conexión de estilo convencional y de estilo de atributo.
* Se usa <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> para determinar si un parámetro de dirección URL contiene un valor válido para una restricción de punto de conexión determinada.
* Los modelos de aplicación, como MVC y Razor Pages, registran todos sus puntos de conexión, que presentan una implementación predecible de los escenarios de enrutamiento.
* La implementación de enrutamiento toma decisiones relativas al enrutamiento siempre que sea lo deseado en la canalización de middleware.
* El middleware que aparece después de un middleware de enrutamiento puede inspeccionar el resultado de la decisión del punto de conexión del middleware de enrutamiento para un URI de solicitud determinado.
* Se pueden enumerar todos los puntos de conexión de la aplicación en cualquier parte de la canalización de middleware.
* Una aplicación puede usar el enrutamiento para generar direcciones URL (por ejemplo, para el redireccionamiento o los vínculos) en función de la información del punto de conexión. De este modo, se evita codificar de forma rígida las direcciones URL, lo que facilita el mantenimiento.
* La generación de direcciones URL se basa en direcciones, que admiten la extensibilidad arbitraria:

  * La API del generador de vínculos (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>) se puede resolver en cualquier lugar mediante la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection) para generar direcciones URL.
  * Cuando la API del generador de vínculos no está disponible a través de DI, <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> ofrece métodos para generar direcciones URL.

> [!NOTE]
> Con el lanzamiento del enrutamiento de punto de conexión en ASP.NET Core 2.2, la vinculación de punto de conexión se limita a acciones y páginas de Razor Pages y MVC. Las expansiones de las funciones de vinculación de punto de conexión están previstas para próximas versiones.

La clase <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> conecta el enrutamiento a la canalización de [software intermedio](xref:fundamentals/middleware/index). [ASP.NET Core MVC](xref:mvc/overview) agrega enrutamiento a la canalización de middleware como parte de su configuración y controla el enrutamiento en las aplicaciones de MVC y Razor Pages. Para obtener información sobre cómo usar el enrutamiento como componente independiente, vea la sección [Uso de software intermedio de enrutamiento](#use-routing-middleware).

### <a name="url-matching"></a>Coincidencia de dirección URL

La coincidencia de dirección URL es el proceso por el cual el enrutamiento envía una solicitud entrante a un *punto de conexión*. Este proceso se basa en datos de la ruta de dirección URL, pero se puede ampliar para tener en cuenta cualquier dato de la solicitud. La capacidad de enviar solicitudes a controladores independientes es clave para escalar el tamaño y la complejidad de una aplicación.

El sistema de enrutamiento en el enrutamiento de punto de conexión es responsable de todas las decisiones relativas al envío. Como el middleware aplica las directivas en función del punto de conexión seleccionado, es importante que cualquier decisión que pueda afectar a la distribución o la aplicación de directivas de seguridad se realice dentro del sistema de enrutamiento.

Cuando se ejecuta el delegado del punto de conexión, las propiedades de [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) se establecen en los valores adecuados en función del procesamiento de solicitudes realizado hasta el momento.

[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) es un diccionario de los *valores de ruta* generados desde la ruta. Estos valores se suelen determinar mediante la conversión en tokens de la dirección URL, y se pueden usar para aceptar la entrada del usuario o para tomar otras decisiones sobre el envío dentro de la aplicación.

[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) es un contenedor de propiedades de datos adicionales relacionados con la ruta coincidente. Se proporcionan <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> para permitir la asociación de datos de estado con cada ruta, de modo que la aplicación pueda tomar decisiones en función de las rutas que han coincidido. Estos valores los define el desarrollador y **no** afectan de ninguna manera al comportamiento del enrutamiento. Además, los valores que se guardan provisionalmente en [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) pueden ser de cualquier tipo, a diferencia de [RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values), que deben poder convertirse fácilmente en cadenas y a partir de estas.

[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) es una lista de las rutas que han participado en encontrar una coincidencia correcta con la solicitud. Las rutas se pueden anidar unas dentro de otras. La propiedad <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> refleja la ruta de acceso del árbol lógico de rutas que han tenido como resultado una coincidencia. Por lo general, el primer elemento de <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> es la colección de rutas y se debe usar para la generación de direcciones URL. El último elemento de <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> es el controlador de ruta que ha coincidido.

<a name="lg"></a>

### <a name="url-generation-with-linkgenerator"></a>Generación de direcciones URL con LinkGenerator

La generación de dirección URL es el proceso por el cual el enrutamiento puede crear una ruta de dirección URL basada en un conjunto de valores de ruta. Esto permite una separación lógica entre los puntos de conexión y las direcciones URL que tienen acceso a ellos.

El enrutamiento de punto de conexión incluye la API del generador de vínculos (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>). <xref:Microsoft.AspNetCore.Routing.LinkGenerator> es un servicio singleton que se puede recuperar a partir de la DI. La API se puede usar fuera del contexto de una solicitud en ejecución. <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> de MVC y los escenarios que dependen de <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>, como los [asistentes de etiquetas](xref:mvc/views/tag-helpers/intro), los de HTML y [los resultados de acción](xref:mvc/controllers/actions), usan el generador de vínculos para proporcionar funciones de generación de vínculos.

El generador de vínculos está respaldado por el concepto de una *dirección* y *esquemas de direcciones*. Un esquema de direcciones es una manera de determinar los puntos de conexión que se deben tener en cuenta para la generación de vínculos. Por ejemplo, los escenarios de nombre y valores de ruta de Razor Pages y MVC con los que muchos usuarios están familiarizados se implementan como un esquema de direcciones.

El generador de vínculos puede vincular a acciones y páginas de Razor Pages y MVC a través de los métodos de extensión siguientes:

* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*>
* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetPathByPage*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetUriByPage*>

Una sobrecarga de estos métodos acepta argumentos que incluyan `HttpContext`. Estos métodos son equivalentes funcionalmente a `Url.Action` y `Url.Page`, pero ofrecen flexibilidad y opciones adicionales.

Los métodos `GetPath*` son más similares a `Url.Action` y `Url.Page`, dado que generan un URI que contiene una ruta de acceso absoluta. Los métodos `GetUri*` siempre generan un URI absoluto que contiene un esquema y un host. Los métodos que aceptan `HttpContext` generan un URI en el contexto de la solicitud que se ejecuta. A menos que se reemplacen, se usan los valores de ruta de ambiente, la ruta de acceso base de la dirección URL, el esquema y el host de la solicitud que se ejecuta.

Se llama a <xref:Microsoft.AspNetCore.Routing.LinkGenerator> con una dirección. La generación de un URI se produce en dos pasos:

1. Se enlaza una dirección a una lista de puntos de conexión que coincidan con la dirección.
1. Se evalúa el elemento `RoutePattern` de cada punto de conexión hasta que se encuentra un patrón de ruta que coincida con los valores proporcionados. La salida resultante se combina con otras partes del URI proporcionadas al generador de vínculos y devueltas.

Los métodos proporcionados por <xref:Microsoft.AspNetCore.Routing.LinkGenerator> admiten funciones estándar de generación de vínculos para cualquier tipo de dirección. La forma más útil de usar el generador de vínculos es a través de métodos de extensión que realicen operaciones para un tipo de dirección específica.

| Método de extensión   | Descripción                                                         |
| ------------------ | ------------------------------------------------------------------- |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*> | Genera un URI con una ruta de acceso absoluta en función de los valores proporcionados. |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetUriByAddress*> | Genera un URI absoluto en función de los valores proporcionados.             |

> [!WARNING]
> Preste atención a las consecuencias siguientes de llamar a los métodos <xref:Microsoft.AspNetCore.Routing.LinkGenerator>:
>
> * Use los métodos de extensión `GetUri*` con precaución en una configuración de aplicación en la que no se valide el encabezado `Host` de las solicitudes entrantes. Si no se valida el encabezado `Host` de las solicitudes entrantes, la entrada de la solicitud que no sea de confianza se puede devolver al cliente en los URI de una página o vista. Se recomienda que todas las aplicaciones de producción configuren su servidor para validar el encabezado `Host` en función de valores válidos conocidos.
>
> * Use <xref:Microsoft.AspNetCore.Routing.LinkGenerator> con precaución en el middleware junto con `Map` o `MapWhen`. `Map*` cambia la ruta de acceso base de la solicitud que se ejecuta, lo que afecta a la salida de la generación de vínculos. Todas las API de <xref:Microsoft.AspNetCore.Routing.LinkGenerator> permiten especificar una ruta de acceso base. Especifique siempre una ruta de acceso base vacía para deshacer el efecto de `Map*` en la generación de vínculos.

## <a name="differences-from-earlier-versions-of-routing"></a>Diferencias con respecto a versiones anteriores del enrutamiento

Existen algunas diferencias entre el enrutamiento de punto de conexión de ASP.NET Core 2.2 o posterior, y las versiones anteriores del enrutamiento en ASP.NET Core:

* El sistema de enrutamiento de punto de conexión no es compatible con la extensibilidad basada en <xref:Microsoft.AspNetCore.Routing.IRouter>, incluida la herencia de <xref:Microsoft.AspNetCore.Routing.Route>.

* El enrutamiento de punto de conexión no es compatible con [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim). Utilice la [versión compatibilidad](xref:mvc/compatibility-version) 2.1 (`.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)`) para seguir usando la corrección de compatibilidad.

* El enrutamiento de punto de conexión tiene un comportamiento diferente para el uso de mayúsculas y minúsculas en los URI generados al usar rutas convencionales.

  Tenga en cuenta la plantilla de ruta predeterminada siguiente:

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  Supongamos que genera un vínculo a una acción mediante la ruta siguiente:

  ```csharp
  var link = Url.Action("ReadPost", "blog", new { id = 17, });
  ```

  Con el enrutamiento basado en <xref:Microsoft.AspNetCore.Routing.IRouter>, este código genera un URI de `/blog/ReadPost/17`, que respeta las mayúsculas y minúsculas del valor de ruta proporcionado. El enrutamiento de punto de conexión de ASP.NET Core 2.2 o versiones posteriores genera `/Blog/ReadPost/17` ("Blog" se pone mayúscula). El enrutamiento de punto de conexión proporciona la interfaz `IOutboundParameterTransformer`, que se puede usar para personalizar este comportamiento de forma global, o bien para aplicar otras convenciones para la asignación de direcciones URL.

  Para obtener más información, vea la sección [Referencia de transformadores de parámetros](#parameter-transformer-reference).

* La generación de vínculos que se usa en Razor Pages y MVC con las rutas convencionales tiene un comportamiento diferente al intentar vincularlo a un controlador o una acción, o a una página que no existe.

  Tenga en cuenta la plantilla de ruta predeterminada siguiente:

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  Supongamos que genera un vínculo a una acción mediante la plantilla predeterminada con lo siguiente:

  ```csharp
  var link = Url.Action("ReadPost", "Blog", new { id = 17, });
  ```

  Con el enrutamiento basado en `IRouter`, el resultado siempre es `/Blog/ReadPost/17`, incluso aunque `BlogController` no exista o no tenga un método de acción `ReadPost`. Como se esperaba, si el método de acción existe, el enrutamiento de punto de conexión de ASP.NET Core 2.2 o versiones posteriores genera `/Blog/ReadPost/17`. *Sin embargo, si la acción no existe, el enrutamiento de punto de conexión genera una cadena vacía.* Conceptualmente, el enrutamiento de punto de conexión no supone que, si la acción no existe, sí que exista el punto de conexión.

* El *algoritmo de invalidación del valor de ambiente* de la generación de vínculos tiene otro comportamiento al usarlo con el enrutamiento de punto de conexión.

  La *invalidación del valor de ambiente* es el algoritmo que decide qué valores de ruta de la solicitud que se ejecuta actualmente (los valores de ambiente) se pueden usar en las operaciones de generación de vínculos. El enrutamiento convencional siempre invalida los valores de ruta adicionales al vincular a otra acción. El enrutamiento mediante atributos no tenía este comportamiento antes del lanzamiento de ASP.NET Core 2.2. En versiones anteriores de ASP.NET Core, los vínculos a otra acción que usara los mismos nombres de parámetro de ruta producían errores de generación de vínculo. En ASP.NET Core 2.2 o versiones posteriores, las dos formas de enrutamiento invalidan los valores cuando se vincula a otra acción.

  Considere el ejemplo siguiente de ASP.NET Core 2.1 o una versión anterior. Al vincular a otra acción (o a otra página), los valores de ruta se pueden reutilizar de formas no deseadas.

  En */Pages/Store/Product.cshtml*:

  ```cshtml
  @page "{id}"
  @Url.Page("/Login")
  ```

  En */Pages/Login.cshtml*:

  ```cshtml
  @page "{id?}"
  ```

  Si el URI es `/Store/Product/18` en ASP.NET Core 2.1 o versiones anteriores, el vínculo que genera `@Url.Page("/Login")` en la página Store/Info es `/Login/18`. Se reutiliza el valor `id` de 18, aunque el destino del vínculo sea otro elemento totalmente distinto de la aplicación. El valor de ruta `id` en el contexto de la página `/Login` probablemente sea un valor de id. de usuario, no un valor de id. de producto de tienda.

  En el enrutamiento de punto de conexión con ASP.NET Core 2.2 o versiones posteriores, el resultado es `/Login`. Los valores de ambiente no se reutilizan cuando el destino vinculado es otra acción o página.

* Sintaxis de parámetro de ruta de ida y vuelta: las barras diagonales no se codifican cuando se usa una sintaxis de parámetro comodín de doble asterisco (`**`).

  Durante la generación de vínculos, el sistema de enrutamiento codifica el valor capturado en un parámetro comodín de doble asterisco (`**`; por ejemplo, `{**myparametername}`), excepto las barras diagonales. El comodín de doble asterisco es compatible con el enrutamiento basado en `IRouter` en ASP.NET Core 2.2 o versiones posteriores.

  La sintaxis de parámetro comodín de un único asterisco en versiones anteriores de ASP.NET Core (`{*myparametername}`) sigue siendo compatible y las barras diagonales se codifican.

  | Ruta              | Vínculo generado con<br>`Url.Action(new { category = "admin/products" })`&hellip; |
  | ------------------ | --------------------------------------------------------------------- |
  | `/search/{*page}`  | `/search/admin%2Fproducts` (la barra diagonal se codifica)             |
  | `/search/{**page}` | `/search/admin/products`                                              |

### <a name="middleware-example"></a>Ejemplo de middleware

En el ejemplo siguiente, un middleware usa la API <xref:Microsoft.AspNetCore.Routing.LinkGenerator> para crear el vínculo a un método de acción que enumera los productos de la tienda. El uso del generador de vínculos mediante su inserción en una clase y la llamada a `GenerateLink` está disponible para cualquier clase de una aplicación.

```csharp
using Microsoft.AspNetCore.Routing;

public class ProductsLinkMiddleware
{
    private readonly LinkGenerator _linkGenerator;

    public ProductsLinkMiddleware(RequestDelegate next, LinkGenerator linkGenerator)
    {
        _linkGenerator = linkGenerator;
    }

    public async Task InvokeAsync(HttpContext httpContext)
    {
        var url = _linkGenerator.GetPathByAction("ListProducts", "Store");

        httpContext.Response.ContentType = "text/plain";

        await httpContext.Response.WriteAsync($"Go to {url} to see our products.");
    }
}
```

### <a name="create-routes"></a>Creación de rutas

La mayoría de las aplicaciones crea rutas mediante una llamada a <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> o a uno de los métodos de extensión similares definidos en <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>. Todos los métodos de extensión <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> crean una instancia de <xref:Microsoft.AspNetCore.Routing.Route> y la agregan a la colección de rutas.

<xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> no acepta un parámetro de controlador de ruta. <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> solo agrega las rutas que se controlan mediante <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>. Para obtener más información sobre el enrutamiento en MVC, vea <xref:mvc/controllers/routing>.

El código siguiente es un ejemplo de una llamada a <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> usada por una definición de ruta típica de ASP.NET Core MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Esta plantilla coincide con una ruta de dirección URL y extrae los valores de ruta. Por ejemplo, la ruta de acceso `/Products/Details/17` genera los valores de ruta siguientes: `{ controller = Products, action = Details, id = 17 }`.

Los valores de ruta se determinan al dividir la ruta de dirección URL en segmentos y hacer coincidir cada segmento con el nombre del *parámetro de ruta* en la plantilla de ruta. Los parámetros de ruta tienen un nombre asignado. Para definir los parámetros, el nombre del parámetro se incluye entre llaves `{ ... }`.

La plantilla anterior también podría coincidir con la ruta de dirección URL `/` y generar los valores `{ controller = Home, action = Index }`. Esto se debe a que los parámetros de ruta `{controller}` y `{action}` tienen valores predeterminados y el parámetro de ruta `id` es opcional. Un signo igual (`=`) seguido de un valor después del nombre del parámetro de ruta define un valor predeterminado para el parámetro. Un signo de interrogación (`?`) después del nombre del parámetro de ruta define un parámetro opcional.

Los parámetros de ruta con un valor predeterminado *siempre* generan un valor de ruta cuando la ruta coincide. Los parámetros opcionales no generan un valor de ruta si no existió el segmento de ruta de dirección URL correspondiente. Vea la sección [Referencia de plantilla de ruta](#route-template-reference) para obtener una descripción detallada de los escenarios y la sintaxis de la plantilla de ruta.

En el ejemplo siguiente, la definición de parámetro de ruta `{id:int}` define una [restricción de ruta](#route-constraint-reference) para el parámetro de ruta `id`:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Esta plantilla coincide con una ruta de dirección URL como `/Products/Details/17`, pero no con `/Products/Details/Apples`. Las restricciones de ruta implementan <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> e inspeccionan los valores de ruta para comprobarlos. En este ejemplo, el valor de ruta `id` debe poder convertirse en un entero. Vea la [Referencia de restricción de ruta](#route-constraint-reference) para obtener una explicación de las restricciones de ruta proporcionadas por el marco de trabajo.

Las sobrecargas adicionales de <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> aceptan valores para `constraints`, `dataTokens` y `defaults`. Estos parámetros suelen usarse para pasar un objeto de tipo anónimo, donde los nombres de propiedad del tipo anónimo coinciden con los nombres de los parámetros de ruta.

En los ejemplos <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> siguientes se crean rutas equivalentes:

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
> La sintaxis insertada para definir las restricciones y los valores predeterminados puede ser conveniente para las rutas simples. Pero hay algunos escenarios, como los tokens de datos, que no son compatibles con la sintaxis insertada.

En el ejemplo siguiente se muestran algunos escenarios más:

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{**article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

La plantilla anterior coincide con una ruta de dirección URL como `/Blog/All-About-Routing/Introduction` y extrae los valores `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. La ruta genera los valores de ruta predeterminados para `controller` y `action`, incluso si no hay parámetros de ruta correspondientes en la plantilla. Los valores predeterminados pueden especificarse en la plantilla de ruta. El parámetro de ruta `article` se define como un *comodín* por la aparición de un asterisco doble (`**`) antes del nombre del parámetro de ruta. Los parámetros de ruta comodín capturan el resto de la ruta de dirección URL y también pueden coincidir con la cadena vacía.

En el ejemplo siguiente se agregan restricciones de ruta y tokens de datos:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

La plantilla anterior coincide con una ruta de dirección URL como `/en-US/Products/5` y extrae los valores `{ controller = Products, action = Details, id = 5 }` y los tokens de datos `{ locale = en-US }`.

![Tokens de Windows de variables locales](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a>Generación de direcciones URL de clase de ruta

La clase <xref:Microsoft.AspNetCore.Routing.Route> también puede llevar a cabo la generación de dirección URL mediante la combinación de un conjunto de valores de ruta con su plantilla de ruta. Se trata lógicamente del proceso inverso de hacer coincidir la ruta de dirección URL.

> [!TIP]
> Para entender mejor la generación de direcciones URL, imagine qué dirección URL quiere generar y, después, piense cómo coincidiría una plantilla de ruta con esa dirección URL. ¿Qué valores se generarían? Este es un equivalente aproximado de cómo funciona la generación de dirección URL en la clase <xref:Microsoft.AspNetCore.Routing.Route>.

En el ejemplo siguiente se usa una ruta predeterminada de ASP.NET Core MVC general:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Con los valores de ruta `{ controller = Products, action = List }`, se genera la dirección URL `/Products/List`. Los valores de ruta se sustituyen por los parámetros de ruta correspondientes para formar la ruta de dirección URL. Como `id` es un parámetro de ruta opcional, la dirección URL se ha generado correctamente sin ningún valor para `id`.

Con los valores de ruta `{ controller = Home, action = Index }`, se genera la dirección URL `/`. Los valores de ruta proporcionados coinciden con los valores predeterminados, y los segmentos correspondientes a los valores predeterminados se omiten sin ningún riesgo.

Ambas direcciones URL generadas realizan un recorrido de ida y vuelta con la definición de ruta siguiente (`/Home/Index` y `/`) y generan los mismos valores de ruta que se usaron para generar la dirección URL.

> [!NOTE]
> Las aplicaciones con ASP.NET Core MVC deben usar <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> para generar direcciones URL en lugar de llamar directamente al enrutamiento.

Para obtener más información sobre la generación de direcciones URL, vea la sección [Referencia de generación de direcciones URL](#url-generation-reference).

## <a name="use-routing-middleware"></a>Uso de software intermedio de enrutamiento

Haga referencia al [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) en el archivo de proyecto de la aplicación.

Agregue enrutamiento al contenedor de servicios en `Startup.ConfigureServices`:

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Las rutas se deben configurar en el método `Startup.Configure`. En la aplicación de ejemplo se usan las API siguientes:

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; solo coincide con solicitudes HTTP GET.
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

En la tabla siguiente se muestran las respuestas con los URI especificados.

| Identificador URI                    | Respuesta                                          |
| ---------------------- | ------------------------------------------------- |
| `/package/create/3`    | Hello! Valores de ruta: [operation, create], [id, 3] |
| `/package/track/-3`    | Hello! Valores de ruta: [operation, track], [id, -3] |
| `/package/track/-3/`   | Hello! Valores de ruta: [operation, track], [id, -3] |
| `/package/track/`      | La solicitud pasa; no hay ninguna coincidencia.              |
| `GET /hello/Joe`       | Hi, Joe!                                          |
| `POST /hello/Joe`      | La solicitud pasa; solo coincide con HTTP GET. |
| `GET /hello/Joe/Smith` | La solicitud pasa; no hay ninguna coincidencia.              |

El marco de trabajo proporciona un conjunto de métodos de extensión para crear rutas (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>):

* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareVerb*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>

Los métodos `Map[Verb]` usan restricciones para limitar la ruta al verbo HTTP en el nombre del método. Por ejemplo, vea <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> y <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.

## <a name="route-template-reference"></a>Referencia de plantilla de ruta

Los tokens entre llaves (`{ ... }`) definen *parámetros de ruta* que se enlazan si se encuentran coincidencias con la ruta. Puede definir más de un parámetro de ruta en un segmento de ruta, pero deben estar separados por un valor literal. Por ejemplo, `{controller=Home}{action=Index}` no es una ruta válida, ya que no hay ningún valor literal entre `{controller}` y `{action}`. Estos parámetros de ruta deben tener un nombre y, opcionalmente, atributos adicionales especificados.

El texto literal diferente de los parámetros de ruta (por ejemplo, `{id}`) y el separador de ruta `/` deben coincidir con el texto de la dirección URL. La coincidencia de texto no distingue mayúsculas de minúsculas y se basa en la representación descodificada de la ruta de las direcciones URL. Para que el delimitador de parámetro de ruta literal coincida (`{` o `}`), repita el carácter (`{{` o `}}`) para usarlo como secuencia de escape.

Los patrones de dirección URL que intentan capturar un nombre de archivo con una extensión de archivo opcional tienen consideraciones adicionales. Por ejemplo, considere la plantilla `files/{filename}.{ext?}`. Cuando existen valores para `filename` y `ext`, los dos valores se rellenan. Si solo existe un valor para `filename` en la dirección URL, la ruta coincide porque el punto final (`.`) es opcional. Las direcciones URL siguientes coinciden con esta ruta:

* `/files/myFile.txt`
* `/files/myFile`

Se puede usar un asterisco (`*`) o un asterisco doble (`**`) como prefijo de un parámetro de ruta para enlazar con el resto del URI. Se denominan parámetros *comodín*. Por ejemplo, `blog/{**slug}` coincide con cualquier URI que empiece por `/blog` y que vaya seguido de cualquier valor, que se asigna al valor de ruta `slug`. Los parámetros comodín también pueden coincidir con una cadena vacía.

El parámetro catch-all inserta los caracteres de escape correspondientes cuando se usa la ruta para generar una dirección URL, incluidos caracteres de separación de ruta de acceso (`/`). Por ejemplo, la ruta `foo/{*path}` con valores de ruta `{ path = "my/path" }` genera `foo/my%2Fpath`. Tenga en cuenta la barra diagonal de escape. Para los caracteres separadores de ruta de acceso de ida y vuelta, use el prefijo de parámetro de ruta `**`. La ruta `foo/{**path}` con `{ path = "my/path" }` genera `foo/my/path`.

Los parámetros de ruta pueden tener *valores predeterminados* designados mediante la especificación del valor predeterminado después del nombre de parámetro, separado por un signo igual (`=`). Por ejemplo, `{controller=Home}` define `Home` como el valor predeterminado de `controller`. El valor predeterminado se usa si no hay ningún valor en la dirección URL para el parámetro. Los parámetros de ruta se pueden convertir en opcionales si se anexa un signo de interrogación (`?`) al final del nombre del parámetro, como en `id?`. La diferencia entre los valores opcionales y los parámetros de ruta predeterminados es que un parámetro de ruta con un valor predeterminado siempre genera un valor, mientras que un parámetro opcional tiene un valor solo cuando la dirección URL de solicitud lo proporciona.

Los parámetros de ruta pueden tener restricciones que deben coincidir con el valor de ruta enlazado desde la dirección URL. Si se agrega un carácter de dos puntos (`:`) y un nombre de restricción después del nombre del parámetro de ruta, se especifica una *restricción insertada* en un parámetro de ruta. Si la restricción requiere argumentos, se incluyen entre paréntesis (`(...)`) después del nombre de restricción. Se pueden especificar varias restricciones insertadas si se anexa otro carácter de dos puntos (`:`) y un nombre de restricción.

El nombre de restricción y los argumentos se pasan al servicio <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> para crear una instancia de <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> para su uso en el procesamiento de direcciones URL. Por ejemplo, la plantilla de ruta `blog/{article:minlength(10)}` especifica una restricción `minlength` con el argumento `10`. Para obtener más información sobre las restricciones de ruta y una lista de las restricciones proporcionadas por el marco de trabajo, vea la sección [Referencia de restricciones de ruta](#route-constraint-reference).

Los parámetros de ruta también pueden tener transformadores de parámetros, que transforman el valor de un parámetro al generar vínculos y acciones y páginas coincidentes para las direcciones URL. Al igual que las restricciones, los transformadores de parámetros se pueden agregar en línea a un parámetro de ruta al incorporar un carácter de dos puntos (`:`) y un nombre de transformador después del nombre del parámetro de ruta. Por ejemplo, la plantilla de ruta `blog/{article:slugify}` especifica un transformador `slugify`. Para obtener más información sobre los transformadores de parámetros, vea la sección [Referencia de transformadores de parámetros](#parameter-transformer-reference).

En la tabla siguiente se muestran algunas plantillas de ruta de ejemplo y su comportamiento.

| Plantilla de ruta                           | URI coincidente de ejemplo    | El URI de la solicitud&hellip;                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | Solo coincide con la ruta de acceso única `/hello`.                                     |
| `{Page=Home}`                            | `/`                     | Coincide y establece `Page` en `Home`.                                         |
| `{Page=Home}`                            | `/Contact`              | Coincide y establece `Page` en `Contact`.                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | Se asigna al controlador `Products` y la acción `List`.                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | Se asigna al controlador `Products` y la acción `Details` (`id` se establece en 123). |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | Se asigna al controlador `Home` y al método `Index` (`id` se omite).        |

El uso de una plantilla suele ser el método de enrutamiento más sencillo. Las restricciones y los valores predeterminados también se pueden especificar fuera de la plantilla de ruta.

> [!TIP]
> Habilite el [registro](xref:fundamentals/logging/index) para ver de qué forma las implementaciones de enrutamiento integradas, como <xref:Microsoft.AspNetCore.Routing.Route>, coinciden con las solicitudes.

## <a name="reserved-routing-names"></a>Nombres de enrutamientos reservados

Las siguientes palabras clave son nombres reservados y no se pueden usar como nombres de ruta o parámetros:

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Referencia de restricción de ruta

Las restricciones de ruta se ejecutan cuando se ha producido una coincidencia con la dirección URL entrante y la ruta de dirección URL se convierte en tokens en valores de ruta. En general, las restricciones de ruta inspeccionan el valor de ruta asociado a través de la plantilla de ruta y deciden si el valor es aceptable o no. Algunas restricciones de ruta usan datos ajenos al valor de ruta para decidir si la solicitud se puede enrutar. Por ejemplo, <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> puede aceptar o rechazar una solicitud basada en su verbo HTTP. Las restricciones se usan en las solicitudes de enrutamiento y la generación de vínculos.

> [!WARNING]
> No use las restricciones para las **validación de entrada**. Si las restricciones se usan para la **validación de entrada**, los resultados de entrada no válidos producirán un error *404 - No encontrado* en lugar de un error *400 - Solicitud incorrecta* con un mensaje de error adecuado. Las restricciones de ruta se usan para **eliminar la ambigüedad** entre rutas similares, no para validar las entradas de una ruta determinada.

En la tabla siguiente se muestran algunas restricciones de ruta de ejemplo y su comportamiento esperado.

| restricción | Ejemplo | Coincidencias de ejemplo | Notas |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789` | Coincide con cualquier entero |
| `bool` | `{active:bool}` | `true`, `FALSE` | Coincide con `true` o `false` (no distingue mayúsculas de minúsculas) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | Coincide con un valor `DateTime` válido (en la referencia cultural invariable; vea la advertencia) |
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
| `required` | `{name:required}` | `Rick` | Se usa para exigir que un valor que no es de parámetro esté presente durante la generación de dirección URL |

Es posible aplicar varias restricciones delimitadas por dos puntos a un único parámetro. Por ejemplo, la siguiente restricción permite limitar un parámetro a un valor entero de 1 o superior:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> Las restricciones de ruta que comprueban la dirección URL y que se convierten en un tipo CLR (como `int` o `DateTime`) usan siempre la referencia cultural invariable. Estas restricciones dan por supuesto que la dirección URL no es localizable. Las restricciones de ruta proporcionadas por el marco de trabajo no modifican los valores almacenados en los valores de ruta. Todos los valores de ruta analizados desde la dirección URL se almacenan como cadenas. Por ejemplo, la restricción `float` intenta convertir el valor de ruta en un valor Float, pero el valor convertido se usa exclusivamente para comprobar que se puede convertir en Float.

## <a name="regular-expressions"></a>Expresiones regulares

El marco de trabajo de ASP.NET Core agrega `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` al constructor de expresiones regulares. Vea <xref:System.Text.RegularExpressions.RegexOptions> para obtener una descripción de estos miembros.

Las expresiones regulares usan delimitadores y tokens similares a los que usan el enrutamiento y el lenguaje C#. Es necesario usar secuencias de escape con los tokens de expresiones regulares. Para usar la expresión regular `^\d{3}-\d{2}-\d{4}$` en el enrutamiento, los caracteres `\` (una barra diagonal inversa) de la cadena se deben proporcionar como caracteres `\\` (barra diagonal inversa doble) en el archivo de código fuente de C# para que el carácter de escape de cadena `\` tenga una secuencia de escape (a menos que se usen [literales de cadena textuales](/dotnet/csharp/language-reference/keywords/string)). Para aplicar secuencias de escape a los caracteres delimitadores de parámetro de enrutamiento (`{`, `}`, `[` y `]`), duplique los caracteres en la expresión (`{{`, `}`, `[[` y `]]`). En la tabla siguiente se muestra una expresión regular y la versión con la secuencia de escape.

| Expresión regular    | Expresión regular con secuencia de escape     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Las expresiones regulares que se usan en el enrutamiento suelen empezar con el carácter de acento circunflejo (`^`) y coinciden con la posición inicial de la cadena. Las expresiones suelen terminar con el carácter del signo de dólar (`$`) y coincidir con el final de la cadena. Los caracteres `^` y `$` garantizan que la expresión regular coincide con el valor completo del parámetro de ruta. Sin los caracteres `^` y `$`, la expresión regular coincide con cualquier subcadena de la cadena, algo que normalmente no es deseable. En la tabla siguiente se proporcionan ejemplos y se explica por qué coinciden o no.

| Expresión   | String    | Coincidir con | Comentario               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | hello     | Sí   | Coincidencias de subcadenas     |
| `[a-z]{2}`   | 123abc456 | Sí   | Coincidencias de subcadenas     |
| `[a-z]{2}`   | mz        | Sí   | Coincide con la expresión    |
| `[a-z]{2}`   | MZ        | Sí   | No distingue mayúsculas de minúsculas    |
| `^[a-z]{2}$` | hello     | No    | Vea `^` y `$` más arriba |
| `^[a-z]{2}$` | 123abc456 | No    | Vea `^` y `$` más arriba |

Para obtener más información sobre la sintaxis de expresiones regulares, vea [Expresiones regulares de .NET Framework](/dotnet/standard/base-types/regular-expression-language-quick-reference).

Para restringir un parámetro a un conjunto conocido de valores posibles, use una expresión regular. Por ejemplo, `{action:regex(^(list|get|create)$)}` solo hace coincidir el valor de ruta `action` con `list`, `get` o `create`. Si se pasa al diccionario de restricciones, la cadena `^(list|get|create)$` es equivalente. Las restricciones que se pasan al diccionario de restricciones (no insertado en una plantilla) que no coinciden con una de las restricciones conocidas también se tratan como expresiones regulares.

## <a name="custom-route-constraints"></a>Restricciones de ruta personalizadas

Además de las restricciones de ruta integradas, se pueden crear restricciones de ruta personalizadas implementando la interfaz <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>. La interfaz <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> contiene un único método, `Match`, que devuelve `true` si se cumple la restricción, y `false` en caso contrario.

Para utilizar una restricción <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> personalizada, el tipo de restricción de ruta debe registrarse con el parámetro <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> de la aplicación en el contenedor de servicios de la aplicación. <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> es un diccionario que asigna claves de restricciones de ruta a implementaciones de <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> que validen esas restricciones. El parámetro <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> de una aplicación puede actualizarse en `Startup.ConfigureServices` como parte de una llamada a [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) o configurando <xref:Microsoft.AspNetCore.Routing.RouteOptions> directamente con `services.Configure<RouteOptions>`. Por ejemplo:

```csharp
services.AddRouting(options =>
{
    options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
});
```

La restricción puede aplicarse a las rutas de la manera habitual, usando el nombre especificado al registrar el tipo de restricción. Por ejemplo:

```csharp
[HttpGet("{id:customName}")]
public ActionResult<string> Get(string id)
```

## <a name="parameter-transformer-reference"></a>Referencia de transformadores de parámetros

Transformadores de parámetros:

* Se ejecutan al generar un vínculo para <xref:Microsoft.AspNetCore.Routing.Route>.
* Implemente `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`.
* Se configuran con <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>.
* Toman el valor de ruta del parámetro y lo transforman en un nuevo valor de cadena.
* Como resultado, el valor transformado se usa en el vínculo generado.

Por ejemplo, un transformador de parámetros personalizado `slugify` en el patrón de ruta `blog\{article:slugify}` con `Url.Action(new { article = "MyTestArticle" })` genera `blog\my-test-article`.

Para usar un transformador de parámetros en un patrón de ruta, configúrelo primero con <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> en `Startup.ConfigureServices`:

```csharp
services.AddRouting(options =>
{
    // Replace the type and the name used to refer to it with your own
    // IOutboundParameterTransformer implementation
    options.ConstraintMap["slugify"] = typeof(SlugifyParameterTransformer);
});
```

El marco usa los transformadores de parámetros para transformar el URI en el que se resuelve un punto de conexión. Por ejemplo, ASP.NET Core MVC usa transformadores de parámetros para transformar el valor de ruta usado para hacer coincidir elementos `area`, `controller`, `action` y `page`.

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller:slugify=Home}/{action:slugify=Index}/{id?}");
```

Con la ruta anterior, la acción `SubscriptionManagementController.GetAll()` coincide con el URI `/subscription-management/get-all`. Un transformador de parámetros no cambia los valores de ruta usados para generar un vínculo. Por ejemplo, `Url.Action("GetAll", "SubscriptionManagement")` genera `/subscription-management/get-all`.

ASP.NET Core proporciona las convenciones de API para usar transformadores de parámetros con rutas generadas:

* ASP.NET Core MVC tiene la convención de API `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention`. Esta convención aplica un transformador de parámetro especificado a todas las rutas de atributo de la aplicación. El transformador de parámetro transforma los tokens de rutas de atributo a medida que se reemplazan. Para más información, consulte [Usar un transformador de parámetro para personalizar el reemplazo de tokens](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).
* Razor Pages tiene la convención de API `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention`. Esta convención aplica un transformador de parámetros especificado a todas las instancias de Razor Pages detectadas de forma automática. El transformador de parámetros transforma los segmentos de nombre de archivo y carpeta de las rutas de Razor Pages. Para más información, consulte el artículo sobre cómo [usar un transformador de parámetro para personalizar rutas de Razor Pages](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).

## <a name="url-generation-reference"></a>Referencia de generación de direcciones URL

En el ejemplo siguiente se muestra cómo se genera un vínculo a una ruta, dado un diccionario de valores de ruta y un valor <xref:Microsoft.AspNetCore.Routing.RouteCollection>.

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

El valor <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> generado al final del ejemplo anterior es `/package/create/123`. El diccionario proporciona los valores de ruta `operation` e `id` de la plantilla "Ruta de paquete de seguimiento", `package/{operation}/{id}`. Para obtener más información, vea el código de ejemplo de la sección [Uso de software intermedio de enrutamiento](#use-routing-middleware) o la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples).

El segundo parámetro del constructor <xref:Microsoft.AspNetCore.Routing.VirtualPathContext> es una colección de *valores de ambiente*. Los valores de ambiente son adecuados porque limitan el número de valores que el desarrollador debe especificar dentro de un contexto de solicitud. Los valores de ruta actuales de la solicitud actual se consideran valores de ambiente para la generación de vínculos. En la acción `About` de `HomeController` de una aplicación ASP.NET Core MVC, no es necesario especificar el valor de ruta de controlador para vincular a la acción `Index` (se usará el valor de ambiente `Home`).

Los valores de ambiente que no coincidan con un parámetro se omiten. También se omiten los valores de ambiente cuando un valor proporcionado de forma explícita invalida el valor de ambiente. La coincidencia se produce de izquierda a derecha en la dirección URL.

Los valores que se proporcionan de forma explícita pero que no coinciden con un segmento de la ruta se agregan a la cadena de consulta. En la tabla siguiente se muestra el resultado cuando se usa la plantilla de ruta `{controller}/{action}/{id?}`.

| Valores de ambiente                     | Valores explícitos                        | Resultado                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| controller = "Home"                | action = "About"                       | `/Home/About`           |
| controller = "Home"                | controller = "Order", action = "About" | `/Order/About`          |
| controller = "Home", color = "Red" | action = "About"                       | `/Home/About`           |
| controller = "Home"                | action = "About", color = "Red"        | `/Home/About?color=Red` |

Si una ruta tiene un valor predeterminado que no se corresponde con un parámetro y ese valor se proporciona de forma explícita, debe coincidir con el valor predeterminado:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

La generación de vínculos solo genera un vínculo para esta ruta si se proporcionan los valores coincidentes para `controller` y `action`.

## <a name="complex-segments"></a>Segmentos complejos

Los segmentos complejos (por ejemplo, `[Route("/x{token}y")]`), se procesan buscando coincidencias de literales de derecha a izquierda de un modo no expansivo. Consulte [este código](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) para obtener una explicación detallada de cómo se comparan los segmentos complejos. El [código de ejemplo](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) no se usa en ASP.NET Core, pero proporciona una buena explicación de los segmentos complejos.
<!-- While that code is no longer used by ASP.NET Core for complex segment matching, it provides a good match to the current algorithm. The [current code](https://github.com/aspnet/AspNetCore/blob/91514c9af7e0f4c44029b51f05a01c6fe4c96e4c/src/Http/Routing/src/Matching/DfaMatcherBuilder.cs#L227-L244) is too abstracted from matching to be useful for understanding complex segment matching.
-->

::: moniker-end

::: moniker range="< aspnetcore-2.2"

El enrutamiento es responsable de asignar los URI de solicitud a los controladores de ruta y de distribuir las solicitudes entrantes. Las rutas se definen en la aplicación y se configuran cuando se inicia la aplicación. Una ruta puede extraer opcionalmente valores de la dirección URL contenida en la solicitud, que se pueden usar para procesar las solicitudes. Mediante las rutas configuradas de la aplicación, el enrutamiento puede generar direcciones URL que se asignan a los controladores de ruta.

Para usar los escenarios de enrutamiento más recientes de ASP.NET Core 2.1, especifique la [versión de compatibilidad](xref:mvc/compatibility-version) en el registro de servicios de MVC en `Startup.ConfigureServices`:

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

> [!IMPORTANT]
> En este documento se describe el enrutamiento de ASP.NET Core de bajo nivel. Para obtener información sobre el enrutamiento de ASP.NET Core MVC, vea <xref:mvc/controllers/routing>. Para obtener más información sobre las convenciones de enrutamiento en Razor Pages, consulte <xref:razor-pages/razor-pages-conventions>.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="routing-basics"></a>Fundamentos del enrutamiento

La mayoría de las aplicaciones deben elegir un esquema de enrutamiento básico y descriptivo para que las direcciones URL sean legibles y significativas. La ruta convencional predeterminada `{controller=Home}/{action=Index}/{id?}`:

* Admite un esquema de enrutamiento básico y descriptivo.
* Se trata de un punto de partida útil para las aplicaciones basadas en la interfaz de usuario.

Los desarrolladores suelen agregar rutas breves adicionales a áreas de mucho tráfico de una aplicación en situaciones especializadas (por ejemplo, puntos de conexión de blog y comercio electrónico) con el [enrutamiento mediante atributos](xref:mvc/controllers/routing#attribute-routing) o rutas convencionales dedicadas.

Las API web deben usar el enrutamiento mediante atributos para modelar la funcionalidad de la aplicación como un conjunto de recursos donde las operaciones se representan mediante verbos HTTP. Esto significa que muchas operaciones (por ejemplo, GET y POST) del mismo recurso lógico usarán la misma dirección URL. El enrutamiento mediante atributos proporciona un nivel de control que es necesario para diseñar cuidadosamente un diseño de puntos de conexión públicos de la API.

En las aplicaciones de Razor Pages se usa el enrutamiento convencional predeterminado para proporcionar recursos con nombre en la carpeta *Páginas* de una aplicación. Existen convenciones adicionales que permiten personalizar el comportamiento de enrutamiento de Razor Pages. Para obtener más información, vea <xref:razor-pages/index> y <xref:razor-pages/razor-pages-conventions>.

La compatibilidad de la generación de direcciones URL permite desarrollar la aplicación sin codificar de forma rígida las direcciones URL para vincular la aplicación. Esta compatibilidad permite empezar con una configuración de enrutamiento básica y modificar las rutas una vez determinado el diseño de los recursos de la aplicación.

El enrutamiento usa *rutas* (implementaciones de <xref:Microsoft.AspNetCore.Routing.IRouter>) para:

* Asignar las solicitudes entrantes a *controladores de ruta*.
* Generar las direcciones URL que se usan en las respuestas.

De forma predeterminada, una aplicación tiene una sola colección de rutas. Cuando llega una solicitud, las rutas de la colección se procesan en el orden en el que se encuentran en la colección. El marco de trabajo intenta hacer coincidir una dirección URL de solicitud entrante con una ruta de la colección mediante una llamada al método <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> en cada ruta de la colección. Una respuesta puede usar el enrutamiento para generar direcciones URL (por ejemplo, para el redireccionamiento o los vínculos) en función de la información de ruta. De este modo, se evita codificar de forma rígida las direcciones URL, lo que facilita el mantenimiento.

El sistema de enrutamiento tiene las características siguientes:

* La sintaxis de plantilla de ruta se usa para definir las rutas con parámetros de ruta con tokens.
* Se permite la configuración de puntos de conexión de estilo convencional y de estilo de atributo.
* Se usa <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> para determinar si un parámetro de dirección URL contiene un valor válido para una restricción de punto de conexión determinada.
* Los modelos de aplicación, como MVC y Razor Pages, registran todas sus rutas, que tienen una implementación predecible de los escenarios de enrutamiento.
* Una respuesta puede usar el enrutamiento para generar direcciones URL (por ejemplo, para el redireccionamiento o los vínculos) en función de la información de ruta. De este modo, se evita codificar de forma rígida las direcciones URL, lo que facilita el mantenimiento.
* La generación de direcciones URL se basa en rutas, que admiten la extensibilidad arbitraria. <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> ofrece métodos para generar direcciones URL.

La clase <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> conecta el enrutamiento a la canalización de [software intermedio](xref:fundamentals/middleware/index). [ASP.NET Core MVC](xref:mvc/overview) agrega enrutamiento a la canalización de middleware como parte de su configuración y controla el enrutamiento en las aplicaciones de MVC y Razor Pages. Para obtener información sobre cómo usar el enrutamiento como componente independiente, vea la sección [Uso de software intermedio de enrutamiento](#use-routing-middleware).

### <a name="url-matching"></a>Coincidencia de dirección URL

La coincidencia de dirección URL es el proceso por el cual el enrutamiento envía una solicitud entrante a un *controlador*. Este proceso se basa en datos de la ruta de dirección URL, pero se puede ampliar para tener en cuenta cualquier dato de la solicitud. La capacidad de enviar solicitudes a controladores independientes es clave para escalar el tamaño y la complejidad de una aplicación.

Las solicitudes entrantes especifican la clase <xref:Microsoft.AspNetCore.Builder.RouterMiddleware>, que llama al método <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> en cada ruta de la secuencia. La instancia de <xref:Microsoft.AspNetCore.Routing.IRouter> decide si *controla* la solicitud mediante el establecimiento de [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) en un <xref:Microsoft.AspNetCore.Http.RequestDelegate> que no sea NULL. Si una ruta establece un controlador para la solicitud, el procesamiento de rutas se detiene y se invoca el controlador para procesar la solicitud. Si no se encuentra ningún controlador de ruta para procesar la solicitud, el middleware entrega la solicitud al siguiente middleware en la canalización de solicitudes.

La entrada principal para <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> es el [RouteContext.HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*) asociado a la solicitud actual. [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler) y [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) son salidas que se establecen después de que una ruta coincida.

Una coincidencia que llama a <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> también establece las propiedades de [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) en los valores adecuados en función del procesamiento de solicitudes realizado hasta el momento.

[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) es un diccionario de los *valores de ruta* generados desde la ruta. Estos valores se suelen determinar mediante la conversión en tokens de la dirección URL, y se pueden usar para aceptar la entrada del usuario o para tomar otras decisiones sobre el envío dentro de la aplicación.

[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) es un contenedor de propiedades de datos adicionales relacionados con la ruta coincidente. Se proporcionan <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> para permitir la asociación de datos de estado con cada ruta, de modo que la aplicación pueda tomar decisiones en función de las rutas que han coincidido. Estos valores los define el desarrollador y **no** afectan de ninguna manera al comportamiento del enrutamiento. Además, los valores que se guardan provisionalmente en [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) pueden ser de cualquier tipo, a diferencia de [RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values), que deben poder convertirse fácilmente en cadenas y a partir de estas.

[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) es una lista de las rutas que han participado en encontrar una coincidencia correcta con la solicitud. Las rutas se pueden anidar unas dentro de otras. La propiedad <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> refleja la ruta de acceso del árbol lógico de rutas que han tenido como resultado una coincidencia. Por lo general, el primer elemento de <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> es la colección de rutas y se debe usar para la generación de direcciones URL. El último elemento de <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> es el controlador de ruta que ha coincidido.

<a name="lg"></a>

### <a name="url-generation"></a>Generación de dirección URL

La generación de dirección URL es el proceso por el cual el enrutamiento puede crear una ruta de dirección URL basada en un conjunto de valores de ruta. Esto permite una separación lógica entre los controladores de ruta y las direcciones URL que tienen acceso a ellos.

La generación de direcciones URL sigue un proceso iterativo similar, pero se inicia cuando el código de usuario o de marco de trabajo llama al método <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> de la colección de rutas. Se llama en secuencia al método <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> de cada *ruta* hasta que se devuelva un valor <xref:Microsoft.AspNetCore.Routing.VirtualPathData> distinto de NULL.

La principal entradas de <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> son:

* [VirtualPathContext.HttpContext](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext)
* [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values)
* [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues)

Las rutas usan principalmente los valores de ruta proporcionados por <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> y <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> para decidir si es posible generar una dirección URL y qué valores se van a incluir. <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> son el conjunto de valores de ruta producidos a partir de la coincidencia con la solicitud actual. En cambio, <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> son los valores de ruta que especifican cómo se genera la dirección URL deseada para la operación actual. Se proporciona <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext> por si una ruta debe obtener servicios o datos adicionales asociados con el contexto actual.

> [!TIP]
> Piense en [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) como un conjunto de invalidaciones para [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*). La generación de direcciones URL intenta reutilizar los valores de ruta de la solicitud actual para generar direcciones URL para los vínculos con la misma ruta o valores de ruta.

La salida de <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> es <xref:Microsoft.AspNetCore.Routing.VirtualPathData>. <xref:Microsoft.AspNetCore.Routing.VirtualPathData> es un valor paralelo de <xref:Microsoft.AspNetCore.Routing.RouteData>. <xref:Microsoft.AspNetCore.Routing.VirtualPathData> contiene el valor <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> de la dirección URL de salida y algunas propiedades más que la ruta debe establecer.

La propiedad [VirtualPathData.VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) contiene la *ruta de acceso virtual* generada por la ruta. Es posible que deba procesar aún más la ruta de acceso, según sus necesidades. Si quiere representar la dirección URL generada en HTML, anteponga la ruta de acceso base de la aplicación.

[VirtualPathData.Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) es una referencia a la ruta que ha generado correctamente la dirección URL.

La propiedad [VirtualPathData.DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) es un diccionario de datos adicionales relacionados con la ruta que ha generado la dirección URL. Se trata del valor paralelo de [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*).

### <a name="create-routes"></a>Creación de rutas

El enrutamiento proporciona la clase <xref:Microsoft.AspNetCore.Routing.Route> como implementación estándar de <xref:Microsoft.AspNetCore.Routing.IRouter>. <xref:Microsoft.AspNetCore.Routing.Route> usa la sintaxis de *plantilla de ruta* para definir patrones que se hacen coincidir con la ruta de dirección URL cuando se llama a <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*>. <xref:Microsoft.AspNetCore.Routing.Route> usa la misma plantilla de ruta para generar una dirección URL cuando se llama a <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*>.

La mayoría de las aplicaciones crea rutas mediante una llamada a <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> o a uno de los métodos de extensión similares definidos en <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>. Todos los métodos de extensión <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> crean una instancia de <xref:Microsoft.AspNetCore.Routing.Route> y la agregan a la colección de rutas.

<xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> no acepta un parámetro de controlador de ruta. <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> solo agrega las rutas que se controlan mediante <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>. El controlador predeterminado es un elemento `IRouter`, y es posible que el controlador no pueda atender la solicitud. Por ejemplo, ASP.NET Core MVC se suele configurar como un controlador predeterminado que solo controla las solicitudes que coinciden con un controlador y una acción disponibles. Para obtener más información sobre el enrutamiento en MVC, vea <xref:mvc/controllers/routing>.

El código siguiente es un ejemplo de una llamada a <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> usada por una definición de ruta típica de ASP.NET Core MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Esta plantilla coincide con una ruta de dirección URL y extrae los valores de ruta. Por ejemplo, la ruta de acceso `/Products/Details/17` genera los valores de ruta siguientes: `{ controller = Products, action = Details, id = 17 }`.

Los valores de ruta se determinan al dividir la ruta de dirección URL en segmentos y hacer coincidir cada segmento con el nombre del *parámetro de ruta* en la plantilla de ruta. Los parámetros de ruta tienen un nombre asignado. Para definir los parámetros, el nombre del parámetro se incluye entre llaves `{ ... }`.

La plantilla anterior también podría coincidir con la ruta de dirección URL `/` y generar los valores `{ controller = Home, action = Index }`. Esto se debe a que los parámetros de ruta `{controller}` y `{action}` tienen valores predeterminados y el parámetro de ruta `id` es opcional. Un signo igual (`=`) seguido de un valor después del nombre del parámetro de ruta define un valor predeterminado para el parámetro. Un signo de interrogación (`?`) después del nombre del parámetro de ruta define un parámetro opcional.

Los parámetros de ruta con un valor predeterminado *siempre* generan un valor de ruta cuando la ruta coincide. Los parámetros opcionales no generan un valor de ruta si no existió el segmento de ruta de dirección URL correspondiente. Vea la sección [Referencia de plantilla de ruta](#route-template-reference) para obtener una descripción detallada de los escenarios y la sintaxis de la plantilla de ruta.

En el ejemplo siguiente, la definición de parámetro de ruta `{id:int}` define una [restricción de ruta](#route-constraint-reference) para el parámetro de ruta `id`:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Esta plantilla coincide con una ruta de dirección URL como `/Products/Details/17`, pero no con `/Products/Details/Apples`. Las restricciones de ruta implementan <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> e inspeccionan los valores de ruta para comprobarlos. En este ejemplo, el valor de ruta `id` debe poder convertirse en un entero. Vea la [Referencia de restricción de ruta](#route-constraint-reference) para obtener una explicación de las restricciones de ruta proporcionadas por el marco de trabajo.

Las sobrecargas adicionales de <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> aceptan valores para `constraints`, `dataTokens` y `defaults`. Estos parámetros suelen usarse para pasar un objeto de tipo anónimo, donde los nombres de propiedad del tipo anónimo coinciden con los nombres de los parámetros de ruta.

En los ejemplos <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> siguientes se crean rutas equivalentes:

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
> La sintaxis insertada para definir las restricciones y los valores predeterminados puede ser conveniente para las rutas simples. Pero hay algunos escenarios, como los tokens de datos, que no son compatibles con la sintaxis insertada.

En el ejemplo siguiente se muestran algunos escenarios más:

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{*article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

La plantilla anterior coincide con una ruta de dirección URL como `/Blog/All-About-Routing/Introduction` y extrae los valores `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. La ruta genera los valores de ruta predeterminados para `controller` y `action`, incluso si no hay parámetros de ruta correspondientes en la plantilla. Los valores predeterminados pueden especificarse en la plantilla de ruta. El parámetro de ruta `article` se define como un *comodín* por la aparición de un asterisco (`*`) antes del nombre del parámetro de ruta. Los parámetros de ruta comodín capturan el resto de la ruta de dirección URL y también pueden coincidir con la cadena vacía.

En el ejemplo siguiente se agregan restricciones de ruta y tokens de datos:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

La plantilla anterior coincide con una ruta de dirección URL como `/en-US/Products/5` y extrae los valores `{ controller = Products, action = Details, id = 5 }` y los tokens de datos `{ locale = en-US }`.

![Tokens de Windows de variables locales](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a>Generación de direcciones URL de clase de ruta

La clase <xref:Microsoft.AspNetCore.Routing.Route> también puede llevar a cabo la generación de dirección URL mediante la combinación de un conjunto de valores de ruta con su plantilla de ruta. Se trata lógicamente del proceso inverso de hacer coincidir la ruta de dirección URL.

> [!TIP]
> Para entender mejor la generación de direcciones URL, imagine qué dirección URL quiere generar y, después, piense cómo coincidiría una plantilla de ruta con esa dirección URL. ¿Qué valores se generarían? Este es un equivalente aproximado de cómo funciona la generación de dirección URL en la clase <xref:Microsoft.AspNetCore.Routing.Route>.

En el ejemplo siguiente se usa una ruta predeterminada de ASP.NET Core MVC general:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Con los valores de ruta `{ controller = Products, action = List }`, se genera la dirección URL `/Products/List`. Los valores de ruta se sustituyen por los parámetros de ruta correspondientes para formar la ruta de dirección URL. Como `id` es un parámetro de ruta opcional, la dirección URL se ha generado correctamente sin ningún valor para `id`.

Con los valores de ruta `{ controller = Home, action = Index }`, se genera la dirección URL `/`. Los valores de ruta proporcionados coinciden con los valores predeterminados, y los segmentos correspondientes a los valores predeterminados se omiten sin ningún riesgo.

Ambas direcciones URL generadas realizan un recorrido de ida y vuelta con la definición de ruta siguiente (`/Home/Index` y `/`) y generan los mismos valores de ruta que se usaron para generar la dirección URL.

> [!NOTE]
> Las aplicaciones con ASP.NET Core MVC deben usar <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> para generar direcciones URL en lugar de llamar directamente al enrutamiento.

Para obtener más información sobre la generación de direcciones URL, vea la sección [Referencia de generación de direcciones URL](#url-generation-reference).

## <a name="use-routing-middleware"></a>Uso de software intermedio de enrutamiento

Haga referencia al [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) en el archivo de proyecto de la aplicación.

Agregue enrutamiento al contenedor de servicios en `Startup.ConfigureServices`:

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Las rutas se deben configurar en el método `Startup.Configure`. En la aplicación de ejemplo se usan las API siguientes:

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; solo coincide con solicitudes HTTP GET.
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

En la tabla siguiente se muestran las respuestas con los URI especificados.

| Identificador URI                    | Respuesta                                          |
| ---------------------- | ------------------------------------------------- |
| `/package/create/3`    | Hello! Valores de ruta: [operation, create], [id, 3] |
| `/package/track/-3`    | Hello! Valores de ruta: [operation, track], [id, -3] |
| `/package/track/-3/`   | Hello! Valores de ruta: [operation, track], [id, -3] |
| `/package/track/`      | La solicitud pasa; no hay ninguna coincidencia.              |
| `GET /hello/Joe`       | Hi, Joe!                                          |
| `POST /hello/Joe`      | La solicitud pasa; solo coincide con HTTP GET. |
| `GET /hello/Joe/Smith` | La solicitud pasa; no hay ninguna coincidencia.              |

Si va a configurar una única ruta, pase una instancia de `IRouter` para llamar a <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>. No tendrá que usar <xref:Microsoft.AspNetCore.Routing.RouteBuilder>.

El marco de trabajo proporciona un conjunto de métodos de extensión para crear rutas (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>):

* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareVerb*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>

Algunos de los métodos enumerados, como <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>, requieren un <xref:Microsoft.AspNetCore.Http.RequestDelegate>. El valor <xref:Microsoft.AspNetCore.Http.RequestDelegate> se usa como *controlador de ruta* cuando la ruta coincida. Otros métodos de esta familia permiten configurar una canalización de software intermedio para usarla como controlador de ruta. Si el método `Map*` no acepta un controlador, como <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>, usa <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.

Los métodos `Map[Verb]` usan restricciones para limitar la ruta al verbo HTTP en el nombre del método. Por ejemplo, vea <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> y <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.

## <a name="route-template-reference"></a>Referencia de plantilla de ruta

Los tokens entre llaves (`{ ... }`) definen *parámetros de ruta* que se enlazan si se encuentran coincidencias con la ruta. Puede definir más de un parámetro de ruta en un segmento de ruta, pero deben estar separados por un valor literal. Por ejemplo, `{controller=Home}{action=Index}` no es una ruta válida, ya que no hay ningún valor literal entre `{controller}` y `{action}`. Estos parámetros de ruta deben tener un nombre y, opcionalmente, atributos adicionales especificados.

El texto literal diferente de los parámetros de ruta (por ejemplo, `{id}`) y el separador de ruta `/` deben coincidir con el texto de la dirección URL. La coincidencia de texto no distingue mayúsculas de minúsculas y se basa en la representación descodificada de la ruta de las direcciones URL. Para que el delimitador de parámetro de ruta literal coincida (`{` o `}`), repita el carácter (`{{` o `}}`) para usarlo como secuencia de escape.

Los patrones de dirección URL que intentan capturar un nombre de archivo con una extensión de archivo opcional tienen consideraciones adicionales. Por ejemplo, considere la plantilla `files/{filename}.{ext?}`. Cuando existen valores para `filename` y `ext`, los dos valores se rellenan. Si solo existe un valor para `filename` en la dirección URL, la ruta coincide porque el punto final (`.`) es opcional. Las direcciones URL siguientes coinciden con esta ruta:

* `/files/myFile.txt`
* `/files/myFile`

Se puede usar el asterisco (`*`) como prefijo de un parámetro de ruta para enlazar con el resto del URI. Es lo que se denomina un parámetro *comodín*. Por ejemplo, `blog/{*slug}` coincide con cualquier URI que empiece por `/blog` y que vaya seguido de cualquier valor, que se asigna al valor de ruta `slug`. Los parámetros comodín también pueden coincidir con una cadena vacía.

El parámetro catch-all inserta los caracteres de escape correspondientes cuando se usa la ruta para generar una dirección URL, incluidos caracteres de separación de ruta de acceso (`/`). Por ejemplo, la ruta `foo/{*path}` con valores de ruta `{ path = "my/path" }` genera `foo/my%2Fpath`. Tenga en cuenta la barra diagonal de escape.

Los parámetros de ruta pueden tener *valores predeterminados* designados mediante la especificación del valor predeterminado después del nombre de parámetro, separado por un signo igual (`=`). Por ejemplo, `{controller=Home}` define `Home` como el valor predeterminado de `controller`. El valor predeterminado se usa si no hay ningún valor en la dirección URL para el parámetro. Los parámetros de ruta se pueden convertir en opcionales si se anexa un signo de interrogación (`?`) al final del nombre del parámetro, como en `id?`. La diferencia entre los valores opcionales y los parámetros de ruta predeterminados es que un parámetro de ruta con un valor predeterminado siempre genera un valor, mientras que un parámetro opcional tiene un valor solo cuando la dirección URL de solicitud lo proporciona.

Los parámetros de ruta pueden tener restricciones que deben coincidir con el valor de ruta enlazado desde la dirección URL. Si se agrega un carácter de dos puntos (`:`) y un nombre de restricción después del nombre del parámetro de ruta, se especifica una *restricción insertada* en un parámetro de ruta. Si la restricción requiere argumentos, se incluyen entre paréntesis (`(...)`) después del nombre de restricción. Se pueden especificar varias restricciones insertadas si se anexa otro carácter de dos puntos (`:`) y un nombre de restricción.

El nombre de restricción y los argumentos se pasan al servicio <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> para crear una instancia de <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> para su uso en el procesamiento de direcciones URL. Por ejemplo, la plantilla de ruta `blog/{article:minlength(10)}` especifica una restricción `minlength` con el argumento `10`. Para obtener más información sobre las restricciones de ruta y una lista de las restricciones proporcionadas por el marco de trabajo, vea la sección [Referencia de restricciones de ruta](#route-constraint-reference).

En la tabla siguiente se muestran algunas plantillas de ruta de ejemplo y su comportamiento.

| Plantilla de ruta                           | URI coincidente de ejemplo    | El URI de la solicitud&hellip;                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | Solo coincide con la ruta de acceso única `/hello`.                                     |
| `{Page=Home}`                            | `/`                     | Coincide y establece `Page` en `Home`.                                         |
| `{Page=Home}`                            | `/Contact`              | Coincide y establece `Page` en `Contact`.                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | Se asigna al controlador `Products` y la acción `List`.                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | Se asigna al controlador `Products` y la acción `Details` (`id` se establece en 123). |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | Se asigna al controlador `Home` y al método `Index` (`id` se omite).        |

El uso de una plantilla suele ser el método de enrutamiento más sencillo. Las restricciones y los valores predeterminados también se pueden especificar fuera de la plantilla de ruta.

> [!TIP]
> Habilite el [registro](xref:fundamentals/logging/index) para ver de qué forma las implementaciones de enrutamiento integradas, como <xref:Microsoft.AspNetCore.Routing.Route>, coinciden con las solicitudes.

## <a name="reserved-routing-names"></a>Nombres de enrutamientos reservados

Las siguientes palabras clave son nombres reservados y no se pueden usar como nombres de ruta o parámetros:

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Referencia de restricción de ruta

Las restricciones de ruta se ejecutan cuando se ha producido una coincidencia con la dirección URL entrante y la ruta de dirección URL se convierte en tokens en valores de ruta. En general, las restricciones de ruta inspeccionan el valor de ruta asociado a través de la plantilla de ruta y deciden si el valor es aceptable o no. Algunas restricciones de ruta usan datos ajenos al valor de ruta para decidir si la solicitud se puede enrutar. Por ejemplo, <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> puede aceptar o rechazar una solicitud basada en su verbo HTTP. Las restricciones se usan en las solicitudes de enrutamiento y la generación de vínculos.

> [!WARNING]
> No use las restricciones para las **validación de entrada**. Si las restricciones se usan para la **validación de entrada**, los resultados de entrada no válidos producirán un error *404 - No encontrado* en lugar de un error *400 - Solicitud incorrecta* con un mensaje de error adecuado. Las restricciones de ruta se usan para **eliminar la ambigüedad** entre rutas similares, no para validar las entradas de una ruta determinada.

En la tabla siguiente se muestran algunas restricciones de ruta de ejemplo y su comportamiento esperado.

| restricción | Ejemplo | Coincidencias de ejemplo | Notas |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789` | Coincide con cualquier entero |
| `bool` | `{active:bool}` | `true`, `FALSE` | Coincide con `true` o `false` (no distingue mayúsculas de minúsculas) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | Coincide con un valor `DateTime` válido (en la referencia cultural invariable; vea la advertencia) |
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
| `required` | `{name:required}` | `Rick` | Se usa para exigir que un valor que no es de parámetro esté presente durante la generación de dirección URL |

Es posible aplicar varias restricciones delimitadas por dos puntos a un único parámetro. Por ejemplo, la siguiente restricción permite limitar un parámetro a un valor entero de 1 o superior:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> Las restricciones de ruta que comprueban la dirección URL y que se convierten en un tipo CLR (como `int` o `DateTime`) usan siempre la referencia cultural invariable. Estas restricciones dan por supuesto que la dirección URL no es localizable. Las restricciones de ruta proporcionadas por el marco de trabajo no modifican los valores almacenados en los valores de ruta. Todos los valores de ruta analizados desde la dirección URL se almacenan como cadenas. Por ejemplo, la restricción `float` intenta convertir el valor de ruta en un valor Float, pero el valor convertido se usa exclusivamente para comprobar que se puede convertir en Float.

## <a name="regular-expressions"></a>Expresiones regulares

El marco de trabajo de ASP.NET Core agrega `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` al constructor de expresiones regulares. Vea <xref:System.Text.RegularExpressions.RegexOptions> para obtener una descripción de estos miembros.

Las expresiones regulares usan delimitadores y tokens similares a los que usan el enrutamiento y el lenguaje C#. Es necesario usar secuencias de escape con los tokens de expresiones regulares. Para usar la expresión regular `^\d{3}-\d{2}-\d{4}$` en el enrutamiento, los caracteres `\` (una barra diagonal inversa) de la cadena se deben proporcionar como caracteres `\\` (barra diagonal inversa doble) en el archivo de código fuente de C# para que el carácter de escape de cadena `\` tenga una secuencia de escape (a menos que se usen [literales de cadena textuales](/dotnet/csharp/language-reference/keywords/string)). Para aplicar secuencias de escape a los caracteres delimitadores de parámetro de enrutamiento (`{`, `}`, `[` y `]`), duplique los caracteres en la expresión (`{{`, `}`, `[[` y `]]`). En la tabla siguiente se muestra una expresión regular y la versión con la secuencia de escape.

| Expresión regular    | Expresión regular con secuencia de escape     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Las expresiones regulares que se usan en el enrutamiento suelen empezar con el carácter de acento circunflejo (`^`) y coinciden con la posición inicial de la cadena. Las expresiones suelen terminar con el carácter del signo de dólar (`$`) y coincidir con el final de la cadena. Los caracteres `^` y `$` garantizan que la expresión regular coincide con el valor completo del parámetro de ruta. Sin los caracteres `^` y `$`, la expresión regular coincide con cualquier subcadena de la cadena, algo que normalmente no es deseable. En la tabla siguiente se proporcionan ejemplos y se explica por qué coinciden o no.

| Expresión   | String    | Coincidir con | Comentario               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | hello     | Sí   | Coincidencias de subcadenas     |
| `[a-z]{2}`   | 123abc456 | Sí   | Coincidencias de subcadenas     |
| `[a-z]{2}`   | mz        | Sí   | Coincide con la expresión    |
| `[a-z]{2}`   | MZ        | Sí   | No distingue mayúsculas de minúsculas    |
| `^[a-z]{2}$` | hello     | No    | Vea `^` y `$` más arriba |
| `^[a-z]{2}$` | 123abc456 | No    | Vea `^` y `$` más arriba |

Para obtener más información sobre la sintaxis de expresiones regulares, vea [Expresiones regulares de .NET Framework](/dotnet/standard/base-types/regular-expression-language-quick-reference).

Para restringir un parámetro a un conjunto conocido de valores posibles, use una expresión regular. Por ejemplo, `{action:regex(^(list|get|create)$)}` solo hace coincidir el valor de ruta `action` con `list`, `get` o `create`. Si se pasa al diccionario de restricciones, la cadena `^(list|get|create)$` es equivalente. Las restricciones que se pasan al diccionario de restricciones (no insertado en una plantilla) que no coinciden con una de las restricciones conocidas también se tratan como expresiones regulares.

## <a name="custom-route-constraints"></a>Restricciones de ruta personalizadas

Además de las restricciones de ruta integradas, se pueden crear restricciones de ruta personalizadas implementando la interfaz <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>. La interfaz <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> contiene un único método, `Match`, que devuelve `true` si se cumple la restricción, y `false` en caso contrario.

Para utilizar una restricción <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> personalizada, el tipo de restricción de ruta debe registrarse con el parámetro <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> de la aplicación en el contenedor de servicios de la aplicación. <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> es un diccionario que asigna claves de restricciones de ruta a implementaciones de <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> que validen esas restricciones. El parámetro <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> de una aplicación puede actualizarse en `Startup.ConfigureServices` como parte de una llamada a [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) o configurando <xref:Microsoft.AspNetCore.Routing.RouteOptions> directamente con `services.Configure<RouteOptions>`. Por ejemplo:

```csharp
services.AddRouting(options =>
{
    options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
});
```

La restricción puede aplicarse a las rutas de la manera habitual, usando el nombre especificado al registrar el tipo de restricción. Por ejemplo:

```csharp
[HttpGet("{id:customName}")]
public ActionResult<string> Get(string id)
```

## <a name="url-generation-reference"></a>Referencia de generación de direcciones URL

En el ejemplo siguiente se muestra cómo se genera un vínculo a una ruta, dado un diccionario de valores de ruta y un valor <xref:Microsoft.AspNetCore.Routing.RouteCollection>.

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

El valor <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> generado al final del ejemplo anterior es `/package/create/123`. El diccionario proporciona los valores de ruta `operation` e `id` de la plantilla "Ruta de paquete de seguimiento", `package/{operation}/{id}`. Para obtener más información, vea el código de ejemplo de la sección [Uso de software intermedio de enrutamiento](#use-routing-middleware) o la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples).

El segundo parámetro del constructor <xref:Microsoft.AspNetCore.Routing.VirtualPathContext> es una colección de *valores de ambiente*. Los valores de ambiente son adecuados porque limitan el número de valores que el desarrollador debe especificar dentro de un contexto de solicitud. Los valores de ruta actuales de la solicitud actual se consideran valores de ambiente para la generación de vínculos. En la acción `About` de `HomeController` de una aplicación ASP.NET Core MVC, no es necesario especificar el valor de ruta de controlador para vincular a la acción `Index` (se usará el valor de ambiente `Home`).

Los valores de ambiente que no coincidan con un parámetro se omiten. También se omiten los valores de ambiente cuando un valor proporcionado de forma explícita invalida el valor de ambiente. La coincidencia se produce de izquierda a derecha en la dirección URL.

Los valores que se proporcionan de forma explícita pero que no coinciden con un segmento de la ruta se agregan a la cadena de consulta. En la tabla siguiente se muestra el resultado cuando se usa la plantilla de ruta `{controller}/{action}/{id?}`.

| Valores de ambiente                     | Valores explícitos                        | Resultado                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| controller = "Home"                | action = "About"                       | `/Home/About`           |
| controller = "Home"                | controller = "Order", action = "About" | `/Order/About`          |
| controller = "Home", color = "Red" | action = "About"                       | `/Home/About`           |
| controller = "Home"                | action = "About", color = "Red"        | `/Home/About?color=Red` |

Si una ruta tiene un valor predeterminado que no se corresponde con un parámetro y ese valor se proporciona de forma explícita, debe coincidir con el valor predeterminado:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

La generación de vínculos solo genera un vínculo para esta ruta si se proporcionan los valores coincidentes para `controller` y `action`.

## <a name="complex-segments"></a>Segmentos complejos

Los segmentos complejos (por ejemplo, `[Route("/x{token}y")]`), se procesan buscando coincidencias de literales de derecha a izquierda de un modo no expansivo. Consulte [este código](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) para obtener una explicación detallada de cómo se comparan los segmentos complejos. El [código de ejemplo](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) no se usa en ASP.NET Core, pero proporciona una buena explicación de los segmentos complejos.
<!-- While that code is no longer used by ASP.NET Core for complex segment matching, it provides a good match to the current algorithm. The [current code](https://github.com/aspnet/AspNetCore/blob/91514c9af7e0f4c44029b51f05a01c6fe4c96e4c/src/Http/Routing/src/Matching/DfaMatcherBuilder.cs#L227-L244) is too abstracted from matching to be useful for understanding complex segment matching.
-->

::: moniker-end
