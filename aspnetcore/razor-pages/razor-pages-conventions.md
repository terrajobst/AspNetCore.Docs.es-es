---
title: Convenciones de aplicación y de ruta de páginas de Razor en ASP.NET Core
author: guardrex
description: Vea cómo las convenciones de proveedor de modelos de aplicación y de ruta sirven para controlar el enrutamiento, la detección y el procesamiento de páginas.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/22/2019
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: a0a1eda69da34d1865fd11ef464c3697bcd01eff
ms.sourcegitcommit: 810d5831169770ee240d03207d6671dabea2486e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2019
ms.locfileid: "72779217"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a>Convenciones de aplicación y de ruta de páginas de Razor en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

Aquí encontrará información para saber cómo usar las [convenciones de proveedor de modelos de aplicación y de ruta](xref:mvc/controllers/application-model#conventions) con objeto de controlar el enrutamiento, la detección y el procesamiento en aplicaciones de páginas de Razor.

Si necesita configurar rutas de una página personalizadas para páginas concretas, configure el enrutamiento a esas páginas con la [convención AddPageRoute](#configure-a-page-route), descrita más adelante en este tema.

Para especificar una ruta de página, agregar segmentos de ruta o agregar parámetros a una ruta, use la Directiva de `@page` de la página. Para obtener más información, vea [rutas personalizadas](xref:razor-pages/index#custom-routes).

Hay palabras reservadas que no se pueden usar como segmentos de ruta o nombres de parámetro. Para obtener más información, vea [Routing: Reserved Routing names](xref:fundamentals/routing#reserved-routing-names).

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))

| Escenario | El ejemplo explica cómo... |
| -------- | --------------------------- |
| [Convenciones de modelo](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li><li>IPageHandlerModelConvention</li></ul> | Agregar un encabezado y una plantilla de ruta a las páginas de una aplicación. |
| [Convenciones de acción de ruta de página](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Agregar una plantilla de ruta a las páginas de una carpeta y a una sola página. |
| [Convenciones de acción del modelo de página](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (clase de filtro, expresión lambda o fábrica de filtros)</li></ul> | Agregar un encabezado a las páginas de una carpeta, agregar un encabezado a una sola página y configurar una [fábrica de filtros](xref:mvc/controllers/filters#ifilterfactory) para agregar un encabezado a las páginas de la aplicación. |

Las convenciones de Razor Pages se agregan y configuran mediante el método de extensión <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> para <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> en la colección de servicios de la clase `Startup`. Más avanzado el tema veremos los siguientes ejemplos de convención:

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddRazorPages()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add( ... );
            options.Conventions.AddFolderRouteModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageRouteModelConvention(
                "/About", model => { ... });
            options.Conventions.AddPageRoute(
                "/Contact", "TheContactPage/{text?}");
            options.Conventions.AddFolderApplicationModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageApplicationModelConvention(
                "/About", model => { ... });
            options.Conventions.ConfigureFilter(model => { ... });
            options.Conventions.ConfigureFilter( ... );
        });
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add( ... );
            options.Conventions.AddFolderRouteModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageRouteModelConvention(
                "/About", model => { ... });
            options.Conventions.AddPageRoute(
                "/Contact", "TheContactPage/{text?}");
            options.Conventions.AddFolderApplicationModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageApplicationModelConvention(
                "/About", model => { ... });
            options.Conventions.ConfigureFilter(model => { ... });
            options.Conventions.ConfigureFilter( ... );
        });
}
```

::: moniker-end

## <a name="route-order"></a>Orden de ruta

Las rutas especifican un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> para su procesamiento (coincidencia de rutas).

| Ordenar            | Comportamiento |
| :--------------: | -------- |
| -1               | La ruta se procesa antes de que se procesen otras rutas. |
| 0                | No se ha especificado el orden (valor predeterminado). Si no se asigna `Order` (`Order = null`), el valor predeterminado de la ruta `Order` en 0 (cero) para su procesamiento. |
| 1,2 &hellip; n | Especifica el orden de procesamiento de la ruta. |

El procesamiento de rutas se establece por Convención:

* Las rutas se procesan en orden secuencial (-1, 0, 1, 2, &hellip; n).
* Cuando las rutas tienen el mismo `Order`, la ruta más específica se compara primero seguida por rutas menos específicas.
* Cuando las rutas con el mismo `Order` y el mismo número de parámetros coinciden con una dirección URL de solicitud, las rutas se procesan en el orden en que se agregan a la <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.

Si es posible, evite depender de un orden de procesamiento de rutas establecido. Por lo general, el enrutamiento selecciona la ruta correcta con coincidencia de dirección URL. Si tiene que establecer las propiedades de ruta `Order` para enrutar las solicitudes correctamente, es probable que el esquema de enrutamiento de la aplicación sea confuso para los clientes y que el mantenimiento sea frágil. Busque para simplificar el esquema de enrutamiento de la aplicación. La aplicación de ejemplo requiere un orden de procesamiento de rutas explícito para mostrar varios escenarios de enrutamiento con una sola aplicación. Sin embargo, debe intentar evitar la práctica de configurar `Order` de enrutamiento en aplicaciones de producción.

El enrutamiento del controlador de MVC y el enrutamiento de Razor Pages comparten una implementación. La información sobre el orden de las rutas en los temas de MVC está disponible en [enrutamiento a las acciones del controlador: ordenación de rutas de atributo](xref:mvc/controllers/routing#ordering-attribute-routes).

## <a name="model-conventions"></a>Convenciones de modelo

Agregue un delegado para <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> para agregar [convenciones de modelo](xref:mvc/controllers/application-model#conventions) que se aplican a Razor pages.

### <a name="add-a-route-model-convention-to-all-pages"></a>Agregar una Convención de modelo de ruta a todas las páginas

Utilice <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> para crear y agregar un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> a la colección de instancias de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> que se aplican durante la construcción del modelo de ruta de página.

En la aplicación de ejemplo se agrega una plantilla de ruta `{globalTemplate?}` a todas las páginas de la aplicación:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

La propiedad <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> está establecida en `1`. Esto garantiza el siguiente comportamiento de coincidencia de rutas en la aplicación de ejemplo:

* Más adelante en el tema se agrega una plantilla de ruta para `TheContactPage/{text?}`. La ruta de la página de contacto tiene un orden predeterminado de `null` (`Order = 0`), por lo que coincide antes de la plantilla de ruta `{globalTemplate?}`.
* Más adelante en el tema se agrega una plantilla de ruta de `{aboutTemplate?}`. La plantilla `{aboutTemplate?}` tiene un `Order` con un valor de `2`. Cuando la página About se solicita en `/About/RouteDataValue`, "RouteDataValue" se carga en `RouteData.Values["globalTemplate"]` (`Order = 1`) y no `RouteData.Values["aboutTemplate"]` (`Order = 2`), debido a la configuración de la propiedad `Order`.
* Más adelante en el tema se agrega una plantilla de ruta de `{otherPagesTemplate?}`. La plantilla `{otherPagesTemplate?}` tiene un `Order` con un valor de `2`. Cuando se solicita una página de la carpeta *pages/OtherPages* con un parámetro de ruta (por ejemplo, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" se carga en `RouteData.Values["globalTemplate"]` (`Order = 1`) y no `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) debido al establecimiento de la propiedad `Order`.

