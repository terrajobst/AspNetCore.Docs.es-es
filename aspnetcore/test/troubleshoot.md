---
title: Solucionar problemas de proyectos de ASP.NET Core
author: Rick-Anderson
description: Conozca y solucione advertencias y errores en proyectos de ASP.NET Core.
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: ae4e6f191d8f856de60ecf21cb882b5ee9b02064
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274598"
---
# <a name="troubleshoot-aspnet-core-projects"></a>Solucionar problemas de proyectos de ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Los vínculos siguientes proporcionan instrucciones para solucionar problemas:

* [Solución de problemas de ASP.NET Core en Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)
* [Solución de problemas de ASP.NET Core en IIS](xref:host-and-deploy/iis/troubleshoot)
* [Referencia de errores comunes de Azure App Service e IIS con ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Conferencia NDC (Londres, 2018): Diagnosticar problemas en aplicaciones de ASP.NET Core](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [Blog de ASP.NET: Solucionar problemas de rendimiento de ASP.NET Core](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>Advertencias de .NET core SDK

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>Se instalan las versiones de 64 bits de .NET Core SDK y 32 bits

En el **nuevo proyecto** cuadro de diálogo para ASP.NET Core, verá la advertencia siguiente:

> Se instalan las versiones de 32 y 64 bits de .NET Core SDK. Sólo las plantillas de las versiones de 64 bits instaladas en ' C:\\archivos de programa\\dotnet\\sdk\\' se mostrará.

![Una captura de pantalla del cuadro de diálogo OneASP.NET que muestra el mensaje de advertencia](troubleshoot/_static/both32and64bit.png)

Esta advertencia aparece cuando (x86) 32 bits y las versiones de 64 bits (x 64) de la [.NET Core SDK](https://www.microsoft.com/net/download/all) están instalados. Ambas versiones pueden instalarse de motivos comunes son:

* Descargar al instalador de .NET Core SDK con un equipo de 32 bits pero, a continuación, copiarla en y originalmente había instalado en un equipo de 64 bits.
* El SDK de núcleo de .NET de 32 bits se instaló por otra aplicación.
* Se ha descargado e instalado una versión incorrecta.

Desinstale el SDK de núcleo de .NET de 32 bits para evitar esta advertencia. Desinstalar desde **el Panel de Control** > **programas y características** > **desinstalar o cambiar un programa**. Si entiende por qué se produce la advertencia y sus implicaciones, puede omitir la advertencia.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>.NET Core SDK está instalado en varias ubicaciones

En el **nuevo proyecto** cuadro de diálogo para ASP.NET Core, verá la advertencia siguiente:

> .NET Core SDK está instalado en varias ubicaciones. Sólo las plantillas de la SDK(s) instalado en ' C:\\archivos de programa\\dotnet\\sdk\\' se mostrará.

![Una captura de pantalla del cuadro de diálogo OneASP.NET que muestra el mensaje de advertencia](troubleshoot/_static/multiplelocations.png)

Verá este mensaje cuando se tiene al menos una instalación de .NET Core SDK en un directorio fuera de *C:\\archivos de programa\\dotnet\\sdk\\*. Normalmente esto sucede cuando se ha implementado el SDK de .NET Core en una máquina con copiar y pegar en lugar del instalador MSI.

Desinstale el SDK de núcleo de .NET de 32 bits para evitar esta advertencia. Desinstalar desde **el Panel de Control** > **programas y características** > **desinstalar o cambiar un programa**. Si entiende por qué se produce la advertencia y sus implicaciones, puede omitir la advertencia.

### <a name="no-net-core-sdks-were-detected"></a>No se detectaron ningún SDK de .NET Core

En el **nuevo proyecto** cuadro de diálogo para ASP.NET Core, verá la advertencia siguiente:

> No se detectaron ningún SDK de .NET Core, asegúrese de que se incluyen en la variable de entorno 'PATH'.

![Una captura de pantalla del cuadro de diálogo OneASP.NET que muestra el mensaje de advertencia](troubleshoot/_static/NoNetCore.png)

Esta advertencia aparece cuando la variable de entorno `PATH` no apunta a los SDK de núcleo de .NET en el equipo. Para resolver este problema:

* Instalar o comprobar que está instalado el SDK de .NET Core.
* Compruebe el `PATH` variable de entorno apunta a la ubicación que está instalado el SDK. El instalador normalmente establece el `PATH`.

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-app-deadlocks"></a>Uso de IHtmlHelper.Partial puede producir interbloqueos de aplicación

En ASP.NET Core 2.1 y versiones posteriores, al llamar a `Html.Partial` da como resultado una advertencia de analizador debido a la posibilidad de interbloqueos. Es el mensaje de advertencia:

> Uso de IHtmlHelper.Partial puede producir interbloqueos de la aplicación. Considere el uso de `<partial>` auxiliar de etiqueta o `IHtmlHelper.PartialAsync`.

Las llamadas a `@Html.Partial` deben reemplazarse por `@await Html.PartialAsync` o la aplicación auxiliar de la etiqueta parcial `<partial name="_Partial" />`.

::: moniker-end
