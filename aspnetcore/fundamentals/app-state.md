---
title: Estado de sesión y aplicación en ASP.NET Core
author: rick-anderson
description: Enfoques para la conservación del estado de la aplicación y el usuario (sesión) entre solicitudes.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/app-state
ms.openlocfilehash: 887aefdeaa45957f7b95bfe8df342eb34d267e3a
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/18/2018
ms.locfileid: "34306652"
---
# <a name="session-and-application-state-in-aspnet-core"></a>Estado de sesión y aplicación en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/) y [Diana LaRose](https://github.com/DianaLaRose)

HTTP es un protocolo sin estado. Un servidor web trata de manera independiente cada solicitud HTTP y no conserva los valores de usuario de las solicitudes anteriores. En este artículo se describen diferentes maneras de mantener el estado sesión y aplicación entre las solicitudes.

## <a name="session-state"></a>Estado de sesión

El estado de sesión es una característica de ASP.NET Core que se puede usar para guardar y almacenar datos de usuario mientras el usuario explora la aplicación web. El estado de sesión, que está compuesto por un diccionario o tabla hash en el servidor, conserva los datos de todas las solicitudes desde un explorador. Los datos de sesión se guardan en una copia de seguridad en una memoria caché.

Para mantener el estado de sesión, ASP.NET Core proporciona al cliente una cookie que contiene el identificador de sesión, que se envía al servidor con cada solicitud. El servidor utiliza el identificador de sesión para capturar los datos de sesión. Dado que la cookie de sesión es específica del explorador, no es posible compartir las sesiones entre los exploradores. Las cookies de sesión se eliminan cuando finaliza la sesión del explorador. Si se recibe una cookie de una sesión que ha expirado, se crea una nueva sesión que usa la misma cookie de sesión.

El servidor conserva una sesión durante un tiempo limitado después de la última solicitud. Se puede especificar un tiempo de espera de sesión o usar el valor predeterminado de 20 minutos. El estado de sesión es ideal para almacenar datos de usuario que son específicos de una sesión determinada, pero que no necesitan conservarse de forma permanente. Los datos se eliminan de la memoria auxiliar cuando se llama a `Session.Clear` o cuando la sesión expira en el almacén de datos. El servidor no sabe cuándo se cierra el explorador o cuándo se elimina la cookie de sesión.

> [!WARNING]
> No almacene datos confidenciales en la sesión, dado que el cliente podría no cerrar el explorador ni borrar la cookie de sesión (además, algunos exploradores mantienen las cookies de sesión activas en distintas ventanas). También es posible que una sesión no esté restringida a un único usuario y que el siguiente usuario continúe con la misma sesión.

