---
title: "Migrar controladores HTTP y módulos ASP.NET Core middleware"
author: rick-anderson
description: 
manager: wpickett
ms.author: tdykstra
ms.date: 12/07/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/http-modules
ms.openlocfilehash: 8aac6c649b22dc8f6cfc916aa78d56efad7821a0
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2018
---
# <a name="migrating-http-handlers-and-modules-to-aspnet-core-middleware"></a>Migrar controladores HTTP y módulos ASP.NET Core middleware 

Por [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)

Este artículo muestra cómo migrar ASP.NET existente [módulos HTTP y controladores de system.webserver](https://docs.microsoft.com/iis/configuration/system.webserver/) a ASP.NET Core [middleware](xref:fundamentals/middleware/index).

## <a name="modules-and-handlers-revisited"></a>Módulos y controladores revisan

Antes de proceder con middleware de ASP.NET Core, primero Resumamos cómo funcionan los controladores y módulos HTTP:

![Controlador de módulos](http-modules/_static/moduleshandlers.png)

**Los controladores son:**

   * Las clases que implementan [IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)

   * Utilizado para controlar solicitudes con un nombre de archivo especificado o una extensión, como *informes*

   * [Configurar](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/) en *Web.config*

**Los módulos son:**

   * Las clases que implementan [IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)

   * Se invoca para cada solicitud

   * Capaz de cortocircuito (detener el procesamiento de una solicitud)

   * Puede agregar a la respuesta HTTP, o crear sus propios

   * [Configurar](https://docs.microsoft.com//iis/configuration/system.webserver/modules/) en *Web.config*

**El orden en el que los módulos de procesan las solicitudes entrantes viene determinado por:**

   1. El [ciclo de vida de aplicación](https://msdn.microsoft.com/library/ms227673.aspx), que es un eventos serie desencadenados por ASP.NET: [BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest), etcetera. Cada módulo puede crear un controlador para uno o varios eventos.

   2. Para el mismo evento, el orden en el que está configurados en *Web.config*.

Además de los módulos, puede agregar controladores para los eventos de ciclo de vida a sus *Global.asax.cs* archivo. Estos controladores se ejecutan después de los controladores en los módulos configurados.

## <a name="from-handlers-and-modules-to-middleware"></a>De controladores y módulos middleware

**Middleware son más sencillas que controladores y módulos HTTP:**

   * Módulos, controladores, *Global.asax.cs*, *Web.config* (excepto para la configuración de IIS) y el ciclo de vida de la aplicación han desaparecido

   * Los roles de los módulos y los controladores se hayan realizado el middleware

   * Middleware se configuran mediante código en lugar de en *Web.config*

   * [Bifurcación de canalización](xref:fundamentals/middleware/index#middleware-run-map-use) le permite enviar las solicitudes de middleware específico, según no solo la dirección URL sino también en los encabezados de solicitud, las cadenas de consulta, etcetera.

**Middleware son muy similares a los módulos:**

   * Se invoca en la entidad de seguridad para todas las solicitudes

   * Capaz de una solicitud de cortocircuito por [no pasar la solicitud al siguiente middleware](#http-modules-shortcircuiting-middleware)

   * Puede crear su propia respuesta HTTP

**El software intermedio y módulos se procesan en un orden diferente:**

   * Orden de middleware se basa en el orden en el que se insertan en la canalización de solicitudes, mientras que el orden de los módulos se basa principalmente en [ciclo de vida de aplicación](https://msdn.microsoft.com/library/ms227673.aspx) eventos

   * Orden de middleware para las respuestas es el inverso de la para las solicitudes, mientras que el orden de los módulos es el mismo para las solicitudes y respuestas

   * Vea [crear una canalización de middleware con IApplicationBuilder](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder)

![Software intermedio](http-modules/_static/middleware.png)

Tenga en cuenta cómo en la imagen anterior, el middleware de autenticación había cortocircuitado la solicitud.

## <a name="migrating-module-code-to-middleware"></a>Migrar código del módulo middleware

Un módulo HTTP existente tendrá un aspecto similar al siguiente:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

Como se muestra en el [Middleware](xref:fundamentals/middleware/index) página, un middleware de ASP.NET Core es una clase que expone un `Invoke` tomar método un `HttpContext` y devolver un `Task`. El middleware de nuevo tendrá un aspecto similar al siguiente:

<a name="http-modules-usemiddleware"></a>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

La plantilla de middleware anterior se realizó desde la sección de [escribir middleware](xref:fundamentals/middleware/index#middleware-writing-middleware).

El *MyMiddlewareExtensions* clase auxiliar resulta más fácil de configurar el middleware en su `Startup` clase. El `UseMyMiddleware` método agrega la clase de middleware a la canalización de solicitudes. Servicios requeridos por el middleware obtengan insertados en el constructor del middleware.

<a name="http-modules-shortcircuiting-middleware"></a>

El módulo podría terminar una solicitud, por ejemplo, si el usuario no autorizado:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

Un middleware encarga de ello llamando no `Invoke` en el siguiente middleware en la canalización. Tenga en cuenta que esto no finaliza completamente la solicitud, porque middlewares anterior aún se invocará cuando la respuesta realiza su forma de volver a través de la canalización.

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

Cuando se migra la funcionalidad de su módulo el middleware de nuevo, es posible que el código no se compila porque la `HttpContext` clase ha cambiado significativamente en ASP.NET Core. [Más adelante en](#migrating-to-the-new-httpcontext), verá cómo migrar a la nueva HttpContext principales de ASP.NET.

## <a name="migrating-module-insertion-into-the-request-pipeline"></a>Migrar inserción de módulo en la canalización de solicitud

Módulos HTTP normalmente se agregan a la canalización de solicitud con *Web.config*:

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

Convertir esto por [agregar su middleware nueva](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder) a la canalización de solicitud en su `Startup` clase:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

El lugar exacto de la canalización donde insertar el middleware nueva depende de los eventos que tratan como un módulo (`BeginRequest`, `EndRequest`, etc.) y su orden en la lista de módulos de *Web.config*.

Como anteriormente indicó, no hay ningún ciclo de vida de la aplicación de ASP.NET Core y el orden en que se procesan las respuestas middleware distinto del orden usado por módulos. Esto podría tomar una decisión de ordenación más complejo.

Si la ordenación se convierte en un problema, puede dividir el módulo de varios componentes de software intermedio que se pueden ordenar de forma independiente.

## <a name="migrating-handler-code-to-middleware"></a>Migrar código del controlador de middleware

Un controlador HTTP es algo parecido a esto:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

En el proyecto de ASP.NET Core, se traduciría en un middleware similar al siguiente:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

Este middleware es muy similar del middleware correspondiente a los módulos. La única diferencia es que aquí no hay ninguna llamada a `_next.Invoke(context)`. Que tiene sentido, porque el controlador no es el final de la canalización de solicitud, por lo que no habrá ningún siguiente middleware para invocar.

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a>Migrar inserción de controlador en la canalización de solicitud

La configuración de un controlador HTTP se realiza *Web.config* y es algo parecido a esto:

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

Puede convertir este agregando el middleware de controlador nuevo a la canalización de solicitud en su `Startup` (clase), similar a middleware convertido a partir de los módulos. El problema con este enfoque es que enviaría todas las solicitudes para el middleware de controlador nuevo. Sin embargo, sólo desea que las solicitudes con una extensión específica para alcanzar el middleware. Esto le proporcionaría la misma funcionalidad que no tenga el controlador HTTP.

Una solución es crear una bifurcación de la canalización de solicitudes con una extensión determinada, mediante el `MapWhen` método de extensión. Para ello, en el mismo `Configure` método donde se agrega el middleware de otro:

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

`MapWhen`toma estos parámetros:

1. Una expresión lambda que toma el `HttpContext` y devuelve `true` si la solicitud debe pasar a la rama. Esto significa que puede crear una bifurcación de solicitudes no solo en función de su extensión, sino también en los encabezados de solicitud, los parámetros de cadena de consulta, etcetera.

2. Una expresión lambda que toma un `IApplicationBuilder` y agrega el software intermedio para la bifurcación. Esto significa que puede agregar middleware adicional a la bifurcación delante el middleware de controlador.

Software intermedio se agrega a la canalización antes de que se invocará la bifurcación en todas las solicitudes; la bifurcación no tendrá ningún impacto en ellos.

## <a name="loading-middleware-options-using-the-options-pattern"></a>Opciones de middleware usando el patrón de opciones de carga

Algunos controladores y módulos tienen opciones de configuración que se almacenan en *Web.config*. Sin embargo, en ASP.NET Core un nuevo modelo de configuración se utiliza en lugar de *Web.config*.

El nuevo [sistema de configuración](xref:fundamentals/configuration/index) ofrece las siguientes opciones para resolver este problema:

* Insertar directamente las opciones en el middleware, como se muestra en el [próxima sección](#loading-middleware-options-through-direct-injection).

* Use la [patrón opciones](xref:fundamentals/configuration/options):

1.  Cree una clase para contener las opciones de middleware, por ejemplo:

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2.  Almacenar los valores de opción

    El sistema de configuración le permite almacenar valores de opción en cualquier lugar que desee. Sin embargo, los sitios más uso *appSettings.JSON que se*, por lo que le llevaremos a ese método:

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

    *MyMiddlewareOptionsSection* aquí es un nombre de sección. No tiene que ser el mismo que el nombre de la clase de opciones.

3. Asociar a los valores de opción de la clase de opciones

    El patrón de opciones usa framework de inyección de dependencia de ASP.NET Core para asociar el tipo de opciones (como `MyMiddlewareOptions`) con un `MyMiddlewareOptions` objeto que tiene las opciones reales.

    Actualización de su `Startup` clase:

    1.  Si usas *appSettings.JSON que se*, agregar para el generador de configuración de la `Startup` constructor:

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

    2.  Configurar el servicio de opciones:

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    3.  Asocie las opciones de la clase de opciones:

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4.  Insertar las opciones en el constructor de middleware. Esto es similar a insertar opciones en un controlador.

  [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

  El [UseMiddleware](#http-modules-usemiddleware) método de extensión que agrega el middleware para la `IApplicationBuilder` se encarga de inserción de dependencias.

  Esto no se limita a `IOptions` objetos. Puede insertar cualquier otro objeto que requiere el middleware de esta manera.

## <a name="loading-middleware-options-through-direct-injection"></a>Opciones de middleware a través de inyección directa de carga

El patrón de opciones tiene la ventaja de que crea un acoplamiento entre los valores de las opciones y sus consumidores flexible. Una vez que haya asociado una clase de opciones con los valores de opciones real, cualquier otra clase puede obtener acceso a las opciones a través del marco de inyección de dependencia. No es necesario pasar valores de opciones.

Este modo se divide aunque si desea utilizar el mismo middleware dos veces, con diferentes opciones. Por ejemplo un middleware de autorización utilizado en distintas ramas que permite a distintos roles. No se puede asociar dos objetos diferentes opciones con la clase de uno opciones.

La solución consiste en obtener los objetos de opciones con los valores de opciones real en su `Startup` clase y las pasará directamente a cada instancia de su middleware.

1.  Agregar una segunda clave para *appSettings.JSON que se*

    Para agregar un segundo conjunto de opciones para la *appSettings.JSON que se* de archivos, use una nueva clave para identificar de forma exclusiva:

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2.  Recuperar valores de las opciones y pasarlos a middleware. El `Use...` método de extensión (que agrega el middleware a la canalización) es un punto lógico para pasar los valores de opción: 

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

4.  Habilita el middleware tomar un parámetro de opciones. Proporcionar una sobrecarga de la `Use...` método de extensión (que toma el parámetro options y lo pasa a `UseMiddleware`). Cuando `UseMiddleware` se llama con parámetros, pasa los parámetros al constructor de middleware cuando crea una instancia del objeto de middleware.

    [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

    Tenga en cuenta cómo se ajusta el objeto de opciones en un `OptionsWrapper` objeto. Esto implementa `IOptions`, tal y como se esperaba el constructor de middleware.

## <a name="migrating-to-the-new-httpcontext"></a>Migrar a la nueva HttpContext

Se ha visto anteriormente que el `Invoke` método en el middleware toma un parámetro de tipo `HttpContext`:

```csharp
public async Task Invoke(HttpContext context)
```

`HttpContext`ha cambiado significativamente en ASP.NET Core. Esta sección muestra cómo traducir las propiedades más utilizadas de [System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext) al nuevo `Microsoft.AspNetCore.Http.HttpContext`.

### <a name="httpcontext"></a>HttpContext

**HttpContext.Items** se convierte en:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

**Identificador único de la solicitud (ningún homólogo System.Web.HttpContext)**

Proporciona un identificador único para cada solicitud. Resulta muy útil para incluir en los registros.

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a>HttpContext.Request

**HttpContext.Request.HttpMethod** se convierte en:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

**HttpContext.Request.QueryString** se convierte en:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

**HttpContext.Request.Url** y **HttpContext.Request.RawUrl** traducir al:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

**HttpContext.Request.IsSecureConnection** se convierte en:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

**HttpContext.Request.UserHostAddress** se convierte en:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

**HttpContext.Request.Cookies** se convierte en:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

**HttpContext.Request.RequestContext.RouteData** se convierte en:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

**HttpContext.Request.Headers** se convierte en:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

**HttpContext.Request.UserAgent** se convierte en:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

**HttpContext.Request.UrlReferrer** se convierte en:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

**HttpContext.Request.ContentType** se convierte en:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

**HttpContext.Request.Form** se convierte en:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> Leer valores de formulario sólo si el tipo de contenido sub es *x--www-form-urlencoded* o *datos del formulario*.

**HttpContext.Request.InputStream** se convierte en:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> Use este código solo en un middleware de tipo de controlador, al final de una canalización.
>
>Puede leer el cuerpo sin formato, como se indicó anteriormente una sola vez por solicitud. Middleware de intentar leer el cuerpo después de la primera lectura leerá un cuerpo vacío.
>
>Esto no se aplica a un formulario de lectura como se muestra anteriormente, ya proviene de un búfer.

### <a name="httpcontextresponse"></a>HttpContext.Response

**HttpContext.Response.Status** y **HttpContext.Response.StatusDescription** traducir al:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

**HttpContext.Response.ContentEncoding** y **HttpContext.Response.ContentType** traducir al:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

**HttpContext.Response.ContentType** en su propio también se convierte en:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

**HttpContext.Response.Output** se convierte en:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

**HttpContext.Response.TransmitFile**

Servir el archivo se analiza [aquí](../fundamentals/request-features.md#middleware-and-request-features).

**HttpContext.Response.Headers**

Enviar encabezados de respuesta se ve complicado por el hecho de que si se establece después de que algo se ha escrito en el cuerpo de respuesta, no se enviará.

La solución consiste en establecer un método de devolución de llamada que se llamará derecha antes de escribir en la respuesta se inicia. Es mejor hacerlo al principio de la `Invoke` método en el middleware. Es este método de devolución de llamada que establece los encabezados de respuesta.

El código siguiente define un método de devolución de llamada llama `SetHeaders`:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

El `SetHeaders` método de devolución de llamada sería similar al siguiente:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

**HttpContext.Response.Cookies**

Las cookies de viaje en el explorador en un *Set-Cookie* encabezado de respuesta. Como resultado, al enviar cookies, requiere la misma devolución de llamada que se usan para enviar los encabezados de respuesta:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

El `SetCookies` método de devolución de llamada sería similar al siguiente:

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a>Recursos adicionales

* [Información general de los módulos HTTP y controladores HTTP](/iis/configuration/system.webserver/)
* [Configuración](xref:fundamentals/configuration/index)
* [Inicio de aplicaciones](xref:fundamentals/startup)
* [Middleware](xref:fundamentals/middleware/index)
