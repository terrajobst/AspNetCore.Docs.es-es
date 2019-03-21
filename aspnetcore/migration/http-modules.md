---
title: Migración de módulos y controladores HTTP a middleware de ASP.NET Core
author: rick-anderson
description: ''
ms.author: tdykstra
ms.date: 12/07/2016
uid: migration/http-modules
ms.openlocfilehash: 516230a66ee3edba986c91d79684256aa8e4c994
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209851"
---
# <a name="migrate-http-handlers-and-modules-to-aspnet-core-middleware"></a>Migración de módulos y controladores HTTP a middleware de ASP.NET Core

Por [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)

En este artículo se muestra cómo migrar de ASP.NET existentes [módulos y controladores de system.webserver](/iis/configuration/system.webserver/) a ASP.NET Core [middleware](xref:fundamentals/middleware/index).

## <a name="modules-and-handlers-revisited"></a>Módulos y controladores para

Antes de continuar con el middleware de ASP.NET Core, en primer lugar repasemos cómo funcionan los controladores y módulos HTTP:

![Controlador de módulos](http-modules/_static/moduleshandlers.png)

**Los controladores son:**

* Las clases que implementan [IHttpHandler](/dotnet/api/system.web.ihttphandler)

* Utilizado para controlar solicitudes con un nombre de archivo dado o una extensión, como *informes*

* [Configurar](/iis/configuration/system.webserver/handlers/) en *Web.config*

**Los módulos son:**

* Las clases que implementan [IHttpModule](/dotnet/api/system.web.ihttpmodule)

* Se invoca para cada solicitud

* Capaz de cortocircuito (detener el procesamiento de una solicitud)

* Puede agregar a la respuesta HTTP, o crear sus propios

* [Configurar](/iis/configuration/system.webserver/modules/) en *Web.config*

**El orden en que los módulos de procesan las solicitudes entrantes viene determinado por:**

