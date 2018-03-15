---
title: Reemplace machineKey en ASP.NET
author: rick-anderson
description: "Descubra cómo reemplazar machineKey en ASP.NET para permitir el uso de un sistema de protección de datos nueva y más segura."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: 8113780dd7b8b3010cf77d14abb45730a2f28959
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
---
# <a name="replace-machinekey-in-aspnet"></a><span data-ttu-id="1cc62-103">Reemplace machineKey en ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1cc62-103">Replace machineKey in ASP.NET</span></span>

<a name="compatibility-replacing-machinekey"></a>

<span data-ttu-id="1cc62-104">La implementación de la `<machineKey>` elemento en ASP.NET [es reemplazable](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span><span class="sxs-lookup"><span data-stu-id="1cc62-104">The implementation of the `<machineKey>` element in ASP.NET [is replaceable](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span></span> <span data-ttu-id="1cc62-105">Esto permite la mayoría de las llamadas a rutinas criptográficas de ASP.NET se enruten a través de un mecanismo de protección de datos de reemplazo, incluido el nuevo sistema de protección de datos.</span><span class="sxs-lookup"><span data-stu-id="1cc62-105">This allows most calls to ASP.NET cryptographic routines to be routed through a replacement data protection mechanism, including the new data protection system.</span></span>

## <a name="package-installation"></a><span data-ttu-id="1cc62-106">Instalación del paquete</span><span class="sxs-lookup"><span data-stu-id="1cc62-106">Package installation</span></span>

> [!NOTE]
> <span data-ttu-id="1cc62-107">El nuevo sistema de protección de datos solo puede instalarse en una aplicación ASP.NET existente como destino .NET 4.5.1 o posterior.</span><span class="sxs-lookup"><span data-stu-id="1cc62-107">The new data protection system can only be installed into an existing ASP.NET application targeting .NET 4.5.1 or higher.</span></span> <span data-ttu-id="1cc62-108">Instalación se producirá un error si la aplicación tiene como destino .NET 4.5 o Bajar.</span><span class="sxs-lookup"><span data-stu-id="1cc62-108">Installation will fail if the application targets .NET 4.5 or lower.</span></span>

<span data-ttu-id="1cc62-109">Para instalar el nuevo sistema de protección de datos en un proyecto de 4.5.1+ ASP.NET existente, instale el paquete Microsoft.AspNetCore.DataProtection.SystemWeb.</span><span class="sxs-lookup"><span data-stu-id="1cc62-109">To install the new data protection system into an existing ASP.NET 4.5.1+ project, install the package Microsoft.AspNetCore.DataProtection.SystemWeb.</span></span> <span data-ttu-id="1cc62-110">Esto creará una instancia del sistema de protección de datos mediante la [configuración predeterminada](xref:security/data-protection/configuration/default-settings) configuración.</span><span class="sxs-lookup"><span data-stu-id="1cc62-110">This will instantiate the data protection system using the [default configuration](xref:security/data-protection/configuration/default-settings) settings.</span></span>

<span data-ttu-id="1cc62-111">Cuando se instala el paquete, inserta una línea en *Web.config* que le indica a ASP.NET para usarla para [más operaciones criptográficas](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), como la autenticación de formularios, estado de vista y llamadas a MachineKey.Protect.</span><span class="sxs-lookup"><span data-stu-id="1cc62-111">When you install the package, it inserts a line into *Web.config* that tells ASP.NET to use it for [most cryptographic operations](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), including forms authentication, view state, and calls to MachineKey.Protect.</span></span> <span data-ttu-id="1cc62-112">La línea que se inserta quede como sigue.</span><span class="sxs-lookup"><span data-stu-id="1cc62-112">The line that's inserted reads as follows.</span></span>

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> <span data-ttu-id="1cc62-113">Puede indicar si el nuevo sistema de protección de datos está activo mediante la inspección de campos como `__VIEWSTATE`, que debe comenzar por "CfDJ8" en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="1cc62-113">You can tell if the new data protection system is active by inspecting fields like `__VIEWSTATE`, which should begin with "CfDJ8" as in the example below.</span></span> <span data-ttu-id="1cc62-114">"CfDJ8" es la representación base64 del encabezado de magia "09 F0 C9 F0" que identifica una carga protegida por el sistema de protección de datos.</span><span class="sxs-lookup"><span data-stu-id="1cc62-114">"CfDJ8" is the base64 representation of the magic "09 F0 C9 F0" header that identifies a payload protected by the data protection system.</span></span>

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk..." />
```

## <a name="package-configuration"></a><span data-ttu-id="1cc62-115">Configuración de paquetes</span><span class="sxs-lookup"><span data-stu-id="1cc62-115">Package configuration</span></span>

<span data-ttu-id="1cc62-116">El sistema de protección de datos se crea una instancia con una configuración predeterminada del programa de instalación de cero.</span><span class="sxs-lookup"><span data-stu-id="1cc62-116">The data protection system is instantiated with a default zero-setup configuration.</span></span> <span data-ttu-id="1cc62-117">Sin embargo, puesto que de forma predeterminada, las claves se conservan al sistema de archivos local, esto no funcionará para las aplicaciones que se implementan en una granja de servidores.</span><span class="sxs-lookup"><span data-stu-id="1cc62-117">However, since by default keys are persisted to the local file system, this won't work for applications which are deployed in a farm.</span></span> <span data-ttu-id="1cc62-118">Para resolver este problema, puede proporcionar la configuración mediante la creación de un tipo que las subclases DataProtectionStartup e invalida su método ConfigureServices.</span><span class="sxs-lookup"><span data-stu-id="1cc62-118">To resolve this, you can provide configuration by creating a type which subclasses DataProtectionStartup and overrides its ConfigureServices method.</span></span>

<span data-ttu-id="1cc62-119">A continuación se muestra un ejemplo de un tipo de inicio de protección de datos personalizado que configura tanto donde se conservan las claves, y cómo está cifrados en reposo.</span><span class="sxs-lookup"><span data-stu-id="1cc62-119">Below is an example of a custom data protection startup type which configured both where keys are persisted and how they're encrypted at rest.</span></span> <span data-ttu-id="1cc62-120">También invalida la directiva de aislamiento de aplicaciones predeterminado proporcionando su propio nombre de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1cc62-120">It also overrides the default app isolation policy by providing its own application name.</span></span>

```csharp
using System;
using System.IO;
using Microsoft.AspNetCore.DataProtection;
using Microsoft.AspNetCore.DataProtection.SystemWeb;
using Microsoft.Extensions.DependencyInjection;

namespace DataProtectionDemo
{
    public class MyDataProtectionStartup : DataProtectionStartup
    {
        public override void ConfigureServices(IServiceCollection services)
        {
            services.AddDataProtection()
                .SetApplicationName("my-app")
                .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\myapp-keys\"))
                .ProtectKeysWithCertificate("thumbprint");
        }
    }
}
```

>[!TIP]
> <span data-ttu-id="1cc62-121">También puede usar `<machineKey applicationName="my-app" ... />` en lugar de una llamada explícita a SetApplicationName.</span><span class="sxs-lookup"><span data-stu-id="1cc62-121">You can also use `<machineKey applicationName="my-app" ... />` in place of an explicit call to SetApplicationName.</span></span> <span data-ttu-id="1cc62-122">Se trata de un mecanismo de comodidad para evitar la fuerza al desarrollador para crear un tipo derivado de DataProtectionStartup si todos los que deseaban configurar se establecen el nombre de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1cc62-122">This is a convenience mechanism to avoid forcing the developer to create a DataProtectionStartup-derived type if all they wanted to configure was setting the application name.</span></span>

<span data-ttu-id="1cc62-123">Para habilitar esta configuración personalizada, vuelva al archivo Web.config y busque la `<appSettings>` elemento que instalar el paquete agregado al archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="1cc62-123">To enable this custom configuration, go back to Web.config and look for the `<appSettings>` element that the package install added to the config file.</span></span> <span data-ttu-id="1cc62-124">Tendrá una apariencia similar el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="1cc62-124">It will look like the following markup:</span></span>

```xml
<appSettings>
  <!--
  If you want to customize the behavior of the ASP.NET Core Data Protection stack, set the
  "aspnet:dataProtectionStartupType" switch below to be the fully-qualified name of a
  type which subclasses Microsoft.AspNetCore.DataProtection.SystemWeb.DataProtectionStartup.
  -->
  <add key="aspnet:dataProtectionStartupType" value="" />
</appSettings>
```

<span data-ttu-id="1cc62-125">Rellene el valor en blanco con el nombre completo de ensamblado del tipo derivado de DataProtectionStartup que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="1cc62-125">Fill in the blank value with the assembly-qualified name of the DataProtectionStartup-derived type you just created.</span></span> <span data-ttu-id="1cc62-126">Si el nombre de la aplicación es DataProtectionDemo, esto sería el siguiente.</span><span class="sxs-lookup"><span data-stu-id="1cc62-126">If the name of the application is DataProtectionDemo, this would look like the below.</span></span>

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

<span data-ttu-id="1cc62-127">El sistema de protección de datos recién configurada ahora está listo para su uso dentro de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1cc62-127">The newly-configured data protection system is now ready for use inside the application.</span></span>
