---
title: Elementos de aplicación en ASP.NET Core
author: ardalis
description: Obtenga información sobre cómo usar elementos de aplicación (que son abstracciones de los recursos de una aplicación) para detectar o evitar la carga de características desde un ensamblado.
ms.author: riande
ms.date: 01/04/2017
uid: mvc/extensibility/app-parts
ms.openlocfilehash: c0d3ad6bcdf2e56df915b176b28759c59e76faf6
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206568"
---
# <a name="application-parts-in-aspnet-core"></a>Elementos de aplicación en ASP.NET Core

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))

Un *elemento de aplicación* es una abstracción sobre los recursos de una aplicación desde el que se pueden detectar características de MVC como controladores, componentes de vista o asistentes de etiquetas. Un ejemplo de un elemento de aplicación es AssemblyPart, que encapsula una referencia de ensamblado y expone los tipos y las referencias de la compilación. Los *proveedores de características* trabajan con los elementos de aplicación para rellenar las características de una aplicación de ASP.NET Core MVC. El uso principal de los elementos de aplicación es permitir la configuración de la aplicación para detectar características MVC (o evitar su carga) de un ensamblado.

## <a name="introducing-application-parts"></a>Introducción a los elementos de aplicación

Las aplicaciones MVC cargan sus características desde [elementos de aplicación](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart). En particular, la clase [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) representa un elemento de aplicación que está respaldado por un ensamblado. Puede utilizar estas clases para detectar y cargar las características MVC, tales como controladores, componentes de vista, asistentes de etiquetas y orígenes de compilación de Razor. [ApplicationPartManager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) es responsable del seguimiento de los elementos de aplicación y los proveedores de características disponibles para la aplicación MVC. Puede interactuar con `ApplicationPartManager` en `Startup` cuando configura MVC:

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => apm.ApplicationParts.Add(part));
```

De forma predeterminada, MVC examinará el árbol de dependencias y buscará controladores (incluso en otros ensamblados). Para cargar un ensamblado arbitrario (por ejemplo, desde un complemento al que no se hace referencia en tiempo de compilación), puede utilizar un elemento de aplicación.

Los elementos de aplicación se pueden usar para *evitar* tener que buscar controladores en una determinada ubicación o ensamblado. Modifique la colección `ApplicationParts` de `ApplicationPartManager` para controlar qué elementos (o ensamblados) están disponibles para la aplicación. El orden de las entradas de la colección `ApplicationParts` es irrelevante. Es importante configurar totalmente `ApplicationPartManager` antes de usarlo para configurar los servicios en el contenedor. Por ejemplo, debe configurar totalmente `ApplicationPartManager` antes de invocar a `AddControllersAsServices`. Si no lo hace, significará que los controladores de los elementos de aplicación que se han agregado después de esa llamada al método no se verán afectados (no se registrarán como servicios), lo que podría dar lugar a un comportamiento incorrecto de la aplicación.

Si tiene un ensamblado que contiene controladores que no quiere usar, quítelo de `ApplicationPartManager`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm =>
    {
        var dependentLibrary = apm.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           apm.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

Además del ensamblado del proyecto y sus ensamblados dependientes, `ApplicationPartManager` incluirá elementos de `Microsoft.AspNetCore.Mvc.TagHelpers` y `Microsoft.AspNetCore.Mvc.Razor` de forma predeterminada.

## <a name="application-feature-providers"></a>Proveedores de características de la aplicación

Los proveedores de características de la aplicación examinan los elementos de aplicación y proporcionan características para esos elementos. Existen proveedores de características integrados para las siguientes características MVC:

* [Controladores](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [Referencia de los metadatos](/dotnet/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [Asistentes de etiquetas](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [Componentes de vista](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

Los proveedores de características heredan de `IApplicationFeatureProvider<T>`, donde `T` es el tipo de la característica. Puede implementar sus propios proveedores de características para cualquiera de los tipos de características de MVC mencionados anteriormente. El orden de los proveedores de características en la colección `ApplicationPartManager.FeatureProviders` puede ser importante, puesto que los proveedores posteriores pueden reaccionar ante las acciones realizadas por los proveedores anteriores.

### <a name="sample-generic-controller-feature"></a>Ejemplo: característica de controlador genérico

De forma predeterminada, ASP.NET Core MVC omite los controladores genéricos (por ejemplo, `SomeController<T>`). En este ejemplo se usa un proveedor de características de controlador que se ejecuta después del proveedor predeterminado y agrega instancias de controlador genérico para una lista especificada de tipos (definida en `EntityTypes.Types`):

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

Tipos de entidad:

[!code-csharp[](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

El proveedor de características se agrega en `Startup`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm => 
        apm.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

De forma predeterminada, los nombres de controlador genérico utilizados para el enrutamiento tendrían el formato *GenericController`1[Widget]* en lugar de *Widget*. El siguiente atributo se usa para modificar el nombre para coincidir con el tipo genérico usado por el controlador:

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

La clase `GenericController`:

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

Este es el resultado cuando se solicita una ruta coincidente:

![En la salida de la aplicación de ejemplo se lee "Hello from a generic Sproket controller".](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a>Ejemplo: mostrar las características disponibles

Para iterar a través de las características rellenadas disponibles para la aplicación, puede solicitar un administrador `ApplicationPartManager` mediante [inserción de dependencias](../../fundamentals/dependency-injection.md) y usarlo para rellenar las instancias de las características adecuadas:

[!code-csharp[](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

Salida del ejemplo:

![Ejemplo de salida de la aplicación de ejemplo](app-parts/_static/available-features.png)
