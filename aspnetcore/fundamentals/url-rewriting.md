---
title: Middleware de reescritura de URL en ASP.NET Core
author: guardrex
description: Aprenda a reescribir y redireccionar URL con el middleware de reescritura de URL en aplicaciones ASP.NET Core.
ms.author: riande
ms.date: 08/17/2017
uid: fundamentals/url-rewriting
ms.openlocfilehash: d3484e222c4412a427d086c1b71a12b81095ba72
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276352"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a>Middleware de reescritura de URL en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex) y [Mikael Mengistu](https://github.com/mikaelm12)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

La reescritura de URL consiste en modificar varias URL de solicitud basadas en una o varias reglas predefinidas. La reescritura de URL crea una abstracción entre las ubicaciones de recursos y sus direcciones para que las ubicaciones y direcciones no estén estrechamente vinculadas. Hay varias situaciones en las que la reescritura de URL es útil:

* Mover o reemplazar recursos del servidor de forma temporal o permanente a la vez que se mantienen estables los localizadores para esos recursos.
* Dividir el procesamiento de solicitudes entre distintas aplicaciones o entre áreas de una aplicación.
* Quitar, agregar o reorganizar segmentos de URL en solicitudes entrantes.
* Optimizar URL públicas para la optimización del motor de búsqueda (SEO).
* Permitir el uso de URL públicas preparadas para ayudar a los usuarios a predecir el contenido que encontrarán siguiendo un vínculo.
* Redirigir solicitudes no seguras para proteger puntos de conexión.
* Evitar la creación de vínculos activos de imagen.

Puede definir reglas para cambiar la URL de varias maneras, incluidas expresiones regulares, reglas del módulo mod_rewrite de Apache, reglas del módulo de reescritura de IIS y lógica de regla personalizada. En este documento se ofrece una introducción a la reescritura de URL e instrucciones sobre cómo usar el middleware de reescritura de URL en aplicaciones ASP.NET Core.

> [!NOTE]
> La reescritura de URL puede reducir el rendimiento de una aplicación. Cuando sea factible, debe limitar el número y la complejidad de las reglas.

## <a name="url-redirect-and-url-rewrite"></a>Redireccionamiento y reescritura de URL

La diferencia entre los términos *redirección de URL* y *reescritura de URL* en principio puede parecer sutil, pero tiene importantes implicaciones para proporcionar recursos a los clientes. El middleware de reescritura de URL de ASP.NET Core es capaz de satisfacer las necesidades de ambos.

La *redirección de URL* es una operación del lado cliente, que da la instrucción al cliente de acceder a un recurso en otra dirección. Esto requiere un recorrido de ida y vuelta al servidor. La URL de redireccionamiento que se devuelve al cliente aparece en la barra de direcciones del explorador cuando el cliente realiza una nueva solicitud para el recurso. 

Si `/resource` se *redirige* a `/different-resource`, el cliente solicita `/resource`. El servidor responde que el cliente debe obtener el recurso en `/different-resource` con un código de estado que indica que la redirección es temporal o permanente. El cliente ejecuta una nueva solicitud para el recurso en la URL de redireccionamiento.

![Se cambió temporalmente un punto de conexión del servicio WebAPI de la versión 1 (v1) a la versión 2 (v2) en el servidor. Un cliente realiza una solicitud al servicio en la ruta de acceso de la versión 1 /v1/api. El servidor devuelve una respuesta 302 (Encontrado) con la nueva ruta temporal para el servicio en la versión 2 /v2/api. El cliente realiza una segunda solicitud al servicio en la URL de redireccionamiento. El servidor responde con un código de estado 200 (Correcto).](url-rewriting/_static/url_redirect.png)

