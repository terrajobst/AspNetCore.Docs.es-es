---
title: "Dirección URL de reescritura de Middleware en ASP.NET Core"
author: guardrex
description: "Obtenga información acerca de la dirección URL de reescritura y la redirección con Middleware de reescritura de dirección URL en las aplicaciones de ASP.NET Core."
keywords: "Núcleo de ASP.NET, volver a escribir, dirección URL de reescritura de direcciones URL, dirección URL de redirección, redirección de la dirección URL, middleware, apache_mod"
ms.author: riande
manager: wpickett
ms.date: 08/17/2017
ms.topic: article
ms.assetid: e6130638-c410-4161-9921-b658ce988bd1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/url-rewriting
ms.openlocfilehash: 0a4024edf13651e2ed7e0f87e554e8ba8d895619
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2017
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a>Dirección URL de reescritura de Middleware en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex) y [Mikael Mengistu](https://github.com/mikaelm12)

[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample))

Reescritura de direcciones URL es el acto de modificar la solicitud las direcciones URL que se basan en una o varias reglas predefinidas. Reescritura de direcciones URL, crea una abstracción entre ubicaciones de los recursos y sus direcciones para que las ubicaciones y direcciones no están estrechamente vinculadas. Hay varios escenarios donde la reescritura de direcciones URL es útil:
* Mover o reemplazar los recursos de servidor temporal o permanentemente mientras se mantiene estables localizadores para esos recursos
* División entre las diferentes aplicaciones o entre áreas de una aplicación de procesamiento de solicitudes
* Quitar, agregar o reorganizar los segmentos de dirección URL en las solicitudes entrantes
* Optimizar direcciones URL públicas para la optimización de motor de búsqueda (SEO)
* Permitir el uso de URL públicas preparadas para ayudar a los usuarios a predecir el contenido que se encuentran disponibles mediante el vínculo
* Redirigir solicitudes no seguras para proteger los puntos de conexión
* Evitar hotlinking de imagen

Puede definir reglas para cambiar la dirección URL de varias maneras, incluido regex, reglas de módulo de Apache mod_rewrite, las reglas del módulo de Rewrite para IIS y usar lógica de la regla personalizada. Este documento presenta la reescritura de direcciones URL con instrucciones sobre cómo usar el Middleware de reescritura de dirección URL en las aplicaciones de ASP.NET Core.

> [!NOTE]
> Reescritura de direcciones URL, puede reducir el rendimiento de una aplicación. Cuando sea factible, debe limitar el número y la complejidad de las reglas.

## <a name="url-redirect-and-url-rewrite"></a>Vuelva a escribir el redireccionamiento de dirección URL y la dirección URL
La diferencia en texto entre *redirección de la dirección URL* y *URL rewrite* puede parecer sutiles en primer pero tiene importantes implicaciones para proporcionar recursos a los clientes. Middleware de volver a escribir de dirección URL de ASP.NET Core es capaz de satisfacer la necesidad de ambos.

A *redirección de la dirección URL* es una operación de cliente, donde se indica el cliente para tener acceso a un recurso en otra dirección. Esto requiere una ida y vuelta al servidor, y la dirección URL de redireccionamiento devuelve al cliente aparece en la barra de direcciones del explorador cuando el cliente realiza una solicitud nueva para el recurso. Si `/resource` es *redirigido* a `/different-resource`, las solicitudes del cliente `/resource`, y el servidor responde que el cliente debe obtener el recurso en `/different-resource` con un código de estado que indica que la redirección es puede ser temporal o permanente. El cliente ejecuta una nueva solicitud para el recurso en la dirección URL de redireccionamiento.

![Un punto de conexión de servicio de WebAPI se ha cambiado temporalmente de la versión 1 (v1) a la versión 2 (v2) en el servidor. Un cliente realiza una solicitud al servicio en la /v1/api de ruta de acceso de versión 1. El servidor devuelve una respuesta 302 de (encontrado) con la ruta de acceso nuevo, temporal para el servicio en /v2/api versión 2. El cliente realiza una segunda solicitud al servicio en la dirección URL de redireccionamiento. El servidor responde con un código de estado 200 (OK).](url-rewriting/_static/url_redirect.png)

