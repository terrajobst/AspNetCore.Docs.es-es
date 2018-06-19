---
title: Migrar la configuración a ASP.NET Core
author: ardalis
description: Obtenga información acerca de cómo migrar la configuración de un proyecto de MVC de ASP.NET a un proyecto de MVC de ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/configuration
ms.openlocfilehash: ead4f96aa0041cd919caa972d3bb05bd94a857b3
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851727"
---
# <a name="migrate-configuration-to-aspnet-core"></a><span data-ttu-id="14d81-103">Migrar la configuración a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="14d81-103">Migrate configuration to ASP.NET Core</span></span>

<span data-ttu-id="14d81-104">Por [Steve Smith](https://ardalis.com/) y [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="14d81-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="14d81-105">En el artículo anterior, se comenzó a [migrar un proyecto de MVC de ASP.NET a ASP.NET MVC de núcleo](xref:migration/mvc).</span><span class="sxs-lookup"><span data-stu-id="14d81-105">In the previous article, we began [migrate an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/mvc).</span></span> <span data-ttu-id="14d81-106">En este artículo, se migra la configuración.</span><span class="sxs-lookup"><span data-stu-id="14d81-106">In this article, we migrate configuration.</span></span>

<span data-ttu-id="14d81-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="14d81-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="setup-configuration"></a><span data-ttu-id="14d81-108">Parámetros de configuración</span><span class="sxs-lookup"><span data-stu-id="14d81-108">Setup configuration</span></span>

<span data-ttu-id="14d81-109">ASP.NET Core ya no utiliza la *Global.asax* y *web.config* archivos que utilizan versiones anteriores de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="14d81-109">ASP.NET Core no longer uses the *Global.asax* and *web.config* files that previous versions of ASP.NET utilized.</span></span> <span data-ttu-id="14d81-110">En versiones anteriores de ASP.NET, la lógica de inicio de la aplicación se ha puesto en un `Application_StartUp` método dentro de *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="14d81-110">In earlier versions of ASP.NET, application startup logic was placed in an `Application_StartUp` method within *Global.asax*.</span></span> <span data-ttu-id="14d81-111">Más adelante, en ASP.NET MVC, un *Startup.cs* archivo se incluyó en la raíz del proyecto; y, se llamó cuando se inició la aplicación.</span><span class="sxs-lookup"><span data-stu-id="14d81-111">Later, in ASP.NET MVC, a *Startup.cs* file was included in the root of the project; and, it was called when the application started.</span></span> <span data-ttu-id="14d81-112">ASP.NET Core ha adoptado este enfoque por completo mediante la colocación de toda la lógica de inicio en el *Startup.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="14d81-112">ASP.NET Core has adopted this approach completely by placing all startup logic in the *Startup.cs* file.</span></span>

<span data-ttu-id="14d81-113">El *web.config* archivo también se ha sustituido en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="14d81-113">The *web.config* file has also been replaced in ASP.NET Core.</span></span> <span data-ttu-id="14d81-114">Configuración en Sí ahora se puede configurar, como parte del procedimiento de inicio de aplicación se describen en *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="14d81-114">Configuration itself can now be configured, as part of the application startup procedure described in *Startup.cs*.</span></span> <span data-ttu-id="14d81-115">Configuración todavía puede usar los archivos XML, pero normalmente los proyectos de ASP.NET Core colocará los valores de configuración en un archivo con formato JSON, como *appSettings.JSON que se*.</span><span class="sxs-lookup"><span data-stu-id="14d81-115">Configuration can still utilize XML files, but typically ASP.NET Core projects will place configuration values in a JSON-formatted file, such as *appsettings.json*.</span></span> <span data-ttu-id="14d81-116">Sistema de configuración de ASP.NET Core también es posible tener acceso a las variables de entorno, lo que pueden proporcionar un [ubicación más segura y sólida](xref:security/app-secrets) para valores específicos del entorno.</span><span class="sxs-lookup"><span data-stu-id="14d81-116">ASP.NET Core's configuration system can also easily access environment variables, which can provide a [more secure and robust location](xref:security/app-secrets) for environment-specific values.</span></span> <span data-ttu-id="14d81-117">Esto es especialmente cierto para los secretos como cadenas de conexión y las claves de API que no deben protegerse en el control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="14d81-117">This is especially true for secrets like connection strings and API keys that shouldn't be checked into source control.</span></span> <span data-ttu-id="14d81-118">Vea [configuración](xref:fundamentals/configuration/index) para obtener más información sobre la configuración de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="14d81-118">See [Configuration](xref:fundamentals/configuration/index) to learn more about configuration in ASP.NET Core.</span></span>

<span data-ttu-id="14d81-119">En este artículo, estamos comenzando con el proyecto de ASP.NET Core parcialmente migrado desde [el artículo anterior](xref:migration/mvc).</span><span class="sxs-lookup"><span data-stu-id="14d81-119">For this article, we are starting with the partially migrated ASP.NET Core project from [the previous article](xref:migration/mvc).</span></span> <span data-ttu-id="14d81-120">Para la configuración de la instalación, agregue el siguiente constructor y la propiedad a la *Startup.cs* archivo se encuentra en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="14d81-120">To setup configuration, add the following constructor and property to the *Startup.cs* file located in the root of the project:</span></span>

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-16)]