Al redirigir las solicitudes a una URL diferente, se indica si la redirección es permanente o temporal. El código de estado 301 (Movido definitivamente) se usa cuando el recurso tiene una nueva URL permanente y quiere indicar al cliente que todas las solicitudes futuras para el recurso deben usar la nueva URL. *El cliente puede almacenar en caché la respuesta cuando se recibe un código de estado 301.* El código de estado 302 (Encontrado) se usa cuando la redirección es temporal o normalmente está sujeta a cambios, de modo que el cliente no debe almacenar y reutilizar la URL de redireccionamiento en el futuro. Para más información, vea [RFC 2616: definiciones de código de estado](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

La *reescritura de URL* es una operación del lado servidor que proporciona un recurso desde una dirección de recursos distinta. La reescritura de una URL no requiere un recorrido de ida y vuelta al servidor. La dirección URL reescrita no se devuelve al cliente y no aparece en la barra de direcciones del explorador. Cuando `/resource` se *reescribe* como `/different-resource`, el cliente solicita `/resource` y el servidor recupera *internamente* el recurso en `/different-resource`. Aunque el cliente podría recuperar el recurso en la URL reescrita, el cliente no informará de que el recurso existe en la URL reescrita cuando realice su solicitud y reciba la respuesta.

![Se cambió un punto de conexión del servicio WebAPI de la versión 1 (v1) a la versión 2 (v2) en el servidor. Un cliente realiza una solicitud al servicio en la ruta de acceso de la versión 1 /v1/api. La URL de la solicitud se reescribe para acceder al servicio en la ruta de acceso de la versión 2 /v2/api. El servicio responde al cliente con un código de estado 200 (Correcto).](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a>Aplicación de ejemplo de reescritura de URL

Puede explorar las características del middleware de reescritura de URL con la [aplicación de ejemplo de reescritura de URL](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/). Esta aplicación aplica reglas de reescritura y redirección, además de mostrar la URL redirigida o reescrita.

## <a name="when-to-use-url-rewriting-middleware"></a>Cuándo usar el middleware de reescritura de URL

Use el middleware de reescritura de URL cuando no pueda usar el [módulo de reescritura de URL](https://www.iis.net/downloads/microsoft/url-rewrite) con IIS en Windows Server, el [módulo mod_rewrite de Apache](https://httpd.apache.org/docs/2.4/rewrite/) en el servidor Apache, la [reescritura de URL en Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/) o cuando la aplicación esté hospedada en el [servidor HTTP.sys](xref:fundamentals/servers/httpsys) (antes denominado [WebListener](xref:fundamentals/servers/weblistener)). Las principales razones para usar las tecnologías de reescritura de URL basadas en servidor en IIS, Apache o Nginx son que el middleware no es compatible con todas las características de estos módulos y el rendimiento del middleware probablemente no coincida con el de los módulos. Pero algunas características de los módulos de servidor no funcionan con proyectos de ASP.NET Core, como las restricciones `IsFile` y `IsDirectory` del módulo de reescritura de IIS. En estos casos, es mejor usar el middleware.

## <a name="package"></a>Package

Para incluir el middleware en el proyecto, agregue una referencia al paquete [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/). Esta característica está disponible para aplicaciones que tienen como destino ASP.NET Core 1.1 o posterior.

## <a name="extension-and-options"></a>Extensión y opciones

Establezca las reglas de reescritura y redirección de URL mediante la creación de una instancia de la clase `RewriteOptions` con métodos de extensión para cada una de las reglas. Encadene varias reglas en el orden que quiera procesarlas. Las `RewriteOptions` se pasan al middleware de reescritura de URL cuando se agregan a la canalización de solicitudes con `app.UseRewriter(options);`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1")
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true)
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")
        .Add(RedirectXMLRequests)
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

---

### <a name="url-redirect"></a>Redirección de URL

Use `AddRedirect` para redirigir las solicitudes. El primer parámetro contiene la expresión regular para hacer coincidir la ruta de acceso de la URL entrante. El segundo parámetro es la cadena de reemplazo. El tercer parámetro, si está presente, especifica el código de estado. Si no se especifica el código de estado, el valor predeterminado es 302 (Encontrado), lo que indica que el recurso se ha movido o reemplazado temporalmente.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1");

    app.UseRewriter(options);
}
```

---

En un explorador con herramientas de desarrollo habilitadas, realice una solicitud a la aplicación de ejemplo con la ruta de acceso `/redirect-rule/1234/5678`. La expresión regular coincide con la ruta de acceso de la solicitud en `redirect-rule/(.*)` y la ruta de acceso se reemplaza con `/redirected/1234/5678`. La URL de redireccionamiento se devuelve al cliente con un código de estado 302 (Encontrado). El explorador realiza una solicitud nueva en la URL de redireccionamiento, que aparece en la barra de direcciones del explorador. Puesto que no hay ninguna regla en la aplicación de ejemplo que coincida con la URL de redireccionamiento, la segunda solicitud recibe una respuesta 200 (Correcto) de la aplicación y el cuerpo de la respuesta muestra la URL de redireccionamiento. Se realiza un recorrido de ida y vuelta al servidor cuando se *redirige* una URL.

> [!WARNING]
> Tenga cuidado al establecer las reglas de redirección. Las reglas de redirección se evalúan en cada solicitud enviada a la aplicación, incluso después de una redirección. Es fácil crear por error un bucle de redirecciones infinitas.

Solicitud original: `/redirect-rule/1234/5678`

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas](url-rewriting/_static/add_redirect.png)

La parte de la expresión incluida entre paréntesis se denomina *grupo de capturas*. El punto (`.`) de la expresión significa *buscar cualquier carácter*. El asterisco (`*`) indica *buscar el carácter precedente cero o más veces*. Por tanto, el grupo de capturas `(.*)` busca los dos últimos segmentos de la ruta de acceso de la URL, `1234/5678`. Cualquier valor que se proporcione en la URL de la solicitud después de `redirect-rule/` es capturado por este grupo de capturas único.

En la cadena de reemplazo, se insertan los grupos capturados en la cadena con el signo de dólar (`$`) seguido del número de secuencia de la captura. Se obtiene el valor del primer grupo de capturas con `$1`, el segundo con `$2`, y así siguen en secuencia para los grupos de capturas de la expresión regular. Solo hay un grupo capturado en la expresión regular de la regla de redirección de la aplicación de ejemplo, por lo que solo hay un grupo insertado en la cadena de reemplazo, que es `$1`. Cuando se aplica la regla, la URL se convierte en `/redirected/1234/5678`.

### <a name="url-redirect-to-a-secure-endpoint"></a>Redirección de URL a un punto de conexión segura

Use `AddRedirectToHttps` para redirigir solicitudes HTTP al mismo host y ruta con HTTPS (`https://`). Si no se proporciona el código de estado, el middleware muestra de forma predeterminada 302 (Encontrado). Si no se proporciona el puerto, el middleware muestra de forma predeterminada `null`, lo que significa que el protocolo cambia a `https://` y el cliente accede al recurso en el puerto 443. En el ejemplo se muestra cómo establecer el código de estado en 301 (Movido definitivamente) y cambiar el puerto a 5001.

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

Use `AddRedirectToHttpsPermanent` para redirigir las solicitudes poco seguras al mismo host y ruta de acceso mediante el protocolo HTTPS seguro (`https://` en el puerto 443). El middleware establece el código de estado en 301 (Movido definitivamente).

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> Al redirigir a HTTPS sin la necesidad de disponer de reglas de redirección adicionales, se recomienda usar el Middleware de redirección de HTTPS. Para más información, vea el tema [Aplicación de HTTPS](xref:security/enforcing-ssl#require-https).

La aplicación de ejemplo es capaz de mostrar cómo usar `AddRedirectToHttps` o `AddRedirectToHttpsPermanent`. Agregue el método de extensión a `RewriteOptions`. Realice una solicitud poco segura a la aplicación en cualquier URL. Descarte la advertencia de seguridad del explorador que indica que el certificado autofirmado no es de confianza o cree una excepción para confiar en el certificado en cuestión.

Solicitud original mediante `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas](url-rewriting/_static/add_redirect_to_https.png)

Solicitud original mediante `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a>Reescritura de URL

Use `AddRewrite` para crear una regla para reescribir URL. El primer parámetro contiene la expresión regular para buscar coincidencias en la ruta de dirección de URL entrante. El segundo parámetro es la cadena de reemplazo. El tercer parámetro, `skipRemainingRules: {true|false}`, indica al middleware si al aplicar la regla actual tiene que omitir o no alguna regla de redirección adicional.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true);

    app.UseRewriter(options);
}
```

---

Solicitud original: `/rewrite-rule/1234/5678`

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas](url-rewriting/_static/add_rewrite.png)

Lo primero que se observa en la expresión regular es el acento circunflejo (`^`) al principio de la expresión. Esto significa que la búsqueda de coincidencias empieza al principio de la ruta de dirección de URL.

En el ejemplo anterior con la regla de redirección, `redirect-rule/(.*)`, no hay ningún acento circunflejo al principio de la expresión regular; por tanto, cualquier carácter puede preceder a `redirect-rule/` en la ruta de acceso para una coincidencia correcta.

| Ruta de acceso                               | Coincidir con |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | Sí   |
| `/my-cool-redirect-rule/1234/5678` | Sí   |
| `/anotherredirect-rule/1234/5678`  | Sí   |

La regla de reescritura, `^rewrite-rule/(\d+)/(\d+)`, solo encuentra rutas de acceso que empiezan con `rewrite-rule/`. Observe la diferencia de coincidencia entre la siguiente regla de reescritura y la regla de redirección anterior.

| Ruta de acceso                              | Coincidir con |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | Sí   |
| `/my-cool-rewrite-rule/1234/5678` | No    |
| `/anotherrewrite-rule/1234/5678`  | No    |

Después de la parte `^rewrite-rule/` de la expresión, hay dos grupos de captura, `(\d+)/(\d+)`. `\d` significa *buscar un dígito (número)*. El signo más (`+`) significa *buscar uno o más de los caracteres anteriores*. Por tanto, la URL debe contener un número seguido de una barra diagonal, seguida de otro número. Estos grupos de capturas se insertan en la URL de reescritura como `$1` y `$2`. La cadena de reemplazo de la regla de reescritura coloca los grupos capturados en la cadena de consulta. La ruta de acceso solicitada de `/rewrite-rule/1234/5678` se reescribe para obtener el recurso en `/rewritten?var1=1234&var2=5678`. Si una cadena de consulta está presente en la solicitud original, se conserva cuando se reescribe la URL.

No hay ningún recorrido de ida y vuelta al servidor para obtener el recurso. Si el recurso existe, se captura y se devuelve al cliente con un código de estado 200 (Correcto). Como el cliente no se redirige, la URL no cambia en la barra de direcciones del explorador. En lo que al cliente se refiere, la operación de reescritura de URL nunca se produjo.

> [!NOTE]
> Use `skipRemainingRules: true` siempre que sea posible, ya que las reglas de coincidencia son un proceso costoso y reducen el tiempo de respuesta de aplicación. Para obtener la respuesta más rápida de la aplicación:
> * Ordene las reglas de reescritura desde la que coincida con más frecuencia a la que coincida con menos frecuencia.
> * Omita el procesamiento de las reglas restantes cuando se produzca una coincidencia; no es necesario ningún procesamiento de reglas adicional.

### <a name="apache-modrewrite"></a>mod_rewrite de Apache

Aplique reglas mod_rewrite de Apache con `AddApacheModRewrite`. Asegúrese de que el archivo de reglas se implementa con la aplicación. Para obtener más información y ejemplos de reglas mod_rewrite, vea [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/) (mod_rewrite de Apache).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Se usa `StreamReader` para leer las reglas del archivo de reglas *ApacheModRewrite.txt*.

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=3-4,12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

El primer parámetro toma un `IFileProvider`, que se proporciona mediante una [inserción de dependencias](dependency-injection.md). Se inserta `IHostingEnvironment` para proporcionar `ContentRootFileProvider`. El segundo parámetro es la ruta de acceso al archivo de reglas, que es *ApacheModRewrite.txt* en la aplicación de ejemplo.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt");

    app.UseRewriter(options);
}
```

