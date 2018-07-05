---
title: Introducción a SignalR en ASP.NET Core
author: rachelappel
description: En este tutorial, creará una aplicación con SignalR para ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: ca9145d9e16c23e34bbc1d84ff01ce02709187ce
ms.sourcegitcommit: 08f1a9baa97060da5168840b332c9c0805b5f901
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2018
ms.locfileid: "37144877"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a>Introducción a SignalR en ASP.NET Core

Por [Rachel Appel](https://twitter.com/rachelappel)

Este tutorial le enseña los conceptos básicos de la creación de una aplicación en tiempo real con SignalR para ASP.NET Core.

   ![Soluciones](signalr/_static/signalr-get-started-finished.png)

En este tutorial se muestran las siguientes tareas de desarrollo de SignalR:

> [!div class="checklist"]
> * Crear una aplicación web de SignalR en ASP.NET Core.
> * Cree un concentrador de SignalR para insertar contenido en los clientes.
> * Modificar la clase `Startup` y configurar la aplicación.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Requisitos previos

Instale el software siguiente:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET Core SDK 2.1 o posterior](https://www.microsoft.com/net/download/all)
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) versión 15.7 o posterior con la carga de trabajo **ASP.NET y desarrollo web**
* [npm](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [.NET Core SDK 2.1 o posterior](https://www.microsoft.com/net/download/all)
* [Visual Studio Code](https://code.visualstudio.com/download)
* [C# para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Crear un proyecto de ASP.NET Core que hospede el servidor y cliente de SignalR

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Use la opción de menú **Archivo** > **Nuevo proyecto** y elija **Aplicación web ASP.NET Core**. Asigne al proyecto el nombre *SignalRChat*.

   ![Cuadro de diálogo Nuevo proyecto en Visual Studio](signalr/_static/signalr-new-project-dialog.png)

2. Seleccione **Aplicación web** para crear un proyecto con Razor Pages. Después, haga clic en **Aceptar**. Asegúrese de que **ASP.NET Core 2.1** está seleccionada en el selector de framework, aunque se ejecute SignalR en versiones anteriores de .NET.

   ![Cuadro de diálogo Nuevo proyecto en Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

Visual Studio incluye el paquete `Microsoft.AspNetCore.SignalR` que contiene sus bibliotecas de servidor como parte de la plantilla **Aplicación web ASP.NET Core**. Pero debe instalarse la biblioteca de cliente de JavaScript para SignalR con *npm*.

3. Ejecute los comandos siguientes en la ventana **Consola del Administrador de paquetes**, desde la raíz del proyecto:

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. Cree una carpeta denominada "signalr" dentro de la carpeta *lib* del proyecto. Copie el archivo *signalr.js* de *node_modules\\@aspnet\signalr\dist\browser*  a esta carpeta.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. En el **terminal integrado**, ejecute el siguiente comando:

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. Instale la biblioteca de cliente de JavaScript con *npm*.

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. Cree una carpeta denominada "signalr" dentro de la carpeta *lib* del proyecto. Copie el archivo *signalr.js* de *node_modules\\@aspnet\signalr\dist\browser*  a esta carpeta.

---

## <a name="create-the-signalr-hub"></a>Crear el concentrador SignalR

Un concentrador es una clase que actúa como una canalización de alto nivel que permite al cliente y el servidor llamar a métodos entre sí.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Agregue una clase al proyecto eligiendo **Archivo** > **Nuevo** > **Archivo** y seleccionando **Clase de Visual C#**. Asigne el nombre `ChatHub` a la clase y *ChatHub.cs* al archivo.

2. Heredan de `Microsoft.AspNetCore.SignalR.Hub`. La clase `Hub` contiene propiedades y eventos para administrar las conexiones y grupos, así como enviar y recibir datos.

3. Cree el método `SendMessage` que envía un mensaje a todos los clientes de chat conectados. Tenga en cuenta que devuelve una [Tarea](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), porque SignalR es asincrónica. El código asincrónico se escala mejor.

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. Abra la carpeta *SignalRChat* en Visual Studio Code.

2. Agregue una clase al proyecto seleccionando **Archivo** > **Nuevo archivo** en el menú. Asigne el nombre `ChatHub` a la clase y *ChatHub.cs* al archivo.

3. Heredan de `Microsoft.AspNetCore.SignalR.Hub`. La clase `Hub` contiene propiedades y eventos para administrar las conexiones y grupos, así como enviar y recibir datos de los clientes.

4. Agregue un método `SendMessage` a la clase. El método `SendMessage` envía un mensaje a todos los clientes de chat conectados. Tenga en cuenta que devuelve una [Tarea](/dotnet/api/system.threading.tasks.task), porque SignalR es asincrónica. El código asincrónico se escala mejor.

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a>Configurar el proyecto para que use SignalR

El servidor de SignalR debe configurarse para que sepa pasar las solicitudes a SignalR.

1. Para configurar un proyecto de SignalR, modifique el método `Startup.ConfigureServices` del proyecto.

   `services.AddSignalR` hace que los servicios SignalR estén disponibles para el sistema de [inserción de dependencias](xref:fundamentals/dependency-injection).

1. Configure rutas a las centrales con `UseSignalR` en el método `Configure`. `app.UseSignalR` agrega SignalR a la canalización de [middleware](xref:fundamentals/middleware/index).

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a>Crear el código de cliente de SignalR

1. Agregue un archivo de JavaScript denominado *chat.js*, a la carpeta *wwwroot\js*. Agregue el código siguiente a él:

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. Reemplace el contenido de *Pages/Index.cshtml* con el código siguiente:

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   El código HTML anterior muestra los campos de nombre y mensaje, y un botón de envío. Tenga en cuenta las referencias de script en la parte inferior: una referencia a SignalR y *chat.js*.

## <a name="run-the-app"></a>Ejecutar la aplicación

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Seleccione **Depurar** > **Iniciar sin depurar** para iniciar un explorador y cargar el sitio web de forma local. Copie la dirección URL de la barra de direcciones.

1. Abra otra instancia del explorador (cualquier explorador) y pegue la dirección URL en la barra de direcciones.

1. Elija cualquier explorador, escriba un nombre y un mensaje, y haga clic en el botón **Enviar**. El nombre y el mensaje se muestran en ambas páginas al instante.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Presione **Depurar** (F5) para compilar y ejecutar el programa. Al ejecutar el programa se abre una ventana del explorador.

1. Abra otra ventana del explorador y cargue el sitio web de forma local en ella.

1. Elija cualquier explorador, escriba un nombre y un mensaje, y haga clic en el botón **Enviar**. El nombre y el mensaje se muestran en ambas páginas al instante.

---

  ![Soluciones](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a>Recursos relacionados

[Introducción a SignalR de ASP.NET Core](xref:signalr/introduction)
