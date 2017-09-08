---
title: El enrutamiento a las acciones de controlador
author: rick-anderson
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.assetid: 26250a4d-bf62-4d45-8549-26801cf956e9
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/routing
ms.openlocfilehash: da67124ffc874c4f83fff077c6429e9f3e571587
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="routing-to-controller-actions"></a>El enrutamiento a las acciones de controlador

Por [Ryan Nowak](https://github.com/rynowak) y [Rick Anderson](https://twitter.com/RickAndMSFT)

Núcleo de ASP.NET MVC utiliza el enrutamiento [middleware](../../fundamentals/middleware.md) que coincida con las direcciones URL de las solicitudes entrantes y asignarlos a las acciones. Las rutas se definen en el código de inicio o atributos. Las rutas describen cómo se deberían buscar rutas de acceso de dirección URL a las acciones. Rutas también se usan para generar direcciones URL (para vínculos) enviadas en las respuestas. 

Acciones o se enrutan convencional o enruta de atributo. Genera una ruta en el controlador o la acción hace que se enrutan de atributo. Vea [mixto enrutamiento](#routing-mixed-ref-label) para obtener más información.

Este documento explica las interacciones entre MVC y enrutamiento y asegúrese de aplicaciones MVC típica cómo el uso de características de enrutamiento. Vea [enrutamiento](xref:fundamentals/routing) para obtener más información acerca del enrutamiento avanzada.

## <a name="setting-up-routing-middleware"></a>Configurar el enrutamiento de Middleware

En su *configurar* método puede ver código similar al:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Dentro de la llamada a `UseMvc`, `MapRoute` se utiliza para crear una ruta única, que nos referiremos a como el `default` ruta. Mayoría de las aplicaciones MVC usará una ruta con una plantilla similar a la `default` ruta.

La plantilla de ruta `"{controller=Home}/{action=Index}/{id?}"` puede coincidir con una ruta de acceso de dirección URL como `/Products/Details/5` y va a extraer los valores de ruta `{ controller = Products, action = Details, id = 5 }` mediante encadenamiento de la ruta de acceso. MVC intentará encontrar un controlador denominado `ProductsController` y ejecutar la acción `Details`:

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

Tenga en cuenta que en este ejemplo, el enlace de modelos usarían el valor de `id = 5` para establecer el `id` parámetro `5` al invocar esta acción. Consulte la [enlace de modelos](../models/model-binding.md) para obtener más detalles.

Mediante el `default` ruta:

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

La plantilla de ruta:

* `{controller=Home}`define `Home` como valor predeterminado`controller`

* `{action=Index}`define `Index` como valor predeterminado`action`

* `{id?}`define `id` como opcionales

De forma predeterminada y los parámetros de ruta opcional no necesita estar presente en la ruta de acceso de dirección URL para una coincidencia. Vea [referencia de plantilla de ruta](../../fundamentals/routing.md#route-template-reference) para obtener una descripción detallada de la sintaxis de la plantilla de ruta.

`"{controller=Home}/{action=Index}/{id?}"`puede coincidir con la ruta de acceso de dirección URL `/` y generará los valores de ruta `{ controller = Home, action = Index }`. Los valores de `controller` y `action` hacer uso de los valores predeterminados, `id` no genera un valor porque no hay ningún segmento correspondiente en la ruta de acceso de dirección URL. MVC utilizaría estos valores de ruta para seleccionar la `HomeController` y `Index` acción:

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

Utilizar esta definición de controlador y la plantilla de ruta, el `HomeController.Index` acción se ejecutaría para cualquiera de las rutas de acceso de dirección URL siguientes:

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

El método conveniencia `UseMvcWithDefaultRoute`:

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

`UseMvc`y `UseMvcWithDefaultRoute` agregar una instancia de `RouterMiddleware` a la canalización de middleware. MVC no interactúa directamente con middleware y usa el enrutamiento para controlar las solicitudes. MVC está conectado a las rutas a través de una instancia de `MvcRouteHandler`. El código dentro de `UseMvc` es similar al siguiente:

<!-- literal_block {"ids": [], "names": [], "backrefs": [], "dupnames": [], "xml:space": "preserve", "classes": []} -->

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

`UseMvc`no se define directamente ninguna ruta, agrega un marcador de posición a la colección de rutas para la `attribute` ruta. La sobrecarga `UseMvc(Action<IRouteBuilder>)` le permite agregar sus propias rutas y también admite el enrutamiento del atributo.  `UseMvc`y todas sus variaciones agrega un marcador de posición para la ruta de atributo - siempre está disponible, independientemente de cómo configurar el enrutamiento de atributo `UseMvc`. `UseMvcWithDefaultRoute`define una ruta predeterminada y admite el enrutamiento del atributo. El [atributo enrutamiento](#attribute-routing-ref-label) sección incluye más detalles acerca del enrutamiento de atributo.

<a name=routing-conventional-ref-label></a>

## <a name="conventional-routing"></a>El enrutamiento convencional

El `default` ruta:

<!-- literal_block {"ids": [], "names": [], "backrefs": [], "dupnames": [], "xml:space": "preserve", "classes": []} -->

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

es un ejemplo de un *enrutamiento convencional*. Llamamos a este estilo *enrutamiento convencional* porque establece un *convención* para rutas de acceso de dirección URL:

* el primer segmento de ruta de acceso asignada al nombre de controlador

* la segunda se asigna al nombre de acción.

* el tercer segmento se utiliza para una función opcional `id` usa para asignar a un modelo de entidad

Mediante este `default` ruta, la ruta de acceso de dirección URL `/Products/List` se asigna a la `ProductsController.List` acción, y `/Blog/Article/17` se asigna a `BlogController.Article`. Esta asignación se basa en los nombres de acción y controlador **sólo** y no se basa en espacios de nombres, ubicaciones de archivos de origen o los parámetros de método.

> [!TIP]
> Usar el enrutamiento convencional con la ruta predeterminada, podrá generar rápidamente la aplicación sin necesidad de tener acceso a un nuevo patrón de dirección URL para cada acción que se define. Para una aplicación con acciones de estilo CRUD, contenedoras de coherencia para las direcciones URL a través de los controladores pueden ayudar a simplificar el código y hacer que la interfaz de usuario más predecibles.

> [!WARNING]
> El `id` se define como opcionales mediante la plantilla de ruta, lo que significa que las acciones se pueden ejecutar sin el identificador proporcionado como parte de la dirección URL. Normalmente lo que ocurrirá si `id` se omite de la dirección URL es la que se establecerá como `0` por el enlace de modelos y, como resultado ninguna entidad se encuentra en la base de datos de la búsqueda de coincidencias `id == 0`. Ruta de atributo puede proporcionar un control específico para realizar el Id. de necesarios para algunas acciones y otros no. Por convención incluirá la documentación de los parámetros opcionales, como `id` cuando es probables que aparezcan en el uso correcto.

## <a name="multiple-routes"></a>Varias rutas

Puede agregar varias rutas dentro de `UseMvc` agregando más llamadas a `MapRoute`. Si lo hace, le permite definir varias convenciones, o para agregar rutas convencionales que se dedican a una acción específica, como:

<!-- literal_block {"ids": [], "names": [], "backrefs": [], "dupnames": [], "xml:space": "preserve", "classes": []} -->

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
}
```

El `blog` ruta aquí es un *ruta convencional dedicado*, lo que significa que utiliza el sistema de enrutamiento convencional, pero está dedicado a una acción específica. Puesto que `controller` y `action` no aparecen en la plantilla de ruta como parámetros, solo pueden tener los valores predeterminados y, por tanto, esta ruta siempre se asignará a la acción `BlogController.Article`.

Rutas de la colección de rutas se ordenan y se procesarán en el orden en que se agregaron. Por lo que en este ejemplo, el `blog` ruta se intentará antes de la `default` ruta.

> [!NOTE]
> *Dedicado rutas convencionales* suelen usar parámetros de ruta de catch-all como `{*article}` para capturar la parte restante de la ruta de acceso de dirección URL. Esto puede ser una ruta de 'demasiado expansivo' lo que significa que coincide con las direcciones URL que se pretende ser elegidas por las otras rutas. Coloque las rutas "expansivas" más adelante en la tabla de rutas para resolver este problema.

### <a name="fallback"></a>Reserva

Como parte del procesamiento de la solicitud, MVC comprobará que los valores de ruta se pueden usar para buscar un controlador y la acción en la aplicación. Si los valores de ruta no coincide con una acción, a continuación, la ruta no se considera a una coincidencia y se intentará la ruta siguiente. Esto se denomina *reserva*, y se ha diseñado para simplificar los casos que se superponen a rutas convencionales.

### <a name="disambiguating-actions"></a>Eliminar la ambigüedad de acciones

Cuando coinciden los dos acciones a través de enrutamiento, debe eliminar la ambigüedad de MVC para elegir al candidato 'mejor'; de lo contrario producen una excepción. Por ejemplo:

<!-- literal_block {"ids": [], "names": [], "backrefs": [], "dupnames": [], "xml:space": "preserve", "classes": []} -->

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

Este controlador define dos acciones que coincidiría con la ruta de acceso de dirección URL `/Products/Edit/17` y los datos de ruta `{ controller = Products, action = Edit, id = 17 }`. Se trata de un patrón típico para controladores MVC donde `Edit(int)` muestra un formulario para editar un producto, y `Edit(int, Product)` procesa el formulario expuesto. Para hacer esto posible MVC sería necesario seleccionar `Edit(int, Product)` cuando la solicitud es un HTTP `POST` y `Edit(int)` cuando el verbo HTTP es cualquier otra cosa.

El `HttpPostAttribute` ( `[HttpPost]` ) es una implementación de `IActionConstraint` que solo permitirá la acción que se seleccione cuando el verbo HTTP es `POST`. La presencia de un `IActionConstraint` realiza el `Edit(int, Product)` coincide con 'mejor' que `Edit(int)`, por lo que `Edit(int, Product)` se intentará en primer lugar.

Sólo necesitará escribir personalizado `IActionConstraint` implementaciones en escenarios especializados, pero la importante para comprender el rol de atributos como `HttpPostAttribute` -se definen atributos similares para otros verbos HTTP. En el enrutamiento convencional es común para las acciones que se usará el mismo nombre de acción al que forman parte de un `show form -> submit form` flujo de trabajo. La comodidad de este patrón serán más evidente después de revisar la [IActionConstraint descripción](#understanding-iactionconstraint) sección.

Si coincide con varias rutas y MVC no encontró ninguna ruta 'recomendada', se producirá un `AmbiguousActionException`.

<a name=routing-route-name-ref-label></a>

### <a name="route-names"></a>Nombres de ruta

Las cadenas `"blog"` y `"default"` en los ejemplos siguientes son nombres de ruta:


```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Los nombres de ruta proporcionan un nombre lógico de la ruta para que la ruta con nombre puede utilizarse para la generación de dirección URL. Esto simplifica en gran medida la creación de dirección URL cuando el orden de las rutas podría realizar la generación de direcciones URL complicada. Nombres de las rutas deben ser únicos en toda la aplicación.

Los nombres de ruta no tienen ningún impacto en la dirección URL que coinciden o control de solicitudes; se usan únicamente para la generación de direcciones URL. [Enrutamiento](xref:fundamentals/routing) contiene información sobre la generación de dirección URL incluyendo generación de direcciones URL en aplicaciones auxiliares de MVC específicos más detallada.

<a name=attribute-routing-ref-label></a>

## <a name="attribute-routing"></a>Ruta de atributo

Ruta de atributo, utiliza un conjunto de atributos para asignar acciones directamente a las plantillas de ruta. En el ejemplo siguiente, `app.UseMvc();` se utiliza en el `Configure` se pasa el método y no hay ninguna ruta. El `HomeController` coincidirá con un conjunto de direcciones URL similares a qué la ruta predeterminada `{controller=Home}/{action=Index}/{id?}` coincidiría con:

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

El `HomeController.Index()` acción se ejecutará para cualquiera de las rutas de acceso de dirección URL `/`, `/Home`, o `/Home/Index`.

> [!NOTE]
> Este ejemplo resalta una diferencia clave programación entre el atributo y enrutamiento convencional. Ruta de atributo requiere más entradas para especificar una ruta; la ruta predeterminada convencional controla más brevemente las rutas. Sin embargo, enrutamiento de atributo permite (y necesita) un control preciso de los cuales se aplican las plantillas de ruta para cada acción.

Con el nombre del controlador y los nombres de acción de enrutamiento de atributo reproducir **sin** rol en el que se selecciona la acción. Este ejemplo coincidirá con las mismas direcciones URL que el ejemplo anterior.

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
> Las plantillas de ruta anteriores no definen parámetros de ruta para `action`, `area`, y `controller`. De hecho, estos parámetros de ruta no se permiten en rutas de atributo. Puesto que la plantilla de ruta ya está asociada a una acción, no tendría sentido al analizar el nombre de acción de la dirección URL.

## <a name="attribute-routing-with-httpverb-attributes"></a>Atributo de enrutamiento con atributos de Http [verbo]

Ruta de atributo también puede hacer uso de la `Http[Verb]` atributos como `HttpPostAttribute`. Todos estos atributos pueden aceptar una plantilla de ruta. Este ejemplo muestra dos acciones que coinciden con la misma plantilla de ruta:

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

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

Para una ruta de acceso de dirección URL como `/products` el `ProductsApi.ListProducts` acción se ejecutará cuando el verbo HTTP es `GET` y `ProductsApi.CreateProduct` se ejecutará cuando el verbo HTTP es `POST`. Atributo enrutamiento primero coincide con la dirección URL en el conjunto de plantillas de ruta que se definen mediante atributos de ruta. Una vez que coincide con una plantilla de ruta, `IActionConstraint` las restricciones se aplican para determinar qué acciones se pueden ejecutar.

> [!TIP]
> Al compilar una API de REST, es poco frecuente que desea usar `[Route(...)]` en un método de acción. Es mejor usar más específica `Http*Verb*Attributes` para ser precisos sobre lo que es compatible con la API. Se esperan que los clientes de API de REST para saber qué rutas de acceso y verbos HTTP que se asignan a determinadas operaciones lógicas.

Puesto que una ruta de atributo se aplica a una acción específica, es fácil realizar parámetros necesarios como parte de la definición de plantilla de ruta. En este ejemplo, `id` es necesario como parte de la ruta de acceso de dirección URL.

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

El `ProductsApi.GetProduct(int)` acción se ejecutará para una ruta de acceso de dirección URL como `/products/3` , pero no para una ruta de acceso de dirección URL como `/products`. Vea [enrutamiento](../../fundamentals/routing.md) para obtener una descripción completa de plantillas de ruta y opciones relacionadas.

## <a name="route-name"></a>Nombre de ruta

El código siguiente define un *nombre de ruta* de `Products_List`:

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

Los nombres de ruta se pueden utilizar para generar una dirección URL basada en una ruta específica. Los nombres de ruta no tienen ningún impacto en la dirección URL del comportamiento de enrutamiento de la coincidencia y solo se usan para la generación de direcciones URL. Los nombres de ruta deben ser único en toda la aplicación.

> [!NOTE]
> Compare esto con el convencional *ruta predeterminada*, que define la `id` parámetro como opcional (`{id?}`). Esta capacidad de especificar con precisión las API tiene sus ventajas, como permitir que los `/products` y `/products/5` va a enviar a acciones diferentes.

<a name=routing-combining-ref-label></a>

### <a name="combining-routes"></a>Rutas de combinación

Para realizar el enrutamiento de atributo menos repetitivos, atributos de ruta en el controlador se combinan con los atributos de ruta en las acciones individuales. Las plantillas de ruta definidas en el controlador se anteponen a plantillas de ruta en las acciones. Colocación de un atributo de ruta en el controlador hace **todos los** acciones en el controlador use el enrutamiento de atributo.

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

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

En este ejemplo, la ruta de acceso de dirección URL `/products` puede coincidir con `ProductsApi.ListProducts`y la ruta de acceso de dirección URL `/products/5` puede coincidir con `ProductsApi.GetProduct(int)`. Ambas acciones coincide solo con HTTP `GET` porque se decoran con el `HttpGetAttribute`.

Enrutar ninguna plantilla aplicada a una acción que comienzan por un `/` no se combinen con plantillas de ruta que se aplica al controlador. En este ejemplo coinciden con un conjunto de rutas de acceso de dirección URL similar a la *ruta predeterminada*.

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")]      // Combines to define the route template "Home"
    [Route("Index")] // Combines to define the route template "Home/Index"
    [Route("/")]     // Does not combine, defines the route template ""
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

<a name=routing-ordering-ref-label></a>

### <a name="ordering-attribute-routes"></a>Ordenación de las rutas de atributo

A diferencia de las rutas convencionales que se ejecutan en un orden definido, el enrutamiento de atributo genera un árbol y coincide con todas las rutas al mismo tiempo. Esto se comporta como-si las entradas de ruta se colocaron en un orden ideal; las rutas más específicas tienen una oportunidad de ejecutarse antes de las rutas más generales.

Por ejemplo, una ruta como `blog/search/{topic}` es más específico que una ruta como `blog/{*article}`. Habla lógicamente el `blog/search/{topic}` ruta 'ejecuta' en primer lugar, de forma predeterminada, ya que ese es el orden solo significativo. Con el enrutamiento convencional, el programador es responsable de colocación de las rutas en el orden deseado.

Rutas de atributo pueden configurar un pedido, mediante el `Order` propiedad de todos los atributos de ruta del marco. Las rutas se procesan según un orden ascendente de la `Order` propiedad. El orden predeterminado es `0`. Configurar una ruta mediante `Order = -1` se ejecutará antes que rutas que no establece un pedido. Configurar una ruta mediante `Order = 1` se ejecutará después de la ordenación de ruta predeterminada.

> [!TIP]
> Evitar según `Order`. Si el espacio de direcciones URL requiere que los valores de orden explícito para enrutar correctamente, es probable que confuso para los clientes, así. Por lo general la ruta de atributo seleccionará la ruta correcta con la coincidencia de dirección URL. Si el orden predeterminado que se usa para la generación de dirección URL no funciona, usando el nombre de ruta como una invalidación es normalmente más sencilla que aplicar la `Order` propiedad.

<a name=routing-token-replacement-templates-ref-label></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a>Símbolo (token) de sustitución en plantillas de ruta ([controlador], [acción] [area])

Para mayor comodidad, se admiten rutas de atributo *reemplazo del token* incluyendo un token entre llaves cuadrado (`[`, `]`). Los tokens `[action]`, `[area]`, y `[controller]` se sustituirán por los valores del nombre de la acción, el nombre de área y nombre del controlador de la acción donde se define la ruta. En este ejemplo las acciones pueden coincidir con las rutas de acceso de dirección URL como se describe en los comentarios:

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

Reemplazo del token se produce cuando el último paso de la creación de las rutas de atributo. El ejemplo anterior comportará igual que el código siguiente:

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

Rutas de atributo también pueden combinarse con la herencia. Esto es especialmente eficaz a combinar con el reemplazo del token.

```csharp
[Route("api/[controller]")]
public abstract class MyBaseController : Controller { ... }

public class ProductsController : MyBaseController
{
   [HttpGet] // Matches '/api/Products'
   public IActionResult List() { ... }

   [HttpPost("{id}")] // Matches '/api/Products/{id}'
   public IActionResult Edit(int id) { ... }
}
```

Reemplazo del token también se aplica a los nombres de ruta definidos por las rutas de atributo. `[Route("[controller]/[action]", Name="[controller]_[action]")]`se generará un nombre de ruta único para cada acción.

Para que coincida con el delimitador de reemplazo del token literal `[` o `]`, escape repitiendo el carácter (`[[` o `]]`).

<a name=routing-multiple-routes-ref-label></a>

### <a name="multiple-routes"></a>Varias rutas

Atributo enrutamiento permite definir varias rutas que llegan a la misma acción. El uso más común de esto es imitar el comportamiento de la *ruta convencional de predeterminada* tal como se muestra en el ejemplo siguiente:

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

Colocar varios atributos de ruta en el controlador significa que cada uno de ellos se combinará con cada uno de los atributos de ruta en los métodos de acción.

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

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

Cuando varios atributos de ruta (que implementan `IActionConstraint`) se colocan en una acción, a continuación, cada acción de restricción se combina con la plantilla de ruta del atributo que lo define.

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

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
> Aunque puede parecer eficaz utilizando varias rutas en acciones, es mejor mantener el espacio de direcciones URL de la aplicación simple y bien definidos. Utilice varias rutas de acciones solo cuando sea necesario, por ejemplo, para admitir a los clientes existentes.

<a name=routing-attr-options></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a>Especificación de parámetros opcionales de ruta de atributo, valores predeterminados y restricciones

Rutas de atributo admiten la misma sintaxis en línea como rutas convencionales para especificar los parámetros opcionales, valores predeterminados y restricciones.

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

Vea [referencia de plantilla de ruta](../../fundamentals/routing.md#route-template-reference) para obtener una descripción detallada de la sintaxis de la plantilla de ruta.

<a name=routing-cust-rt-attr-irt-ref-label></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a>Atributos de ruta personalizados utilizando`IRouteTemplateProvider`

Todos los atributos de la ruta proporcionados en el marco de trabajo ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implementan la `IRouteTemplateProvider` interfaz. MVC busca atributos en las clases de controlador y los métodos de acción cuando se inicia la aplicación y usa los que implementarán `IRouteTemplateProvider` para generar el conjunto inicial de rutas.

Puede implementar `IRouteTemplateProvider` para definir sus propios atributos de ruta. Cada `IRouteTemplateProvider` le permite definir una única ruta con una plantilla de ruta personalizados, ordenar y el nombre:

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

El atributo por el ejemplo anterior se establece automáticamente el `Template` a `"api/[controller]"` cuando `[MyApiController]` se aplica.

<a name=routing-app-model-ref-label></a>

### <a name="using-application-model-to-customize-attribute-routes"></a>Uso de modelo de aplicación para personalizar las rutas de atributo

El *modelo de aplicación* es un modelo de objetos creado durante el inicio con todos los metadatos de MVC utilizado para enrutar y ejecutar las acciones. El *modelo de aplicación* incluye todos los datos recopilados de los atributos de ruta (a través de `IRouteTemplateProvider`). Puede escribir *convenciones* para modificar el modelo de aplicación en tiempo de inicio para personalizar el comportamiento de enrutamiento. Esta sección muestra un ejemplo sencillo de personalizar el enrutamiento mediante el modelo de aplicación.

[!code-csharp[Main](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name=routing-mixed-ref-label></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a>Mixto enrutamiento: atributo enrutamiento enrutamiento convencional de vs

Las aplicaciones MVC pueden mezclar el uso de convencional y atributo de enrutamiento. Es habitual para usar las rutas de convencionales para controladores de servir páginas HTML para los exploradores y el atributo de enrutamiento para los controladores que sirve al API de REST.

Acciones o se enrutan convencional o enruta de atributo. Genera una ruta en el controlador o la acción hace que se enrutan de atributo. Las acciones que definen rutas de atributo no se puede alcanzar a través de las rutas convencionales y viceversa. **Cualquier** atributo de ruta en el controlador hace que todas las acciones en el atributo de controlador enrutando.

> [!NOTE]
> Lo que distingue entre los dos tipos de sistemas de enrutamientos es el proceso que se aplica después de que una dirección URL coincide con una plantilla de ruta. En el enrutamiento convencional, se usan los valores de ruta de la búsqueda de coincidencias para elegir la acción y el controlador de una tabla de búsqueda de todas las acciones enrutadas convencionales. En el enrutamiento de atributo, cada plantilla ya está asociado a una acción y no es necesaria ninguna otra búsqueda.

<a name=routing-url-gen-ref-label></a>

## <a name="url-generation"></a>Generación de direcciones URL

Las aplicaciones de MVC pueden usar características de generación de dirección URL de enrutamiento para generar vínculos de dirección URL a las acciones. Generar direcciones URL elimina las direcciones URL de codificar, hacer que el código sea más compacto y fácil de mantener. En esta sección se centra en las características de generación de dirección URL proporcionadas por MVC y sólo aborda los conceptos básicos de cómo funciona la generación de direcciones URL. Vea [enrutamiento](../../fundamentals/routing.md) para obtener una descripción detallada de generación de direcciones URL.

La `IUrlHelper` interfaz es la pieza subyacente de la infraestructura entre MVC y enrutamiento de generación de direcciones URL. Puede encontrar una instancia de `IUrlHelper` disponibles a través de la `Url` propiedad en componentes de la vista, vistas y controladores.

En este ejemplo, el `IUrlHelper` interfaz se usa a través de la `Controller.Url` propiedad que se va a generar una dirección URL a otra acción.

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

Si la aplicación está usando el valor predeterminado convencional de ruta, el valor de la `url` variable será la cadena de ruta de acceso de dirección URL `/UrlGeneration/Destination`. Esta ruta de acceso de dirección URL se crea mediante el enrutamiento mediante la combinación de los valores de ruta de la solicitud actual (valores de ambiente), con los valores pasados a `Url.Action` y sustituir los valores en la plantilla de ruta:

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

Cada parámetro de ruta en la plantilla de ruta tiene su valor sustituido por nombres coincidentes con los valores y los valores de ambiente. Un parámetro de ruta que no tiene un valor puede utilizar un valor predeterminado si tiene uno, o se pueden omitir si es opcional (al igual que en el caso de `id` en este ejemplo). Se producirá un error en la generación de direcciones URL si cualquier parámetro de ruta necesaria no tiene un valor correspondiente. Si se produce un error en la generación de direcciones URL para una ruta, se prueba con la ruta siguiente hasta que se hayan probado todas las rutas o se encuentra una coincidencia.

En el ejemplo de `Url.Action` anteriormente se supone que el enrutamiento convencional, pero funciona de la generación de dirección URL del mismo modo con el enrutamiento de atributo, aunque los conceptos son diferentes. Con el enrutamiento convencional, se usan los valores de ruta para expandir una plantilla y los valores de ruta de `controller` y `action` suelen aparecer en esa plantilla - esto funciona porque las direcciones URL que coinciden con el enrutamiento adhieren a un *convención*. En el enrutamiento de atributo, los valores de la ruta de `controller` y `action` no se permite que aparezca en la plantilla - en su lugar, se usan para buscar qué plantilla utilizar.

Este ejemplo utiliza la ruta de atributo:

[!code-csharp[Main](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

MVC genera una tabla de búsqueda de todas las acciones de atributo que se enrutan y se corresponderá con el `controller` y `action` valores para seleccionar la plantilla de ruta que se usará para la generación de direcciones URL. En el ejemplo anterior, `custom/url/to/destination` se genera.

### <a name="generating-urls-by-action-name"></a>Generar direcciones URL al nombre de acción

`Url.Action` (`IUrlHelper` . `Action`) y todos los relacionados con las sobrecargas todos se basan en esa idea que desea especificar lo que está vinculando a especificando un nombre de acción y controlador.

> [!NOTE]
> Cuando se usa `Url.Action`, los valores de la ruta actual para `controller` y `action` se especifican para usted, el valor de `controller` y `action` forman parte de ambos *valores ambiente* **y** *valores*. El método `Url.Action`, siempre utiliza los valores actuales de `action` y `controller` y generará una ruta de acceso de dirección URL que se enruta a la acción actual.

Intenta utilizar los valores en los valores de ambiente para rellenar la información que no proporcionan al generar una dirección URL de enrutamiento. Uso de una ruta como `{a}/{b}/{c}/{d}` y valores de ambiente `{ a = Alice, b = Bob, c = Carol, d = David }`, enrutamiento tiene suficiente información para generar una dirección URL sin ningún valor adicional: puesto que en todas las rutas parámetros tienen un valor. Si agrega el valor `{ d = Donovan }`, el valor `{ d = David }` se ignorará y la ruta de acceso de dirección URL generada sería `Alice/Bob/Carol/Donovan`.

> [!WARNING]
> Las rutas de acceso de dirección URL son jerárquicos. En el ejemplo anterior, si se agrega el valor `{ c = Cheryl }`, ambos de los valores `{ c = Carol, d = David }` se ignoraría. En este caso ya no tenemos un valor para `d` y se producirá un error en la generación de direcciones URL. Debe especificar el valor deseado de `c` y `d`.  Puede esperar a que se alcanza este problema con la ruta predeterminada (`{controller}/{action}/{id?}`)- pero raramente encontrará este comportamiento en la práctica como `Url.Action` especificará siempre explícitamente una `controller` y `action` valor.

Más sobrecargas de `Url.Action` tener más *valores de ruta* objeto para proporcionar valores para parámetros de ruta distinto de `controller` y `action`. Normalmente verá esto utiliza con `id` como `Url.Action("Buy", "Products", new { id = 17 })`. Por convención la *valores de ruta* normalmente es un objeto de tipo anónimo, pero también puede ser un `IDictionary<>` o un *objeto plain old de .NET*. Los valores de ruta adicionales que no coinciden con los parámetros de ruta se colocan en la cadena de consulta.

[!code-csharp[Main](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> Para crear una dirección URL absoluta, use una sobrecarga que acepta un `protocol`:`Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`

<a name=routing-gen-urls-route-ref-label></a>

### <a name="generating-urls-by-route"></a>Generación de direcciones URL de ruta

El código anterior muestra generando una dirección URL al pasar el nombre de acción y controlador. `IUrlHelper`También proporciona la `Url.RouteUrl` familia de métodos. Estos métodos son similares a `Url.Action`, pero no que se copian los valores actuales de `action` y `controller` a los valores de ruta. El uso más común es especificar un nombre de ruta que utilice una ruta específica para generar la dirección URL, por lo general *sin* especificando un nombre de acción o controlador.

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name=routing-gen-urls-html-ref-label></a>

### <a name="generating-urls-in-html"></a>Generar direcciones URL en HTML

`IHtmlHelper`proporciona el `HtmlHelper` métodos `Html.BeginForm` y `Html.ActionLink` para generar `<form>` y `<a>` elementos respectivamente. Estos métodos utilizan el `Url.Action` método para generar una dirección URL y aceptan argumentos similares. El `Url.RouteUrl` compañeros para `HtmlHelper` son `Html.BeginRouteForm` y `Html.RouteLink` que tienen una funcionalidad similar.

TagHelpers generar direcciones URL a través de la `form` TagHelper y `<a>` TagHelper. Ambos usan `IUrlHelper` para su implementación. Vea [trabajar con formularios](../views/working-with-forms.md) para obtener más información.

Dentro de las vistas, la `IUrlHelper` está disponible a través de la `Url` propiedad para una generación de dirección URL de ad hoc no cubierta por los pasos anteriores.

<a name=routing-gen-urls-action-ref-label></a>

### <a name="generating-urls-in-action-results"></a>Generar direcciones URL en los resultados de acción

Los ejemplos anteriores han demostrado utilizando `IUrlHelper` en un controlador, mientras que el uso de un controlador más común consiste en generar una dirección URL como parte de un resultado de acción.

El `ControllerBase` y `Controller` clases base proporcionan métodos de conveniencia para los resultados de la acción que hacen referencia a otra acción. Un uso típico es redirigir después de Aceptar proporcionados por el usuario.

```csharp
public Task<IActionResult> Edit(int id, Customer customer)
{
    if (ModelState.IsValid)
    {
        // Update DB with new details.
        return RedirectToAction("Index");
    }
}
```

Los métodos de generador de resultados de acción siguen un patrón similar a los métodos `IUrlHelper`.

<a name=routing-dedicated-ref-label></a>

### <a name="special-case-for-dedicated-conventional-routes"></a>Caso especial para rutas convencionales dedicadas

El enrutamiento convencional puede usar un tipo especial de definición de ruta denominada una *dedicado ruta convencional*. En el ejemplo siguiente, la ruta con el nombre `blog` es una ruta convencional dedicada.

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

¿Con estas definiciones de route, `Url.Action("Index", "Home")` generará la ruta de acceso de dirección URL `/` con el `default` ruta, pero por qué? Se pueden suponer que los valores de ruta `{ controller = Home, action = Index }` sería suficiente para generar una dirección URL utilizar `blog`, y el resultado sería `/blog?action=Index&controller=Home`.

Rutas convencionales dedicadas que se basan en un comportamiento especial de los valores predeterminados que no tienen un parámetro de ruta correspondiente que impide que la ruta se "demasiado expansiva" con la generación de direcciones URL. En este caso, los valores predeterminados son `{ controller = Blog, action = Article }`y ni `controller` ni `action` aparece como un parámetro de ruta. Cuando el enrutamiento realiza la generación de direcciones URL, los valores proporcionados deben coincidir con los valores predeterminados. Generación de dirección URL mediante `blog` se producirá un error porque los valores `{ controller = Home, action = Index }` no coincide con `{ controller = Blog, action = Article }`. Enrutamiento, a continuación, vuelve para probar `default`, que se realiza correctamente.

<a name=routing-areas-ref-label></a>

## <a name="areas"></a>Áreas

[Áreas](areas.md) son una característica MVC que se usan para organizar funcionalidad relacionada en un grupo como un enrutamiento-espacio de nombres independiente (para las acciones de controlador) y la estructura de carpetas (para vistas). Usar áreas de permite que una aplicación que tiene varios controladores con el mismo nombre - siempre que tienen diferentes *áreas*. El uso de áreas crea una jerarquía con el fin de enrutamiento mediante la adición de otro parámetro de ruta, `area` a `controller` y `action`. En esta sección veremos cómo enrutamiento interactúa con áreas - vea [áreas](areas.md) para obtener más información sobre cómo se utilizan las áreas con las vistas.

En el ejemplo siguiente se configura MVC para usar la ruta predeterminada convencional y un *ruta de área* para un área denominada `Blog`:

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

Cuando coincide con una ruta de acceso de dirección URL como `/Manage/Users/AddUser`, la primera ruta generará los valores de ruta `{ area = Blog, controller = Users, action = AddUser }`. El `area` valor de ruta se genera con un valor predeterminado para `area`, de hecho la ruta creada por `MapAreaRoute` es equivalente a la siguiente:

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

`MapAreaRoute`crea una ruta con un valor predeterminado y la restricción para `area` utilizando el nombre del área proporcionados, en este caso `Blog`. El valor predeterminado garantiza que siempre produce la ruta `{ area = Blog, ... }`, la restricción requiere que el valor `{ area = Blog, ... }` para la generación de dirección URL.

> [!TIP]
> El enrutamiento convencional depende del orden. En general, las rutas con áreas deben colocarse anteriormente en la tabla de rutas tal como están más específicos que las rutas sin un área.

En el ejemplo anterior, los valores de ruta coincidiría con la acción siguiente:

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

El `AreaAttribute` es lo que denota un controlador como parte de un área, se dice que este controlador está en la `Blog` área. Controladores sin un `[Area]` atributo no son miembros de cualquier área y le **no** coincide con cuando el `area` ruta valor lo proporciona el enrutamiento. En el ejemplo siguiente, solo el primer controlador aparecen puede coincidir con los valores de ruta `{ area = Blog, controller = Users, action = AddUser }`.

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> Aquí se muestra el espacio de nombres de cada controlador por integridad; en caso contrario, los controladores tendría una nomenclatura entran en conflicto y generar un error del compilador. Espacios de nombres de clase no tienen ningún efecto en el enrutamiento de MVC.

Los dos primeros controladores son miembros de las áreas y coincida solo cuando su nombre de área respectiva se proporciona por el `area` enrutar valor. El controlador de terceros no es un miembro de cualquier área y puede única coincidencia cuando ningún valor para `area` proporciona el enrutamiento.

> [!NOTE]
> En términos de búsqueda de coincidencias *ningún valor*, la ausencia de la `area` tiene el mismo valor como si el valor de `area` eran null o una cadena vacía.

Al ejecutar una acción en un área, la ruta valor `area` estarán disponibles como un *valor ambiente* para el enrutamiento para que utilice para la generación de direcciones URL. Esto significa que de forma predeterminada las áreas actúen *rápidas* para la generación de dirección URL como se muestra en el ejemplo siguiente.

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/mvc/controllers/routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs"} -->

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name=iactionconstraint-ref-label></a>

## <a name="understanding-iactionconstraint"></a>Descripción IActionConstraint

> [!NOTE]
> Esta sección es un análisis más profundo en funcionamiento interno de framework y cómo MVC elige una acción que se va a ejecutar. Una aplicación típica no tendrá un personalizado`IActionConstraint`

Es probable que ya haya usado `IActionConstraint` incluso si no está familiarizado con la interfaz. El `[HttpGet]` atributo y similares `[Http-VERB]` atributos implementan `IActionConstraint` con el fin de limitar la ejecución de un método de acción.

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

Suponiendo que la ruta convencional de forma predeterminada, la ruta de acceso de dirección URL `/Products/Edit` generaría los valores `{ controller = Products, action = Edit }`, que coincidiría con **ambos** de las acciones que se muestra aquí. En `IActionConstraint` terminología haría decimos que ambas de estas acciones se consideran candidatos - cuando coincidan con los datos de ruta.

Cuando el `HttpGetAttribute` se ejecuta, lo dirá que *Edit()* es una coincidencia para *obtener* y no es una coincidencia para cualquier otro verbo HTTP. El `Edit(...)` acción no tiene ninguna restricción definida y, por lo que coincidirá con cualquier verbo HTTP. Por lo que suponiendo un `POST` : solo `Edit(...)` coincide con. Sin embargo, para un `GET` ambas acciones pueden coincidir con los-sin embargo, una acción con un `IActionConstraint` siempre se considera *mejor* que una acción sin. Por lo que ya `Edit()` tiene `[HttpGet]` se considera más específica y se seleccionará si pueden hacer coincidir ambas acciones.

Conceptualmente, `IActionConstraint` es una forma de *sobrecarga*, pero en lugar de la sobrecarga de métodos con el mismo nombre, se sobrecarga entre las acciones que coinciden con la misma dirección URL. Ruta de atributo también utiliza `IActionConstraint` y puede dar lugar a acciones de diferentes controladores ambos están considerando candidatos.

<a name=iactionconstraint-impl-ref-label></a>

### <a name="implementing-iactionconstraint"></a>Implementar IActionConstraint

La manera más sencilla de implementar un `IActionConstraint` consiste en crear una clase derivada de `System.Attribute` y lo coloca en las acciones y los controladores. MVC detecta automáticamente cualquier `IActionConstraint` que se aplican como atributos. Puede usar el modelo de aplicaciones para aplicar restricciones y se trata probablemente el enfoque más flexible puesto que permite a metaprogram cómo se aplican.

En el ejemplo siguiente, una restricción elige una acción según un *código de país* de los datos de ruta. El [ejemplo completo en GitHub](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).

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

Usted es responsable de implementar la `Accept` método y elija un 'pedido' para la restricción para ejecutar. En este caso, el `Accept` método `true` para denotar la acción es una coincidencia cuando el `country` enrutar coincidencias de valor. Esto es diferente de un `RouteValueAttribute` en que se permite el retroceso a una acción sin atributos. El ejemplo muestra que si se define un `en-US` acción, a continuación, un código de país como `fr-FR` recurra a un controlador más genérico que no tiene `[CountrySpecific(...)]` aplicado.

El `Order` propiedad decide en qué *fase* la restricción es parte de. Restricciones de acciones que se ejecutan en grupos basados en la `Order`. Por ejemplo, todo el marco de trabajo proporciona los atributos de métodos HTTP utilizan el mismo `Order` valor para que se ejecutan en la misma fase. Puede tener tantas fases según sea necesario implementar las directivas deseadas.

> [!TIP]
> Para decidir cuál es un valor para `Order` piense o no se debe aplicar la restricción antes que los métodos HTTP. Los números más bajos se ejecutan primeros.