---

La aplicación de ejemplo redirige las solicitudes de `/apache-mod-rules-redirect/(.\*)` a `/redirected?id=$1`. El código de estado de la respuesta es 302 (Encontrado).

[!code[](url-rewriting/sample/ApacheModRewrite.txt)]

Solicitud original: `/apache-mod-rules-redirect/1234`

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a>Variables de servidor compatibles

El middleware admite las siguientes variables de servidor mod_rewrite de Apache:

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
* TIME
* TIME_DAY
* TIME_HOUR
* TIME_MIN
* TIME_MON
* TIME_SEC
* TIME_WDAY
* TIME_YEAR

### <a name="iis-url-rewrite-module-rules"></a>Reglas del Módulo URL Rewrite para IIS

Para usar reglas que se apliquen al Módulo URL Rewrite para IIS, use `AddIISUrlRewrite`. Asegúrese de que el archivo de reglas se implementa con la aplicación. No dirija el middleware para que use el archivo *web.config* cuando se ejecute en Windows Server IIS. Con IIS, estas reglas deben almacenarse fuera de *web.config* para evitar conflictos con el Módulo URL Rewrite para IIS. Para obtener más información y ejemplos de reglas del Módulo URL Rewrite para IIS, vea [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) (Uso del Módulo URL Rewrite 2.0) y [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference) (Referencia de configuración del Módulo URL Rewrite).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Se usa `StreamReader` para leer las reglas del archivo de reglas *IISUrlRewrite.xml*.

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=5-6,13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

