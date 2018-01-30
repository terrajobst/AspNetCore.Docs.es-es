---
title: Reemplazar `<machineKey>` en ASP.NET
author: rick-anderson
description: Reemplazar `<machineKey>` en ASP.NET
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: 865c949a526e07d41ac4627fdf0411d5422adc16
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="replacing-machinekey-in-aspnet"></a>Reemplazar `<machineKey>` en ASP.NET

<a name="compatibility-replacing-machinekey"></a>

La implementación de la `<machineKey>` elemento en ASP.NET [es reemplazable](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/). Esto permite la mayoría de las llamadas a rutinas criptográficas de ASP.NET se enruten a través de un mecanismo de protección de datos de reemplazo, incluido el nuevo sistema de protección de datos.

## <a name="package-installation"></a>Instalación del paquete

> [!NOTE]
> El nuevo sistema de protección de datos solo puede instalarse en una aplicación ASP.NET existente como destino .NET 4.5.1 o posterior. Instalación se producirá un error si la aplicación tiene como destino .NET 4.5 o Bajar.

Para instalar el nuevo sistema de protección de datos en un proyecto de 4.5.1+ ASP.NET existente, instale el paquete Microsoft.AspNetCore.DataProtection.SystemWeb. Esto creará una instancia del sistema de protección de datos mediante la [configuración predeterminada](xref:security/data-protection/configuration/default-settings) configuración.

Cuando se instala el paquete, inserta una línea en *Web.config* que le indica a ASP.NET para usarla para [más operaciones criptográficas](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), como la autenticación de formularios, estado de vista y llamadas a MachineKey.Protect. La línea que se inserta quede como sigue.

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> Puede indicar si el nuevo sistema de protección de datos está activo mediante la inspección de campos como `__VIEWSTATE`, que debe comenzar por "CfDJ8" en el ejemplo siguiente. "CfDJ8" es la representación base64 del encabezado de magia "09 F0 C9 F0" que identifica una carga protegida por el sistema de protección de datos.

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk..." />
```

## <a name="package-configuration"></a>Configuración de paquetes

El sistema de protección de datos se crea una instancia con una configuración predeterminada del programa de instalación de cero. Sin embargo, puesto que de forma predeterminada, las claves se conservan al sistema de archivos local, esto no funcionará para las aplicaciones que se implementan en una granja de servidores. Para resolver este problema, puede proporcionar la configuración mediante la creación de un tipo que las subclases DataProtectionStartup e invalida su método ConfigureServices.

A continuación se muestra un ejemplo de un tipo de inicio de protección de datos personalizado que configura tanto donde se conservan las claves, y cómo está cifrados en reposo. También invalida la directiva de aislamiento de aplicaciones predeterminado proporcionando su propio nombre de la aplicación.

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
> También puede usar `<machineKey applicationName="my-app" ... />` en lugar de una llamada explícita a SetApplicationName. Se trata de un mecanismo de comodidad para evitar la fuerza al desarrollador para crear un tipo derivado de DataProtectionStartup si todos los que deseaban configurar se establecen el nombre de la aplicación.

Para habilitar esta configuración personalizada, vuelva al archivo Web.config y busque la `<appSettings>` elemento que instalar el paquete agregado al archivo de configuración. Tendrá una apariencia similar el siguiente marcado:

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

Rellene el valor en blanco con el nombre completo de ensamblado del tipo derivado de DataProtectionStartup que acaba de crear. Si el nombre de la aplicación es DataProtectionDemo, esto sería el siguiente.

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

El sistema de protección de datos recién configurada ahora está listo para su uso dentro de la aplicación.