Al redirigir las solicitudes a una dirección URL diferente, se indica si la redirección se permanentes o temporales. El código de estado (movida permanentemente) 301 sirve para indicar al cliente que todas las solicitudes futuras para el recurso deben usar la nueva dirección URL donde el recurso tiene una dirección URL nueva, permanente y desea. *El cliente puede almacenar en caché la respuesta cuando se recibe un código de 301 estado.* Se utiliza el código de estado 302 (encontrado) donde la redirección es temporal o generalmente sujeto para cambiar, por ejemplo, que el cliente no debe almacenar y reutilizar la dirección URL de redireccionamiento en el futuro. Para obtener más información, consulte [RFC 2616: definiciones de código de estado](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

A *URL rewrite* es una operación de servidor para proporcionar un recurso desde una dirección de recursos distinto. Volver a escribir una dirección URL no requiere una ida y vuelta al servidor. La dirección URL reescrita no se devuelve al cliente y no aparecerá en la barra de direcciones del explorador. Cuando `/resource` es *reescrito* a `/different-resource`, las solicitudes del cliente `/resource`y el servidor de *internamente* captura el recurso en `/different-resource`. Aunque el cliente podría ser capaz de recuperar el recurso en la dirección URL ha vuelto a escribir, el cliente no informará de que el recurso existe en la dirección URL reescrita cuando realiza su solicitud y recibe la respuesta.

![Se cambió un extremo de servicio de WebAPI de la versión 1 (v1) a la versión 2 (v2) en el servidor. Un cliente realiza una solicitud al servicio en la /v1/api de ruta de acceso de versión 1. La dirección URL de la solicitud se ha reescrito para tener acceso al servicio en la /v2/api de ruta de acceso de versión 2. El servicio responde al cliente con un código de estado 200 (OK).](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a>Aplicación de ejemplo de reescritura de dirección URL
Puede explorar las características de Middleware de reescritura de dirección URL con el [aplicación de ejemplo de reescritura de dirección URL](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/). La aplicación aplica reescritura y reglas de redirección y muestra la dirección URL redirigida o ha vuelto a escribir.

## <a name="when-to-use-url-rewriting-middleware"></a>Cuándo usar el Middleware de reescritura de dirección URL
Usar el Middleware de reescritura de dirección URL cuando no se puede usar el [módulo URL Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite) con IIS en Windows Server, el [módulo de Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/) en el servidor Apache, [reescritura de direcciones URL en Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), o la aplicación se hospeda en [servidor HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente denominados [WebListener](xref:fundamentals/servers/weblistener)). Las principales razones para usar el servidor URL reescritura tecnologías basadas en IIS, Apache o Nginx son que el middleware no es compatible con todas las características de estos módulos y el rendimiento del middleware de probablemente no coincide con el de los módulos. Sin embargo, hay algunas características de los módulos de servidor que no funcionan con proyectos de ASP.NET Core, como el `IsFile` y `IsDirectory` restricciones del módulo Rewrite para IIS. En estos casos, utilice en su lugar el middleware.

## <a name="package"></a>Package
Para incluir el software intermedio en el proyecto, agregue una referencia a la [ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) paquete. Esta característica está disponible para las aplicaciones que tienen como destino ASP.NET Core 1.1 o posterior.

## <a name="extension-and-options"></a>Extensión y opciones
Establecer la reescritura de dirección URL y redirigir las reglas mediante la creación de una instancia de la `RewriteOptions` clase con métodos de extensión para cada una de las reglas. Encadenar varias reglas en el orden que desea procesar. El `RewriteOptions` se pasan en el Middleware de reescritura de dirección URL cuando se agrega a la canalización de solicitud con `app.UseRewriter(options);`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1)]

---

### <a name="url-redirect"></a>Redirección de la dirección URL
Use `AddRedirect` para redirigir las solicitudes. El primer parámetro contiene la expresión regular de coincidencia en la ruta de acceso de la dirección URL entrante. El segundo parámetro es la cadena de reemplazo. El tercer parámetro, si está presente, especifica el código de estado. Si no se especifica el código de estado, el valor predeterminado es a 302 (encontrado), lo que indica que el recurso está temporalmente movido o reemplazado.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=2)]

---

