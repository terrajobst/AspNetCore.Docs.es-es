---
title: "Páginas de ayuda de ASP.NET Core Web API mediante Swagger"
author: spboyer
description: "En este tutorial se proporciona una guía sobre cómo incorporar Swagger para generar documentación y páginas de ayuda para una aplicación de API web."
ms.author: spboyer
manager: wpickett
ms.date: 09/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 302199bb0b32d4f6610e04455bb28372095e9873
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-core-web-api-help-pages-using-swagger"></a>Páginas de ayuda de ASP.NET Core Web API mediante Swagger

<a name="web-api-help-pages-using-swagger"></a>

Por [Shayne Boyer](https://twitter.com/spboyer) y [Scott Addie](https://twitter.com/Scott_Addie)

Comprender los distintos métodos de una API puede representar un reto para un desarrollador a la hora de compilar una aplicación de consumo.

Para generar páginas de ayuda y una documentación de calidad para la API web, por medio de [Swagger](https://swagger.io/) con la implementación de .NET Core [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), lo único que tiene que hacer es agregar un par de paquetes de NuGet y modificar el archivo *Startup.cs*.

* [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) es un proyecto de código abierto para generar documentos de Swagger para las API web de ASP.NET Core.

* [Swagger](https://swagger.io/) es una representación de lectura mecánica de una API de REST que admite la compatibilidad con la documentación interactiva, la generación de SDK de cliente y la detectabilidad.

Este tutorial se basa en el ejemplo de [Cree su primera API web con ASP.NET Core MVC y Visual Studio](xref:tutorials/first-web-api). Si quiere seguir, descargue el ejemplo en [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).

## <a name="getting-started"></a>Introducción

Hay tres componentes principales de Swashbuckle:

* `Swashbuckle.AspNetCore.Swagger`: un modelo de objetos de Swagger y middleware para exponer objetos `SwaggerDocument` como puntos de conexión JSON.

* `Swashbuckle.AspNetCore.SwaggerGen`: un generador de Swagger que genera objetos `SwaggerDocument` directamente de sus rutas, controladores y modelos. Se suele combinar con el middleware de punto de conexión de Swagger para exponer automáticamente el JSON de Swagger.

* `Swashbuckle.AspNetCore.SwaggerUI`: una versión insertada de la herramienta de interfaz de usuario de Swagger, que interpreta el JSON de Swagger para crear una experiencia enriquecida y personalizable para describir la funcionalidad de la API web. Incluye herramientas de ejecución de pruebas integradas para los métodos públicos.

## <a name="nuget-packages"></a>Paquetes NuGet

Se puede agregar Swashbuckle con los métodos siguientes:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* En la ventana **Consola del Administrador de paquetes**:

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* En el cuadro de diálogo **Administrar paquetes NuGet**:

     * Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** > **Administrar paquetes NuGet**.
     * Establezca el **origen del paquete** en "nuget.org".
     * Escriba "Swashbuckle.AspNetCore" en el cuadro de búsqueda.
     * Seleccione el paquete "Swashbuckle.AspNetCore" en la pestaña **Examinar** y haga clic en **Instalar**.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* Haga clic con el botón derecho en la carpeta *Paquetes* en **Panel de solución** > **Agregar paquetes...**
* Establezca el menú desplegable **Origen** de la ventana **Agregar paquetes** en "nuget.org".
* Escriba Swashbuckle.AspNetCore en el cuadro de búsqueda.
* Seleccione el paquete Swashbuckle.AspNetCore en el panel de resultados y haga clic en **Agregar paquete**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Ejecute el siguiente comando en el **terminal integrado**:

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Ejecute el siguiente comando:

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-to-the-middleware"></a>Agregar y configurar Swagger en el middleware

Agregue el generador de Swagger a la colección de servicios en el método `ConfigureServices` de *Startup.cs*:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

Agregue la instrucción using siguiente para la clase `Info`:

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

En el método `Configure` de *Startup.cs*, habilite el middleware para servir el documento JSON generado y la IU de Swagger:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

Inicie la aplicación y vaya a `http://localhost:<random_port>/swagger/v1/swagger.json`. Aparecerá el documento generado en el que se describen los puntos de conexión.

**Nota:** En Microsoft Edge, Google Chrome y Firefox se muestran los documentos JSON de forma nativa. Existen extensiones de Chrome que dan formato al documento para facilitar la lectura. *El siguiente ejemplo se ha reducido por motivos de brevedad.*

```json
{
   "swagger": "2.0",
   "info": {
       "version": "v1",
       "title": "API V1"
   },
   "basePath": "/",
   "paths": {
       "/api/Todo": {
           "get": {
               "tags": [
                   "Todo"
               ],
               "operationId": "ApiTodoGet",
               "consumes": [],
               "produces": [
                   "text/plain",
                   "application/json",
                   "text/json"
               ],
               "responses": {
                   "200": {
                       "description": "Success",
                       "schema": {
                           "type": "array",
                           "items": {
                               "$ref": "#/definitions/TodoItem"
                           }
                       }
                   }
                }
           },
           "post": {
               ...
           }
       },
       "/api/Todo/{id}": {
           "get": {
               ...
           },
           "put": {
               ...
           },
           "delete": {
               ...
   },
   "definitions": {
       "TodoItem": {
           "type": "object",
            "properties": {
                "id": {
                    "format": "int64",
                    "type": "integer"
                },
                "name": {
                    "type": "string"
                },
                "isComplete": {
                    "default": false,
                    "type": "boolean"
                }
            }
       }
   },
   "securityDefinitions": {}
}
```

Este documento le guía por la interfaz de usuario de Swagger, que se puede visualizar yendo a `http://localhost:<random_port>/swagger`:

![Interfaz de usuario de Swagger](web-api-help-pages-using-swagger/_static/swagger-ui.png)

Todos los métodos de acción pública aplicados a `TodoController` se pueden probar desde la interfaz de usuario. Haga clic en un nombre de método para expandir la sección. Agregue todos los parámetros necesarios y haga clic en "Try it out!" (¡Pruébelo!).

![Ejemplo de prueba de acción GET de Swagger](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

## <a name="customization--extensibility"></a>Personalización y extensibilidad

Swagger proporciona opciones para documentar el modelo de objetos y personalizar la interfaz de usuario para que coincida con el tema.

### <a name="api-info-and-description"></a>Información y descripción de la API

La acción de configuración que se pasa al método `AddSwaggerGen` se puede usar para agregar información, como el autor, la licencia y la descripción:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?range=20-30,36)]

En la siguiente imagen se muestra la interfaz de usuario de Swagger, donde aparece la información de la versión:

![Interfaz de usuario de Swagger con información de la versión: descripción, autor y el vínculo See more (Ver más)](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a>Comentarios XML

Los comentarios XML se pueden habilitar con los métodos siguientes:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Propiedades**.
* Seleccione la casilla **Archivo de documentación XML** en la sección **Salida** de la pestaña **Compilar**:

![Pestaña Compilar de las propiedades del proyecto](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* Abra el cuadro de diálogo **Opciones del proyecto** > **Compilar** > **Compilador**.
* Seleccione la casilla **Generar documentación XML** en la sección **Opciones generales**:

![Sección Opciones generales de las opciones del proyecto](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Agregue manualmente el siguiente fragmento de código al archivo *.csproj*:

[!code-xml[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?range=7-9)]

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Vea Visual Studio Code.

---

Puede habilitar los comentarios XML para proporcionar información de depuración para miembros y tipos públicos sin documentación. Los miembros y tipos sin documentación se indican mediante el mensaje de advertencia: *Falta el comentario XML para el tipo o miembro visible públicamente*.

Configure Swagger para usar el archivo XML generado. Para Linux o sistemas operativos que no sean Windows, las rutas de acceso y los nombres de archivo pueden distinguir entre mayúsculas y minúsculas. Por ejemplo, un archivo denominado *ToDoApi.XML* podría aparecer en Windows, pero no en CentOS.

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

En el código anterior, `ApplicationBasePath` obtiene la ruta de acceso base de la aplicación. La ruta de acceso base se usa para buscar el archivo de comentarios XML. *TodoApi.xml* solo funciona en este ejemplo, puesto que el nombre del archivo de comentarios XML generado se basa en el nombre de la aplicación.

Agregar los comentarios con la triple barra diagonal al método mejora la interfaz de usuario de Swagger mediante la adición de la descripción al encabezado de la sección:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

![Interfaz de usuario de Swagger en la que se muestra el comentario XML "Deletes a specific TodoItem" (Elimina una tarea pendiente específica) para el método DELETE](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

La interfaz de usuario se controla con el archivo JSON generado, que también contiene estos comentarios:

```json
"delete": {
    "tags": [
        "Todo"
    ],
    "summary": "Deletes a specific TodoItem.",
    "operationId": "ApiTodoByIdDelete",
    "consumes": [],
    "produces": [],
    "parameters": [
        {
            "name": "id",
            "in": "path",
            "description": "",
            "required": true,
            "type": "integer",
            "format": "int64"
        }
    ],
    "responses": {
        "200": {
            "description": "Success"
        }
    }
}
```

Agregue una etiqueta [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) a la documentación del método de acción `Create`. Complementa la información especificada en la etiqueta `<summary>` y proporciona una interfaz de usuario de Swagger más sólida. El contenido de la etiqueta `<remarks>` puede estar formado por texto, JSON o XML.

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

Fíjese en las mejoras de la interfaz de usuario con estos comentarios adicionales.

![Interfaz de usuario de Swagger con comentarios adicionales](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a>Anotaciones de datos

Incorpore al modelo atributos que se encuentran en `System.ComponentModel.DataAnnotations` para controlar los componentes de la interfaz de usuario de Swagger.

Agregue el atributo `[Required]` a la propiedad `Name` de la clase `TodoItem`:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Models/TodoItem.cs?highlight=10)]

La presencia de este atributo cambia el comportamiento de la interfaz de usuario y modifica el esquema JSON subyacente:

```json
"definitions": {
    "TodoItem": {
        "required": [
            "name"
        ],
        "type": "object",
        "properties": {
            "id": {
                "format": "int64",
                "type": "integer"
            },
            "name": {
                "type": "string"
            },
            "isComplete": {
                "default": false,
                "type": "boolean"
            }
        }
    }
},
```

Agregue el atributo `[Produces("application/json")]` al controlador de API. Su propósito consiste en declarar que las acciones del controlador admitan un tipo de contenido de *application/json*:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

En el menú desplegable **Response Content Type** (Tipo de contenido de respuesta) se selecciona este tipo de contenido como el valor predeterminado para las acciones GET del controlador:

![Interfaz de usuario de Swagger con el tipo de contenido de respuesta predeterminado](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

A medida que aumenta el uso de anotaciones de datos en la API web, la interfaz de usuario y las páginas de ayuda de la API pasan a ser más descriptivas y útiles.

### <a name="describing-response-types"></a>Describir los tipos de respuesta

Lo que más preocupa a los desarrolladores de consumo es lo que se devuelve, sobre todo los tipos de respuesta y los códigos de error (si no son los habituales). Estos se administran en las anotaciones de datos y en los comentarios XML.

La acción `Create` devuelve `201 Created` en caso de éxito o `400 Bad Request` si el cuerpo de solicitud publicado es nulo. Sin la documentación correcta en la interfaz de usuario de Swagger, el consumidor no dispone de la información necesaria de estos resultados esperados. El problema se corrige agregando las líneas resaltadas en el ejemplo siguiente:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

Ahora, la interfaz de usuario de Swagger documenta de forma clara los códigos de respuesta HTTP esperados:

![Interfaz de usuario de Swagger, donde se muestra la descripción de la clase de respuesta POST "Returns the newly created item" (Devuelve el elemento recién creado) y "400 - If the item is null" (400 - Si el elemento es nulo) para el código de estado y el motivo en Response Messages (Mensajes de respuesta)](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customizing-the-ui"></a>Personalizar la interfaz de usuario

La interfaz de usuario es funcional y clara pero, a la hora de compilar las páginas de documentación de la API, quiere que represente su marca o tema. Para llevar a cabo esta tarea con los componentes de Swashbuckle, se deben agregar los recursos para servir archivos estáticos y, luego, generar la estructura de carpetas que hospedará estos archivos.

Si el destino es .NET Framework, agregue el paquete NuGet `Microsoft.AspNetCore.StaticFiles` al proyecto:

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

Habilite el middleware de los archivos estáticos:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_Configure&highlight=3)]

Obtenga el contenido de la carpeta *dist* en el [repositorio de GitHub de la interfaz de usuario de Swagger](https://github.com/swagger-api/swagger-ui/tree/2.x/dist). Esta carpeta contiene los recursos necesarios para la página de interfaz de usuario de Swagger.

Cree una carpeta *wwwroot/swagger/ui* y copie en ella el contenido de la carpeta *dist*.

Cree un archivo *wwwroot/swagger/ui/css/custom.css* con el siguiente código CSS para personalizar el encabezado de página:

[!code-css[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/css/custom.css)]

Cree una referencia a *custom.css* en el archivo *index.html*:

[!code-html[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/index.html?range=14)]

Vaya a la página *index.html* en `http://localhost:<random_port>/swagger/ui/index.html`. Escriba `http://localhost:<random_port>/swagger/v1/swagger.json` en el cuadro de texto del encabezado y haga clic en el botón **Explorar**. La página resultante tiene el siguiente aspecto:

![Interfaz de usuario de Swagger con el título de encabezado personalizado](web-api-help-pages-using-swagger/_static/custom-header.png)

Puede hacer muchas más cosas con la página. Vea todas las capacidades de los recursos de la interfaz de usuario en el [repositorio de GitHub de la interfaz de usuario de Swagger](https://github.com/swagger-api/swagger-ui).
