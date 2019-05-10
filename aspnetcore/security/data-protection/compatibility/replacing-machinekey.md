---
title: Reemplace el elemento machineKey ASP.NET en ASP.NET Core
author: rick-anderson
description: Descubra cómo reemplazar machineKey en ASP.NET para permitir el uso de un sistema de protección de datos nueva y más segura.
ms.author: riande
ms.date: 04/06/2019
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: 2317cb50cfe63226baf336ebfc5d681d1cebe5c6
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895342"
---
# <a name="replace-the-aspnet-machinekey-in-aspnet-core"></a>Reemplace el elemento machineKey ASP.NET en ASP.NET Core

<a name="compatibility-replacing-machinekey"></a>

La implementación de la `<machineKey>` elemento en ASP.NET [es reemplazable](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/). Esto permite la mayoría de las llamadas a rutinas criptográficas de ASP.NET se enrute a través de un mecanismo de protección de datos de reemplazo, incluido el nuevo sistema de protección de datos.

## <a name="package-installation"></a>Instalación del paquete

> [!NOTE]
> El nuevo sistema de protección de datos solo puede ser instalado en una aplicación existente de ASP.NET destinadas a .NET 4.5.1 o posterior. Se producirá un error si la aplicación tiene como destino .NET 4.5 instalación o se reduzca.

Para instalar el nuevo sistema de protección de datos en un proyecto de 4.5.1+ ASP.NET existente, instale el paquete Microsoft.AspNetCore.DataProtection.SystemWeb. Esto creará una instancia del sistema de protección de datos mediante el [configuración predeterminada](xref:security/data-protection/configuration/default-settings) configuración.

Cuando se instala el paquete, inserta una línea en *Web.config* que le indica a ASP.NET que se usa para [más operaciones criptográficas](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), incluida la autenticación de formularios, el estado de vista y las llamadas a MachineKey.Protect. La línea que se inserta quede como sigue.

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> Puede indicar si el nuevo sistema de protección de datos está activo mediante la inspección de los campos como `__VIEWSTATE`, que debe comenzar por "CfDJ8" como se muestra en el ejemplo siguiente. "CfDJ8" es la representación en base64 del encabezado de magia "09 F0 C9 F0" que identifica una carga protegida por el sistema de protección de datos.

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk...">
```

## <a name="package-configuration"></a>configuración de paquete

El sistema de protección de datos se crea una instancia con una configuración predeterminada del programa de instalación de cero. Sin embargo, dado que de forma predeterminada las claves se conservan en el sistema de archivos local, esto no funcionará para las aplicaciones que se implementan en una granja de servidores. Para resolver este problema, puede proporcionar la configuración mediante la creación de un tipo cuyas subclases DataProtectionStartup e invalida su método ConfigureServices.

A continuación es un ejemplo de un tipo de inicio de protección de datos personalizados que había configurado donde se almacenan las claves y cómo se cifran en reposo. También invalida la directiva de aislamiento de aplicaciones de forma predeterminada, ya que proporciona su propio nombre de la aplicación.

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
> También puede usar `<machineKey applicationName="my-app" ... />` en lugar de una llamada explícita a SetApplicationName. Se trata de un mecanismo de conveniencia para evitar forzar al desarrollador a crear un tipo derivado de DataProtectionStartup si todos los que desean configurar se estableciendo el nombre de la aplicación.

Para habilitar esta configuración personalizada, vuelva al archivo Web.config y busque el `<appSettings>` elemento que instalar el paquete agregado al archivo de configuración. Tendrá un aspecto como el marcado siguiente:

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

El sistema de protección de datos recién configuradas ahora está listo para su uso dentro de la aplicación.