El primer parámetro toma `IFileProvider`, mientras que el segundo parámetro es la ruta de acceso al archivo de reglas XML, que en la aplicación de ejemplo es *IISUrlRewrite.xml*.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml");

    app.UseRewriter(options);
}
```

---

La aplicación de ejemplo reescribe las solicitudes de `/iis-rules-rewrite/(.*)` a `/rewritten?id=$1`. La respuesta se envía al cliente con un código de estado 200 (Correcto).

[!code-xml[](url-rewriting/sample/IISUrlRewrite.xml)]

Solicitud original: `/iis-rules-rewrite/1234`

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas](url-rewriting/_static/add_iis_url_rewrite.png)

Si tiene un Módulo URL Rewrite para IIS activo con reglas configuradas en el nivel de servidor que podrían afectar a la aplicación de manera no deseada, puede deshabilitar el Módulo URL Rewrite para IIS para una aplicación. Para más información, vea [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules) (Deshabilitación de módulos de IIS).

#### <a name="unsupported-features"></a>Características no admitidas

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

El middleware publicado con ASP.NET Core 2.x no admite las siguientes características de Módulo URL Rewrite para IIS:

* Reglas de salida
* Variables de servidor personalizadas
* Caracteres comodín
* LogRewrittenUrl

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

El middleware publicado con ASP.NET Core 1.x no admite las siguientes características de Módulo URL Rewrite para IIS:

* Reglas globales
* Reglas de salida
* Mapas de reescritura
* Acción CustomResponse
* Variables de servidor personalizadas
* Caracteres comodín
* Action:CustomResponse
* LogRewrittenUrl

---

#### <a name="supported-server-variables"></a>Variables de servidor compatibles

El middleware admite las siguientes variables de servidor del Módulo URL Rewrite para IIS:

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
> También puede obtener `IFileProvider` a través de `PhysicalFileProvider`. Con este enfoque logrará mayor flexibilidad para la ubicación de los archivos de reglas de reescritura. Asegúrese de que los archivos de reglas de reescritura se implementan en el servidor en la ruta de acceso que proporcione.
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a>Regla basada en métodos

Use `Add(Action<RewriteContext> applyRule)` para implementar su propia lógica de la regla en un método. `RewriteContext` expone el `HttpContext` para usarlo en el método. `context.Result` determina cómo se administra el procesamiento adicional en la canalización.

| context.Result                       | Acción                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| `RuleResult.ContinueRules` (valor predeterminado) | Continuar aplicando reglas                                         |
| `RuleResult.EndResponse`             | Dejar de aplicar reglas y enviar la respuesta                       |
| `RuleResult.SkipRemainingRules`      | Dejar de aplicar reglas y enviar el contexto al siguiente middleware |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(RedirectXMLRequests);

    app.UseRewriter(options);
}
```