Siempre que sea posible, no establezca el `Order`, lo que resulta en `Order = 0`. Confíe en el enrutamiento para seleccionar la ruta correcta.

Razor Pages opciones, como agregar <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, se agregan cuando MVC se agrega a la colección de servicios en `Startup.ConfigureServices`. Para obtener un ejemplo, vea la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

Solicite la página About del ejemplo en `localhost:5000/About/GlobalRouteValue` y revise el resultado:

![La página About se solicita con un segmento de ruta de GlobalRouteValue. La página presentada refleja que el valor de datos de ruta se captura en el método OnGet de la página.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a>Agregar una Convención de modelo de aplicación a todas las páginas

Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> para crear y agregar un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> a la colección de instancias de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> que se aplican durante la construcción del modelo de aplicación de página.

Para demostrar esta y otras convenciones más adelante en este tema, la aplicación de ejemplo incluye una clase `AddHeaderAttribute`. El constructor de esta clase acepta una cadena `name` y una matriz de cadenas `values`. Estos valores se usan en su método `OnResultExecuting` para establecer un encabezado de respuesta. La clase completa se muestra en la sección [Convenciones de acción del modelo de página](#page-model-action-conventions) más adelante en este tema.

En la aplicación de ejemplo se usa la clase `AddHeaderAttribute` para agregar un encabezado (`GlobalHeader`) a todas las páginas de la aplicación:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

::: moniker-end

*Startup.cs*:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

::: moniker-end

Solicite la página About del ejemplo en `localhost:5000/About` y revise los encabezados para ver el resultado:

![Los encabezados de respuesta de la página About ponen de manifiesto que GlobalHeader se ha agregado.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a>Agregar una Convención de modelo de controlador a todas las páginas

Utilice <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> para crear y agregar un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> a la colección de instancias de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> que se aplican durante la construcción del modelo de controlador de páginas.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

::: moniker-end

*Startup.cs*:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

::: moniker-end

## <a name="page-route-action-conventions"></a>Convenciones de acción de ruta de página

El proveedor de modelos de ruta predeterminado que deriva de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invoca convenciones que están diseñadas para proporcionar puntos de extensibilidad para configurar rutas de página.

### <a name="folder-route-model-convention"></a>Convención de modelo de ruta de carpeta

Utilice <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> para crear y agregar un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> que invoca una acción en el <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> para todas las páginas de la carpeta especificada.

En la aplicación de ejemplo se usa <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> para agregar una plantilla de ruta `{otherPagesTemplate?}` a las páginas de la carpeta *OtherPages*:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

La propiedad <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> está establecida en `2`. Esto garantiza que la plantilla para `{globalTemplate?}` (establecida anteriormente en el tema para `1`) tiene prioridad para la primera posición de valor de datos de ruta cuando se proporciona un valor de ruta único. Si se solicita una página de la carpeta *pages/OtherPages* con un valor de parámetro de ruta (por ejemplo, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" se carga en `RouteData.Values["globalTemplate"]` (`Order = 1`) y no `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) debido al establecimiento de la propiedad `Order`.

Siempre que sea posible, no establezca el `Order`, lo que resulta en `Order = 0`. Confíe en el enrutamiento para seleccionar la ruta correcta.

Solicite la página Page1 del ejemplo en `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` y revise el resultado:

![Page1 en la carpeta OtherPages solicitada con un segmento de ruta de GlobalRouteValue y OtherPagesRouteValue La página presentada refleja que los valores de datos de ruta se capturan en el método OnGet de la página.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a>Convención de modelo de ruta de página

Utilice <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> para crear y agregar un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> que invoca una acción en el <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> de la página con el nombre especificado.

En la aplicación de ejemplo se usa `AddPageRouteModelConvention` para agregar una plantilla de ruta `{aboutTemplate?}` a la página About:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

La propiedad <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> está establecida en `2`. Esto garantiza que la plantilla para `{globalTemplate?}` (establecida anteriormente en el tema para `1`) tiene prioridad para la primera posición de valor de datos de ruta cuando se proporciona un valor de ruta único. Si la página about se solicita con un valor de parámetro de ruta en `/About/RouteDataValue`, "RouteDataValue" se carga en `RouteData.Values["globalTemplate"]` (`Order = 1`) y no `RouteData.Values["aboutTemplate"]` (`Order = 2`) debido al establecimiento de la propiedad `Order`.

Siempre que sea posible, no establezca el `Order`, lo que resulta en `Order = 0`. Confíe en el enrutamiento para seleccionar la ruta correcta.

Solicite la página About del ejemplo en `localhost:5000/About/GlobalRouteValue/AboutRouteValue` y revise el resultado:

![La página About se solicita con los segmentos de ruta de GlobalRouteValue y AboutRouteValue. La página presentada refleja que los valores de datos de ruta se capturan en el método OnGet de la página.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

::: moniker range=">= aspnetcore-2.2"

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a>Usar un transformador de parámetros para personalizar las rutas de página

Las rutas de página generadas por ASP.NET Core pueden personalizarse mediante un transformador de parámetros. Un transformador de parámetro implementa `IOutboundParameterTransformer` y transforma el valor de parámetros. Por ejemplo, un transformador de parámetros `SlugifyParameterTransformer` personalizado cambia el valor de ruta `SubscriptionManagement` a `subscription-management`.

La Convención del modelo de ruta de página `PageRouteTransformerConvention` aplica un transformador de parámetros a los segmentos de nombre de archivo y carpeta de las rutas de página generadas automáticamente en una aplicación. Por ejemplo, el archivo Razor Pages en */pages/SubscriptionManagement/ViewAll.cshtml* tendría que volver a escribir su ruta de `/SubscriptionManagement/ViewAll` a `/subscription-management/view-all`.

`PageRouteTransformerConvention` solo transforma los segmentos generados automáticamente de una ruta de página que proceden de la carpeta Razor Pages y el nombre de archivo. No transforma segmentos de ruta agregados con la Directiva `@page`. La Convención tampoco transforma las rutas agregadas por <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.

El `PageRouteTransformerConvention` se registra como una opción en `Startup.ConfigureServices`:

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddRazorPages()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(
                new PageRouteTransformerConvention(
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

::: moniker range="= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(
                new PageRouteTransformerConvention(
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

## <a name="configure-a-page-route"></a>Configurar una ruta de página

Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> para configurar una ruta a una página en la ruta de acceso de la página especificada. Los vínculos generados que apuntan a esa página usan la ruta especificada. `AddPageRoute` usa `AddPageRouteModelConvention` para establecer la ruta.

En la aplicación de ejemplo se crea una ruta a `/TheContactPage` para *Contact.cshtml*:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

A la página Contact también se puede llegar en `/Contact` a través de su ruta predeterminada.

La ruta personalizada de la aplicación de ejemplo que lleva a la página Contact tiene cabida para un segmento de ruta `text` opcional (`{text?}`). La página también incluye este segmento opcional en su directiva `@page` por si el usuario que la visita tiene acceso a la página en su ruta `/Contact`:

::: moniker range=">= aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/3.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

Observe que la dirección URL generada para el vínculo **Contact** en la página presentada refleja la ruta actualizada:

![Vínculo Contact de la aplicación de ejemplo en la barra de navegación](razor-pages-conventions/_static/contact-link.png)

![Si revisamos el vínculo Contact en el HTML presentado, vemos que href está establecido en "/TheContactPage".](razor-pages-conventions/_static/contact-link-source.png)

Visite la página Contact a través de su ruta normal, `/Contact`, o de la ruta personalizada, `/TheContactPage`. Si se proporciona un segmento de ruta `text` adicional, la página muestra el segmento codificado en HTML que se ha proporcionado:

![Ejemplo del explorador Microsoft Edge en el que se proporciona un segmento de ruta opcional "text" de "TextValue" en la dirección URL La página presentada muestra el valor del segmento "text".](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>Convenciones de acción del modelo de página

El proveedor de modelos de página predeterminado que implementa <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invoca convenciones que están diseñadas para proporcionar puntos de extensibilidad para la configuración de modelos de página. Estas convenciones son útiles al crear y modificar escenarios de detección y procesamiento de páginas.

En los ejemplos de esta sección, la aplicación de ejemplo usa una clase `AddHeaderAttribute`, que es una <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, que aplica un encabezado de respuesta:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

Si empleamos convenciones, el ejemplo indica cómo aplicar el atributo a todas las páginas de una carpeta y a una sola página.

**Convención de modelo de aplicación de carpeta**

Utilice <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> para crear y agregar un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> que invoca una acción en <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instancias de para todas las páginas de la carpeta especificada.

En el ejemplo se demuestra el uso de `AddFolderApplicationModelConvention` agregando un encabezado (`OtherPagesHeader`) a las páginas de la carpeta *OtherPages* de la aplicación:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

Solicite la página Page1 del ejemplo en `localhost:5000/OtherPages/Page1` y revise los encabezados para ver el resultado:

![Los encabezados de respuesta de la página OtherPages/Page1 ponen de manifiesto que OtherPagesHeader se ha agregado.](razor-pages-conventions/_static/page1-otherpages-header.png)

**Convención de modelo de aplicación de página**

Utilice <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> para crear y agregar un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> que invoca una acción en el <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> de la página con el nombre especificado.

En el ejemplo se demuestra el uso de `AddPageApplicationModelConvention` agregando un encabezado (`AboutHeader`) a la página About:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

Solicite la página About del ejemplo en `localhost:5000/About` y revise los encabezados para ver el resultado:

![Los encabezados de respuesta de la página About ponen de manifiesto que AboutHeader se ha agregado.](razor-pages-conventions/_static/about-page-about-header.png)

**Configurar un filtro**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configura el filtro especificado que se va a aplicar. Se puede implementar una clase de filtro, pero en la aplicación de ejemplo se muestra cómo implementar un filtro en una expresión lambda, cosa que sucede en segundo plano como una fábrica que devuelve un filtro:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

El modelo de páginas de aplicación se usa para comprobar la ruta de acceso relativa de los segmentos que conducen a la página Page2 en la carpeta *OtherPages*. Si la condición se supera, se agrega un encabezado. Si no, se aplica `EmptyFilter`.

`EmptyFilter` es un [filtro de acciones](xref:mvc/controllers/filters#action-filters). Dado que Razor Pages omite los filtros de acción, el `EmptyFilter` no tiene ningún efecto según lo previsto si la ruta de acceso no contiene `OtherPages/Page2`.

Solicite la página Page2 del ejemplo en `localhost:5000/OtherPages/Page2` y revise los encabezados para ver el resultado:

![OtherPagesPage2Header se agrega a la respuesta de Page2.](razor-pages-conventions/_static/page2-filter-header.png)

**Configurar una fábrica de filtros**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configura el generador especificado para aplicar [filtros](xref:mvc/controllers/filters) a todos los Razor pages.

En la aplicación de ejemplo se proporciona un ejemplo del uso de una [fábrica de filtros](xref:mvc/controllers/filters#ifilterfactory), para lo cual se agrega un encabezado (`FilterFactoryHeader`) con dos valores a las páginas de la aplicación:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet9)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

::: moniker-end

*AddHeaderWithFactory.cs*:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

::: moniker-end

Solicite la página About del ejemplo en `localhost:5000/About` y revise los encabezados para ver el resultado:

![Los encabezados de respuesta de la página About ponen de manifiesto que se han agregado dos encabezados FilterFactoryHeader.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>Filtros de MVC y filtro de página (IPageFilter)

Las páginas de Razor no tienen en cuenta los [filtros de acciones](xref:mvc/controllers/filters#action-filters) de MVC, porque en este tipo de páginas se usan métodos de control. Hay disponibles otros tipos de filtro de MVC que puede usar: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters) y [Result](xref:mvc/controllers/filters#result-filters). Para más información, vea el tema [Filters](xref:mvc/controllers/filters) (Filtros).

El filtro de página (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) es un filtro que se aplica a Razor Pages. Para más información, vea [Filter methods for Razor Pages](xref:razor-pages/filter) (Métodos de filtrado para páginas de Razor).

## <a name="additional-resources"></a>Recursos adicionales

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>
