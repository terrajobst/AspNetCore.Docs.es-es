---
title: Crear una API web con ASP.NET Core y Visual Studio para Mac
author: rick-anderson
description: Crear una API web con ASP.NET Core MVC y Visual Studio para Mac
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 699fbbf54abf1dc5c4156c559761110cdb375558
ms.sourcegitcommit: c867d7427bd4a88a78b2322e156367733b532730
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a>Crear una API web con ASP.NET Core y Visual Studio para Mac

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Mike Wasson](https://github.com/mikewasson)

En este tutorial se compila una API web para administrar una lista de tareas pendientes. No se construye la interfaz de usuario.

Hay tres versiones de este tutorial:

* macOS: API web con Visual Studio para Mac (este tutorial)
* Windows: [API web con Visual Studio para Windows](xref:tutorials/first-web-api)
* macOS, Linux y Windows: [API web con Visual Studio Code](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

Vea [Introduction to ASP.NET Core MVC on macOS or Linux](xref:tutorials/first-mvc-app-xplat/index) (Introducción a ASP.NET Core MVC en macOS o Linux) para obtener un ejemplo en el que se usa una base de datos persistente.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a>Crear el proyecto

En Visual Studio, seleccione **Archivo** > **Nueva solución**.

![macOS: Nueva solución](first-web-api-mac/_static/sln.png)

Seleccione **Aplicación .NET Core** > **API web de ASP.NET Core** > **Siguiente**.

![macOS: Cuadro de diálogo Nuevo proyecto](first-web-api-mac/_static/1.png)

Escriba *TodoApi* en **Nombre del proyecto** y haga clic en **Crear**.

![cuadro de diálogo de configuración](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a>Iniciar la aplicación

En Visual Studio, seleccione **Ejecutar** > **Iniciar con depuración** para iniciar la aplicación. Visual Studio inicia un explorador y se desplaza a `http://localhost:5000`. Obtendrá un error HTTP 404 (No encontrado). Cambie la dirección URL a `http://localhost:<port>/api/values`. Se muestran los datos de `ValuesController`:

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a>Agregar compatibilidad con Entity Framework Core

Instale el proveedor de base de datos [Entity Framework Core InMemory](/ef/core/providers/in-memory/). Este proveedor de base de datos permite usar Entity Framework Core con una base de datos en memoria.

* En el menú **Proyecto**, seleccione **Agregar paquetes NuGet**.

  * Como alternativa, puede hacer clic con el botón derecho en **Dependencias** y seleccionar **Agregar paquetes**.

* Escriba `EntityFrameworkCore.InMemory` en el cuadro de búsqueda.
* Seleccione `Microsoft.EntityFrameworkCore.InMemory` y, luego, **Agregar paquete**.

### <a name="add-a-model-class"></a>Agregar una clase de modelo

Un modelo es un objeto que representa los datos de la aplicación. En este caso, el único modelo es una tarea pendiente.

En el Explorador de soluciones, haga clic con el botón derecho en el proyecto. Seleccione **Agregar** > **Nueva carpeta**. Asigne a la carpeta el nombre *Modelos*.

![nueva carpeta](first-web-api-mac/_static/folder.png)

> [!NOTE]
> Puede colocar clases de modelo en cualquier lugar del proyecto, pero la carpeta *Models* se usa por convención.

Haga clic con el botón derecho en la carpeta *Modelos* y seleccione **Agregar** > **Nuevo archivo** > **General** > **Clase vacía**. Denomine la clase *TodoItem* y, después, haga clic en **Nuevo**.

Reemplace el código generado por el siguiente:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

La base de datos genera el `Id` cuando se crea `TodoItem`.

### <a name="create-the-database-context"></a>Crear el contexto de base de datos

El *contexto de base de datos* es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos determinado. Esta clase se crea derivándola de la clase `Microsoft.EntityFrameworkCore.DbContext`.

Agregue una clase `TodoContext` a la carpeta *Modelos*.

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Agregar un controlador

En el Explorador de soluciones, en la carpeta *Controladores*, agregue la clase `TodoController`.

Reemplace el código generado con el siguiente:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Iniciar la aplicación

En Visual Studio, seleccione **Ejecutar** > **Iniciar con depuración** para iniciar la aplicación. Visual Studio inicia un explorador y navega hasta `http://localhost:<port>`, donde `<port>` es un número de puerto elegido aleatoriamente. Obtendrá un error HTTP 404 (No encontrado). Cambie la dirección URL a `http://localhost:<port>/api/values`. Se muestran los datos de `ValuesController`:

```json
["value1","value2"]
```

Vaya al controlador `Todo` en `http://localhost:<port>/api/todo`. Se devuelve el siguiente JSON:

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a>Implementar las otras operaciones CRUD

Vamos a agregar los métodos `Create`, `Update` y `Delete` al controlador. Estos métodos son variaciones de un tema, así que solo mostraré el código y comentaré las diferencias principales. Compile el proyecto después de agregar o cambiar el código.

### <a name="create"></a>Crear

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

El código anterior responde a un método HTTP POST, como se puede apreciar por el atributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute). El atributo [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) indica a MVC que obtenga el valor de la tarea pendiente del cuerpo de la solicitud HTTP.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

El código anterior responde a un método HTTP POST, como se puede apreciar por el atributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute). MVC obtiene el valor de la tarea pendiente del cuerpo de la solicitud HTTP.
::: moniker-end

El método `CreatedAtRoute` devuelve una respuesta 201. Se trata de la respuesta estándar de un método HTTP POST que crea un recurso en el servidor. `CreatedAtRoute` también agrega un encabezado de ubicación a la respuesta. El encabezado de ubicación especifica el URI de la tarea pendiente recién creada. Vea [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (10.2.2 201 creada).

### <a name="use-postman-to-send-a-create-request"></a>Usar Postman para enviar una solicitud de creación

* Inicie la aplicación (**Ejecutar** > **Iniciar con depuración**).
* Abra Postman.

![Consola de Postman](first-web-api/_static/pmc.png)

* Actualice el número de puerto en la dirección URL de localhost.
* Establezca el método HTTP en *POST*.
* Haga clic en la pestaña **Body** (Cuerpo).
* Seleccione el botón de radio **Raw** (Sin formato).
* Establezca el tipo en *JSON (application/json)*.
* Escriba un cuerpo de solicitud con una tarea pendiente parecida al siguiente JSON:

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* Haga clic en el botón **Send** (Enviar).

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> Si no aparece ninguna respuesta tras hacer clic en **Send** (Enviar), deshabilite la opción **SSL certification verification** (Comprobación de certificación SSL). La encontrará en **File** > **Settings** (Archivo > Configuración). Vuelva a hacer clic en el botón **Send** (Enviar) después de deshabilitar la configuración.
::: moniker-end

Haga clic en la pestaña **Headers** (Encabezados) del panel **Response** (Respuesta) y copie el valor de encabezado de **Location** (Ubicación):

![Pestaña Headers (Encabezados) de la consola de Postman](first-web-api/_static/pmc2.png)

Puede usar el URI del encabezado Location (Ubicación) para tener acceso al recurso que ha creado. El método `Create` devuelve [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_). El primer parámetro que se pasa a `CreatedAtRoute` representa la ruta con nombre que se usa para generar la dirección URL. Recuerde que el método `GetById` creó la ruta con nombre `"GetTodo"`:

```csharp
[HttpGet("{id}", Name = "GetTodo")]
```

### <a name="update"></a>Actualizar

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end

`Update` es similar a `Create`, pero usa HTTP PUT. La respuesta es [204 Sin contenido](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Según la especificación HTTP, una solicitud PUT requiere que el cliente envíe toda la entidad actualizada, no solo los deltas. Para admitir actualizaciones parciales, use HTTP PATCH.

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Consola de Postman que muestra la respuesta 204 Sin contenido](first-web-api/_static/pmcput.png)

### <a name="delete"></a>Eliminar

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

La respuesta es [204 Sin contenido](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![Consola de Postman que muestra la respuesta 204 Sin contenido](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