---

La aplicación de ejemplo muestra un método que redirige las solicitudes para las rutas de acceso que terminen con *.xml*. Si realiza una solicitud para `/file.xml`, se redirige a `/xmlfiles/file.xml`. El código de estado se establece en 301 (Movido definitivamente). Para una redirección, debe establecer explícitamente el código de estado de la respuesta; en caso contrario, se devuelve un código de estado 200 (Correcto) y no se produce la redirección en el cliente.

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet1)]

Solicitud original: `/file.xml`

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas para file.xml](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a>Regla basada en IRule

Use `Add(IRule)` para implementar su propia lógica de la regla en una clase que deriva de `IRule`. Al usar `IRule`, se logra mayor flexibilidad que si se usa el enfoque de reglas basadas en métodos. La clase derivada puede incluir un constructor, donde puede pasar parámetros para el método `ApplyRule`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=15-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

---

Se comprueba que los valores de los parámetros en la aplicación de ejemplo para `extension` y `newPath` cumplen ciertas condiciones. `extension` debe contener un valor, que debe ser *.png*, *.jpg* o *.gif*. Si `newPath` no es válido, se genera `ArgumentException` . Si se realiza una solicitud para *image.png*, se redirige a `/png-images/image.png`. Si se realiza una solicitud para *image.jpg*, se redirige a `/jpg-images/image.jpg`. El código de estado se establece en 301 (Movido definitivamente) y se establece `context.Result` para detener el procesamiento de reglas y enviar la respuesta.

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet2)]