En un explorador con herramientas de desarrollo habilitado, realizar una solicitud a la aplicación de ejemplo con la ruta de acceso `/redirect-rule/1234/5678`. La expresión regular coincide con la ruta de acceso de solicitud de `redirect-rule/(.*)`, y la ruta de acceso se reemplaza con `/redirected/1234/5678`. La redirección de la dirección URL se envía al cliente con un código de estado 302 (encontrado). El explorador realiza una solicitud nueva en la dirección URL de redireccionamiento, que aparece en la barra de direcciones del explorador. Puesto que coincide con ninguna regla en la aplicación de ejemplo en la dirección URL de redireccionamiento, la segunda solicitud recibe una respuesta de 200 (OK) de la aplicación y el cuerpo de la respuesta muestra la dirección URL de redireccionamiento. Se realiza un ida y vuelta al servidor cuando una dirección URL es *redirigido*.

> [!WARNING]
> Tenga cuidado al establecer las reglas de redirección. Las reglas de redirección se evalúan en cada solicitud a la aplicación, incluso después de una redirección. Es fácil accidentalmente creará un bucle de redirecciones infinitos.

Solicitud original:`/redirect-rule/1234/5678`

![Ventana del explorador con herramientas de desarrollo de seguimiento de las solicitudes y respuestas](url-rewriting/_static/add_redirect.png)

La parte de la expresión contenida entre paréntesis se denomina un *grupo de capturas*. El punto (`.`) de la expresión significa *coincide con cualquier carácter*. El asterisco (`*`) indica *coincide con el carácter anterior cero o más veces*. Por lo tanto, los dos últimos segmentos de la dirección URL, `1234/5678`, se capturan por grupo de captura `(.*)`. Cualquier valor que se proporciona en la dirección URL de solicitud después `redirect-rule/` capturada por este grupo de captura único.

En la cadena de reemplazo, se insertan los grupos capturados en la cadena con el signo de dólar (`$`) seguido del número de secuencia de la captura. Se obtiene el valor del primer grupo de captura con `$1`, el segundo con `$2`, que siguen en secuencia para los grupos de captura en la expresión regular. Hay solo un grupo capturado en la expresión regular regla de redirección de la aplicación de ejemplo, por lo que hay solo un grupo insertado en la cadena de reemplazo, que es `$1`. Cuando se aplica la regla, se convierte en la dirección URL `/redirected/1234/5678`.

<a name=url-redirect-to-secure-endpoint></a>
### <a name="url-redirect-to-a-secure-endpoint"></a>Redireccionamiento de la dirección URL para un punto de conexión segura
Use `AddRedirectToHttps` para redirigir las solicitudes HTTP en el mismo host y la ruta de acceso con HTTPS (`https://`). Si no se proporciona el código de estado, el middleware predeterminado 302 (encontrado). Si no se especifica el puerto, el middleware tiene como valor predeterminado `null`, lo que significa que el protocolo cambia a `https://` y el cliente tiene acceso al recurso en el puerto 443. En el ejemplo se muestra cómo establecer el código de estado para 301 (movida permanentemente) y cambie el puerto a 5001.
```csharp
var options = new RewriteOptions()
    .AddRedirectToHttps(301, 5001);

app.UseRewriter(options);
```
Use `AddRedirectToHttpsPermanent` para redirigir las solicitudes poco en el mismo host y la ruta de acceso mediante el protocolo HTTPS seguro (`https://` en el puerto 443). El middleware establece el código de estado en 301 (movida permanentemente).

La aplicación de ejemplo es capaz de mostrar cómo usar `AddRedirectToHttps` o `AddRedirectToHttpsPermanent`. Agregue el método de extensión a la `RewriteOptions`. Realizar una solicitud poco a la aplicación en cualquier dirección URL. Descartar la advertencia de que el certificado autofirmado no es de confianza de seguridad de explorador.

Solicitud original mediante `AddRedirectToHttps(301, 5001)`:`/secure`

![Ventana del explorador con herramientas de desarrollo de seguimiento de las solicitudes y respuestas](url-rewriting/_static/add_redirect_to_https.png)

Solicitud original mediante `AddRedirectToHttpsPermanent`:`/secure`

![Ventana del explorador con herramientas de desarrollo de seguimiento de las solicitudes y respuestas](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a>Reescritura de dirección URL
Use `AddRewrite` para crear un reglas para volver a escribir las direcciones URL. El primer parámetro contiene la expresión regular de coincidencia en la ruta de acceso de dirección URL entrante. El segundo parámetro es la cadena de reemplazo. El tercer parámetro, `skipRemainingRules: {true|false}`, indica el middleware de reglas de reescritura adicionales deben omitirse si se aplica la regla actual o no.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=6)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=3)]

