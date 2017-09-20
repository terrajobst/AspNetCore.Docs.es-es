---
title: "Información general de hospedaje e implementación - ASP.NET Core"
author: tdykstra
description: "Información general sobre cómo configurar entornos de hospedaje e implementar en ellos aplicaciones de ASP.NET Core."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: f0930c68-4d17-4748-adbf-801e17601eb6
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/index
ms.openlocfilehash: df3c1f0c2768b89c3ea5dc901782170c530a542e
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2017
---
# <a name="hosting-and-deployment-overview-for-aspnet-core-apps"></a>Información general de hospedaje e implementación para aplicaciones ASP.NET Core

A continuación se describen los pasos principales necesarios para implementar una aplicación de ASP.NET Core en un entorno de hospedaje:

* Publique la aplicación en una carpeta del servidor de hospedaje.
* Configure un administrador de procesos que inicie la aplicación cuando lleguen las solicitudes y la reinicie si se bloquea o si se reinicia el servidor.
* Configure un proxy inverso que reenvíe las solicitudes a la aplicación.

## <a name="publish-to-a-folder"></a>Publicar la aplicación en una carpeta 

El comando de la CLI [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) compila el código de la aplicación y copia los archivos necesarios para ejecutar la aplicación en una carpeta *publish*. Al efectuar una implementación desde Visual Studio, el paso `dotnet publish` se lleva a cabo de forma automática antes de que se copien los archivos en el destino de implementación.

### <a name="folder-contents"></a>Contenido de la carpeta

La carpeta *publish* contiene archivos *.exe* y *.dll* de la aplicación, sus dependencias y, de forma opcional, el tiempo de ejecución .NET.

Se puede publicar una aplicación .NET Core como *independiente* o *dependiente del marco*. Si la aplicación es independiente, los archivos *.dll* que contienen el tiempo de ejecución .NET se incluyen en la carpeta *publish*.  Si la aplicación es dependiente del marco, los archivos del tiempo de ejecución .NET no se incluyen porque la aplicación tiene una referencia a una versión de .NET que está instalada en el equipo. El modelo de implementación predeterminado es dependiente del marco. Para más información, vea [Implementación de aplicaciones .NET Core](https://docs.microsoft.com/dotnet/articles/core/deploying/index).

Además de los archivos *.exe* y *.dll*, la carpeta *publish* de una aplicación ASP.NET Core suele contener archivos de configuración, recursos estáticos y vistas de MVC.  Para más información, vea [Directory structure](xref:hosting/directory-structure) (Estructura de directorios).

## <a name="set-up-a-process-manager"></a>Configurar un administrador de procesos

Una aplicación ASP.NET Core es una aplicación de consola que se debe iniciar cuando se inicia un servidor y se debe reiniciar cuando este se bloquea. Para automatizar los inicios y los reinicios, necesita un administrador de procesos. Los administradores de procesos más comunes para ASP.NET Core son [Nginx](xref:publishing/linuxproduction) y [Apache](xref:publishing/apache-proxy) en Linux, e [IIS](xref:publishing/iis) y [Servicio de Windows](xref:hosting/windows-service) en Windows.

## <a name="set-up-a-reverse-proxy"></a>Configurar un proxy inverso

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Si su aplicación usa el servidor web [Kestrel](xref:fundamentals/servers/kestrel), puede usar [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy) o [IIS](xref:publishing/iis) como servidor proxy inverso. Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel después de un control preliminar. Para más información, vea [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#when-to-use-kestrel-with-a-reverse-proxy) (Cuándo se debe usar Kestrel con un proxy inverso).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Si su aplicación usa el servidor web [Kestrel](xref:fundamentals/servers/kestrel) y se verá expuesto a Internet, debe usar [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy) o [IIS](xref:publishing/iis) como servidor proxy inverso. Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel después de un control preliminar. La razón principal por la que debe usar un proxy inverso es la seguridad. Para más información, vea [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy) (Cuándo se debe usar Kestrel con un proxy inverso).

---

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a>Usar Visual Studio y MSBuild para automatizar la implementación

La implementación a menudo requiere tareas adicionales además de copiar el resultado de `dotnet publish` a un servidor. Por ejemplo, puede que quiera incluir más archivos en la carpeta *publish* (o excluir archivos de esta). Visual Studio usa MSBuild para la implementación web; puede personalizar MSBuild para llevar a cabo muchas otras tareas durante la implementación. Para más información, vea [Publish profiles in Visual Studio](xref:publishing/web-publishing-vs) (Publicar perfiles en Visual Studio) y el libro [Using MSBuild and Team Foundation Build](http://msbuildbook.com/) (Usar MSBuild y Team Foundation Build).

Puede efectuar una implementación directamente desde Visual Studio a Azure App Service mediante la [característica de publicación web](xref:tutorials/publish-to-azure-webapp-using-vs) o mediante la [compatibilidad integrada con Git](xref:publishing/azure-continuous-deployment). Visual Studio Team Services es compatible con la [implementación continua en Azure App Service](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure).

## <a name="additional-resources"></a>Recursos adicionales

Para información sobre cómo usar Docker como entorno de hospedaje, vea [Host ASP.NET Core apps in Docker](xref:publishing/docker) (Hospedar aplicaciones ASP.NET Core en Docker).
