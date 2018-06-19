---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: ASP.NET Web Pages (Razor) Guía de solución de problemas | Documentos de Microsoft
author: tfitzmac
description: Este artículo describen los problemas que podría tener al trabajar con ASP.NET Web Pages (Razor) y algunas soluciones sugeridas. Versiones de software de página Web de ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: ec51169ccea0016712de3fdb28a16a174150a8bd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30898519"
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a>Guía de solución de problemas (Razor) de ASP.NET Web Pages
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artículo describen los problemas que podría tener al trabajar con ASP.NET Web Pages (Razor) y algunas soluciones sugeridas.
> 
> ## <a name="software-versions"></a>Versiones de software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2 y páginas Web de ASP.NET 1.0.


Este tema contiene las siguientes secciones:

- [Problemas con la ejecución de páginas](#Issues_Running_.cshtml_Pages)
- [Problemas con el código Razor](#IssuesWithRazorCode)
- [Problemas con la seguridad y la pertenencia a](#membership)
- [Problemas con el envío de correo electrónico](#email)
- [Recursos adicionales](#AdditionalResources)

Para preguntas generales, vea [ASP.NET Web Pages (Razor) preguntas más frecuentes sobre](https://go.microsoft.com/fwlink/?LinkId=253000).

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a>Problemas con la ejecución de páginas

Puede evitar que una variedad de problemas *.cshtml* y *.vbhtml* páginas ejecuten correctamente. En esta sección se enumera mensajes de error comunes y causas probables.

### <a name="http-error-403---forbidden-access-is-denied"></a>HTTP Error 403 - Prohibido: Acceso denegado

*No tiene permiso para ver este directorio o esta página con las credenciales que ha proporcionado.*

Este error puede producirse si el servidor no está ejecutando la versión correcta de .NET Framework. Asegúrese de que el equipo que ejecuta el servidor (local o remotamente) tiene al menos .NET Framework 4 instalado. Además, asegúrese de que la propia aplicación se configura para ejecutar la versión correcta.

Si ve este problema localmente mientras trabaja en WebMatrix, haga clic en el **sitio** área de trabajo y en la vista de árbol, después, haga clic en **configuración**. En el **seleccione la versión de .NET Framework** seleccione **.NET 4 (integrado)**. Si ya se ha establecido esta versión, intente ejecutar WebMatrix como administrador.

Asegúrese de que la raíz del sitio Web tiene al menos un *.cshtml* archivos en ella.

Si ve este error cuando el servidor web está en un servidor remoto, póngase en contacto con el administrador del servidor. Asegúrese de que el servidor tenga .NET Framework 4 o posterior instalado. Además, asegúrese de que la aplicación se ejecuta en un grupo de aplicaciones está configurado para utilizar esa versión de.NET Framework.

Si tiene control sobre el servidor, asegúrese de que se está ejecutando la versión correcta de .NET Framework. También puede intentar reparar la instalación mediante la ejecución de la `aspnet_regiis -iru` comando. (Por ejemplo, si instaló IIS después de instalar .NET Framework, IIS no se configurarse correctamente para ejecutar las páginas ASP.NET.) Para obtener más información, consulte [herramienta de registro de IIS de ASP.NET (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).

### <a name="http-error-40314---forbidden"></a>Error de HTTP 403.14 - prohibido

*El servidor Web está configurado para no mostrar el contenido de este directorio.*

Este error puede producirse si se solicita un recurso protegido (como el *Web.config* archivo) o que se encuentra en una carpeta protegida (como *aplicación\_datos* o *aplicación\_Código*).

### <a name="http-error-40417---not-found"></a>No se encontró un Error HTTP 404.17-

*El contenido solicitado parece ser un script y el controlador de archivos estáticos no lo servirá.*

Este error puede producirse si el servidor no está configurado correctamente para que utilice .NET Framework 4 o posterior y, por tanto, no reconoce el código en `@{ }` bloques. Vea la descripción anterior de *HTTP Error 403 - Prohibido: acceso denegado*.

### <a name="http-error-4047---not-found"></a>No se encontró un Error HTTP 404.7:

*El módulo de filtrado de solicitudes está configurada para denegar la extensión de archivo*

Este error puede producirse si *.cshtml* o *.vbhtml* extensiones se ha bloqueado explícitamente en el servidor. Un síntoma de este problema es que funcionan las direcciones URL cuando no incluya la extensión, pero las direcciones URL que incluyen *.cshtml* o *.vbhtml* no funcionan. Una posible solución consiste en volver a habilitar las extensiones en el sitio *Web.config* archivo. En el ejemplo siguiente se muestra cómo habilitar la *.cshtml* extensión.

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a>No se encontró un Error HTTP 404.8:

*El módulo de filtrado de solicitudes está configurada para denegar una ruta de acceso en la dirección URL que contiene una sección hiddenSegment.*

Este error puede producirse si se solicita un recurso protegido (como el *Web.config* archivo) o que se encuentra en una carpeta protegida (como *aplicación\_datos* o *aplicación\_Código*).

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a>Este tipo de página no disponible (Error de servidor en la aplicación '/')

Vea la descripción anterior de 404.17 de Error de HTTP.

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a>Problemas con el código Razor

### <a name="the-name-class-does-not-exist-in-the-current-context"></a>El nombre '*clase*' no existe en el contexto actual

A menudo, una razón verá este error es que `class` referencias no se instala una aplicación auxiliar, pero la aplicación auxiliar. Por ejemplo, si intenta utilizar una aplicación auxiliar, pero si no ha instalado el paquete de NuGet, verá este error. Uso de la galería en WebMatrix para buscar e instalar la aplicación auxiliar.

Si se instala la aplicación auxiliar, pero la página aún no la reconoce, intente agregar agregar un `using` instrucción en el código. En el `using` instrucción, referencia de espacio de nombres que incluye la aplicación auxiliar. Por ejemplo, las aplicaciones auxiliares básicas que se encuentran en el paquete de aplicación auxiliar de ASP.NET Web están en el `System.Web.Helpers` espacio de nombres. En la parte superior de la página donde desea utilizar la aplicación auxiliar, agregue esta línea:

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a>Problemas con la seguridad y la pertenencia a

Si se usa el sistema de seguridad integrados (pertenencia) en ASP.NET Web Pages (Razor), pueden surgir los siguientes problemas.

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a>Para llamar a este método, la propiedad "Membership.Provider" debe ser una instancia de "ExtendedMembershipProvider"

Este error puede indicar que no hay `AspNetSqlMembershipProvider` clase se configura. (Un síntoma es que el sitio funciona bien localmente pero este error produce cuando se publica en el servidor de un proveedor de hospedaje). Una solución para este problema consiste en habilitar explícitamente la pertenencia sencillo debe agregar lo siguiente en el sitio *Web.config* archivo:

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a>Problemas con el envío de correo electrónico

Problemas con el envío de correo electrónico pueden resultar complicado para depurar. Un problema inicial puede ser que no se puede conectar al servidor SMTP. Si la conexión es correcta, ASP.NET entrega el mensaje al servidor SMTP. Sin embargo, puede haber problemas con el propio mensaje que impide que el servidor SMTP de enviarlo.

Si la aplicación no envía correo electrónico correctamente, intente lo siguiente:

- El nombre del servidor SMTP suele ser algo parecido a `smtp.provider.com` o `smtp.provider.net`. Sin embargo, si se publica un sitio en un proveedor de hospedaje, el nombre del servidor SMTP en ese momento podría ser `localhost`. Esta situación se produce porque una vez que ha publicado y el sitio se ejecuta en el servidor del proveedor, el servidor SMTP puede ser local desde la perspectiva de la aplicación. Este cambio en los nombres de servidor, podría significar que tiene que cambiar el nombre del servidor SMTP como parte del proceso de publicación.
- Normalmente, el número de puerto es 25. Sin embargo, algunos proveedores requieren que se va a utilizar el puerto 587 o algún otro puerto. Consulte con el propietario del servidor SMTP qué número de puerto que esperan utilizar.
- Asegúrese de que utiliza las credenciales correctas. Si ha publicado su sitio en un proveedor de hospedaje, utilice las credenciales que el proveedor ha indicado específicamente son para correo electrónico. Estas credenciales podrían ser diferentes de las credenciales que se usa para publicar.
- En ocasiones, no necesita credenciales en absoluto. Si va a enviar correo electrónico mediante el uso de su ISP personal, el proveedor de correo electrónico ya quizá sepa que sus credenciales. Después de publicar, debe utilizar credenciales diferentes a cuando se prueba en el equipo local.
- Si su proveedor de correo electrónico usa cifrado, establezca `WebMail.EnableSsl` a `true`.

Si hay un error al enviar correo electrónico, verá un mensaje de error ASP.NET estándar, que es similar a esto:

![Mensaje de error ASP.NET cuando hay un problema con el correo electrónico](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

También es posible depurar problemas con el envío de correo electrónico mediante el uso de un `try-catch` bloque, como en el ejemplo siguiente. Cuando se usa un `try-catch` bloque, ASP.NET no muestra los mensajes de error estándar. En su lugar, puede capturar el error en la `catch` parte del bloque.

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

Sustituya los valores adecuados para `your-SMTP-server-name`, y así sucesivamente. Algunos de los mensajes de error, es posible que vea este modo incluyen lo siguiente:

- *Error al enviar correo.*

    O bien

    *Un intento de conexión no se pudo porque la parte conectada no respondió adecuadamente tras un período de tiempo o conexión establecida no se pudo porque el host conectado no respondió*

    Este error normalmente significa que la aplicación no se pudo conectar al servidor SMTP. Compruebe el nombre del servidor y número de puerto.
- <em>Buzón no disponible. La respuesta del servidor fue: 5.1.0 &lt; someuser@invaliddomain &gt; remitente rechazado: dominio del remitente no válido</em>

    Este mensaje puede indicar que el `From` dirección no es correcta o falta.
- *La cadena especificada no está en la forma necesaria para una dirección de correo electrónico.*

    Este error puede indicar que el valor de la `To` o `From` propiedades no se reconocen como direcciones de correo electrónico. (ASP.NET no puede comprobar que la dirección de correo electrónico es válida, solo 's en el formato correcto, como *name@domain.com*.)

> [!NOTE]
> Quite el código que muestra el error (`@errorMessage`) antes de publicar la página en un sitio en vivo. No es una buena idea de que los usuarios puedan ver los mensajes de error que se obtengan de un servidor.


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Recursos adicionales

[Preguntas frecuentes de ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253000)

[WebMatrix y ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) foro en el sitio Web ASP.NET
