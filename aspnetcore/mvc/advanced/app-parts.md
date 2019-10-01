---
title: Elementos de aplicación en ASP.NET Core
author: rick-anderson
description: Uso compartido de controladores, vistas, Razor Pages y mucho más con los elementos de aplicación en ASP.NET Core
ms.author: riande
ms.date: 05/14/2019
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 4b4c8c554a7045a180b56cf9998ab1a8496cde1b
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71207352"
---
# <a name="share-controllers-views-razor-pages-and-more-with-application-parts-in-aspnet-core"></a>Uso compartido de controladores, vistas, Razor Pages y mucho más con los elementos de aplicación en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([cómo descargarlo](xref:index#how-to-download-a-sample))

Un *elemento de aplicación* es una abstracción sobre los recursos de una aplicación. Los elementos de aplicación permiten a ASP.NET Core detectar controladores, componentes de vista, aplicaciones auxiliares de etiquetas, Razor Pages, orígenes de compilación de Razor, etc. [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) is un elemento de aplicación. `AssemblyPart` encapsula una referencia de ensamblado y expone los tipos y las referencias de compilación.

Los *proveedores de características* trabajan con los elementos de aplicación para rellenar las características de una aplicación de ASP.NET Core. El caso de uso principal de los elementos de aplicación es configurar una aplicación para detectar (o evitar cargar) características de ASP.NET Core de un ensamblado. Por ejemplo, puede que desee compartir la funcionalidad común entre varias aplicaciones. Mediante el uso de elementos de aplicación, puede compartir un ensamblado (DLL) que contenga controladores, vistas, Razor Pages, orígenes de compilación de Razor, aplicaciones auxiliares de etiquetas y mucho más con varias aplicaciones. Es preferible compartir un ensamblado a duplicar el código en varios proyectos.

Las aplicaciones ASP.NET Core cargan características de <xref:System.Web.WebPages.ApplicationPart>. La clase <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> representa un elemento de aplicación que está respaldado por un ensamblado.

## <a name="load-aspnet-core-features"></a>Carga de características de ASP.NET Core

Use las clases `ApplicationPart` y `AssemblyPart` para detectar y cargar características de ASP.NET Core (controladores, componentes de vista, etc.). <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> realiza un seguimiento de los elementos de aplicación y los proveedores de características disponibles. `ApplicationPartManager` se configura en `Startup.ConfigureServices`:

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

El código siguiente proporciona un enfoque alternativo para configurar `ApplicationPartManager` mediante `AssemblyPart`:

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

Los dos ejemplos de código anteriores cargan `SharedController` de un ensamblado. `SharedController` no está en el proyecto de la aplicación. Vea la descarga de ejemplo de la [solución WebAppParts](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts).

### <a name="include-views"></a>Inclusión de vistas

Para incluir vistas en el ensamblado:

* Agregue el marcado siguiente al archivo de proyecto compartido:

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* Agregue <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> a <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a>Impedimento de la carga de los recursos

Los elementos de aplicación se pueden usar para *evitar* la carga de recursos en un ensamblado o ubicación en concreto. Agregue o quite miembros de la colección <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> para ocultar los recursos o hacer que estos estén disponibles. El orden de las entradas de la colección `ApplicationParts` es irrelevante. Configure `ApplicationPartManager` antes de usarlo para configurar los servicios en el contenedor. Por ejemplo, configure `ApplicationPartManager` antes de invocar a `AddControllersAsServices`. Llame a `Remove` en la colección `ApplicationParts` para quitar un recurso.

En el código siguiente se usa <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> para quitar `MyDependentLibrary` de la aplicación: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]

`ApplicationPartManager` incluye elementos para:

* El ensamblado de la aplicación y los ensamblados dependientes.
* `Microsoft.AspNetCore.Mvc.TagHelpers`.
* `Microsoft.AspNetCore.Mvc.Razor`.

## <a name="application-feature-providers"></a>Proveedores de características de la aplicación

Los proveedores de características de la aplicación examinan los elementos de aplicación y proporcionan características para esos elementos. Existen proveedores de características integrados para las siguientes características de ASP.NET Core:

* [Controladores](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [Asistentes de etiquetas](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [Componentes de vista](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

Los proveedores de características heredan de <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, donde `T` es el tipo de la característica. Los proveedores de características se pueden implementar para cualquiera de los tipos de características enumerados anteriormente. El orden de los proveedores de características en `ApplicationPartManager.FeatureProviders` puede afectar al comportamiento en tiempo de ejecución. Los proveedores agregados posteriormente pueden reaccionar ante las acciones realizadas por los proveedores agregados anteriormente.

### <a name="generic-controller-feature"></a>característica de controlador genérico

ASP.NET Core omite los [controladores genéricos](/dotnet/csharp/programming-guide/generics/generic-classes). Un controlador genérico tiene un parámetro de tipo (por ejemplo, `MyController<T>`). En el ejemplo siguiente se agregan instancias de controlador genérico para una lista especificada de tipos:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

Los tipos se definen en `EntityTypes.Types`:

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

El proveedor de características se agrega en `Startup`:

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

Los nombres de controlador genérico utilizados para el enrutamiento tienen el formato *GenericController`1[Widget]* en lugar de *Widget*. El siguiente atributo modifica el nombre para coincidir con el tipo genérico usado por el controlador:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

La clase `GenericController`:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

Por ejemplo, si se solicita una dirección URL de `https://localhost:5001/Sprocket`, se obtiene la siguiente respuesta:

```text
Hello from a generic Sprocket controller.
```

### <a name="display-available-features"></a>muestra de las características disponibles

Las características disponibles para una aplicación se pueden enumerar solicitando `ApplicationPartManager` a través de la [inserción de dependencias](../../fundamentals/dependency-injection.md):

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

El [ejemplo de descarga](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) usa el código anterior para mostrar las características de la aplicación:

```text
Controllers:
    - FeaturesController
    - HomeController
    - HelloController
    - GenericController`1
    - GenericController`1
Tag Helpers:
    - PrerenderTagHelper
    - AnchorTagHelper
    - CacheTagHelper
    - DistributedCacheTagHelper
    - EnvironmentTagHelper
    - Additional Tag Helpers omitted for brevity.
View Components:
    - SampleViewComponent
```