Solicitud original: `/image.png`

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas para image.png](url-rewriting/_static/add_redirect_png_requests.png)

Solicitud original: `/image.jpg`

![Ventana del explorador con herramientas de desarrollo realizando un seguimiento de las solicitudes y las respuestas para image.jpg](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a>Ejemplos de expresiones regulares

| Objetivo | Cadena de expresión regular &<br>Ejemplo de coincidencia | Cadena de reemplazo &<br>Ejemplo de resultado |
| ---- | :-----------------------------: | :------------------------------------: |
| Ruta de acceso de reescritura en la cadena de consulta | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| Quitar barra diagonal final | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| Exigir barra diagonal final | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| Evitar reescritura de solicitudes específicas | `^(.*)(?<!\.axd)$` o `^(?!.*\.axd$)(.*)$`<br>Sí: `/resource.htm`<br>No: `/resource.axd` | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| Reorganizar los segmentos de URL | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| Reemplazar un segmento de URL | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a>Recursos adicionales

* [Inicio de aplicaciones](startup.md)
* [Middleware](xref:fundamentals/middleware/index)
* [Expresiones regulares en .NET](/dotnet/articles/standard/base-types/regular-expressions)
* [Lenguaje de expresiones regulares: referencia rápida](/dotnet/articles/standard/base-types/quick-ref)
* [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/) (mod_rewrite de Apache)
* [Using Url Rewrite Module 2.0 (for IIS)](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) (Uso del Módulo URL Rewrite 2.0 para IIS)
* [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference) (Referencia de configuración del Módulo URL Rewrite)
* [IIS URL Rewrite Module Forum](https://forums.iis.net/1152.aspx) (Foro del Módulo URL Rewrite para IIS)
* [Cómo simplificar la estructura de direcciones URL](https://support.google.com/webmasters/answer/76329?hl=en)
* [10 URL Rewriting Tips and Tricks](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/) (10 trucos y consejos para reescritura de URL)
* [To slash or not to slash](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html) (Usar la barra diagonal o no)
