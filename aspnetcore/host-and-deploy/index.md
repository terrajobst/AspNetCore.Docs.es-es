---
title: "Hospedaje e implementación de ASP.NET Core"
author: tdykstra
description: "Obtenga información sobre cómo configurar entornos de hospedaje e implementar aplicaciones de ASP.NET Core."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 08/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/index
ms.openlocfilehash: 6ce77922dd8a0fcb81ea6a72f9179c9c81105dda
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="host-and-deploy-aspnet-core"></a>Hospedaje e implementación de ASP.NET Core

En general, para implementar una aplicación de ASP.NET Core en un entorno de hospedaje:

* Publique la aplicación en una carpeta del servidor de hospedaje.
* Configure un administrador de procesos que inicie la aplicación cuando lleguen las solicitudes y la reinicie si se bloquea o si se reinicia el servidor.
* Configure un proxy inverso que reenvíe las solicitudes a la aplicación.

## <a name="publish-to-a-folder"></a>Publicar la aplicación en una carpeta 

El comando de la CLI [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) compila el código de la aplicación y copia los archivos necesarios para ejecutar la aplicación en una carpeta *publish*. Al efectuar una implementación desde Visual Studio, el paso `dotnet publish` se lleva a cabo de forma automática antes de que se copien los archivos en el destino de implementación.

### <a name="folder-contents"></a>Contenido de la carpeta

La carpeta *publish* contiene archivos *.exe* y *.dll* de la aplicación, sus dependencias y, de forma opcional, el tiempo de ejecución .NET.

Se puede publicar una aplicación .NET Core como *independiente* o *dependiente del marco*. Si la aplicación es independiente, los archivos *.dll* que contienen el tiempo de ejecución .NET se incluyen en la carpeta *publish*. Si la aplicación es dependiente del marco, los archivos del tiempo de ejecución .NET no se incluyen porque la aplicación tiene una referencia a una versión de .NET que está instalada en el servidor. El modelo de implementación predeterminado es dependiente del marco. Para más información, vea [Implementación de aplicaciones .NET Core](/dotnet/articles/core/deploying/index).

Además de los archivos *.exe* y *.dll*, la carpeta *publish* de una aplicación ASP.NET Core suele contener archivos de configuración, recursos estáticos y vistas de MVC. Para más información, vea [Directory structure](xref:host-and-deploy/directory-structure) (Estructura de directorios).

## <a name="set-up-a-process-manager"></a>Configurar un administrador de procesos

Una aplicación de ASP.NET Core es una aplicación de consola que se debe iniciar cuando se inicia un servidor y se debe reiniciar si este se bloquea. Para automatizar los inicios y los reinicios, se necesita un administrador de procesos. Los administradores de procesos más comunes para ASP.NET Core son:

* Linux
  * [Nginx](xref:host-and-deploy/linux-nginx)
  * [Apache](xref:host-and-deploy/linux-apache)
* Windows
  * [IIS](xref:host-and-deploy/iis/index)
  * [Servicio de Windows](xref:host-and-deploy/windows-service)

## <a name="set-up-a-reverse-proxy"></a>Configurar un proxy inverso

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Si la aplicación usa el servidor web [Kestrel](xref:fundamentals/servers/kestrel), puede usar [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache) o [IIS](xref:host-and-deploy/iis/index) como servidor proxy inverso. Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel después de un control preliminar. Para más información, vea [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#when-to-use-kestrel-with-a-reverse-proxy) (Cuándo se debe usar Kestrel con un proxy inverso).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Si la aplicación usa el servidor web [Kestrel](xref:fundamentals/servers/kestrel) y se verá expuesto a Internet, debe usar [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache) o [IIS](xref:host-and-deploy/iis/index) como servidor proxy inverso. Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel después de un control preliminar. La razón principal por la que debe usar un proxy inverso es la seguridad. Para más información, vea [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy) (Cuándo se debe usar Kestrel con un proxy inverso).

---

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a>Usar Visual Studio y MSBuild para automatizar la implementación

La implementación a menudo requiere tareas adicionales además de copiar el resultado de `dotnet publish` a un servidor. Por ejemplo, podrían necesitarse o eliminarse archivos adicionales de la carpeta *publish*. Para la implementación web, Visual Studio usa MSBuild, que puede personalizar de modo que lleve a cabo muchas otras tareas durante la implementación. Para más información, vea [Publish profiles in Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) (Publicar perfiles en Visual Studio) y el libro [Using MSBuild and Team Foundation Build](http://msbuildbook.com/) (Usar MSBuild y Team Foundation Build).

Mediante la [característica de publicación web](xref:tutorials/publish-to-azure-webapp-using-vs) o la [compatibilidad integrada con Git](xref:host-and-deploy/azure-apps/azure-continuous-deployment), puede implementar las aplicaciones directamente desde Visual Studio en Azure App Service. Visual Studio Team Services es compatible con la [implementación continua en Azure App Service](/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?tabs=vsts).

## <a name="publishing-to-azure"></a>Publicación en Azure

Vea [Publicar una aplicación web de ASP.NET Core en Azure App Service con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) para obtener instrucciones sobre cómo publicar una aplicación en Azure con Visual Studio. También se puede publicar la aplicación en Azure desde la [línea de comandos](xref:tutorials/publish-to-azure-webapp-using-cli).

## <a name="additional-resources"></a>Recursos adicionales

Para información sobre cómo usar Docker como entorno de hospedaje, vea [Host ASP.NET Core apps in Docker](xref:host-and-deploy/docker/index) (Hospedar aplicaciones de ASP.NET Core en Docker).