---

Solicitud original:`/rewrite-rule/1234/5678`

![Ventana del explorador con herramientas de desarrollo de seguimiento de la solicitud y respuesta](url-rewriting/_static/add_rewrite.png)

Lo primero que observa en la expresión regular es el acento circunflejo (`^`) al principio de la expresión. Esto significa que búsqueda de coincidencias comienza al principio de la ruta de acceso de dirección URL.

En el ejemplo anterior con la regla de redirección, `redirect-rule/(.*)`, no hay ningún acento circunflejo al principio de la expresión regular; por lo tanto, puede preceder cualquier carácter `redirect-rule/` en la ruta de acceso para una coincidencia correcta.

| Ruta de acceso                               | Coincidir con |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | Sí   |
| `/my-cool-redirect-rule/1234/5678` | Sí   |
| `/anotherredirect-rule/1234/5678`  | Sí   |

La regla de reescritura, `^rewrite-rule/(\d+)/(\d+)`, solo coincide con las rutas de acceso si se inician con `rewrite-rule/`. Observe la diferencia en la coincidencia entre la siguiente regla de reescritura y la regla de redirección anteriormente.

| Ruta de acceso                              | Coincidir con |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | Sí   |
| `/my-cool-rewrite-rule/1234/5678` | No    |
| `/anotherrewrite-rule/1234/5678`  | No    |

Después de la `^rewrite-rule/` parte de la expresión, hay dos grupos de captura, `(\d+)/(\d+)`. El `\d` significa *coincide con un dígito (número)*. El signo más (`+`) significa *coincide con uno o varios de los caracteres anteriores*. Por lo tanto, la dirección URL debe contener un número seguido de una diagonal seguida por otro número. Captura de estos grupos se insertan en la dirección URL ha vuelto a escribir como `$1` y `$2`. La cadena de reemplazo de la regla de reescritura coloca los grupos capturados en la cadena de consulta. La ruta de acceso solicitada de `/rewrite-rule/1234/5678` se ha reescrito para obtener el recurso en `/rewritten?var1=1234&var2=5678`. Si una cadena de consulta está presente en la solicitud original, se conservan cuando se vuelve a escribir la dirección URL.

No hay ningún ida y vuelta al servidor para obtener el recurso. Si el recurso existe, se capturan y se devuelve al cliente con un código de 200 estado (Aceptar). Dado que el cliente no está redirigido, no cambia la dirección URL en la barra de direcciones del explorador. En lo que el cliente se refiere, la operación de reescritura de dirección URL nunca se ha producido.

> [!NOTE]
> Use `skipRemainingRules: true` siempre que sea posible, ya que las reglas de coincidencia es un proceso costoso y reduce el tiempo de respuesta de aplicación. Para la respuesta más rápida de la aplicación:
> * Ordene las reglas de reescritura de la regla coincidente con más frecuencia a la regla coincidente con menor frecuencia.
> * Omitir el procesamiento de las reglas restantes cuando se produce una coincidencia y no es necesario ningún procesamiento de reglas adicionales.

### <a name="apache-modrewrite"></a>Apache mod_rewrite
Aplicar reglas de Apache mod_rewrite con `AddApacheModRewrite`. Asegúrese de que el archivo de reglas se implementa con la aplicación. Para obtener más información y ejemplos de reglas de mod_rewrite, consulte [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

A `StreamReader` se utiliza para leer las reglas de la *ApacheModRewrite.txt* archivo de reglas.

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=1,7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

El primer parámetro tiene un `IFileProvider`, que se proporciona a través de [inyección de dependencia](dependency-injection.md). El `IHostingEnvironment` se inyecta para proporcionar la `ContentRootFileProvider`. El segundo parámetro es la ruta de acceso al archivo de reglas, que es *ApacheModRewrite.txt* en la aplicación de ejemplo.

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=4)]

---

La aplicación de ejemplo redirige las solicitudes de `/apache-mod-rules-redirect/(.\*)` a `/redirected?id=$1`. El código de estado de respuesta es 302 (encontrado).

[!code[Main](url-rewriting/samples/2.x/ApacheModRewrite.txt)]

Solicitud original:`/apache-mod-rules-redirect/1234`

