---
title: Estructura de directorios de ASP.NET Core
author: guardrex
description: La estructura de directorios de aplicaciones de ASP.NET Core publicadas.
keywords: "Núcleo de ASP.NET, estructura de directorios"
ms.author: riande
manager: wpickett
ms.date: 03/15/2017
ms.topic: article
ms.assetid: e55eb131-d42e-4bf6-b130-fd626082243c
ms.technology: aspnet
ms.prod: asp.net-core
uid: hosting/directory-structure
ms.openlocfilehash: 94b629859ddfd26095c44d444ee403bf65b127ec
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="directory-structure-of-published-aspnet-core-apps"></a>Estructura de directorios de las aplicaciones ASP.NET Core publicadas

Por [Luke Latham](https://github.com/GuardRex)

En el núcleo de ASP.NET, el directorio de la aplicación, *publicar*, consta de los archivos de la aplicación, archivos de configuración, activos estáticos, paquetes y el tiempo de ejecución (para aplicaciones independientes). Se trata de la misma estructura de directorios que en versiones anteriores de ASP.NET, donde toda la aplicación reside en el directorio raíz de web.

| Tipo de aplicación | Estructura de directorios |
| --- | --- |
| Implementación del marco de trabajo dependiente | <ul><li>publicar\*<ul><li>registros de\* (si se incluye en publishOptions)</li><li>Refs\*</li><li>tiempos de ejecución\*</li><li>Vistas\* (si se incluye en publishOptions)</li><li>wwwroot\* (si se incluye en publishOptions)</li><li>archivos .dll</li><li>MyApp.Deps.JSON</li><li>MyApp.dll</li><li>MyApp.pdb</li><li>MyApp. PrecompiledViews.dll (si precompilar vistas Razor)</li><li>MyApp. PrecompiledViews.pdb (si precompilar vistas Razor)</li><li>MyApp.runtimeconfig.JSON</li><li>Web.config (si se incluye en publishOptions)</li></ul></li></ul> |
| Implementación independiente | <ul><li>publicar\*<ul><li>registros de\* (si se incluye en publishOptions)</li><li>Refs\*</li><li>Vistas\* (si se incluye en publishOptions)</li><li>wwwroot\* (si se incluye en publishOptions)</li><li>archivos .dll</li><li>MyApp.Deps.JSON</li><li>MyApp.exe</li><li>MyApp.pdb</li><li>MyApp. PrecompiledViews.dll (si precompilar vistas Razor)</li><li>MyApp. PrecompiledViews.pdb (si precompilar vistas Razor)</li><li>MyApp.runtimeconfig.JSON</li><li>Web.config (si se incluye en publishOptions)</li></ul></li></ul> |
\*Indica un directorio

El contenido de la *publicar* directorio representa el *ruta de acceso raíz del contenido*, también denominado el *ruta de acceso base de aplicación*, de la implementación. El nombre se asigna a la *publicar* su ubicación de directorio en la implementación, actúa como ruta de acceso física del servidor a la aplicación hospedada. El *wwwroot* directory, si está presente, sólo contiene recursos estáticos. El *registros* directorio puede incluirse en la implementación mediante la creación del proyecto y agregar la `<Target>` elemento se muestra a continuación para su *.csproj* archivo o creando físicamente el directorio en el servidor.

```xml
<Target Name="CreateLogsFolder" AfterTargets="AfterPublish">
  <MakeDir Directories="$(PublishDir)logs" Condition="!Exists('$(PublishDir)logs')" />
  <MakeDir Directories="$(PublishUrl)Logs" Condition="!Exists('$(PublishUrl)Logs')" />
</Target>
```

La primera `<MakeDir>` elemento, que usa el `PublishDir` propiedad, se utiliza la CLI de núcleo de .NET para determinar la ubicación de destino para la operación de publicación. El segundo `<MakeDir>` elemento, que usa el `PublishUrl` propiedad, se utiliza Visual Studio para determinar la ubicación de destino. Visual Studio usa el `PublishUrl` propiedad para la compatibilidad con proyectos no .NET Core.

El directorio de implementación requiere permisos de lectura y ejecución, mientras que la *registros* directory requiere permisos de lectura/escritura. Directorios adicionales donde se escribirá activos requieren permisos de lectura/escritura.
