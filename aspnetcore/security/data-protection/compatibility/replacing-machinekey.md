---
title: Reemplace el elemento machineKey ASP.NET en ASP.NET Core
author: rick-anderson
description: Descubra cómo reemplazar machineKey en ASP.NET para permitir el uso de un sistema de protección de datos nueva y más segura.
ms.author: riande
ms.date: 04/06/2019
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: 2317cb50cfe63226baf336ebfc5d681d1cebe5c6
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59705554"
---
# <a name="replace-the-aspnet-machinekey-in-aspnet-core"></a><span data-ttu-id="a6d42-103">Reemplace el elemento machineKey ASP.NET en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a6d42-103">Replace the ASP.NET machineKey in ASP.NET Core</span></span>

<a name="compatibility-replacing-machinekey"></a>

<span data-ttu-id="a6d42-104">La implementación de la `<machineKey>` elemento en ASP.NET [es reemplazable](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span><span class="sxs-lookup"><span data-stu-id="a6d42-104">The implementation of the `<machineKey>` element in ASP.NET [is replaceable](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span></span> <span data-ttu-id="a6d42-105">Esto permite la mayoría de las llamadas a rutinas criptográficas de ASP.NET se enrute a través de un mecanismo de protección de datos de reemplazo, incluido el nuevo sistema de protección de datos.</span><span class="sxs-lookup"><span data-stu-id="a6d42-105">This allows most calls to ASP.NET cryptographic routines to be routed through a replacement data protection mechanism, including the new data protection system.</span></span>

## <a name="package-installation"></a><span data-ttu-id="a6d42-106">Instalación del paquete</span><span class="sxs-lookup"><span data-stu-id="a6d42-106">Package installation</span></span>

> [!NOTE]
> <span data-ttu-id="a6d42-107">El nuevo sistema de protección de datos solo puede ser instalado en una aplicación existente de ASP.NET destinadas a .NET 4.5.1 o posterior.</span><span class="sxs-lookup"><span data-stu-id="a6d42-107">The new data protection system can only be installed into an existing ASP.NET application targeting .NET 4.5.1 or later.</span></span> <span data-ttu-id="a6d42-108">Se producirá un error si la aplicación tiene como destino .NET 4.5 instalación o se reduzca.</span><span class="sxs-lookup"><span data-stu-id="a6d42-108">Installation will fail if the application targets .NET 4.5 or lower.</span></span>

<span data-ttu-id="a6d42-109">Para instalar el nuevo sistema de protección de datos en un proyecto de 4.5.1+ ASP.NET existente, instale el paquete Microsoft.AspNetCore.DataProtection.SystemWeb.</span><span class="sxs-lookup"><span data-stu-id="a6d42-109">To install the new data protection system into an existing ASP.NET 4.5.1+ project, install the package Microsoft.AspNetCore.DataProtection.SystemWeb.</span></span> <span data-ttu-id="a6d42-110">Esto creará una instancia del sistema de protección de datos mediante el [configuración predeterminada](xref:security/data-protection/configuration/default-settings) configuración.</span><span class="sxs-lookup"><span data-stu-id="a6d42-110">This will instantiate the data protection system using the [default configuration](xref:security/data-protection/configuration/default-settings) settings.</span></span>

<span data-ttu-id="a6d42-111">Cuando se instala el paquete, inserta una línea en *Web.config* que le indica a ASP.NET que se usa para [más operaciones criptográficas](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), incluida la autenticación de formularios, el estado de vista y las llamadas a MachineKey.Protect.</span><span class="sxs-lookup"><span data-stu-id="a6d42-111">When you install the package, it inserts a line into *Web.config* that tells ASP.NET to use it for [most cryptographic operations](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), including forms authentication, view state, and calls to MachineKey.Protect.</span></span> <span data-ttu-id="a6d42-112">La línea que se inserta quede como sigue.</span><span class="sxs-lookup"><span data-stu-id="a6d42-112">The line that's inserted reads as follows.</span></span>

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> <span data-ttu-id="a6d42-113">Puede indicar si el nuevo sistema de protección de datos está activo mediante la inspección de los campos como `__VIEWSTATE`, que debe comenzar por "CfDJ8" como se muestra en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="a6d42-113">You can tell if the new data protection system is active by inspecting fields like `__VIEWSTATE`, which should begin with "CfDJ8" as in the example below.</span></span> <span data-ttu-id="a6d42-114">"CfDJ8" es la representación en base64 del encabezado de magia "09 F0 C9 F0" que identifica una carga protegida por el sistema de protección de datos.</span><span class="sxs-lookup"><span data-stu-id="a6d42-114">"CfDJ8" is the base64 representation of the magic "09 F0 C9 F0" header that identifies a payload protected by the data protection system.</span></span>

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk...">
```

## <a name="package-configuration"></a><span data-ttu-id="a6d42-115">configuración de paquete</span><span class="sxs-lookup"><span data-stu-id="a6d42-115">Package configuration</span></span>

<span data-ttu-id="a6d42-116">El sistema de protección de datos se crea una instancia con una configuración predeterminada del programa de instalación de cero.</span><span class="sxs-lookup"><span data-stu-id="a6d42-116">The data protection system is instantiated with a default zero-setup configuration.</span></span> <span data-ttu-id="a6d42-117">Sin embargo, dado que de forma predeterminada las claves se conservan en el sistema de archivos local, esto no funcionará para las aplicaciones que se implementan en una granja de servidores.</span><span class="sxs-lookup"><span data-stu-id="a6d42-117">However, since by default keys are persisted to the local file system, this won't work for applications which are deployed in a farm.</span></span> <span data-ttu-id="a6d42-118">Para resolver este problema, puede proporcionar la configuración mediante la creación de un tipo cuyas subclases DataProtectionStartup e invalida su método ConfigureServices.</span><span class="sxs-lookup"><span data-stu-id="a6d42-118">To resolve this, you can provide configuration by creating a type which subclasses DataProtectionStartup and overrides its ConfigureServices method.</span></span>

<span data-ttu-id="a6d42-119">A continuación es un ejemplo de un tipo de inicio de protección de datos personalizados que había configurado donde se almacenan las claves y cómo se cifran en reposo.</span><span class="sxs-lookup"><span data-stu-id="a6d42-119">Below is an example of a custom data protection startup type which configured both where keys are persisted and how they're encrypted at rest.</span></span> <span data-ttu-id="a6d42-120">También invalida la directiva de aislamiento de aplicaciones de forma predeterminada, ya que proporciona su propio nombre de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a6d42-120">It also overrides the default app isolation policy by providing its own application name.</span></span>

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
> <span data-ttu-id="a6d42-121">También puede usar `<machineKey applicationName="my-app" ... />` en lugar de una llamada explícita a SetApplicationName.</span><span class="sxs-lookup"><span data-stu-id="a6d42-121">You can also use `<machineKey applicationName="my-app" ... />` in place of an explicit call to SetApplicationName.</span></span> <span data-ttu-id="a6d42-122">Se trata de un mecanismo de conveniencia para evitar forzar al desarrollador a crear un tipo derivado de DataProtectionStartup si todos los que desean configurar se estableciendo el nombre de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a6d42-122">This is a convenience mechanism to avoid forcing the developer to create a DataProtectionStartup-derived type if all they wanted to configure was setting the application name.</span></span>

<span data-ttu-id="a6d42-123">Para habilitar esta configuración personalizada, vuelva al archivo Web.config y busque el `<appSettings>` elemento que instalar el paquete agregado al archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="a6d42-123">To enable this custom configuration, go back to Web.config and look for the `<appSettings>` element that the package install added to the config file.</span></span> <span data-ttu-id="a6d42-124">Tendrá un aspecto como el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="a6d42-124">It will look like the following markup:</span></span>

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

<span data-ttu-id="a6d42-125">Rellene el valor en blanco con el nombre completo de ensamblado del tipo derivado de DataProtectionStartup que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="a6d42-125">Fill in the blank value with the assembly-qualified name of the DataProtectionStartup-derived type you just created.</span></span> <span data-ttu-id="a6d42-126">Si el nombre de la aplicación es DataProtectionDemo, esto sería el siguiente.</span><span class="sxs-lookup"><span data-stu-id="a6d42-126">If the name of the application is DataProtectionDemo, this would look like the below.</span></span>

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

<span data-ttu-id="a6d42-127">El sistema de protección de datos recién configuradas ahora está listo para su uso dentro de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a6d42-127">The newly-configured data protection system is now ready for use inside the application.</span></span>
