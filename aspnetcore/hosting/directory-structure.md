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
ms.openlocfilehash: 60797bff85a44dd10caad4aabc109ee12dedfe61
ms.sourcegitcommit: 7efdc4b6025ad70c15c26bf7451c3c0411123794
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2017
---
# <a name="directory-structure-of-published-aspnet-core-apps"></a>Estructura de directorios de las aplicaciones ASP.NET Core publicadas

Por [Luke Latham](https://github.com/guardrex)

En el núcleo de ASP.NET, el directorio de la aplicación, *publicar*, consta de los archivos de la aplicación, archivos de configuración, activos estáticos, paquetes y el tiempo de ejecución (para aplicaciones independientes). Se trata de la misma estructura de directorios que en versiones anteriores de ASP.NET, donde toda la aplicación reside en el directorio raíz de web.

| Tipo de aplicación | Estructura de directorios |
| --- | --- |
| Implementación del marco de trabajo dependiente | <ul><li>publicar\*<ul><li>registros de\* (si se incluye en publishOptions)</li><li>Refs\*</li><li>tiempos de ejecución\*</li><li>Vistas\* (si se incluye en publishOptions)</li><li>wwwroot\* (si se incluye en publishOptions)</li><li>archivos .dll</li><li>MyApp.Deps.JSON</li><li>MyApp.dll</li><li>MyApp.pdb</li><li>MyApp. PrecompiledViews.dll (si precompilar vistas Razor)</li><li>MyApp. PrecompiledViews.pdb (si precompilar vistas Razor)</li><li>MyApp.runtimeconfig.JSON</li><li>Web.config (si se incluye en publishOptions)</li></ul></li></ul> |
| Implementación independiente | <ul><li>publicar\*<ul><li>registros de\* (si se incluye en publishOptions)</li><li>Refs\*</li><li>Vistas\* (si se incluye en publishOptions)</li><li>wwwroot\* (si se incluye en publishOptions)</li><li>archivos .dll</li><li>MyApp.Deps.JSON</li><li>MyApp.exe</li><li>MyApp.pdb</li><li>MyApp. PrecompiledViews.dll (si precompilar vistas Razor)</li><li>MyApp. PrecompiledViews.pdb (si precompilar vistas Razor)</li><li>MyApp.runtimeconfig.JSON</li><li>Web.config (si se incluye en publishOptions)</li></ul></li></ul> |
\*Indica un directorio

El contenido de la *publicar* directorio representa el *ruta de acceso raíz del contenido*, también denominado el *ruta de acceso base de aplicación*, de la implementación. El nombre se asigna a la *publicar* su ubicación de directorio en la implementación, actúa como ruta de acceso física del servidor a la aplicación hospedada. El *wwwroot* directory, si está presente, sólo contiene recursos estáticos. El *registros* directorio puede incluirse en la implementación mediante la creación del proyecto y agregar la `<Target>` elemento se muestra a continuación para su *.csproj* archivo o creando físicamente el directorio en el servidor.

```xml
<Target Name="CreateLogsFolder" AfterTargets="Publish">
  <MakeDir Directories="$(PublishDir)Logs" 
           Condition="!Exists('$(PublishDir)Logs')" />
  <WriteLinesToFile File="$(PublishDir)Logs\.log" 
                    Lines="Generated file" 
                    Overwrite="True" 
                    Condition="!Exists('$(PublishDir)Logs\.log')" />
</Target>
```

El `<MakeDir>` elemento crea vacío *registros* carpeta en la salida publicada. El elemento utiliza el `PublishDir` propiedad para determinar la ubicación de destino para crear la carpeta. Varios métodos de implementación, por ejemplo, Web Deploy, omiten las carpetas vacías durante la implementación. El `<WriteLinesToFile>` elemento genera un archivo en el *registros* carpeta, lo que garantiza la implementación de la carpeta en el servidor. Tenga en cuenta que la creación de carpetas todavía puede fallar si el proceso de trabajo no tiene acceso de escritura a la carpeta de destino.

El directorio de implementación requiere permisos de lectura y ejecución, mientras que la *registros* directory requiere permisos de lectura/escritura. Directorios adicionales donde se escribirá activos requieren permisos de lectura/escritura.
