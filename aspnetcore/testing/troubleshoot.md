---
title: Solucionar problemas principales de ASP.NET
author: Rick-Anderson
description: Entender y solucionar problemas de advertencias y errores con los proyectos de ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: testing/troubleshoot
ms.openlocfilehash: a75dc666621600e1e2fe36c29acbe7484bae9229
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2018
---
# <a name="troubleshoot-aspnet-core-projects"></a>Solucionar problemas de proyectos de ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Los vínculos siguientes proporcionan instrucciones para solucionar problemas:

* [Solución de problemas de ASP.NET Core en Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)
* [Solución de problemas de ASP.NET Core en IIS](xref:host-and-deploy/iis/troubleshoot)
* [Referencia de errores comunes de Azure App Service e IIS con ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [YouTube: Diagnosticar problemas en aplicaciones de ASP.NET Core](https://www.youtube.com/watch?v=RYI0DHoIVaA)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a>Advertencias de .NET core SDK

### <a name="both-the-32-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>Ambas ediciones de 32 y 64 bits de .NET Core SDK se instalan las versiones
En el **nuevo proyecto** cuadro de diálogo para ASP.NET Core, verá la siguiente advertencia aparece en la parte superior: 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![Una captura de pantalla del cuadro de diálogo OneASP.NET que muestra el mensaje de advertencia](troubleshoot/_static/both32and64bit.png)

Esta advertencia aparece cuando (x86) 32 bits y las versiones de 64 bits (x 64) de la [.NET Core SDK](https://www.microsoft.com/net/download/all) están instalados. Los motivos comunes que se pueden instalar ambas versiones son:

* Descargar al instalador de .NET Core SDK con un equipo de 32 bits, pero, a continuación, copiarla en y originalmente había instalado en un equipo de 64 bits. 
* El SDK de núcleo de .NET de 32 bits se instaló por otra aplicación.
* Se ha descargado e instalado una versión incorrecta.

Desinstale el SDK de núcleo de .NET de 32 bits para evitar esta advertencia. Desinstalar desde **el Panel de Control** > **programas y características** > **desinstalar o cambiar un programa**. Si entiende por qué se produce la advertencia y sus implicaciones, puede omitir la advertencia.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>.NET Core SDK está instalado en varias ubicaciones
En el **nuevo proyecto** cuadro de diálogo para ASP.NET Core puede ver la siguiente advertencia aparece en la parte superior: 

 .NET Core SDK está instalado en varias ubicaciones. Sola plantillas desde el SDK(s) instalado en ' C:\Program Files\dotnet\sdk\' se mostrará.

![Una captura de pantalla del cuadro de diálogo OneASP.NET que muestra el mensaje de advertencia](troubleshoot/_static/multiplelocations.png)

Está viendo este mensaje porque tiene al menos una instalación de .NET Core SDK en un directorio fuera de * C:\Program Files\dotnet\sdk\*. Normalmente esto sucede cuando el SDK de .NET Core se ha implementado en un equipo con copiar y pegar en lugar del instalador MSI.

Desinstale el SDK de núcleo de .NET de 32 bits para evitar esta advertencia. Desinstalar desde **el Panel de Control** > **programas y características** > **desinstalar o cambiar un programa**. Si entiende por qué se produce la advertencia y sus implicaciones, puede omitir la advertencia.
