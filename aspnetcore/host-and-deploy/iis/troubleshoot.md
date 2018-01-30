---
title: "Solucionar problemas de núcleo de ASP.NET en IIS"
author: guardrex
description: "Obtenga información acerca de cómo diagnosticar problemas con las implementaciones de IIS de las aplicaciones de ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: c68070a9cba5667504d1ad4927b02b73f83e6573
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>Solucionar problemas de núcleo de ASP.NET en IIS

Por [Luke Latham](https://github.com/guardrex)

Para diagnosticar problemas con las implementaciones de IIS:

* Estudie la salida del explorador.
* Examine el registro de **aplicaciones** del sistema mediante el **Visor de eventos**.
* Habilite el registro `stdout`. El registro del **módulo ASP.NET Core** se encuentra en la ruta de acceso proporcionada en el atributo *stdoutLogFile* del elemento `<aspNetCore>` de *web.config*. Las carpetas de la ruta de acceso proporcionada en el valor del atributo deben existir en la implementación. Establecer *stdoutLogEnabled* a `true`. Aplicaciones que usan el el `Microsoft.NET.Sdk.Web` SDK para crear el *web.config* archivos de forma predeterminada el *stdoutLogEnabled* si se establece en `false`, manualmente proporcionan la *web.config* de archivos o modificar el archivo con el fin de habilitar `stdout` registro.

Utilizar la información de los tres orígenes con el [tema de referencia de errores comunes](xref:host-and-deploy/azure-iis-errors-reference) para determinar el problema. Siga los consejos para solucionar problemas que se proporciona para resolver el problema.

Algunos de los errores comunes no aparecen en el explorador, el registro de aplicaciones ni el registro del módulo ASP.NET Core hasta que se ha pasado el módulo *startupTimeLimit* (valor predeterminado: 120 segundos) y *startupRetryCount* (valor predeterminado: 2). Por lo tanto, espere seis minutos completos antes de deducir que el módulo no ha iniciado un proceso de la aplicación.

Una manera rápida de determinar si la aplicación funciona correctamente es ejecutarla directamente en Kestrel. Si la aplicación se publicó como un [framework dependiente implementación](/dotnet/core/deploying/#framework-dependent-deployments-fdd), ejecute `dotnet <assembly_name>.dll` en la carpeta de implementación, que es la ruta de acceso física de IIS para la aplicación. Si la aplicación se publicó como un [implementación independiente](/dotnet/core/deploying/#self-contained-deployments-scd), ejecute la aplicación del ejecutable directamente desde un símbolo del sistema, `<assembly_name>.exe`, en la carpeta de implementación. Si Kestrel está escuchando en el puerto 5000 de forma predeterminada, la aplicación debe estar disponible en `http://localhost:5000/`. Si la aplicación responde normalmente en la dirección del extremo Kestrel, el problema es más probable relacionadas con la configuración de proxy inverso y menos probable que dentro de la aplicación.

Una manera de determinar si el proxy inverso funciona correctamente es realizar una solicitud de archivo estático simple para una hoja de estilos, script o imagen de archivos estáticos de la aplicación en *wwwroot* con [Middleware de archivos estáticos](xref:fundamentals/static-files). Si la aplicación puede servir archivos estáticos, pero se producen errores en las vistas de MVC y otros puntos de conexión, el problema es más probable que dentro de la aplicación (por ejemplo, el enrutamiento MVC o 500 Error interno del servidor) y menos probable que relacionados con la configuración de proxy inverso.

Cuando Kestrel inicia normalmente detrás de IIS, pero la aplicación no se ejecutará en el sistema después de ejecutar satisfactoriamente localmente, se puede agregar una variable de entorno temporalmente a *web.config* para establecer el `ASPNETCORE_ENVIRONMENT` a `Development`. Siempre y cuando el entorno no se reemplaza en el inicio de la aplicación, establecer la variable de entorno permite la [página de excepción para desarrolladores](xref:fundamentals/error-handling) aparecen cuando la aplicación se ejecute. Establecer la variable de entorno para `ASPNETCORE_ENVIRONMENT` de este modo se recomienda únicamente para servidores de ensayo o pruebas que no están expuestos a Internet. No olvide quitar la variable de entorno desde el *web.config* cuando termine de archivos. Para obtener información acerca de cómo establecer variables de entorno mediante *web.config*, consulte [environmentVariables elemento secundario aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

En la mayoría de los casos, la habilitación del registro de la aplicación ayuda a solucionar problemas con el proxy inverso o la aplicación. Vea [Logging](xref:fundamentals/logging/index) (Registro) para más información.

La última sugerencia de solución de problemas pertenece a las aplicaciones que no se ejecutan después de actualizar el SDK de .NET Core en las versiones de paquete o la máquina de desarrollo dentro de la aplicación. En algunos casos, los paquetes incoherentes pueden interrumpir una aplicación al realizar actualizaciones importantes. La mayoría de estos problemas se puede corregir con:

* Eliminar el `bin` y `obj` carpetas en el proyecto.
* Paquete de borrar almacena en caché en `%UserProfile%\.nuget\packages\` y `%LocalAppData%\Nuget\v3-cache`.
* La restauración y volver a generar el proyecto.
* Confirmar que la implementación anterior en el servidor se ha eliminado por completo antes de volver a implementar la aplicación.

> [!TIP]
> Una manera cómoda para borrar las cachés del paquete es ejecutar `dotnet nuget locals all --clear` desde un símbolo del sistema.
> 
> Borrar las memorias caché del paquete también se puede lograr mediante el uso de la [nuget.exe](https://www.nuget.org/downloads) herramienta y ejecutar el comando `nuget locals all -clear`. *NuGet.exe* no es una instalación incluida con Windows 10 y debe obtenerse por separado desde el sitio Web de NuGet.
<!--
> [!TIP]
> A convenient way to clear package caches is to:
>
> * Obtain the *NuGet.exe* tool from [NuGet.org](https://www.nuget.org/).
> * Add the path to *NuGet.exe* to the system PATH.
> * Execute `nuget locals all -clear` from a command prompt.
>
> Alternatively, execute `dotnet nuget locals all --clear` from a command prompt without obtaining *NuGet.exe*. -->

## <a name="additional-resources"></a>Recursos adicionales

* [Referencia de errores comunes de Azure App Service e IIS con ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Referencia de configuración del módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