1. El [ciclo de vida de aplicación](https://msdn.microsoft.com/library/ms227673.aspx), que es una serie desencadenan ASP.NET: [BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest), etcetera. Cada módulo puede crear un controlador de eventos de uno o más.

2. Para el mismo evento, el orden en el que está configurados en *Web.config*.

Además de los módulos, puede agregar controladores para los eventos de ciclo de vida a su *Global.asax.cs* archivo. Estos controladores se ejecutan después de los controladores de los módulos configurados.

## <a name="from-handlers-and-modules-to-middleware"></a>Desde los controladores y módulos al middleware

**Middleware son más sencillas que los controladores y módulos HTTP:**

* Los módulos, controladores, *Global.asax.cs*, *Web.config* (excepto para la configuración de IIS) y el ciclo de vida de aplicación han desaparecido

* Se han realizado los roles de los módulos y controladores de middleware de la tecla TAB

* Middleware se configuran mediante código en lugar de en *Web.config*

* [Canalización bifurcación](xref:fundamentals/middleware/index#use-run-and-map) le permite enviar solicitudes al middleware específica, según no solo la dirección URL, sino también en los encabezados de solicitud, las cadenas de consulta, etcetera.

**Middleware son muy similares a los módulos:**

* Invoca en principio para cada solicitud

* Puede interrumpir una solicitud, por [no pasar la solicitud al siguiente middleware](#http-modules-shortcircuiting-middleware)

* Puede crear su propia respuesta HTTP

**Middleware y los módulos se procesan en un orden diferente:**

* Orden de middleware se basa en el orden en el que se ha insertado en la canalización de solicitudes, mientras que el orden de los módulos se basa principalmente en [ciclo de vida de aplicación](https://msdn.microsoft.com/library/ms227673.aspx) eventos

* Orden de middleware para las respuestas es el inverso de para las solicitudes, mientras que el orden de los módulos es el mismo para las solicitudes y respuestas

* Consulte [crear una canalización de middleware con IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)

![Software intermedio](http-modules/_static/middleware.png)

Tenga en cuenta cómo en la imagen anterior, el middleware de autenticación había cortocircuitado la solicitud.

## <a name="migrating-module-code-to-middleware"></a>Migrar código de módulo a middleware

Un módulo HTTP existente tendrá un aspecto similar al siguiente:

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

Como se muestra en el [Middleware](xref:fundamentals/middleware/index) página, un middleware de ASP.NET Core es una clase que expone un `Invoke` toma método un `HttpContext` y devolver un `Task`. El middleware nuevo tendrá un aspecto similar al siguiente:

<a name="http-modules-usemiddleware"></a>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

La plantilla de middleware anterior se ha tomado de la sección [escribir middleware](xref:fundamentals/middleware/write).

El *MyMiddlewareExtensions* clase auxiliar resulta más fácil de configurar su middleware en su `Startup` clase. El `UseMyMiddleware` método agrega la clase de middleware a la canalización de solicitudes. Los servicios requeridos por el middleware se insertarán en el constructor del middleware.

<a name="http-modules-shortcircuiting-middleware"></a>

El módulo puede finalizar una solicitud, por ejemplo, si el usuario no autorizado:

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

Un software intermedio encarga de ello llamando no `Invoke` en el siguiente middleware en la canalización. Tenga en cuenta que esto no termina por completo la solicitud, porque el middleware anterior aún se invocará cuando la respuesta ésta avanza a través de la canalización.

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

Al migrar a su middleware nueva funcionalidad de su módulo, es posible que no se compila el código porque la `HttpContext` clase ha cambiado significativamente en ASP.NET Core. [Más adelante](#migrating-to-the-new-httpcontext), verá cómo migrar a ASP.NET Core HttpContext nuevo.

## <a name="migrating-module-insertion-into-the-request-pipeline"></a>Migrar la inserción de módulo en la canalización de solicitudes

Los módulos HTTP normalmente se agregan a la canalización de solicitudes mediante *Web.config*:

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

Convertir esto por [agregar middleware nueva](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) a la canalización de solicitud en su `Startup` clase:

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

El lugar exacto de la canalización en la que insertar su middleware nueva depende de los eventos que tratan como un módulo (`BeginRequest`, `EndRequest`, etc.) y su orden en la lista de módulos de *Web.config*.

Como ya se ha indicado, no hay ningún ciclo de vida de la aplicación en ASP.NET Core y el orden en que se procesan las respuestas de middleware es diferente del usado por módulos. Esto podría hacer que su decisión de ordenación más difícil.

Si la ordenación se convierte en un problema, podría dividir el módulo en varios componentes de middleware que se pueden ordenar de forma independiente.

## <a name="migrating-handler-code-to-middleware"></a>Migrar código de controlador a middleware

Un controlador HTTP es algo parecido a esto:

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

En el proyecto de ASP.NET Core, lo traduciría en un middleware similar al siguiente:

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

Este middleware es muy similar al middleware correspondientes a los módulos. La única diferencia es que aquí no hay ninguna llamada a `_next.Invoke(context)`. Esto tiene sentido, porque el controlador no es al final de la canalización de solicitudes, por lo que no habrá ningún middleware siguiente para invocar.

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a>Migrar la inserción de controlador en la canalización de solicitudes

Configurar un controlador HTTP se realiza en *Web.config* y es algo parecido a esto:

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

Podría convertir esto agregando su middleware controlador nuevo a la canalización de solicitudes en su `Startup` (clase), similar al middleware convertido a partir de los módulos. El problema con este enfoque es que todas las solicitudes tendría que enviar a su nuevo software de controlador intermedio. Sin embargo, solo desea que las solicitudes con una extensión específica para llegar a su middleware. Esto le proporcionaría la misma funcionalidad que tenía con el controlador HTTP.

Una solución consiste en bifurcar la canalización de solicitudes con una extensión específica, mediante el `MapWhen` método de extensión. Para ello, en el mismo `Configure` método donde agregar el middleware de otro:

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

`MapWhen` toma estos parámetros:

1. Una expresión lambda que toma el `HttpContext` y devuelve `true` si la solicitud debe pasar a la rama. Esto significa que se pueden bifurcar solicitudes no solo en función de su extensión, sino también en los encabezados de solicitud, los parámetros de cadena de consulta, etcetera.

2. Una expresión lambda que toma un `IApplicationBuilder` y agrega el software intermedio para la bifurcación. Esto significa que puede agregar middleware adicional a la rama delante de su software de controlador intermedio.

Middleware agregado a la canalización antes de la rama se invocará en todas las solicitudes; la rama tendrá ningún impacto en ellos.

## <a name="loading-middleware-options-using-the-options-pattern"></a>Opciones de middleware con el patrón de opciones de carga

Algunos módulos y controladores tienen opciones de configuración que se almacenan en *Web.config*. Sin embargo, en ASP.NET Core se usa un nuevo modelo de configuración en lugar de *Web.config*.

El nuevo [sistema de configuración](xref:fundamentals/configuration/index) ofrece las siguientes opciones para resolver este problema:

* Insertar directamente las opciones al middleware, como se muestra en el [siguiente sección](#loading-middleware-options-through-direct-injection).

* Use la [patrón de opciones](xref:fundamentals/configuration/options):

1. Cree una clase para contener las opciones de middleware, por ejemplo:

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2. Store los valores de opción

   El sistema de configuración le permite almacenar valores de opción en cualquier lugar que desee. Sin embargo, los sitios más uso *appsettings.json*, por lo que te guiaremos ese enfoque:

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

   *MyMiddlewareOptionsSection* aquí es un nombre de sección. No debe ser el mismo que el nombre de la clase de opciones.

3. Asociar los valores de opción de la clase de opciones

    El patrón de opciones usa el marco de inserción de dependencias de ASP.NET Core para asociar el tipo de opciones (como `MyMiddlewareOptions`) con un `MyMiddlewareOptions` objeto que tiene las opciones reales.

    Actualización de su `Startup` clase:

   1. Si usas *appsettings.json*, agréguelo al generador de configuración en el `Startup` constructor:

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

   2. Configurar el servicio de opciones:

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

   3. Asociar las opciones de la clase de opciones:

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4. Insertar las opciones en el constructor de middleware. Esto es similar a la inserción en un controlador de opciones.

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

   El [UseMiddleware](#http-modules-usemiddleware) método de extensión que agrega el middleware para la `IApplicationBuilder` se encarga de inserción de dependencias.

   Esto no se limita a `IOptions` objetos. Cualquier otro objeto que requiere su middleware puede insertarse de esta manera.

## <a name="loading-middleware-options-through-direct-injection"></a>Opciones de middleware a través de inyección directa de carga

El patrón de opciones tiene la ventaja que crea un acoplamiento flexible entre los valores de opciones y sus consumidores. Una vez que haya asociado una clase de opciones con los valores de opciones real, cualquier otra clase puede obtener acceso a las opciones mediante el marco de inserción de dependencia. No hay ninguna necesidad de pasar los valores de opciones.

Este modo se divide aunque si desea utilizar el mismo middleware dos veces, con diferentes opciones. Por ejemplo un middleware de autorización usado en distintas ramas, lo que permite diferentes roles. No se puede asociar dos objetos diferentes opciones con la clase de una de las opciones.

La solución consiste en obtener los objetos de opciones con los valores de opciones real en su `Startup` de clases y las pasará directamente a cada instancia del middleware.

1. Agregar una segunda clave para *appsettings.json*

   Para agregar un segundo conjunto de opciones para la *appsettings.json* , debe usar una clave nueva para identificarlo:

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2. Recuperar valores de opciones y pasarlos al middleware. El `Use...` método de extensión (que agrega el middleware a la canalización) es un lugar lógico para pasar los valores de opción: 

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

3. Habilitar el middleware para que tome un parámetro de opciones. Proporcionar una sobrecarga de la `Use...` método de extensión (que toma el parámetro options y lo pasa al `UseMiddleware`). Cuando `UseMiddleware` se llama con parámetros, pasa los parámetros a su constructor de middleware cuando crea una instancia del objeto de middleware.

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

   Tenga en cuenta cómo se ajusta el objeto de opciones en un `OptionsWrapper` objeto. Esto implementa `IOptions`, tal y como se esperaba el constructor de middleware.

## <a name="migrating-to-the-new-httpcontext"></a>Migrar a nuevo HttpContext

Ya ha visto que la `Invoke` método en su middleware toma un parámetro de tipo `HttpContext`:

```csharp
public async Task Invoke(HttpContext context)
```

`HttpContext` ha cambiado significativamente en ASP.NET Core. En esta sección se muestra cómo traducir las propiedades más utilizadas de [System.Web.HttpContext](/dotnet/api/system.web.httpcontext) al nuevo `Microsoft.AspNetCore.Http.HttpContext`.

### <a name="httpcontext"></a>HttpContext

**HttpContext.Items** se convierte en:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

**Identificador único de la solicitud (ningún homólogo System.Web.HttpContext)**

Proporciona un identificador único para cada solicitud. Resulta muy útil para incluir en los registros.

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a>HttpContext.Request

**HttpContext.Request.HttpMethod** se convierte en:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

**HttpContext.Request.QueryString** se convierte en:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

**HttpContext.Request.Url** y **HttpContext.Request.RawUrl** traducir al:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

**HttpContext.Request.IsSecureConnection** se convierte en:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

**HttpContext.Request.UserHostAddress** se convierte en:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

**HttpContext.Request.Cookies** se convierte en:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

**HttpContext.Request.RequestContext.RouteData** se convierte en:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

**HttpContext.Request.Headers** se convierte en:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

**HttpContext.Request.UserAgent** se convierte en:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

**HttpContext.Request.UrlReferrer** se convierte en:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

**HttpContext.Request.ContentType** se convierte en:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

**HttpContext.Request.Form** se convierte en:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> Leer valores de formulario solo si el tipo de contenido secundaria es *x--www-form-urlencoded* o *datos del formulario*.

**HttpContext.Request.InputStream** se convierte en:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> Use este código solo en un middleware de tipo de controlador, al final de una canalización.
>
>Puede leer el cuerpo sin formato, como se muestra arriba solo una vez por solicitud. Middleware al intentar leer el cuerpo después de la primera lectura leerá un cuerpo vacío.
>
>Esto no se aplica a un formulario de lectura como se mostró anteriormente, ya se realiza desde un búfer.

### <a name="httpcontextresponse"></a>HttpContext.Response

**HttpContext.Response.Status** y **HttpContext.Response.StatusDescription** traducir al:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

**HttpContext.Response.ContentEncoding** y **HttpContext.Response.ContentType** traducir al:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

**HttpContext.Response.ContentType** en su propio también se traduce en:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

**HttpContext.Response.Output** se convierte en:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

**HttpContext.Response.TransmitFile**

Servir como un archivo se analiza [aquí](../fundamentals/request-features.md#middleware-and-request-features).

**HttpContext.Response.Headers**

Enviar encabezados de respuesta resulta complicada por el hecho de que si se establece después de que nada se ha escrito en el cuerpo de respuesta, no se enviará.

La solución es establecer un método de devolución de llamada que se llamará derecha antes de escribir en el que se inicie la respuesta. Esto se hace mejor al principio de la `Invoke` método en el middleware. Resulta que este método de devolución de llamada que establece los encabezados de respuesta.

El código siguiente define un método de devolución de llamada denominado `SetHeaders`:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

El `SetHeaders` el método de devolución de llamada tendría el aspecto siguiente:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

**HttpContext.Response.Cookies**

Las cookies de viaje al explorador en un *Set-Cookie* encabezado de respuesta. Como resultado, al envío de cookies, requiere la misma devolución de llamada que se usa para enviar los encabezados de respuesta:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

El `SetCookies` el método de devolución de llamada podría ser similar al siguiente:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a>Recursos adicionales

* [Introducción a los módulos y controladores HTTP](/iis/configuration/system.webserver/)
* [Configuración](xref:fundamentals/configuration/index)
* [Inicio de aplicaciones](xref:fundamentals/startup)
* [Middleware](xref:fundamentals/middleware/index)
