---
title: 'Tutorial: Introducción a SignalR en ASP.NET Core'
author: tdykstra
description: En este tutorial, creará una aplicación de chat en la que se usa SignalR para ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: tutorials/signalr
ms.openlocfilehash: 6f93d6dc664f68425ef0fa0d02f9011e4875bc33
ms.sourcegitcommit: 9bdba90b2c97a4016188434657194b2d7027d6e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47402138"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a>Tutorial: Introducción a SignalR en ASP.NET Core

En este tutorial se describen los conceptos básicos de la creación de una aplicación en tiempo real con SignalR. Aprenderá a:

> [!div class="checklist"]
> * Crear una aplicación web en la que se usa SignalR en ASP.NET Core.
> * Crear un concentrador SignalR en el servidor.
> * Conectarse con el concentrador SignalR desde clientes de JavaScript.
> * Usar el concentrador para enviar mensajes desde cualquier cliente a todos los clientes conectados.

Al final, tendrá una aplicación de chat funcional:

![Aplicación de ejemplo de SignalR](signalr/_static/signalr-get-started-finished.png)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample)).

## <a name="prerequisites"></a>Requisitos previos

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2017 versión 15.8 o posterior](https://www.visualstudio.com/downloads/) con la carga de trabajo **ASP.NET y desarrollo web**
* [.NET Core SDK 2.1 o posterior](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [.NET Core SDK 2.1 o posterior](https://www.microsoft.com/net/download/all)
* [C# para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* [Visual Studio para Mac, versión 7.5.4 o posterior](https://www.visualstudio.com/downloads/)
* [SDK de .NET Core 2.1 o posterior](https://www.microsoft.com/net/download/all) (incluido en la instalación de Visual Studio)

---

## <a name="create-the-project"></a>Crear el proyecto

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* En el menú, seleccione **Archivo > Nuevo proyecto**.

* En el cuadro de diálogo **Nuevo proyecto**, seleccione **Instalado > Visual C# > Web > Aplicación web ASP.NET Core**. Asigne al proyecto el nombre *SignalRChat*.

  ![Cuadro de diálogo Nuevo proyecto en Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* Seleccione **Aplicación web** para crear un proyecto en el que se use Razor Pages.

* Seleccione una plataforma de destino de **.NET Core**, seleccione **ASP.NET Core 2.1** y haga clic en **Aceptar**.

  ![Cuadro de diálogo Nuevo proyecto en Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* Abra una carpeta que pueda usar para un proyecto nuevo.

* En el **terminal integrado**, ejecute el comando siguiente:

   ```console
   dotnet new webapp -o SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* En el menú, seleccione **Archivo > Nueva solución**.

* Seleccione **.NET Core > Aplicación > Aplicación web ASP.NET Core** (no seleccione **Aplicación web de ASP.NET Core (MVC)**).

* Seleccione **Siguiente**.

* Asigne el nombre *SignalRChat* al proyecto y, después, haga clic en **Crear**.

---

## <a name="add-the-signalr-client-library"></a>Agregar la biblioteca cliente de SignalR

La biblioteca de servidor de SignalR se incluye en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). La biblioteca cliente de JavaScript no se incluye automáticamente en el proyecto. En este tutorial, usará el [Administrador de bibliotecas (LibMan)](xref:client-side/libman/index) para obtener la biblioteca cliente de *unpkg*. [unpkg](https://unpkg.com/#/) es una [red de entrega de contenido](https://wikipedia.org/wiki/Content_delivery_network) que puede entregar todo lo que encuentre en [npm, el Administrador de paquetes Node.js](https://www.npmjs.com/get-npm).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **Client-Side Library** (Biblioteca del lado cliente).

* En el cuadro de diálogo **Add Client-Side Library** (Agregar biblioteca del lado cliente), en **Proveedor**, seleccione **unpkg**. 

* En **Biblioteca**, escriba _@aspnet/signalr@1_ y seleccione la versión más reciente que no sea una versión preliminar.

  ![Cuadro de diálogo Add Client-Side Library (Agregar biblioteca del lado cliente): selección de la biblioteca](signalr/_static/libman1.png)

* Seleccione **Choose specific files** (Elegir archivos específicos), expanda la carpeta *dist/browser* y seleccione *signalr.js* y *signalr.min.js*.

* Establezca **Ubicación de destino** en *wwwroot/lib/signalr/* y seleccione **Instalar**.

  ![Cuadro de diálogo Add Client-Side Library (Agregar biblioteca del lado cliente): selección de archivos y destino](signalr/_static/libman2.png)

  [LibMan](xref:client-side/libman/index) crea una carpeta *wwwroot/lib/signalr* y copia en ella los archivos seleccionados.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* En el **terminal integrado**, ejecute el comando siguiente para instalar LibMan.

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Vaya a la carpeta de proyecto (la que contiene el archivo *SignalRChat.csproj*).

* Ejecute el comando siguiente para obtener la biblioteca cliente de SignalR con LibMan. Puede que deba esperar unos segundos antes de ver la salida.

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  Los parámetros especifican las opciones siguientes:
  * Use el proveedor unpkg.
  * Copie los archivos en el destino *wwwroot/lib/signalr*.
  * Copie solo los archivos especificados.

  La salida se parece al ejemplo siguiente:

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* En el **terminal**, ejecute el siguiente comando para instalar LibMan.

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Vaya a la carpeta de proyecto (la que contiene el archivo *SignalRChat.csproj*).

* Ejecute el comando siguiente para obtener la biblioteca cliente de SignalR con LibMan.

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  Los parámetros especifican las opciones siguientes:
  * Use el proveedor unpkg.
  * Copie los archivos en el destino *wwwroot/lib/signalr*.
  * Copie solo los archivos especificados.

  La salida se parece al ejemplo siguiente:

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-the-signalr-hub"></a>Crear el concentrador SignalR

Un [concentrador](xref:signalr/hubs) es una clase que actúa como una canalización general que controla la comunicación entre el cliente y el servidor.

* En la carpeta del proyecto SignalRChat, cree una carpeta *Hubs*.

* En la carpeta *Hubs*, cree un archivo *ChatHub.cs* con el código siguiente:

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  La clase `ChatHub` hereda de la clase [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) de SignalR. La clase `Hub` administra las conexiones, los grupos y la mensajería.

  Cualquier cliente conectado puede llamar al método `SendMessage`. Envía el mensaje recibido a todos los clientes. El código de SignalR es asincrónico para proporcionar la máxima escalabilidad.

## <a name="configure-the-project-to-use-signalr"></a>Configurar el proyecto para que use SignalR

El servidor de SignalR se debe configurar para que pase las solicitudes de SignalR a SignalR.

* Agregue el código resaltado siguiente al archivo *Startup.cs*.

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  Estos cambios agregan SignalR al sistema de [inserción de dependencias](xref:fundamentals/dependency-injection) y a la canalización de [software intermedio](xref:fundamentals/middleware/index).

## <a name="create-the-signalr-client-code"></a>Crear el código de cliente de SignalR

* Reemplace el contenido de *Pages/Index.cshtml* con el código siguiente:

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  El código anterior:

  * Crea cuadros de texto para el nombre y el mensaje de texto, y un botón de envío.
  * Crea una lista con `id="messagesList"` para mostrar los mensajes que se reciben desde el concentrador SignalR.
  * Incluye las referencias de script en SignalR y el código de aplicación de *chat.js* que se va a crear en el paso siguiente.

* En la carpeta *wwwroot/js*, cree un archivo *chat.js* con el código siguiente:

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  El código anterior:

  * Crea e inicia una conexión.
  * Agrega al botón de envío un controlador que envía mensajes al concentrador.
  * Agrega al objeto de conexión un controlador que recibe mensajes desde el concentrador y los agrega a la lista.

## <a name="run-the-app"></a>Ejecutar la aplicación

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Presione **CTRL+F5** para ejecutar la aplicación sin depurar.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Presione **CTRL+F5** para ejecutar la aplicación sin depurar.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* En el menú, seleccione **Ejecutar > Iniciar sin depurar**.

---

* Copie la dirección URL de la barra de direcciones, abra otra instancia o pestaña del explorador, y pegue la dirección URL en la barra de direcciones.

* Elija cualquier explorador, escriba un nombre y un mensaje, y haga clic en el botón **Enviar**.

  El nombre y el mensaje se muestran en ambas páginas al instante.

  ![Aplicación de ejemplo de SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> Si la aplicación no funciona, abra las herramientas para desarrolladores del explorador (F12) y vaya a la consola. Es posible que vea errores relacionados con el código HTML y JavaScript. Por ejemplo, suponga que coloca *signalr.js* en una carpeta distinta a la indicada. En ese caso, la referencia a ese archivo no funcionará y verá un error 404 en la consola.
> ![Error: signalr.js no encontrado](signalr/_static/f12-console.png)

## <a name="next-steps"></a>Pasos siguientes

Si quiere que los clientes se conecten a una aplicación de SignalR desde otros dominios, tendrá que habilitar el uso compartido de recursos entre orígenes (CORS). Para obtener más información, vea [Uso compartido de recursos entre orígenes](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).

Para obtener más información sobre SignalR, los concentradores y los clientes de JavaScript, vea estos recursos:

* [Introducción a SignalR para ASP.NET Core](xref:signalr/introduction)
* [Uso de concentradores de SignalR para ASP.NET Core](xref:signalr/hubs)
* [Cliente de JavaScript de SignalR para ASP.NET Core](xref:signalr/javascript-client)
