---
title: Crear una API Web con ASP.NET Core y Visual Studio Code
author: rick-anderson
description: Compilar una API web con ASP.NET Core MVC y Visual Studio Code en macOS, Linux o Windows
manager: wpickett
ms.author: riande
ms.date: 09/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/web-api-vsc
ms.openlocfilehash: 44566c4014400aa2ca3d512eeaa226637b5f0b97
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a>Crear una Web API con ASP.NET Core MVC y Visual Studio Code en Linux, macOS y Windows

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Mike Wasson](https://github.com/mikewasson)

En este tutorial se compila una API web para administrar una lista de tareas pendientes. No se construye una interfaz de usuario.

Hay tres versiones de este tutorial:

* macOS, Linux y Windows: API Web con Visual Studio Code (este tutorial)
* macOS: [API Web con Visual Studio para Mac](xref:tutorials/first-web-api-mac)
* Windows: [API Web con Visual Studio para Windows](xref:tutorials/first-web-api)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a>Configuración del entorno de desarrollo

Descargue e instale:
- [SDK de .NET Core 2.0.0](https://www.microsoft.com/net/core) o versiones posteriores
- [Visual Studio Code](https://code.visualstudio.com)
- [Extensión de C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) de Visual Studio Code

## <a name="create-the-project"></a>Crear el proyecto

Desde una consola, ejecute los siguientes comandos:

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

Abra la carpeta *TodoApi* en Visual Studio Code (VS Code) y seleccione el archivo *Startup.cs*.

- Seleccione **Sí** en el mensaje de **advertencia** "Required assets to build and debug are missing from 'TodoApi'. Add them?" (Faltan los activos necesarios para compilar y depurar en 'TodoApi'. ¿Quiere agregarlos?).
- Seleccione **Restaurar** en el mensaje de **información** "There are unresolved dependencies" (Hay dependencias no resueltas).

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code con la advertencia "Required assets to build and debug are missing from 'TodoApi'. Add them?" No volver a preguntar, Ahora no, Sí](web-api-vsc/_static/vsc_restore.png)

Presione **Depurar** (F5) para compilar y ejecutar el programa. En un explorador, vaya a http://localhost:5000/api/values. Se muestra lo siguiente:

`["value1","value2"]`

Vea [Ayuda de Visual Studio Code](#visual-studio-code-help) para obtener sugerencias sobre el uso de VS Code.

## <a name="add-support-for-entity-framework-core"></a>Agregar compatibilidad con Entity Framework Core

Al crear un proyecto nuevo en .NET Core 2.0, se agrega el proveedor "Microsoft.AspNetCore.All" en el archivo *TodoApi.csproj*. No es necesario instalar el proveedor de base de datos [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) por separado. Este proveedor de base de datos permite usar Entity Framework Core con una base de datos en memoria.

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

## <a name="add-a-model-class"></a>Agregar una clase de modelo

Un modelo es un objeto que representa los datos de la aplicación. En este caso, el único modelo es una tarea pendiente.

Agregue una carpeta denominada *Models*. Puede colocar clases de modelo en cualquier lugar del proyecto, pero la carpeta *Models* se usa por convención.

Agregue una clase `TodoItem` con el siguiente código:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

La base de datos genera el `Id` cuando se crea `TodoItem`.

## <a name="create-the-database-context"></a>Crear el contexto de base de datos

El *contexto de base de datos* es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos determinado. Esta clase se crea al derivar de la clase `Microsoft.EntityFrameworkCore.DbContext`.

Agregue una clase `TodoContext` a la carpeta *Models*:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Adición de un controlador

En la carpeta *Controladores*, cree una clase denominada `TodoController`. Agregue el código siguiente:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Iniciar la aplicación

En VS Code, presione F5 para iniciar la aplicación. Vaya a http://localhost:5000/api/todo (el controlador `Todo` que se acaba de crear).

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a>Ayuda de Visual Studio Code

- [Introducción](https://code.visualstudio.com/docs)
- [Depuración](https://code.visualstudio.com/docs/editor/debugging)
- [Terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [Métodos abreviados de teclado](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [Métodos abreviados de teclado de Mac](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Métodos abreviados de teclado de Linux](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Métodos abreviados de teclado de Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


