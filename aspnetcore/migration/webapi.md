---
title: Migrar de ASP.NET Web API a ASP.NET Core
author: ardalis
description: Obtenga información sobre cómo migrar una implementación de Web API desde la API Web de ASP.NET 4. x ASP.NET Core a MVC.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/05/2019
uid: migration/webapi
ms.openlocfilehash: 7f61b78c589fc9d01061b50554e5a639e372c3d8
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78653009"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>Migrar de ASP.NET Web API a ASP.NET Core

Por [Scott Addie](https://twitter.com/scott_addie) y [Steve Smith](https://ardalis.com/)

Una API Web ASP.NET 4. x es un servicio HTTP que llega a una amplia gama de clientes, incluidos exploradores y dispositivos móviles. ASP.NET Core unifica los modelos de aplicaciones MVC y Web API de ASP.NET 4. x en un modelo de programación más sencillo conocido como ASP.NET Core MVC. En este artículo se muestran los pasos necesarios para migrar desde la API Web de ASP.NET 4. x ASP.NET Core a MVC.

[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/migration/webapi/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisites

[!INCLUDE [prerequisites](../includes/net-core-prereqs-vs2019-2.2.md)]

## <a name="review-aspnet-4x-web-api-project"></a>Revisar el proyecto de Web API de ASP.NET 4. x

Como punto de partida, en este artículo se usa el proyecto *ProductsApp* creado en [Introducción con ASP.net web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api). En ese proyecto, se configura un proyecto de API Web ASP.NET 4. x simple como se indica a continuación.

En *global.asax.CS*, se realiza una llamada a `WebApiConfig.Register`:

[!code-csharp[](webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

La clase `WebApiConfig` se encuentra en la carpeta *App_Start* y tiene un método `Register` estático:

[!code-csharp[](webapi/sample/ProductsApp/App_Start/WebApiConfig.cs)]

Esta clase configura el [enrutamiento de atributos](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), aunque en realidad no se usa en el proyecto. También configura la tabla de enrutamiento, que se usa en ASP.NET Web API. En este caso, la API Web de ASP.NET 4. x espera que las direcciones URL coincidan con el formato `/api/{controller}/{id}`, `{id}` ser opcional.

El proyecto *ProductsApp* incluye un controlador. El controlador hereda de `ApiController` y contiene dos acciones:

[!code-csharp[](webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=28,33)]

El modelo de `Product` utilizado por `ProductsController` es una clase simple:

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

En las secciones siguientes se muestra cómo migrar el proyecto de Web API a ASP.NET Core MVC.

## <a name="create-destination-project"></a>Crear proyecto de destino

Siga los pasos que se describen a continuación en Visual Studio:

* Vaya a **archivo** > **nuevo** **proyecto** de >  > **otros tipos de proyectos** > soluciones de **Visual Studio**. Seleccione **solución en blanco**y asigne el nombre *WebAPIMigration*a la solución. Haga clic en el botón **Aceptar** .
* Agregue el proyecto *ProductsApp* existente a la solución.
* Agregue un nuevo proyecto de **aplicación Web de ASP.net Core** a la solución. Seleccione la plataforma de destino de **.net Core** en la lista desplegable y seleccione la plantilla de proyecto de **API** . Asigne al proyecto el nombre *ProductsCore*y haga clic en el botón **Aceptar** .

La solución ahora contiene dos proyectos. En las secciones siguientes se explica cómo migrar el contenido del proyecto *ProductsApp* al proyecto *ProductsCore* .

## <a name="migrate-configuration"></a>Migración de la configuración

ASP.NET Core no utiliza la carpeta *App_Start* o el archivo *global. asax* , y el archivo *Web. config* se agrega en el momento de la publicación. *Startup.CS* es el sustituto de *global. asax* y se encuentra en la raíz del proyecto. La clase `Startup` controla todas las tareas de inicio de la aplicación. Para más información, consulte <xref:fundamentals/startup>.

En ASP.NET Core MVC, el enrutamiento de atributos se incluye de forma predeterminada cuando se llama a <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> en `Startup.Configure`. La siguiente llamada de `UseMvc` reemplaza el archivo *App_Start/webapiconfig.CS* del proyecto *ProductsApp* :

[!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13])]

## <a name="migrate-models-and-controllers"></a>Migración de modelos y controladores

Copie el controlador del proyecto de *ProductApp* y el modelo que usa. Siga estos pasos:

1. Copie *Controllers/ProductsController. CS* del proyecto original al nuevo.
1. Copie toda la carpeta *Models* del proyecto original al nuevo.
1. Cambie los espacios de nombres de los archivos copiados para que coincidan con el nuevo nombre del proyecto (*ProductsCore*). Ajuste también la instrucción `using ProductsApp.Models;` en *ProductsController.CS* .

En este punto, la creación de la aplicación produce una serie de errores de compilación. Los errores se producen porque los componentes siguientes no existen en ASP.NET Core:

* Clase `ApiController`
* Espacio de nombres `System.Web.Http`
* `IHttpActionResult` (interfaz)

Corrija los errores de la siguiente manera:

1. Cambio de `ApiController` a <xref:Microsoft.AspNetCore.Mvc.ControllerBase>. Agregue `using Microsoft.AspNetCore.Mvc;` para resolver el `ControllerBase` referencia.
1. Elimine `using System.Web.Http;`.
1. Cambie el tipo de valor devuelto de la acción `GetProduct` de `IHttpActionResult` a `ActionResult<Product>`.

Simplifique la instrucción `return` de la acción de `GetProduct` para lo siguiente:

```csharp
return product;
```

## <a name="configure-routing"></a>Configuración del enrutamiento

Configure el enrutamiento de la siguiente manera:

1. Marque la clase `ProductsController` con los siguientes atributos:

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    El atributo de [`[Route]`](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) anterior configura el patrón de enrutamiento de atributo del controlador. El atributo [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) hace que el enrutamiento de atributos sea un requisito para todas las acciones de este controlador.

    El enrutamiento de atributos admite tokens, como `[controller]` y `[action]`. En tiempo de ejecución, cada token se reemplaza por el nombre del controlador o la acción, respectivamente, a la que se ha aplicado el atributo. Los tokens reducen el número de cadenas mágicas del proyecto. Los tokens también garantizan que las rutas permanezcan sincronizadas con los controladores y acciones correspondientes cuando se apliquen las refactorizaciones de cambio de nombre automáticas.
1. Establezca el modo de compatibilidad del proyecto en ASP.NET Core 2,2:

    [!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    El cambio anterior:

    * Se requiere para usar el atributo `[ApiController]` en el nivel de controlador.
    * Opta por los posibles saltos de los comportamientos introducidos en ASP.NET Core 2,2.
1. Habilite las solicitudes HTTP GET para las acciones de `ProductController`:
    * Aplique el atributo [`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) a la acción `GetAllProducts`.
    * Aplique el atributo `[HttpGet("{id}")]` a la acción `GetProduct`.

Después de los cambios anteriores y la eliminación de las instrucciones de `using` sin usar, el archivo *ProductsController.CS* tiene el siguiente aspecto:

[!code-csharp[](webapi/sample/ProductsCore/Controllers/ProductsController.cs)]

Ejecute el proyecto migrado y busque `/api/products`. Aparece una lista completa de tres productos. Vaya a `/api/products/1`. Aparece el primer producto.

## <a name="compatibility-shim"></a>Corrección de compatibilidad

La biblioteca [Microsoft. AspNetCore. Mvc. WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) proporciona una corrección de compatibilidad para migrar los proyectos de API Web de ASP.net 4. x a ASP.net Core. La corrección de compatibilidad amplía ASP.NET Core para admitir una serie de convenciones de la API Web de ASP.NET 4. x. El ejemplo que se ha migrado anteriormente en este documento es lo suficientemente básico como para que la corrección de compatibilidad no fuera necesaria. En el caso de los proyectos de mayor tamaño, el uso de la corrección de compatibilidad puede ser útil para el puente temporal entre ASP.NET Core y ASP.NET 4. x Web API 2.

La corrección de compatibilidad de la API Web está pensada para usarse como medida temporal para admitir la migración de proyectos de API Web de ASP.NET 4. x grandes a ASP.NET Core. Con el tiempo, los proyectos deben actualizarse para usar patrones de ASP.NET Core en lugar de basarse en la corrección de compatibilidad.

Entre las características de compatibilidad incluidas en `Microsoft.AspNetCore.Mvc.WebApiCompatShim` se incluyen:

* Agrega un tipo de `ApiController` para que no sea necesario actualizar los tipos base de los controladores.
* Habilita el enlace de modelos de estilo de API Web. ASP.NET Core enlace de modelos de MVC funciona de forma predeterminada con el de ASP.NET 4. x MVC 5. La corrección de compatibilidad cambia el enlace de modelos para que sea más similar a las convenciones de enlace de modelos de la API Web 2 de ASP.NET 4. x. Por ejemplo, los tipos complejos se enlazan automáticamente desde el cuerpo de la solicitud.
* Extiende el enlace de modelos para que las acciones de controlador puedan tomar parámetros de tipo `HttpRequestMessage`.
* Agrega formateadores de mensajes que permiten que las acciones devuelvan resultados de tipo `HttpResponseMessage`.
* Agrega métodos de respuesta adicionales que pueden usar las acciones de Web API 2 para servir respuestas:
  * `HttpResponseMessage` generadores:
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * Métodos del resultado de la acción:
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* Agrega una instancia de `IContentNegotiator` al contenedor de inserción de dependencias (DI) de la aplicación y pone a disposición los tipos relacionados con la negociación de contenido de [Microsoft. Aspnet. WebApi. Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/). Entre los ejemplos de estos tipos se incluyen `DefaultContentNegotiator` y `MediaTypeFormatter`.

Para usar la corrección de compatibilidad:

1. Instale el paquete NuGet [Microsoft. AspNetCore. Mvc. WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) .
1. Registre los servicios de la corrección de compatibilidad con el contenedor de DI de la aplicación mediante una llamada a `services.AddMvc().AddWebApiConventions()` en `Startup.ConfigureServices`.
1. Defina rutas específicas de la API Web mediante `MapWebApiRoute` en el `IRouteBuilder` en la llamada de `IApplicationBuilder.UseMvc` de la aplicación.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:web-api/index>
* <xref:web-api/action-return-types>
* <xref:mvc/compatibility-version>
