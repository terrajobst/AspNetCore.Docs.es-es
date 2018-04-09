---
title: Estructura de directorios de ASP.NET Core
author: guardrex
description: Ver la estructura de directorio de aplicaciones de ASP.NET Core publicadas.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/directory-structure
ms.openlocfilehash: 2a6ee4fefcc6d23b1c893a40b7b1be9edfcf9732
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-core-directory-structure"></a>Estructura de directorios de ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

En el núcleo de ASP.NET, el directorio de la aplicación, *publicar*, consta de los archivos de la aplicación, archivos de configuración, activos estáticos, paquetes y el tiempo de ejecución (para aplicaciones independientes).


|            Tipo de aplicación            |                                                                                                                                                                                                                                                     Estructura de directorios                                                                                                                                                                                                                                                      |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Implementación del marco de trabajo dependiente | <ul><li>publish\*<ul><li>registros de\* (si se incluye en publishOptions)</li><li>Refs\*</li><li>tiempos de ejecución\*</li><li>Vistas\* (si se incluye en publishOptions)</li><li>wwwroot\* (si se incluye en publishOptions)</li><li>archivos .dll</li><li>myapp.deps.json</li><li>MyApp.dll</li><li>MyApp.pdb</li><li>MyApp. PrecompiledViews.dll (si precompilar vistas Razor)</li><li>MyApp. PrecompiledViews.pdb (si precompilar vistas Razor)</li><li>myapp.runtimeconfig.json</li><li>Web.config (si se incluye en publishOptions)</li></ul></li></ul> |
|   Implementación independiente    |          <ul><li>publish\*<ul><li>registros de\* (si se incluye en publishOptions)</li><li>Refs\*</li><li>Vistas\* (si se incluye en publishOptions)</li><li>wwwroot\* (si se incluye en publishOptions)</li><li>archivos .dll</li><li>myapp.deps.json</li><li>MyApp.exe</li><li>MyApp.pdb</li><li>MyApp. PrecompiledViews.dll (si precompilar vistas Razor)</li><li>MyApp. PrecompiledViews.pdb (si precompilar vistas Razor)</li><li>myapp.runtimeconfig.json</li><li>Web.config (si se incluye en publishOptions)</li></ul></li></ul>           |

\* Indica un directorio

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
