---
title: Migrar de ASP.NET a ASP.NET Core 2.0
author: isaac2004
description: "Este documento de referencia proporciona orientación para migrar aplicaciones existentes de ASP.NET MVC o ASP.NET Web API a ASP.NET Core 2.0."
ms.author: scaddie
manager: wpickett
ms.date: 08/27/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/mvc2
ms.openlocfilehash: f9845449659960e82afd4f51d64084b7f55f68d4
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="migrating-from-aspnet-to-aspnet-core-20"></a><span data-ttu-id="a5b5e-103">Migrar de ASP.NET a ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="a5b5e-103">Migrating From ASP.NET to ASP.NET Core 2.0</span></span>

<span data-ttu-id="a5b5e-104">Por [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="a5b5e-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="a5b5e-105">Este artículo sirve de guía de referencia para migrar aplicaciones de ASP.NET a ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-105">This article serves as a reference guide for migrating ASP.NET applications to ASP.NET Core 2.0.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a5b5e-106">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="a5b5e-106">Prerequisites</span></span>

* <span data-ttu-id="a5b5e-107">[SDK de .NET Core 2.0.0](https://www.microsoft.com/net/core) o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="a5b5e-107">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

## <a name="target-frameworks"></a><span data-ttu-id="a5b5e-108">Versiones de .NET Framework de destino</span><span class="sxs-lookup"><span data-stu-id="a5b5e-108">Target Frameworks</span></span>
<span data-ttu-id="a5b5e-109">Los proyectos de ASP.NET Core 2.0 proporcionan a los desarrolladores la flexibilidad de usar la versión .NET Core, .NET Framework o ambas.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-109">ASP.NET Core 2.0 projects offer developers the flexibility of targeting .NET Core, .NET Framework, or both.</span></span> <span data-ttu-id="a5b5e-110">Vea [Selección entre .NET Core y .NET Framework para aplicaciones de servidor](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) para determinar qué plataforma de destino es más adecuada.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-110">See [Choosing between .NET Core and .NET Framework for server apps](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) to determine which target framework is most appropriate.</span></span>

<span data-ttu-id="a5b5e-111">Cuando se usa la versión .NET Framework, es necesario que los proyectos hagan referencia a paquetes de NuGet individuales.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-111">When targeting .NET Framework, projects need to reference individual NuGet packages.</span></span>

<span data-ttu-id="a5b5e-112">Cuando se usa .NET Core, se pueden eliminar numerosas referencias del paquete explícitas, gracias al [metapaquete](xref:fundamentals/metapackage) de ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-112">Targeting .NET Core allows you to eliminate numerous explicit package references, thanks to the ASP.NET Core 2.0 [metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="a5b5e-113">Instale el metapaquete `Microsoft.AspNetCore.All` en el proyecto:</span><span class="sxs-lookup"><span data-stu-id="a5b5e-113">Install the `Microsoft.AspNetCore.All` metapackage in your project:</span></span>

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
</ItemGroup>
```

<span data-ttu-id="a5b5e-114">Cuando se usa el metapaquete, con la aplicación no se implementa ningún paquete al que se hace referencia en el metapaquete.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-114">When the metapackage is used, no packages referenced in the metapackage are deployed with the app.</span></span> <span data-ttu-id="a5b5e-115">El almacén de tiempo de ejecución de .NET Core incluye estos activos que están compilados previamente para mejorar el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-115">The .NET Core Runtime Store includes these assets, and they are precompiled to improve performance.</span></span> <span data-ttu-id="a5b5e-116">Vea [Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x](xref:fundamentals/metapackage) (Metapaquete Microsoft.AspNetCore.All para ASP.NET Core 2.x) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-116">See [Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x](xref:fundamentals/metapackage) for more detail.</span></span>

## <a name="project-structure-differences"></a><span data-ttu-id="a5b5e-117">Diferencias en la estructura de proyecto</span><span class="sxs-lookup"><span data-stu-id="a5b5e-117">Project structure differences</span></span>
<span data-ttu-id="a5b5e-118">El formato de archivo *.csproj* se ha simplificado en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-118">The *.csproj* file format has been simplified in ASP.NET Core.</span></span> <span data-ttu-id="a5b5e-119">Estos son algunos cambios importantes:</span><span class="sxs-lookup"><span data-stu-id="a5b5e-119">Some notable changes include:</span></span>
- <span data-ttu-id="a5b5e-120">La inclusión explícita de archivos no es necesaria para que se consideren parte del proyecto.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-120">Explicit inclusion of files is not necessary for them to be considered part of the project.</span></span> <span data-ttu-id="a5b5e-121">Esto reduce el riesgo de conflictos al fusionar XML cuando se trabaja en equipos grandes.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-121">This reduces the risk of XML merge conflicts when working on large teams.</span></span>
- <span data-ttu-id="a5b5e-122">No hay referencias de GUID a otros proyectos, lo cual mejora la legibilidad del archivo.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-122">There are no GUID-based references to other projects, which improves file readability.</span></span>
- <span data-ttu-id="a5b5e-123">El archivo se puede editar sin descargarlo en Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="a5b5e-123">The file can be edited without unloading it in Visual Studio:</span></span>

    ![Edición de la opción de menú contextual CSPROJ en Visual Studio de 2017](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a><span data-ttu-id="a5b5e-125">Reemplazo del archivo Global.asax</span><span class="sxs-lookup"><span data-stu-id="a5b5e-125">Global.asax file replacement</span></span>
<span data-ttu-id="a5b5e-126">ASP.NET Core introdujo un nuevo mecanismo para arrancar una aplicación.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-126">ASP.NET Core introduced a new mechanism for bootstrapping an app.</span></span> <span data-ttu-id="a5b5e-127">El punto de entrada para las aplicaciones ASP.NET es el archivo *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-127">The entry point for ASP.NET applications is the *Global.asax* file.</span></span> <span data-ttu-id="a5b5e-128">En el archivo *Global.asax* se controlan tareas como la configuración de enrutamiento y los registros de filtro y de área.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-128">Tasks such as route configuration and filter and area registrations are handled in the *Global.asax* file.</span></span>

[!code-csharp[Main](samples/globalasax-sample.cs)]

<span data-ttu-id="a5b5e-129">Este enfoque acopla la aplicación y el servidor en el que está implementada de forma que interfiere con la implementación.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-129">This approach couples the application and the server to which it's deployed in a way that interferes with the implementation.</span></span> <span data-ttu-id="a5b5e-130">En un esfuerzo por desacoplar, se introdujo [OWIN](http://owin.org/) para ofrecer una forma más limpia de usar varios marcos de trabajo de manera conjunta.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-130">In an effort to decouple, [OWIN](http://owin.org/) was introduced to provide a cleaner way to use multiple frameworks together.</span></span> <span data-ttu-id="a5b5e-131">OWIN proporciona una canalización para agregar solo los módulos necesarios.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-131">OWIN provides a pipeline to add only the modules needed.</span></span> <span data-ttu-id="a5b5e-132">El entorno de hospedaje toma una función de [inicio](xref:fundamentals/startup) para configurar servicios y la canalización de solicitud de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-132">The hosting environment takes a [Startup](xref:fundamentals/startup) function to configure services and the app's request pipeline.</span></span> <span data-ttu-id="a5b5e-133">`Startup` registra un conjunto de middleware en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-133">`Startup` registers a set of middleware with the application.</span></span> <span data-ttu-id="a5b5e-134">Para cada solicitud, la aplicación llama a cada uno de los componentes de middleware con el puntero principal de una lista vinculada a un conjunto de controladores existente.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-134">For each request, the application calls each of the the middleware components with the head pointer of a linked list to an existing set of handlers.</span></span> <span data-ttu-id="a5b5e-135">Cada componente de middleware puede agregar uno o varios controladores a la canalización de control de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-135">Each middleware component can add one or more handlers to the request handling pipeline.</span></span> <span data-ttu-id="a5b5e-136">Esto se consigue mediante la devolución de una referencia al controlador que ahora es el primero de la lista.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-136">This is accomplished by returning a reference to the handler that is the new head of the list.</span></span> <span data-ttu-id="a5b5e-137">Cada controlador se encarga de recordar e invocar el controlador siguiente en la lista.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-137">Each handler is responsible for remembering and invoking the next handler in the list.</span></span> <span data-ttu-id="a5b5e-138">Con ASP.NET Core, el punto de entrada a una aplicación es `Startup` y ya no se tiene dependencia de *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-138">With ASP.NET Core, the entry point to an application is `Startup`, and you no longer have a dependency on *Global.asax*.</span></span> <span data-ttu-id="a5b5e-139">Cuando utilice OWIN con .NET Framework, use algo parecido a lo siguiente como canalización:</span><span class="sxs-lookup"><span data-stu-id="a5b5e-139">When using OWIN with .NET Framework, use something like the following as a pipeline:</span></span>

[!code-csharp[Main](samples/webapi-owin.cs)]

<span data-ttu-id="a5b5e-140">Esto configura las rutas predeterminadas y tiene como valor predeterminado XmlSerialization a través de Json.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-140">This configures your default routes, and defaults to XmlSerialization over Json.</span></span> <span data-ttu-id="a5b5e-141">Agregue otro middleware a esta canalización según sea necesario (carga de servicios, opciones de configuración, archivos estáticos, etcétera).</span><span class="sxs-lookup"><span data-stu-id="a5b5e-141">Add other Middleware to this pipeline as needed (loading services, configuration settings, static files, etc.).</span></span>

<span data-ttu-id="a5b5e-142">ASP.NET Core usa un enfoque similar, pero no depende de OWIN para controlar la entrada.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-142">ASP.NET Core uses a similar approach, but doesn't rely on OWIN to handle the entry.</span></span> <span data-ttu-id="a5b5e-143">En lugar de eso, usa el método *Program.cs* `Main` (similar a las aplicaciones de consola) y `Startup` se carga a través de ahí.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-143">Instead, that is done through the *Program.cs* `Main` method (similar to console applications) and `Startup` is loaded through there.</span></span>

[!code-csharp[Main](samples/program.cs)]

<span data-ttu-id="a5b5e-144">`Startup` debe incluir un método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-144">`Startup` must include a `Configure` method.</span></span> <span data-ttu-id="a5b5e-145">En `Configure`, agregue el middleware necesario a la canalización.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-145">In `Configure`, add the necessary middleware to the pipeline.</span></span> <span data-ttu-id="a5b5e-146">En el ejemplo siguiente (de la plantilla de sitio web predeterminada), se usan varios métodos de extensión para configurar la canalización con compatibilidad para:</span><span class="sxs-lookup"><span data-stu-id="a5b5e-146">In the following example (from the default web site template), several extension methods are used to configure the pipeline with support for:</span></span>

* [<span data-ttu-id="a5b5e-147">BrowserLink</span><span class="sxs-lookup"><span data-stu-id="a5b5e-147">BrowserLink</span></span>](http://vswebessentials.com/features/browserlink)
* <span data-ttu-id="a5b5e-148">Páginas de error</span><span class="sxs-lookup"><span data-stu-id="a5b5e-148">Error pages</span></span>
* <span data-ttu-id="a5b5e-149">Archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="a5b5e-149">Static files</span></span>
* <span data-ttu-id="a5b5e-150">ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="a5b5e-150">ASP.NET Core MVC</span></span>
* <span data-ttu-id="a5b5e-151">identidad</span><span class="sxs-lookup"><span data-stu-id="a5b5e-151">Identity</span></span>

[!code-csharp[Main](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

<span data-ttu-id="a5b5e-152">El host y la aplicación se han desacoplado, lo que proporciona la flexibilidad de pasar a una plataforma diferente en el futuro.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-152">The host and application have been decoupled, which provides the flexibility of moving to a different platform in the future.</span></span>

<span data-ttu-id="a5b5e-153">**Nota:** Para una referencia más detallada sobre el inicio de ASP.NET Core y middleware en [Startup in ASP.NET Core](xref:fundamentals/startup) (Inicio de aplicaciones en ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="a5b5e-153">**Note:** For a more in-depth reference to ASP.NET Core Startup and Middleware, see [Startup in ASP.NET Core](xref:fundamentals/startup)</span></span>

## <a name="storing-configurations"></a><span data-ttu-id="a5b5e-154">Almacenamiento de configuraciones</span><span class="sxs-lookup"><span data-stu-id="a5b5e-154">Storing Configurations</span></span>
<span data-ttu-id="a5b5e-155">ASP.NET admite el almacenamiento de valores de configuración.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-155">ASP.NET supports storing settings.</span></span> <span data-ttu-id="a5b5e-156">Estos valores de configuración se usan, por ejemplo, para admitir el entorno donde se implementan las aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-156">These setting are used, for example, to support the environment to which the applications were deployed.</span></span> <span data-ttu-id="a5b5e-157">Antes se solían almacenar todos los pares de clave y valor personalizados en la sección `<appSettings>` del archivo *Web.config*:</span><span class="sxs-lookup"><span data-stu-id="a5b5e-157">A common practice was to store all custom key-value pairs in the `<appSettings>` section of the *Web.config* file:</span></span>

[!code-xml[Main](samples/webconfig-sample.xml)]

<span data-ttu-id="a5b5e-158">Las aplicaciones leen esta configuración mediante la colección `ConfigurationManager.AppSettings` en el espacio de nombres `System.Configuration`:</span><span class="sxs-lookup"><span data-stu-id="a5b5e-158">Applications read these settings using the `ConfigurationManager.AppSettings` collection in the `System.Configuration` namespace:</span></span>

[!code-csharp[Main](samples/read-webconfig.cs)]

<span data-ttu-id="a5b5e-159">ASP.NET Core puede almacenar datos de configuración para la aplicación en cualquier archivo y cargarlos como parte del arranque de middleware.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-159">ASP.NET Core can store configuration data for the application in any file and load them as part of middleware bootstrapping.</span></span> <span data-ttu-id="a5b5e-160">El archivo predeterminado que se usa en las plantillas de proyecto es *appSettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="a5b5e-160">The default file used in the project templates is *appsettings.json*:</span></span>

[!code-json[Main](samples/appsettings-sample.json)]

<span data-ttu-id="a5b5e-161">La carga de este archivo en una instancia de `IConfiguration` dentro de la aplicación se realiza en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a5b5e-161">Loading this file into an instance of `IConfiguration` inside your application is done in *Startup.cs*:</span></span>

[!code-csharp[Main](samples/startup-builder.cs)]

<span data-ttu-id="a5b5e-162">La aplicación lee de `Configuration` para obtener la configuración:</span><span class="sxs-lookup"><span data-stu-id="a5b5e-162">The app reads from `Configuration` to get the settings:</span></span>

[!code-csharp[Main](samples/read-appsettings.cs)]

<span data-ttu-id="a5b5e-163">Existen extensiones de este enfoque para lograr que el proceso sea más sólido, como el uso de [inserción de dependencias](xref:fundamentals/dependency-injection) para cargar un servicio con estos valores.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-163">There are extensions to this approach to make the process more robust, such as using [Dependency Injection](xref:fundamentals/dependency-injection) (DI) to load a service with these values.</span></span> <span data-ttu-id="a5b5e-164">El enfoque de la inserción de dependencias proporciona un conjunto fuertemente tipado de objetos de configuración.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-164">The DI approach provides a strongly-typed set of configuration objects.</span></span>

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

<span data-ttu-id="a5b5e-165">**Nota:** Para una referencia más detallada sobre la configuración de ASP.NET Core, vea [Configuration in ASP.NET Core](xref:fundamentals/configuration/index) (Configuración en ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="a5b5e-165">**Note:** For a more in-depth reference to ASP.NET Core configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

## <a name="native-dependency-injection"></a><span data-ttu-id="a5b5e-166">Inserción de dependencias nativa</span><span class="sxs-lookup"><span data-stu-id="a5b5e-166">Native Dependency Injection</span></span>
<span data-ttu-id="a5b5e-167">Un objetivo importante al compilar aplicaciones grandes y escalables es lograr el acoplamiento flexible de los componentes y los servicios.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-167">An important goal when building large, scalable applications is the loose coupling of components and services.</span></span> <span data-ttu-id="a5b5e-168">La [inserción de dependencias](xref:fundamentals/dependency-injection) es una técnica popular para lograrlo y se trata de un componente nativo de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-168">[Dependency Injection](xref:fundamentals/dependency-injection) is a popular technique for achieving this, and it is a native component of ASP.NET Core.</span></span>

<span data-ttu-id="a5b5e-169">En las aplicaciones ASP.NET, los desarrolladores se basan en una biblioteca de terceros para implementar la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-169">In ASP.NET applications, developers rely on a third-party library to implement Dependency Injection.</span></span> <span data-ttu-id="a5b5e-170">Una de estas bibliotecas es [Unity](https://github.com/unitycontainer/unity), suministrada por Microsoft Patterns & Practices.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-170">One such library is [Unity](https://github.com/unitycontainer/unity), provided by Microsoft Patterns & Practices.</span></span> 

<span data-ttu-id="a5b5e-171">Un ejemplo de configuración de la inserción de dependencias con Unity consiste en implementar `IDependencyResolver` que encapsula un `UnityContainer`:</span><span class="sxs-lookup"><span data-stu-id="a5b5e-171">An example of setting up Dependency Injection with Unity is implementing `IDependencyResolver` that wraps a `UnityContainer`:</span></span>

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]

<span data-ttu-id="a5b5e-172">Cree una instancia de `UnityContainer`, registre el servicio y establezca la resolución de dependencias de `HttpConfiguration` en la nueva instancia de `UnityResolver` para el contenedor:</span><span class="sxs-lookup"><span data-stu-id="a5b5e-172">Create an instance of your `UnityContainer`, register your service, and set the dependency resolver of `HttpConfiguration` to the new instance of `UnityResolver` for your container:</span></span>

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]

<span data-ttu-id="a5b5e-173">Inserte `IProductRepository` cuando sea necesario:</span><span class="sxs-lookup"><span data-stu-id="a5b5e-173">Inject `IProductRepository` where needed:</span></span>

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]

<span data-ttu-id="a5b5e-174">Dado que la inserción de dependencia forma parte de ASP.NET Core, puede agregar el servicio en el método `ConfigureServices` de *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a5b5e-174">Because Dependency Injection is part of ASP.NET Core, you can add your service in the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[Main](samples/configure-services.cs)]

<span data-ttu-id="a5b5e-175">El repositorio se puede insertar en cualquier lugar, como ocurría con Unity.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-175">The repository can be injected anywhere, as was true with Unity.</span></span>

<span data-ttu-id="a5b5e-176">**Nota:** Para una referencia detallada sobre la inserción de dependencias en ASP.NET Core, vea [Dependency Injection in ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container) (Inserción de dependencias en ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="a5b5e-176">**Note:** For an in-depth reference to dependency injection in ASP.NET Core, see [Dependency Injection in ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)</span></span>

## <a name="serving-static-files"></a><span data-ttu-id="a5b5e-177">Trabajar con archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="a5b5e-177">Serving Static Files</span></span>
<span data-ttu-id="a5b5e-178">Una parte importante del desarrollo web es la capacidad de trabajar con activos estáticos de cliente.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-178">An important part of web development is the ability to serve static, client-side assets.</span></span> <span data-ttu-id="a5b5e-179">Los ejemplos más comunes de archivos estáticos son HTML, CSS, JavaScript e imágenes.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-179">The most common examples of static files are HTML, CSS, Javascript, and images.</span></span> <span data-ttu-id="a5b5e-180">Estos archivos deben guardarse en la ubicación de publicación de la aplicación (o la red de entrega de contenido) y es necesario hacer referencia a ellos para que una solicitud los pueda cargar.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-180">These files need to be saved in the published location of the app (or CDN) and referenced so they can be loaded by a request.</span></span> <span data-ttu-id="a5b5e-181">Este proceso ha cambiado en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-181">This process has changed in ASP.NET Core.</span></span>

<span data-ttu-id="a5b5e-182">En ASP.NET, los archivos estáticos se almacenan en directorios distintos y se hace referencia a ellos en las vistas.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-182">In ASP.NET, static files are stored in various directories and referenced in the views.</span></span>

<span data-ttu-id="a5b5e-183">En ASP.NET Core, los archivos estáticos se almacenan en la "raíz web" (*&lt;raíz del contenido&gt;/wwwroot*), a menos que se configuren de otra manera.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-183">In ASP.NET Core, static files are stored in the "web root" (*&lt;content root&gt;/wwwroot*), unless configured otherwise.</span></span> <span data-ttu-id="a5b5e-184">Los archivos se cargan en la canalización de solicitud mediante la invocación del método de extensión `UseStaticFiles` desde `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="a5b5e-184">The files are loaded into the request pipeline by invoking the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

[!code-csharp[Main](../../fundamentals/static-files/samples/1x/StartupStaticFiles.cs?highlight=3&name=snippet_ConfigureMethod)]

<span data-ttu-id="a5b5e-185">**Nota:** Si el destino es .NET Framework, debe instalarse el paquete de NuGet `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-185">**Note:** If targeting .NET Framework, install the NuGet package `Microsoft.AspNetCore.StaticFiles`.</span></span>

<span data-ttu-id="a5b5e-186">Por ejemplo, el explorador puede acceder a un recurso de imagen en la carpeta *wwwroot/images* en una ubicación como `http://<app>/images/<imageFileName>`.</span><span class="sxs-lookup"><span data-stu-id="a5b5e-186">For example, an image asset in the *wwwroot/images* folder is accessible to the browser at a location such as `http://<app>/images/<imageFileName>`.</span></span>

<span data-ttu-id="a5b5e-187">**Nota:** Para una referencia más detallada sobre cómo trabajar con archivos estáticos en ASP.NET Core, vea [Introduction to working with static files in ASP.NET Core](xref:fundamentals/static-files) (Introducción sobre cómo trabajar con archivos estáticos en ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="a5b5e-187">**Note:** For a more in-depth reference to serving static files in ASP.NET Core, see [Introduction to working with static files in ASP.NET Core](xref:fundamentals/static-files).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a5b5e-188">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="a5b5e-188">Additional Resources</span></span>
* [<span data-ttu-id="a5b5e-189">Traslado a .NET Core: bibliotecas</span><span class="sxs-lookup"><span data-stu-id="a5b5e-189">Porting Libraries to .NET Core</span></span>](https://docs.microsoft.com/dotnet/core/porting/libraries)
