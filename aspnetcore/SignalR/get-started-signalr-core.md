---
title: Empezar a trabajar con SignalR en ASP.NET Core
author: rachelappel
description: "Obtenga información acerca de los conceptos básicos de la creación de una aplicación en tiempo real con SignalR para ASP.NET Core."
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/06/2018
ms.prod: aspnet-core
ms.technology: dotnet-signalr
ms.topic: tutorial
uid: signalr/get-started-signalr-core
ms.openlocfilehash: 79af59fc8c2ada71d764ada95a431e10f4f00f27
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a>Tutorial: Introducción a SignalR para ASP.NET Core

Por [Rachel Appel](https://twitter.com/rachelappel)

Este tutorial le enseña los fundamentos de la creación de una aplicación en tiempo real con SignalR para ASP.NET Core.

   ![Soluciones](get-started-signalr-core/_static/signalr-get-started-finished.png)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

Este tutorial muestra las tareas de desarrollo de SignalR siguientes:

> [!div class="checklist"]
> * Crear una aplicación web de ASP.NET Core.
> * Cree un concentrador de SignalR para insertar contenido en los clientes.
> * Utilice la biblioteca de SignalR JavaScript para enviar mensajes y muestra las actualizaciones desde el concentrador.

## <a name="prerequisites"></a>Requisitos previos

Instalar el software siguiente:

* [Obtener una vista previa en .NET core 2.1.0 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) o posterior
* [Visual Studio de 2017](https://www.visualstudio.com/downloads/) versión 15.6 o posterior con la carga de trabajo de desarrollo web y ASP.NET
* [npm](https://www.npmjs.com/get-npm)

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Cree un proyecto de ASP.NET Core que hospeda el servidor y cliente de SignalR

1. Use la **archivo** > **nuevo proyecto** menú opción y elija **aplicación Web de ASP.NET Core**. Dé un nombre al proyecto `SignalRChat`.

  ![Cuadro de diálogo nuevo proyecto en Visual Studio](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. Seleccione **aplicación Web** para crear un proyecto con páginas de Razor. A continuación, seleccione **Aceptar**. Asegúrese de que **ASP.NET Core 2.1** está seleccionada en el selector de framework, aunque se ejecute SignalR en versiones anteriores de .NET.

  ![Cuadro de diálogo nuevo proyecto en Visual Studio](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

  Las bibliotecas que hospedan SignalR código del lado servidor se incluyen en la plantilla de proyecto. Instalar el código de JavaScript de cliente por separado con [npm](https://www.npmjs.com/).

  ```console
   npm install @aspnet/signalr
  ```

3. Copia la *signalr.js* de *node_modules\\ @aspnet\signalr\dist\browser*  a la *wwwroot\lib* carpeta del proyecto.

## <a name="create-the-signalr-hub"></a>Crear el concentrador de SignalR

Un concentrador es una clase que actúa como una canalización de alto nivel que permite al cliente y servidor llamar a métodos en entre sí.

1. Agregar una clase al proyecto eligiendo **archivo** > **New** > **archivo** y seleccionando **clase de Visual C#**. 

1. Heredar de `Microsoft.AspNetCore.SignalR.Hub`. La `Hub` clase contiene propiedades y eventos para administrar las conexiones y grupos, así como enviar y recibir datos.

1. Crear el `Send` método que envía un mensaje a todos los clientes de chat conectado. Tenga en cuenta devuelve un `Task`, porque SignalR es asincrónica. Código asincrónico se escala mejor.

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

## <a name="configure-the-project-to-use-signalr"></a>Configurar el proyecto para que use SignalR

El servidor de SignalR debe configurarse para que sepa para pasar las solicitudes para SignalR.

1. Para configurar un proyecto de SignalR, modifique la `ConfigureServices` método de la aplicación `Startup` clase mediante la inserción de una llamada a `services.AddSignalR`.

  `services.AddSignalR` Agrega SignalR como parte de la [middleware de ASP.NET Core](xref:fundamentals/middleware/index) canalización.

1. Configurar las rutas a los concentradores con `UseSignalR`.

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a>Crear el código de cliente de SignalR

1. Reemplace el contenido de *Pages\Index.cshtml* con el código siguiente:

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  El código HTML anterior muestra el nombre y campos del mensaje y un botón de envío. Tenga en cuenta las referencias de script en la parte inferior: una referencia a SignalR y *chat.js*.

1. Agregue un archivo de JavaScript a la *wwwroot\js* carpeta denominada *chat.js* y agregue el siguiente código en él:

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a>Ejecutar la aplicación

1. Seleccione **depurar** > **iniciar sin depurar** para iniciar un explorador y cargar el sitio Web de forma local. Copie la dirección URL de la barra de direcciones.

1. Abra otra instancia del explorador (cualquier explorador) y pegue la dirección URL en la barra de direcciones.

1. Elija cualquiera de estos exploradores, escriba un nombre y un mensaje y haga clic en el **enviar** botón. El nombre y el mensaje se muestran en ambas páginas al instante.

  ![Soluciones](get-started-signalr-core/_static/signalr-get-started-finished.png)
