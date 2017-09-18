---
title: "Introducción a ASP.NET Core MVC y Visual Studio para Mac"
author: rick-anderson
description: "Introducción a ASP.NET Core MVC y Visual Studio"
keywords: ASP.NET Core,MVC,Visual Studio para Mac,Entity Framework
ms.author: riande
manager: wpickett
ms.date: 8/23/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 028d6d3246908a9cd44a6834449d2fdbc9cae0b8
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a>Introducción a ASP.NET Core MVC y Visual Studio para Mac

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En este tutorial aprenderá los aspectos básicos de la creación de una aplicación web de ASP.NET Core MVC con [Visual Studio para Mac](https://www.visualstudio.com/vs/visual-studio-mac/). [!INCLUDE[consider RP](../../includes/razor.md)]

Hay tres versiones de este tutorial:

* macOS: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* Linux, macOS y Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="prerequisites"></a>Requisitos previos

Para realizar este tutorial se necesita el [SDK de .NET Core 2.0.0](https://www.microsoft.com/net/core) o una versión posterior. Vea [este PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) para la versión 1.1 de ASP.NET Core.

Instale el software siguiente:

- [SDK de .NET Core 2.0.0](https://www.microsoft.com/net/core) o versiones posteriores
- [Visual Studio para Mac](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-web-app"></a>Creación de una aplicación web

Desde Visual Studio, seleccione **Archivo > Nueva solución**.

![macOS: Nueva solución](../first-web-api-mac/_static/sln.png)

Seleccione **Aplicación .NET Core > ASP.NET Core > Aplicación web > Siguiente**.

![macOS: Cuadro de diálogo Nuevo proyecto](start-mvc/1.png)

Asigne el nombre **MvcMovie** al proyecto y, después, seleccione **Crear**.

![macOS: Cuadro de diálogo Nuevo proyecto](start-mvc/2.png)

### <a name="launch-the-app"></a>Iniciar la aplicación

En Visual Studio, seleccione **Ejecutar > Iniciar sin depurar** para iniciar la aplicación. Visual Studio inicia [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), inicia un explorador y navega a `http://localhost:port`, donde *port* es un número de puerto elegido aleatoriamente.

![Explorador con el nuevo proyecto](start-mvc/b1.png)

* En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`). Esto es así porque `localhost` es el nombre de host estándar del equipo local. Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web. Al ejecutar la aplicación verá otro puerto distinto.
* Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el menú **Ejecutar**.

La plantilla predeterminada proporciona los vínculos **Inicio, Acerca de** y **Contacto**. En la imagen del explorador anterior no se muestran estos vínculos. Según el tamaño del explorador, tendrá que hacer clic en el icono de navegación para que se muestren.

![Explorador con el nuevo proyecto](start-mvc/b2.png)

En la siguiente sección de este tutorial conocerá MVC y empezará a escribir código.

>[!div class="step-by-step"]
[Siguiente](adding-controller.md)  