El proveedor de sesión en memoria almacena datos de la sesión en el servidor local. Si tiene previsto ejecutar la aplicación web en una granja de servidores, debe usar sesiones permanentes para asociar cada sesión a un servidor específico. El valor predeterminado de la plataforma Windows Azure Web Sites es usar sesiones permanentes (enrutamiento de solicitud de aplicación o ARR). Pero las sesiones permanentes pueden afectar a la escalabilidad y complicar la actualización de las aplicaciones web. Una mejor opción consiste en usar las memorias caché distribuidas de Redis o SQL Server, que no requieren sesiones permanentes. Para más información, vea [Working with a Distributed Cache](xref:performance/caching/distributed) (Trabajar con una memoria caché distribuida). Para obtener más información sobre cómo configurar los proveedores de servicios, consulte [Configuración de sesión](#configuring-session) más adelante en este artículo.

## <a name="tempdata"></a>TempData

ASP.NET Core MVC expone la propiedad [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) en un [controlador](/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0). Esta propiedad almacena datos hasta que se leen. Los métodos `Keep` y `Peek` se pueden usar para examinar los datos sin que se eliminen. `TempData` es particularmente útil para el redireccionamiento cuando se necesitan los datos de más de una única solicitud. Los proveedores de TempData implementan `TempData` mediante, por ejemplo, cookies o estado de sesión.

### <a name="tempdata-providers"></a>Proveedores de TempData

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

En ASP.NET Core 2.0 y versiones posteriores, el proveedor TempData basado en cookies se utiliza de forma predeterminada para almacenar TempData en cookies.

Los datos de cookie se cifran mediante [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), codificado con [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), y después se fragmentan. Como la cookie está fragmentada, no se aplica el límite de tamaño único de cookie que se encuentra en ASP.NET Core 1.x. Los datos de cookie no se comprimen porque la compresión de datos cifrados puede provocar problemas de seguridad como los ataques [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) y [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)). Para obtener más información sobre el proveedor TempData basado en cookies, consulte [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

En ASP.NET Core 1.0 y 1.1, el proveedor TempData de estado de sesión es el valor predeterminado.

---

### <a name="choosing-a-tempdata-provider"></a>Elegir un proveedor TempData

Elegir un proveedor TempData implica una serie de consideraciones:

1. ¿La aplicación ya usa el estado de sesión para otros fines? Si es así, la utilización del proveedor TempData de estado de sesión no tiene costo adicional para la aplicación (excepto en el tamaño de los datos).
2. ¿La aplicación usa TempData con moderación, solo para cantidades relativamente pequeñas de datos (hasta 500 bytes)? Si es así, el proveedor TempData de cookies agregará un pequeño costo a cada solicitud que transporta TempData. De lo contrario, el proveedor TempData de estado de sesión puede ser beneficioso para evitar que una gran cantidad de datos hagan un recorrido de ida y vuelta en cada solicitud hasta que se consuma TempData.
3. ¿La aplicación se ejecuta en una granja de servidores web (varios servidores)? En caso afirmativo, no es necesario realizar ninguna otra configuración para usar el proveedor TempData de cookies.

> [!NOTE]
> La mayoría de los clientes de web (por ejemplo, los exploradores web) aplican límites en el tamaño máximo de cada cookie, el número total de cookies o ambos. Por lo tanto, cuando use el proveedor TempData de cookies, compruebe que la aplicación no supera esos límites. Valore el tamaño total de los datos, teniendo en cuenta las sobrecargas de cifrado y fragmentación.

### <a name="configure-the-tempdata-provider"></a>Configurar el proveedor TempData

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

El proveedor TempData basado en cookies está habilitado de forma predeterminada. El siguiente código de clase `Startup` configura el proveedor TempData basado en sesión:

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,6,11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

El siguiente código de clase `Startup` configura el proveedor TempData basado en sesión:

[!code-csharp[](app-state/sample/src/WebAppSession/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,9)]

---

El orden es fundamental para los componentes de middleware. En el ejemplo anterior, se produce una excepción de tipo `InvalidOperationException` cuando `UseSession` se invoca después de `UseMvcWithDefaultRoute`. Vea [Ordenación de middleware](xref:fundamentals/middleware/index#ordering) para obtener más detalles.

> [!IMPORTANT]
> Si el destino es .NET Framework y usa el proveedor basado en sesión, agregue el paquete NuGet [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) a su proyecto.

## <a name="query-strings"></a>Cadenas de consulta

Puede pasar una cantidad limitada de datos de una solicitud a otra si agrega los datos a la cadena de consulta de la solicitud nueva. Esto es útil para capturar el estado de una forma persistente que permita que los vínculos con estado insertado se compartan a través del correo electrónico o las redes sociales. Pero, por esta razón, nunca deben usarse las cadenas de consulta para datos confidenciales. Además de que se pueden compartir con facilidad, la inclusión de los datos en cadenas de consulta puede propiciar ataques de [falsificación de solicitud entre sitios (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)), cuya intención es engañar a los usuarios para que visiten sitios malintencionados mientras están autenticados. A continuación, los atacantes pueden robar los datos de usuario de la aplicación o realizar acciones malintencionadas en nombre del usuario. Cualquier estado de sesión o aplicación conservado debe protegerse contra los ataques CSRF. Para más información sobre los ataques CSRF, vea [Prevent Cross-Site Request Forgery (XSRF/CSRF) Attacks](xref:security/anti-request-forgery) (Evitar los ataques de falsificación de solicitud entre sitios [XSRF/CSRF]).

## <a name="post-data-and-hidden-fields"></a>Exposición de datos y campos ocultos

Los datos pueden guardarse en campos ocultos de formulario e incluirse de nuevo en la siguiente solicitud. Esto es habitual en los formularios de varias páginas. Pero, dado que el cliente puede llegar a alterar los datos, el servidor debe siempre revalidarlos.

## <a name="cookies"></a>Cookies

Las cookies proporcionan una manera de almacenar datos específicos del usuario en aplicaciones web. Dado que las cookies se envían con cada solicitud, su tamaño debe reducirse al mínimo. Lo ideal es que en cada cookie se almacene un solo identificador y que los datos reales se almacenen en el servidor. La mayoría de los exploradores restringen el tamaño de las cookies a 4096 bytes. Además, solo hay disponible un número limitado de cookies para cada dominio.

Como las cookies están expuestas a alteraciones, deben validarse en el servidor. Aunque la duración de la cookie en un cliente está sujeta a un plazo de expiración y a la intervención del usuario, generalmente son la forma más duradera de persistencia de datos en el cliente.

Las cookies suelen utilizarse para personalizar el contenido ofrecido a un usuario conocido. Puesto que en la mayoría de los casos el usuario tan solo se identifica y no se autentica, normalmente se puede proteger una cookie si se almacena el nombre de usuario, el nombre de cuenta o un identificador de usuario único (por ejemplo, un GUID) en la cookie. A continuación, se puede usar la cookie para tener acceso a la infraestructura de personalización de usuarios de un sitio.

## <a name="httpcontextitems"></a>HttpContext.Items

La colección `Items` es un buen lugar para almacenar los datos que solo son necesarios para el procesamiento de una solicitud determinada. El contenido de la colección se descarta después de cada solicitud. Se aconseja utilizar la colección `Items` como forma de comunicación entre los componentes o el middleware cuando operan en distintos puntos en el tiempo durante una solicitud y no pueden pasarse parámetros de forma directa. Para más información, vea [Trabajar con HttpContext.Items](#working-with-httpcontextitems), más adelante en este artículo.

## <a name="cache"></a>instancias y claves

El almacenamiento en caché es una manera eficaz de almacenar y recuperar datos. Puede controlar la duración de los elementos almacenados en caché en función del tiempo y otras consideraciones. Obtenga más información sobre [cómo almacenar en caché](../performance/caching/index.md).

## <a name="working-with-session-state"></a>Trabajar con el estado de sesión

### <a name="configuring-session"></a>Configuración de sesión

El paquete `Microsoft.AspNetCore.Session` proporciona middleware para administrar el estado de sesión. Para habilitar el middleware de sesión, `Startup` debe contener:

- Cualquiera de las cachés de memoria [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache). La implementación de `IDistributedCache` se usa como una memoria auxiliar para la sesión.
- Una llamada a [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_), para lo cual se requiere el paquete NuGet "Microsoft.AspNetCore.Session".
- Una llamada a [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_).

El código siguiente muestra cómo configurar el proveedor de sesión en memoria.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

Puede hacer referencia a la sesión de `HttpContext` una vez que se ha instalado y configurado.

Si se intenta acceder a `Session` antes de haber llamado a `UseSession`, se produce la excepción `InvalidOperationException: Session has not been configured for this application or request`.

Si intenta crear una nueva `Session` (es decir, no se ha creado ninguna cookie de sesión) después de que ya haya empezado a escribir en el flujo `Response`, se produce la excepción `InvalidOperationException: The session cannot be established after the response has started`. La excepción puede encontrarse en el registro de servidor web; no se mostrará en el explorador.

### <a name="loading-session-asynchronously"></a>Carga de sesión de forma asincrónica

El proveedor de sesión predeterminado de ASP.NET Core carga asincrónicamente el registro de sesión desde el almacén subyacente [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) solo si el método [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) se llama explícitamente antes que los métodos `TryGetValue`, `Set` o `Remove`. Si primero no se llama a `LoadAsync`, el registro de sesión subyacente se carga de forma sincrónica, lo que podría tener consecuencias sobre la capacidad de la aplicación para escalarse.

Para que las aplicaciones impongan este patrón, ajuste las implementaciones de [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) y [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) con versiones que produzcan una excepción si el método `LoadAsync` no se llama antes que `TryGetValue`, `Set` o `Remove`. Registre las versiones ajustadas en el contenedor de servicios.

### <a name="implementation-details"></a>Detalles de la implementación

La sesión utiliza una cookie para realizar el seguimiento de las solicitudes emitidas por un solo explorador e identificarlas. De manera predeterminada, esta cookie se denomina ".AspNet.Session" y utiliza una ruta de acceso de "/". Dado que el valor predeterminado de la cookie no especifica un dominio, no estará disponible para el script de cliente en la página (porque `CookieHttpOnly` tiene como valor predeterminado `true`).

Para reemplazar los valores predeterminados de la sesión, use `SessionOptions`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

El servidor usa la propiedad `IdleTimeout` para determinar el tiempo que una sesión puede estar inactiva antes de que se abandone su contenido. Esta propiedad es independiente de la expiración de la cookie. Cada solicitud que se pasa a través del middleware de sesión (leídas o escritas) restablece el tiempo de espera.

Dado que `Session` *no realiza bloqueo*, si dos solicitudes intentan modificar el contenido de la sesión, la última de ellas reemplaza a la primera. `Session` se implementa como una *sesión coherente*, lo que significa que todo el contenido se almacena junto. Dos solicitudes que estén modificando distintas partes de la sesión (claves diferentes) aún podrían verse afectadas entre sí.

### <a name="set-and-get-session-values"></a>Establecer y obtener valores de Session

El acceso a Session se realiza desde una vista o página de Razor con `Context.Session`:

[!code-cshtml[](app-state/sample/src/WebAppSessionDotNetCore2.0App/Views/Home/About.cshtml)]

El acceso a Session se realiza desde un controlador o clase `PageModel` con `HttpContext.Session`: Esta propiedad es una implementación de [ISession](/dotnet/api/microsoft.aspnetcore.http.isession).

En el ejemplo siguiente se muestra cómo establecer y obtener un valor int y una cadena:

[!code-csharp[](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

Si agrega los siguientes métodos de extensión, puede establecer y obtener objetos serializables en sesión:

[!code-csharp[](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

En el ejemplo siguiente se muestra cómo establecer y obtener un objeto serializable:

[!code-csharp[](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]

## <a name="working-with-httpcontextitems"></a>Trabajar con HttpContext.Items

La abstracción `HttpContext` proporciona compatibilidad para una colección de diccionarios de tipo `IDictionary<object, object>`, llamada `Items`. Esta colección está disponible desde el inicio de una solicitud *HttpRequest* y se descarta al final de cada solicitud. Para acceder a ella, se puede asignar un valor a una entrada con clave, o solicitar el valor de una clave determinada.

En el ejemplo siguiente, [Middleware](xref:fundamentals/middleware/index) agrega `isVerified` a la colección `Items`.

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

Más adelante en la canalización, otro middleware podría acceder a ella:

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " +
        context.Items["isVerified"]);
});
```

Si el middleware se va a usar en una única aplicación, se aceptan claves `string`. Pero el middleware que se compartirá entre aplicaciones debe usar claves de objeto únicas para evitar cualquier posibilidad de colisión entre claves. Si está desarrollando middleware que debe trabajar en varias aplicaciones, utilice una clave de objeto única definida en la clase de middleware, tal y como se muestra a continuación:

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

Este enfoque también tiene la ventaja de eliminar la repetición de "cadenas mágicas" en varios lugares en el código.

## <a name="application-state-data"></a>Datos de estado de aplicación

Use [inserción de dependencias](xref:fundamentals/dependency-injection) para que los datos estén disponibles para todos los usuarios:

1. Defina un servicio que contenga los datos (por ejemplo, una clase denominada `MyAppData`).

    ```csharp
    public class MyAppData
    {
        // Declare properties/methods/etc.
    } 
    ```

2. Agregue la clase de servicio a `ConfigureServices` (por ejemplo `services.AddSingleton<MyAppData>();`).

3. Utilice la clase de servicio de datos en cada controlador:

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

## <a name="common-errors-when-working-with-session"></a>Errores comunes al trabajar con sesión

* "No se puede resolver el servicio para el tipo 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' al intentar activar 'Microsoft.AspNetCore.Session.DistributedSessionStore'".

  Esto puede deberse a que no se ha configurado al menos una implementación `IDistributedCache`. Para más información, vea [Working with a Distributed Cache](xref:performance/caching/distributed) (Trabajar con una memoria caché distribuida) e [In memory caching](xref:performance/caching/memory) (Almacenamiento en memoria caché).

* En caso de que el middleware de sesión no logre conservar una sesión (por ejemplo, si la base de datos no está disponible), registra la excepción y la acepta. La solicitud continuará entonces de modo normal, lo que provoca un comportamiento muy imprevisible.

Este es un ejemplo típico:

Alguien almacena una cesta de la compra en la sesión. El usuario agrega un elemento, pero se produce un error en la confirmación. La aplicación no se percata del error y notifica que "el elemento se ha agregado", lo cual no es cierto.

La manera recomendada para comprobar estos errores es llamar a `await feature.Session.CommitAsync();` desde el código de la aplicación cuando se haya terminado de escribir en la sesión. A continuación, puede hacer lo que quiera con el error. El funcionamiento es el mismo que al llamar a `LoadAsync`.

### <a name="additional-resources"></a>Recursos adicionales

* [ASP.NET Core 1.x: ejemplo de código que se emplea en este documento](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [ASP.NET Core 2.x: ejemplo de código que se emplea en este documento](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
