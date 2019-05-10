---
title: Enrutar a acciones de controlador de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo ASP.NET Core MVC usa el middleware de enrutamiento para encontrar direcciones URL de las solicitudes entrantes y asignarlas a acciones.
ms.author: riande
ms.date: 01/24/2019
uid: mvc/controllers/routing
ms.openlocfilehash: b4d5cd3add3fda6b70873eb5cce1dcee651f9185
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087506"
---
# <a name="routing-to-controller-actions-in-aspnet-core"></a>Enrutar a acciones de controlador de ASP.NET Core

Por [Ryan Nowak](https://github.com/rynowak) y [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core MVC utiliza el [middleware](xref:fundamentals/middleware/index) de enrutamiento para buscar las direcciones URL de las solicitudes entrantes y asignarlas a acciones. Las rutas se definen en el código de inicio o los atributos. Las rutas describen cómo se deben asociar las rutas de dirección URL a las acciones. Las rutas también se usan para generar direcciones URL (para vínculos) enviadas en las respuestas.

Las acciones se enrutan bien mediante convención o bien mediante atributos. Colocar una ruta en el controlador o la acción hace que se enrute mediante atributos. Consulte [Enrutamiento mixto](#routing-mixed-ref-label) para obtener más información.

Este documento explica las interacciones entre MVC y el enrutamiento, así como el uso que las aplicaciones MVC suelen hacer de las características de enrutamiento. Consulte [Enrutamiento](xref:fundamentals/routing) para obtener más información sobre enrutamiento avanzado.

## <a name="setting-up-routing-middleware"></a>Configurar el middleware de enrutamiento

En su método *Configure* puede ver código similar al siguiente:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Dentro de la llamada a `UseMvc`, `MapRoute` se utiliza para crear una ruta única, a la que nos referiremos como la ruta `default`. La mayoría de las aplicaciones MVC usarán una ruta con una plantilla similar a la ruta `default`.

La plantilla de ruta `"{controller=Home}/{action=Index}/{id?}"` puede coincidir con una ruta de dirección URL como `/Products/Details/5` y extraerá los valores de ruta `{ controller = Products, action = Details, id = 5 }` mediante la conversión de la ruta en tokens. MVC intentará encontrar un controlador denominado `ProductsController` y ejecutar la acción `Details`:

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

Tenga en cuenta que, en este ejemplo, el enlace de modelos usaría el valor de `id = 5` para establecer el parámetro `id` en `5` al invocar esta acción. Consulte [Enlace de modelos](../models/model-binding.md) para obtener más detalles.

Utilizando la ruta `default`:

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

La plantilla de ruta:

* `{controller=Home}` define `Home` como el valor `controller` predeterminado

* `{action=Index}` define `Index` como el valor `action` predeterminado

* `{id?}` define `id` como opcional

No es necesario que los parámetros de ruta opcionales y predeterminados estén presentes en la ruta de dirección URL para una coincidencia. Consulte [Referencia de plantilla de ruta](../../fundamentals/routing.md#route-template-reference) para obtener una descripción detallada de la sintaxis de la plantilla de ruta.

`"{controller=Home}/{action=Index}/{id?}"` puede coincidir con la ruta de dirección URL `/` y generará los valores de ruta `{ controller = Home, action = Index }`. Los valores de `controller` y `action` utilizan los valores predeterminados, `id` no genera un valor porque no hay ningún segmento correspondiente en la ruta de dirección URL. MVC utilizaría estos valores de ruta para seleccionar `HomeController` y la acción `Index`:

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

Usando esta definición de controlador y la plantilla de ruta, la acción `HomeController.Index` se ejecutaría para cualquiera de las rutas de dirección URL siguientes:

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

El método de conveniencia `UseMvcWithDefaultRoute`:

```csharp
app.UseMvcWithDefaultRoute();
```

Se puede usar para reemplazar:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

`UseMvc` y `UseMvcWithDefaultRoute` agregan una instancia de `RouterMiddleware` a la canalización de middleware. MVC no interactúa directamente con middleware y usa el enrutamiento para controlar las solicitudes. MVC está conectado a las rutas a través de una instancia de `MvcRouteHandler`. El código dentro de `UseMvc` es similar al siguiente:

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

`UseMvc` no define directamente ninguna ruta, sino que agrega un marcador de posición a la colección de rutas para la ruta `attribute`. La sobrecarga `UseMvc(Action<IRouteBuilder>)` le permite agregar sus propias rutas y también admite el enrutamiento mediante atributos.  `UseMvc` y todas sus variaciones agregan un marcador de posición para la ruta de atributo (el enrutamiento mediante atributos siempre está disponible, independientemente de cómo configure `UseMvc`. `UseMvcWithDefaultRoute` define una ruta predeterminada y admite el enrutamiento mediante atributos. La sección [Enrutamiento mediante atributos](#attribute-routing-ref-label) incluye más detalles sobre este tipo de enrutamiento.

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a>Enrutamiento convencional

La ruta `default`:

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

es un ejemplo de un *enrutamiento convencional*. Llamamos a este estilo *enrutamiento convencional* porque establece una *convención* para las rutas de dirección URL:

* El primer segmento de la ruta asigna el nombre de controlador.

* El segundo asigna el nombre de la acción.

* El tercer segmento se utiliza para un identificador `id` opcional usado para asignar un modelo de entidad

Mediante esta ruta `default`, la ruta de dirección URL `/Products/List` se asigna a la acción `ProductsController.List`, y `/Blog/Article/17` se asigna a `BlogController.Article`. Esta asignación **solo** se basa en los nombres de acción y controlador, y no en espacios de nombres, ubicaciones de archivos de origen ni parámetros de método.

> [!TIP]
> Usar el enrutamiento convencional con la ruta predeterminada permite generar rápidamente la aplicación sin necesidad de elaborar un nuevo patrón de dirección URL para cada acción que se define. Para una aplicación con acciones de estilo CRUD, la coherencia en las direcciones URL en todos los controladores puede ayudar a simplificar el código y hacer que la interfaz de usuario sea más predecible.

> [!WARNING]
> `id` se define como opcional mediante la plantilla de ruta, lo que significa que las acciones se pueden ejecutar sin el identificador proporcionado como parte de la dirección URL. Normalmente lo que ocurrirá si `id` se omite de la dirección URL es que el enlace de modelos establecerá en `0` su valor y, como resultado, no se encontrará ninguna entidad en la base de datos que coincida con `id == 0`. El enrutamiento mediante atributos proporciona un mayor control que permite requerir el identificador para algunas acciones y para otras no. Por convención, la documentación incluirá parámetros opcionales como `id` cuando sea más probable que su uso sea correcto.

## <a name="multiple-routes"></a>Varias rutas

Para agregar varias rutas dentro de `UseMvc`, agregue más llamadas a `MapRoute`. Hacerlo le permite definir varias convenciones o agregar rutas convencionales que se dedican a una acción específica, como en el ejemplo siguiente:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Aquí, la ruta `blog` es una *ruta convencional dedicada*, lo que significa que utiliza el sistema de enrutamiento convencional, pero que está dedicada a una acción específica. Puesto que `controller` y `action` no aparecen en la plantilla de ruta como parámetros, solo pueden tener los valores predeterminados y, por tanto, esta ruta siempre se asignará a la acción `BlogController.Article`.

Las rutas de la colección de rutas están ordenadas y se procesan en el orden en que se hayan agregado. Por tanto, la ruta `blog` de este ejemplo se intentará antes que la ruta `default`.

> [!NOTE]
> Las *rutas convencionales dedicadas* suelen usar parámetros de ruta comodín como `{*article}` para capturar la parte restante de la ruta de dirección URL. Esto puede hacer que la ruta sea "demasiado expansiva", lo que significa que coincidirá con direcciones URL que se pretendía que coincidieran con otras rutas. Coloque las rutas "expansivas" más adelante en la tabla de rutas para resolver este problema.

### <a name="fallback"></a>Reserva

Como parte del procesamiento de la solicitud, MVC comprobará que los valores de ruta se pueden usar para buscar un controlador y la acción en la aplicación. Si los valores de ruta no coinciden con una acción, entonces la ruta no se considera una coincidencia y se intentará la ruta siguiente. Esto se denomina *reserva*, y se ha diseñado para simplificar los casos donde se superponen rutas convencionales.

### <a name="disambiguating-actions"></a>Eliminar la ambigüedad de acciones

Cuando dos acciones coinciden en el enrutamiento, MVC debe eliminar la ambigüedad para elegir el candidato "recomendado", o de lo contrario se produce una excepción. Por ejemplo:

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

Este controlador define dos acciones que coincidirían con la ruta de dirección URL `/Products/Edit/17` y los datos de ruta `{ controller = Products, action = Edit, id = 17 }`. Se trata de un patrón típico para controladores MVC donde `Edit(int)` muestra un formulario para editar un producto, y `Edit(int, Product)` procesa el formulario expuesto. Para hacer esto posible, MVC necesita seleccionar `Edit(int, Product)` cuando la solicitud es HTTP `POST` y `Edit(int)` cuando el verbo HTTP es cualquier otra cosa.

`HttpPostAttribute` ( `[HttpPost]` ) es una implementación de `IActionConstraint` que solo permitirá que la acción se seleccione cuando el verbo HTTP sea `POST`. La presencia de `IActionConstraint` hace que `Edit(int, Product)` sea una mejor coincidencia que `Edit(int)`, por lo que `Edit(int, Product)` se intentará en primer lugar.

Solo necesitará escribir implementaciones de `IActionConstraint` personalizadas en escenarios especializados, pero es importante comprender el rol de atributos como `HttpPostAttribute` (para otros verbos HTTP se definen atributos similares). En el enrutamiento convencional es común que las acciones utilicen el mismo nombre de acción cuando son parte de un flujo de trabajo `show form -> submit form`. La comodidad de este patrón será más evidente después de revisar la sección [Descripción de IActionConstraint](#understanding-iactionconstraint).

Si coinciden varias rutas y MVC no encuentra una ruta "recomendada", se produce una excepción `AmbiguousActionException`.

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a>Nombres de ruta

Las cadenas `"blog"` y `"default"` de los ejemplos siguientes son nombres de ruta:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Los nombres de ruta proporcionan un nombre lógico a la ruta para que la ruta con nombre pueda utilizarse en la generación de direcciones URL. Esto simplifica en gran medida la creación de direcciones URL en los casos en que el orden de las rutas podría complicar la generación de direcciones URL. Los nombres de ruta deben ser únicos en toda la aplicación.

Los nombres de ruta no tienen ningún impacto en la coincidencia de direcciones URL ni en el control de las solicitudes; se utilizan únicamente para la generación de direcciones URL. En [Enrutamiento](xref:fundamentals/routing) se incluye información más detallada sobre la generación de direcciones URL, como la generación de direcciones URL en asistentes específicos de MVC.

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a>Enrutamiento mediante atributos

El enrutamiento mediante atributos utiliza un conjunto de atributos para asignar acciones directamente a las plantillas de ruta. En el ejemplo siguiente, `app.UseMvc();` se utiliza en el método `Configure` y no se pasa ninguna ruta. `HomeController` coincidirá con un conjunto de direcciones URL similares a las que coincidirían con la ruta predeterminada `{controller=Home}/{action=Index}/{id?}`:

```csharp
public class HomeController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult Index()
   {
      return View();
   }
   [Route("Home/About")]
   public IActionResult About()
   {
      return View();
   }
   [Route("Home/Contact")]
   public IActionResult Contact()
   {
      return View();
   }
}
```

La acción `HomeController.Index()` se ejecutará para cualquiera de las rutas de dirección URL `/`, `/Home` o `/Home/Index`.

> [!NOTE]
> Este ejemplo resalta una diferencia clave de programación entre el enrutamiento mediante atributos y el enrutamiento convencional. El enrutamiento mediante atributos requiere más entradas para especificar una ruta; la ruta predeterminada convencional controla las rutas de manera más sucinta. Pero el enrutamiento mediante atributos permite (y requiere) un control preciso de las plantillas de ruta que se aplicarán a cada acción.

Con el enrutamiento mediante atributos, el nombre del controlador y los nombres de acción **no** juegan ningún papel en la selección de las acciones. Este ejemplo coincidirá con las mismas direcciones URL que el ejemplo anterior.

```csharp
public class MyDemoController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult MyIndex()
   {
      return View("Index");
   }
   [Route("Home/About")]
   public IActionResult MyAbout()
   {
      return View("About");
   }
   [Route("Home/Contact")]
   public IActionResult MyContact()
   {
      return View("Contact");
   }
}
```

> [!NOTE]
> Las plantillas de ruta anteriores no definen parámetros de ruta para `action`, `area` y `controller`. De hecho, estos parámetros de ruta no se permiten en rutas de atributo. Puesto que la plantilla de ruta ya está asociada a una acción, no tendría sentido analizar el nombre de acción de la dirección URL.

## <a name="attribute-routing-with-httpverb-attributes"></a>Enrutamiento mediante atributos con atributos Http[Verb]

El enrutamiento mediante atributos también puede hacer uso de los atributos `Http[Verb]` como `HttpPostAttribute`. Todos estos atributos pueden aceptar una plantilla de ruta. Este ejemplo muestra dos acciones que coinciden con la misma plantilla de ruta:

```csharp
[HttpGet("/products")]
public IActionResult ListProducts()
{
   // ...
}

[HttpPost("/products")]
public IActionResult CreateProduct(...)
{
   // ...
}
```

Para una ruta de dirección URL como `/products`, la acción `ProductsApi.ListProducts` se ejecutará cuando el verbo HTTP sea `GET` y `ProductsApi.CreateProduct` se ejecutará cuando el verbo HTTP sea `POST`. El enrutamiento mediante atributos primero busca una coincidencia de la dirección URL en el conjunto de plantillas de ruta que se definen mediante atributos de ruta. Una vez que se encuentra una plantilla de ruta que coincide, las restricciones `IActionConstraint` se aplican para determinar qué acciones se pueden ejecutar.

> [!TIP]
> Cuando se compila una API de REST, es poco frecuente que se quiera usar `[Route(...)]` en un método de acción. Es mejor usar `Http*Verb*Attributes` más específicos para precisar lo que es compatible con la API. Se espera que los clientes de API de REST sepan qué rutas y verbos HTTP se asignan a determinadas operaciones lógicas.

Puesto que una ruta de atributo se aplica a una acción específica, es fácil crear parámetros necesarios como parte de la definición de plantilla de ruta. En este ejemplo, se requiere `id` como parte de la ruta de dirección URL.

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

La acción `ProductsApi.GetProduct(int)` se ejecutará para una ruta de dirección URL como `/products/3`, pero no para una ruta de dirección URL como `/products`. Consulte [Enrutamiento](../../fundamentals/routing.md) para obtener una descripción completa de las plantillas de ruta y las opciones relacionadas.

## <a name="route-name"></a>Nombre de ruta

El código siguiente define un *nombre de ruta* de `Products_List`:

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

Los nombres de ruta se pueden utilizar para generar una dirección URL basada en una ruta específica. Los nombres de ruta no tienen ningún impacto en el comportamiento de coincidencia de direcciones URL del enrutamiento, y solo se usan para la generación de direcciones URL. Los nombres de ruta deben ser únicos en toda la aplicación.

> [!NOTE]
> Compare esto con la *ruta predeterminada* convencional, que define el parámetro `id` como opcional (`{id?}`). Esta capacidad de especificar con precisión las API tiene sus ventajas, como permitir que `/products` y `/products/5` se envíen a acciones diferentes.

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a>Combinación de rutas

Para que el enrutamiento mediante atributos sea menos repetitivo, los atributos de ruta del controlador se combinan con los atributos de ruta de las acciones individuales. Las plantillas de ruta definidas en el controlador se anteponen a las plantillas de ruta de las acciones. La colocación de un atributo de ruta en el controlador hace que **todas** las acciones del controlador usen el enrutamiento mediante atributos.

```csharp
[Route("products")]
public class ProductsApiController : Controller
{
   [HttpGet]
   public IActionResult ListProducts() { ... }

   [HttpGet("{id}")]
   public ActionResult GetProduct(int id) { ... }
}
```

En este ejemplo, la ruta de dirección URL `/products` puede coincidir con `ProductsApi.ListProducts` y la ruta de dirección URL `/products/5` puede coincidir con `ProductsApi.GetProduct(int)`. Ambas acciones coinciden solo con HTTP `GET` porque incluyen `HttpGetAttribute`.

Las plantillas de ruta aplicadas a una acción que comienzan por `/` o `~/` no se combinan con las plantillas de ruta que se aplican al controlador. En este ejemplo coinciden un conjunto de rutas de dirección URL similares a la *ruta predeterminada*.

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")]      // Combines to define the route template "Home"
    [Route("Index")] // Combines to define the route template "Home/Index"
    [Route("/")]     // Doesn't combine, defines the route template ""
    public IActionResult Index()
    {
        ViewData["Message"] = "Home index";
        var url = Url.Action("Index", "Home");
        ViewData["Message"] = "Home index" + "var url = Url.Action; =  " + url;
        return View();
    }

    [Route("About")] // Combines to define the route template "Home/About"
    public IActionResult About()
    {
        return View();
    }   
}
```

<a name="routing-ordering-ref-label"></a>

### <a name="ordering-attribute-routes"></a>Ordenación de rutas de atributo

A diferencia de las rutas convencionales que se ejecutan en un orden definido, el enrutamiento mediante atributos genera un árbol y busca coincidir con todas las rutas al mismo tiempo. Es como si las entradas de ruta se colocasen en un orden ideal; las rutas más específicas tienen la oportunidad de ejecutarse antes que las rutas más generales.

Por ejemplo, una ruta como `blog/search/{topic}` es más específica que una ruta como `blog/{*article}`. Desde un punto de vista lógico, la ruta `blog/search/{topic}` se ejecuta en primer lugar de forma predeterminada, ya que ese es el único orden significativo. En el enrutamiento convencional, el desarrollador es responsable de colocar las rutas en el orden deseado.

Las rutas de atributo pueden configurar un orden mediante la propiedad `Order` de todos los atributos de ruta proporcionados por el marco. Las rutas se procesan de acuerdo con el orden ascendente de la propiedad `Order`. El orden predeterminado es `0`. Si una ruta se configura con `Order = -1`, se ejecutará antes que las rutas que establecen un orden. Si una ruta se configura con `Order = 1`, se ejecutará después del orden de rutas predeterminado.

> [!TIP]
> Evite depender de `Order`. Si su espacio de direcciones URL requiere unos valores de orden explícitos para un enrutamiento correcto, es probable que también sea confuso para los clientes. Por lo general, el enrutamiento mediante atributos seleccionará la ruta correcta con la coincidencia de dirección URL. Si el orden predeterminado que se usa para la generación de direcciones URL no funciona, normalmente es más sencillo utilizar el nombre de ruta como una invalidación que aplicar la propiedad `Order`.

El enrutamiento del controlador de MVC y el enrutamiento de Razor Pages comparten una implementación. La información sobre el orden de la ruta de los temas de Razor Pages se encuentra disponible en [Convenciones de aplicación y de ruta de Razor Pages: Orden de la ruta](xref:razor-pages/razor-pages-conventions#route-order).

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a>Reemplazo de tokens en plantillas de ruta ([controller], [action], [area])

Para mayor comodidad, las rutas de atributo admiten *reemplazo de token*. Para ello, incluyen un token entre corchetes (`[`, `]`). Los tokens `[action]`, `[area]` y `[controller]` se reemplazan con los valores del nombre de la acción, el nombre del área y el nombre del controlador de la acción donde se define la ruta. En este ejemplo, las acciones coinciden con las rutas de dirección URL, tal como se describe en los comentarios:

[!code-csharp[](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

El reemplazo del token se produce como último paso de la creación de las rutas de atributo. El ejemplo anterior se comportará igual que el código siguiente:

[!code-csharp[](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

Las rutas de atributo también se pueden combinar con la herencia. Esto es especialmente eficaz si se combina con el reemplazo de token.

```csharp
[Route("api/[controller]")]
public abstract class MyBaseController : Controller { ... }

public class ProductsController : MyBaseController
{
   [HttpGet] // Matches '/api/Products'
   public IActionResult List() { ... }

   [HttpPut("{id}")] // Matches '/api/Products/{id}'
   public IActionResult Edit(int id) { ... }
}
```

El reemplazo de token también se aplica a los nombres de ruta definidos por las rutas de atributo. `[Route("[controller]/[action]", Name="[controller]_[action]")]` genera un nombre de ruta único para cada acción.

Para que el delimitador de reemplazo de token `[` o `]` coincida, repita el carácter (`[[` o `]]`) para usarlo como secuencia de escape.

::: moniker range=">= aspnetcore-2.2"

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a>Usar un transformador de parámetro para personalizar el reemplazo de tokens

El reemplazo de tokens se puede personalizarse mediante un transformador de parámetro. Un transformador de parámetro implementa `IOutboundParameterTransformer` y transforma el valor de parámetros. Por ejemplo, un transformador de parámetros `SlugifyParameterTransformer` personalizado cambia el valor de ruta `SubscriptionManagement` a `subscription-management`.

`RouteTokenTransformerConvention` es una convención de modelo de aplicación que:

* Aplica un transformador de parámetro a todas las rutas de atributo en una aplicación.
* Personaliza los valores de token de ruta de atributo que se reemplazan.

```csharp
public class SubscriptionManagementController : Controller
{
    [HttpGet("[controller]/[action]")] // Matches '/subscription-management/list-all'
    public IActionResult ListAll() { ... }
}
```

`RouteTokenTransformerConvention` está registrado como una opción en `ConfigureServices`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc(options =>
    {
        options.Conventions.Add(new RouteTokenTransformerConvention(
                                     new SlugifyParameterTransformer()));
    });
}

public class SlugifyParameterTransformer : IOutboundParameterTransformer
{
    public string TransformOutbound(object value)
    {
        if (value == null) { return null; }

        // Slugify value
        return Regex.Replace(value.ToString(), "([a-z])([A-Z])", "$1-$2").ToLower();
    }
}
```

::: moniker-end

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a>Varias rutas

El enrutamiento mediante atributos permite definir varias rutas que llegan a la misma acción. El uso más común de esto es imitar el comportamiento de la *ruta convencional predeterminada* tal como se muestra en el ejemplo siguiente:

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

La colocación de varios atributos de ruta en el controlador significa que cada uno de ellos se combinará con cada uno de los atributos de ruta en los métodos de acción.

```csharp
[Route("Store")]
[Route("[controller]")]
public class ProductsController : Controller
{
   [HttpPost("Buy")]     // Matches 'Products/Buy' and 'Store/Buy'
   [HttpPost("Checkout")] // Matches 'Products/Checkout' and 'Store/Checkout'
   public IActionResult Buy()
}
```

Cuando en una acción se colocan varios atributos de ruta (que implementan `IActionConstraint`), cada restricción de acción se combina con la plantilla de ruta del atributo que lo define.

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
   [HttpPut("Buy")]      // Matches PUT 'api/Products/Buy'
   [HttpPost("Checkout")] // Matches POST 'api/Products/Checkout'
   public IActionResult Buy()
}
```

> [!TIP]
> Si bien el uso de varias rutas en acciones puede parecer eficaz, es mejor que el espacio de direcciones URL de la aplicación sea sencillo y esté bien definido. Utilice varias rutas en acciones solo cuando sea necesario, por ejemplo, para admitir a clientes existentes.

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a>Especificación de parámetros opcionales de ruta de atributo, valores predeterminados y restricciones

Las rutas de atributo admiten la misma sintaxis en línea que las rutas convencionales para especificar parámetros opcionales, valores predeterminados y restricciones.

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

Consulte [Referencia de plantilla de ruta](../../fundamentals/routing.md#route-template-reference) para obtener una descripción detallada de la sintaxis de la plantilla de ruta.

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a>Atributos de ruta personalizados con `IRouteTemplateProvider`

Todos los atributos de ruta proporcionados en el marco (`[Route(...)]`, `[HttpGet(...)]`, etc.) implementan la interfaz `IRouteTemplateProvider`. Al iniciarse la aplicación, MVC busca atributos en las clases de controlador y los métodos de acción, y usa los que implementan `IRouteTemplateProvider` para generar el conjunto inicial de rutas.

Implemente `IRouteTemplateProvider` para definir sus propios atributos de ruta. Cada `IRouteTemplateProvider` le permite definir una única ruta con una plantilla de ruta, un orden y un nombre personalizados:

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

El atributo del ejemplo anterior establece automáticamente `Template` en `"api/[controller]"` cuando se aplica `[MyApiController]`.

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a>Uso del modelo de aplicación para personalizar las rutas de atributo

El *modelo de aplicación* es un modelo de objetos creado durante el inicio con todos los metadatos utilizados por MVC para enrutar y ejecutar las acciones. El *modelo de aplicación* incluye todos los datos recopilados de los atributos de ruta (a través de `IRouteTemplateProvider`). Puede escribir *convenciones* para modificar el modelo de aplicación en el momento del inicio y personalizar el comportamiento del enrutamiento. En esta sección se muestra un ejemplo sencillo de personalización del enrutamiento mediante el modelo de aplicación.

[!code-csharp[](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a>Enrutamiento mixto: enrutamiento mediante atributos frente a enrutamiento convencional

Las aplicaciones MVC pueden combinar el uso de enrutamiento convencional y enrutamiento mediante atributos. Es habitual usar las rutas convencionales para controladores que sirven páginas HTML para los exploradores, y el enrutamiento mediante atributos para los controladores que sirven las API de REST.

Las acciones se enrutan bien mediante convención o bien mediante atributos. Colocar una ruta en el controlador o la acción hace que se enrute mediante atributos. Las acciones que definen rutas de atributo no se pueden alcanzar a través de las rutas convencionales y viceversa. **Cualquier** atributo de ruta en el controlador hace que todas las acciones del controlador se enruten mediante atributos.

> [!NOTE]
> Lo que distingue los dos tipos de sistemas de enrutamiento es el proceso que se aplica después de que una dirección URL coincida con una plantilla de ruta. En el enrutamiento convencional, los valores de ruta de la coincidencia se usan para elegir la acción y el controlador en una tabla de búsqueda de todas las acciones enrutadas convencionales. En el enrutamiento mediante atributos, cada plantilla ya está asociada a una acción y no es necesaria ninguna otra búsqueda.

## <a name="complex-segments"></a>Segmentos complejos

Los segmentos complejos (por ejemplo, `[Route("/dog{token}cat")]`), se procesan buscando coincidencias de literales de derecha a izquierda de un modo no expansivo. Para ver una descripción, eche un vistazo al [código fuente](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296). Para más información, vea [este problema](https://github.com/aspnet/AspNetCore.Docs/issues/8197).

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a>Generación de direcciones URL

Las aplicaciones MVC pueden usar características de generación de direcciones URL de enrutamiento para generar vínculos URL a las acciones. La generación de direcciones URL elimina las direcciones URL codificadas de forma rígida, por lo que el código es más compacto y fácil de mantener. Esta sección se centra en las características de generación de direcciones URL proporcionadas por MVC y solo aborda los conceptos básicos de su funcionamiento. Consulte [Enrutamiento](../../fundamentals/routing.md) para obtener una descripción detallada de la generación de direcciones URL.

La interfaz `IUrlHelper` es la pieza subyacente de la infraestructura entre MVC y el enrutamiento para la generación de direcciones URL. Encontrará una instancia de `IUrlHelper` disponible a través de la propiedad `Url` en los controladores, las vistas y los componentes de vista.

En este ejemplo, la interfaz `IUrlHelper` se usa a través de la propiedad `Controller.Url` para generar una dirección URL a otra acción.

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

Si la aplicación está usando la ruta convencional predeterminada, el valor de la variable `url` será la cadena de ruta de dirección URL `/UrlGeneration/Destination`. Esta ruta de dirección URL se crea por enrutamiento mediante la combinación de los valores de ruta de la solicitud actual (valores de ambiente) con los valores pasados a `Url.Action`, y sustituyendo esos valores en la plantilla de ruta:

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

El valor de cada uno de los parámetros de ruta incluidos en la plantilla de ruta se sustituye por nombres que coincidan con los valores y los valores de ambiente. Si un parámetro de ruta no tiene un valor, puede utilizar un valor predeterminado en caso de tenerlo, o se puede omitir si es opcional (como en el caso de `id` en este ejemplo). La generación de direcciones URL producirá un error si cualquiera de los parámetros de ruta necesarios no tiene un valor correspondiente. Si se produce un error en la generación de direcciones URL para una ruta, se prueba con la ruta siguiente hasta que se hayan probado todas las rutas o se encuentra una coincidencia.

En el ejemplo anterior de `Url.Action` se supone que el enrutamiento es convencional, pero la generación de direcciones URL funciona del mismo modo con el enrutamiento mediante atributos, si bien los conceptos son diferentes. En el enrutamiento convencional, los valores de ruta se usan para expandir una plantilla; los valores de ruta de `controller` y `action` suelen aparecer en esa plantilla. Esto es válido porque las direcciones URL que coinciden con el enrutamiento se adhieren a una *convención*. En el enrutamiento mediante atributos, no está permitido que los valores de ruta de `controller` y `action` aparezcan en la plantilla. En vez de eso, se utilizan para buscar la plantilla que se va a usar.

Este ejemplo utiliza la enrutamiento mediante atributos:

[!code-csharp[](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

MVC genera una tabla de búsqueda de todas las acciones enrutadas mediante atributos y hará coincidir los valores `controller` y `action` para seleccionar la plantilla de ruta que se usará para la generación de direcciones URL. En el ejemplo anterior se genera `custom/url/to/destination`.

### <a name="generating-urls-by-action-name"></a>Generación de direcciones URL por nombre de acción

`Url.Action` (`IUrlHelper` . `Action`) y todas las sobrecargas relacionadas se basan en la idea de especificar el destino del vínculo mediante un nombre de controlador y un nombre de acción.

> [!NOTE]
> Cuando se usa `Url.Action`, se especifican los valores actuales de la ruta para `controller` y `action`. Los valores de `controller` y `action` forman parte tanto de los *valores de ambiente* **y**como de los *valores*. El método `Url.Action` siempre utiliza los valores actuales de `action` y `controller` y genera una ruta de dirección URL que dirige a la acción actual.

Intentos de enrutamiento para utilizar los valores en los valores de ambiente para rellenar la información que no se proporcionó al generar una dirección URL. Al utilizar una ruta como `{a}/{b}/{c}/{d}` y los valores de ambiente `{ a = Alice, b = Bob, c = Carol, d = David }`, el enrutamiento tiene suficiente información para generar una dirección URL sin ningún valor adicional, puesto que todos los parámetros de ruta tienen un valor. Si se agregó el valor `{ d = Donovan }`, el valor `{ d = David }` se ignorará y la ruta de dirección URL generada será `Alice/Bob/Carol/Donovan`.

> [!WARNING]
> Las rutas de dirección URL son jerárquicas. En el ejemplo anterior, si se agrega el valor `{ c = Cheryl }`, ambos valores `{ c = Carol, d = David }` se ignorarán. En este caso ya no tenemos un valor para `d` y se producirá un error en la generación de direcciones URL. Debe especificar el valor deseado de `c` y `d`.  Es probable que este problema se produzca con la ruta predeterminada (`{controller}/{action}/{id?}`), pero raramente encontrará este comportamiento en la práctica, dado que `Url.Action` especificará siempre de manera explícita un valor `controller` y `action`.

Las sobrecargas más largas de `Url.Action` también toman un objeto de *valores de ruta* adicional para proporcionar valores para parámetros de ruta distintos de `controller` y `action`. Normalmente verá esto utilizado con `id`, como `Url.Action("Buy", "Products", new { id = 17 })`. Por convención, el objeto de *valores de ruta* normalmente es un objeto de tipo anónimo, pero también puede ser `IDictionary<>` o un *objeto .NET estándar*. Los valores de ruta adicionales que no coinciden con los parámetros de ruta se colocan en la cadena de consulta.

[!code-csharp[](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> Para crear una dirección URL absoluta, use una sobrecarga que acepte `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a>Generación de direcciones URL por ruta

En el código anterior se pasó el nombre de acción y de controlador para generar una dirección URL. `IUrlHelper` también proporciona la familia de métodos `Url.RouteUrl`. Estos métodos son similares a `Url.Action`, pero no copian los valores actuales de `action` y `controller` en los valores de ruta. Lo más común es especificar un nombre de ruta para utilizar una ruta específica y generar la dirección URL, por lo general *sin* especificar un nombre de acción o controlador.

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a>Generación de direcciones URL en HTML

`IHtmlHelper` proporciona los métodos de `HtmlHelper` `Html.BeginForm` y `Html.ActionLink` para generar elementos `<form>` y `<a>`, respectivamente. Estos métodos utilizan el método `Url.Action` para generar una dirección URL y aceptan argumentos similares. Los métodos `Url.RouteUrl` complementarios de `HtmlHelper` son `Html.BeginRouteForm` y `Html.RouteLink`, cuya funcionalidad es similar.

Las TagHelper generan direcciones URL a través de la TagHelper `form` y la TagHelper `<a>`. Ambos usan `IUrlHelper` para su implementación. Consulte [Trabajar con formularios](../views/working-with-forms.md) para obtener más información.

Dentro de las vistas, `IUrlHelper` está disponible a través de la propiedad `Url` para una generación de direcciones URL ad hoc no cubierta por los pasos anteriores.

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a>Generar direcciones URL en los resultados de acción

Los ejemplos anteriores han demostrado el uso de `IUrlHelper` en un controlador, aunque el uso más común de un controlador consiste en generar una dirección URL como parte de un resultado de acción.

Las clases base `ControllerBase` y `Controller` proporcionan métodos de conveniencia para los resultados de acción que hacen referencia a otra acción. Un uso típico es redirigir después de aceptar la entrada del usuario.

```csharp
public IActionResult Edit(int id, Customer customer)
{
    if (ModelState.IsValid)
    {
        // Update DB with new details.
        return RedirectToAction("Index");
    }
    return View(customer);
}
```

Los patrones de diseño Factory Method de resultados de acción siguen un patrón similar a los métodos en `IUrlHelper`.

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a>Caso especial para rutas convencionales dedicadas

El enrutamiento convencional puede usar un tipo especial de definición de ruta denominada *ruta convencional dedicada*. En el ejemplo siguiente, la ruta llamada `blog` es una ruta convencional dedicada.

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Con estas definiciones de ruta, `Url.Action("Index", "Home")` generará la ruta de dirección URL `/` con la ruta `default`. Pero, ¿por qué? Se puede suponer que los valores de ruta `{ controller = Home, action = Index }` son suficientes para generar una dirección URL utilizando `blog`, con el resultado `/blog?action=Index&controller=Home`.

Las rutas convencionales dedicadas se basan en un comportamiento especial de los valores predeterminados que no tienen un parámetro de ruta correspondiente que impida que la ruta sea "demasiado expansiva" con la generación de direcciones URL. En este caso, los valores predeterminados son `{ controller = Blog, action = Article }`, y ni `controller` ni `action` aparecen como un parámetro de ruta. Cuando el enrutamiento realiza la generación de direcciones URL, los valores proporcionados deben coincidir con los valores predeterminados. La generación de direcciones URL con `blog` producirá un error porque los valores `{ controller = Home, action = Index }` no coinciden con `{ controller = Blog, action = Article }`. Después, el enrutamiento vuelve para probar `default`, operación que se realiza correctamente.

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a>Áreas

Las [áreas](areas.md) son una característica de MVC que se usa para organizar funciones relacionadas en un grupo como un espacio de nombres de enrutamiento independiente (para acciones de controlador) y una estructura de carpetas (para las vistas). La utilización de áreas permite que una aplicación tenga varios controladores con el mismo nombre, siempre y cuando tengan *áreas* diferentes. El uso de áreas crea una jerarquía para el enrutamiento mediante la adición de otro parámetro de ruta, `area`, a `controller` y `action`. En esta sección veremos la interacción entre el enrutamiento y las áreas. Consulte [Áreas](areas.md) para obtener más información sobre cómo se utilizan las áreas con las vistas.

En el ejemplo siguiente se configura MVC para usar la ruta predeterminada convencional y una *ruta de área* para un área denominada `Blog`:

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

Cuando coincide con una ruta de dirección URL como `/Manage/Users/AddUser`, la primera ruta generará los valores de ruta `{ area = Blog, controller = Users, action = AddUser }`. El valor de ruta `area` se genera con un valor predeterminado para `area`. De hecho, la ruta creada por `MapAreaRoute` es equivalente a la siguiente:

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

`MapAreaRoute` utiliza el nombre de área proporcionado, que en este caso es `Blog`, para crear una ruta con un valor predeterminado y una restricción para `area`. El valor predeterminado garantiza que la ruta siempre produce `{ area = Blog, ... }`; la restricción requiere el valor `{ area = Blog, ... }` para la generación de la dirección URL.

> [!TIP]
> El enrutamiento convencional depende del orden. En general, las rutas con áreas deben colocarse antes en la tabla de rutas, ya que son más específicas que las rutas sin un área.

En el ejemplo anterior, los valores de ruta coincidirían con la acción siguiente:

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

`AreaAttribute` es lo que denota un controlador como parte de un área. Se dice que este controlador está en el área `Blog`. Los controladores sin un atributo `[Area]` no son miembros de ningún área y **no** coincidirán cuando el enrutamiento proporcione el valor de ruta `area`. En el ejemplo siguiente, solo el primer controlador enumerado puede coincidir con los valores de ruta `{ area = Blog, controller = Users, action = AddUser }`.

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> En aras de la exhaustividad, aquí se muestra el espacio de nombres de cada controlador; en caso contrario, los controladores tendrían un conflicto de nomenclatura y generarían un error del compilador. Los espacios de nombres de clase no tienen ningún efecto en el enrutamiento de MVC.

Los dos primeros controladores son miembros de las áreas y solo coinciden cuando el valor de ruta `area` proporciona su respectivo nombre de área. El tercer controlador no es miembro de ningún área y solo puede coincidir cuando el enrutamiento no proporciona ningún valor para `area`.

> [!NOTE]
> En términos de búsqueda de coincidencias de *ningún valor*, la ausencia del valor `area` es igual que si el valor de `area` fuese null o una cadena vacía.

Al ejecutar una acción en un área, el valor de ruta para `area` estará disponible como un *valor de ambiente* para que el enrutamiento pueda usarlo en la generación de direcciones URL. Esto significa que, de forma predeterminada, las áreas actúan de forma *adhesiva* para la generación de direcciones URL, tal como se muestra en el ejemplo siguiente.

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a>Descripción de IActionConstraint

> [!NOTE]
> En esta sección se analiza con mayor profundidad el funcionamiento interno del marco y la elección por parte de MVC de la acción que se va a ejecutar. Una aplicación típica no necesitará un `IActionConstraint` personalizado.

Es probable que ya haya usado `IActionConstraint` incluso si no está familiarizado con la interfaz. El atributo `[HttpGet]` y los atributos `[Http-VERB]` similares implementan `IActionConstraint` con el fin de limitar la ejecución de un método de acción.

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

Adoptando la ruta convencional predeterminada, la ruta de dirección URL `/Products/Edit` generaría los valores `{ controller = Products, action = Edit }`, que coincidirían con **ambas** acciones que se muestran aquí. En terminología de `IActionConstraint`, diríamos que ambas acciones se consideran candidatas, puesto que las dos coinciden con los datos de ruta.

Cuando `HttpGetAttribute` se ejecute, dirá que *Edit()* es una coincidencia para *GET*, pero no para cualquier otro verbo HTTP. La acción `Edit(...)` no tiene ninguna restricción definida, por lo que coincidirá con cualquier verbo HTTP. Con `POST`, solamente `Edit(...)` coincide. Pero con `GET` ambas acciones pueden coincidir. No obstante, una acción con `IActionConstraint` siempre se considera *mejor* que una acción sin dicha restricción. Por tanto, como `Edit()` tiene `[HttpGet]`, se considera más específica y se seleccionará si ambas acciones pueden coincidir.

Conceptualmente, `IActionConstraint` es una forma de *sobrecarga*, pero en lugar de sobrecargar métodos con el mismo nombre, la sobrecarga se produce entre acciones que coinciden con la misma dirección URL. El enrutamiento mediante atributos también utiliza `IActionConstraint` y puede dar lugar a que acciones de diferentes controladores se consideren candidatas.

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a>Implementación de IActionConstraint

La manera más sencilla de implementar `IActionConstraint` consiste en crear una clase derivada de `System.Attribute` y colocarla en las acciones y los controladores. MVC detectará automáticamente cualquier `IActionConstraint` que se aplique como atributo. El modelo de aplicaciones es quizá el enfoque más flexible para la aplicación de restricciones, puesto que permite metaprogramar cómo se aplican.

En el ejemplo siguiente, una restricción elige una acción según un *código de país* de los datos de ruta. El [ejemplo completo se encuentra en GitHub](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).

```csharp
public class CountrySpecificAttribute : Attribute, IActionConstraint
{
    private readonly string _countryCode;

    public CountrySpecificAttribute(string countryCode)
    {
        _countryCode = countryCode;
    }

    public int Order
    {
        get
        {
            return 0;
        }
    }

    public bool Accept(ActionConstraintContext context)
    {
        return string.Equals(
            context.RouteContext.RouteData.Values["country"].ToString(),
            _countryCode,
            StringComparison.OrdinalIgnoreCase);
    }
}
```

Usted es responsable de implementar el método `Accept` y elegir un valor para "Order" para que la restricción se ejecute. En este caso, el método `Accept` devuelve `true` para denotar que la acción es una coincidencia cuando el valor de ruta `country` coincide. Esto difiere de `RouteValueAttribute` en que permite volver a una acción sin atributos. El ejemplo muestra que si se define una acción `en-US`, entonces un código de país como `fr-FR` recurriría a un controlador más genérico que no tenga `[CountrySpecific(...)]` aplicado.

La propiedad `Order` decide de qué *fase* forma parte la restricción. Restricciones de acciones que se ejecutan en grupos basados en la `Order`. Por ejemplo, todos los atributos del método HTTP proporcionados por el marco utilizan el mismo valor `Order` para que se ejecuten en la misma fase. Puede tener las fases que sean necesarias para implementar las directivas deseadas.

> [!TIP]
> Para decidir el valor de `Order`, piense si la restricción se debería o no aplicar antes que los métodos HTTP. Los números más bajos se ejecutan primero.
