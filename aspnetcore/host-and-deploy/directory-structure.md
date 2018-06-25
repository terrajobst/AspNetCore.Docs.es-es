---
title: Estructura de directorios de ASP.NET Core
author: guardrex
description: Obtenga información sobre la estructura de directorios de las aplicaciones ASP.NET Core publicadas.
ms.author: riande
ms.custom: mvc
ms.date: 04/09/2018
uid: host-and-deploy/directory-structure
ms.openlocfilehash: 06d3f097cd93ceb2a23b9f6516a9b7a1f3ca3089
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273678"
---
# <a name="aspnet-core-directory-structure"></a>Estructura de directorios de ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

En ASP.NET Core, el directorio de aplicaciones publicadas, *publish*, consta de archivos de aplicación, archivos de configuración, recursos estáticos, paquetes y el entorno de tiempo de ejecución (para las [implementaciones independientes](/dotnet/core/deploying/#self-contained-deployments-scd)).


| Tipo de aplicación | Estructura de directorios |
| -------- | ------------------- |
| [Implementación dependiente de marco](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li>publish&dagger;<ul><li>logs&dagger; (opcional a menos que sea necesario para recibir registros de stdout)</li><li>Views&dagger; (aplicaciones MVC; si las vistas no están precompiladas)</li><li>Pages&dagger; (aplicaciones MVC o de páginas Razor; si las páginas no están precompiladas)</li><li>wwwroot&dagger;</li><li>archivos *\.dll</li><li>\<nombre del ensamblado>.deps.json</li><li>\<nombre del ensamblado>.dll</li><li>\<nombre del ensamblado>.pdb</li><li>\<nombre del ensamblado>.PrecompiledViews.dll</li><li>\<nombre del ensamblado>.PrecompiledViews.pdb</li><li>\<nombre del ensamblado>.runtimeconfig.json</li><li>web.config (implementaciones de IIS)</li></ul></li></ul> |
| [Implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>publish&dagger;<ul><li>logs&dagger; (opcional a menos que sea necesario para recibir registros de stdout)</li><li>refs&dagger;</li><li>Views&dagger; (aplicaciones MVC; si las vistas no están precompiladas)</li><li>Pages&dagger; (aplicaciones MVC o de páginas Razor; si las páginas no están precompiladas)</li><li>wwwroot&dagger;</li><li>archivos \*.dll</li><li>\<nombre del ensamblado>.deps.json</li><li>\<nombre del ensamblado>.exe</li><li>\<nombre del ensamblado>.pdb</li><li>\<nombre del ensamblado>.PrecompiledViews.dll</li><li>\<nombre del ensamblado>.PrecompiledViews.pdb</li><li>\<nombre del ensamblado>.runtimeconfig.json</li><li>web.config (implementaciones de IIS)</li></ul></li></ul> |

&dagger;Indica un directorio

El directorio *publish* representa la *ruta de acceso raíz del contenido*, también conocida como la *ruta de acceso base de aplicación*, de la implementación. Sea cual sea el nombre que se asigna al directorio *publish* de la aplicación implementada en el servidor, su ubicación funciona como la ruta física del servidor a la aplicación hospedada.

El directorio *wwwroot*, si existe, solo contiene recursos estáticos.

El directorio *registros* de stdout se puede crear para la implementación mediante una de las dos estrategias siguientes:

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

   El elemento `<MakeDir>` crea una carpeta *Logs* vacía en la salida publicada. El elemento usa la propiedad `PublishDir` para determinar la ubicación de destino para la creación de la carpeta. Varios métodos de implementación, como Web Deploy, omiten las carpetas vacías durante la implementación. El elemento `<WriteLinesToFile>` genera un archivo en la carpeta *Logs*, que garantiza la implementación de la carpeta en el servidor. Tenga en cuenta que puede producirse un error en la creación de carpetas si el proceso de trabajo no tiene acceso de escritura a la carpeta de destino.

* Cree físicamente el directorio *Logs* en el servidor de la implementación.

El directorio de implementación requiere permisos de lectura y ejecución. El directorio *Logs* requiere permisos de lectura y escritura. Otros directorios donde se escriben los archivos requieren permisos de lectura y escritura.
