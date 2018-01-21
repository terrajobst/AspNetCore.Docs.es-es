---
title: "Partes de la aplicación de ASP.NET Core"
author: ardalis
description: "Obtenga información acerca de cómo utilizar componentes de la aplicación, que son abstrations sobre los recursos de una aplicación, para configurar la aplicación para detectar o evitar la carga de características desde un ensamblado."
ms.author: riande
manager: wpickett
ms.date: 01/04/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 12c34b7a9521835533998c5609870bc712a6d48c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="application-parts-in-aspnet-core"></a>Partes de la aplicación de ASP.NET Core

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

Un *parte de la aplicación* es una abstracción sobre los recursos de una aplicación, desde el que MVC características como controladores, componentes de la vista, o se pueden detectar aplicaciones auxiliares de etiquetas. Un ejemplo de una parte de la aplicación es un AssemblyPart, que encapsula una referencia de ensamblado y expone los tipos y las referencias de la compilación. *Proveedores de características* funcionan con los componentes de la aplicación para rellenar las características de una aplicación de MVC de ASP.NET Core. Es el caso de uso principal para las partes de la aplicación para que pueda configurar la aplicación para detectar (o si se evita la carga) características MVC desde un ensamblado.

## <a name="introducing-application-parts"></a>Introducción a partes de la aplicación

Aplicaciones MVC cargar sus características de [partes de la aplicación](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart). En concreto, el [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) clase representa una parte de la aplicación que está respaldada por un ensamblado. Puede utilizar estas clases para detectar y cargar las características MVC, tales como controladores, componentes de la vista, aplicaciones auxiliares de etiquetas y orígenes de compilación de razor. El [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) es responsable de seguimiento de los componentes de la aplicación y proveedores de características disponibles para la aplicación MVC. Puede interactuar con el `ApplicationPartManager` en `Startup` cuando configura MVC:

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => p.ApplicationParts.Add(part));
```

De forma predeterminada MVC buscará el árbol de dependencias y buscar controladores (incluso en otros ensamblados). Para cargar un ensamblado arbitrario (por ejemplo, desde un complemento que no se hace referencia en tiempo de compilación), puede utilizar una parte de la aplicación.

Puede utilizar elementos de aplicación para *evitar* buscando controladores en una ubicación o un ensamblado concreto. Puede controlar qué elementos (o ensamblados) están disponibles para la aplicación mediante la modificación de la `ApplicationParts` colección de la `ApplicationPartManager`. El orden de las entradas de la `ApplicationParts` colección no es importante. Es importante configurar totalmente el `ApplicationPartManager` antes de usarlo para configurar los servicios en el contenedor. Por ejemplo, debe configurar totalmente el `ApplicationPartManager` antes de invocar `AddControllersAsServices`. Si no lo hace así, significará que agregar controladores en partes de la aplicación después de que no se verán afectadas las llamadas de método (no obtener registrado como servicios) que podría dar lugar a bevavior incorrecta de la aplicación.

Si tiene un ensamblado que contiene los controladores que no desea que se utilicen, quítelo de la `ApplicationPartManager`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p =>
    {
        var dependentLibrary = p.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           p.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

Además de ensamblado del proyecto y sus ensamblados dependientes, la `ApplicationPartManager` incluirá partes de `Microsoft.AspNetCore.Mvc.TagHelpers` y `Microsoft.AspNetCore.Mvc.Razor` de forma predeterminada.

## <a name="application-feature-providers"></a>Proveedores de características de la aplicación

Proveedores de características de la aplicación examinar los componentes de la aplicación y proporciona características para las partes. Existen proveedores de características integradas para las siguientes características MVC:

* [Controladores](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [Referencia de metadatos](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [Aplicaciones auxiliares de etiquetas](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [Componentes de la vista](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

Proveedores de características que se heredan de `IApplicationFeatureProvider<T>`, donde `T` es el tipo de la característica. Puede implementar su propia característica proveedores para cualquiera de los tipos de características de MVC mencionados anteriormente. El orden de los proveedores de características en el `ApplicationPartManager.FeatureProviders` colección puede ser importante, puesto que proveedores posteriores pueden reaccionar ante las acciones realizadas por proveedores anteriores.

### <a name="sample-generic-controller-feature"></a>Ejemplo: Característica de controladora genérica

De forma predeterminada, ASP.NET Core MVC omite controladores genéricos (por ejemplo, `SomeController<T>`). Este ejemplo usa un proveedor de la característica de controlador que se ejecuta una vez que el proveedor predeterminado y agrega instancias de controlador genérico de una lista especificada de tipos (definido en `EntityTypes.Types`):

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

Los tipos de entidad:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

El proveedor de características se agrega en `Startup`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p => 
        p.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

De forma predeterminada, los nombres de controlador genérico utilizados para el enrutamiento sería del formulario *GenericController'1 [Widget]* en lugar de *Widget*. El siguiente atributo se usa para modificar el nombre que corresponda al tipo genérico usado por el controlador:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

La `GenericController` clase:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

El resultado, cuando se solicita una ruta coincidente:

![Ejemplo de salida de la aplicación de ejemplo que se lee, 'Hello desde un controlador de Sproket genérico'.](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a>Ejemplo: Características disponibles de pantalla

Puede iterar a través de las características rellenos disponibles para la aplicación solicitando una `ApplicationPartManager` a través de [inyección de dependencia](../../fundamentals/dependency-injection.md) y usarlo para rellenar las instancias de la característica adecuada:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

Resultado de ejemplo:

![Ejemplo de salida de la aplicación de ejemplo](app-parts/_static/available-features.png)
