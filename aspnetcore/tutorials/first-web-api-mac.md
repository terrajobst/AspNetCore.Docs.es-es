---
title: Crear una API web con ASP.NET Core y Visual Studio para Mac
description: Crear una API web con ASP.NET Core MVC y Visual Studio para Mac
author: rick-anderson
ms.author: riande
ms.date: 09/15/2017
ms.topic: get-started-article
ms.prod: asp.net-core
uid: tutorials/first-web-api-mac
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
ms.technology: aspnet
keywords: ASP.NET Core, WebAPI, API web, REST, mac, macOS, HTTP, servicio, servicio HTTP
manager: wpickett
ms.openlocfilehash: 3f84fa55baf6d808185a28db290d9e6d3c46bdac
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a>Crear una API web con ASP.NET Core MVC y Visual Studio para Mac

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Mike Wasson](https://github.com/mikewasson)

En este tutorial compilará una API Web para administrar una lista de tareas pendientes. No compilará ninguna interfaz de usuario.

Hay tres versiones de este tutorial:

* macOS: API web con Visual Studio para Mac (este tutorial)
* Windows: [API web con Visual Studio para Windows](xref:tutorials/first-web-api)
* macOS, Linux y Windows: [API web con Visual Studio Code](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* Vea [Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index) (Introducción a ASP.NET Core MVC en Mac o Linux) para ver un ejemplo en el que se usa una base de datos persistente.

## <a name="prerequisites"></a>Requisitos previos

Instale el software siguiente:

- [SDK de .NET Core 2.0.0](https://www.microsoft.com/net/core) o versiones posteriores
- [Visual Studio para Mac](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a>Crear el proyecto

Desde Visual Studio, seleccione **Archivo > Nueva solución**.

![macOS: Nueva solución](first-web-api-mac/_static/sln.png)

Seleccione **Aplicación .NET Core > API web de ASP.NET Core > Siguiente**.

![macOS: Cuadro de diálogo Nuevo proyecto](first-web-api-mac/_static/1.png)

Escriba **TodoApi** en **Nombre del proyecto** y seleccione Crear.

![cuadro de diálogo de configuración](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a>Iniciar la aplicación

En Visual Studio, seleccione **Ejecutar > Iniciar con depuración** para iniciar la aplicación. Visual Studio inicia un explorador y se desplaza a `http://localhost:5000`. Obtendrá un error HTTP 404 (No encontrado).  Cambie la dirección URL a `http://localhost:port/api/values`. Se mostrarán los datos de `ValuesController`:

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a>Agregar compatibilidad para Entity Framework Core

Instale el proveedor de base de datos [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/). Este proveedor de base de datos permite usar Entity Framework Core con una base de datos en memoria.

* En el menú **Proyecto**, seleccione **Agregar paquetes NuGet**. 

  *  Como alternativa, puede hacer clic con el botón derecho en **Dependencias** y seleccionar **Agregar paquetes**.

* Escriba `EntityFrameworkCore.InMemory` en el cuadro de búsqueda.
* Seleccione `Microsoft.EntityFrameworkCore.InMemory` y, luego, **Agregar paquete**.

### <a name="add-a-model-class"></a>Agregar una clase de modelo

Un modelo es un objeto que representa los datos de la aplicación. En este caso, el único modelo es una tarea pendiente.

Agregue una carpeta denominada *Models*. En el Explorador de soluciones, haga clic con el botón derecho en el proyecto. Seleccione **Agregar** > **Nueva carpeta**. Asigne a la carpeta el nombre *Modelos*.

![nueva carpeta](first-web-api-mac/_static/folder.png)

Nota: Puede colocar clases de modelo en cualquier lugar del proyecto, pero la carpeta *Modelos* se usa por convención.

Agregue una clase `TodoItem`. Haga clic con el botón derecho en la carpeta *Modelos* y seleccione **Agregar > Nuevo archivo > General > Clase vacía**. Asigne a la clase el nombre `TodoItem` y seleccione **Nuevo**.

Reemplace el código generado por el siguiente:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

La base de datos genera el `Id` cuando se crea `TodoItem`.

### <a name="create-the-database-context"></a>Crear el contexto de base de datos

El *contexto de base de datos* es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos determinado. Esta clase se crea derivándola de la clase `Microsoft.EntityFrameworkCore.DbContext`.

Agregue una clase `TodoContext` a la carpeta *Modelos*.

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Agregar un controlador

En el Explorador de soluciones, en la carpeta *Controladores*, agregue la clase `TodoController`.

Reemplace el código generado por el siguiente (y agregue llaves de cierre):

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Iniciar la aplicación

En Visual Studio, seleccione **Ejecutar > Iniciar con depuración** para iniciar la aplicación. Visual Studio inicia un explorador y navega hasta `http://localhost:port`, donde *port* es un número de puerto elegido aleatoriamente. Obtendrá un error HTTP 404 (No encontrado).  Cambie la dirección URL a `http://localhost:port/api/values`. Se mostrarán los datos de `ValuesController`:

```
["value1","value2"]
```

Vaya al controlador `Todo` en `http://localhost:port/api/todo`:

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a>Implementar las otras operaciones CRUD

Vamos a agregar los métodos `Create`, `Update` y `Delete` al controlador. Son variaciones de un tema, así que solo mostraré el código y comentaré las diferencias principales. Compile el proyecto después de agregar o cambiar el código.

### <a name="create"></a>Crear

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Se trata de un método HTTP POST, indicado por el atributo [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute). El atributo [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) indica a MVC que obtenga el valor de la tarea pendiente del cuerpo de la solicitud HTTP.

El método `CreatedAtRoute` devuelve una respuesta 201, que es la respuesta estándar para un método HTTP POST que crea un nuevo recurso en el servidor. `CreatedAtRoute` también agrega un encabezado de ubicación a la respuesta. El encabezado de ubicación especifica el URI de la tarea pendiente recién creada. Vea [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (10.2.2 201 creada).

### <a name="use-postman-to-send-a-create-request"></a>Usar Postman para enviar una solicitud de creación

* Inicie la aplicación (**Ejecutar > Iniciar con depuración**).
* Inicie Postman.

![Consola de Postman](first-web-api/_static/pmc.png)

* Establezca el método HTTP en `POST`.
* Seleccione el botón de radio **Body** (Cuerpo).
* Seleccione el botón de radio **Raw** (Sin formato).
* Establezca el tipo en JSON.
* En el editor de pares clave-valor, escriba una tarea pendiente como la siguiente:

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* Seleccione **Send** (Enviar).

* Seleccione la pestaña Headers (Encabezados) en el panel inferior y copie el encabezado **Location** (Ubicación):

![Pestaña Headers (Encabezados) de la consola de Postman](first-web-api/_static/pmget.png)

Puede usar el URI del encabezado Location (Ubicación) para acceder al recurso que acaba de crear. Recuerde que el método `GetById` creó la ruta denominada `"GetTodo"`:

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a>Actualizar

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`Update` es similar a `Create`, pero usa HTTP PUT. La respuesta es [204 Sin contenido](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Según la especificación HTTP, una solicitud PUT requiere que el cliente envíe toda la entidad actualizada, no solo los deltas. Para admitir actualizaciones parciales, use HTTP PATCH.

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Consola de Postman que muestra la respuesta 204 Sin contenido](first-web-api/_static/pmcput.png)

### <a name="delete"></a>Eliminar

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

La respuesta es [204 Sin contenido](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![Consola de Postman que muestra la respuesta 204 Sin contenido](first-web-api/_static/pmd.png)

## <a name="next-steps"></a>Pasos siguientes

* [Routing to Controller Actions](xref:mvc/controllers/routing) (Enrutamiento a acciones del controlador)
* Para obtener más información sobre la implementación de la API, vea [Hospedaje e implementación](xref:host-and-deploy/index).
* [Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))
* [Postman](https://www.getpostman.com/)
* [Fiddler](https://www.telerik.com/download/fiddler)
