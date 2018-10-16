---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Habilitación de solicitudes entre orígenes en ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Muestra cómo admitir el uso compartido de recursos entre orígenes (CORS) en ASP.NET Web API.
ms.author: riande
ms.date: 10/10/2018
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 118b779c89edb874f7f928315d1094738be5f097
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348525"
---
<a name="enable-cross-origin-requests-in-aspnet-web-api-2"></a>Habilitar solicitudes entre orígenes en ASP.NET Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

> La seguridad del explorador impide que una página web realice solicitudes AJAX a otro dominio. Esta restricción se denomina *"directiva de mismo origen"* e impide que un sitio malintencionado lea los datos confidenciales desde otro sitio. Sin embargo, a veces es posible que desee permitir que otros sitios de llamada a la API web.
>
> El [uso compartido de recursos entre orígenes](http://www.w3.org/TR/cors/) (CORS) es un estándar del W3C que permite que un servidor modere la directiva de mismo origen. Mediante CORS, un servidor puede permitir explícitamente algunas solicitudes entre orígenes y rechazar otras. CORS es más seguro y flexible que técnicas anteriores como [JSONP](http://en.wikipedia.org/wiki/JSONP). Este tutorial muestra cómo se habilita la CORS en la aplicación de API Web.
>
> ## <a name="software-used-in-the-tutorial"></a>Software que se usa en el tutorial
>
> - [Visual Studio](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - Web API 2.2

## <a name="introduction"></a>Introducción

Este tutorial se muestra la compatibilidad con CORS en ASP.NET Web API. Comenzaremos creando dos proyectos ASP.NET: uno llamado "WebService", que hospeda un controlador Web API, y otra llamada "WebClient", que llama al servicio Web. Dado que las dos aplicaciones se hospedan en dominios diferentes, una solicitud de AJAX de WebClient para el servicio Web es una solicitud entre orígenes.

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a>¿Qué es el "mismo origen"?

Dos direcciones URL tienen el mismo origen si tienen puertos, hosts y esquemas idénticos. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Estas dos direcciones URL tienen el mismo origen:

- `http://example.com/foo.html`
- `http://example.com/bar.html`

Estas direcciones URL tienen orígenes diferentes de los dos anteriores:

- `http://example.net`: dominio diferente
- `http://example.com:9000/foo.html`: puerto diferente
- `https://example.com/foo.html`: esquema diferente
- `http://www.example.com/foo.html`: subdominio diferente

> [!NOTE]
> Internet Explorer no tiene en cuenta el puerto al comparar orígenes.

## <a name="create-the-webservice-project"></a>Crear el proyecto WebService

> [!NOTE]
> En esta sección se da por supuesto que ya sabe cómo crear proyectos de Web API. Si no, consulte [Introducción a ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

1. Inicie Visual Studio y cree un nuevo **aplicación Web ASP.NET (.NET Framework)** proyecto.
2. En el **nueva aplicación Web ASP.NET** cuadro de diálogo, seleccione el **vacía** plantilla de proyecto. En **agregar carpetas y referencias centrales para**, seleccione el **API Web** casilla de verificación.

   ![Nuevo cuadro de diálogo de proyecto ASP.NET en Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog.png)

3. Agregar un controlador de Web API llamado `TestController` con el código siguiente:

   [!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

4. Puede ejecutar la aplicación localmente o implementar en Azure. (Para las capturas de pantalla en este tutorial, la aplicación se implementa en Azure App Service Web Apps.) Para comprobar que funciona la API web, vaya a `http://hostname/api/test/`, donde *hostname* es el dominio donde se implementa la aplicación. Debería ver el texto de respuesta, &quot;obtener: mensaje de prueba&quot;.

   ![Mensaje de prueba de que se muestra de explorador Web](enabling-cross-origin-requests-in-web-api/_static/image4.png)

## <a name="create-the-webclient-project"></a>Crear el proyecto de WebClient

1. Crear otro **aplicación Web ASP.NET (.NET Framework)** del proyecto y seleccione el **MVC** plantilla de proyecto. Si lo desea, seleccione **Cambiar autenticación** > **sin autenticación**. No necesita autenticación para este tutorial.

   ![Plantilla MVC en el cuadro de diálogo nuevo proyecto de ASP.NET en Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog-mvc.png)

2. En **el Explorador de soluciones**, abra el archivo *Views/Home/Index.cshtml*. Reemplace el código de este archivo por lo siguiente:

   [!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

   Para el *serviceUrl* variable, use el identificador URI de la aplicación de servicio Web.

3. Ejecutar la aplicación de WebClient localmente o publicarla en otro sitio Web.

Al hacer clic en el botón "Pruébelo", se envía una solicitud AJAX a la aplicación de servicio Web mediante el método HTTP que se muestran en el cuadro de lista desplegable (GET, POST o PUT). Esto le permite examinar las solicitudes entre orígenes diferentes. Actualmente, la aplicación de servicio Web no admite la CORS, por lo que si hace clic en el botón aparece un error.

!['Try' error en el explorador](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> Si observa que el tráfico HTTP en una herramienta como [Fiddler](http://www.telerik.com/fiddler), verá que el explorador envía la solicitud GET y la solicitud se realiza correctamente, pero la llamada AJAX devuelve un error. Es importante comprender que la directiva de mismo origen no evita que el Explorador de *enviar* la solicitud. En su lugar, impide que la aplicación vean el *respuesta*.

![Depurador de Fiddler web que muestra las solicitudes web](enabling-cross-origin-requests-in-web-api/_static/image8.png)

## <a name="enable-cors"></a>Habilitar la CORS

Ahora vamos a habilitar la CORS en la aplicación de servicio Web. En primer lugar, agregue el paquete NuGet de la CORS. En Visual Studio, desde el **herramientas** menú, seleccione **Administrador de paquetes de NuGet**, a continuación, seleccione **Package Manager Console**. En la ventana de consola de administrador de paquetes, escriba el siguiente comando:

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

Este comando instala el paquete más reciente y actualiza todas las dependencias, incluidas las bibliotecas de core Web API. Use el `-Version` marca como destino una versión específica. El paquete de la CORS necesita Web API 2.0 o posterior.

Abra el archivo *aplicación\_Start/WebApiConfig.cs*. Agregue el código siguiente a la **WebApiConfig.Register** método:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

A continuación, agregue el **[EnableCors]** atributo a la `TestController` clase:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

Para el *orígenes* parámetro, use el identificador URI que se implementó la aplicación de WebClient. Esto permite solicitudes entre orígenes de WebClient, mientras todavía no se permiten todas las demás solicitudes entre dominios. Más adelante, describiré los parámetros de **[EnableCors]** con más detalle.

No incluya una barra diagonal al final de la *orígenes* dirección URL.

Volver a implementar la aplicación de servicio Web actualizada. No es necesario actualizar WebClient. Ahora debería realizarse correctamente la solicitud de AJAX de WebClient. Todos los métodos GET, PUT y POST se permiten.

![Mensaje de prueba correcta de mostrando de explorador Web](enabling-cross-origin-requests-in-web-api/_static/image9.png)

## <a name="how-cors-works"></a>Cómo funciona la CORS

En esta sección se describe lo que sucede en una solicitud CORS, en el nivel de los mensajes HTTP. Es importante entender cómo funciona la CORS, lo que puede configurar el **[EnableCors]** atributo correctamente y solucionar problemas si algo no funciona según lo esperado.

La especificación de CORS presenta varios encabezados HTTP nuevos que permiten las solicitudes entre orígenes. Si un explorador es compatible con la CORS, establece estos encabezados automáticamente para las solicitudes entre orígenes; no es necesario hacer nada especial en el código de JavaScript.

Este es un ejemplo de una solicitud entre orígenes. El encabezado de "Origen" proporciona el dominio del sitio que está realizando la solicitud.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

Si el servidor permite la solicitud, Establece el encabezado de Access-Control-Allow-Origin. El valor de este encabezado coincide con el encabezado de origen, o es el valor de carácter comodín "\*", lo que significa que se permite cualquier origen.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

Si la respuesta no incluye el encabezado de Access-Control-Allow-Origin, se produce un error en la solicitud de AJAX. En concreto, el explorador no permite la solicitud. Incluso si el servidor devuelve una respuesta correcta, el explorador no disponer de la respuesta a la aplicación cliente.

**Solicitudes preparatorias**

Para algunas solicitudes CORS, el explorador envía una solicitud adicional, denominada "solicitud preparatoria," antes de enviar la solicitud para el recurso real.

El explorador puede omitir la solicitud preparatoria si se cumplen las condiciones siguientes:

- El método de solicitud es GET, HEAD o POST, *y*
- La aplicación no establece los encabezados de solicitud que no sean Accept, Accept-Language, Content-Language, Content-Type o último-Event-ID, *y*
- El encabezado Content-Type (si se establece) es uno de los siguientes:

    - application/x-www-form-urlencoded
    - varias partes/datos de formulario
    - text/plain

Se aplica la regla acerca de los encabezados de solicitud a los encabezados de la aplicación se establece mediante una llamada a **setRequestHeader** en el **XMLHttpRequest** objeto. (La especificación de CORS llama a estos "encabezados de solicitud de autor"). La regla no se aplica a los encabezados de la *explorador* puede establecer como User-Agent, Host o Content-Length.

Este es un ejemplo de una solicitud de preflight:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

La solicitud preparatoria usa el método HTTP OPTIONS. Incluye dos encabezados especiales:

- Access-Control-Request-Method: el método HTTP que se usará para la solicitud real.
- Access-Control-Request-Headers: Una lista de encabezados de solicitud que el *aplicación* establecida en la solicitud real. (Nuevamente, esto no incluye los encabezados que establece el explorador.)

Este es un ejemplo de respuesta, suponiendo que el servidor permite la solicitud:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

La respuesta incluye un encabezado Access-Control-Allow-Methods que enumera los métodos permitidos y, opcionalmente, un encabezado Access-Control-Allow-Headers, que muestra los encabezados permitidos. Si la solicitud de comprobaciones preparatorias se realiza correctamente, el explorador envía la solicitud real, como se ha descrito anteriormente.

## <a name="scope-rules-for-enablecors"></a>Reglas de ámbito para [EnableCors]

Puede habilitar la CORS por cada acción, por controlador o globalmente para todos los controladores de API Web en la aplicación.

**Por acción**

Para habilitar CORS para una única acción, establezca el **[EnableCors]** atributo en el método de acción. En el ejemplo siguiente se habilita la CORS para el `GetItem` solo método.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

**Por cada controlador**

Si establece **[EnableCors]** en la clase de controlador, se aplica a todas las acciones en el controlador. Para deshabilitar CORS para una acción, agregue el **[DisableCors]** atributo a la acción. El ejemplo siguiente habilita la CORS para todos los métodos excepto `PutItem`.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

**Globalmente**

Para habilitar CORS para todos los controladores de API Web en la aplicación, pase un **EnableCorsAttribute** de instancia para el **EnableCors** método:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

Si establece el atributo en más de un ámbito, el orden de prioridad es:

1. Acción
2. Controlador
3. Global

## <a name="set-the-allowed-origins"></a>Establecer los orígenes permitidos

El *orígenes* parámetro de la **[EnableCors]** atributo especifica qué orígenes tienen permiso para acceder al recurso. El valor es una lista separada por comas de los orígenes permitidos.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

También puede usar el valor de carácter comodín "\*" para permitir las solicitudes de los orígenes.

Considere detenidamente antes de permitir las solicitudes desde cualquier origen. Significa que literalmente cualquier sitio Web puede realizar llamadas de AJAX a la API web.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

## <a name="set-the-allowed-http-methods"></a>Establecer los métodos HTTP permitidos

El *métodos* parámetro de la **[EnableCors]** atributo especifica qué métodos HTTP pueden tener acceso al recurso. Para permitir que todos los métodos, use el valor de carácter comodín "\*". El ejemplo siguiente permite solo las solicitudes GET y POST.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

## <a name="set-the-allowed-request-headers"></a>Establecer los encabezados de solicitudes permitidos

En este artículo se describe cómo una solicitud de preflight podría incluir un encabezado de Access-Control-Request-Headers, enumerar los encabezados HTTP establecidos por la aplicación (el llamado "author encabezados de solicitud"). El *encabezados* parámetro de la **[EnableCors]** atributo especifica qué encabezados de solicitud del autor se permiten. Para permitir que todos los encabezados, establezca *encabezados* a "\*". A encabezados específicos de la lista de permitidos, establezca *encabezados* a una lista separada por comas de los encabezados permitidos:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

Sin embargo, los exploradores no son totalmente coherentes en forma de conjunto de Access-Control-Request-Headers. Por ejemplo, Chrome actualmente incluye "origin". FireFox no incluye encabezados estándar, como "Accept", incluso cuando la aplicación establece en la secuencia de comandos.

Si establece *encabezados* para que no sea "\*", debe incluir al menos "accept", "content-type" y "origen", además de los encabezados personalizados que desee admitir.

## <a name="set-the-allowed-response-headers"></a>Establecer los encabezados de respuesta permitidos

De forma predeterminada, el explorador no expone todos los encabezados de respuesta a la aplicación. Los encabezados de respuesta que están disponibles de forma predeterminada son:

- Control de caché
- Idioma del contenido
- Content-Type
- Expires
- Last-Modified
- pragma

La especificación CORS llama a estos [encabezados de respuesta simple](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header). Para que otros encabezados disponibles para la aplicación, establezca el *exposedHeaders* parámetro de **[EnableCors]**.

En del ejemplo siguiente, el controlador `Get` método establece un encabezado personalizado denominado "X-Custom-Header". De forma predeterminada, el explorador no expondrá este encabezado en una solicitud entre orígenes. Para que esté disponible el encabezado, incluir "X-Custom-Header" en *exposedHeaders*.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

## <a name="pass-credentials-in-cross-origin-requests"></a>Pase las credenciales en solicitudes entre orígenes

Las credenciales requieren un tratamiento especial en una solicitud de CORS. De forma predeterminada, el explorador no envía ninguna credencial con una solicitud entre orígenes. Las credenciales incluyen las cookies, así como los esquemas de autenticación HTTP. Para enviar las credenciales con una solicitud entre orígenes, el cliente debe establecer **XMLHttpRequest.withCredentials** en true.

Uso de **XMLHttpRequest** directamente:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

En jQuery:

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

Además, el servidor debe permitir las credenciales. Para permitir que las credenciales de origen cruzado en la API Web, establezca el **SupportsCredentials** propiedad en true en el **[EnableCors]** atributo:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

Si esta propiedad es true, la respuesta HTTP incluirá un encabezado de Access-Control-Allow-Credentials. Este encabezado indica al explorador que el servidor permite credenciales para una solicitud entre orígenes.

Si el explorador envía credenciales, pero la respuesta no incluye un encabezado válido de Access-Control-Allow-Credentials, el explorador no expondrá la respuesta a la aplicación y se produce un error en la solicitud de AJAX.

Tenga cuidado sobre la configuración de **SupportsCredentials** en true, ya que significa que un sitio Web en otro dominio puede enviar credenciales ha iniciado la sesión de un usuario a la API Web en el nombre de usuario, sin que el usuario que se va a tener en cuenta. La especificación CORS también establece ese valor *orígenes* a &quot; \* &quot; no es válido si **SupportsCredentials** es true.

## <a name="custom-cors-policy-providers"></a>Proveedores personalizados de directiva CORS

El **[EnableCors]** atributo implementa el **ICorsPolicyProvider** interfaz. Puede proporcionar su propia implementación mediante la creación de una clase que derive de **atributo** e implementa **ICorsProlicyProvider**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

Ahora puede aplicar el atributo de cualquier lugar que tendría que poner **[EnableCors]**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

Por ejemplo, un proveedor personalizado de directiva CORS pudo leer la configuración desde un archivo de configuración.

Como alternativa al uso de atributos, puede registrar un **ICorsPolicyProviderFactory** objeto que crea **ICorsPolicyProvider** objetos.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

Para establecer el **ICorsPolicyProviderFactory**, llame a la **SetCorsPolicyProviderFactory** método de extensión en el inicio, como se indica a continuación:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

## <a name="browser-support"></a>Compatibilidad con exploradores

El paquete de Web API CORS es una tecnología del lado servidor. El explorador del usuario también debe admitir la CORS. Afortunadamente, las versiones actuales de todos los exploradores principales incluyen [compatibilidad con CORS](http://caniuse.com/cors).