<span data-ttu-id="14d81-121">Tenga en cuenta que en este momento, el *Startup.cs* archivo no se compilará, tal y como necesitamos agregar lo siguiente `using` instrucción:</span><span class="sxs-lookup"><span data-stu-id="14d81-121">Note that at this point, the *Startup.cs* file won't compile, as we still need to add the following `using` statement:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="14d81-122">Agregar un *appSettings.JSON que se* archivo a la raíz del proyecto mediante la plantilla de elemento apropiado:</span><span class="sxs-lookup"><span data-stu-id="14d81-122">Add an *appsettings.json* file to the root of the project using the appropriate item template:</span></span>

![Agregar AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a><span data-ttu-id="14d81-124">Migrar la configuración de web.config</span><span class="sxs-lookup"><span data-stu-id="14d81-124">Migrate configuration settings from web.config</span></span>

<span data-ttu-id="14d81-125">Nuestro proyecto de ASP.NET MVC incluye la cadena de conexión de base de datos necesarios en *web.config*, en la `<connectionStrings>` elemento.</span><span class="sxs-lookup"><span data-stu-id="14d81-125">Our ASP.NET MVC project included the required database connection string in *web.config*, in the `<connectionStrings>` element.</span></span> <span data-ttu-id="14d81-126">En el proyecto de ASP.NET Core, vamos a almacenar esta información en el *appSettings.JSON que se* archivo.</span><span class="sxs-lookup"><span data-stu-id="14d81-126">In our ASP.NET Core project, we are going to store this information in the *appsettings.json* file.</span></span> <span data-ttu-id="14d81-127">Abra *appSettings.JSON que se*y tenga en cuenta que ya incluye lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="14d81-127">Open *appsettings.json*, and note that it already includes the following:</span></span>

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]

<span data-ttu-id="14d81-128">En la línea resaltada descrita anteriormente, cambie el nombre de la base de datos de **_CHANGE_ME** en el nombre de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="14d81-128">In the highlighted line depicted above, change the name of the database from **_CHANGE_ME** to the name of your database.</span></span>

## <a name="summary"></a><span data-ttu-id="14d81-129">Resumen</span><span class="sxs-lookup"><span data-stu-id="14d81-129">Summary</span></span>

<span data-ttu-id="14d81-130">ASP.NET Core coloca toda la lógica de inicio de la aplicación en un único archivo, en el que los servicios necesarios y las dependencias pueden definirse y configuradas.</span><span class="sxs-lookup"><span data-stu-id="14d81-130">ASP.NET Core places all startup logic for the application in a single file, in which the necessary services and dependencies can be defined and configured.</span></span> <span data-ttu-id="14d81-131">Reemplaza el *web.config* archivo con una característica de configuración flexible que puede aprovechar una variedad de formatos de archivo, como JSON, así como las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="14d81-131">It replaces the *web.config* file with a flexible configuration feature that can leverage a variety of file formats, such as JSON, as well as environment variables.</span></span>