![Ventana del explorador con herramientas de desarrollo de seguimiento de las solicitudes y respuestas](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a>Variables de servidor compatibles
El middleware admite las siguientes variables de servidor de Apache mod_rewrite:
* CONN_REMOTE_ADDR
* HTTP_ACCEPT
* HTTP_CONNECTION
* HTTP_COOKIE
* HTTP_FORWARDED
* HTTP_HOST
* HTTP_REFERER
* HTTP_USER_AGENT
* HTTPS
* IPV6
* QUERY_STRING
* REMOTE_ADDR
* REMOTE_PORT
* REQUEST_FILENAME
* REQUEST_METHOD
* REQUEST_SCHEME
* REQUEST_URI
* SCRIPT_FILENAME
* SERVER_ADDR
* SERVER_PORT
* SERVER_PROTOCOL
* TIEMPO
* TIME_DAY
* TIME_HOUR
* TIME_MIN
* TIME_MON
* TIME_SEC
* TIME_WDAY
* TIME_YEAR

### <a name="iis-url-rewrite-module-rules"></a>Reglas del módulo URL Rewrite de IIS
Para utilizar reglas que se aplican en el módulo URL Rewrite de IIS, utilice `AddIISUrlRewrite`. Asegúrese de que el archivo de reglas se implementa con la aplicación. No dirigir el middleware para usar su *web.config* archivo cuando se ejecuta en Windows Server IIS. Con IIS, estas reglas deben estar almacenadas fuera de su *web.config* para evitar conflictos con el módulo Rewrite para IIS. Para obtener más información y ejemplos de reglas de módulo URL Rewrite de IIS, consulte [utilizando Url Rewriter 2.0 módulo](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) y [referencia de configuración del módulo de volver a escribir dirección URL](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

A `StreamReader` se utiliza para leer las reglas de la *IISUrlRewrite.xml* archivo de reglas.

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=2,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

El primer parámetro tiene un `IFileProvider`, mientras que el segundo parámetro es la ruta de acceso al archivo de reglas XML, que es *IISUrlRewrite.xml* en la aplicación de ejemplo.

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=5)]

---

La aplicación de ejemplo vuelve a escribir las solicitudes de `/iis-rules-rewrite/(.*)` a `/rewritten?id=$1`. La respuesta se envía al cliente con un código de estado 200 (OK).

[!code-xml[Main](url-rewriting/samples/2.x/IISUrlRewrite.xml)]

Solicitud original:`/iis-rules-rewrite/1234`

![Ventana del explorador con herramientas de desarrollo de seguimiento de la solicitud y respuesta](url-rewriting/_static/add_iis_url_rewrite.png)

Si tiene un módulo de reescritura de IIS activo con reglas de nivel de servidor configuradas que pudieran afectar a la aplicación de maneras no deseados, puede deshabilitar el módulo de reescritura de IIS para una aplicación. Para obtener más información, consulte [módulos IIS deshabilitar](xref:hosting/iis-modules#disabling-iis-modules).

#### <a name="unsupported-features"></a>Características no admitidas

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

El middleware publicado con ASP.NET Core 2.x no es compatible con las siguientes características de módulo URL Rewrite de IIS:
* Reglas de salida
* Variables de servidor personalizado
* Caracteres comodín
* LogRewrittenUrl

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

El middleware publicado con ASP.NET Core 1.x no es compatible con las siguientes características de módulo URL Rewrite de IIS:
* Reglas globales
* Reglas de salida
* Mapas de reescritura
* Acción CustomResponse
* Variables de servidor personalizado
* Caracteres comodín
* Acción: CustomResponse
* LogRewrittenUrl

---

#### <a name="supported-server-variables"></a>Variables de servidor compatibles
El middleware admite las siguientes variables de servidor de módulo URL Rewrite de IIS:
* CONTENT_LENGTH
* CONTENT_TYPE
* HTTP_ACCEPT
* HTTP_CONNECTION
* HTTP_COOKIE
* HTTP_HOST
* HTTP_REFERER
* HTTP_URL
* HTTP_USER_AGENT
* HTTPS
* LOCAL_ADDR
* QUERY_STRING
* REMOTE_ADDR
* REMOTE_PORT
* REQUEST_FILENAME
* REQUEST_URI

> [!NOTE]
> También puede obtener un `IFileProvider` a través de un `PhysicalFileProvider`. Este enfoque puede proporcionar una mayor flexibilidad para la ubicación de la reescritura de archivos de reglas. Asegúrese de que los archivos de reglas de reescritura se implementan en el servidor en la ruta de acceso que proporcione.
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a>Regla de recopilación (método)
Use `Add(Action<RewriteContext> applyRule)` para implementar su propia lógica de la regla en un método. El `RewriteContext` expone el `HttpContext` para su uso en el método. El `context.Result` determina cómo adicional canalización controla el procesamiento.

| contexto. Resultado                       | Acción                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| `RuleResult.ContinueRules` (valor predeterminado) | Continúe aplicando reglas                                         |
| `RuleResult.EndResponse`             | Dejar de aplicar las reglas y enviar la respuesta                       |
| `RuleResult.SkipRemainingRules`      | Dejar de aplicar las reglas y enviar el contexto al siguiente middleware |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=6)]

