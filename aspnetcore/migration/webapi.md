---
title: Migrar desde ASP.NET Web API a ASP.NET Core
author: ardalis
description: Obtenga información sobre cómo migrar una implementación de API web de ASP.NET 4.x Web API a ASP.NET Core MVC.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/10/2018
uid: migration/webapi
ms.openlocfilehash: ea14b7128582a21b36c70041d59fb638eebdbef0
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895012"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>Migrar desde ASP.NET Web API a ASP.NET Core

Por [Scott Addie](https://twitter.com/scott_addie) y [Steve Smith](https://ardalis.com/)

ASP.NET 4.x Web API es un servicio HTTP que llegue a una amplia gama de clientes, incluidos los exploradores y dispositivos móviles. Modelos de aplicación de API Web en un modelo de programación más sencillo, conocido como ASP.NET Core MVC y unifica de ASP.NET Core MVC de ASP.NET de 4.x. En este artículo muestra los pasos necesarios para migrar desde ASP.NET 4.x Web API a ASP.NET Core MVC.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/migration/webapi/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [net-core-prereqs-vs-2.2](../includes/net-core-prereqs-vs-2.2.md)]

## <a name="review-aspnet-4x-web-api-project"></a>Revise el proyecto ASP.NET 4.x Web API

Como punto de partida, este artículo se usa el *ProductsApp* proyecto creado en [Introducción a ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api). En ese proyecto, un proyecto simple ASP.NET 4.x Web API se configura como se indica a continuación.

En *Global.asax.cs*, se realiza una llamada a `WebApiConfig.Register`:

[!code-csharp[](webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

El `WebApiConfig` clase se encuentra en la *App_Start* carpeta y tiene una estática `Register` método:

[!code-csharp[](webapi/sample/ProductsApp/App_Start/WebApiConfig.cs)]

Esta clase configura [enrutamiento mediante atributos](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), aunque realmente no se usa en el proyecto. También configura la tabla de enrutamiento, que es utilizada por ASP.NET Web API. En este caso, ASP.NET 4.x Web API espera que las direcciones URL para que coincida con el formato `/api/{controller}/{id}`, con `{id}` que es opcional.

El *ProductsApp* proyecto incluye un controlador. El controlador hereda de `ApiController` y contiene dos acciones:

[!code-csharp[](webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=28,33)]

El `Product` modelo usado por `ProductsController` es una clase sencilla:

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

Las siguientes secciones muestran la migración del proyecto Web API a ASP.NET Core MVC.

## <a name="create-destination-project"></a>Crear proyecto de destino

Complete los pasos siguientes en Visual Studio:

* Vaya a **archivo** > **nueva** > **proyecto** > **otros tipos de proyecto**  >  **Soluciones de visual Studio**. Seleccione **solución en blanco**y el nombre de la solución *WebAPIMigration*. Haga clic en el **Aceptar** botón.
* Agregar existente *ProductsApp* proyecto a la solución.
* Agregue un nuevo **aplicación Web ASP.NET Core** proyecto a la solución. Seleccione el **.NET Core** destino framework en la lista desplegable y seleccione el **API** plantilla de proyecto. Denomine el proyecto *ProductsCore*y haga clic en el **Aceptar** botón.

La solución contiene ahora dos proyectos. Las siguientes secciones explican migrar el *ProductsApp* contenido del proyecto para el *ProductsCore* proyecto.

## <a name="migrate-configuration"></a>Migración de la configuración

ASP.NET Core no usa el *App_Start* carpeta o el *Global.asax* archivo y el *web.config* se agrega al archivo en el momento de la publicación. *Startup.cs* es el reemplazo de *Global.asax* y se encuentra en la raíz del proyecto. La `Startup` clase controla todas las tareas de inicio de la aplicación. Para obtener más información, consulta <xref:fundamentals/startup>.

En ASP.NET Core MVC, el enrutamiento mediante atributos se incluye de forma predeterminada cuando <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> se denomina en `Startup.Configure`. La siguiente `UseMvc` llamar reemplaza el *ProductsApp* del proyecto *app_start/webapiconfig.cs* archivo:

[!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13])]

## <a name="migrate-models-and-controllers"></a>Migrar los modelos y controladores

Copiar a través de la *ProductApp* controlador del proyecto y el modelo que usa. Siga estos pasos:

1. Copia *Controllers/ProductsController.cs* desde el proyecto original a una nueva.
1. Copiar todo el *modelos* carpeta del proyecto original al nuevo.
1. Cambiar los espacios de nombres de los archivos copiados para que coincida con el nuevo nombre del proyecto (*ProductsCore*). Ajustar el `using ProductsApp.Models;` instrucción *ProductsController.cs* demasiado.

En este momento, compilar los resultados de la aplicación en un número de errores de compilación. Los errores se producen porque no existen los siguientes componentes en ASP.NET Core:

* Clase `ApiController`
* Espacio de nombres `System.Web.Http`
* `IHttpActionResult` Interfaz

Corrija los errores como sigue:

