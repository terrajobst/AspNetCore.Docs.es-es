---
title: Convenciones de aplicación y de ruta de páginas de Razor en ASP.NET Core
author: guardrex
description: Vea cómo las convenciones de proveedor de modelos de aplicación y de ruta sirven para controlar el enrutamiento, la detección y el procesamiento de páginas.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/17/2018
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: ea4f785dc8a64b430e312fd122a4d3184b61949e
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011867"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a>Convenciones de aplicación y de ruta de páginas de Razor en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

Aquí encontrará información para saber cómo usar las [convenciones de proveedor de modelos de aplicación y de ruta](xref:mvc/controllers/application-model#conventions) con objeto de controlar el enrutamiento, la detección y el procesamiento en aplicaciones de páginas de Razor.

Si necesita configurar rutas de una página personalizadas para páginas concretas, configure el enrutamiento a esas páginas con la [convención AddPageRoute](#configure-a-page-route), descrita más adelante en este tema.

Para especificar una ruta de página, agregue segmentos de ruta o agregar parámetros a una ruta, use la página `@page` directiva. Para obtener más información, consulte [rutas personalizadas](xref:razor-pages/index#custom-routes).

Hay palabras reservadas que no se puede usar como segmentos de ruta o nombres de parámetro. Para obtener más información, consulte [enrutamiento: enrutamientos nombres reservados](xref:fundamentals/routing#reserved-routing-names).

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/sample/) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

::: moniker range="= aspnetcore-2.0"

| Escenario | El ejemplo explica cómo... |
| -------- | --------------------------- |
| [Convenciones de modelo](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li></ul> | Agregar un encabezado y una plantilla de ruta a las páginas de una aplicación. |
| [Convenciones de acción de ruta de página](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Agregar una plantilla de ruta a las páginas de una carpeta y a una sola página. |
| [Convenciones de acción del modelo de página](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (clase de filtro, expresión lambda o fábrica de filtros)</li></ul> | Agregar un encabezado a las páginas de una carpeta, agregar un encabezado a una sola página y configurar una [fábrica de filtros](xref:mvc/controllers/filters#ifilterfactory) para agregar un encabezado a las páginas de la aplicación. |
| [Proveedor de modelos de aplicación de página predeterminado](#replace-the-default-page-app-model-provider) | Reemplazar el proveedor de modelos de página predeterminado para cambiar las convenciones de nombres de controlador. |

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

| Escenario | El ejemplo explica cómo... |
| -------- | --------------------------- |
| [Convenciones de modelo](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li><li>IPageHandlerModelConvention</li></ul> | Agregar un encabezado y una plantilla de ruta a las páginas de una aplicación. |
| [Convenciones de acción de ruta de página](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Agregar una plantilla de ruta a las páginas de una carpeta y a una sola página. |
| [Convenciones de acción del modelo de página](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (clase de filtro, expresión lambda o fábrica de filtros)</li></ul> | Agregar un encabezado a las páginas de una carpeta, agregar un encabezado a una sola página y configurar una [fábrica de filtros](xref:mvc/controllers/filters#ifilterfactory) para agregar un encabezado a las páginas de la aplicación. |
| [Proveedor de modelos de aplicación de página predeterminado](#replace-the-default-page-app-model-provider) | Reemplazar el proveedor de modelos de página predeterminado para cambiar las convenciones de nombres de controlador. |

::: moniker-end

Las convenciones de páginas de Razor se agregan y configuran a través del método de extensión [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) a [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc) en la colección de servicio de la clase `Startup`. Más avanzado el tema veremos los siguientes ejemplos de convención:

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

| Orden            | Comportamiento |
| :--------------: | -------- |
| -1               | La ruta se procesa antes de otras rutas se procesan. |
| 0                | No se especifica el orden (valor predeterminado). No asignar `Order` (`Order = null`) el valor predeterminado es la ruta `Order` en 0 (cero) para su procesamiento. |
| 1, 2, &hellip; n | Especifica el orden de procesamiento de la ruta. |

Procesamiento de la ruta se establece mediante la convención:

* Las rutas se procesan en orden secuencial (-1, 0, 1, 2, &hellip; n).
* Cuando las rutas tienen el mismo `Order`, más ruta específica se compara primero, seguido por rutas menos específicas.
* Cuando las rutas con el mismo `Order` y el mismo número de parámetros coincide con una dirección URL de solicitud, las rutas se procesan en el orden en que se agreguen a la <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.

Si es posible, evite según un orden de procesamiento de la ruta establecida. Por lo general, selecciona la ruta correcta con coincidencia de dirección URL de enrutamiento. Si se debe establecer ruta `Order` propiedades para enrutar las solicitudes correctamente, esquema de enrutamiento de la aplicación es probablemente confuso para los clientes y frágil mantener. Buscar simplificar el esquema de enrutamiento de la aplicación. La aplicación de ejemplo requiere una ruta explicita de procesamiento de orden en que se muestran varios escenarios de enrutamientos mediante una sola aplicación. Sin embargo, debe intentar evitar la práctica de la ruta de configuración `Order` en aplicaciones de producción.

Enrutamiento de las páginas de Razor y compartir de enrutamiento de controlador MVC una implementación. Información sobre el orden de la ruta en los temas MVC está disponible en [enrutamiento a acciones del controlador: orden de las rutas de atributo](xref:mvc/controllers/routing#ordering-attribute-routes).

## <a name="model-conventions"></a>Convenciones de modelo

Agregue un delegado de [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) con el fin de agregar [convenciones de modelo](xref:mvc/controllers/application-model#conventions) y aplicarlas a páginas de Razor.

**Agregar una convención de modelo de ruta a todas las páginas**

Use [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) para crear y agregar un [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) a la colección de instancias [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) que se van a aplicar durante la construcción del modelo de ruta de página.

En la aplicación de ejemplo se agrega una plantilla de ruta `{globalTemplate?}` a todas las páginas de la aplicación:

[!code-csharp[](razor-pages-conventions/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

La propiedad <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> está establecida en `1`. Esto garantiza que la ruta siguiente que coincida con el comportamiento de la aplicación de ejemplo:

* Una plantilla de ruta para `TheContactPage/{text?}` se agrega más adelante en el tema. La ruta de la página Contact tiene un orden predeterminado de `null` (`Order = 0`), para que coincida con antes el `{globalTemplate?}` plantilla de ruta.
* Un `{aboutTemplate?}` plantilla de ruta se agrega más adelante en el tema. La plantilla `{aboutTemplate?}` tiene un `Order` con un valor de `2`. Cuando la página About se solicita en `/About/RouteDataValue`, "RouteDataValue" se carga en `RouteData.Values["globalTemplate"]` (`Order = 1`) y no `RouteData.Values["aboutTemplate"]` (`Order = 2`), debido a la configuración de la propiedad `Order`.
* Un `{otherPagesTemplate?}` plantilla de ruta se agrega más adelante en el tema. La plantilla `{otherPagesTemplate?}` tiene un `Order` con un valor de `2`. Cuando cualquier página en el *OtherPages/páginas* carpeta se solicita con un parámetro de ruta (por ejemplo, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" se carga en `RouteData.Values["globalTemplate"]` (`Order = 1`) y no `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) debido a la configuración del `Order` propiedad.

Siempre que sea posible, no establezca la `Order`, que da como resultado `Order = 0`. Se basan en enrutamiento para seleccionar la ruta correcta.

Las opciones de páginas de Razor (como, por ejemplo, agregar [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)) se agregan cuando MVC se agrega a la colección de servicio en `Startup.ConfigureServices`. Para obtener un ejemplo, vea la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/sample/).

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet1)]

Solicite la página About del ejemplo en `localhost:5000/About/GlobalRouteValue` y revise el resultado:

![La página About se solicita con un segmento de ruta de GlobalRouteValue. La página presentada refleja que el valor de datos de ruta se captura en el método OnGet de la página.](razor-pages-conventions/_static/about-page-global-template.png)

**Agregar una convención de modelo de aplicación a todas las páginas**

Use [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) para crear y agregar un [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) a la colección de instancias [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) que se van a aplicar durante la construcción del modelo de aplicación de página.

Para demostrar esta y otras convenciones más adelante en este tema, la aplicación de ejemplo incluye una clase `AddHeaderAttribute`. El constructor de esta clase acepta una cadena `name` y una matriz de cadenas `values`. Estos valores se usan en su método `OnResultExecuting` para establecer un encabezado de respuesta. La clase completa se muestra en la sección [Convenciones de acción del modelo de página](#page-model-action-conventions) más adelante en este tema.

En la aplicación de ejemplo se usa la clase `AddHeaderAttribute` para agregar un encabezado (`GlobalHeader`) a todas las páginas de la aplicación:

[!code-csharp[](razor-pages-conventions/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet2)]

Solicite la página About del ejemplo en `localhost:5000/About` y revise los encabezados para ver el resultado:

![Los encabezados de respuesta de la página About ponen de manifiesto que GlobalHeader se ha agregado.](razor-pages-conventions/_static/about-page-global-header.png)

::: moniker range=">= aspnetcore-2.1"

**Agregar una convención de modelo de controlador a todas las páginas**

Use [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) para crear y agregar un [IPageHandlerModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipagehandlermodelconvention) a la colección de instancias [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) que se van a aplicar durante la construcción del modelo de controlador de página.

```csharp
public class GlobalPageHandlerModelConvention 
    : IPageHandlerModelConvention
{
    public void Apply(PageHandlerModel model)
    {
        ...
    }
}
```

`Startup.ConfigureServices`:

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(new GlobalPageHandlerModelConvention());
        });
```

::: moniker-end

## <a name="page-route-action-conventions"></a>Convenciones de acción de ruta de página

El proveedor de modelos de ruta predeterminado que se deriva de [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) invoca convenciones diseñadas para proporcionar puntos de extensibilidad que permitan configurar rutas de página.

**Convención de modelo de ruta de carpeta**

Use [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) para crear y agregar un [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) que invoque una acción en el modelo [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) de todas las páginas contenidas en la carpeta especificada.

En la aplicación de ejemplo se usa `AddFolderRouteModelConvention` para agregar una plantilla de ruta `{otherPagesTemplate?}` a las páginas de la carpeta *OtherPages*:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet3)]

La propiedad <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> está establecida en `2`. Esto garantiza que la plantilla para `{globalTemplate?}` (establecido anteriormente en este tema para `1`) tiene prioridad para la posición de valor de datos de la primera ruta cuando se proporciona un solo valor de ruta. Si una página en el *OtherPages/páginas* carpeta se solicita con un valor de parámetro de ruta (por ejemplo, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" se carga en `RouteData.Values["globalTemplate"]` (`Order = 1`) y no `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) debido a la configuración del `Order` propiedad.

Siempre que sea posible, no establezca la `Order`, que da como resultado `Order = 0`. Se basan en enrutamiento para seleccionar la ruta correcta.

Solicite la página Page1 del ejemplo en `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` y revise el resultado:

![Page1 en la carpeta OtherPages solicitada con un segmento de ruta de GlobalRouteValue y OtherPagesRouteValue La página presentada refleja que los valores de datos de ruta se capturan en el método OnGet de la página.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

**Convención de modelo de ruta de página**

Use [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) para crear y agregar un [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) que invoque una acción en el modelo [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) de la página con el nombre especificado.

En la aplicación de ejemplo se usa `AddPageRouteModelConvention` para agregar una plantilla de ruta `{aboutTemplate?}` a la página About:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet4)]

La propiedad <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> está establecida en `2`. Esto garantiza que la plantilla para `{globalTemplate?}` (establecido anteriormente en este tema para `1`) tiene prioridad para la posición de valor de datos de la primera ruta cuando se proporciona un solo valor de ruta. Si la página About se solicita con un valor de parámetro de ruta en `/About/RouteDataValue`, "RouteDataValue" se carga en `RouteData.Values["globalTemplate"]` (`Order = 1`) y no `RouteData.Values["aboutTemplate"]` (`Order = 2`) debido a la configuración del `Order` propiedad.

Siempre que sea posible, no establezca la `Order`, que da como resultado `Order = 0`. Se basan en enrutamiento para seleccionar la ruta correcta.

Solicite la página About del ejemplo en `localhost:5000/About/GlobalRouteValue/AboutRouteValue` y revise el resultado:

![La página About se solicita con los segmentos de ruta de GlobalRouteValue y AboutRouteValue. La página presentada refleja que los valores de datos de ruta se capturan en el método OnGet de la página.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a>Configurar una ruta de página

Use [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) para configurar una ruta a una página en la ruta de acceso de página especificada. Los vínculos generados que apuntan a esa página usan la ruta especificada. `AddPageRoute` usa `AddPageRouteModelConvention` para establecer la ruta.

En la aplicación de ejemplo se crea una ruta a `/TheContactPage` para *Contact.cshtml*:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet5)]

A la página Contact también se puede llegar en `/Contact` a través de su ruta predeterminada.

La ruta personalizada de la aplicación de ejemplo que lleva a la página Contact tiene cabida para un segmento de ruta `text` opcional (`{text?}`). La página también incluye este segmento opcional en su directiva `@page` por si el usuario que la visita tiene acceso a la página en su ruta `/Contact`:

[!code-cshtml[](razor-pages-conventions/sample/Pages/Contact.cshtml?highlight=1)]

Observe que la dirección URL generada para el vínculo **Contact** en la página presentada refleja la ruta actualizada:

![Vínculo Contact de la aplicación de ejemplo en la barra de navegación](razor-pages-conventions/_static/contact-link.png)

![Si revisamos el vínculo Contact en el HTML presentado, vemos que href está establecido en "/TheContactPage".](razor-pages-conventions/_static/contact-link-source.png)

Visite la página Contact a través de su ruta normal, `/Contact`, o de la ruta personalizada, `/TheContactPage`. Si se proporciona un segmento de ruta `text` adicional, la página muestra el segmento codificado en HTML que se ha proporcionado:

![Ejemplo del explorador Microsoft Edge en el que se proporciona un segmento de ruta opcional "text" de "TextValue" en la dirección URL La página presentada muestra el valor del segmento "text".](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>Convenciones de acción del modelo de página

El proveedor de modelos de página predeterminado que implementa [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) invoca convenciones que están diseñadas para proporcionar puntos de extensibilidad que permitan configurar modelos de página. Estas convenciones son útiles al crear y modificar escenarios de detección y procesamiento de páginas.

En los ejemplos de esta sección, la aplicación de ejemplo usa una clase `AddHeaderAttribute` (que es un atributo [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute)) que se aplica un encabezado de respuesta:

[!code-csharp[](razor-pages-conventions/sample/Filters/AddHeader.cs?name=snippet1)]

Si empleamos convenciones, el ejemplo indica cómo aplicar el atributo a todas las páginas de una carpeta y a una sola página.

**Convención de modelo de aplicación de carpeta**

Use [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) para crear y agregar un [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) que invoque una acción en las instancias de [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) de todas las páginas contenidas en la carpeta especificada.

En el ejemplo se demuestra el uso de `AddFolderApplicationModelConvention` agregando un encabezado (`OtherPagesHeader`) a las páginas de la carpeta *OtherPages* de la aplicación:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet6)]

Solicite la página Page1 del ejemplo en `localhost:5000/OtherPages/Page1` y revise los encabezados para ver el resultado:

![Los encabezados de respuesta de la página OtherPages/Page1 ponen de manifiesto que OtherPagesHeader se ha agregado.](razor-pages-conventions/_static/page1-otherpages-header.png)

**Convención de modelo de aplicación de página**

Use [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) para crear y agregar un [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) que invoque una acción en el [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) para la página con el nombre especificado.

En el ejemplo se demuestra el uso de `AddPageApplicationModelConvention` agregando un encabezado (`AboutHeader`) a la página About:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet7)]

Solicite la página About del ejemplo en `localhost:5000/About` y revise los encabezados para ver el resultado:

![Los encabezados de respuesta de la página About ponen de manifiesto que AboutHeader se ha agregado.](razor-pages-conventions/_static/about-page-about-header.png)

**Configurar un filtro**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) permite configurar el filtro que se quiere aplicar. Se puede implementar una clase de filtro, pero en la aplicación de ejemplo se muestra cómo implementar un filtro en una expresión lambda, cosa que sucede en segundo plano como una fábrica que devuelve un filtro:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet8)]

El modelo de páginas de aplicación se usa para comprobar la ruta de acceso relativa de los segmentos que conducen a la página Page2 en la carpeta *OtherPages*. Si la condición se supera, se agrega un encabezado. Si no, se aplica `EmptyFilter`.

`EmptyFilter` es un [filtro de acciones](xref:mvc/controllers/filters#action-filters). Las páginas de Razor pasan por alto los filtros de acción, así que `EmptyFilter` no funciona del modo previsto si la ruta de acceso no contiene `OtherPages/Page2`.

Solicite la página Page2 del ejemplo en `localhost:5000/OtherPages/Page2` y revise los encabezados para ver el resultado:

![OtherPagesPage2Header se agrega a la respuesta de Page2.](razor-pages-conventions/_static/page2-filter-header.png)

**Configurar una fábrica de filtros**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) configura la fábrica especificada para aplicar [filtros](xref:mvc/controllers/filters) a todas las páginas de Razor.

En la aplicación de ejemplo se proporciona un ejemplo del uso de una [fábrica de filtros](xref:mvc/controllers/filters#ifilterfactory), para lo cual se agrega un encabezado (`FilterFactoryHeader`) con dos valores a las páginas de la aplicación:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*:

[!code-csharp[](razor-pages-conventions/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

Solicite la página About del ejemplo en `localhost:5000/About` y revise los encabezados para ver el resultado:

![Los encabezados de respuesta de la página About ponen de manifiesto que se han agregado dos encabezados FilterFactoryHeader.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a>Reemplazar el proveedor de modelo de aplicación de página predeterminado

Las páginas de Razor usan la interfaz `IPageApplicationModelProvider` para crear un [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider). Puede hacer uso del proveedor de modelo predeterminado para proporcionar su propia lógica de implementación de detección y procesamiento de controladores. La implementación predeterminada ([origen de referencia](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) establece convenciones para la nomenclatura de métodos de control *sin nombre* y *con nombre*, descrita más abajo.

**Métodos de control sin nombre predeterminados**

Los métodos de control de verbos HTTP (métodos de controlador "sin nombre") siguen la convención: `On<HTTP verb>[Async]` (anexar `Async` es opcional, pero recomendable en los métodos asincrónicos).

| Método de control sin nombre     | Operación                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | Inicializar el estado de la página     |
| `OnPost`/`OnPostAsync`     | Controlar las solicitudes POST          |
| `OnDelete`/`OnDeleteAsync` | Controlar las solicitudes DELETE&#8224; |
| `OnPut`/`OnPutAsync`       | Controlar las solicitudes PUT&#8224;    |
| `OnPatch`/`OnPatchAsync`   | Controlar las solicitudes PATCH&#8224;  |

&#8224;Usadas para realizar llamadas API a la página.

**Métodos de control con nombre predeterminados**

Los métodos de control proporcionados por el desarrollador (métodos de control "con nombre") siguen una convención similar. El nombre del método de control aparece tras el verbo HTTP o entre el verbo HTTP y `Async`: `On<HTTP verb><handler name>[Async]` (anexar `Async` es opcional, pero recomendable en los métodos asincrónicos). Por ejemplo, los métodos que procesan mensajes pueden asimilar la nomenclatura se muestra en la siguiente tabla.

| Ejemplo de método de control con nombre             | Operación de ejemplo        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | Obtener un mensaje        |
| `OnPostMessage`/`OnPostMessageAsync`     | Publicar un mensaje (POST)          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | Eliminar un mensaje (DELETE)&#8224; |
| `OnPutMessage`/`OnPutMessageAsync`       | Colocar un mensaje (PUT)&#8224;    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | Aplicar una revisión a un mensaje (PATCH)#8224;  |

&#8224;Usadas para realizar llamadas API a la página.

**Personalizar los nombres de los métodos de control**

Imaginemos que quiere cambiar la forma en que se denominan los métodos de control con y sin nombre. Un esquema de nomenclatura alternativo consiste en evitar que los nombres de método comiencen por "On" y usar el primer segmento de palabra para establecer el verbo HTTP. Puede realizar otros cambios, como convertir los verbos DELETE, PUT y PATCH en POST. Un esquema de este tipo proporcionaría los nombres de método mostrados en la siguiente tabla.

| Método de control                       | Operación                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | Inicializar el estado de la página     |
| `Post`/`PostAsync`                   | Controlar las solicitudes POST          |
| `Delete`/`DeleteAsync`               | Controlar las solicitudes DELETE&#8224; |
| `Put`/`PutAsync`                     | Controlar las solicitudes PUT&#8224;    |
| `Patch`/`PatchAsync`                 | Controlar las solicitudes PATCH&#8224;  |
| `GetMessage`                         | Obtener un mensaje              |
| `PostMessage`/`PostMessageAsync`     | Publicar un mensaje (POST)                |
| `DeleteMessage`/`DeleteMessageAsync` | Publicar (POST) un mensaje para eliminarlo      |
| `PutMessage`/`PutMessageAsync`       | Publicar (POST) un mensaje para colocarlo         |
| `PatchMessage`/`PatchMessageAsync`   | Publicar (POST) un mensaje para aplicarle una revisión       |

&#8224;Usadas para realizar llamadas API a la página.

Para establecer este esquema, haga uso de la clase `DefaultPageApplicationModelProvider` e invalide el método [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) para proporcionar lógica personalizada que resuelva nombres de controlador de [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel). En la aplicación de ejemplo se muestra cómo llevar esto a cabo en su propia clase `CustomPageApplicationModelProvider`:

[!code-csharp[](razor-pages-conventions/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

Esta clase de caracteriza por lo siguiente:

* Hereda de `DefaultPageApplicationModelProvider`.
* `TryParseHandlerMethod` procesa un método de control para averiguar el verbo HTTP (`httpMethod`) y el nombre del método de control con nombre (`handlerName`) al crear la `PageHandlerModel`.
  * Si lo hay, el postfijo `Async` se omite.
  * Se distingue entre mayúsculas y minúsculas al analizar el verbo HTTP desde el nombre del método.
  * Cuando el nombre del método (sin `Async`) es igual que el nombre del verbo HTTP, no hay ningún método de control con nombre. `handlerName` está establecido en `null` y el nombre del método es `Get`, `Post`, `Delete`, `Put` o `Patch`.
  * Cuando el nombre del método (sin `Async`) es más largo que el nombre del verbo HTTP, sí hay un método de control con nombre. La propiedad `handlerName` se establece como `<method name (less 'Async', if present)>`. Por ejemplo, tanto "GetMessage" como "GetMessageAsync" producen el nombre de método de control "GetMessage".
  * Los verbos HTTP DELETE, PUT y PATCH se convierten en POST.

Registre `CustomPageApplicationModelProvider` en la clase `Startup`:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet10)]

El modelo de página en *Index.cshtml.cs* muestra cómo se cambian las convenciones de nomenclatura de métodos de control habituales de las páginas de la aplicación. La nomenclatura con el prefijo "On" típicamente usado en páginas de Razor se quita. El método que inicializa el estado de página ahora se denomina `Get`. Esta convención, usada a lo largo de la aplicación, se puede ver si se abre un modelo de página de cualquiera de las páginas.

El resto de métodos comienzan por el verbo HTTP, que describe su procesamiento. Los dos métodos que empiezan por `Delete` se tratarían normalmente como verbos HTTP DELETE, pero la lógica de `TryParseHandlerMethod` establece explícitamente el verbo POST para ambos métodos de control.

Recuerde que `Async` es opcional entre `DeleteAllMessages` y `DeleteMessageAsync`. Ambos son métodos asincrónicos, pero puede optar entre usar el postfijo `Async` o no. Le recomendamos que lo haga. `DeleteAllMessages` se usa aquí con fines de demostración, pero se recomienda denominar este método `DeleteAllMessagesAsync`. Aunque no afecta al procesamiento de la implementación de ejemplo, usar el postfijo `Async` resalta el hecho de que es un método asincrónico.

[!code-csharp[](razor-pages-conventions/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

Fíjese en que los nombres de control proporcionados en *Index.cshtml* coinciden con los métodos de control `DeleteAllMessages` y `DeleteMessageAsync`:

[!code-cshtml[](razor-pages-conventions/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

`Async` en el nombre del método de control `DeleteMessageAsync` se descarta por medio de `TryParseHandlerMethod` para que coincida con la solicitud POST al método. El nombre `asp-page-handler` de `DeleteMessage` coincide con el método de control `DeleteMessageAsync`.

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>Filtros de MVC y filtro de página (IPageFilter)

Las páginas de Razor no tienen en cuenta los [filtros de acciones](xref:mvc/controllers/filters#action-filters) de MVC, porque en este tipo de páginas se usan métodos de control. Hay disponibles otros tipos de filtro de MVC que puede usar: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters) y [Result](xref:mvc/controllers/filters#result-filters). Para más información, vea el tema [Filters](xref:mvc/controllers/filters) (Filtros).

El filtro de página ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) es un filtro que se aplica a las páginas de Razor. Para más información, vea [Filter methods for Razor Pages](xref:razor-pages/filter) (Métodos de filtrado para páginas de Razor).

## <a name="additional-resources"></a>Recursos adicionales

* [Convenciones de autorización de las páginas de Razor](xref:security/authorization/razor-pages-authorization)
