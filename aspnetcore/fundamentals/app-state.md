---
title: Estado de sesión y aplicación en ASP.NET Core
author: rick-anderson
description: Detecte enfoques para conservar el estado de sesión y aplicación entre las solicitudes.
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2018
uid: fundamentals/app-state
ms.openlocfilehash: 072699113a45056ec3ea79436ad56896ba0a4197
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095819"
---
# <a name="session-and-app-state-in-aspnet-core"></a>Estado de sesión y aplicación en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose) y [Luke Latham](https://github.com/guardrex)

HTTP es un protocolo sin estado. Sin realizar pasos adicionales, las solicitudes HTTP son mensajes independientes que no conservan los valores de usuario ni el estado de la aplicación. En este artículo se describen varios enfoques para conservar el estado de la aplicación y los datos de usuario entre las solicitudes.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="state-management"></a>Administración de estado

El estado se puede almacenar mediante varios enfoques. Cada enfoque se describe más adelante en este tema.

| Enfoque de almacenamiento | Mecanismo de almacenamiento |
| ---------------- | ----------------- |
| [Cookies](#cookies) | Cookies HTTP (pueden incluir los datos almacenados mediante el código de aplicación del lado servidor) |
| [Estado de sesión](#session-state) | Cookies HTTP y código de aplicación del lado servidor |
| [TempData](#tempdata) | Cookies HTTP o estado de sesión |
| [Cadenas de consulta](#query-strings) | Cadenas de consulta HTTP |
| [Campos ocultos](#hidden-fields) | Campos de formularios HTTP |
| [HttpContext.Items](#httpcontextitems) | Código de aplicación del lado servidor |
| [Caché](#cache) | Código de aplicación del lado servidor |
| [Inserción de dependencias](#dependency-injection) | Código de aplicación del lado servidor |

## <a name="cookies"></a>Cookies

Las cookies almacenan datos de todas las solicitudes. Dado que las cookies se envían con cada solicitud, su tamaño debe reducirse al mínimo. Lo ideal es que en cada cookie se almacene un solo identificador con los datos almacenados por la aplicación. La mayoría de los exploradores restringen el tamaño de las cookies a 4096 bytes. Solo hay disponible un número limitado de cookies para cada dominio.

Como las cookies están expuestas a alteraciones, deben validarse por la aplicación. Los usuarios pueden eliminar las cookies y estas pueden caducar en los clientes. Pero las cookies generalmente son la forma más duradera de persistencia de datos en el cliente.

Las cookies suelen utilizarse para personalizar el contenido ofrecido a un usuario conocido. En la mayoría de los casos, el usuario solo se identifica y no se autentica. La cookie puede almacenar el nombre de usuario, el nombre de cuenta o el id. de usuario único (por ejemplo, un GUID). Después, puede usar la cookie para tener acceso a la configuración personalizada del usuario, como su color de fondo del sitio web preferido.

Preste atención al [reglamento general de protección de datos (GDPR) de la Unión Europea](https://ec.europa.eu/info/law/law-topic/data-protection) cuando emita cookies y trate con casos de privacidad. Para obtener más información, vea [Compatibilidad con el Reglamento general de protección de datos (RGPD) en ASP.NET Core](xref:security/gdpr).

## <a name="session-state"></a>Estado de sesión

El estado de sesión es un escenario de ASP.NET Core para el almacenamiento de datos de usuario mientras el usuario examina una aplicación web. El estado de sesión usa un almacén mantenido por la aplicación para conservar los datos en las solicitudes de un cliente. Los datos de sesión están respaldados por una memoria caché y se consideran datos efímeros y el sitio debería continuar funcionando correctamente sin los datos de sesión.

> [!NOTE]
> La sesión no es compatible con aplicaciones [SignalR](xref:signalr/index) porque un [concentrador SignalR](xref:signalr/hubs) podría ejecutarse independientemente de un contexto HTTP. Por ejemplo, esto puede ocurrir cuando un concentrador mantiene abierta una solicitud de sondeo larga más allá de la duración del contexto HTTP de la solicitud.

Para mantener el estado de sesión, ASP.NET Core proporciona una cookie al cliente que contiene un identificador de sesión, que se envía a la aplicación con cada solicitud. La aplicación usa el identificador de sesión para capturar los datos de sesión.

El estado de sesión muestra los siguientes comportamientos:

* Dado que la cookie de sesión es específica del explorador, las sesiones no se comparten entre los exploradores.
* Las cookies de sesión se eliminan cuando finaliza la sesión del explorador.
* Si se recibe una cookie de una sesión que ha expirado, se crea una nueva sesión que usa la misma cookie de sesión.
* Las sesiones vacías no se mantienen y la sesión debe tener, al menos un conjunto de valores en ella para que se conserve entre solicitudes. Cuando una sesión no se conserva, se genera un nuevo identificador de sesión para cada nueva solicitud.
* La aplicación conserva una sesión durante un tiempo limitado después de la última solicitud. La aplicación especifica un tiempo de espera de sesión o usa el valor predeterminado de 20 minutos. El estado de sesión es ideal para almacenar datos de usuario que son específicos de una sesión determinada, pero que no necesitan conservarse de forma permanente entre las sesiones.
* Los datos de sesión se eliminan cuando se llama a la implementación [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) o cuando expira la sesión.
* No hay ningún mecanismo predeterminado para informar al código de aplicación que se ha cerrado un explorador del cliente o cuando la cookie de sesión se elimina o caduca en el cliente.

> [!WARNING]
> No almacene datos confidenciales en un estado de sesión. El usuario podría no cerrar el explorador y borrar la cookie de sesión. Algunos exploradores mantienen las cookies de sesión válidas en las ventanas del explorador. Es posible que una sesión no esté restringida a un único usuario y que el siguiente usuario continúe examinando la aplicación con la misma cookie de sesión.

El proveedor de caché en memoria almacena datos de sesión en la memoria del servidor donde reside la aplicación. En un escenario de granja de servidores:

* Use *sesiones permanentes* para asociar cada sesión a una instancia de aplicación específica en un servidor individual. [Azure App Service](https://azure.microsoft.com/services/app-service/) usa [enrutamiento de solicitud de aplicaciones (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) para exigir sesiones permanentes de forma predeterminada. Pero las sesiones permanentes pueden afectar a la escalabilidad y complicar la actualización de las aplicaciones web. Un enfoque mejor consiste en usar una memoria caché distribuida de Redis o SQL Server, que no requiere sesiones permanentes. Para obtener más información, vea [Working with a Distributed Cache](xref:performance/caching/distributed) (Trabajar con una memoria caché distribuida).
* La cookie de sesión se cifra mediante [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector). La protección de datos debe configurarse correctamente para que lea las cookies de sesión en cada equipo. Para obtener más información, vea [Protección de datos en ASP.NET Core](xref:security/data-protection/index) y [Key storage providers](xref:security/data-protection/implementation/key-storage-providers) (Proveedores de almacenamiento de claves).

### <a name="configure-session-state"></a>Configurar el estado de sesión

::: moniker range=">= aspnetcore-2.0"

El paquete [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/), que se incluye en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), proporciona middleware para administrar el estado de sesión. Para habilitar el middleware de sesión, `Startup` debe contener:

::: moniker-end

::: moniker range="< aspnetcore-2.0"

El paquete [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) proporciona middleware para administrar el estado de sesión. Para habilitar el middleware de sesión, `Startup` debe contener:

::: moniker-end

* Cualquiera de las cachés de memoria [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache). La implementación de `IDistributedCache` se usa como una memoria auxiliar para la sesión. Para obtener más información, vea [Working with a Distributed Cache](xref:performance/caching/distributed) (Trabajar con una memoria caché distribuida).
* Una llamada a [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) en `ConfigureServices`.
* Una llamada a [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) en `Configure`.

El código siguiente muestra cómo configurar el proveedor de sesión en memoria con una implementación en memoria de `IDistributedCache`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13-18,39)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5,7-12,19)]

::: moniker-end

El orden del middleware es importante. En el ejemplo anterior, se produce una excepción `InvalidOperationException` cuando `UseSession` se invoca después de `UseMvc`. Para obtener más información vea [Ordenación de Middleware](xref:fundamentals/middleware/index#ordering).

[HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) está disponible después de configurar el estado de sesión.

No se puede acceder a `HttpContext.Session` antes de llamar a `UseSession`.

No se puede crear una nueva sesión con una cookie de sesión nueva después de que la aplicación haya empezado a escribir en la secuencia de respuesta. La excepción se registra en el registro del servidor web y no se muestra en el explorador.

### <a name="load-session-state-asynchronously"></a>Cargar de forma asincrónica el estado de sesión

El proveedor de sesión predeterminado de ASP.NET Core carga asincrónicamente los registros de sesión desde la memoria auxiliar subyacente [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) solo si se llama explícitamente al método [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) antes que a los métodos [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [Set](/dotnet/api/microsoft.aspnetcore.http.isession.set) o [Remove](/dotnet/api/microsoft.aspnetcore.http.isession.remove). Si primero no se llama a `LoadAsync`, el registro de sesión subyacente se carga de forma sincrónica, lo que podría conllevar una disminución del rendimiento al escalar.

Para que las aplicaciones impongan este patrón, ajuste las implementaciones de [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) y [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) con versiones que produzcan una excepción si el método `LoadAsync` no se llama antes que `TryGetValue`, `Set` o `Remove`. Registre las versiones ajustadas en el contenedor de servicios.

### <a name="session-options"></a>Opciones de sesión

Para reemplazar los valores predeterminados de la sesión, use [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).

::: moniker range=">= aspnetcore-2.0"

| Opción | Descripción |
| ------ | ----------- |
| [Cookie](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookie) | Determina la configuración usada para crear la cookie. [Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) tiene como valor predeterminado [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`). [Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) tiene como valor predeterminado [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`). [SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) tiene como valor predeterminado [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`). [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) tiene como valor predeterminado `true`. [IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) tiene como valor predeterminado `false`. |
| [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | `IdleTimeout` indica cuánto tiempo puede estar inactiva la sesión antes de que se abandone su contenido. Cada acceso a la sesión restablece el tiempo de espera. Tenga en cuenta que esto solo es aplicable al contenido de la sesión, no a la cookie. El valor predeterminado es de 20 minutos. |
| [IOTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.iotimeout) | El período máximo de tiempo permitido para cargar una sesión del almacén o devolverla a él. Tenga en cuenta que esto solo es aplicable a las operaciones asincrónicas. Puede deshabilitar este tiempo de espera mediante [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan). El valor predeterminado es 1 minuto. |

La sesión utiliza una cookie para realizar el seguimiento de las solicitudes emitidas por un solo explorador e identificarlas. De manera predeterminada, esta cookie se denomina `.AspNetCore.Session` y usa una ruta de acceso de `/`. Dado que el valor predeterminado de la cookie no especifica un dominio, no estará disponible para el script de cliente en la página (porque [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) tiene como valor predeterminado `true`).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| Opción | Descripción |
| ------ | ----------- |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiedomain) | Determina el dominio usado para crear la cookie. `CookieDomain` no se encuentra configurado de forma predeterminada. |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiehttponly) | Determina si el explorador debe permitir que la cookie tenga acceso a código JavaScript del lado cliente. El valor predeterminado es `true`, lo que significa que la cookie solo se pasa a las solicitudes HTTP y no está disponible para script en la página. |
| [CookieName](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiename) | Determina el nombre de cookie que se usa para conservar el identificador de sesión. El valor predeterminado es [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`). |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiepath) | Determina la ruta de acceso usada para crear la cookie. Tiene como valor predeterminado [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`). |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiesecure) | Determina si la cookie solo se debe transmitir en las solicitudes HTTPS. El valor predeterminado es [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`). |
| [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | `IdleTimeout` indica cuánto tiempo puede estar inactiva la sesión antes de que se abandone su contenido. Cada acceso a la sesión restablece el tiempo de espera. Tenga en cuenta que esto solo es aplicable al contenido de la sesión, no a la cookie. El valor predeterminado es de 20 minutos. |

La sesión utiliza una cookie para realizar el seguimiento de las solicitudes emitidas por un solo explorador e identificarlas. De manera predeterminada, esta cookie se denomina `.AspNet.Session` y usa una ruta de acceso de `/`.

::: moniker-end

Para reemplazar los valores predeterminados de la sesión de cookies, use `SessionOptions`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/2.x/SessionSample/Startup.cs?name=snippet1&highlight=13-18)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5-9)]

::: moniker-end

La aplicación usa la propiedad [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) para determinar el tiempo que una sesión puede estar inactiva antes de que se abandone el contenido de la caché de servidor. Esta propiedad es independiente de la expiración de la cookie. Cada solicitud que se pasa a través del [middleware de sesión](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) restablece el tiempo de espera.

El estado de sesión es *no realiza bloqueo*. Si dos solicitudes intentan modificar el contenido de una sesión simultáneamente, la última solicitud reemplaza a la primera. `Session` se implementa como una *sesión coherente*, lo que significa que todo el contenido se almacena junto. Cuando dos solicitudes buscan modificar diferentes valores de sesión, la última solicitud podría reemplazar los cambios de sesión realizados por la primera.

### <a name="set-and-get-session-values"></a>Establecer y obtener valores de Session

Se accede al estado de sesión desde una clase [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) de Razor Pages o una clase [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller) de MVC con [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session). Esta propiedad es una implementación de [ISession](/dotnet/api/microsoft.aspnetcore.http.isession).

::: moniker range=">= aspnetcore-2.0"

La implementación `ISession` proporciona varios métodos de extensión para establecer y recuperar valores de cadena y enteros. Los métodos de extensión están en el espacio de nombres [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) (agregue una instrucción `using Microsoft.AspNetCore.Http;` para obtener acceso a los métodos de extensión) cuando el proyecto hace referencia al paquete [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/). Ambos paquetes están incluidos en el metapaquete [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

La implementación `ISession` proporciona varios métodos de extensión para establecer y recuperar valores de cadena y enteros. Los métodos de extensión están en el espacio de nombres [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) (agregue una instrucción `using Microsoft.AspNetCore.Http;` para obtener acceso a los métodos de extensión) cuando el proyecto hace referencia al paquete [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/).

::: moniker-end

Métodos de extensión `ISession`:

* [Get(ISession, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.get)
* [GetInt32(ISession, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getint32)
* [GetString(ISession, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getstring)
* [SetInt32(ISession, String, Int32)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setint32)
* [SetString(ISession, String, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setstring)

En el ejemplo siguiente se recupera el valor de sesión para la clave `IndexModel.SessionKeyName` (`_Name` en la aplicación de ejemplo) en una página de Razor Pages:

```csharp
@page
@using Microsoft.AspNetCore.Http
@model IndexModel

...

Name: @HttpContext.Session.GetString(IndexModel.SessionKeyName)
```

En el ejemplo siguiente se muestra cómo establecer y obtener un entero y una cadena:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=18-19,22-23)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet1&highlight=10-11,18-19)]

::: moniker-end

Todos los datos de sesión se deben serializar para habilitar un escenario de caché distribuida, incluso cuando se usa la caché en memoria. Se proporcionan una cadena mínima y serializadores de número (vea los métodos y los métodos de extensión de [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)). El usuario debe serializar los tipos complejos mediante otro mecanismo, como JSON.

Agregue los siguientes métodos de extensión, para establecer y obtener objetos serializables:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

En el ejemplo siguiente se muestra cómo establecer y obtener un objeto serializable con los método de extensión:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet2&highlight=4,12)]

::: moniker-end

## <a name="tempdata"></a>TempData

ASP.NET Core expone la [propiedad TempData de un modelo de página de Razor Pages](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) o [TempData de un controlador de MVC](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata). Esta propiedad almacena datos hasta que se leen. Los métodos [Keep](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) y [Peek](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) se pueden usar para examinar los datos sin que se eliminen. TempData es particularmente útil para el redireccionamiento cuando se necesitan los datos para más de una única solicitud. Los proveedores de TempData implementan TempData mediante cookies o estado de sesión.

### <a name="tempdata-providers"></a>Proveedores de TempData

::: moniker range=">= aspnetcore-2.0"

En ASP.NET Core 2.0 y versiones posteriores, el proveedor TempData basado en cookies se usa de forma predeterminada para almacenar TempData en cookies.

Los datos de cookie se cifran mediante [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), codificado con [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), y después se fragmentan. Como la cookie está fragmentada, no se aplica el límite de tamaño único de cookie que se encuentra en ASP.NET Core 1.x. Los datos de cookie no se comprimen porque la compresión de datos cifrados puede provocar problemas de seguridad como los ataques [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) y [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)). Para obtener más información sobre el proveedor TempData basado en cookies, consulte [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

En ASP.NET Core 1.0 y 1.1, el proveedor TempData de estado de sesión es el proveedor predeterminado.

::: moniker-end

### <a name="choose-a-tempdata-provider"></a>Elegir un proveedor TempData

Elegir un proveedor TempData implica una serie de consideraciones:

1. ¿La aplicación ya usa el estado de sesión? Si es así, el uso del proveedor TempData de estado de sesión no tiene costo adicional para la aplicación (excepto en el tamaño de los datos).
2. ¿La aplicación usa TempData con moderación, solo para cantidades relativamente pequeñas de datos (hasta 500 bytes)? Si es así, el proveedor TempData de cookies agrega un pequeño costo a cada solicitud que transporta TempData. De lo contrario, el proveedor TempData de estado de sesión puede ser beneficioso para evitar que una gran cantidad de datos hagan un recorrido de ida y vuelta en cada solicitud hasta que se consuma TempData.
3. ¿La aplicación se ejecuta en una granja de servidores en varios servidores? Si es así, no es necesaria ninguna configuración adicional para usar el proveedor de TempData de cookies además de la protección de datos (vea [Protección de datos ](xref:security/data-protection/index) y [Key storage providers](xref:security/data-protection/implementation/key-storage-providers) (Proveedores de almacenamiento de claves)).

> [!NOTE]
> La mayoría de los clientes de web (por ejemplo, los exploradores web) aplican límites en el tamaño máximo de cada cookie, el número total de cookies o ambos. Cuando use el proveedor TempData de cookies, compruebe que la aplicación no supera esos límites. Tenga en cuenta el tamaño total de los datos. Cuenta para los aumentos de tamaño de cookie debidos a la fragmentación y el cifrado.

### <a name="configure-the-tempdata-provider"></a>Configurar el proveedor TempData

::: moniker range=">= aspnetcore-2.0"

El proveedor TempData basado en cookies está habilitado de forma predeterminada.

Para habilitar el proveedor TempData basado en sesión, use el método de extensión [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider):

[!code-csharp[](app-state/samples_snapshot_2/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13,32)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

El siguiente código de clase `Startup` configura el proveedor TempData basado en sesión:

[!code-csharp[](app-state/samples_snapshot_2/1.x/SessionSample/Startup.cs?name=snippet1&highlight=4,9)]

::: moniker-end

El orden del middleware es importante. En el ejemplo anterior, se produce una excepción `InvalidOperationException` cuando `UseSession` se invoca después de `UseMvc`. Para obtener más información vea [Ordenación de Middleware](xref:fundamentals/middleware/index#ordering).

> [!IMPORTANT]
> Si el destino es .NET Framework y usa el proveedor TempData basado en sesión, agregue el paquete [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) al proyecto.

## <a name="query-strings"></a>Cadenas de consulta

Se puede pasar una cantidad limitada de datos de una solicitud a otra si agrega los datos a la cadena de consulta de la solicitud nueva. Esto es útil para capturar el estado de una forma persistente que permita que los vínculos con estado insertado se compartan a través del correo electrónico o las redes sociales. Dado que las cadenas de consulta de direcciones URL son públicas, nunca use las cadenas de consulta para datos confidenciales.

Además del uso compartido no intencionado, la inclusión de datos en las cadenas de consulta puede propiciar ataques de [falsificación de solicitud entre sitios (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)), cuya intención es engañar a los usuarios para que visiten sitios malintencionados mientras están autenticados. Después, los atacantes pueden robar los datos de usuario de la aplicación o realizar acciones malintencionadas en nombre del usuario. Cualquier estado de sesión o aplicación conservado debe protegerse contra los ataques CSRF. Para más información, vea [Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks](xref:security/anti-request-forgery) (Evitar los ataques de falsificación de solicitud entre sitios [XSRF/CSRF]).

## <a name="hidden-fields"></a>Campos ocultos

Los datos pueden guardarse en campos ocultos de formulario e incluirse de nuevo en la siguiente solicitud. Esto es habitual en los formularios de varias páginas. Dado que el cliente puede llegar a alterar los datos, la aplicación siempre debe revalidar los datos almacenados en campos ocultos.

## <a name="httpcontextitems"></a>HttpContext.Items

La colección [HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) se usa para almacenar los datos al procesar una única solicitud. El contenido de la colección se descarta después de procesar una solicitud. A menudo se usa la colección `Items` para permitir que los componentes o el middleware se comuniquen cuando operan en distintos puntos en el tiempo durante una solicitud y no pueden pasarse parámetros de forma directa.

En el ejemplo siguiente, [Middleware](xref:fundamentals/middleware/index) agrega `isVerified` a la colección `Items`.

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

Más adelante en la canalización, otro middleware puede acceder al valor de `isVerified`:

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync($"Verified: {context.Items["isVerified"]}");
});
```

Si el middleware solo se usa en una única aplicación, se aceptan claves `string`. El middleware compartido entre instancias de aplicación debería usar claves de objeto únicas para evitar conflictos de clave. En el ejemplo siguiente se muestra cómo usar una clave de objeto única definida en una clase de middleware:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=4,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=5,14)]

::: moniker-end

Otro código puede tener acceso al valor almacenado en `HttpContext.Items` con la clave que expone la clase de middleware:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet3)]

::: moniker-end

Este enfoque también tiene la ventaja de eliminar el uso de cadenas de claves en el código.

## <a name="cache"></a>instancias y claves

El almacenamiento en caché es una manera eficaz de almacenar y recuperar datos. La aplicación puede controlar la duración de los elementos almacenados en caché.

Los datos almacenados en caché no están asociados a una solicitud, usuario o sesión específicos. **Procure no almacenar en caché datos específicos de usuario que podrían recuperar las solicitudes de otros usuarios.**

Para obtener más información, vea el tema [Almacenamiento en caché de respuestas](xref:performance/caching/index).

## <a name="dependency-injection"></a>Inserción de dependencias

Use [inserción de dependencias](xref:fundamentals/dependency-injection) para que los datos estén disponibles para todos los usuarios:

1. Definir un servicio que contiene los datos. Por ejemplo, una clase denominada `MyAppData` se define:

    ```csharp
    public class MyAppData
    {
        // Declare properties and methods
    }
    ```

2. Agregue la clase de servicio a `Startup.ConfigureServices`:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<MyAppData>();
    }
    ```

3. Use la clase de servicio de datos:

    ::: moniker range=">= aspnetcore-2.0"

    ```csharp
    public class IndexModel : PageModel
    {
        public IndexModel(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

    ::: moniker range="< aspnetcore-2.0"

    ```csharp
    public class HomeController : Controller
    {
        public HomeController(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

## <a name="common-errors"></a>Errores comunes

* "No se puede resolver el servicio para el tipo 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' al intentar activar 'Microsoft.AspNetCore.Session.DistributedSessionStore'".

  Esto puede deberse a que no se ha configurado al menos una implementación `IDistributedCache`. Para obtener más información, vea [Working with a Distributed Cache](xref:performance/caching/distributed) (Trabajar con una memoria caché distribuida) e [In memory caching](xref:performance/caching/memory) (Almacenamiento en memoria caché).

* En caso de que el middleware de sesión no logre conservar una sesión (por ejemplo, si la memoria auxiliar no está disponible), el middleware registra la excepción y la solicitud continúa con normalidad. Esto provoca un comportamiento imprevisible.

  Por ejemplo, un usuario almacena un carro de la compra en la sesión. El usuario agrega un elemento al carro, pero se produce un error en la confirmación. La aplicación no se percata del error y notifica al usuario que el elemento se ha agregado al carro, lo cual no es cierto.

  El enfoque recomendado para comprobar los errores es llamar a `await feature.Session.CommitAsync();` desde el código de la aplicación cuando esta haya terminado de escribir en la sesión. `CommitAsync` produce una excepción si la memoria auxiliar no está disponible. Si `CommitAsync` produce un error, la aplicación puede procesar la excepción. `LoadAsync` se produce en las mismas condiciones donde el almacén de datos no está disponible.

## <a name="additional-resources"></a>Recursos adicionales

<xref:host-and-deploy/web-farm>