---

La aplicación de ejemplo muestra un método que redirige las solicitudes para las rutas de acceso que terminen con *.xml*. Si se realiza una solicitud `/file.xml`, se redirige a `/xmlfiles/file.xml`. El código de estado se establece en 301 (movida permanentemente). Para una redirección, debe establecer explícitamente el código de estado de la respuesta; en caso contrario, se devuelve un código de estado 200 (OK) y no se produzca la redirección en el cliente.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet2)]

---

Solicitud original:`/file.xml`

![Ventana del explorador con herramientas de desarrollo de seguimiento de las solicitudes y respuestas para file.xml](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a>Regla de recopilación IRule
Use `Add(IRule)` para implementar su propia lógica de la regla en una clase que deriva de `IRule`. Mediante una `IRule` proporciona mayor flexibilidad en comparación con el enfoque de reglas basadas en métodos. La clase derivada puede incluir un constructor, donde puede pasar parámetros para el `ApplyRule` método.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=7-8)]

---

Los valores de los parámetros en la aplicación de ejemplo para la `extension` y `newPath` se comprueban para cumplir varias condiciones. El `extension` debe contener un valor, y el valor debe ser *.png*, *.jpg*, o *.gif*. Si el `newPath` no es válido, un `ArgumentException` se produce. Si se realiza una solicitud *image.png*, se redirige a `/png-images/image.png`. Si se realiza una solicitud *image.jpg*, se redirige a `/jpg-images/image.jpg`. El código de estado se establece en 301 (movida permanentemente) y el `context.Result` se establece para detener el procesamiento de las reglas y enviar la respuesta.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/RewriteRule.cs?name=snippet1)]

---

Solicitud original:`/image.png`

![Ventana del explorador con herramientas de desarrollo de seguimiento de las solicitudes y respuestas para image.png](url-rewriting/_static/add_redirect_png_requests.png)

Solicitud original:`/image.jpg`

![Ventana del explorador con herramientas de desarrollo de seguimiento de las solicitudes y respuestas para image.jpg](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a>Ejemplos de expresión regular

| Objetivo | Cadena de expresión regular &<br>Ejemplo de coincidencia | Cadena de reemplazo &<br>Ejemplo de salida |
| ---- | :-----------------------------: | :------------------------------------: |
| Ruta de acceso de reescritura en la cadena de consulta | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| Quitar la barra diagonal final | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| Exigir la barra diagonal final | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| Evitar tener que escribir solicitudes específicas | `(.*[^(\.axd)])$`<br>Sí:`/resource.htm`<br>No:`/resource.axd` | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| Reorganizar los segmentos de dirección URL | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| Reemplazar un segmento de dirección URL | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a>Recursos adicionales
* [Inicio de aplicaciones](startup.md)
* [Middleware](middleware.md)
* [Expresiones regulares en .NET](/dotnet/articles/standard/base-types/regular-expressions)
* [Lenguaje de expresiones regulares: referencia rápida](/dotnet/articles/standard/base-types/quick-ref)
* [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)
* [Usar módulo Url Rewrite 2.0 (para IIS)](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [Referencia de configuración del módulo de reescritura de dirección URL](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [Foro de módulo de reescritura de dirección URL de IIS](https://forums.iis.net/1152.aspx)
* [Mantener una estructura de dirección URL simple](https://support.google.com/webmasters/answer/76329?hl=en)
* [Reescritura de direcciones URL en 10 sugerencias y trucos](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [Barra diagonal o no barra diagonal](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