1. Cambio `ApiController` a <xref:Microsoft.AspNetCore.Mvc.ControllerBase>. Agregar `using Microsoft.AspNetCore.Mvc;` para resolver el `ControllerBase` referencia.
1. Elimine `using System.Web.Http;`.
1. Cambiar el `GetProduct` tipo de valor devuelto de la acción de `IHttpActionResult` a `ActionResult<Product>`.

Simplificar la `GetProduct` la acción `return` instrucción a la siguiente:

```csharp
return product;
```

## <a name="configure-routing"></a>Configurar el enrutamiento

Configurar el enrutamiento como sigue:

1. Decorar el `ProductsController` clase con los siguientes atributos:

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    Anterior [[ruta]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) atributo configura el modelo de enrutamiento de atributos del controlador. El [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) atributo hace que sea un requisito para todas las acciones de enrutamiento en este controlador de atributos.

    Enrutamiento mediante atributos admite tokens, como `[controller]` y `[action]`. En tiempo de ejecución, cada token se reemplaza por el nombre del controlador o acción, respectivamente, al que se ha aplicado el atributo. Los tokens de reducen el número de cadenas mágicas en el proyecto. Los tokens también Asegúrese de rutas sigan estando sincronizadas con los controladores correspondientes y se aplican acciones al automático cambiar el nombre de refactorizaciones.
1. Establece el modo de compatibilidad del proyecto en ASP.NET Core 2.2:

    [!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    El cambio anterior:

    * Se requiere para usar el `[ApiController]` atributo en el nivel de controlador.
    * Decida en que se generen los comportamientos que se introdujo en ASP.NET Core 2.2.
1. Habilitar las solicitudes HTTP Get a la `ProductController` acciones:
    * Aplicar el [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) atributo a la `GetAllProducts` acción.
    * Aplicar el `[HttpGet("{id}")]` atributo a la `GetProduct` acción.

Después de los cambios anteriores y la eliminación de sin usar `using` instrucciones, *ProductsController.cs* archivo tiene este aspecto:

[!code-csharp[](webapi/sample/ProductsCore/Controllers/ProductsController.cs)]

Ejecute el proyecto migrado y vaya a `/api/products`. Aparece una lista completa de los tres productos. Vaya a `/api/products/1`. El primer producto aparece.

## <a name="compatibility-shim"></a>Corrección de compatibilidad

El [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) biblioteca proporciona una corrección de compatibilidad para mover proyectos de ASP.NET 4.x Web API a ASP.NET Core. La corrección de compatibilidad amplía ASP.NET Core para admitir una serie de convenciones de ASP.NET 4.x Web API 2. El ejemplo portado anteriormente en este documento es bastante básico para la corrección de compatibilidad no era necesario. Para proyectos grandes, el uso de la corrección de compatibilidad puede ser útil para temporalmente reduciendo la brecha de API entre ASP.NET Core y ASP.NET 4.x Web API 2.

La corrección de compatibilidad de API Web está pensada para usarse como una medida temporal para admitir migración grandes proyectos ASP.NET 4.x Web API a ASP.NET Core. Con el tiempo, los proyectos deben actualizarse para utilizar patrones de ASP.NET Core en lugar de depender de la corrección de compatibilidad.

Características de compatibilidad que se incluyen en `Microsoft.AspNetCore.Mvc.WebApiCompatShim` incluyen:

* Agrega un `ApiController` del tipo para que los tipos de base de controladores no deben actualizarse.
* Habilita el enlace de modelo de estilo Web API. ASP.NET Core MVC modelar las funciones de enlace de forma similar al de ASP.NET 4.x MVC 5, de forma predeterminada. Los cambios de correcciones de compatibilidad de enlace para que sea más similar a las convenciones de enlace de modelo ASP.NET 4.x Web API 2 de modelos. Por ejemplo, los tipos complejos se enlazan automáticamente desde el cuerpo de solicitud.
* Amplía el enlace de modelos para que las acciones de controlador pueden tomar parámetros de tipo `HttpRequestMessage`.
* Agrega los formateadores de mensajes que permite las acciones para devolver los resultados de tipo `HttpResponseMessage`.
* Agrega métodos de respuesta adicionales que Web API 2 acciones que haya utilizado para atender respuestas:
  * `HttpResponseMessage` generadores de:
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * Métodos de resultados de acción:
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* Agrega una instancia de `IContentNegotiator` a la aplicación del contenedor de dependencias (DI) de la inyección de código y hace que estén disponibles los tipos relacionados con la negociación de contenido desde [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/). Ejemplos de tales tipos `DefaultContentNegotiator` y `MediaTypeFormatter`.

Para usar la corrección de compatibilidad:

1. Instalar el [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) paquete NuGet.
1. Registrar servicios de corrección de compatibilidad con el contenedor de DI de la aplicación mediante una llamada a `services.AddMvc().AddWebApiConventions()` en `Startup.ConfigureServices`.
1. Definir web específicos de la API se enruta mediante `MapWebApiRoute` en el `IRouteBuilder` en la aplicación `IApplicationBuilder.UseMvc` llamar.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:web-api/index>
* <xref:web-api/action-return-types>
* <xref:mvc/compatibility-version>
