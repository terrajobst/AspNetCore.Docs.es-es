---
title: Estructura de directorios de ASP.NET Core
author: guardrex
description: Obtenga información sobre la estructura de directorios de las aplicaciones ASP.NET Core publicadas.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 06/17/2019
uid: host-and-deploy/directory-structure
ms.openlocfilehash: f1df047decc7a0a6b7dcee57a690c55eea428b05
ms.sourcegitcommit: 28a2874765cefe9eaa068dceb989a978ba2096aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "67166981"
---
# <a name="aspnet-core-directory-structure"></a>Estructura de directorios de ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

El directorio *publish* contiene recursos de la aplicación producidos por el comando [dotnet publish](/dotnet/core/tools/dotnet-publish) que se pueden implementar. El directorio contiene lo siguiente:

* Archivos de aplicación
* Archivos de configuración
* Recursos estáticos
* Paquetes
* Entorno de ejecución (solo [implementación autocontenida](/dotnet/core/deploying/#self-contained-deployments-scd))

| Tipo de aplicación | Estructura de directorios |
| -------- | ------------------- |
| [Implementación dependiente de marco](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li>publish&dagger;<ul><li>Views&dagger; (aplicaciones MVC; si las vistas no están precompiladas)</li><li>Pages&dagger; (aplicaciones MVC o de páginas Razor; si las páginas no están precompiladas)</li><li>wwwroot&dagger;</li><li>archivos *\.dll</li><li>{NOMBRE DE ENSAMBLADO}.deps.json</li><li>{NOMBRE DE ENSAMBLADO}.dll</li><li>{NOMBRE DE ENSAMBLADO}.pdb</li><li>{NOMBRE DE ENSAMBLADO}.Views.dll</li><li>{NOMBRE DE ENSAMBLADO}.Views.pdb</li><li>{NOMBRE DE ENSAMBLADO}.runtimeconfig.json</li><li>web.config (implementaciones de IIS)</li></ul></li></ul> |
| [Implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>publish&dagger;<ul><li>Views&dagger; (aplicaciones MVC; si las vistas no están precompiladas)</li><li>Pages&dagger; (aplicaciones MVC o de páginas Razor; si las páginas no están precompiladas)</li><li>wwwroot&dagger;</li><li>archivos \*.dll</li><li>{NOMBRE DE ENSAMBLADO}.deps.json</li><li>{NOMBRE DE ENSAMBLADO}.dll</li><li>{NOMBRE DE ENSAMBLADO}.exe</li><li>{NOMBRE DE ENSAMBLADO}.pdb</li><li>{NOMBRE DE ENSAMBLADO}.Views.dll</li><li>{NOMBRE DE ENSAMBLADO}.Views.pdb</li><li>{NOMBRE DE ENSAMBLADO}.runtimeconfig.json</li><li>web.config (implementaciones de IIS)</li></ul></li></ul> |

&dagger;Indica un directorio

El directorio *publish* representa la *ruta de acceso raíz del contenido*, también conocida como la *ruta de acceso base de aplicación*, de la implementación. Sea cual sea el nombre que se asigna al directorio *publish* de la aplicación implementada en el servidor, su ubicación funciona como la ruta física del servidor a la aplicación hospedada.

El directorio *wwwroot*, si existe, solo contiene recursos estáticos.

::: moniker range="< aspnetcore-3.0"

Crear una carpeta *Logs* es útil para el [registro de depuración mejorado del módulo de ASP.NET Core](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs). El módulo no crea automáticamente las carpetas de la ruta de acceso proporcionada al valor `<handlerSetting>`, que deben existir previamente en la implementación para permitir que el módulo escriba el registro de depuración.

Se puede crear un directorio *Logs* para la implementación mediante uno de los dos enfoques siguientes:

* Agregue el siguiente elemento `<Target>` al archivo del proyecto:

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

   El elemento `<MakeDir>` crea una carpeta *Logs* vacía en la salida publicada. El elemento usa la propiedad `PublishDir` para determinar la ubicación de destino para la creación de la carpeta. Varios métodos de implementación, como Web Deploy, omiten las carpetas vacías durante la implementación. El elemento `<WriteLinesToFile>` genera un archivo en la carpeta *Logs*, que garantiza la implementación de la carpeta en el servidor. Puede producirse un error en la creación de carpetas si el proceso de trabajo no tiene acceso de escritura a la carpeta de destino.

* Cree físicamente el directorio *Logs* en el servidor de la implementación.

El directorio de implementación requiere permisos de lectura y ejecución. El directorio *Logs* requiere permisos de lectura y escritura. Otros directorios donde se escriben los archivos requieren permisos de lectura y escritura.

::: moniker-end

## <a name="additional-resources"></a>Recursos adicionales

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* [Implementación de aplicaciones .NET Core](/dotnet/core/deploying/)
* [Marcos de trabajo de destino](/dotnet/standard/frameworks)
* [Catálogo de identificadores de entorno de ejecución (RID) de .NET Core](/dotnet/core/rid-catalog)
