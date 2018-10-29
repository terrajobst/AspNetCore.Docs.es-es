---
title: Introducción a NSwag y ASP.NET Core
author: zuckerthoben
description: Obtenga información sobre cómo usar NSwag para generar documentación y páginas de ayuda de una ASP.NET Web API.
ms.author: scaddie
ms.custom: mvc
ms.date: 09/20/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 6c7d76e2202bf47c8d3e5d296e64e9e8c820e2a1
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207854"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a>Introducción a NSwag y ASP.NET Core

Por [Christoph Nienaber](https://twitter.com/zuckerthoben) y [Rico Suter](https://rsuter.com)

::: moniker range=">= aspnetcore-2.1"

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([cómo descargarlo](xref:index#how-to-download-a-sample))

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([cómo descargarlo](xref:index#how-to-download-a-sample))

::: moniker-end

Registre el middleware de NSwag para:

* Generar la especificación de Swagger para la API web implementada.
* Proporcionar la interfaz de usuario de Swagger para examinar y probar la API web.

Para usar [NSwag](https://github.com/RSuter/NSwag) con middleware de ASP.NET Core, instale el paquete NuGet [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/). Este paquete contiene el middleware para generar y proporcionar la especificación de Swagger, la interfaz de usuario de Swagger (v2 y v3) y la [interfaz de usuario de ReDoc](https://github.com/Rebilly/ReDoc).

Además, se recomienda encarecidamente usar las funcionalidades de generación de código de NSwag. Elija una de las siguientes opciones para usar las funcionalidades de generación de código:

* Usar [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), una aplicación de escritorio de Windows para generar código de cliente en C# y TypeScript para la API.
* Usar los paquetes NuGet [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) o [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) para generar código dentro del proyecto.
* Usar NSwag desde la [línea de comandos](https://github.com/NSwag/NSwag/wiki/CommandLine).
* Usar el paquete NuGet [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild).

## <a name="features"></a>Características

La razón principal para usar NSwag es la posibilidad no ya de incorporar la interfaz de usuario de Swagger y el generador de Swagger, sino también de usar sus funcionalidades de generación de código flexibles. No es necesario que exista una API: se pueden usar API de terceros que incluyan Swagger y que permitan que NSwag genere una implementación de cliente. En cualquier caso, el ciclo de desarrollo se agiliza y es más fácil adaptarse a los cambios de API.

## <a name="package-installation"></a>Instalación del paquete

El paquete NuGet NSwag se puede agregar con uno de los siguientes métodos:

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* En la ventana **Consola del Administrador de paquetes**:
  * Vaya a **Vista** > **Otras ventanas** > **Consola del Administrador de paquetes**.
  * Vaya al directorio en el que está *TodoApi.csproj*.
  * Ejecute el siguiente comando:

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* En el cuadro de diálogo **Administrar paquetes NuGet**:
  * Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** > **Administrar paquetes NuGet**.
  * Establezca el **origen del paquete** en "nuget.org".
  * Escriba "NSwag.AspNetCore" en el cuadro de búsqueda.
  * Seleccione el paquete "NSwag.AspNetCore" en la pestaña **Examinar** y haga clic en **Instalar**.

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* Haga clic con el botón derecho en la carpeta *Paquetes* en **Panel de solución** > **Agregar paquetes...**
* Establezca el menú desplegable **Origen** de la ventana **Agregar paquetes** en "nuget.org".
* Escriba "NSwag.AspNetCore" en el cuadro de búsqueda.
* Seleccione el paquete "NSwag.AspNetCore" en el panel de resultados y haga clic en **Agregar paquete**.

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Ejecute el siguiente comando en el **terminal integrado**:

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Ejecute el siguiente comando:

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>Agregar y configurar el middleware de Swagger

Importe los siguientes espacios de nombres a la clase `Startup`:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

En el método `Startup.ConfigureServices`, registre los servicios necesarios de Swagger: 

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_ConfigureServices&highlight=8)]

En el método `Startup.Configure`, habilite el middleware para servir la especificación y la interfaz de usuario de Swagger v3 generadas:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

Inicie la aplicación. Vaya a `http://localhost:<port>/swagger` para ver la interfaz de usuario de Swagger. Vaya a `http://localhost:<port>/swagger/v1/swagger.json` para ver la especificación de Swagger.

## <a name="code-generation"></a>Generación de código

### <a name="via-nswagstudio"></a>Con NSwagStudio

* Instale NSwagStudio desde el [repositorio de GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio) oficial.
* Inicie NSwagStudio. Indique la dirección URL del archivo *swagger.json* en el cuadro de texto **Swagger Specification URL** (Dirección URL de la especificación Swagger) y haga clic en el botón **Create local Copy** (Crear copia local).
* Seleccione el tipo de salida de cliente **CSharp**. Las otras opciones son **TypeScript Client** (Cliente de TypeScript) y **CSharp Web API Controller** (Controlador Web API de CSharp). Si se usa un controlador Web API, consistirá básicamente en una generación inversa. Se usa una especificación de un servicio para recompilar dicho servicio.
* Haga clic en el botón **Generate Outputs** (Generar salidas). Se creará una implementación de cliente de C# completa del proyecto *TodoApi.NSwag*. Haga clic en la pestaña **CSharp Client** (Cliente de CSharp) en la sección **Outputs** (Salidas) para ver el código de cliente generado:

```csharp
//----------------------
// <auto-generated>
//     Generated using the NSwag toolchain v11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0)) (http://NSwag.org)
// </auto-generated>
//----------------------

namespace MyNamespace
{
    #pragma warning disable // Disable all warnings

    [System.CodeDom.Compiler.GeneratedCode("NSwag",
        "11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0))")]
    public partial class TodoClient
    {
        private string _baseUrl = "http://localhost:50499";
        private System.Lazy<Newtonsoft.Json.JsonSerializerSettings> _settings;

        public TodoClient()
        {
            _settings = new System.Lazy
                <Newtonsoft.Json.JsonSerializerSettings>(() =>
            {
                var settings = new Newtonsoft.Json.JsonSerializerSettings();
                UpdateJsonSerializerSettings(settings);
                return settings;
            });
        }

        public string BaseUrl
        {
            get { return _baseUrl; }
            set { _baseUrl = value; }
        }

        // code omitted for brevity
```

> [!TIP]
> El código de cliente de C# se genera según la configuración que haya definida en la pestaña **Settings** (Configuración) de la pestaña **CSharp Client** (Cliente de CSharp). Modifique esta configuración para realizar tareas como cambiar el nombre del espacio de nombres predeterminado y generar un método sincrónico.

* Coloque el código de C# generado en un archivo del proyecto de cliente (por ejemplo, una aplicación [Xamarin.Forms](/xamarin/xamarin-forms/)).
* Empiece a consumir la Web API:

```csharp
var todoClient = new TodoClient();

// Gets all to-dos from the API
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem, and save it in the API
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> Puede insertar una dirección URL base o un cliente HTTP en el cliente de API. El procedimiento recomendado para ello es siempre [reutilizar HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).

### <a name="other-ways-to-generate-client-code"></a>Otras formas de generar código de cliente

Se puede generar código de cliente de otras maneras más acordes con el flujo de trabajo:

* [MSBuild](https://www.nuget.org/packages/NSwag.MSBuild/)

* [En código](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [Plantillas T4](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a>Personalizar

Swagger proporciona opciones para documentar el modelo de objetos de cara a facilitar el consumo de Web API.

### <a name="api-info-and-description"></a>Información y descripción de la API

En el método `Startup.Configure`, la acción de configuración que se pasa al método `UseSwagger` agrega información, como el autor, la licencia y la descripción:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

La interfaz de usuario de Swagger muestra información de la versión:

![Interfaz de usuario de Swagger con información de la versión](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a>comentarios XML

Los comentarios XML se habilitan con los siguientes métodos:

# <a name="visual-studiotabvisual-studio-xml"></a>[Visual Studio](#tab/visual-studio-xml/)

::: moniker range=">= aspnetcore-2.0"

* Haga clic con el botón derecho en el **Explorador de soluciones** y seleccione **Editar <nombre_de_proyecto>.csproj**.
* Agregue manualmente las líneas resaltadas al archivo *.csproj*:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Propiedades**.
* Active la casilla **Archivo de documentación XML** en la sección **Salida** de la pestaña **Compilar**.

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[Visual Studio para Mac](#tab/visual-studio-mac-xml/)

::: moniker range=">= aspnetcore-2.0"

* Desde el *Panel de solución*, presione **Control** y haga clic en el nombre del proyecto. Vaya a **Herramientas** > **Editar archivo**.
* Agregue manualmente las líneas resaltadas al archivo *.csproj*:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* Abra el cuadro de diálogo **Opciones del proyecto** > **Compilar** > **Compilador**.
* Active la casilla **Generar documentación XML** en la sección **Opciones generales**.

::: moniker-end

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[Visual Studio Code](#tab/visual-studio-code-xml/)

Agregue manualmente las líneas resaltadas al archivo *.csproj*:

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a>Anotaciones de datos

::: moniker range="<= aspnetcore-2.0"

NSwag usa [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), y el tipo de valor devuelto recomendado para las acciones de Web API es [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult). Por tanto, NSwag no puede deducir lo que la acción realiza y lo que devuelve. Considere el ejemplo siguiente:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

La acción anterior devuelve `IActionResult` pero, dentro de la acción, devuelve [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) o [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest). Las anotaciones de datos sirven para indicar a los clientes qué códigos de estado HTTP se sabe que esta acción devuelve. Complete la acción con los siguientes atributos:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

NSwag usa [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), y el tipo de valor devuelto recomendado para las acciones de Web API es [ActionResult\<](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1). Por lo tanto, NSwag puede inferir únicamente el tipo de valor devuelto definido por `T`. No podrá inferir otros tipos de valor devuelto que son factibles en la acción. Considere el ejemplo siguiente:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

La acción anterior devuelve `ActionResult<T>` pero, dentro de la acción, devuelve [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute). Como el controlador se completa con el atributo [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute), también es posible una respuesta [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest). Vea [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses) (Respuestas HTTP 400 automáticas) para más información. Las anotaciones de datos sirven para indicar a los clientes qué códigos de estado HTTP se sabe que esta acción devuelve. Complete la acción con los siguientes atributos:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

Ahora, el generador de Swagger puede describir con precisión esta acción y los clientes generados sabrán lo que reciben cuando llamen al punto de conexión. Completar todas las acciones con estos atributos es tremendamente recomendable. Para obtener instrucciones sobre qué respuestas HTTP deben devolver las acciones de API, vea la [especificación RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).
