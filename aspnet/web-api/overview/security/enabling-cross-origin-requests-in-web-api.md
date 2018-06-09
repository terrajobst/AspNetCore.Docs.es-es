---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Habilitar las solicitudes entre orígenes en ASP.NET Web API 2 | Documentos de Microsoft
author: MikeWasson
description: Muestra cómo admitir el uso compartido de recursos entre orígenes (CORS) en ASP.NET Web API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/15/2014
ms.topic: article
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 453ad29ff4f10f9660f3aa8bab358519b4cfd48b
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "26508384"
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a>Habilitar las solicitudes entre orígenes en ASP.NET Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

> Seguridad del explorador impide que una página web que realiza las solicitudes AJAX a otro dominio. Esta restricción se denomina la *directiva de mismo origen*y se impide que un sitio malintencionado pueda leer los datos confidenciales desde otro sitio. Sin embargo, en ocasiones, puede permitir que otros sitios a llame a la API web.
> 
> [Entre el uso compartido de recursos de origen](http://www.w3.org/TR/cors/) (CORS) es un estándar de W3C que permite que un servidor relajar la directiva de mismo origen. Con CORS, un servidor puede permitir explícitamente algunas solicitudes entre orígenes y rechazar los otros usuarios. CORS es más seguro y más flexible que técnicas anteriores como [JSONP](http://en.wikipedia.org/wiki/JSONP). Este tutorial muestra cómo habilitar CORS en la aplicación de API Web.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - [Visual Studio 2013 Update 2](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - 2.2 API Web


<a id="intro"></a>
## <a name="introduction"></a>Introducción

Este tutorial se muestra la compatibilidad con CORS en ASP.NET Web API. Comenzaremos mediante la creación de dos proyectos ASP.NET: una llamada "WebService", que hospeda un controlador de API Web, y el otra llamada "WebClient", que llama el servicio Web. Dado que las dos aplicaciones se hospedan en dominios diferentes, una solicitud de AJAX de WebClient al servicio Web es una solicitud entre orígenes.

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a>¿Qué es el "Mismo origen"?

Dos direcciones URL tienen el mismo origen si tienen puertos, los hosts y los esquemas idénticos. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Estas dos direcciones URL tienen el mismo origen:

- `http://example.com/foo.html`
- `http://example.com/bar.html`

Estas direcciones URL tengan diferentes orígenes que el anterior dos:

- `http://example.net` -Dominio diferente
- `http://example.com:9000/foo.html` -Otro puerto
- `https://example.com/foo.html` -Esquema diferente
- `http://www.example.com/foo.html` -Otro subdominio

> [!NOTE]
> Internet Explorer no tiene en cuenta el puerto al comparar elementos Origin.


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a>Crear el proyecto de servicio Web

> [!NOTE]
> En esta sección se da por supuesto que ya sabe cómo crear proyectos Web API. Si no es así, consulte [Introducción a ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).


Inicie Visual Studio y cree un nuevo **aplicación Web ASP.NET** proyecto. Seleccione el **vacía** plantilla de proyecto. En "Agregar carpetas y principales referencias de", seleccione la **API Web** casilla de verificación. Si lo desea, seleccione la opción de "Host en la nube" para implementar la aplicación en Microsoft Azure. Microsoft ofrece el hospedaje web gratuito para hasta 10 sitios Web en un [libre de la cuenta de prueba de Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

Agregar un controlador de API Web denominado `TestController` con el código siguiente:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

Puede ejecutar la aplicación localmente o implementar en Azure. (Para las capturas de pantalla en este tutorial, implementa para aplicaciones de Web del servicio de aplicación de Azure.) Para comprobar que funciona la API web, vaya a `http://hostname/api/test/`, donde *hostname* es el dominio donde se implementa la aplicación. Debería ver el texto de respuesta, &quot;obtener: mensaje de prueba&quot;.

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a>Crear el proyecto de WebClient

Cree otro proyecto de aplicación Web ASP.NET y seleccione el **MVC** plantilla de proyecto. Si lo desea, seleccione **Cambiar autenticación** > **sin autenticación**. No necesita autenticación para este tutorial.

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

En el Explorador de soluciones, abra el archivo Views/Home/Index.cshtml. Reemplace el código de este archivo con lo siguiente:

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

Para el *serviceUrl* variable, utilice el URI de la aplicación de servicio Web. Ahora ejecuta la aplicación de WebClient localmente o publicarlo en otro sitio Web.

Haga clic en el botón "Pruébelo" envía una solicitud AJAX a la aplicación de servicio Web mediante el método HTTP que se muestran en el cuadro de lista desplegable (GET, POST o PUT). Esto nos permite examinar las solicitudes entre orígenes diferentes. En este momento, la aplicación de servicio Web no admiten CORS, por lo que si hace clic en el botón, se producirá un error.

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> Si ve el tráfico HTTP en una herramienta como [Fiddler](http://www.telerik.com/fiddler), verá que el explorador envía la solicitud GET y la solicitud se realiza correctamente, pero la llamada de AJAX devuelve un error. Es importante comprender que la directiva de mismo origen no evita que el Explorador de *enviar* la solicitud. En su lugar, se evita que la aplicación vean el *respuesta*.


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a>Habilitar CORS

Ahora vamos a habilitar CORS en la aplicación de servicio Web. En primer lugar, agregue el paquete NuGet de CORS. En Visual Studio, desde la **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**. En la ventana de la consola de administrador de paquetes, escriba el siguiente comando:

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

Este comando instala el paquete más reciente y actualiza todas las dependencias, incluidas las bibliotecas de API Web principal. Usuario-marca de versión para dirigirse a una versión específica. El paquete CORS requiere Web API 2.0 o posterior.

Abra el archivo de aplicación\_Start/WebApiConfig.cs. Agregue el código siguiente a la **WebApiConfig.Register** método.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

A continuación, agregue el **[EnableCors]** atribuir a la `TestController` clase:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

Para el *orígenes* parámetro, use el URI en la que implementó la aplicación WebClient. Esto permite que las solicitudes entre orígenes de WebClient, mientras todavía no se permiten todas las demás solicitudes entre dominios. Más adelante, voy a describir los parámetros de **[EnableCors]** con más detalle.

No incluya una barra diagonal al final de la *orígenes* dirección URL.

Volver a implementar la aplicación de servicio Web actualizada. No es necesario actualizar WebClient. Ahora debe ejecutarse correctamente la solicitud de AJAX de WebClient. Todos los métodos GET, PUT y POST se permiten.

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
## <a name="how-cors-works"></a>Funcionamiento de CORS

En esta sección se describe lo que sucede en una solicitud CORS, en el nivel de los mensajes HTTP. Es importante comprender el funcionamiento de CORS, por lo que puede configurar el **[EnableCors]** atributo correctamente y solucionar los problemas si lo no funciona como esperaba.

La especificación de CORS presenta varios encabezados HTTP nuevo que permiten las solicitudes entre orígenes. Si un explorador es compatible con CORS, establece estos encabezados automáticamente para las solicitudes entre orígenes; no es necesario hacer nada especial en el código de JavaScript.

Este es un ejemplo de una solicitud entre orígenes. El encabezado de "Origen" proporciona el dominio del sitio que está realizando la solicitud.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

Si el servidor permite la solicitud, Establece el encabezado de acceso Access-Control-Allow-Origin. El valor de este encabezado coincide con el encabezado de origen, o es el valor de carácter comodín "\*", lo que significa que se permite cualquier origen.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

Si la respuesta no incluye el encabezado de acceso Access-Control-Allow-Origin, se produce un error en la solicitud de AJAX. En concreto, el explorador no permite la solicitud. Incluso si el servidor devuelve una respuesta correcta, el explorador no disponer de la respuesta a la aplicación cliente.

**Solicitudes preparatorias**

Para algunas solicitudes CORS, el explorador envía una solicitud adicional, denominada "solicitud preparatoria," antes de enviar la solicitud real para el recurso.

El explorador puede omitir la solicitud preparatoria si se cumplen las condiciones siguientes:

- El método de solicitud es GET, HEAD o POST, *y*
- La aplicación no establece los encabezados de solicitud que no sean Accept, Accept-Language, Content-Language, Content-Type o último--Id. de evento, *y*
- El encabezado Content-Type (si establece) es uno de los siguientes: 

    - application/x-www-form-urlencoded
    - varias partes/de datos de formulario
    - texto/sin formato

La regla acerca de los encabezados de solicitud se aplica a los encabezados de la aplicación se establece mediante una llamada a **setRequestHeader** en el **XMLHttpRequest** objeto. (La especificación de CORS llama a estos "encabezados de solicitud de autor"). La regla no se aplica a los encabezados de la *explorador* pueden establecer, como el agente de usuario, el Host o Content-Length.

Este es un ejemplo de una solicitud preparatoria:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

La solicitud preparatoria utiliza el método HTTP OPTIONS. Incluye dos encabezados especiales:

- Acceso Access-Control-Request-Method: El método HTTP que se usará para la solicitud real.
- Acceso Access-Control-Request-Headers: Una lista de encabezados de solicitud que el *aplicación* establecido en la solicitud real. (De nuevo, esto no incluye encabezados que establece el explorador.)

Esta es una respuesta de ejemplo, suponiendo que el servidor permite que la solicitud:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

La respuesta incluye un encabezado de acceso-Control-Allow-Methods que enumera los métodos permitidos y, opcionalmente, un encabezado Access-Control-permitir-Headers, que se muestra los encabezados permitidos. Si la solicitud preparatoria se realiza correctamente, el explorador envía la solicitud real, como se describió anteriormente.

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a>Reglas de ámbito para [EnableCors]

Puede habilitar CORS por cada acción, por cada controlador o globalmente para todos los controladores de API Web de la aplicación.

**Por cada acción**

Para habilitar CORS para una única acción, establezca el **[EnableCors]** atributo en el método de acción. En el ejemplo siguiente se habilita CORS para el `GetItem` método únicamente.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

**Por cada controlador**

Si establece **[EnableCors]** en la clase de controlador, se aplica a todas las acciones en el controlador. Para deshabilitar CORS para una acción, agregue el **[DisableCors]** atribuir a la acción. En el ejemplo siguiente se habilita CORS para cada método excepto `PutItem`.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

**Global**

Para habilitar CORS para todos los controladores de API Web en la aplicación, pase una **EnableCorsAttribute** de instancia para la **EnableCors** método:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

Si establece el atributo en más de un ámbito, el orden de prioridad es:

1. Acción
2. Controlador
3. Global

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a>Establecer los orígenes permitidos

El *orígenes* parámetro de la **[EnableCors]** atributo especifica qué orígenes tendrán permiso para tener acceso al recurso. El valor es una lista separada por comas de los orígenes permitidos.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

También puede utilizar el valor de carácter comodín "\*" para permitir las solicitudes de los orígenes.

Debe considerar detenidamente antes de permitir que las solicitudes de cualquier origen. Significa que literalmente cualquier sitio Web puede realizar llamadas de AJAX a la API web.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a>Establecer los métodos HTTP permitidos

El *métodos* parámetro de la **[EnableCors]** atributo especifica qué métodos HTTP se permiten tener acceso al recurso. Para permitir que todos los métodos, utilice el valor de carácter comodín "\*". En el ejemplo siguiente, se permite solamente las solicitudes GET y POST.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a>Establecer los encabezados de solicitudes permitido

Anteriormente describe cómo una solicitud preparatoria podría incluir un encabezado de acceso-Access-Control-Request-Headers, enumerar los encabezados HTTP establecidos por la aplicación (la "crear encabezados de solicitud"). El *encabezados* parámetro de la **[EnableCors]** atributo especifica que se permiten los encabezados de solicitud de autor. Para permitir que los encabezados, establezca *encabezados* a "\*". A encabezados específicos de la lista de permitidos, establezca *encabezados* a una lista separada por comas de los encabezados permitidos:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

Sin embargo, los exploradores no son completamente coherentes en forma de conjunto de acceso-Access-Control-Request-Headers. Por ejemplo, Chrome incluye actualmente "origen"; mientras FireFox no incluye encabezados estándar como "Accept", incluso cuando la aplicación los establece en el script.

Si establece *encabezados* en ningún otro valor distinto de "\*", debe incluir al menos "Aceptar", "content-type" y "origen", además de los encabezados personalizados que desee admitir.

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a>Establecer los encabezados de respuesta permitido

De forma predeterminada, el explorador no expone todos los encabezados de respuesta a la aplicación. Los encabezados de respuesta que están disponibles de forma predeterminada son:

- Control de caché
- Content-Language
- Tipo de contenido
- Expira
- Última modificación
- Pragma

La especificación CORS llama a estos [encabezados de respuesta simple](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header). Para hacer que otros encabezados disponibles para la aplicación, establezca el *exposedHeaders* parámetro de **[EnableCors]**.

En del ejemplo siguiente, el controlador `Get` método establece un encabezado personalizado denominado 'X-Custom-Header'. De forma predeterminada, el explorador no expondrá este encabezado en una solicitud entre orígenes. Para que esté disponible el encabezado, incluir 'X-Custom-Header' en *exposedHeaders*.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a>Transferencia de credenciales en solicitudes Cross-Origin

Las credenciales requieren un tratamiento especial en una solicitud de CORS. De forma predeterminada, el explorador no envía ninguna credencial con una solicitud entre orígenes. Las credenciales son las cookies, así como esquemas de autenticación HTTP. Para enviar las credenciales con una solicitud entre orígenes, el cliente debe establecer **XMLHttpRequest.withCredentials** en true.

Usar **XMLHttpRequest** directamente:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

En jQuery:

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

Además, el servidor debe permitir las credenciales. Para permitir las credenciales entre orígenes de API Web, establezca el **SupportsCredentials** propiedad en true en la **[EnableCors]** atributo:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

Si esta propiedad es true, la respuesta HTTP incluirá un encabezado de acceso-Control-Allow-Credentials. Este encabezado indica al explorador que el servidor permite que las credenciales para una solicitud entre orígenes.

Si el explorador envía las credenciales, pero la respuesta no incluye un encabezado de acceso-Control-Allow-Credentials válido, el explorador no expondrá la respuesta a la aplicación y se produce un error en la solicitud de AJAX.

Tenga mucho cuidado sobre la configuración de **SupportsCredentials** en true, ya que significa que un sitio Web en otro dominio puede enviar credenciales ha iniciado la sesión de un usuario a su API Web en nombre del usuario, sin el conocimiento del usuario. La especificación CORS dice también ese valor *orígenes* a &quot; \* &quot; no es válido si **SupportsCredentials** es true.

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a>Proveedores de directiva personalizada de CORS

El **[EnableCors]** atributo implementa la **ICorsPolicyProvider** interfaz. Puede proporcionar su propia implementación mediante la creación de una clase que deriva de **atributo** e implementa **ICorsProlicyProvider**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

Ahora puede aplicar el atributo de cualquier lugar en el que aplicaría **[EnableCors]**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

Por ejemplo, un proveedor de directivas personalizado de CORS pudo leer la configuración de un archivo de configuración.

Como alternativa al uso de atributos, puede registrar un **ICorsPolicyProviderFactory** objeto que crea **ICorsPolicyProvider** objetos.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

Para establecer el **ICorsPolicyProviderFactory**, llame a la **SetCorsPolicyProviderFactory** método de extensión en el inicio, como se indica a continuación:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a>Compatibilidad de exploradores

El paquete de Web API CORS es una tecnología de servidor. El explorador del usuario también debe admitir CORS. Afortunadamente, se incluyen las versiones actuales de los principales exploradores [compatibilidad con CORS](http://caniuse.com/cors).

Internet Explorer 8 e Internet Explorer 9 tienen compatibilidad parcial para CORS, use el objeto heredado de XDomainRequest en lugar de XMLHttpRequest. Para obtener más información, consulte [XDomainRequest - soluciones alternativas, limitaciones y restricciones](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).
