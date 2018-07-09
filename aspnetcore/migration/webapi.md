---
title: Migrar desde ASP.NET Web API a ASP.NET Core
author: ardalis
description: Obtenga información sobre cómo migrar una implementación de la API Web de ASP.NET Web API a ASP.NET Core MVC.
ms.author: riande
ms.date: 05/10/2018
uid: migration/webapi
ms.openlocfilehash: 8dd969c8644525606227989ca87e41fbfae5aed1
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894197"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>Migrar desde ASP.NET Web API a ASP.NET Core

Por [Steve Smith](https://ardalis.com/) y [Scott Addie](https://scottaddie.com)

API de Web son servicios HTTP que lleguen a una amplia gama de clientes, incluidos los exploradores y dispositivos móviles. ASP.NET Core MVC incluye compatibilidad para la creación de API Web que proporciona una forma única y coherente de la creación de aplicaciones web. En este artículo, se muestran los pasos necesarios para migrar una implementación de la API Web de ASP.NET Web API a ASP.NET Core MVC.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="review-aspnet-web-api-project"></a>Proyecto de ASP.NET Web API de revisión

Este artículo usa el proyecto de ejemplo, *ProductsApp*, creado en el artículo [Introducción a ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) como punto de partida. En ese proyecto, un proyecto de ASP.NET Web API sencillo se configura como se indica a continuación.

En *Global.asax.cs*, se realiza una llamada a `WebApiConfig.Register`:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig` se define en *App_Start*, y tiene solo uno estático `Register` método:

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]

Esta clase configura [enrutamiento mediante atributos](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), aunque realmente no se usa en el proyecto. También configura la tabla de enrutamiento, que es utilizada por ASP.NET Web API. En este caso, ASP.NET Web API esperará a direcciones URL para que coincida con el formato */api/ {controller} / {id}*, con *{id}* que es opcional.

El *ProductsApp* proyecto incluye un solo controlador simple, que hereda de `ApiController` y expone dos métodos:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

Por último, el modelo, *producto*, utilizada por el *ProductsApp*, es una clase sencilla:

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

Ahora que tenemos un proyecto simple que se inicia, podemos demostramos cómo migrar este proyecto Web API a ASP.NET Core MVC.

## <a name="create-the-destination-project"></a>Crear el proyecto de destino

Con Visual Studio, cree una solución nueva y vacía y asígnele el nombre *WebAPIMigration*. Agregar existente *ProductsApp* proyecto a él, a continuación, agregue un nuevo proyecto de aplicación de ASP.NET Core Web a la solución. Asigne al nuevo proyecto *ProductsCore*.

![Abrir el cuadro de diálogo nuevo proyecto en las plantillas Web](webapi/_static/add-web-project.png)

A continuación, elija la plantilla de proyecto Web API. Se migrará el *ProductsApp* contenido a este nuevo proyecto.

![Cuadro de diálogo nueva aplicación Web con la plantilla de proyecto de API Web seleccionada en la lista de plantillas de ASP.NET Core](webapi/_static/aspnet-5-webapi.png)

Eliminar el `Project_Readme.html` archivo desde el nuevo proyecto. La solución ahora un aspecto similar al siguiente:

![Solución de aplicación abierta en el Explorador de soluciones con archivos y carpetas de los proyectos ProductsApp y ProductsCore](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a>Migración de la configuración

ASP.NET Core ya no usa *Global.asax*, *web.config*, o *App_Start* carpetas. En su lugar, se realizan todas las tareas de inicio en *Startup.cs* en la raíz del proyecto (vea [inicio de la aplicación](../fundamentals/startup.md)). En ASP.NET Core MVC, el enrutamiento basado en atributo ahora se incluye de forma predeterminada cuando `UseMvc()` se denomina; y, esto es el enfoque recomendado para configurar las rutas de la API Web (y es la forma en que el proyecto de inicio de la API Web controla el enrutamiento).

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

Suponiendo que desea usar el enrutamiento mediante atributos en el proyecto a partir de ahora, se necesita ninguna configuración adicional. Solo tiene que aplicar los atributos según sea necesario para los controladores y acciones, como se hace en el ejemplo `ValuesController` clase que se incluye en el proyecto de inicio de la API Web:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

Tenga en cuenta la presencia de *[controller]* en la línea 8. Enrutamiento basado en atributo ahora es compatible con ciertos tokens, como *[controller]* y *[action]*. Estos tokens se reemplazan en tiempo de ejecución con el nombre del controlador o acción, respectivamente, al que se ha aplicado el atributo. Esto sirve para reducir el número de cadenas mágicas en el proyecto y se garantiza que las rutas se mantienen sincronizadas con sus correspondientes controladores y acciones cuando se aplican las refactorizaciones de cambio de nombre automático.

Para migrar el controlador de API de productos, nos debemos copiar *ProductsController* al nuevo proyecto. A continuación, basta con incluir el atributo de ruta en el controlador:

```csharp
[Route("api/[controller]")]
```

También deberá agregar el `[HttpGet]` atribuir a los dos métodos, ya que ambos se deben llamar a través de HTTP Get. Incluir la expectativa de un parámetro "id" en el atributo para `GetProduct()`:

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

En este momento, el enrutamiento está configurado correctamente; Sin embargo, aún no podemos probarlo. Se deben realizar cambios adicionales antes de *ProductsController* se compilará.

## <a name="migrate-models-and-controllers"></a>Migrar los modelos y controladores

El último paso del proceso de migración para este proyecto Web API sencilla es copiar a través de los controladores y los modelos que usan. En este caso, simplemente copie *Controllers/ProductsController.cs* desde el proyecto original a una nueva. A continuación, copie toda la carpeta modelos del proyecto original al nuevo. Ajustar los espacios de nombres para que coincida con el nuevo nombre del proyecto (*ProductsCore*).  En este momento, puede compilar la aplicación, y encontrará un número de errores de compilación. Estos deben generalmente se dividen en las siguientes categorías:

* *ApiController* no existe

* *System.Web.Http* espacio de nombres no existe.

* *IHttpActionResult* no existe

Afortunadamente, estos son muy fáciles corregir:

* Cambio *ApiController* a *controlador* (es posible que deba agregar *mediante Microsoft.AspNetCore.Mvc*)

* Eliminar cualquiera con la instrucción que hace referencia a *System.Web.Http*

* Cambiar a cualquier método de devolución *IHttpActionResult* para devolver un *IActionResult*

Una vez que estos cambios se han realizado y que no use las instrucciones using quitado, el migrados *ProductsController* clase tiene este aspecto:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

Ahora podrá ejecutar el proyecto migrado y vaya a */api/productos*; y debería ver la lista completa de los 3 productos. Vaya a */api/products/1* y debería ver el primer producto.

## <a name="aspnet-4x-web-api-2-compatibility-shim"></a>Corrección de compatibilidad ASP.NET 4.x Web API 2

Es una herramienta muy útil cuando los proyectos de migración de ASP.NET Web API a ASP.NET Core el [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) biblioteca. La corrección de compatibilidad amplía ASP.NET Core para permitir a un número de diferentes convenciones de Web API 2 que se usará. El ejemplo portado anteriormente en este documento es bastante básico que la corrección de compatibilidad no era necesaria. Para proyectos grandes, el uso de la corrección de compatibilidad puede ser útil para temporalmente reduciendo la brecha de API entre ASP.NET Core y ASP.NET Web API 2.

La corrección de compatibilidad de API Web está pensada para usarse como una medida temporal para facilitar la migración de grandes proyectos de Web API a ASP.NET Core. Con el tiempo, los proyectos deben actualizarse para utilizar patrones de ASP.NET Core en lugar de depender de la corrección de compatibilidad.

Características de compatibilidad que se incluyen en `Microsoft.AspNetCore.Mvc.WebApiCompatShim` incluyen:

* Agrega un `ApiController` del tipo para que los tipos de base de controladores no deben actualizarse.
* Habilita el enlace de modelo de estilo Web API. ASP.NET Core MVC modelar las funciones de enlace de forma similar a MVC 5, de forma predeterminada. Los cambios de correcciones de compatibilidad de enlace para que sea más similar a las convenciones de enlace de modelo de Web API 2 de modelos. Por ejemplo, los tipos complejos se enlazan automáticamente desde el cuerpo de solicitud.
* Amplía el enlace de modelos para que las acciones de controlador pueden tomar parámetros de tipo `HttpRequestMessage`.
* Agrega los formateadores de mensajes que permite las acciones para devolver los resultados de tipo `HttpResponseMessage`.
* Agrega métodos de respuesta adicionales que Web API 2 acciones que haya utilizado para atender respuestas:
  * Generadores de HttpResponseMessage:
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * Métodos de resultados de acción:
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* Agrega una instancia de `IContentNegotiator` al contenedor de inserción de dependencias de la aplicación y tipos relacionados con la negociación de contenido de hace [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) disponibles. Esto incluye tipos como `DefaultContentNegotiator`, `MediaTypeFormatter`, etcetera.

Para usar la corrección de compatibilidad, es preciso:

* Instalar el [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) paquete NuGet.
* Registrar servicios de corrección de compatibilidad con el contenedor de DI de la aplicación mediante una llamada a `services.AddMvc().AddWebApiConventions()` en la aplicación `Startup.ConfigureServices` método.
* Definir rutas específicas de Web API mediante `MapWebApiRoute` en el `IRouteBuilder` en la aplicación `IApplicationBuilder.UseMvc` llamar.

## <a name="summary"></a>Resumen

Migrar un proyecto de ASP.NET Web API simple para ASP.NET Core MVC es bastante sencillo, gracias a la compatibilidad integrada para las API Web en ASP.NET Core MVC. Las partes fundamentales, que deberá migrar cada proyecto de ASP.NET Web API son las rutas, controladores y modelos, junto con las actualizaciones de los tipos utilizados por los controladores y acciones.
