---
title: Convenciones de aplicación y de ruta de páginas de Razor en ASP.NET Core
author: guardrex
description: Vea cómo las convenciones de proveedor de modelos de aplicación y de ruta sirven para controlar el enrutamiento, la detección y el procesamiento de páginas.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/07/2019
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: 4e07b5803adbce94982584212fa65afbfd427b64
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64893512"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a>Convenciones de aplicación y de ruta de páginas de Razor en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

Aquí encontrará información para saber cómo usar las [convenciones de proveedor de modelos de aplicación y de ruta](xref:mvc/controllers/application-model#conventions) con objeto de controlar el enrutamiento, la detección y el procesamiento en aplicaciones de páginas de Razor.

Si necesita configurar rutas de una página personalizadas para páginas concretas, configure el enrutamiento a esas páginas con la [convención AddPageRoute](#configure-a-page-route), descrita más adelante en este tema.

Para especificar una ruta de página, agregue segmentos de ruta o agregar parámetros a una ruta, use la página `@page` directiva. Para obtener más información, consulte [rutas personalizadas](xref:razor-pages/index#custom-routes).

Hay palabras reservadas que no se puede usar como segmentos de ruta o nombres de parámetro. Para obtener más información, consulte [enrutamiento: Enrutamientos nombres reservados](xref:fundamentals/routing#reserved-routing-names).

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))

| Escenario | El ejemplo explica cómo... |
| -------- | --------------------------- |
| [Convenciones de modelo](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li><li>IPageHandlerModelConvention</li></ul> | Agregar un encabezado y una plantilla de ruta a las páginas de una aplicación. |
| [Convenciones de acción de ruta de página](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Agregar una plantilla de ruta a las páginas de una carpeta y a una sola página. |
| [Convenciones de acción del modelo de página](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (clase de filtro, expresión lambda o fábrica de filtros)</li></ul> | Agregar un encabezado a las páginas de una carpeta, agregar un encabezado a una sola página y configurar una [fábrica de filtros](xref:mvc/controllers/filters#ifilterfactory) para agregar un encabezado a las páginas de la aplicación. |

Convenciones de páginas de Razor se agregan y configuran mediante el <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> método de extensión para <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> en la colección de servicios en la `Startup` clase. Más avanzado el tema veremos los siguientes ejemplos de convención:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
            {
                options.Conventions.Add( ... );
                options.Conventions.AddFolderRouteModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageRouteModelConvention("/About", model => { ... });
                options.Conventions.AddPageRoute("/Contact", "TheContactPage/{text?}");
                options.Conventions.AddFolderApplicationModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageApplicationModelConvention("/About", model => { ... });
                options.Conventions.ConfigureFilter(model => { ... });
                options.Conventions.ConfigureFilter( ... );
            });
}
```

## <a name="route-order"></a>Orden de la ruta

Rutas especifican una <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> para procesar (coincidencia de ruta).

| Ordenar            | Comportamiento |
| :--------------: | -------- |
| -1               | La ruta se procesa antes de otras rutas se procesan. |
| 0                | No se especifica el orden (valor predeterminado). No asignar `Order` (`Order = null`) el valor predeterminado es la ruta `Order` en 0 (cero) para su procesamiento. |
| 1, 2, &hellip; n | Especifica el orden de procesamiento de la ruta. |

Procesamiento de la ruta se establece mediante la convención:

* Las rutas se procesan en orden secuencial (-1, 0, 1, 2, &hellip; n).
* Cuando las rutas tienen el mismo `Order`, más ruta específica se compara primero, seguido por rutas menos específicas.
* Cuando las rutas con el mismo `Order` y el mismo número de parámetros coincide con una dirección URL de solicitud, las rutas se procesan en el orden en que se agreguen a la <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.

Si es posible, evite según un orden de procesamiento de la ruta establecida. Por lo general, selecciona la ruta correcta con coincidencia de dirección URL de enrutamiento. Si se debe establecer ruta `Order` propiedades para enrutar las solicitudes correctamente, esquema de enrutamiento de la aplicación es probablemente confuso para los clientes y frágil mantener. Buscar simplificar el esquema de enrutamiento de la aplicación. La aplicación de ejemplo requiere una ruta explicita de procesamiento de orden en que se muestran varios escenarios de enrutamientos mediante una sola aplicación. Sin embargo, debe intentar evitar la práctica de la ruta de configuración `Order` en aplicaciones de producción.

El enrutamiento del controlador de MVC y el enrutamiento de Razor Pages comparten una implementación. Información sobre el orden de la ruta en los temas MVC está disponible en [enrutamiento a acciones del controlador: Orden de las rutas de atributo](xref:mvc/controllers/routing#ordering-attribute-routes).

## <a name="model-conventions"></a>Convenciones de modelo

Agregar un delegado para <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> para agregar [convenciones del modelo](xref:mvc/controllers/application-model#conventions) que se aplican a las páginas de Razor.

### <a name="add-a-route-model-convention-to-all-pages"></a>Agregar una convención de modelo de ruta a todas las páginas

Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> para crear y agregar un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> a la colección de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instancias que se aplican durante la ruta de página construcción del modelo.

En la aplicación de ejemplo se agrega una plantilla de ruta `{globalTemplate?}` a todas las páginas de la aplicación:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

La propiedad <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> está establecida en `1`. Esto garantiza que la ruta siguiente que coincida con el comportamiento de la aplicación de ejemplo:

* Una plantilla de ruta para `TheContactPage/{text?}` se agrega más adelante en el tema. La ruta de la página Contact tiene un orden predeterminado de `null` (`Order = 0`), para que coincida con antes el `{globalTemplate?}` plantilla de ruta.
* Un `{aboutTemplate?}` plantilla de ruta se agrega más adelante en el tema. La plantilla `{aboutTemplate?}` tiene un `Order` con un valor de `2`. Cuando la página About se solicita en `/About/RouteDataValue`, "RouteDataValue" se carga en `RouteData.Values["globalTemplate"]` (`Order = 1`) y no `RouteData.Values["aboutTemplate"]` (`Order = 2`), debido a la configuración de la propiedad `Order`.
* Un `{otherPagesTemplate?}` plantilla de ruta se agrega más adelante en el tema. La plantilla `{otherPagesTemplate?}` tiene un `Order` con un valor de `2`. Cuando cualquier página en el *OtherPages/páginas* carpeta se solicita con un parámetro de ruta (por ejemplo, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" se carga en `RouteData.Values["globalTemplate"]` (`Order = 1`) y no `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) debido a la configuración del `Order` propiedad.

Siempre que sea posible, no establezca la `Order`, que da como resultado `Order = 0`. Se basan en enrutamiento para seleccionar la ruta correcta.

Opciones de páginas de Razor, como agregar <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, se agregan cuando MVC se agrega a la colección de servicios en `Startup.ConfigureServices`. Para obtener un ejemplo, vea la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

Solicite la página About del ejemplo en `localhost:5000/About/GlobalRouteValue` y revise el resultado:

![La página About se solicita con un segmento de ruta de GlobalRouteValue. La página presentada refleja que el valor de datos de ruta se captura en el método OnGet de la página.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a>Agregar una convención de modelo de aplicación a todas las páginas

Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> para crear y agregar un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> a la colección de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instancias que se aplican durante la construcción del modelo de aplicación de página.

Para demostrar esta y otras convenciones más adelante en este tema, la aplicación de ejemplo incluye una clase `AddHeaderAttribute`. El constructor de esta clase acepta una cadena `name` y una matriz de cadenas `values`. Estos valores se usan en su método `OnResultExecuting` para establecer un encabezado de respuesta. La clase completa se muestra en la sección [Convenciones de acción del modelo de página](#page-model-action-conventions) más adelante en este tema.

En la aplicación de ejemplo se usa la clase `AddHeaderAttribute` para agregar un encabezado (`GlobalHeader`) a todas las páginas de la aplicación:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

Solicite la página About del ejemplo en `localhost:5000/About` y revise los encabezados para ver el resultado:

![Los encabezados de respuesta de la página About ponen de manifiesto que GlobalHeader se ha agregado.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a>Agregar una convención de modelo de controlador a todas las páginas

Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> para crear y agregar un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> a la colección de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instancias que se aplican durante la construcción del modelo de controlador de página.

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a>Convenciones de acción de ruta de página

El proveedor de modelo de ruta predeterminado que se deriva de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invoca convenciones que están diseñadas para proporcionar puntos de extensibilidad para configurar las rutas de la página.

### <a name="folder-route-model-convention"></a>Convención de modelo de ruta de carpeta

Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> para crear y agregar un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> que invoque una acción en el <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> para todas las páginas en la carpeta especificada.

En la aplicación de ejemplo se usa <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> para agregar una plantilla de ruta `{otherPagesTemplate?}` a las páginas de la carpeta *OtherPages*:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

La propiedad <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> está establecida en `2`. Esto garantiza que la plantilla para `{globalTemplate?}` (establecido anteriormente en este tema para `1`) tiene prioridad para la posición de valor de datos de la primera ruta cuando se proporciona un solo valor de ruta. Si una página en el *OtherPages/páginas* carpeta se solicita con un valor de parámetro de ruta (por ejemplo, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" se carga en `RouteData.Values["globalTemplate"]` (`Order = 1`) y no `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) debido a la configuración del `Order` propiedad.

Siempre que sea posible, no establezca la `Order`, que da como resultado `Order = 0`. Se basan en enrutamiento para seleccionar la ruta correcta.

Solicite la página Page1 del ejemplo en `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` y revise el resultado:

![Page1 en la carpeta OtherPages solicitada con un segmento de ruta de GlobalRouteValue y OtherPagesRouteValue La página presentada refleja que los valores de datos de ruta se capturan en el método OnGet de la página.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a>Convención de modelo de ruta de página

Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> para crear y agregar un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> que invoque una acción en el <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> para la página con el nombre especificado.

En la aplicación de ejemplo se usa `AddPageRouteModelConvention` para agregar una plantilla de ruta `{aboutTemplate?}` a la página About:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

La propiedad <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> está establecida en `2`. Esto garantiza que la plantilla para `{globalTemplate?}` (establecido anteriormente en este tema para `1`) tiene prioridad para la posición de valor de datos de la primera ruta cuando se proporciona un solo valor de ruta. Si la página About se solicita con un valor de parámetro de ruta en `/About/RouteDataValue`, "RouteDataValue" se carga en `RouteData.Values["globalTemplate"]` (`Order = 1`) y no `RouteData.Values["aboutTemplate"]` (`Order = 2`) debido a la configuración del `Order` propiedad.

Siempre que sea posible, no establezca la `Order`, que da como resultado `Order = 0`. Se basan en enrutamiento para seleccionar la ruta correcta.

Solicite la página About del ejemplo en `localhost:5000/About/GlobalRouteValue/AboutRouteValue` y revise el resultado:

![La página About se solicita con los segmentos de ruta de GlobalRouteValue y AboutRouteValue. La página presentada refleja que los valores de datos de ruta se capturan en el método OnGet de la página.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

::: moniker range=">= aspnetcore-2.2"

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a>Usar un transformador de parámetro para personalizar las rutas de página

Las rutas de página generadas por ASP.NET Core pueden personalizarse mediante un transformador de parámetro. Un transformador de parámetro implementa `IOutboundParameterTransformer` y transforma el valor de parámetros. Por ejemplo, un transformador de parámetros `SlugifyParameterTransformer` personalizado cambia el valor de ruta `SubscriptionManagement` a `subscription-management`.

El `PageRouteTransformerConvention` convención de modelo de ruta de página aplica un transformador de parámetro a los segmentos de nombre de carpeta y archivo de rutas de página generada automáticamente en una aplicación. Por ejemplo, las páginas de Razor de archivos en */Pages/SubscriptionManagement/ViewAll.cshtml* tendría su ruta reescribe desde `/SubscriptionManagement/ViewAll` a `/subscription-management/view-all`.

`PageRouteTransformerConvention` solo transforma los segmentos de una ruta de página generados automáticamente que proceden de la carpeta de las páginas de Razor y el nombre de archivo. No transforma los segmentos de ruta agregados con el `@page` directiva. La convención también no transforma las rutas agregadas por <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.

El `PageRouteTransformerConvention` está registrado como una opción en `Startup.ConfigureServices`:

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

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

A la página Contact también se puede llegar en `/Contact` a través de su ruta predeterminada.

La ruta personalizada de la aplicación de ejemplo que lleva a la página Contact tiene cabida para un segmento de ruta `text` opcional (`{text?}`). La página también incluye este segmento opcional en su directiva `@page` por si el usuario que la visita tiene acceso a la página en su ruta `/Contact`:

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

Observe que la dirección URL generada para el vínculo **Contact** en la página presentada refleja la ruta actualizada:

![Vínculo Contact de la aplicación de ejemplo en la barra de navegación](razor-pages-conventions/_static/contact-link.png)

![Si revisamos el vínculo Contact en el HTML presentado, vemos que href está establecido en "/TheContactPage".](razor-pages-conventions/_static/contact-link-source.png)

Visite la página Contact a través de su ruta normal, `/Contact`, o de la ruta personalizada, `/TheContactPage`. Si se proporciona un segmento de ruta `text` adicional, la página muestra el segmento codificado en HTML que se ha proporcionado:

![Ejemplo del explorador Microsoft Edge en el que se proporciona un segmento de ruta opcional "text" de "TextValue" en la dirección URL La página presentada muestra el valor del segmento "text".](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>Convenciones de acción del modelo de página

El proveedor de modelo de página predeterminado que implementa <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invoca convenciones que están diseñadas para proporcionar puntos de extensibilidad para configurar los modelos de página. Estas convenciones son útiles al crear y modificar escenarios de detección y procesamiento de páginas.

Los ejemplos en esta sección, la aplicación de ejemplo usa un `AddHeaderAttribute` (clase), que es un <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, que aplica un encabezado de respuesta:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

Si empleamos convenciones, el ejemplo indica cómo aplicar el atributo a todas las páginas de una carpeta y a una sola página.

**Convención de modelo de aplicación de carpeta**

Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> para crear y agregar un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> que invoque una acción en <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instancias para todas las páginas en la carpeta especificada.

En el ejemplo se demuestra el uso de `AddFolderApplicationModelConvention` agregando un encabezado (`OtherPagesHeader`) a las páginas de la carpeta *OtherPages* de la aplicación:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

Solicite la página Page1 del ejemplo en `localhost:5000/OtherPages/Page1` y revise los encabezados para ver el resultado:

![Los encabezados de respuesta de la página OtherPages/Page1 ponen de manifiesto que OtherPagesHeader se ha agregado.](razor-pages-conventions/_static/page1-otherpages-header.png)

**Convención de modelo de aplicación de página**

Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> para crear y agregar un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> que invoque una acción en el <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> para la página con el nombre especificado.

En el ejemplo se demuestra el uso de `AddPageApplicationModelConvention` agregando un encabezado (`AboutHeader`) a la página About:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

Solicite la página About del ejemplo en `localhost:5000/About` y revise los encabezados para ver el resultado:

![Los encabezados de respuesta de la página About ponen de manifiesto que AboutHeader se ha agregado.](razor-pages-conventions/_static/about-page-about-header.png)

**Configurar un filtro**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configura el filtro que se aplican. Se puede implementar una clase de filtro, pero en la aplicación de ejemplo se muestra cómo implementar un filtro en una expresión lambda, cosa que sucede en segundo plano como una fábrica que devuelve un filtro:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

El modelo de páginas de aplicación se usa para comprobar la ruta de acceso relativa de los segmentos que conducen a la página Page2 en la carpeta *OtherPages*. Si la condición se supera, se agrega un encabezado. Si no, se aplica `EmptyFilter`.

`EmptyFilter` es un [filtro de acciones](xref:mvc/controllers/filters#action-filters). Las páginas de Razor pasan por alto los filtros de acción, así que `EmptyFilter` no funciona del modo previsto si la ruta de acceso no contiene `OtherPages/Page2`.

Solicite la página Page2 del ejemplo en `localhost:5000/OtherPages/Page2` y revise los encabezados para ver el resultado:

![OtherPagesPage2Header se agrega a la respuesta de Page2.](razor-pages-conventions/_static/page2-filter-header.png)

**Configurar una fábrica de filtros**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configura la fábrica especificada para aplicar [filtros](xref:mvc/controllers/filters) a todas las páginas de Razor.

En la aplicación de ejemplo se proporciona un ejemplo del uso de una [fábrica de filtros](xref:mvc/controllers/filters#ifilterfactory), para lo cual se agrega un encabezado (`FilterFactoryHeader`) con dos valores a las páginas de la aplicación:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

Solicite la página About del ejemplo en `localhost:5000/About` y revise los encabezados para ver el resultado:

![Los encabezados de respuesta de la página About ponen de manifiesto que se han agregado dos encabezados FilterFactoryHeader.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>Filtros de MVC y filtro de página (IPageFilter)

Las páginas de Razor no tienen en cuenta los [filtros de acciones](xref:mvc/controllers/filters#action-filters) de MVC, porque en este tipo de páginas se usan métodos de control. Otros tipos de filtros de MVC están disponibles para su uso: [Autorización](xref:mvc/controllers/filters#authorization-filters), [excepción](xref:mvc/controllers/filters#exception-filters), [recursos](xref:mvc/controllers/filters#resource-filters), y [resultado](xref:mvc/controllers/filters#result-filters). Para más información, vea el tema [Filters](xref:mvc/controllers/filters) (Filtros).

El filtro de página (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) es un filtro que se aplica a las páginas de Razor. Para más información, vea [Filter methods for Razor Pages](xref:razor-pages/filter) (Métodos de filtrado para páginas de Razor).

## <a name="additional-resources"></a>Recursos adicionales

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>
