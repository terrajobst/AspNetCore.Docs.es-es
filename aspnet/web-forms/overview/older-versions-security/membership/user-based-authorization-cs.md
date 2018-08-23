---
uid: web-forms/overview/older-versions-security/membership/user-based-authorization-cs
title: Autorización basada en usuario (C#) | Microsoft Docs
author: rick-anderson
description: En este tutorial veremos limitar el acceso a las páginas y restringir la funcionalidad de nivel de página a través de una variedad de técnicas.
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 3c815a9e-2296-4b9b-b945-776d54989daa
msc.legacyurl: /web-forms/overview/older-versions-security/membership/user-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: 18aad3bde961747af64de2b76ff83feb767597d9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836123"
---
<a name="user-based-authorization-c"></a>Autorización basada en usuario (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_07_CS.zip) o [descargar PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial07_UserAuth_cs.pdf)

> En este tutorial veremos limitar el acceso a las páginas y restringir la funcionalidad de nivel de página a través de una variedad de técnicas.


## <a name="introduction"></a>Introducción

La mayoría de las aplicaciones web que ofrecen las cuentas de usuario hacerlo en parte para restringir ciertos visitantes tengan acceso a determinadas páginas del sitio. En los sitios de panel de mensajes más en línea, por ejemplo, todos los usuarios - autenticados y anónimos - están posible ver entradas del panel de mensajes, pero sólo los usuarios autenticados pueden visitar la página web para crear una nueva publicación. Y puede haber páginas administrativas que solo son accesibles para un usuario determinado (o un conjunto determinado de usuarios). Además, la funcionalidad de nivel de página puede diferir según el usuario por el usuario. Al ver una lista de publicaciones, los usuarios autenticados se muestran una interfaz para la evaluación de cada publicación, mientras que esta interfaz no está disponible para los visitantes anónimos.

ASP.NET facilita definir las reglas de autorización basada en usuario. Con un poco de marcado en `Web.config`, páginas web específicas o directorios completos pueden bloquearse para que solo sean accesibles a un subconjunto específico de usuarios. Funcionalidad de nivel de página puede se puede activar o desactivar según el usuario actualmente conectado a través de medios declarativos y de programación.

En este tutorial veremos limitar el acceso a las páginas y restringir la funcionalidad de nivel de página a través de una variedad de técnicas. Comencemos.

## <a name="a-look-at-the-url-authorization-workflow"></a>Un vistazo al flujo de trabajo de autorización de URL

Como se describe en el [ *una visión general de autenticación mediante formularios* ](../introduction/an-overview-of-forms-authentication-cs.md) tutorial, cuando el tiempo de ejecución ASP.NET procesa una solicitud para un recurso ASP.NET la solicitud genera un número de eventos durante su ciclo de vida. *Los módulos HTTP* son clases administradas cuyo código se ejecuta en respuesta a un evento determinado en el ciclo de vida de la solicitud. ASP.NET incluye una serie de módulos HTTP que realizan tareas esenciales en segundo plano.

Es uno de esos módulos HTTP [ `FormsAuthenticationModule` ](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx). Como se describe en los tutoriales anteriores, la función principal de la `FormsAuthenticationModule` consiste en determinar la identidad de la solicitud actual. Esto se consigue mediante la inspección el vale de autenticación de formularios, que se encuentra en una cookie o incrustado en la dirección URL. Esta identificación tiene lugar durante la [ `AuthenticateRequest` eventos](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Otro módulo HTTP importante es la [ `UrlAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx), que se produce en respuesta a la [ `AuthorizeRequest` eventos](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx) (lo que sucede después de la `AuthenticateRequest` eventos). El `UrlAuthorizationModule` examina el marcado de la configuración en `Web.config` para determinar si la identidad actual tiene autoridad para visitar la página especificada. Este proceso se conoce como *autorización de URL*.

Examinaremos la sintaxis de las reglas de autorización de dirección URL en el paso 1, pero primero Echemos un vistazo a lo que el `UrlAuthorizationModule` no hace según si la solicitud está autorizada o no. Si el `UrlAuthorizationModule` determina que la solicitud está autorizada, entonces no hace nada y la solicitud continúa a través de su ciclo de vida. Sin embargo, si la solicitud es *no* autorizado, el `UrlAuthorizationModule` anula el ciclo de vida e indica a la `Response` objeto para devolver un [HTTP 401 no autorizado](http://www.checkupdown.com/status/E401.html) estado. Cuando se usa la autenticación de formularios este estado HTTP 401 nunca se devuelve al cliente porque si el `FormsAuthenticationModule` detecta es el estado HTTP 401 que modifica una [redirección de HTTP 302](http://www.checkupdown.com/status/E302.html) a la página de inicio de sesión.

Figura 1 ilustra el flujo de trabajo de la canalización ASP.NET, el `FormsAuthenticationModule`y el `UrlAuthorizationModule` cuando llega una solicitud no autorizada. En concreto, la figura 1 muestra una solicitud para un visitante de anónimo `ProtectedPage.aspx`, que es una página que se deniega el acceso a los usuarios anónimos. Puesto que el visitante es anónimo, el `UrlAuthorizationModule` anula la solicitud y devuelve un estado HTTP 401 no autorizado. El `FormsAuthenticationModule` , a continuación, convierte el estado 401 en una redirección 302 en la página de inicio de sesión. Una vez autenticado el usuario a través de la página de inicio de sesión, se le redirige a `ProtectedPage.aspx`. Esta vez el `FormsAuthenticationModule` identifica al usuario según su vale de autenticación. Ahora que el visitante se autentica, el `UrlAuthorizationModule` permite el acceso a la página.


[![La autenticación de formularios y el flujo de trabajo de autorización de URL](user-based-authorization-cs/_static/image2.png)](user-based-authorization-cs/_static/image1.png)

**Figura 1**: la autenticación de formularios y flujo de trabajo de autorización de dirección URL ([haga clic aquí para ver imagen en tamaño completo](user-based-authorization-cs/_static/image3.png))


Figura 1 se muestra la interacción que tiene lugar cuando un visitante anónimo intenta obtener acceso a un recurso que no está disponible para los usuarios anónimos. En tal caso, el visitante anónimo se redirige a la página de inicio de sesión con la página que intentó visite especificado en la cadena de consulta. Una vez que el usuario ha iniciado sesión correctamente en, ella le redirigirá automáticamente a los recursos que inicialmente estaba intentando ver.

Cuando se realiza la solicitud no autorizada por un usuario anónimo, este flujo de trabajo es sencillo y fácil para el visitante comprender lo que ha sucedido y por qué. Pero tenga en cuenta que el `FormsAuthenticationModule` redirigirá *cualquier* no autorizado a la página de inicio de sesión, usuario, incluso si la solicitud se realiza por un usuario autenticado. Si un usuario autenticado intenta visitar una página que no tiene autoridad para el que esto puede producir en una experiencia de usuario confusa.

Imagine que nuestro sitio Web tiene sus reglas de autorización de dirección URL configuradas tal que la página ASP.NET `OnlyTito.aspx` era accesible sólo a Tito. Ahora, imagine que Sam visita el sitio, inicie sesión y, a continuación, intenta visitar `OnlyTito.aspx`. El `UrlAuthorizationModule` detendrá el ciclo de vida de la solicitud y devolver un estado HTTP 401 no autorizado, que el `FormsAuthenticationModule` detectará y, a continuación, redirigir Sam a la página de inicio de sesión. Puesto que Sam ya ha iniciado sesión, no obstante, quizás se pregunte por qué ha enviado a la página de inicio de sesión. Podría razón para que sus credenciales de inicio de sesión se han perdido alguna manera, o que escrito credenciales no válidas. Si Sam llegar sus credenciales de la página de inicio de sesión se registrará (nuevo) y redirige a `OnlyTito.aspx`. El `UrlAuthorizationModule` detectará que Sam no puede visitar esta página y se devolverá a la página de inicio de sesión.

Figura 2 se ilustra este flujo de trabajo confuso.


[![El flujo de trabajo predeterminada puede dar lugar a un ciclo confuso](user-based-authorization-cs/_static/image5.png)](user-based-authorization-cs/_static/image4.png)

**Figura 2**: The Default flujo de trabajo puede dar lugar a un ciclo confuso ([haga clic aquí para ver imagen en tamaño completo](user-based-authorization-cs/_static/image6.png))


El flujo de trabajo que se muestra en la figura 2 puede befuddle rápidamente incluso la mayoría equipo más experimentados visitante. Veremos cómo evitarlo confuso ciclo en el paso 2.

> [!NOTE]
> ASP.NET usa dos mecanismos para determinar si el usuario actual puede tener acceso a una página web determinada: autorización de dirección URL y de archivo. Autorización de archivos se implementa mediante el [ `FileAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx), que determina la entidad consultando la ACL de archivos solicitados. Autorización de archivo se utiliza con más frecuencia con la autenticación de Windows porque las ACL son los permisos que se aplican a las cuentas de Windows. Cuando se usa la autenticación de formularios, la misma cuenta de Windows, independientemente del usuario que visite el sitio ejecuta todas las solicitudes de nivel de sistema operativo system - y archivo. Puesto que esta serie de tutoriales se centra en la autenticación de formularios, no analizaremos la autorización de archivos.


### <a name="the-scope-of-url-authorization"></a>El ámbito de autorización de URL

El `UrlAuthorizationModule` es código administrado que forma parte de la ejecución de ASP.NET. Antes de la versión 7 de Microsoft [Internet Information Services (IIS)](https://www.iis.net/) servidor web, se ha producido una barrera distinta entre la canalización HTTP de IIS y la canalización de la ejecución de ASP.NET. En resumen, en IIS 6 y versiones anteriores, ASP. La red `UrlAuthorizationModule` solo se ejecuta cuando se delega una solicitud de IIS para el tiempo de ejecución ASP.NET. De forma predeterminada, IIS procesa el contenido estático Sí -, como páginas HTML y CSS, JavaScript y archivos de imagen - y entrega solamente las solicitudes para el tiempo de ejecución ASP.NET cuando una página con la extensión `.aspx`, `.asmx`, o `.ashx` se solicita.

IIS 7, sin embargo, permite integrada IIS y ASP.NET canalizaciones. Con algunos valores de configuración, puede configurar IIS 7 para invocar el `UrlAuthorizationModule` para *todas* solicitudes, lo que significa que se pueden definir las reglas de autorización de dirección URL para los archivos de cualquier tipo. Además, IIS 7 incluye su propio motor de autorización de dirección URL. Para obtener más información sobre la integración de ASP.NET y la funcionalidad de autorización de URL nativa de IIS 7, consulte [autorización de URL de IIS7 descripción](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization). Para una información más detallada sobre la integración de ASP.NET e IIS 7, adquiera un ejemplar del libro de Shahram Khosravi, *Professional IIS 7 y ASP.NET Programming integrado* (ISBN: 978 0470152539).

En pocas palabras, en las versiones anteriores de IIS 7, solo se aplican las reglas de autorización de dirección URL a los recursos que controla el tiempo de ejecución ASP.NET. Pero con IIS 7 se pueden usar la característica de autorización de URL nativo de IIS o integrar ASP. La red `UrlAuthorizationModule` en la canalización HTTP de IIS, lo cual amplía esta funcionalidad en todas las solicitudes.

> [!NOTE]
> Hay algunas diferencias sutiles, aunque importantes en ASP. La red `UrlAuthorizationModule` y la característica de autorización de URL de IIS 7 procesar las reglas de autorización. En este tutorial no examina la funcionalidad de autorización de dirección URL de IIS 7 o las diferencias en cómo los analiza las reglas de autorización en comparación con el `UrlAuthorizationModule`. Para obtener más información sobre estos temas, consulte la documentación de IIS 7 en MSDN o en [www.iis.net](https://www.iis.net/).


## <a name="step-1-defining-url-authorization-rules-inwebconfig"></a>Paso 1: Definir las reglas de autorización de dirección URL en`Web.config`

El `UrlAuthorizationModule` determina si se debe conceder o denegar el acceso a un recurso solicitado para una identidad concreta según las reglas de autorización de URL definidas en la configuración de la aplicación. Las reglas de autorización se establece en el [ `<authorization>` elemento](https://msdn.microsoft.com/library/8d82143t.aspx) en forma de `<allow>` y `<deny>` elementos secundarios. Cada `<allow>` y `<deny>` puede especificar el elemento secundario:

- Un usuario determinado
- Una lista delimitada por comas de los usuarios
- Todos los usuarios anónimos, indicados por un signo de interrogación (?)
- Todos los usuarios, indicados por un asterisco (\*)

El marcado siguiente muestra cómo usar las reglas de autorización de dirección URL para permitir que a los usuarios Tito y Scott y denegar a todos los demás:

[!code-xml[Main](user-based-authorization-cs/samples/sample1.xml)]

El `<allow>` elemento define qué usuarios tienen permiso - Tito y Scott - mientras el `<deny>` elemento indica que *todas* se deniegan a los usuarios.

> [!NOTE]
> El `<allow>` y `<deny>` elementos también pueden especificar las reglas de autorización para los roles. Examinaremos la autorización basada en roles en un futuro tutorial.


La siguiente configuración concede acceso a nadie que no sea de Sam (incluidos los visitantes anónimos):

[!code-xml[Main](user-based-authorization-cs/samples/sample2.xml)]

Para permitir que solo los usuarios autenticados, utilice la siguiente configuración, que se deniega el acceso a todos los usuarios anónimos:

[!code-xml[Main](user-based-authorization-cs/samples/sample3.xml)]

Las reglas de autorización se definen dentro de la `<system.web>` elemento `Web.config` y se aplican a todos los recursos de la aplicación web ASP.NET. A menudo, una aplicación tiene distintas reglas de autorización para las diferentes secciones. Por ejemplo, en un sitio de comercio electrónico, todos los visitantes pueden leer detenidamente los productos, consulte reseñas de productos, busque en el catálogo y así sucesivamente. Sin embargo, solo los usuarios autenticados pueden llegar a la desprotección o las páginas que se va a administrar el historial de trasvase de registros de uno. Además, puede haber algunas partes del sitio que sólo son accesibles para seleccionar usuarios, por ejemplo, los administradores del sitio.

ASP.NET facilita definir las reglas de autorización diferentes para distintos archivos y carpetas en el sitio. Las reglas de autorización especificadas en la carpeta raíz `Web.config` archivo se aplican a todos los recursos ASP.NET en el sitio. Sin embargo, se puede invalidar esta configuración de autorización predeterminado para una carpeta concreta mediante la adición de un `Web.config` con un `<authorization>` sección.

Vamos a actualizar nuestro sitio Web para que solo los usuarios autenticados pueden visitar las páginas ASP.NET en el `Membership` carpeta. Para lograr esto es necesario agregar un `Web.config` del archivo a la `Membership` carpeta y establecer su configuración de autorización para denegar a los usuarios anónimos. Haga clic en el `Membership` carpeta en el Explorador de soluciones, elija el menú Agregar nuevo elemento en el menú contextual y agregue un nuevo archivo de configuración Web denominado `Web.config`.


[![Agregue un archivo Web.config en la carpeta de pertenencia](user-based-authorization-cs/_static/image8.png)](user-based-authorization-cs/_static/image7.png)

**Figura 3**: agregar un `Web.config` del archivo a la `Membership` carpeta ([haga clic aquí para ver imagen en tamaño completo](user-based-authorization-cs/_static/image9.png))


En este momento, el proyecto debe contener dos `Web.config` archivos: uno en el directorio raíz y uno en el `Membership` carpeta.


[![La aplicación ahora debe contener dos archivos Web.config](user-based-authorization-cs/_static/image11.png)](user-based-authorization-cs/_static/image10.png)

**Figura 4**: su aplicación debería ahora contienen dos `Web.config` archivos ([haga clic aquí para ver imagen en tamaño completo](user-based-authorization-cs/_static/image12.png))


Actualizar el archivo de configuración en el `Membership` carpeta, por lo que TI prohíbe el acceso a los usuarios anónimos.

[!code-xml[Main](user-based-authorization-cs/samples/sample4.xml)]

Eso es todo esto!

Para probar este cambio, visite la página principal en un explorador y asegúrese de que se cerró. Puesto que es el comportamiento predeterminado de una aplicación ASP.NET permitir que todos los visitantes, y puesto que no realizamos ninguna modificación de la autorización en el directorio raíz `Web.config` archivo, somos capaces de visitar los archivos en el directorio raíz como un visitante anónimo.

Haga clic en el vínculo de creación de cuentas de usuario que se encuentra en la columna izquierda. Esto le llevará a la `~/Membership/CreatingUserAccounts.aspx`. Puesto que la `Web.config` de archivos en el `Membership` carpeta define las reglas de autorización para prohibir el acceso anónimo, el `UrlAuthorizationModule` anula la solicitud y devuelve un estado HTTP 401 no autorizado. El `FormsAuthenticationModule` modifica esto a un estado de redireccionamiento 302, enviándonos a la página de inicio de sesión. Tenga en cuenta que la página estábamos intentando obtener acceso a (`CreatingUserAccounts.aspx`) se pasa a la página de inicio de sesión a través de la `ReturnUrl` parámetro querystring.


[![Desde la dirección URL de autorización reglas prohibir el acceso anónimo, nos estamos redirigirá a la página de inicio de sesión](user-based-authorization-cs/_static/image14.png)](user-based-authorization-cs/_static/image13.png)

**Figura 5**: puesto que la autorización de dirección URL de reglas de prohibir el acceso anónimo, nos estamos redirigirá a la página de inicio de sesión ([haga clic aquí para ver imagen en tamaño completo](user-based-authorization-cs/_static/image15.png))


Tras iniciar sesión correctamente, estamos redirige a la `CreatingUserAccounts.aspx` página. Esta vez el `UrlAuthorizationModule` permite el acceso a la página porque ya no son anónimas.

### <a name="applying-url-authorization-rules-to-a-specific-location"></a>Aplicar reglas de autorización de dirección URL a una ubicación específica

La configuración de autorización definida en el `<system.web>` sección de `Web.config` se aplican a todos los recursos ASP.NET en ese directorio y sus subdirectorios (hasta que lo contrario, se reemplaza por otro `Web.config` archivo). En algunos casos, sin embargo, es posible que queramos todos los recursos ASP.NET en un directorio determinado para tener una configuración de autorización determinada excepto uno o dos páginas específicas. Esto puede lograrse mediante la adición de un `<location>` elemento `Web.config`, que apunta al archivo cuyas reglas de autorización se diferencian y definir sus reglas de autorización única en él.

Para ilustrar el uso de la `<location>` elemento para invalidar las opciones de configuración para un recurso específico, vamos a personalizar la configuración de autorización para que solo Tito puede visitar `CreatingUserAccounts.aspx`. Para ello, agregue un `<location>` elemento a la `Membership` la carpeta `Web.config` de archivos y actualizar su marcado, por lo que se parezca a lo siguiente:

[!code-xml[Main](user-based-authorization-cs/samples/sample5.xml)]

El `<authorization>` elemento `<system.web>` define las reglas de autorización de dirección URL predeterminada para los recursos ASP.NET en el `Membership` carpeta y sus subcarpetas. El `<location>` elemento nos permite invalidar estas reglas para un recurso determinado. En el marcado anterior el `<location>` referencias del elemento de la `CreatingUserAccounts.aspx` página y especifica su autorización reglas Permitir Tito, pero denegar todos los demás.

Para probar este cambio de autorización, empiece por visitar el sitio Web como un usuario anónimo. Si intentas visitar cualquier página en el `Membership` carpeta, como `UserBasedAuthorization.aspx`, el `UrlAuthorizationModule` denegará la solicitud y se le redirigirá a la página de inicio de sesión. Después de iniciar sesión como, por ejemplo, Scott, puede visitar cualquier página en el `Membership` carpeta *excepto* para `CreatingUserAccounts.aspx`. Al intentar visitar `CreatingUserAccounts.aspx` iniciado sesión como cualquier usuario pero Tito dará como resultado en un intento de acceso no autorizado, se le redirigirá a la página de inicio de sesión.

> [!NOTE]
> El `<location>` elemento debe aparecer fuera de la configuración `<system.web>` elemento. Debe usar otro `<location>` (elemento) para cada recurso cuya configuración de autorización que desea reemplazar.


### <a name="a-look-at-how-theurlauthorizationmoduleuses-the-authorization-rules-to-grant-or-deny-access"></a>Un vistazo a cómo el`UrlAuthorizationModule`usa las reglas de autorización para conceder o denegar acceso

El `UrlAuthorizationModule` determina si autorizar a una identidad concreta para una dirección URL determinada mediante el análisis de la autorización de dirección URL de reglas de uno en uno, a partir de la primera de ellas y avanzando hacia abajo. Tan pronto como se encuentra una coincidencia, se concede o deniega el acceso al usuario, según si se encontró la coincidencia en una `<allow>` o `<deny>` elemento. <strong>Si se encuentra ninguna coincidencia, el usuario se concede acceso.</strong> Por lo tanto, si desea restringir el acceso, es esencial que utilice un `<deny>` como el último elemento en la configuración de autorización de dirección URL. <strong>Si se omite un</strong><strong>`<deny>`</strong><strong>elemento, todos los usuarios se les concederá acceso.</strong>

Para comprender mejor el proceso utilizado por el `UrlAuthorizationModule` para determinar la entidad, considere el ejemplo, las reglas de autorización de dirección URL que vimos anteriormente en este paso. La primera regla es un `<allow>` elemento que permite el acceso a Tito y Scott. Las reglas de segundo es un `<deny>` elemento que se deniega el acceso a todos los usuarios. ¿Si un usuario anónimo visita, los `UrlAuthorizationModule` se inicia con la pregunta es anónimo Scott o Tito? La respuesta, obviamente, es No, por lo que pasa a la segunda regla. ¿Es anónimo en el conjunto de todo el mundo? Desde la respuesta aquí es afirmativa, el `<deny>` regla se coloca en vigor y el visitante que se redirige a la página de inicio de sesión. ¿De forma similar, si está visitando Jisun, el `UrlAuthorizationModule` comienza con la pregunta es Jisun Scott o Tito? ¿Puesto que no, es el `UrlAuthorizationModule` avanza a la segunda pregunta, Jisun está en el conjunto de todo el mundo? Es, por lo que ella también se deniega el acceso. Por último, si visita Tito, la primera pregunta que plantea el `UrlAuthorizationModule` es una respuesta afirmativa, por lo que Tito se le concede acceso.

Puesto que el `UrlAuthorizationModule` procesos de reglas de la autorización de arriba abajo, deteniéndose en cualquier coincidencia, es importante contar con las reglas más específicas preceder a las menos específicas. Es decir, para definir las reglas de autorización que prohíben Jisun y los usuarios anónimos, pero permitir que todos los demás usuarios autenticados, podría comenzar con la regla más específica - el uno que puedan afectar al Jisun - y, a continuación, continúe con las reglas menos específicas: aquellos que permite todos los demás los usuarios autenticados, pero denegar todos los usuarios anónimos. Las reglas de autorización de dirección URL siguientes implementa esta directiva Denegar primero Jisun y, a continuación, deniega cualquier usuario anónimo. Cualquier usuario autenticado que no sean Jisun se le concederá acceso porque ninguna de estas `<deny>` coincidirán con las instrucciones.

[!code-xml[Main](user-based-authorization-cs/samples/sample6.xml)]

## <a name="step-2-fixing-the-workflow-for-unauthorized-authenticated-users"></a>Paso 2: Corregir el flujo de trabajo para que los usuarios no autorizados y autenticados

Como se explicó anteriormente en este tutorial en el A buscar en la sección de flujo de trabajo de autorización de dirección URL, en cualquier momento una solicitud no autorizada ocurre, el `UrlAuthorizationModule` anula la solicitud y devuelve un estado HTTP 401 no autorizado. Este estado 401 es modificado por el `FormsAuthenticationModule` en 302 redirigir el estado que se envía al usuario a la página de inicio de sesión. Este flujo de trabajo se produce en cualquier solicitud no autorizada, incluso si el usuario está autenticado.

Devolución de un usuario autenticado a la página de inicio de sesión es probable que confundirlos puesto que ya ha iniciado sesión en el sistema. Con un poco de trabajo podemos mejorar este flujo de trabajo mediante el redireccionamiento de los usuarios autenticados que realizan solicitudes no autorizadas en una página que explica que ha intentado obtener acceso a una página restringida.

Empiece por crear una nueva página ASP.NET en la carpeta raíz de la aplicación web denominada `UnauthorizedAccess.aspx`; no se olvide de asociar esta página con el `Site.master` página maestra. Después de crear esta página, quite el control de contenido que hace referencia a la `LoginContent` ContentPlaceHolder para que la página principal de contenido predeterminada se mostrará. A continuación, agregue un mensaje que explica la situación, es decir, el usuario que intentó obtener acceso a un recurso protegido. Después de agregar este tipo de mensaje, el `UnauthorizedAccess.aspx` marcado declarativo de la página debe ser similar al siguiente:

[!code-aspx[Main](user-based-authorization-cs/samples/sample7.aspx)]

Ahora tenemos que modificar el flujo de trabajo de modo que si se realiza una solicitud no autorizada por un usuario autenticado se envían a la `UnauthorizedAccess.aspx` página en lugar de la página de inicio de sesión. La lógica que redirige las solicitudes no autorizadas a la página de inicio de sesión se incrusta en un método privado de la `FormsAuthenticationModule` clase, por lo que no podemos personalizar este comportamiento. Lo que podemos hacer, sin embargo, es agregar nuestra propia lógica a la página de inicio de sesión que redirige al usuario a `UnauthorizedAccess.aspx`, si es necesario.

Cuando el `FormsAuthenticationModule` redirige un visitante no autorizado a la página de inicio de sesión que se anexe la dirección de URL solicitada, no autorizado a la cadena de consulta con el nombre `ReturnUrl`. Por ejemplo, si un usuario no autorizado intenta visitar `OnlyTito.aspx`, `FormsAuthenticationModule` les redirigiría `Login.aspx?ReturnUrl=OnlyTito.aspx`. Por lo tanto, si se alcanza la página de inicio de sesión por un usuario autenticado con una cadena de consulta que incluye la `ReturnUrl` parámetro, a continuación, se sabe que este usuario no autenticado intentó simplemente visite una página que no está autorizada a ver. En tal caso, queremos redirigir a `UnauthorizedAccess.aspx`.

Para ello, agregue el código siguiente a la página de inicio de sesión `Page_Load` controlador de eventos:

[!code-csharp[Main](user-based-authorization-cs/samples/sample8.cs)]

El código anterior redirige usuarios autenticados, no autorizados a la `UnauthorizedAccess.aspx` página. Para ver esta lógica en acción, visite el sitio como un visitante anónimo y haga clic en el vínculo crear cuentas de usuario en la columna izquierda. Esto le llevará a la `~/Membership/CreatingUserAccounts.aspx` página, que en el paso 1 se configura solo para permitir el acceso a Tito. Puesto que los usuarios anónimos están prohibidos, el `FormsAuthenticationModule` nos redirige a la página de inicio de sesión.

En este momento estamos anónimos, por lo que `Request.IsAuthenticated` devuelve `false` y no nos estamos redirige a `UnauthorizedAccess.aspx`. En su lugar, se muestra la página de inicio de sesión. Inicie sesión como un usuario distinto Tito, por ejemplo, Bruce. Después de escribir las credenciales adecuadas, el inicio de sesión página redirige nos de nuevo a `~/Membership/CreatingUserAccounts.aspx`. Sin embargo, puesto que esta página solo es accesible para Tito, nos no está autorizados para verlo y rápidamente se devuelven a la página de inicio de sesión. Esta vez, sin embargo, `Request.IsAuthenticated` devuelve `true` (y el `ReturnUrl` parámetro querystring existe), por lo que nos estamos redirige a la `UnauthorizedAccess.aspx` página.


[![Autenticado, los usuarios no autorizados se le redirigirá a UnauthorizedAccess.aspx](user-based-authorization-cs/_static/image17.png)](user-based-authorization-cs/_static/image16.png)

**Figura 6**: autenticado, se redirige a los usuarios no autorizados `UnauthorizedAccess.aspx` ([haga clic aquí para ver imagen en tamaño completo](user-based-authorization-cs/_static/image18.png))


Este flujo de trabajo personalizado presenta una experiencia de usuario más sencilla y razonable por cortocircuito el ciclo que se muestra en la figura 2.

## <a name="step-3-limiting-functionality-based-on-the-currently-logged-in-user"></a>Paso 3: Limitar la funcionalidad basada en el usuario con sesión iniciada actualmente

Autorización de URL facilita especificar reglas de autorización general. Como hemos visto en el paso 1, con autorización para URL nos podemos sucintamente qué identidades están permitidos y cuáles se deniegan al ver una página determinada o todas las páginas en una carpeta. En determinados escenarios, sin embargo, nos conviene que todos los usuarios visitan una página, pero también limitar la funcionalidad de la página según el usuario visita.

Considere el caso de un sitio Web de comercio electrónico que permite a los visitantes autenticados revisar sus productos. Cuando un usuario anónimo visita una página de producto, se vería la información del producto y podría no tener la oportunidad de dejen un comentario. Sin embargo, un usuario autenticado que visita la misma página debería ver la interfaz de revisión. Si el usuario autenticado no tenía aún se ha revisado este producto, la interfaz les permita enviar una revisión; en caso contrario, mostraría ellos su revisión enviado anteriormente. Además, para aprovechar esta situación un paso de la página del producto podría mostrar información adicional y ofrecen características extendidas para aquellos usuarios que trabajan para la empresa de comercio electrónico. Por ejemplo, la página del producto podría enumerar el inventario en existencias e incluyen opciones para editar el precio del producto y su descripción cuando visita por un empleado.

Estas reglas de autorización de grano fino pueden implementarse de forma declarativa o mediante programación (o a través de alguna combinación de ambos). En la siguiente sección veremos cómo implementar la autorización de grano fino mediante el control LoginView. A continuación, exploraremos las técnicas de programación. Antes de que podemos adentrarnos en aplicar reglas de autorización de grano fino, sin embargo, primero es necesario crear una página cuya funcionalidad depende el usuario visita.

Vamos a crear una página que enumera los archivos en un directorio determinado dentro de un control GridView. Además de mostrar el nombre de cada archivo, tamaño y otra información, el control GridView incluirá dos columnas de LinkButtons: uno denominado vista y una eliminación titulada. Si se hace clic en LinkButton vista, se mostrará el contenido del archivo seleccionado; Si se hace clic en LinkButton eliminar, se eliminará el archivo. Vamos a crear inicialmente esta página tal que su funcionalidad de vista y delete no está disponible para todos los usuarios. En el uso en las secciones LoginView Control y limitar la funcionalidad mediante programación, veremos cómo habilitar o deshabilitar estas características según el usuario visita la página.

> [!NOTE]
> La página ASP.NET que se van a compilar usa un control GridView para mostrar una lista de archivos. Desde el tutorial de esta serie se centra en los roles, las cuentas de usuario, autorización y autenticación de formularios, no quiero dedicar demasiado tiempo en analizar el funcionamiento interno del control GridView. Aunque este tutorial proporciona instrucciones paso a paso específicas para la configuración de esta página, no inspeccionaremos los detalles de por qué se realizaron determinadas elecciones o lo tienen determinadas propiedades de efecto en la salida representada. Para obtener un análisis completo del control GridView, consulte mi *[trabajar con datos en ASP.NET 2.0](../../data-access/index.md)* serie de tutoriales.


Comience abriendo la `UserBasedAuthorization.aspx` de archivos en el `Membership` carpeta y la adición de un control GridView a la página llamada `FilesGrid`. En las etiquetas inteligentes de GridView, haga clic en el vínculo Edit Columns para abrir el cuadro de diálogo campos. Desde aquí, desactive la casilla generar automáticamente los campos en la esquina inferior izquierda. A continuación, agregue un botón de selección, un botón de eliminación y dos BoundFields desde la esquina superior izquierda (los botones Seleccionar y eliminar pueden encontrarse en el tipo CommandField). Establezca el botón de selección `SelectText` propiedad a la vista y el BoundField primera `HeaderText` y `DataField` las propiedades de nombre. Establecer el BoundField segundo `HeaderText` propiedad de tamaño en Bytes, su `DataField` propiedad a la longitud, su `DataFormatString` propiedad {0:N0} y su `HtmlEncode` propiedad en False.

Después de configurar las columnas de GridView, haga clic en Aceptar para cerrar el cuadro de diálogo campos. En la ventana Propiedades, establezca la GridView `DataKeyNames` propiedad `FullName`. En este punto marcado declarativo de GridView debería ser similar al siguiente:

[!code-aspx[Main](user-based-authorization-cs/samples/sample9.aspx)]

Con el marcado de GridView que creado, ya estamos listos para escribir el código que se recuperará los archivos en un directorio determinado y enlazarlos a GridView. Agregue el código siguiente a la página `Page_Load` controlador de eventos:

[!code-csharp[Main](user-based-authorization-cs/samples/sample10.cs)]

El código anterior usa el [ `DirectoryInfo` clase](https://msdn.microsoft.com/library/system.io.directoryinfo.aspx) para obtener una lista de los archivos en la carpeta raíz de la aplicación. El [ `GetFiles()` método](https://msdn.microsoft.com/library/system.io.directoryinfo.getfiles.aspx) devuelve como una matriz de todos los archivos en el directorio [ `FileInfo` objetos](https://msdn.microsoft.com/library/system.io.fileinfo.aspx), que se enlaza a la GridView. El `FileInfo` objeto tiene una gran variedad de propiedades, como `Name`, `Length`, y `IsReadOnly`, entre otros. Como puede ver en el marcado declarativo, el control GridView muestra sólo el `Name` y `Length` propiedades.

> [!NOTE]
> El `DirectoryInfo` y `FileInfo` clases se encuentran en el [ `System.IO` espacio de nombres](https://msdn.microsoft.com/library/system.io.aspx). Por lo tanto, necesitará colocar delante de estos nombres de clases con sus nombres de espacio de nombres o tener el espacio de nombres importado en el archivo de clase (a través de `using System.IO`).


Dedique un momento a visitar esta página a través de un explorador. Mostrará la lista de archivos que residen en el directorio raíz de la aplicación. Al hacer clic en cualquiera de la vista o eliminar LinkButtons provocará una devolución de datos, pero no se producirá ninguna acción porque aún no hemos crear los controladores de eventos necesarios.


[![El control GridView incluyen los archivos en el directorio raíz de la aplicación Web](user-based-authorization-cs/_static/image20.png)](user-based-authorization-cs/_static/image19.png)

**Figura 7**: el control GridView incluyen los archivos en el directorio raíz de la aplicación Web ([haga clic aquí para ver imagen en tamaño completo](user-based-authorization-cs/_static/image21.png))


Necesitamos un medio para mostrar el contenido del archivo seleccionado. Vuelva a Visual Studio y agregue un cuadro de texto denominado `FileContents` por encima del control GridView. Establezca su `TextMode` propiedad `MultiLine` y su `Columns` y `Rows` propiedades al 95% y 10, respectivamente.

[!code-aspx[Main](user-based-authorization-cs/samples/sample11.aspx)]

A continuación, cree un controlador de eventos para el control de GridView [ `SelectedIndexChanged` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindexchanged.aspx) y agregue el código siguiente:

[!code-csharp[Main](user-based-authorization-cs/samples/sample12.cs)]

Este código usa la GridView `SelectedValue` propiedad para determinar el nombre completo del archivo del archivo seleccionado. Internamente, el `DataKeys` hace referencia a la colección con el fin de obtener el `SelectedValue`, por lo que es imperativo que establezca la GridView `DataKeyNames` propiedad al nombre, como se describe anteriormente en este paso. El [ `File` clase](https://msdn.microsoft.com/library/system.io.file.aspx) se usa para leer contenido del archivo seleccionado en una cadena, que, a continuación, se asigna a la `FileContents` del cuadro de texto `Text` propiedad, con lo que se muestra el contenido del archivo seleccionado en la página.


[![Contenido del archivo seleccionado se muestra en el cuadro de texto](user-based-authorization-cs/_static/image23.png)](user-based-authorization-cs/_static/image22.png)

**Figura 8**: del contenido del archivo seleccionado The se muestra en el cuadro de texto ([haga clic aquí para ver imagen en tamaño completo](user-based-authorization-cs/_static/image24.png))


> [!NOTE]
> Si ver el contenido de un archivo que contiene código HTML y, a continuación, intente ver o eliminar un archivo, recibirá un `HttpRequestValidationException` error. Esto sucede porque en el postback contenido del cuadro de texto se envían al servidor web. De forma predeterminada, ASP.NET genera un `HttpRequestValidationException` error cada vez que el contenido de devolución de datos potencialmente peligroso, como marcado HTML, se detecta. Para deshabilitar este error se produzca, desactivar la validación de solicitudes de la página agregando `ValidateRequest="false"` a la `@Page` directiva. Para obtener más información sobre las ventajas de la validación de solicitudes como así como qué precauciones debería tomar cuando deshabilitarlo, lea [validación de solicitud: evitar los ataques de secuencias de comandos](https://asp.net/learn/whitepapers/request-validation/).


Finalmente, agregue un controlador de eventos con el código siguiente para el control de GridView [ `RowDeleting` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx):

[!code-csharp[Main](user-based-authorization-cs/samples/sample13.cs)]

El código simplemente muestra el nombre completo del archivo que desea eliminar en el `FileContents` TextBox *sin* eliminar realmente el archivo.


[![Al hacer clic en el botón de eliminación no elimina realmente el archivo](user-based-authorization-cs/_static/image26.png)](user-based-authorization-cs/_static/image25.png)

**Figura 9**: al hacer clic en la eliminación botón no elimina realmente el archivo ([haga clic aquí para ver imagen en tamaño completo](user-based-authorization-cs/_static/image27.png))


En el paso 1, hemos configurado las reglas de autorización de dirección URL para impedir que los usuarios anónimos ver las páginas en el `Membership` carpeta. Con el fin de presentar mejor la autenticación de grano fino, vamos a permitir a los usuarios anónimos visitar el `UserBasedAuthorization.aspx` página, pero con funcionalidad limitada. Para abrir esta página para tener acceso a todos los usuarios, agregue el siguiente `<location>` elemento a la `Web.config` de archivos en el `Membership` carpeta:

[!code-xml[Main](user-based-authorization-cs/samples/sample14.xml)]

Después de agregar este `<location>` elemento, las nuevas reglas de autorización de dirección URL de prueba iniciando sesión fuera del sitio. Como usuario anónimo debería permite visitar la `UserBasedAuthorization.aspx` página.

Actualmente, puede visitar cualquier usuario autenticado o anónimo el `UserBasedAuthorization.aspx` página y ver o eliminar archivos. Vamos a hacer para que solo los usuarios autenticados pueden ver contenido de un archivo y Tito sólo puede eliminar un archivo. Estas reglas de autorización de grano fino pueden aplicarse mediante declaración, mediante programación o mediante una combinación de ambos métodos. Vamos a usar el enfoque declarativo para limitar quién puede ver el contenido de un archivo; vamos a usar el enfoque de programación para limitar quién puede eliminar un archivo.

### <a name="using-the-loginview-control"></a>Uso del Control LoginView

Como hemos visto en los tutoriales anteriores, el control LoginView es útil para mostrar distintas interfaces para los usuarios autenticados y anónimos y ofrece una forma sencilla para ocultar la funcionalidad que no es accesible a usuarios anónimos. Puesto que los usuarios anónimos no pueden ver o eliminar archivos, solo se deben mostrar el `FileContents` cuadro de texto cuando se visita la página de un usuario autenticado. Para ello, agregue un control LoginView a la página, asígnele el nombre `LoginViewForFileContentsTextBox`y mover el `FileContents` marcado declarativo del cuadro de texto en el control LoginView `LoggedInTemplate`.

[!code-aspx[Main](user-based-authorization-cs/samples/sample15.aspx)]

Los controles Web de plantillas de LoginView ya no son directamente accesibles desde la clase de código subyacente. Por ejemplo, el `FilesGrid` de GridView `SelectedIndexChanged` y `RowDeleting` actualmente hacen referencia a los controladores de eventos el `FileContents` como control de cuadro de texto con código:

[!code-csharp[Main](user-based-authorization-cs/samples/sample16.cs)]

Sin embargo, este código ya no es válido. Moviendo el `FileContents` cuadro de texto en el `LoggedInTemplate` no se puede acceder directamente el cuadro de texto. En su lugar, debemos usar la `FindControl("controlId")` método mediante programación hace referencia al control. Actualización de la `FilesGrid` controladores de eventos para hacer referencia el cuadro de texto de este modo:

[!code-csharp[Main](user-based-authorization-cs/samples/sample17.cs)]

Después de mover el cuadro de texto a la LoginView `LoggedInTemplate` y actualizar el código de la página a la referencia del cuadro de texto mediante el `FindControl("controlId")` de patrón, visite la página como usuario anónimo. Como se muestra en la figura 10, la `FileContents` no se muestra el cuadro de texto. Sin embargo, se sigue mostrando el control LinkButton de vista.


[![El Control LoginView solo representa el cuadro de texto entre sí para usuarios autenticados](user-based-authorization-cs/_static/image29.png)](user-based-authorization-cs/_static/image28.png)

**Figura 10**: The LoginView Control solo presenta la `FileContents` TextBox para usuarios autenticados ([haga clic aquí para ver imagen en tamaño completo](user-based-authorization-cs/_static/image30.png))


Es una manera de ocultar el botón de vista para los usuarios anónimos convertir el campo GridView en TemplateField. Esto generará una plantilla que contiene el marcado declarativo para el control LinkButton de vista. A continuación, podemos agregar un control LoginView a TemplateField y colocar dentro de la LoginView LinkButton `LoggedInTemplate`, lo que ocultar el botón de vista de los visitantes anónimos. Para ello, haga clic en el vínculo Editar columnas de etiqueta de GridView inteligente para iniciar el cuadro de diálogo campos. A continuación, seleccione el botón de selección de la lista en la esquina inferior izquierda y, a continuación, haga clic en este campo a un vínculo a TemplateField la función Convert. Si lo hace, modificará el marcado declarativo del campo de:

[!code-aspx[Main](user-based-authorization-cs/samples/sample18.aspx)]

 A: 

[!code-aspx[Main](user-based-authorization-cs/samples/sample19.aspx)]

En este momento, podemos agregar un LoginView a TemplateField. El siguiente marcado muestra LinkButton vista solo para los usuarios autenticados.

[!code-aspx[Main](user-based-authorization-cs/samples/sample20.aspx)]

Como se muestra en la figura 11, el resultado final no es que bastante como la vista de columna se sigue mostrando, aunque se ocultan la LinkButtons vista dentro de la columna. Veremos cómo ocultar toda la columna de GridView (y no solo LinkButton) en la sección siguiente.


[![El Control LoginView oculta la LinkButtons vista para los visitantes anónimos](user-based-authorization-cs/_static/image32.png)](user-based-authorization-cs/_static/image31.png)

**Figura 11**: el Control LoginView oculta la LinkButtons vista para los visitantes anónimos ([haga clic aquí para ver imagen en tamaño completo](user-based-authorization-cs/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Limitar la funcionalidad mediante programación

En algunas circunstancias, declarativas técnicas son insuficientes para limitar la funcionalidad a una página. Por ejemplo, la disponibilidad de cierta funcionalidad de la página puede depender de criterios más allá de si el usuario visita la página es anónimo o autenticado. En tales casos, se pueden mostrar u ocultos a través de medios mediante programación los distintos elementos de interfaz de usuario.

Para limitar la funcionalidad mediante programación, es necesario realizar dos tareas:

1. Determinar si el usuario visita la página puede tener acceso a la funcionalidad, y
2. Modificar mediante programación la interfaz de usuario en función de si el usuario tiene acceso a la funcionalidad en cuestión.

Para demostrar la aplicación de estas dos tareas, vamos a permitir sólo Tito eliminar los archivos de GridView. A continuación, es nuestra primera tarea determinar si se trata de Tito visitar la página. Una vez que se ha determinado, es necesario ocultar columna de eliminación de GridView (o mostrar). Las columnas de GridView son accesibles a través de su `Columns` propiedad; solo se representa una columna si su `Visible` propiedad está establecida en `true` (valor predeterminado).

Agregue el código siguiente a la `Page_Load` controlador de eventos antes de enlazar los datos en GridView:

[!code-csharp[Main](user-based-authorization-cs/samples/sample21.cs)]

Como se explicó en la [ *una visión general de autenticación mediante formularios* ](../introduction/an-overview-of-forms-authentication-cs.md) tutorial, `User.Identity.Name` devuelve el nombre de identidad. Esto corresponde al nombre de usuario escrito en el control de inicio de sesión. Si es Tito visitando la página, segunda columna de GridView `Visible` propiedad está establecida en `true`; en caso contrario, se establece en `false`. El resultado neto es que cuando alguien ajeno a Tito visita la página, otro usuario autenticado o un usuario anónimo, la columna de eliminación no se representa (Véase la figura 12). Sin embargo, cuando Tito visita la página, la columna de eliminación está presente (consulte la figura 13).


[![Eliminar columna no es procesa cuando visitó por alguien distinto Tito (por ejemplo, Bruce)](user-based-authorization-cs/_static/image35.png)](user-based-authorization-cs/_static/image34.png)

**Figura 12**: eliminar la columna no es procesa cuando visitó por alguien distinto Tito (por ejemplo, Bruce) ([haga clic aquí para ver imagen en tamaño completo](user-based-authorization-cs/_static/image36.png))


[![Eliminar columna se representa para Tito](user-based-authorization-cs/_static/image38.png)](user-based-authorization-cs/_static/image37.png)

**Figura 13**: eliminar la columna se representa para Tito ([haga clic aquí para ver imagen en tamaño completo](user-based-authorization-cs/_static/image39.png))


## <a name="step-4-applying-authorization-rules-to-classes-and-methods"></a>Paso 4: Aplicar reglas de autorización a las clases y métodos

En el paso 3 se no se permiten usuarios anónimos de la vista de contenido de un archivo y se prohíbe la eliminación de archivos de todos los usuarios pero Tito. Esto se logra al ocultar los elementos de interfaz de usuario asociado para los visitantes no autorizados a través de técnicas declarativos y programáticos. Para nuestro ejemplo, ocultando correctamente los elementos de interfaz de usuario fue sencillo, pero ¿qué sucede sitios más complejos donde puede haber muchas maneras diferentes para realizar la misma funcionalidad? En la limitación de esa funcionalidad a los usuarios no autorizados, ¿qué ocurre si olvidamos ocultar o deshabilitar todos los elementos de interfaz de usuario aplicable?

Es una manera sencilla de asegurarse de que un usuario no autorizado no puede tener acceso a un elemento determinado de funcionalidad decorar esa clase o método con el [ `PrincipalPermission` atributo](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Cuando el tiempo de ejecución de .NET usa una clase o ejecuta uno de sus métodos, comprueba para asegurarse de que el contexto de seguridad actual tiene permiso para usar la clase o el método execute. El `PrincipalPermission` atributo proporciona un mecanismo a través del cual podemos definir estas reglas.

Vamos a demostrar el uso de la `PrincipalPermission` atributo en el control de GridView `SelectedIndexChanged` y `RowDeleting` controladores de eventos para prohibir la ejecución, los usuarios anónimos y los usuarios que no sean Tito, respectivamente. Todo lo que necesitamos hacer es agregar el atributo apropiado de la parte superior de cada definición de función:

[!code-csharp[Main](user-based-authorization-cs/samples/sample22.cs)]

El atributo para el `SelectedIndexChanged` dicta de controlador de eventos que solo los usuarios autenticados puede ejecutar el controlador de eventos, mientras que el atributo en el `RowDeleting` controlador de eventos limita la ejecución a Tito.

Si alguna manera, un usuario distinto Tito intenta ejecutar la `RowDeleting` controlador de eventos o un usuario no autenticado intenta ejecutar la `SelectedIndexChanged` controlador de eventos, se producirá el runtime de .NET un `SecurityException`.


[![Si el contexto de seguridad no está autorizado para ejecutar el método, se produce una excepción SecurityException](user-based-authorization-cs/_static/image41.png)](user-based-authorization-cs/_static/image40.png)

**Figura 14**: si el contexto de seguridad no está autorizado para ejecutar el método, un `SecurityException` se produce ([haga clic aquí para ver imagen en tamaño completo](user-based-authorization-cs/_static/image42.png))


> [!NOTE]
> Para permitir que varios contextos de seguridad tener acceso a una clase o método, decore la clase o método con un `PrincipalPermission` atributo para cada contexto de seguridad. Es decir, para permitir Tito y Bruce ejecutar el `RowDeleting` controlador de eventos, agregue *dos* `PrincipalPermission` atributos:


[!code-csharp[Main](user-based-authorization-cs/samples/sample23.cs)]

Además de las páginas ASP.NET, muchas aplicaciones también tienen una arquitectura que incluye varias capas, como lógica de negocios y los niveles de acceso a datos. Estas capas se implementan normalmente como bibliotecas de clases y ofrecen las clases y métodos para realizar la funcionalidad relacionada con datos y lógica de negocio. El `PrincipalPermission` atributo es útil para aplicar las reglas de autorización a estos niveles.

Para obtener más información sobre el uso de la `PrincipalPermission` atributo para definir las reglas de autorización en las clases y métodos, consulte [Scott Guthrie](https://weblogs.asp.net/scottgu/)de entrada de blog [agregar reglas de autorización a Business y capas de datos con `PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Resumen

En este tutorial hemos examinado cómo aplicar las reglas de autorización basada en usuario. Comenzamos con una mirada a ASP. Marco de autorización de dirección URL de la red. En cada solicitud, el motor ASP.NET `UrlAuthorizationModule` inspecciona las reglas de autorización de URL definidas en la configuración de la aplicación para determinar si la identidad está autorizada para acceder al recurso solicitado. En resumen, autorización URL facilita especificar las reglas de autorización para una página específica o para todas las páginas en un directorio determinado.

El marco de autorización de dirección URL aplica las reglas de autorización en la página por página. Con autorización de URL, ya sea la identidad del solicitante está autorizada para tener acceso a un recurso determinado o no. Muchos escenarios, sin embargo, se llama para obtener más reglas de autorización de grano fino. En lugar de definir quién tiene permiso de acceso de una página, necesitamos ponernos para todos los usuarios que accedan a una página, pero mostrar datos diferentes o para ofrecer una funcionalidad diferente según el usuario visita la página. Autorización de nivel de página normalmente implica ocultar elementos de la interfaz de usuario específico con el fin de impedir que los usuarios no autorizados tengan acceso a funcionalidad prohibida. Además, es posible usar atributos para restringir el acceso a las clases y la ejecución de sus métodos para determinados usuarios.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Agregar reglas de autorización al negocio y capas de datos mediante `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Autorización de ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Cambios entre IIS6 y la seguridad de IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Configuración de archivos y subdirectorios específicos](https://msdn.microsoft.com/library/6hbkh9s7.aspx)
- [Limitar la funcionalidad de modificación de datos en función del usuario](../../data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs.md)
- [Tutoriales del Control LoginView](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/loginview.aspx)
- [Descripción de autorización de URL de IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)
- [`UrlAuthorizationModule` Documentación técnica](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)
- [Trabajar con datos en ASP.NET 2.0](../../data-access/index.md)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha estado trabajando con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es *[SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Puede ponerse en contacto Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](validating-user-credentials-against-the-membership-user-store-cs.md)
> [Siguiente](storing-additional-user-information-cs.md)
