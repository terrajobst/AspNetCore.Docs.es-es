---
title: 'Tutorial: Llamada a una API Web con jQuery mediante ASP.NET Core'
author: rick-anderson
description: Obtenga información sobre cómo llamar a una API web de ASP.NET Core con jQuery.
ms.author: riande
ms.custom: mvc
ms.date: 07/20/2019
uid: tutorials/web-api-jquery
ms.openlocfilehash: eb8b2453fd037170a49f531fea4c3ef1c056292d
ms.sourcegitcommit: 0efb9e219fef481dee35f7b763165e488aa6cf9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602574"
---
# <a name="tutorial-call-an-aspnet-core-web-api-with-jquery"></a>Tutorial: Llamada a una API web de ASP.NET Core con jQuery

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En este tutorial se muestra cómo llamar a una API web de ASP.NET Core con jQuery.

::: moniker range="< aspnetcore-3.0"

Para ASP.net Core 2.2, consulte la versión 2.2 de [Llamada a la API web con jQuery](xref:tutorials/first-web-api#call-the-api-with-jquery).

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a>Requisitos previos

Finalización del [Tutorial: Creación de una API web](xref:tutorials/first-web-api)

## <a name="call-the-api-with-jquery"></a>Llamada a la API con jQuery

En esta sección, se agrega una página HTML que usa jQuery para llamar a la API web. jQuery inicia la solicitud y actualiza la página con los detalles de la respuesta de la API.

Configure la aplicación para [atender archivos estáticos](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) y [habilitar la asignación de archivos predeterminada](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) mediante la actualización de *Startup.cs* con el siguiente código resaltado:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/StartupJquery.cs?highlight=8-9&name=snippet_configure)]

Cree una carpeta *wwwroot* en el directorio del proyecto.

Agregue un archivo HTML denominado *index.html* al directorio *wwwroot*. Reemplace el contenido por el siguiente marcado:

[!code-html[](first-web-api/samples/3.0/TodoApi/wwwroot/index.html)]

Agregue un archivo JavaScript denominado *site.js* al directorio *wwwroot*. Reemplace el contenido por el siguiente código:

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

Puede que sea necesario realizar un cambio en la configuración de inicio del proyecto de ASP.NET Core para probar la página HTML localmente:

* Abra *Properties\launchSettings.json*.
* Quite la propiedad `launchUrl` para forzar a la aplicación a abrirse en *index.html*, esto es, el archivo predeterminado del proyecto.

Existen varias formas de obtener jQuery. En el fragmento de código anterior, la biblioteca se carga desde una red CDN.

En este ejemplo se llama a todos los métodos CRUD de la API. A continuación, encontrará algunas explicaciones de las llamadas a la API.

### <a name="get-a-list-of-to-do-items"></a>Obtención de una lista de tareas pendientes

La función de JQuery [ajax](https://api.jquery.com/jquery.ajax/) envía una solicitud `GET` a la API, que devuelve código JSON que representa una matriz de tareas pendientes. La función de devolución de llamada `success` se invoca si la solicitud se realiza correctamente. En la devolución de llamada, el DOM se actualiza con la información de la tarea pendiente.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>Incorporación de una tarea pendiente

La función [ajax](https://api.jquery.com/jquery.ajax/) envía una solicitud `POST` con la tarea pendiente en su cuerpo. Las opciones `accepts` y `contentType` se establecen en `application/json` para especificar el tipo de medio que se va a recibir y a enviar. La tarea pendiente se convierte en JSON mediante [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify). Cuando la API devuelve un código de estado correcto, se invoca la función `getData` para actualizar la tabla HTML.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>Actualizar una tarea pendiente

El hecho de actualizar una tarea pendiente es similar al de agregar una. El valor `url` cambia para agregar el identificador único del elemento, y `type` es `PUT`.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>Eliminar una tarea pendiente

Para eliminar una tarea pendiente, hay que establecer el valor `type` de la llamada de AJAX en `DELETE` y especificar el identificador único de la tarea en la dirección URL.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

Pase al siguiente tutorial para obtener información sobre cómo generar páginas de ayuda de API:

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
::: moniker-end