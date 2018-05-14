---
title: Introducción a NSwag y ASP.NET Core
author: zuckerthoben
description: Obtenga información sobre cómo usar NSwag para generar documentación y páginas de ayuda para una aplicación ASP.NET Core Web API.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 80e6a9e1702d8f68d139d2ff9c3a01a27c40cecb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-nswag-and-aspnet-core"></a>Introducción a NSwag y ASP.NET Core

Por [Christoph Nienaber](https://twitter.com/zuckerthoben) y [Rico Suter](https://rsuter.com)

Para usar [NSwag](https://github.com/RSuter/NSwag) con middleware de ASP.NET Core, se necesita el paquete NuGet [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/). Este paquete consta de un generador de Swagger, la interfaz de usuario de Swagger (versiones 2 y 3) y la [interfaz de usuario de ReDoc](https://github.com/Rebilly/ReDoc).

Se recomienda encarecidamente usar las capacidades de generación de código de NSwag. Elija una de las siguientes opciones para generar código:

* Usar [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), una aplicación de escritorio de Windows para generar código de cliente en C# y TypeScript para la API
* Usar los paquetes NuGet [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) o [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) para generar código dentro del proyecto
* Usar NSwag desde la [línea de comandos](https://github.com/NSwag/NSwag/wiki/CommandLine)
* Usar el paquete NuGet [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild)

## <a name="features"></a>Características

La razón principal para usar NSwag es la posibilidad no ya de incorporar la interfaz de usuario de Swagger y el generador de Swagger, sino también de usar sus capacidades de generación de código flexibles. No es necesario que exista una API: se pueden usar API de terceros que incluyan Swagger y que permitan que NSwag genere una implementación de cliente. En cualquier caso, el ciclo de desarrollo se agiliza y es más fácil adaptarse a los cambios de API.

## <a name="package-installation"></a>Instalación del paquete

El paquete NuGet NSwag se puede agregar con uno de los siguientes métodos:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* En la ventana **Consola del Administrador de paquetes**:

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* En el cuadro de diálogo **Administrar paquetes NuGet**:
  * Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** > **Administrar paquetes NuGet**.
  * Establezca el **origen del paquete** en "nuget.org".
  * Escriba "NSwag.AspNetCore" en el cuadro de búsqueda.
  * Seleccione el paquete "NSwag.AspNetCore" en la pestaña **Examinar** y haga clic en **Instalar**.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* Haga clic con el botón derecho en la carpeta *Paquetes* en **Panel de solución** > **Agregar paquetes...**
* Establezca el menú desplegable **Origen** de la ventana **Agregar paquetes** en "nuget.org".
* Escriba NSwag.AspNetCore en el cuadro de búsqueda.
* Seleccione el paquete NSwag.AspNetCore en el panel de resultados y haga clic en **Agregar paquete**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Ejecute el siguiente comando en el **terminal integrado**:

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Ejecute el siguiente comando:

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>Agregar y configurar el middleware de Swagger

Importe los siguientes espacios de nombres a la clase `Info`:

```csharp
using NSwag.AspNetCore;
using System.Reflection;
using NJsonSchema;
```

En el método `Startup.Configure`, habilite el middleware para servir la especificación y la interfaz de usuario de Swagger:

[!code-cs[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=4,7-10)]

Inicie la aplicación. Vaya a `/swagger` para ver la interfaz de usuario de Swagger. Vaya a `/swagger/v1/swagger.json` para ver la especificación de Swagger.

## <a name="code-generation"></a>Generación de código

### <a name="via-nswagstudio"></a>Con NSwagStudio

* Instale `NSwagStudio` desde el [repositorio de GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio) oficial.

* Inicie NSwagStudio. Escriba la ubicación del archivo *swagger.json* o cópielo directamente:

![NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio.png)

* Indique el tipo de salida de cliente que prefiera. Las opciones disponibles son **TypeScript Client** (Cliente de TypeScript), **CSharp Client** (Cliente de CSharp) o **CSharp Web API Controller** (Controlador Web API de CSharp). Si se usa un controlador Web API, consistirá básicamente en una generación inversa. Se usa una especificación de un servicio para recompilar dicho servicio.

* Haga clic en **Generate Outputs** (Generar salidas).

* Aquí tiene una implementación de cliente completa del ejemplo *TodoApi.NSwag* en C#:

![Salida de NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio-Output.png)

* Coloque el archivo en un proyecto de cliente (por ejemplo, una aplicación [Xamarin.Forms](/xamarin/xamarin-forms/)). Empiece a consumir la API:

```csharp
var todoClient = new TodoClient();

// Gets all Todos from the Api
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem and save it in the Api
var createdTodo = await todoClient.CreateAsync(new TodoItem);

// Get a single Todo by Id
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> Puede insertar una dirección URL base o un cliente HTTP en el cliente de API. El procedimiento recomendado para ello es siempre [reutilizar HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).

Ya puede empezar a implementar fácilmente la API en proyectos de cliente.

### <a name="other-ways-to-generate-client-code"></a>Otras formas de generar código de cliente

Se puede generar código de otras maneras más acordes con el flujo de trabajo:

* [MSBuild](https://www.nuget.org/packages/NSwag.MSBuild/)

* [En código](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [Plantillas T4](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customization"></a>Personalización

### <a name="xml-comments"></a>comentarios XML

Los comentarios XML se habilitan con los siguientes métodos:

#### <a name="visual-studiotabvisual-studio-xml"></a>[Visual Studio](#tab/visual-studio-xml/)
* En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Propiedades**.
* Seleccione la casilla **Archivo de documentación XML** en la sección **Salida** de la pestaña **Compilar**:

![Pestaña Compilar de las propiedades del proyecto](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[Visual Studio para Mac](#tab/visual-studio-mac-xml/)
* Abra el cuadro de diálogo **Opciones del proyecto** > **Compilar** > **Compilador**.
* Seleccione la casilla **Generar documentación XML** en la sección **Opciones generales**:

![Sección Opciones generales de las opciones del proyecto](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[Visual Studio Code](#tab/visual-studio-code-xml/)
Agregue manualmente el siguiente fragmento de código al archivo *.csproj*:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/TodoApiNSwag.csproj?range=7-9)]

* * *
## <a name="data-annotations"></a>Anotaciones de datos

NSwag usa [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), y el procedimiento recomendado para las acciones de Web API consiste en devolver [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult). Por tanto, NSwag no puede deducir lo que la acción realiza y lo que devuelve. Considere el ejemplo siguiente:

```csharp
public IActionResult Create([FromBody] TodoItem item)
{
    if (item == null)
    {
        return BadRequest();
    }

    _context.TodoItems.Add(item);
    _context.SaveChanges();

     return CreatedAtRoute("GetTodo", new { id = item.Id }, item);
}
```

La acción anterior devuelve `IActionResult` pero, dentro de la acción, devuelve [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) o [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest). Las anotaciones de datos sirven para indicar a los clientes qué respuesta HTTP devuelve esta acción. Complete la acción con los siguientes atributos:

```csharp
[HttpPost]
[ProducesResponseType(typeof(TodoItem), 201)] // Created
[ProducesResponseType(typeof(TodoItem), 400)] // BadRequest
public IActionResult Create([FromBody] TodoItem item)
```

Ahora, el generador de Swagger puede describir con precisión esta acción y los clientes generados sabrán lo que reciben cuando llamen al punto de conexión. Completar todas las acciones con estos atributos es tremendamente recomendable. Para obtener instrucciones sobre qué respuestas HTTP deben devolver las acciones de API, vea la [especificación RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).
