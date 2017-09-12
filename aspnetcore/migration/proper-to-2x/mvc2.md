---
title: Migrar de ASP.NET a ASP.NET Core 2.0
author: isaac2004
description: "Este documento de referencia proporciona orientación para migrar aplicaciones existentes de ASP.NET MVC o Web API principales de ASP.NET 2.0."
keywords: "Núcleo de ASP.NET, MVC, migrar"
ms.author: scaddie
manager: wpickett
ms.date: 08/27/2017
ms.topic: article
ms.assetid: 3155cc9e-d0c9-424b-886c-35c0ec6f9f4e
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/mvc2
ms.openlocfilehash: e0691b276b63ee12d3163ac48d1392696fb97aa6
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="migrating-from-aspnet-to-aspnet-core-20"></a><span data-ttu-id="b3c15-104">Migrar de ASP.NET a ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="b3c15-104">Migrating From ASP.NET to ASP.NET Core 2.0</span></span>

<span data-ttu-id="b3c15-105">Por [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="b3c15-105">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="b3c15-106">Este artículo sirve como una guía de referencia para migrar aplicaciones de ASP.NET a ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="b3c15-106">This article serves as a reference guide for migrating ASP.NET applications to ASP.NET Core 2.0.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b3c15-107">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="b3c15-107">Prerequisites</span></span>

* <span data-ttu-id="b3c15-108">[.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) o una versión posterior.</span><span class="sxs-lookup"><span data-stu-id="b3c15-108">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

## <a name="target-frameworks"></a><span data-ttu-id="b3c15-109">Versiones de .NET Framework de destino</span><span class="sxs-lookup"><span data-stu-id="b3c15-109">Target Frameworks</span></span>
<span data-ttu-id="b3c15-110">Proyectos de ASP.NET Core 2.0 proporcionan a los desarrolladores la flexibilidad de destino es .NET Core, .NET Framework o ambos.</span><span class="sxs-lookup"><span data-stu-id="b3c15-110">ASP.NET Core 2.0 projects offer developers the flexibility of targeting .NET Core, .NET Framework, or both.</span></span> <span data-ttu-id="b3c15-111">Vea [elegir entre .NET Core y .NET Framework para las aplicaciones de servidor](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) para determinar qué plataforma de destino es más adecuado.</span><span class="sxs-lookup"><span data-stu-id="b3c15-111">See [Choosing between .NET Core and .NET Framework for server apps](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) to determine which target framework is most appropriate.</span></span>

<span data-ttu-id="b3c15-112">Cuando el destino es .NET Framework, proyectos necesitan hacer referencia a paquetes de NuGet individuales.</span><span class="sxs-lookup"><span data-stu-id="b3c15-112">When targeting .NET Framework, projects need to reference individual NuGet packages.</span></span>

<span data-ttu-id="b3c15-113">Como destino .NET Core le permite eliminar numerosas referencias del paquete explícita, gracias a la versión 2.0 de ASP.NET Core [metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="b3c15-113">Targeting .NET Core allows you to eliminate numerous explicit package references, thanks to the ASP.NET Core 2.0 [metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="b3c15-114">Instalar el `Microsoft.AspNetCore.All` metapackage en el proyecto:</span><span class="sxs-lookup"><span data-stu-id="b3c15-114">Install the `Microsoft.AspNetCore.All` metapackage in your project:</span></span>

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
</ItemGroup>
```

<span data-ttu-id="b3c15-115">Cuando se utiliza el metapackage, ningún paquete que se hace referencia en el metapackage se implementa con la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b3c15-115">When the metapackage is used, no packages referenced in the metapackage are deployed with the app.</span></span> <span data-ttu-id="b3c15-116">El almacén de tiempo de ejecución de .NET Core incluye estos activos, y se precompilan para mejorar el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="b3c15-116">The .NET Core Runtime Store includes these assets, and they are precompiled to improve performance.</span></span> <span data-ttu-id="b3c15-117">Vea [Microsoft.AspNetCore.All metapackage para ASP.NET Core 2.x](xref:fundamentals/metapackage) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="b3c15-117">See [Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x](xref:fundamentals/metapackage) for more detail.</span></span>

## <a name="project-structure-differences"></a><span data-ttu-id="b3c15-118">Diferencias de la estructura de proyecto</span><span class="sxs-lookup"><span data-stu-id="b3c15-118">Project structure differences</span></span>
<span data-ttu-id="b3c15-119">El *.csproj* formato de archivo se ha simplificado en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b3c15-119">The *.csproj* file format has been simplified in ASP.NET Core.</span></span> <span data-ttu-id="b3c15-120">Algunos cambios importantes son:</span><span class="sxs-lookup"><span data-stu-id="b3c15-120">Some notable changes include:</span></span>
- <span data-ttu-id="b3c15-121">Inclusión explícita de archivos no es necesario para que se consideran parte del proyecto.</span><span class="sxs-lookup"><span data-stu-id="b3c15-121">Explicit inclusion of files is not necessary for them to be considered part of the project.</span></span> <span data-ttu-id="b3c15-122">Esto reduce el riesgo de conflictos de combinación XML cuando se trabaja en equipos grandes.</span><span class="sxs-lookup"><span data-stu-id="b3c15-122">This reduces the risk of XML merge conflicts when working on large teams.</span></span>
- <span data-ttu-id="b3c15-123">No quedan referencias basada en GUID a otros proyectos, lo cual mejora la legibilidad del archivo.</span><span class="sxs-lookup"><span data-stu-id="b3c15-123">There are no GUID-based references to other projects, which improves file readability.</span></span>
- <span data-ttu-id="b3c15-124">El archivo se puede modificar sin descargarlo en Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="b3c15-124">The file can be edited without unloading it in Visual Studio:</span></span>

    ![Editar la opción de menú contextual CSPROJ en Visual Studio de 2017](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a><span data-ttu-id="b3c15-126">Reemplazar el archivo global.asax</span><span class="sxs-lookup"><span data-stu-id="b3c15-126">Global.asax file replacement</span></span>
<span data-ttu-id="b3c15-127">ASP.NET Core introdujo un nuevo mecanismo para arrancar una aplicación.</span><span class="sxs-lookup"><span data-stu-id="b3c15-127">ASP.NET Core introduced a new mechanism for bootstrapping an app.</span></span> <span data-ttu-id="b3c15-128">El punto de entrada para las aplicaciones ASP.NET es la *Global.asax* archivo.</span><span class="sxs-lookup"><span data-stu-id="b3c15-128">The entry point for ASP.NET applications is the *Global.asax* file.</span></span> <span data-ttu-id="b3c15-129">Las tareas como la configuración de enrutamiento y registros de filtro y de área se controlan en el *Global.asax* archivo.</span><span class="sxs-lookup"><span data-stu-id="b3c15-129">Tasks such as route configuration and filter and area registrations are handled in the *Global.asax* file.</span></span>

<span data-ttu-id="b3c15-130">[!code-csharp[Main](samples/globalasax-sample.cs)]</span><span class="sxs-lookup"><span data-stu-id="b3c15-130">[!code-csharp[Main](samples/globalasax-sample.cs)]</span></span>

<span data-ttu-id="b3c15-131">Este enfoque se acopla con la aplicación y el servidor en el que se ha implementado de forma que interfiere con la implementación.</span><span class="sxs-lookup"><span data-stu-id="b3c15-131">This approach couples the application and the server to which it's deployed in a way that interferes with the implementation.</span></span> <span data-ttu-id="b3c15-132">En un esfuerzo por desacoplar, [OWIN](http://owin.org/) se introdujo para proporcionar una forma más limpia usar conjuntamente varios marcos de trabajo.</span><span class="sxs-lookup"><span data-stu-id="b3c15-132">In an effort to decouple, [OWIN](http://owin.org/) was introduced to provide a cleaner way to use multiple frameworks together.</span></span> <span data-ttu-id="b3c15-133">OWIN proporciona una canalización para agregar solo los módulos necesarios.</span><span class="sxs-lookup"><span data-stu-id="b3c15-133">OWIN provides a pipeline to add only the modules needed.</span></span> <span data-ttu-id="b3c15-134">El entorno de hospedaje toma un [inicio](xref:fundamentals/startup) función para configurar servicios y la canalización de solicitud de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b3c15-134">The hosting environment takes a [Startup](xref:fundamentals/startup) function to configure services and the app's request pipeline.</span></span> <span data-ttu-id="b3c15-135">`Startup`registra un conjunto de middleware en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b3c15-135">`Startup` registers a set of middleware with the application.</span></span> <span data-ttu-id="b3c15-136">Para cada solicitud, la aplicación llama a cada uno de los los componentes de middleware con el puntero principal de una lista vinculada a un conjunto de controladores existente.</span><span class="sxs-lookup"><span data-stu-id="b3c15-136">For each request, the application calls each of the the middleware components with the head pointer of a linked list to an existing set of handlers.</span></span> <span data-ttu-id="b3c15-137">Cada componente de middleware puede agregar uno o varios controladores a la solicitud de control de canalización.</span><span class="sxs-lookup"><span data-stu-id="b3c15-137">Each middleware component can add one or more handlers to the request handling pipeline.</span></span> <span data-ttu-id="b3c15-138">Esto se consigue mediante la devolución de una referencia al controlador que es el nuevo encabezado de la lista.</span><span class="sxs-lookup"><span data-stu-id="b3c15-138">This is accomplished by returning a reference to the handler that is the new head of the list.</span></span> <span data-ttu-id="b3c15-139">Cada controlador es responsable de recordar e invocar el controlador siguiente en la lista.</span><span class="sxs-lookup"><span data-stu-id="b3c15-139">Each handler is responsible for remembering and invoking the next handler in the list.</span></span> <span data-ttu-id="b3c15-140">Con ASP.NET Core, es el punto de entrada a una aplicación `Startup`, y ya no tienen una dependencia en *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="b3c15-140">With ASP.NET Core, the entry point to an application is `Startup`, and you no longer have a dependency on *Global.asax*.</span></span> <span data-ttu-id="b3c15-141">Cuando use OWIN con .NET Framework, usar algo parecido a lo siguiente como una canalización:</span><span class="sxs-lookup"><span data-stu-id="b3c15-141">When using OWIN with .NET Framework, use something like the following as a pipeline:</span></span>

<span data-ttu-id="b3c15-142">[!code-csharp[Main](samples/webapi-owin.cs)]</span><span class="sxs-lookup"><span data-stu-id="b3c15-142">[!code-csharp[Main](samples/webapi-owin.cs)]</span></span>

<span data-ttu-id="b3c15-143">Esto configura las rutas predeterminadas y tiene como valor predeterminado XmlSerialization en Json.</span><span class="sxs-lookup"><span data-stu-id="b3c15-143">This configures your default routes, and defaults to XmlSerialization over Json.</span></span> <span data-ttu-id="b3c15-144">Agregue otro Middleware para esta canalización según sea necesario (cargar servicios, opciones de configuración, archivos estáticos, etcetera.).</span><span class="sxs-lookup"><span data-stu-id="b3c15-144">Add other Middleware to this pipeline as needed (loading services, configuration settings, static files, etc.).</span></span>

<span data-ttu-id="b3c15-145">ASP.NET Core usa un enfoque similar, pero no depende de OWIN para controlar la entrada.</span><span class="sxs-lookup"><span data-stu-id="b3c15-145">ASP.NET Core uses a similar approach, but doesn't rely on OWIN to handle the entry.</span></span> <span data-ttu-id="b3c15-146">En su lugar, que se realiza a través de la *Program.cs* `Main` (método) (similar a las aplicaciones de consola) y `Startup` se carga a través de allí.</span><span class="sxs-lookup"><span data-stu-id="b3c15-146">Instead, that is done through the *Program.cs* `Main` method (similar to console applications) and `Startup` is loaded through there.</span></span>

<span data-ttu-id="b3c15-147">[!code-csharp[Main](samples/program.cs)]</span><span class="sxs-lookup"><span data-stu-id="b3c15-147">[!code-csharp[Main](samples/program.cs)]</span></span>

<span data-ttu-id="b3c15-148">`Startup`debe incluir un `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="b3c15-148">`Startup` must include a `Configure` method.</span></span> <span data-ttu-id="b3c15-149">En `Configure`, agregar el middleware necesario a la canalización.</span><span class="sxs-lookup"><span data-stu-id="b3c15-149">In `Configure`, add the necessary middleware to the pipeline.</span></span> <span data-ttu-id="b3c15-150">En el ejemplo siguiente (desde la plantilla de sitio web predeterminado), varios métodos de extensión se utilizan para configurar la canalización con compatibilidad para:</span><span class="sxs-lookup"><span data-stu-id="b3c15-150">In the following example (from the default web site template), several extension methods are used to configure the pipeline with support for:</span></span>

* [<span data-ttu-id="b3c15-151">BrowserLink</span><span class="sxs-lookup"><span data-stu-id="b3c15-151">BrowserLink</span></span>](http://vswebessentials.com/features/browserlink)
* <span data-ttu-id="b3c15-152">Páginas de errores</span><span class="sxs-lookup"><span data-stu-id="b3c15-152">Error pages</span></span>
* <span data-ttu-id="b3c15-153">Archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="b3c15-153">Static files</span></span>
* <span data-ttu-id="b3c15-154">Núcleo de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b3c15-154">ASP.NET Core MVC</span></span>
* <span data-ttu-id="b3c15-155">Identidad</span><span class="sxs-lookup"><span data-stu-id="b3c15-155">Identity</span></span>

<span data-ttu-id="b3c15-156">[!code-csharp[Main](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]</span><span class="sxs-lookup"><span data-stu-id="b3c15-156">[!code-csharp[Main](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]</span></span>

<span data-ttu-id="b3c15-157">El host y la aplicación se han separado, lo que proporciona la flexibilidad de pasar a una plataforma diferente en el futuro.</span><span class="sxs-lookup"><span data-stu-id="b3c15-157">The host and application have been decoupled, which provides the flexibility of moving to a different platform in the future.</span></span>

<span data-ttu-id="b3c15-158">**Nota:** encontrará una referencia más detallada sobre ASP.NET Core inicio y Middleware, [inicio en ASP.NET Core](xref:fundamentals/startup)</span><span class="sxs-lookup"><span data-stu-id="b3c15-158">**Note:** For a more in-depth reference to ASP.NET Core Startup and Middleware, see [Startup in ASP.NET Core](xref:fundamentals/startup)</span></span>

## <a name="storing-configurations"></a><span data-ttu-id="b3c15-159">Almacenar configuraciones de</span><span class="sxs-lookup"><span data-stu-id="b3c15-159">Storing Configurations</span></span>
<span data-ttu-id="b3c15-160">ASP.NET es compatible con almacenamiento de la configuración.</span><span class="sxs-lookup"><span data-stu-id="b3c15-160">ASP.NET supports storing settings.</span></span> <span data-ttu-id="b3c15-161">Esta configuración se utilizan, por ejemplo, para admitir el entorno en el que se implementaron las aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="b3c15-161">These setting are used, for example, to support the environment to which the applications were deployed.</span></span> <span data-ttu-id="b3c15-162">Era una práctica común almacenar todos los pares de clave y valor personalizados en el `<appSettings>` sección de la *Web.config* archivo:</span><span class="sxs-lookup"><span data-stu-id="b3c15-162">A common practice was to store all custom key-value pairs in the `<appSettings>` section of the *Web.config* file:</span></span>

<span data-ttu-id="b3c15-163">[!code-xml[Main](samples/webconfig-sample.xml)]</span><span class="sxs-lookup"><span data-stu-id="b3c15-163">[!code-xml[Main](samples/webconfig-sample.xml)]</span></span>

<span data-ttu-id="b3c15-164">Aplicaciones leen estos valores mediante la `ConfigurationManager.AppSettings` colección en el `System.Configuration` espacio de nombres:</span><span class="sxs-lookup"><span data-stu-id="b3c15-164">Applications read these settings using the `ConfigurationManager.AppSettings` collection in the `System.Configuration` namespace:</span></span>

<span data-ttu-id="b3c15-165">[!code-csharp[Main](samples/read-webconfig.cs)]</span><span class="sxs-lookup"><span data-stu-id="b3c15-165">[!code-csharp[Main](samples/read-webconfig.cs)]</span></span>

<span data-ttu-id="b3c15-166">ASP.NET Core puede almacenar datos de configuración de la aplicación en cualquier archivo y cargarlos como parte de arranque de middleware.</span><span class="sxs-lookup"><span data-stu-id="b3c15-166">ASP.NET Core can store configuration data for the application in any file and load them as part of middleware bootstrapping.</span></span> <span data-ttu-id="b3c15-167">El archivo predeterminado que se usa en las plantillas de proyecto es *appSettings.JSON que se*:</span><span class="sxs-lookup"><span data-stu-id="b3c15-167">The default file used in the project templates is *appsettings.json*:</span></span>

<span data-ttu-id="b3c15-168">[!code-json[Main](samples/appsettings-sample.json)]</span><span class="sxs-lookup"><span data-stu-id="b3c15-168">[!code-json[Main](samples/appsettings-sample.json)]</span></span>

<span data-ttu-id="b3c15-169">Cargar este archivo en una instancia de `IConfiguration` dentro de la aplicación se realiza en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="b3c15-169">Loading this file into an instance of `IConfiguration` inside your application is done in *Startup.cs*:</span></span>

<span data-ttu-id="b3c15-170">[!code-csharp[Main](samples/startup-builder.cs)]</span><span class="sxs-lookup"><span data-stu-id="b3c15-170">[!code-csharp[Main](samples/startup-builder.cs)]</span></span>

<span data-ttu-id="b3c15-171">La aplicación lee de `Configuration` para obtener la configuración:</span><span class="sxs-lookup"><span data-stu-id="b3c15-171">The app reads from `Configuration` to get the settings:</span></span>

<span data-ttu-id="b3c15-172">[!code-csharp[Main](samples/read-appsettings.cs)]</span><span class="sxs-lookup"><span data-stu-id="b3c15-172">[!code-csharp[Main](samples/read-appsettings.cs)]</span></span>

<span data-ttu-id="b3c15-173">Existen extensiones de este enfoque para hacer que el proceso sea más sólida, como el uso de [inyección de dependencia](xref:fundamentals/dependency-injection) (DI) para cargar un servicio con estos valores.</span><span class="sxs-lookup"><span data-stu-id="b3c15-173">There are extensions to this approach to make the process more robust, such as using [Dependency Injection](xref:fundamentals/dependency-injection) (DI) to load a service with these values.</span></span> <span data-ttu-id="b3c15-174">El enfoque de DI proporciona un conjunto fuertemente tipada de objetos de configuración.</span><span class="sxs-lookup"><span data-stu-id="b3c15-174">The DI approach provides a strongly-typed set of configuration objects.</span></span>

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

<span data-ttu-id="b3c15-175">**Nota:** para una referencia más detallada para la configuración básica de ASP.NET, vea [configuración en ASP.NET Core](xref:fundamentals/configuration).</span><span class="sxs-lookup"><span data-stu-id="b3c15-175">**Note:** For a more in-depth reference to ASP.NET Core configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration).</span></span>

## <a name="native-dependency-injection"></a><span data-ttu-id="b3c15-176">Inyección de dependencia nativo</span><span class="sxs-lookup"><span data-stu-id="b3c15-176">Native Dependency Injection</span></span>
<span data-ttu-id="b3c15-177">Un objetivo importante al compilar aplicaciones grandes y escalables, es el acoplamiento flexible de componentes y servicios.</span><span class="sxs-lookup"><span data-stu-id="b3c15-177">An important goal when building large, scalable applications is the loose coupling of components and services.</span></span> <span data-ttu-id="b3c15-178">[Inyección de dependencia](xref:fundamentals/dependency-injection) es una técnica popular para lograr esto, y es un componente nativo de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b3c15-178">[Dependency Injection](xref:fundamentals/dependency-injection) is a popular technique for achieving this, and it is a native component of ASP.NET Core.</span></span>

<span data-ttu-id="b3c15-179">En las aplicaciones ASP.NET, los desarrolladores se basan en una biblioteca de terceros para implementar la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="b3c15-179">In ASP.NET applications, developers rely on a third-party library to implement Dependency Injection.</span></span> <span data-ttu-id="b3c15-180">Es una de estas bibliotecas [Unity](https://github.com/unitycontainer/unity), suministrado por Microsoft Patterns & Practices.</span><span class="sxs-lookup"><span data-stu-id="b3c15-180">One such library is [Unity](https://github.com/unitycontainer/unity), provided by Microsoft Patterns & Practices.</span></span> 

<span data-ttu-id="b3c15-181">Está implementando un ejemplo de configuración de la inserción de dependencias con Unity `IDependencyResolver` que encapsula un `UnityContainer`:</span><span class="sxs-lookup"><span data-stu-id="b3c15-181">An example of setting up Dependency Injection with Unity is implementing `IDependencyResolver` that wraps a `UnityContainer`:</span></span>

<span data-ttu-id="b3c15-182">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]</span><span class="sxs-lookup"><span data-stu-id="b3c15-182">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]</span></span>

<span data-ttu-id="b3c15-183">Cree una instancia de la `UnityContainer`, registrar el servicio y establecer la resolución de dependencia de `HttpConfiguration` a la nueva instancia de `UnityResolver` para el contenedor:</span><span class="sxs-lookup"><span data-stu-id="b3c15-183">Create an instance of your `UnityContainer`, register your service, and set the dependency resolver of `HttpConfiguration` to the new instance of `UnityResolver` for your container:</span></span>

<span data-ttu-id="b3c15-184">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]</span><span class="sxs-lookup"><span data-stu-id="b3c15-184">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]</span></span>

<span data-ttu-id="b3c15-185">Insertar `IProductRepository` cuando sea necesario:</span><span class="sxs-lookup"><span data-stu-id="b3c15-185">Inject `IProductRepository` where needed:</span></span>

<span data-ttu-id="b3c15-186">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]</span><span class="sxs-lookup"><span data-stu-id="b3c15-186">[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]</span></span>

<span data-ttu-id="b3c15-187">Dado que inyección de dependencia es parte del núcleo de ASP.NET, puede agregar el servicio en la `ConfigureServices` método *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="b3c15-187">Because Dependency Injection is part of ASP.NET Core, you can add your service in the `ConfigureServices` method of *Startup.cs*:</span></span>

<span data-ttu-id="b3c15-188">[!code-csharp[Main](samples/configure-services.cs)]</span><span class="sxs-lookup"><span data-stu-id="b3c15-188">[!code-csharp[Main](samples/configure-services.cs)]</span></span>

<span data-ttu-id="b3c15-189">El repositorio se puede insertar en cualquier lugar, como ocurría con Unity.</span><span class="sxs-lookup"><span data-stu-id="b3c15-189">The repository can be injected anywhere, as was true with Unity.</span></span>

<span data-ttu-id="b3c15-190">**Nota:** para una referencia detallada a la inyección de dependencia en ASP.NET Core, vea [inyección de dependencia en ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)</span><span class="sxs-lookup"><span data-stu-id="b3c15-190">**Note:** For an in-depth reference to dependency injection in ASP.NET Core, see [Dependency Injection in ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)</span></span>

## <a name="serving-static-files"></a><span data-ttu-id="b3c15-191">Enviar archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="b3c15-191">Serving Static Files</span></span>
<span data-ttu-id="b3c15-192">Una parte importante del desarrollo web es la capacidad para atender activos estáticos, de cliente.</span><span class="sxs-lookup"><span data-stu-id="b3c15-192">An important part of web development is the ability to serve static, client-side assets.</span></span> <span data-ttu-id="b3c15-193">Los ejemplos más comunes de los archivos estáticos son HTML, CSS, Javascript e imágenes.</span><span class="sxs-lookup"><span data-stu-id="b3c15-193">The most common examples of static files are HTML, CSS, Javascript, and images.</span></span> <span data-ttu-id="b3c15-194">Estos archivos se deben guardar en la ubicación de publicación de la aplicación (o la red CDN), y hace referencia por lo que se pueden cargar por una solicitud.</span><span class="sxs-lookup"><span data-stu-id="b3c15-194">These files need to be saved in the published location of the app (or CDN) and referenced so they can be loaded by a request.</span></span> <span data-ttu-id="b3c15-195">Este proceso ha cambiado en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b3c15-195">This process has changed in ASP.NET Core.</span></span>

<span data-ttu-id="b3c15-196">En ASP.NET, archivos estáticos se almacenan en directorios distintos y se hace referencia en las vistas.</span><span class="sxs-lookup"><span data-stu-id="b3c15-196">In ASP.NET, static files are stored in various directories and referenced in the views.</span></span>

<span data-ttu-id="b3c15-197">En el núcleo de ASP.NET, archivos estáticos se almacenan en la raíz de web de"" (*&lt;raíz del contenido&gt;/wwwroot*), a menos que configure de otra manera.</span><span class="sxs-lookup"><span data-stu-id="b3c15-197">In ASP.NET Core, static files are stored in the "web root" (*&lt;content root&gt;/wwwroot*), unless configured otherwise.</span></span> <span data-ttu-id="b3c15-198">Los archivos se cargan en la canalización de solicitud invocando la `UseStaticFiles` método de extensión de `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="b3c15-198">The files are loaded into the request pipeline by invoking the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

<span data-ttu-id="b3c15-199">[!code-csharp[Main](../../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="b3c15-199">[!code-csharp[Main](../../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]</span></span>

<span data-ttu-id="b3c15-200">**Nota:** si el destino es .NET Framework, instale el paquete NuGet `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="b3c15-200">**Note:** If targeting .NET Framework, install the NuGet package `Microsoft.AspNetCore.StaticFiles`.</span></span>

<span data-ttu-id="b3c15-201">Por ejemplo, un recurso de imagen en el *wwwroot/imágenes* carpeta es accesible para el explorador en una ubicación como `http://<app>/images/<imageFileName>`.</span><span class="sxs-lookup"><span data-stu-id="b3c15-201">For example, an image asset in the *wwwroot/images* folder is accessible to the browser at a location such as `http://<app>/images/<imageFileName>`.</span></span>

<span data-ttu-id="b3c15-202">**Nota:** para una referencia más detallada para enviar archivos estáticos en ASP.NET Core, vea [Introducción a trabajar con archivos estáticos en ASP.NET Core](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="b3c15-202">**Note:** For a more in-depth reference to serving static files in ASP.NET Core, see [Introduction to working with static files in ASP.NET Core](xref:fundamentals/static-files).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b3c15-203">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b3c15-203">Additional Resources</span></span>
* [<span data-ttu-id="b3c15-204">Migrar las bibliotecas de .NET Core</span><span class="sxs-lookup"><span data-stu-id="b3c15-204">Porting Libraries to .NET Core</span></span>](https://docs.microsoft.com/dotnet/core/porting/libraries)
