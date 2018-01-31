---
title: Crear una API web con ASP.NET Core y Visual Studio para Windows
author: rick-anderson
description: Crear una API web con ASP.NET Core MVC y Visual Studio para Windows
manager: wpickett
ms.author: riande
ms.date: 08/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api
ms.openlocfilehash: 1146132693681eca8f92beb00ebabd7296534688
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a>Crear una API web con ASP.NET Core y Visual Studio para Windows

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Mike Wasson](https://github.com/mikewasson)

En este tutorial se compila una API web para administrar una lista de tareas pendientes. No se crea ninguna interfaz de usuario (UI).

Hay tres versiones de este tutorial:

* Windows: API web con Visual Studio para Windows (este tutorial)
* macOS: [API web con Visual Studio para Mac](xref:tutorials/first-web-api-mac)
* macOS, Linux y Windows: [API web con Visual Studio Code](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE[install 2.0](../includes/install2.0.md)]

Vea [este PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) para la versión 1.1 de ASP.NET Core.

## <a name="create-the-project"></a>Crear el proyecto

En Visual Studio, seleccione el menú **Archivo**, > **Nuevo** > **Proyecto**.

Seleccione la plantilla de proyecto **Aplicación web ASP.NET Core (.NET Core)**. Asigne al proyecto el nombre `TodoApi` y seleccione **Aceptar**.

![Cuadro de diálogo Nuevo proyecto](first-web-api/_static/new-project.png)

En el cuadro de diálogo **Nueva aplicación web ASP.NET Core - TodoApi**, seleccione la plantilla **API web**. Seleccione **Aceptar**. **No** seleccione **Habilitar compatibilidad con Docker**.

![Cuadro de diálogo Nueva aplicación web ASP.NET con la plantilla de proyecto API web seleccionada en las plantillas de ASP.NET Core](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a>Iniciar la aplicación

En Visual Studio, presione CTRL+F5 para iniciar la aplicación. Visual Studio inicia un explorador y navega hasta `http://localhost:port/api/values`, donde *port* es un número de puerto elegido aleatoriamente. En Chrome, Microsoft Edge y Firefox se muestra la salida siguiente:

```
["value1","value2"]
```

### <a name="add-a-model-class"></a>Agregar una clase de modelo

Un modelo es un objeto que representa los datos de la aplicación. En este caso, el único modelo es una tarea pendiente.

Agregue una carpeta denominada "Modelos". En el Explorador de soluciones, haga clic con el botón derecho en el proyecto. Seleccione **Agregar** > **Nueva carpeta**. Asigne a la carpeta el nombre *Models*.

Nota: Las clases del modelo van en cualquier parte del proyecto. La carpeta *Models* se usa por convención para las clases de modelos.

Agregue una clase `TodoItem`. Haga clic con el botón derecho en la carpeta *Modelos* y seleccione **Agregar** > **Clase**. Asigne a la clase el nombre `TodoItem` y seleccione **Agregar**.

Actualice la clase `TodoItem` por el siguiente código:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

La base de datos genera el `Id` cuando se crea `TodoItem`.

### <a name="create-the-database-context"></a>Crear el contexto de base de datos

El *contexto de base de datos* es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos determinado. Esta clase se crea derivándola de la clase `Microsoft.EntityFrameworkCore.DbContext`.

Agregue una clase `TodoContext`. Haga clic con el botón derecho en la carpeta *Modelos* y seleccione **Agregar** > **Clase**. Asigne a la clase el nombre `TodoContext` y seleccione **Agregar**.

Reemplace la clase por el siguiente código:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a>Adición de un controlador

En el Explorador de soluciones, haga clic con el botón derecho en la carpeta *Controladores*. Seleccione **Agregar** > **Nuevo elemento**. En el cuadro de diálogo **Agregar nuevo elemento**, seleccione la plantilla **Clase de controlador de API web**. Asigne a la clase el nombre `TodoController`.

![Cuadro de diálogo Agregar nuevo elemento con la palabra "controlador" en el cuadro de búsqueda y la opción Clase de controlador de API web seleccionada](first-web-api/_static/new_controller.png)

Reemplace la clase por el siguiente código:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Iniciar la aplicación

En Visual Studio, presione CTRL+F5 para iniciar la aplicación. Visual Studio inicia un explorador y navega hasta `http://localhost:port/api/values`, donde *port* es un número de puerto elegido aleatoriamente. Vaya al controlador `Todo` en `http://localhost:port/api/todo`.

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

