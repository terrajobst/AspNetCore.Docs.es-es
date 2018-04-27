---
title: Estructura de directorios de ASP.NET Core
author: guardrex
description: Obtenga información acerca de la estructura de directorio de aplicaciones de ASP.NET Core publicadas.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/directory-structure
ms.openlocfilehash: ac9b777bcc7f4a8634161fc1347a4d0fdc3b4784
ms.sourcegitcommit: 7c8fd9b7445cd77eb7f7d774bfd120c26f3b5d84
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/19/2018
---
# <a name="aspnet-core-directory-structure"></a>Estructura de directorios de ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

En el núcleo de ASP.NET, el directorio de la aplicación publicada, *publicar*, consta de los archivos de la aplicación, archivos de configuración, activos estáticos, paquetes y el tiempo de ejecución (para [implementaciones independientes](/dotnet/core/deploying/#self-contained-deployments-scd)).


| Tipo de aplicación | Estructura de directorios |
| -------- | ------------------- |
| [Implementación del marco de trabajo dependiente](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li>Publicar&dagger;<ul><li>registros&dagger; (opcional, a menos que se requiere para recibir registros stdout)</li><li>Vistas&dagger; (aplicaciones de MVC; si no se precompilan vistas)</li><li>Páginas&dagger; (MVC o las páginas de Razor aplicaciones; si no se precompilan páginas)</li><li>wwwroot&dagger;</li><li>*\.archivos DLL</li><li>\<nombre del ensamblado >. deps.json</li><li>\<nombre del ensamblado > .dll</li><li>\<nombre del ensamblado > .pdb</li><li>\<nombre del ensamblado >. PrecompiledViews.dll</li><li>\<nombre del ensamblado >. PrecompiledViews.pdb</li><li>\<nombre del ensamblado >. runtimeconfig.json</li><li>Web.config (implementaciones de IIS)</li></ul></li></ul> |
| [Implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>Publicar&dagger;<ul><li>registros&dagger; (opcional, a menos que se requiere para recibir registros stdout)</li><li>Refs&dagger;</li><li>Vistas&dagger; (aplicaciones de MVC; si no se precompilan vistas)</li><li>Páginas&dagger; (MVC o las páginas de Razor aplicaciones; si no se precompilan páginas)</li><li>wwwroot&dagger;</li><li>\*archivos .dll</li><li>\<nombre del ensamblado >. deps.json</li><li>\<nombre del ensamblado > .exe</li><li>\<nombre del ensamblado > .pdb</li><li>\<nombre del ensamblado >. PrecompiledViews.dll</li><li>\<nombre del ensamblado >. PrecompiledViews.pdb</li><li>\<nombre del ensamblado >. runtimeconfig.json</li><li>Web.config (implementaciones de IIS)</li></ul></li></ul> |

&dagger;Indica un directorio

El *publicar* directorio representa el *ruta de acceso raíz del contenido*, también denominado el *ruta de acceso base de aplicación*, de la implementación. El nombre se asigna a la *publicar* su ubicación de directorio de la aplicación implementada en el servidor, actúa como ruta de acceso física del servidor para la aplicación hospedada.

El *wwwroot* directory, si está presente, sólo contiene recursos estáticos.

El stdout *registros* directorio puede crearse la implementación mediante uno de los dos métodos siguientes:

* Agregue las siguientes `<Target>` elemento al archivo del proyecto:

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

* Crear físicamente la *registros* directorio en el servidor de la implementación.

El directorio de implementación requiere permisos de lectura y ejecución. El *registros* directory requiere permisos de lectura/escritura. Directorios adicionales donde se escriben los archivos requieren permisos de lectura/escritura.
