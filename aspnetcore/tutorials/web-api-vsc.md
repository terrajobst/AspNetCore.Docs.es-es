---
title: Crear una API web con ASP.NET Core y Visual Studio Code
author: rick-anderson
description: Compilar una API web con ASP.NET Core MVC y Visual Studio Code en macOS, Linux o Windows
ms.author: riande
ms.custom: mvc
ms.date: 07/30/2018
uid: tutorials/web-api-vsc
ms.openlocfilehash: 4ce808ec4241ab2fc3c2fb81c3fdb15dd853cd90
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342281"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a>Crear una API web con ASP.NET Core y Visual Studio Code

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Mike Wasson](https://github.com/mikewasson)

En este tutorial se compila una API web para administrar una lista de tareas pendientes. No se construye una interfaz de usuario.

Hay tres versiones de este tutorial:

* macOS, Linux y Windows: API Web con Visual Studio Code (este tutorial)
* macOS: [API Web con Visual Studio para Mac](xref:tutorials/first-web-api-mac)
* Windows: [API Web con Visual Studio para Windows](xref:tutorials/first-web-api)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-the-project"></a>Crear el proyecto

Desde una consola, ejecute los siguientes comandos:

```console
dotnet new webapi -o TodoApi
code TodoApi
```

La carpeta *TodoApi* se abre en Visual Studio Code (VS Code). Seleccione el archivo *Startup.cs*.

* Seleccione **Sí** en el mensaje de **advertencia** "Required assets to build and debug are missing from 'TodoApi'. Add them?" (Faltan los activos necesarios para compilar y depurar en 'TodoApi'. ¿Quiere agregarlos?).
* Seleccione **Restaurar** en el mensaje de **información** "There are unresolved dependencies" (Hay dependencias no resueltas).

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code con la advertencia "Required assets to build and debug are missing from 'TodoApi'. Add them?" No volver a preguntar, Ahora no, Sí](web-api-vsc/_static/vsc_restore.png)

Presione **Depurar** (F5) para compilar y ejecutar el programa. En un navegador, vaya a http://localhost:5000/api/values. Se muestra el siguiente resultado:

```json
["value1","value2"]
```

Vea [Ayuda de Visual Studio Code](#visual-studio-code-help) para obtener sugerencias sobre el uso de VS Code.

## <a name="add-support-for-entity-framework-core"></a>Agregar compatibilidad con Entity Framework Core

:::moniker range=">= aspnetcore-2.1"

Al crear un proyecto en ASP.NET Core 2.1 o posterior, se agrega la referencia de paquete [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) al archivo *TodoApi.csproj*. Agregue el atributo `Version`, si aún no se ha especificado.

[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

:::moniker range="<= aspnetcore-2.0"

Al crear un proyecto en ASP.NET Core 2.0, se agrega la referencia de paquete [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) al archivo *TodoApi.csproj*:

[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

No es necesario instalar el proveedor de base de datos [Entity Framework Core InMemory](/ef/core/providers/in-memory/) por separado. Este proveedor de base de datos permite usar Entity Framework Core con una base de datos en memoria.

## <a name="add-a-model-class"></a>Agregar una clase de modelo

Un modelo es un objeto que representa los datos de la aplicación. En este caso, el único modelo es una tarea pendiente.

Agregue una carpeta denominada *Models*. Puede colocar clases de modelo en cualquier lugar del proyecto, pero la carpeta *Models* se usa por convención.

Agregue una clase `TodoItem` con el siguiente código:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

La base de datos genera el `Id` cuando se crea `TodoItem`.

## <a name="create-the-database-context"></a>Crear el contexto de base de datos

El *contexto de base de datos* es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos determinado. Esta clase se crea al derivar de la clase `Microsoft.EntityFrameworkCore.DbContext`.

Agregue una clase `TodoContext` a la carpeta *Models*:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Adición de un controlador

En la carpeta *Controladores*, cree una clase denominada `TodoController`. Reemplace el contenido por el siguiente código:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Iniciar la aplicación

En VS Code, presione F5 para iniciar la aplicación. Vaya a http://localhost:5000/api/todo (el controlador `Todo` que se acaba de crear).

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a>Ayuda de Visual Studio Code

* [Introducción](https://code.visualstudio.com/docs)
* [Depuración](https://code.visualstudio.com/docs/editor/debugging)
* [Terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [Métodos abreviados de teclado](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [Funciones rápidas de teclado de macOS](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [Métodos abreviados de teclado de Linux](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [Métodos abreviados de teclado de Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
