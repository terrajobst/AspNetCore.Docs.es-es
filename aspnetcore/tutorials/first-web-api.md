---
title: Crear una API web con ASP.NET Core y Visual Studio
author: rick-anderson
description: Crear una API web con ASP.NET Core MVC y Visual Studio en Windows
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: tutorials/first-web-api
ms.openlocfilehash: eb23d5376742e04712f714263815582fddc5d18e
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450702"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio"></a>Crear una API web con ASP.NET Core y Visual Studio

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Mike Wasson](https://github.com/mikewasson)

En este tutorial se compila una API web para administrar una lista de tareas pendientes. No se crea ninguna interfaz de usuario (UI).

Hay tres versiones de este tutorial:

* Windows: API web con Visual Studio en Windows (en este tutorial, vea la [versión en vídeo](https://www.youtube.com/watch?v=TTkhEyGBfAk))
* macOS: [API Web con Visual Studio para Mac](xref:tutorials/first-web-api-mac)
* macOS, Linux y Windows: [API web con Visual Studio Code](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

> [!NOTE]
> Vamos a probar la facilidad de uso de una nueva estructura propuesta para la tabla de contenido de ASP.NET Core.  Si tiene unos minutos para realizar un ejercicio de búsqueda de 7 temas diferentes en la tabla actual o propuesta de contenido, [haga clic aquí para participar en el estudio](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a>Crear el proyecto

Haga lo siguiente para descargar Visual Studio:

* En el menú **Archivo**, seleccione **Nuevo** > **Proyecto**.
* Seleccione la plantilla **Aplicación web ASP.NET Core**. Denomine el proyecto *TodoApi* y haga clic en **Aceptar**.
* En el cuadro de diálogo **Nueva aplicación web ASP.NET Core - TodoApi**, seleccione la versión ASP.NET Core. Seleccione la plantilla **API** y haga clic en **Aceptar**. **No** seleccione **Habilitar compatibilidad con Docker**.

### <a name="launch-the-app"></a>Iniciar la aplicación

En Visual Studio, presione CTRL+F5 para iniciar la aplicación. Visual Studio inicia un explorador y navega hasta `http://localhost:<port>/api/values`, donde `<port>` es un número de puerto elegido aleatoriamente. En Chrome, Microsoft Edge y Firefox se muestra la salida siguiente:

```json
["value1","value2"]
```

Si usa Internet Explorer, se le pedirá que guarde un archivo *values.json*.

### <a name="add-a-model-class"></a>Agregar una clase de modelo

Un modelo es un objeto que representa los datos de la aplicación. En este caso, el único modelo es una tarea pendiente.

En el Explorador de soluciones, haga clic con el botón derecho en el proyecto. Seleccione **Agregar** > **Nueva carpeta**. Asigne a la carpeta el nombre *Models*.

> [!NOTE]
> Las clases del modelo pueden ir en cualquier parte del proyecto. La carpeta *Models* se usa por convención para las clases de modelos.

En el Explorador de soluciones, haga clic con el botón derecho en la carpeta de *modelos* y seleccione **Agregar** > **Clase**. Denomine la clase *TodoItem* y, después, haga clic en **Agregar**.

Actualice la clase `TodoItem` por el siguiente código:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

La base de datos genera el `Id` cuando se crea `TodoItem`.

### <a name="create-the-database-context"></a>Crear el contexto de base de datos

El *contexto de base de datos* es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos determinado. Esta clase se crea derivándola de la clase `Microsoft.EntityFrameworkCore.DbContext`.

En el Explorador de soluciones, haga clic con el botón derecho en la carpeta de *modelos* y seleccione **Agregar** > **Clase**. Denomine la clase *TodoContext* y, después, haga clic en **Agregar**.

Reemplace la clase por el siguiente código:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a>Adición de un controlador

En el Explorador de soluciones, haga clic con el botón derecho en la carpeta *Controladores*. Seleccione **Agregar** > **Nuevo elemento**. En el cuadro de diálogo **Agregar nuevo elemento**, seleccione la plantilla **Clase de controlador de API**. Denomine la clase *TodoController* y, después, haga clic en **Agregar**.

![Cuadro de diálogo Agregar nuevo elemento con la palabra "controlador" en el cuadro de búsqueda y la opción Clase de controlador de API web seleccionada](first-web-api/_static/new_controller.png)

Reemplace la clase por el siguiente código:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Iniciar la aplicación

En Visual Studio, presione CTRL+F5 para iniciar la aplicación. Visual Studio inicia un explorador y navega hasta `http://localhost:<port>/api/values`, donde `<port>` es un número de puerto elegido aleatoriamente. Vaya al controlador `Todo` en `http://localhost:<port>/api/todo`.

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
