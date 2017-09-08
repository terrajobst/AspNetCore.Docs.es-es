---
title: "Estado de sesión y la aplicación de ASP.NET Core"
author: rick-anderson
description: "Enfoques para la conservación de aplicación y el estado de usuario (sesión) entre las solicitudes."
keywords: "Núcleo de ASP.NET, estado de la aplicación, el estado de sesión, la cadena de consulta, registrar"
ms.author: riande
manager: wpickett
ms.date: 06/08/2017
ms.topic: article
ms.assetid: 18cda488-0769-4cb9-82f6-4c6685f2045d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/app-state
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bdc93b2c06b7f0314b5bf49e0e3ea5aa3c1eb3cf
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a>Introducción al estado de sesión y la aplicación de ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](http://ardalis.com), y [Diana LaRose](https://github.com/DianaLaRose)

HTTP es un protocolo sin estado. Un servidor web trata cada solicitud HTTP como una solicitud independiente y no retiene los valores de usuario de las solicitudes anteriores. Este artículo describe diferentes maneras de mantener la aplicación y el estado de sesión entre las solicitudes. 

## <a name="session-state"></a>Estado de sesión

Estado de sesión es una característica de ASP.NET Core que puede usar para guardar y almacenar datos de usuario mientras el usuario explora la aplicación web. Estado de sesión que consta de una diccionario o tabla hash en el servidor, conserva los datos de todas las solicitudes desde un explorador. Los datos de sesión está respaldados por una memoria caché.

ASP.NET Core mantiene el estado de sesión proporcionando una cookie que contiene el identificador de sesión, que se envía al servidor con cada solicitud de cliente. El servidor utiliza el identificador de sesión para capturar los datos de sesión. Dado que la cookie de sesión es específica del explorador, no puede compartir las sesiones entre los exploradores. Las cookies de sesión se eliminan cuando finaliza la sesión del explorador. Si se recibe una cookie de una sesión que ha expirado, se crea una nueva sesión que utiliza la misma cookie de sesión. 

El servidor conserva una sesión durante un tiempo limitado después de la última solicitud. Puede establecer el tiempo de espera de sesión o usar el valor predeterminado de 20 minutos. Estado de sesión es ideal para almacenar datos de usuario que es específico para una sesión determinada, pero no necesitan que se deben conservar de forma permanente. Datos se eliminan de la memoria auxiliar o cuando se llama a `Session.Clear` o cuando expira la sesión en el almacén de datos. El servidor no conoce cuando se cierra el explorador o cuando se elimina la cookie de sesión.

> [!WARNING]
> No almacene datos confidenciales en la sesión. El cliente no puede cerrar el explorador y desactive la cookie de sesión (y algunos exploradores mantener las cookies de sesión activa a través de windows). Además, una sesión podría no estar restringida a un único usuario; el siguiente usuario puede continuar con la misma sesión.

El proveedor de sesión en memoria almacena datos de la sesión en el servidor local. Si tiene previsto ejecutar la aplicación web en una granja de servidores, debe usar sesiones permanentes para asociar cada sesión en un servidor específico. El valor predeterminado de la plataforma de sitios Web Windows Azure es sesiones permanentes (enrutamiento de solicitud de aplicación o ARR). Sin embargo, sesiones permanentes pueden afectar a la escalabilidad y complicar las actualizaciones de aplicaciones web. Una mejor opción consiste en usar Redis o distribuidas de SQL Server almacena en caché, que no requieren sesiones permanentes. Para obtener más información, consulte [trabajar con una memoria caché distribuida](xref:performance/caching/distributed). Para obtener más información sobre cómo configurar los proveedores de servicios, consulte [configuración de sesión](#configuring-session) más adelante en este artículo.

El resto de esta sección describe las opciones para almacenar datos de usuario.

<a name="temp"></a>
### <a name="tempdata"></a>TempData

MVC de ASP.NET Core expone la [TempData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller#Microsoft_AspNetCore_Mvc_Controller_TempData) propiedad en un [controlador](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller). Esta propiedad almacena los datos hasta que se lee. El `Keep` y `Peek` métodos se pueden utilizar para examinar los datos sin su eliminación. `TempData`es especialmente útil para la redirección cuando se necesitan datos de más de una única solicitud. `TempData`se basa en el estado de sesión. 

## <a name="cookie-based-tempdata-provider"></a>Proveedor de TempData basado en cookies 

En ASP.NET Core 1.1 y versiones posteriores, puede usar el proveedor TempData basado en cookies para almacenar TempData de un usuario en una cookie. Para habilitar el proveedor TempData basado en cookies, registre el `CookieTempDataProvider` servicio en `ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    // Add CookieTempDataProvider after AddMvc and include ViewFeatures.
    // using Microsoft.AspNetCore.Mvc.ViewFeatures;
    services.AddSingleton<ITempDataProvider, CookieTempDataProvider>();
}
```

Los datos de la cookie se codifican con el [Base64UrlTextEncoder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.authentication.base64urltextencoder). Dado que la cookie se cifra y fragmentada, el límite de tamaño de cookie único no se aplica. No se comprimieron los datos de cookies, porque la compresión de datos de encryped puede provocar problemas de seguridad como la [CRIME](https://en.wikipedia.org/wiki/CRIME_(security_exploit)) y [infracción](https://en.wikipedia.org/wiki/BREACH_(security_exploit)) ataques. Para obtener más información sobre el proveedor TempData basado en cookies, consulte [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).

### <a name="query-strings"></a>Cadenas de consulta

Puede pasar una cantidad limitada de datos de una solicitud a otro, éste se agrega a la cadena de consulta de la solicitud nuevo. Esto es útil para capturar el estado de forma persistente que permite vínculos de estado incrustada para compartirse a través de correo electrónico o redes sociales. Sin embargo, por esta razón, nunca debe usar las cadenas de consulta para datos confidenciales. Además de que se va a compartir con facilidad, incluidos los datos de las cadenas de consulta pueden crear oportunidades para [falsificación de solicitud entre sitios (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) ataques, que pueden engañar a los usuarios visitar sitios malintencionados mientras autenticado. Los atacantes, a continuación, pueden robar datos de usuario de la aplicación o realizar acciones malintencionadas en nombre del usuario. Cualquier estado conservado de application o session debe protegerse de ataques CSRF. Para obtener más información sobre los ataques CSRF, consulte [evitar Cross-Site falsificación de solicitud (XSRF/CSRF) ataques en ASP.NET Core](../security/anti-request-forgery.md).

### <a name="post-data-and-hidden-fields"></a>Datos POST y los campos ocultos

Datos pueden guardarse en campos ocultos de formulario y registrados en la siguiente solicitud. Esto es habitual en los formularios de varias páginas. Sin embargo, dado que el cliente potencialmente puede alterar los datos, el servidor debe siempre revalidación. 

### <a name="cookies"></a>Cookies

Las cookies proporcionan una manera de almacenar datos específicos del usuario en aplicaciones web. Dado que las cookies se envían con cada solicitud, su tamaño debe reducirse al mínimo. Idealmente, solo un identificador debe almacenarse en una cookie con los datos almacenados en el servidor. Mayoría de los exploradores restringir las cookies de 4096 bytes. Además, solo un número limitado de las cookies está disponible para cada dominio.  

Dado que las cookies están expuestas a alteraciones, deben validarse en el servidor. Aunque la duración de la cookie en un cliente está sujeto a expiración y la intervención del usuario, generalmente son el formulario más duradero para la persistencia de los datos en el cliente.

A menudo se usan cookies para personalización, donde el contenido se personaliza para un usuario conocido. Dado que el usuario se identifica únicamente y no autenticado en la mayoría de los casos, normalmente puede proteger una cookie al almacenar el nombre de usuario, nombre de cuenta o un identificador de usuario único (por ejemplo, un GUID) en la cookie. A continuación, puede usar la cookie para tener acceso a la infraestructura de personalización de usuario de un sitio.

### <a name="httpcontextitems"></a>HttpContext.Items

El `Items` colección es un buen lugar para almacenar los datos que es solo necesarios mientras el procesamiento de una solicitud determinada. Contenido de la colección se descarta después de cada solicitud. El `Items` colección mejor se usa como una manera de componentes o de middleware para comunicarse cuando operar en distintos puntos en el tiempo durante una solicitud y no pueden pasar parámetros de forma directa. Para obtener más información, consulte [trabajar con HttpContext.Items](#working-with-httpcontextitems), más adelante en este artículo.

### <a name="cache"></a>instancias y claves

Almacenamiento en caché es una manera eficaz para almacenar y recuperar datos. Puede controlar la duración de los elementos almacenados en caché en función de tiempo y otras consideraciones. Obtenga más información sobre [almacenamiento en caché](../performance/caching/index.md).

<a name=session></a>

## <a name="configuring-session"></a>Configuración de sesión

El `Microsoft.AspNetCore.Session` paquete proporciona middleware para administrar el estado de sesión. Para habilitar el middleware de sesión, `Startup`debe contener:

- Cualquiera de los [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) cachés de la memoria. El `IDistributedCache` implementación se usa como una memoria auxiliar para la sesión.
- [AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) llamar a, lo que requiere el paquete NuGet "Microsoft.AspNetCore.Session".
- [UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) llamar.

El código siguiente muestra cómo configurar el proveedor de sesión en memoria.

[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

Puede hacer referencia a la sesión de `HttpContext` una vez que está instalado y configurado.

Si se intenta acceder a `Session` antes de `UseSession` se ha llamado, la excepción `InvalidOperationException: Session has not been configured for this application or request` se produce.

Si intenta crear un nuevo `Session` (es decir, no se ha creado ninguna cookie de sesión) después de que ya ha empezado a escribir en el `Response` transmitir por secuencias, la excepción `InvalidOperationException: The session cannot be established after the response has started` se produce. La excepción puede encontrarse en el registro de servidor web; no se mostrará en el explorador.

### <a name="loading-session-asynchronously"></a>Carga de sesión de forma asincrónica 

El proveedor de sesión predeterminado de ASP.NET Core carga el registro de sesión de subyacente [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) almacén asincrónicamente solo si la [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) método se llama explícitamente antes de  el `TryGetValue`, `Set`, o `Remove` métodos. Si `LoadAsync` no se llama en primer lugar, subyacente registro de sesión se carga de forma sincrónica, que podrían tener consecuencias sobre la capacidad de ampliación de la aplicación.

Para que las aplicaciones aplicar este patrón, ajustar el [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) y [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implementaciones con versiones que producen una excepción si el `LoadAsync` método no es llamado antes de `TryGetValue`, `Set`, o `Remove`. Registrar las versiones ajustadas en el contenedor de servicios.

### <a name="implementation-details"></a>Detalles de implementación

Sesión utiliza una cookie para realizar un seguimiento e identificar las solicitudes de un solo explorador. De manera predeterminada, esta cookie se denomina ". AspNet.Session"y utiliza una ruta de acceso de"/". Dado que el valor predeterminado de la cookie no especifica un dominio, no estará disponible para el script de cliente en la página (porque `CookieHttpOnly` tiene como valor predeterminado `true`).

Para reemplazar los valores predeterminados de la sesión, utilice `SessionOptions`:

[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

El servidor usa el `IdleTimeout` propiedad para determinar el tiempo que una sesión puede estar inactiva antes de que se abandone su contenido. Esta propiedad es independiente de la expiración de la cookie. Todas las solicitudes que se pasan a través del middleware de sesión (de lectura o escritura para) restablece el tiempo de espera.

Dado que `Session` es *no realiza el bloqueo*, si dos solicitudes intentan modificar el contenido de la sesión, la última de ellas sustituye a la primera. `Session`se implementa como un *sesión coherente*, lo que significa que todo el contenido se almacena juntos. Dos solicitudes que están modificando distintas partes de la sesión (claves diferentes) aún podrían verse afectadas entre sí.

## <a name="setting-and-getting-session-values"></a>Establecer u obtener valores de la sesión

Sesión se accede a través del `Session` propiedad `HttpContext`. Esta propiedad es una [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implementación.

En el ejemplo siguiente se muestra cómo establecer y obtener un valor int y una cadena:

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet1)]

Si agrega los siguientes métodos de extensión, puede establecer y obtener objetos serializables en sesión:

[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

El ejemplo siguiente muestra cómo establecer y obtener un objeto serializable:

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a>Trabajar con HttpContext.Items

El `HttpContext` abstracción proporciona compatibilidad para una colección de diccionario de tipo `IDictionary<object, object>`, llamado `Items`. Esta colección está disponible desde el principio de un *HttpRequest* y se descarta al final de cada solicitud. Puede tener acceso a asignar un valor a una entrada con clave, o solicitando el valor de una clave determinada.

En el ejemplo siguiente, [Middleware](middleware.md) agrega `isVerified` a la `Items` colección.

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

Más adelante en la canalización, middleware otro podría acceder a él:

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

Para el middleware que solo se usará en una única aplicación, `string` claves son aceptables. Sin embargo, middleware que se compartirá entre las aplicaciones debe usar claves de objeto único para evitar cualquier posibilidad de colisiones de clave. Si está desarrollando middleware que debe trabajar en varias aplicaciones, utilice una clave de objeto único definida en la clase de middleware tal y como se muestra a continuación:

```csharp
public class SampleMiddleware
{
    public static readonly object SampleKey = new Object();

    public async Task Invoke(HttpContext httpContext)
    {
        httpContext.Items[SampleKey] = "some value";
        // additional code omitted
    }
}
```

Otro código puede tener acceso al valor almacenado en `HttpContext.Items` con la clave que expone la clase de middleware:

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

Este enfoque también tiene la ventaja de eliminar la repetición de cadenas"mágicas" en varios lugares en el código.

<a name=appstate-errors></a>

## <a name="application-state-data"></a>Datos de estado de aplicación

Use [inyección de dependencia](xref:fundamentals/dependency-injection) para que los datos estén disponibles para todos los usuarios:

1. Definir un servicio que contiene los datos (por ejemplo, una clase denominada `MyAppData`).

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. Agregue la clase de servicio a `ConfigureServices` (por ejemplo `services.AddSingleton<MyAppData>();`).
3. Utilizar la clase de servicio de datos en cada controlador:

```csharp
public class MyController : Controller
{
    public MyController(MyAppData myService)
    {
        // Do something with the service (read some data from it, 
        // store it in a private field/property, etc.)
    }
} 
```

### <a name="common-errors-when-working-with-session"></a>Errores comunes al trabajar con sesión

* "No se puede resolver el servicio para el tipo 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' al intentar activar 'Microsoft.AspNetCore.Session.DistributedSessionStore'."

  Esto puede deberse a que si no se configura al menos una `IDistributedCache` implementación. Para obtener más información, consulte [trabajar con una memoria caché distribuida](xref:performance/caching/distributed) y [en la memoria caché](xref:performance/caching/memory).

### <a name="additional-resources"></a>Recursos adicionales


* [Código de ejemplo usado en este documento](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
