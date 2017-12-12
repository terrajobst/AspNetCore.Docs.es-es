---
uid: web-forms/overview/older-versions-security/membership/user-based-authorization-cs
title: "Autorización basada en usuario (C#) | Documentos de Microsoft"
author: rick-anderson
description: "En este tutorial, veremos para limitar el acceso a las páginas y restringir la funcionalidad de nivel de página a través de una serie de técnicas."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 3c815a9e-2296-4b9b-b945-776d54989daa
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/user-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: da03a9c3e22f5a2164534ef7896b5558beb8b6f4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="user-based-authorization-c"></a>Autorización basada en usuario (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_07_CS.zip) o [descarga de PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial07_UserAuth_cs.pdf)

> En este tutorial, veremos para limitar el acceso a las páginas y restringir la funcionalidad de nivel de página a través de una serie de técnicas.


## <a name="introduction"></a>Introducción

Mayoría de las aplicaciones web que ofrecen las cuentas de usuario hacerlo en parte para restringir ciertos visitantes tengan acceso a ciertas páginas dentro del sitio. En los sitios en el panel de mensajes de más en línea, por ejemplo, todos los usuarios - autenticados y anónimos - les permite verla entradas del panel de mensajes, pero sólo los usuarios autenticados pueden visitar la página web para crear una nueva publicación. Y puede haber páginas administrativas que solo están disponibles para un usuario determinado (o un conjunto determinado de usuarios). Además, funcionalidad en el nivel de página puede diferir según el usuario por el usuario. Al ver una lista de entradas, los usuarios autenticados se muestran una interfaz para la clasificación de cada publicación, mientras que esta interfaz no está disponible para los visitantes anónimos.

ASP.NET resulta muy sencillo definir reglas de autorización basada en usuario. Con un poco de marcado en `Web.config`, páginas web específicas o directorios completos pueden bloquearse para que solo están accesibles a un subconjunto específico de usuarios. Funcionalidad en el nivel de página puede estar activada o desactivada según el usuario actualmente conectado a través de medios mediante programación y declarativas.

En este tutorial, veremos para limitar el acceso a las páginas y restringir la funcionalidad de nivel de página a través de una serie de técnicas. Comencemos.

## <a name="a-look-at-the-url-authorization-workflow"></a>Un vistazo al flujo de trabajo de autorización de dirección URL

Como se describe en el [ *una visión general de autenticación mediante formularios* ](../introduction/an-overview-of-forms-authentication-cs.md) tutorial, cuando el tiempo de ejecución ASP.NET procesa una solicitud para un recurso ASP.NET la solicitud, genera un número de eventos durante su ciclo de vida. *Módulos HTTP* son clases administradas cuyo código se ejecuta en respuesta a un evento determinado en el ciclo de vida de la solicitud. ASP.NET se suministra con un número de módulos HTTP que realizan tareas esenciales en segundo plano.

Es uno de esos módulos HTTP [ `FormsAuthenticationModule` ](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthenticationmodule.aspx). Como se describe en los tutoriales anteriores, la función principal de la `FormsAuthenticationModule` consiste en determinar la identidad de la solicitud actual. Esto se consigue mediante la inspección el vale de autenticación de formularios, que se encuentra en una cookie o incrustado en la dirección URL. Esta identificación tiene lugar durante la [ `AuthenticateRequest` eventos](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.authenticaterequest.aspx).

Otro módulo HTTP importante es la [ `UrlAuthorizationModule` ](https://msdn.microsoft.com/en-us/library/system.web.security.urlauthorizationmodule.aspx), que se produce en respuesta a la [ `AuthorizeRequest` eventos](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.authorizerequest.aspx) (algo que tiene lugar después de la `AuthenticateRequest` evento). El `UrlAuthorizationModule` examina el marcado de configuración en `Web.config` para determinar si la identidad actual tiene autoridad para visitar la página especificada. Este proceso se conoce como *autorizaciones de direcciones URL*.

Examinaremos la sintaxis de las reglas de autorización de dirección URL en el paso 1, pero primero vamos a ver lo que el `UrlAuthorizationModule` no hace según si la solicitud está autorizada o no. Si el `UrlAuthorizationModule` determina que la solicitud está autorizada, a continuación, no realiza ninguna acción y la solicitud continúa a través de su ciclo de vida. Sin embargo, si la solicitud es *no* autorizado, la `UrlAuthorizationModule` anula el ciclo de vida e indica la `Response` objeto que se va a devolver un [HTTP 401 no autorizado](http://www.checkupdown.com/status/E401.html) estado. Al utilizar la autenticación de formularios este estado HTTP 401 nunca se devuelve al cliente porque si el `FormsAuthenticationModule` detecta el estado es HTTP 401 que modifica un [HTTP 302 redirigir](http://www.checkupdown.com/status/E302.html) a la página de inicio de sesión.

Figura 1 muestra el flujo de trabajo de la canalización ASP.NET, el `FormsAuthenticationModule`y el `UrlAuthorizationModule` cuando llega una solicitud no autorizada. En concreto, la figura 1 muestra una solicitud por un visitante anónimo para `ProtectedPage.aspx`, que es una página que se deniega el acceso a los usuarios anónimos. Puesto que el visitante es anónimo, la `UrlAuthorizationModule` anula la solicitud y devuelve un estado HTTP 401 no autorizado. El `FormsAuthenticationModule` , a continuación, convierte el estado 401 en una redirección 302 a la página de inicio de sesión. Cuando el usuario se autentica a través de la página de inicio de sesión, se le redirige a `ProtectedPage.aspx`. Esta vez el `FormsAuthenticationModule` identifica al usuario según su vale de autenticación. Ahora que el visitante se autentica, el `UrlAuthorizationModule` permite el acceso a la página.


[![La autenticación de formularios y el flujo de trabajo de autorización de dirección URL](user-based-authorization-cs/_static/image2.png)](user-based-authorization-cs/_static/image1.png)

**Figura 1**: la autenticación de formularios y flujo de trabajo de autorización de dirección URL ([haga clic aquí para ver la imagen a tamaño completo](user-based-authorization-cs/_static/image3.png))


Figura 1 muestra la interacción que tiene lugar cuando un visitante anónimo intenta obtener acceso a un recurso que no está disponible para los usuarios anónimos. En tal caso, el visitante anónimo se redirige a la página de inicio de sesión con la página que intentó visitar especificado en la cadena de consulta. Una vez que el usuario ha iniciado sesión correctamente, se redirigirá automáticamente a los recursos que inicialmente estaba intentando ver.

Cuando se realiza la solicitud no autorizada por un usuario anónimo, este flujo de trabajo es sencillo y es fácil de entender lo que ha ocurrido el visitante y por qué. Pero tenga en cuenta que la `FormsAuthenticationModule` redirigirá *cualquier* no autorizado de usuario a la página de inicio de sesión, incluso si la solicitud se realiza por un usuario autenticado. Si un usuario autenticado intenta visitar una página que no tiene autoridad para los que esto puede producir una experiencia de usuario confusa.

Imagine que nuestro sitio Web tenía sus reglas de autorización de dirección URL configuradas tal que la página ASP.NET `OnlyTito.aspx` era accesible sólo Tito. Ahora, imagine que Sam visita el sitio, inicie sesión y, a continuación, intenta visitar `OnlyTito.aspx`. El `UrlAuthorizationModule` se detendrá el ciclo de vida de la solicitud y devolverá un estado HTTP 401 no autorizado, que la `FormsAuthenticationModule` detectará y, a continuación, redirija Sam a la página de inicio de sesión. Puesto que Sam ya ha iniciado sesión, sin embargo, quizás se pregunte por qué se ha enviado a la página de inicio de sesión. Podría razón para que sus credenciales de inicio de sesión se han perdido algún modo o que había escrito credenciales no válidas. Si se vuelven a entrar en Sam sus credenciales de la página de inicio de sesión ha iniciado sesión (nuevo) y se redirige a `OnlyTito.aspx`. El `UrlAuthorizationModule` detectará que Sam no visite esta página y se devolverá a la página de inicio de sesión.

Figura 2 muestra este flujo de trabajo confuso.


[![Flujo de trabajo predeterminado puede dar lugar a un ciclo confuso](user-based-authorization-cs/_static/image5.png)](user-based-authorization-cs/_static/image4.png)

**Figura 2**: el valor predeterminado flujo de trabajo puede dar lugar a un ciclo confuso ([haga clic aquí para ver la imagen a tamaño completo](user-based-authorization-cs/_static/image6.png))


El flujo de trabajo que se muestra en la figura 2 puede befuddle rápidamente incluso la mayoría equipo expertos visitante. Veremos formas de evitar que esto confuso ciclo en el paso 2.

> [!NOTE]
> ASP.NET usa dos mecanismos para determinar si el usuario actual puede tener acceso a una página web determinada: autorización de dirección URL y de archivo. Autorización de archivo se implementa mediante el [ `FileAuthorizationModule` ](https://msdn.microsoft.com/en-us/library/system.web.security.fileauthorizationmodule.aspx), que determina la entidad consultando los archivos solicitados ACL. Autorización de archivo normalmente se utiliza con la autenticación de Windows porque las ACL son los permisos que se aplican a las cuentas de Windows. Cuando se utiliza la autenticación de formularios, todas las solicitudes de nivel de sistema operativo system - y archivos se ejecutan con la misma cuenta de Windows, independientemente del usuario visita el sitio. Puesto que esta serie de tutoriales se centra en la autenticación de formularios, no analizaremos la autorización de archivos.


### <a name="the-scope-of-url-authorization"></a>El ámbito de autorización de URL

El `UrlAuthorizationModule` es el código administrado que forma parte de la ejecución de ASP.NET. Antes de la versión 7 de Microsoft [Internet Information Services (IIS)](https://www.iis.net/) servidor web, se produjo una barrera distinta entre la canalización HTTP de IIS y la canalización del runtime ASP.NET. En pocas palabras, en IIS 6 y versiones anteriores, ASP. De NET `UrlAuthorizationModule` solo se ejecuta cuando se delega una solicitud de IIS para el tiempo de ejecución ASP.NET. De forma predeterminada, IIS procesa contenido estático propio - como páginas HTML y CSS, JavaScript y archivos de imagen - y las solicitudes para el tiempo de ejecución ASP.NET cuando una página con una extensión de entrega solo `.aspx`, `.asmx`, o `.ashx` se solicita.

Sin embargo, IIS 7, permite para IIS integrada y ASP.NET canalizaciones. Con algunos valores de configuración, puede configurar IIS 7 para invocar la `UrlAuthorizationModule` para *todos los* las solicitudes, lo que significa que se pueden definir las reglas de autorización de dirección URL para los archivos de cualquier tipo. Además, IIS 7 incluye su propio motor de autorización de dirección URL. Para obtener más información sobre la integración de ASP.NET y la funcionalidad de autorización de dirección URL nativa de IIS 7, consulte [autorizaciones de direcciones URL de IIS7 descripción](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization). Para obtener una visión más detallada de la integración de ASP.NET e IIS 7, recoger una copia del libro de Shahram Khosravi, *Professional IIS 7 y ASP.NET integrado Programming* (ISBN: 0470152539 978).

En pocas palabras, en versiones anteriores de IIS 7, las reglas de autorización de dirección URL se aplican sólo a los recursos manejados por el tiempo de ejecución ASP.NET. Pero con IIS 7 es posible utilizar la característica de autorización de URL nativo de IIS o para integrar ASP. De NET `UrlAuthorizationModule` en la canalización de HTTP de IIS, ampliando así esta funcionalidad en todas las solicitudes.

> [!NOTE]
> Hay algunas diferencias sutiles pero importantes en el modo ASP. De NET `UrlAuthorizationModule` y la característica de autorización de dirección URL de IIS 7 procesar las reglas de autorización. Este tutorial no examina la funcionalidad de autorización de dirección URL de IIS 7 o las diferencias en cómo analizar las reglas de autorización en comparación con el `UrlAuthorizationModule`. Para obtener más información acerca de estos temas, consulte la documentación de IIS 7 en MSDN o en [www.iis.net](https://www.iis.net/).


## <a name="step-1-defining-url-authorization-rules-inwebconfig"></a>Paso 1: Definir reglas de autorización de dirección URL en`Web.config`

El `UrlAuthorizationModule` determina si se debe conceder o denegar el acceso a un recurso solicitado para una identidad concreta según las reglas de autorización de dirección URL definidas en la configuración de la aplicación. Las reglas de autorización se establece en el [ `<authorization>` elemento](https://msdn.microsoft.com/en-us/library/8d82143t.aspx) en forma de `<allow>` y `<deny>` los elementos secundarios. Cada `<allow>` y `<deny>` puede especificar el elemento secundario:

- Un usuario determinado
- Una lista delimitada por comas de los usuarios
- Todos los usuarios anónimos, indicados por un signo de interrogación (?)
- Todos los usuarios, se indica con un asterisco (\*)

El marcado siguiente muestra cómo usar las reglas de autorización de dirección URL para permitir que a los usuarios Tito y Scott y rechazar todos los demás:

[!code-xml[Main](user-based-authorization-cs/samples/sample1.xml)]

El `<allow>` elemento define qué usuarios se permiten - Tito y Scott - mientras el `<deny>` elemento indica que *todos los* se deniegan a los usuarios.

> [!NOTE]
> El `<allow>` y `<deny>` elementos también pueden especificar reglas de autorización para los roles. Examinaremos la autorización basada en roles en un tutorial posterior.


La siguiente configuración concede acceso a cualquiera que no sea de Sam (incluidos los visitantes anónimos):

[!code-xml[Main](user-based-authorization-cs/samples/sample2.xml)]

Para permitir que sólo los usuarios autenticados, use la configuración siguiente, que deniega el acceso a todos los usuarios anónimos:

[!code-xml[Main](user-based-authorization-cs/samples/sample3.xml)]

Las reglas de autorización se definen dentro de la `<system.web>` elemento `Web.config` y se aplican a todos los recursos de la aplicación web ASP.NET. A menudo, una aplicación tiene reglas de autorización diferentes para distintas secciones. Por ejemplo, en un sitio de comercio electrónico, todos los visitantes pueden leer detenidamente los productos, consultar las reseñas de producto, buscar en el catálogo y así sucesivamente. Sin embargo, solo los usuarios autenticados pueden llegar a la desprotección o las páginas que se va a administrar el historial de trasvase de uno. Además, puede haber algunas partes del sitio que solo son accesibles para seleccionar usuarios, por ejemplo, los administradores del sitio.

ASP.NET resulta muy sencillo definir reglas de autorización diferentes para distintos archivos y carpetas en el sitio. Las reglas de autorización especificadas en la carpeta raíz `Web.config` archivo se aplican a todos los recursos ASP.NET en el sitio. Sin embargo, se puede invalidar esta configuración de autorización predeterminado para una carpeta concreta mediante la adición de un `Web.config` con un `<authorization>` sección.

Vamos a actualizar nuestro sitio Web para que sólo los usuarios autenticados pueden visitar las páginas ASP.NET en el `Membership` carpeta. Para ello hay que agregar una `Web.config` del archivo a la `Membership` carpeta y establecer su configuración de autorización para denegar a los usuarios anónimos. Haga clic en el `Membership` carpeta en el Explorador de soluciones, elija el menú Agregar nuevo elemento en el menú contextual y agregar un nuevo archivo de configuración Web denominado `Web.config`.


[![Agregue un archivo Web.config en la carpeta de pertenencia](user-based-authorization-cs/_static/image8.png)](user-based-authorization-cs/_static/image7.png)

**Figura 3**: agregar un `Web.config` del archivo a la `Membership` carpeta ([haga clic aquí para ver la imagen a tamaño completo](user-based-authorization-cs/_static/image9.png))


En este momento, el proyecto debe contener dos `Web.config` archivos: uno en el directorio raíz y uno en el `Membership` carpeta.


[![La aplicación debe contener ahora los dos archivos Web.config](user-based-authorization-cs/_static/image11.png)](user-based-authorization-cs/_static/image10.png)

**Figura 4**: la aplicación debe ahora contiene dos `Web.config` archivos ([haga clic aquí para ver la imagen a tamaño completo](user-based-authorization-cs/_static/image12.png))


Actualizar el archivo de configuración en el `Membership` carpeta por lo que TI prohíbe el acceso a los usuarios anónimos.

[!code-xml[Main](user-based-authorization-cs/samples/sample4.xml)]

Eso es todo lo!

Para probar este cambio, visite la página principal en un explorador y asegúrese de que se cerró la sesión. Puesto que es el comportamiento predeterminado de una aplicación ASP.NET permitir que todos los visitantes, y puesto que se no realizar ninguna modificación de autorización en el directorio raíz `Web.config` archivo, se pueden visitar los archivos en el directorio raíz como un visitante anónimo.

Haga clic en el vínculo de creación de cuentas de usuario que se encuentra en la columna izquierda. Esto le llevará a la `~/Membership/CreatingUserAccounts.aspx`. Puesto que la `Web.config` un archivo en el `Membership` carpeta define las reglas de autorización para prohibir el acceso anónimo, la `UrlAuthorizationModule` anula la solicitud y devuelve un estado HTTP 401 no autorizado. El `FormsAuthenticationModule` modifica esto en un estado de redireccionamiento 302, enviarnos a la página de inicio de sesión. Tenga en cuenta que la página se nos ha intentando acceder a (`CreatingUserAccounts.aspx`) se pasa a la página de inicio de sesión a través de la `ReturnUrl` parámetro de cadena de consulta.


[![Desde la dirección URL de autorización reglas prohibir el acceso anónimo, estamos redirige a la página de inicio de sesión](user-based-authorization-cs/_static/image14.png)](user-based-authorization-cs/_static/image13.png)

**Figura 5**: puesto que la autorización de dirección URL reglas prohibir el acceso anónimo, nos estamos redirigirá a la página de inicio de sesión ([haga clic aquí para ver la imagen a tamaño completo](user-based-authorization-cs/_static/image15.png))


Tras iniciar sesión correctamente, estamos redirige a la `CreatingUserAccounts.aspx` página. Esta vez el `UrlAuthorizationModule` permite el acceso a la página porque ya no son anónimos.

### <a name="applying-url-authorization-rules-to-a-specific-location"></a>Aplicar reglas de autorización de dirección URL a una ubicación específica

La configuración de autorización definida en el `<system.web>` sección de `Web.config` se aplican a todos los recursos ASP.NET en ese directorio y sus subdirectorios (hasta que lo contrario, se reemplaza por otro `Web.config` archivo). Sin embargo, en algunos casos, puede que convenga todos los recursos ASP.NET en un directorio determinado para tener una configuración de autorización determinada excepto uno o dos páginas específicas. Esto puede lograrse mediante la adición de un `<location>` elemento `Web.config`, haga que apunte al archivo difieren cuyas reglas de autorización y definir sus reglas de autorización única en él.

Para ilustrar con el `<location>` elemento para reemplazar los valores de configuración para un recurso concreto, vamos a personalizar la configuración de autorización para que puede visitar solo Tito `CreatingUserAccounts.aspx`. Para ello, agregue un `<location>` elemento a la `Membership` la carpeta `Web.config` de archivos y actualizar su marcado para que tenga un aspecto similar al siguiente:

[!code-xml[Main](user-based-authorization-cs/samples/sample5.xml)]

El `<authorization>` elemento `<system.web>` define las reglas de autorización de dirección URL predeterminada para los recursos ASP.NET en el `Membership` carpeta y sus subcarpetas. El `<location>` elemento nos permite invalidar estas reglas para un recurso concreto. En el marcado anterior la `<location>` referencias del elemento de la `CreatingUserAccounts.aspx` página y especifica su autorización reglas como permitir Tito, pero denegar todos los demás.

Para probar este cambio de autorización, empiece por visitar el sitio Web como un usuario anónimo. Si se intenta visitar cualquier página de la `Membership` carpeta, como `UserBasedAuthorization.aspx`, el `UrlAuthorizationModule` deniegue el acceso a la solicitud y se le redirigirá a la página de inicio de sesión. Después de iniciar sesión como, por ejemplo, Scott, puede visitar cualquier página de la `Membership` carpeta *excepto* para `CreatingUserAccounts.aspx`. Intentando visitar `CreatingUserAccounts.aspx` iniciado sesión como cualquier usuario pero Tito dará como resultado en un intento de acceso no autorizado, le estamos redirigiendo a la página de inicio de sesión.

> [!NOTE]
> El `<location>` elemento debe aparecer fuera de la configuración `<system.web>` elemento. Debe usar otro `<location>` (elemento) para cada recurso cuya configuración de autorización que se desea invalidar.


### <a name="a-look-at-how-theurlauthorizationmoduleuses-the-authorization-rules-to-grant-or-deny-access"></a>Un vistazo a cómo el`UrlAuthorizationModule`usa las reglas de autorización para conceder o denegar acceso

El `UrlAuthorizationModule` determina si autorizar una identidad concreta para una dirección URL determinada mediante el análisis de la autorización de dirección URL de reglas de uno en uno, a partir de la primera de ellas y avanzando hacia abajo. En cuanto se encuentra una coincidencia, se concede o deniega el acceso al usuario, dependiendo de if se encontró la coincidencia en un `<allow>` o `<deny>` elemento. **Si se encuentra ninguna coincidencia, se concede al usuario acceso.** Por consiguiente, si desea restringir el acceso, es esencial que utilice un `<deny>` elemento como el último elemento de la configuración de autorización de dirección URL. **Si se omite un ***`<deny>`*** elemento, todos los usuarios se le concederá acceso.**

Para comprender mejor el proceso utilizado por el `UrlAuthorizationModule` para determinar la entidad, considere el ejemplo de las reglas de autorización de dirección URL que explicamos anteriormente en este paso. La primera regla es un `<allow>` elemento que permite el acceso a Tito y Scott. Las reglas de segundo es un `<deny>` elemento que se deniega el acceso a todos los usuarios. ¿Si visita un usuario anónimo, el `UrlAuthorizationModule` es anónimo comienza pidiendo, Scott o Tito? La respuesta, obviamente, es n, por lo que pasa a la segunda regla. ¿Es anónimo en el conjunto de todos los usuarios? Desde la respuesta aquí es "Sí", la `<deny>` regla se coloca en vigor y el visitante se redirige a la página de inicio de sesión. ¿De forma similar, si está visitando a Jisun, la `UrlAuthorizationModule` inicia consultando es Jisun Scott o Tito? ¿Puesto que no es así, es el `UrlAuthorizationModule` avanza a la segunda pregunta, Jisun está en el conjunto de todos los usuarios? Es, por lo que, también, se deniega el acceso. Por último, si visita Tito, la primera pregunta que plantea la `UrlAuthorizationModule` es una respuesta afirmativa, por lo que Tito se le concede acceso.

Puesto que la `UrlAuthorizationModule` procesos reglas de la autorización de arriba abajo, deteniéndose en cualquier coincidencia, es importante contar con las reglas más específicas preceder a las menos específicas. Es decir, para definir las reglas de autorización que prohíban Jisun y los usuarios anónimos, pero permite que todos los demás usuarios autenticados, podría comenzar con la regla más específica - el uno que afecta al Jisun - y, a continuación, continúe con las reglas específicas menos - los que permitir que todos los demás los usuarios autenticados, pero denegar todos los usuarios anónimos. Las reglas de autorización de dirección URL siguientes implementa esta directiva Denegar primero Jisun y, a continuación, denegar cualquier usuario anónimo. Cualquier usuario autenticado que no sea Jisun se le concederá acceso porque ninguna de estas `<deny>` coincidirán con instrucciones.

[!code-xml[Main](user-based-authorization-cs/samples/sample6.xml)]

## <a name="step-2-fixing-the-workflow-for-unauthorized-authenticated-users"></a>Paso 2: Corregir el flujo de trabajo para que los usuarios no autorizados, autenticados

Como se explicó anteriormente en este tutorial en la vista A la sección de flujo de trabajo de autorización de dirección URL, en cualquier momento una solicitud no autorizada ocurre, el `UrlAuthorizationModule` anula la solicitud y devuelve un estado HTTP 401 no autorizado. Este estado 401 es modificado por el `FormsAuthenticationModule` en un 302 redirigir el estado que se envía al usuario a la página de inicio de sesión. Este flujo de trabajo se produce en cualquier solicitud no autorizada, incluso si el usuario está autenticado.

Devolver un usuario autenticado a la página de inicio de sesión es probable que confundirlos puesto que ya han iniciado sesión en el sistema. Con un poco de trabajo podemos mejorar este flujo de trabajo mediante la redirección de los usuarios autenticados que realizan solicitudes no autorizadas a una página que explica que ha intentado obtener acceso a una página restringida.

Empiece por crear una nueva página ASP.NET en la carpeta raíz de la aplicación web denominada `UnauthorizedAccess.aspx`; no olvide asociar esta página con el `Site.master` página maestra. Después de crear esta página, quite el control de contenido que hace referencia el `LoginContent` ContentPlaceHolder para que la página principal del contenido predeterminado se mostrará. A continuación, agregue un mensaje que explica la situación, es decir, el usuario que intentó obtener acceso a un recurso protegido. Después de agregar este tipo de mensaje, el `UnauthorizedAccess.aspx` marcado declarativo de la página debe ser similar al siguiente:

[!code-aspx[Main](user-based-authorization-cs/samples/sample7.aspx)]

Ahora necesitamos modificar el flujo de trabajo, por lo que si se realiza una solicitud no autorizada por un usuario autenticado se envían a la `UnauthorizedAccess.aspx` página en lugar de la página de inicio de sesión. La lógica que redirige las solicitudes no autorizadas a la página de inicio de sesión se incrusta en un método privado de la `FormsAuthenticationModule` de la clase, por lo que se puede personalizar este comportamiento. Lo que podemos hacer, sin embargo, es agregar nuestra propia lógica a la página de inicio de sesión que se redirige al usuario a `UnauthorizedAccess.aspx`, si es necesario.

Cuando el `FormsAuthenticationModule` redirige un visitante no autorizado a la página de inicio de sesión que se anexe la URL solicitada, no autorizada a la cadena de consulta con el nombre `ReturnUrl`. Por ejemplo, si un usuario no autorizado intenta visitar `OnlyTito.aspx`, `FormsAuthenticationModule` se redirigirá a `Login.aspx?ReturnUrl=OnlyTito.aspx`. Por lo tanto, si se alcanza la página de inicio de sesión por un usuario autenticado con una cadena de consulta que incluye la `ReturnUrl` parámetro, a continuación, se sabe que este usuario no autenticado simplemente intentaron visitar una página que no está autorizada a ver. En tal caso, deseamos redirigir le `UnauthorizedAccess.aspx`.

Para ello, agregue el código siguiente a la página de inicio de sesión `Page_Load` controlador de eventos:

[!code-csharp[Main](user-based-authorization-cs/samples/sample8.cs)]

El código anterior redirige los usuarios autenticados, no autorizados a la `UnauthorizedAccess.aspx` página. Para ver esta lógica en acción, visite el sitio como un visitante anónimo y haga clic en el vínculo crear cuentas de usuario en la columna izquierda. Esto le llevará a la `~/Membership/CreatingUserAccounts.aspx` página, que en el paso 1 se configura para que solo permita el acceso a Tito. Puesto que los usuarios anónimos están prohibidos, la `FormsAuthenticationModule` nos redirige a la página de inicio de sesión.

En este momento se son anónimos, de modo que `Request.IsAuthenticated` devuelve `false` y no estamos redirige a `UnauthorizedAccess.aspx`. En su lugar, se muestra la página de inicio de sesión. Inicie sesión como un usuario que no sea Tito, por ejemplo, Bruce. Después de escribir las credenciales adecuadas, el inicio de sesión página redirige nos de nuevo a `~/Membership/CreatingUserAccounts.aspx`. Sin embargo, puesto que esta página solo es accesible para Tito, se no está autorizados para verlo y se devuelven inmediatamente a la página de inicio de sesión. Esta vez, sin embargo, `Request.IsAuthenticated` devuelve `true` (y la `ReturnUrl` existe un parámetro de cadena de consulta), por lo que nos estamos redirige a la `UnauthorizedAccess.aspx` página.


[![Autenticado, los usuarios no autorizados se redirigen a UnauthorizedAccess.aspx](user-based-authorization-cs/_static/image17.png)](user-based-authorization-cs/_static/image16.png)

**Figura 6**: autenticado, se redirige a los usuarios no autorizados `UnauthorizedAccess.aspx` ([haga clic aquí para ver la imagen a tamaño completo](user-based-authorization-cs/_static/image18.png))


Este flujo de trabajo personalizado presenta una experiencia de usuario más sencilla y significativo por cortocircuito el ciclo que se muestra en la figura 2.

## <a name="step-3-limiting-functionality-based-on-the-currently-logged-in-user"></a>Paso 3: Limitar la funcionalidad basada en el usuario que actualmente ha iniciado sesión

Autorización de dirección URL facilita especificar las reglas de autorización general. Como vimos en el paso 1, con autorización de dirección URL se podemos sucinta indicar qué identidades se permiten y cuáles se deniegan de ver una página determinada o todas las páginas en una carpeta. En determinados escenarios, sin embargo, podemos queremos permitir a los usuarios visitar una página, pero limitar la funcionalidad de la página según el usuario visita.

Considere el caso de un sitio Web de comercio electrónico que permite a los visitantes autenticados revisar sus productos. Cuando un usuario anónimo visita la página del producto, se vería la información del producto y no se otorgaría la posibilidad de dejar una revisión. Sin embargo, un usuario autenticado visitar la misma página vería la interfaz de revisión. Si el usuario autenticado no tenía revisado todavía este producto, la interfaz permitiría que enviar una revisión; en caso contrario, mostraría ellos su revisión enviada anteriormente. Además, para aprovechar esta situación un paso de la página del producto puede mostrar información adicional y ofrecen características extendidas para los usuarios que trabajan para la empresa de comercio electrónico. Por ejemplo, la página del producto puede enumerar el inventario en el almacén e incluyen opciones para editar el precio del producto y su descripción cuando visita por un empleado.

Estas reglas de autorización concretos pueden implementarse mediante declaración o mediante programación (o a través de una combinación de ambos). En la siguiente sección veremos cómo implementar la autorización concretos mediante el control LoginView. A continuación, exploraremos técnicas de programación. Antes de que podemos observar aplicar reglas de autorización concretos, sin embargo, primero necesitamos crear una página cuya funcionalidad depende el usuario visita a él.

Vamos a crear una página que enumera los archivos de un directorio concreto dentro de un control GridView. Junto con el nombre de cada archivo, tamaño y otra información de la lista, GridView incluirá dos columnas de LinkButton: uno denominado vista y elimine titulado. Si se hace clic en la vista LinkButton, se mostrará el contenido del archivo seleccionado; Si se hace clic en Eliminar LinkButton, se eliminará el archivo. Vamos a crear inicialmente esta página de forma que su funcionalidad de vista y delete no está disponible para todos los usuarios. En el uso en las secciones de LoginView Control y limitar la funcionalidad mediante programación veremos cómo habilitar o deshabilitar estas características basadas en el usuario visita la página.

> [!NOTE]
> La página ASP.NET que se van a compilar, utiliza un control GridView para mostrar una lista de archivos. Desde este tutorial serie se centra en la autenticación de formularios, autorización, cuentas de usuario y roles, no deseo invierta demasiado tiempo hablar sobre el funcionamiento interno del control GridView. Aunque este tutorial proporciona instrucciones paso a paso específicas para la configuración de esta página, no profundizar en los detalles de por qué se realizaron determinadas opciones o lo tienen determinadas propiedades de efecto en la salida representada. Para realizar un examen completo del control GridView, consulte mi  *[trabajar con datos en ASP.NET 2.0](../../data-access/index.md)*  serie de tutoriales.


Comience abriendo la `UserBasedAuthorization.aspx` un archivo en el `Membership` carpeta y agregar un control GridView a la página denominada `FilesGrid`. En las etiquetas inteligentes del control GridView, haga clic en el vínculo Editar columnas para abrir el cuadro de diálogo de campos. Desde aquí, desactive la casilla de campos de generación automática en la esquina inferior izquierda. A continuación, agregue un botón de selección, un botón Eliminar y dos BoundFields desde la esquina superior izquierda (los botones Seleccionar y eliminar pueden encontrarse en el tipo de CommandField). Establecer el botón Seleccionar `SelectText` propiedad a la vista y el BoundField primera `HeaderText` y `DataField` propiedades al nombre. Establecer el BoundField segundo `HeaderText` propiedad al tamaño en Bytes, su `DataField` propiedad longitud, su `DataFormatString` propiedad {0: N0} y su `HtmlEncode` propiedad en False.

Después de configurar las columnas de GridView, haga clic en Aceptar para cerrar el cuadro de diálogo de campos. En la ventana Propiedades, establezca la GridView `DataKeyNames` propiedad `FullName`. En este momento marcado declarativo del GridView debe ser similar al siguiente:

[!code-aspx[Main](user-based-authorization-cs/samples/sample9.aspx)]

Con el marcado de GridView creado, estamos listos para escribir el código que se recuperan los archivos de un directorio concreto y enlazarlos a GridView. Agregue el código siguiente a la página `Page_Load` controlador de eventos:

[!code-csharp[Main](user-based-authorization-cs/samples/sample10.cs)]

El código anterior usa el [ `DirectoryInfo` clase](https://msdn.microsoft.com/en-us/library/system.io.directoryinfo.aspx) para obtener una lista de los archivos en la carpeta raíz de la aplicación. El [ `GetFiles()` método](https://msdn.microsoft.com/en-us/library/system.io.directoryinfo.getfiles.aspx) devuelve todos los archivos en el directorio como una matriz de [ `FileInfo` objetos](https://msdn.microsoft.com/en-us/library/system.io.fileinfo.aspx), que se enlaza a la GridView. El `FileInfo` objeto tiene una gran variedad de propiedades, como `Name`, `Length`, y `IsReadOnly`, entre otros. Como puede ver en el marcado declarativo, GridView muestra solo la `Name` y `Length` propiedades.

> [!NOTE]
> El `DirectoryInfo` y `FileInfo` clases se encuentran en el [ `System.IO` espacio de nombres](https://msdn.microsoft.com/en-us/library/system.io.aspx). Por lo tanto, necesitará preceder estos nombres de clase con sus nombres de espacio de nombres o hacer que el espacio de nombres importado en el archivo de clase (a través de `using System.IO`).


Tómese un momento a visitar la página a través de un explorador. Mostrará la lista de archivos que residen en el directorio raíz de la aplicación. Al hacer clic en cualquiera de la vista o eliminar LinkButton provocará una devolución de datos, pero no se producirá ninguna acción porque hemos todavía para crear los controladores de eventos necesarios.


[![GridView incluyen los archivos en el directorio raíz de la aplicación Web](user-based-authorization-cs/_static/image20.png)](user-based-authorization-cs/_static/image19.png)

**Figura 7**: GridView incluyen los archivos en el directorio raíz de la aplicación Web ([haga clic aquí para ver la imagen a tamaño completo](user-based-authorization-cs/_static/image21.png))


Necesitamos un medio para mostrar el contenido del archivo seleccionado. Vuelva a Visual Studio y agregue un cuadro de texto denominado `FileContents` por encima de GridView. Establecer su `TextMode` propiedad `MultiLine` y su `Columns` y `Rows` propiedades al 95% y 10, respectivamente.

[!code-aspx[Main](user-based-authorization-cs/samples/sample11.aspx)]

A continuación, cree un controlador de eventos del control de GridView [ `SelectedIndexChanged` eventos](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.selectedindexchanged.aspx) y agregue el código siguiente:

[!code-csharp[Main](user-based-authorization-cs/samples/sample12.cs)]

Este código usa la GridView `SelectedValue` propiedad para determinar el nombre completo del archivo del archivo seleccionado. Internamente, la `DataKeys` hace referencia a la colección con el fin de obtener la `SelectedValue`, por lo que es necesario establecer la GridView `DataKeyNames` propiedad con nombre, tal y como se describió anteriormente en este paso. El [ `File` clase](https://msdn.microsoft.com/en-us/library/system.io.file.aspx) se utiliza para leer contenido del archivo seleccionado en una cadena, que, a continuación, se asigna a la `FileContents` del cuadro de texto `Text` propiedad, con lo que se muestra el contenido del archivo seleccionado en la página.


[![Contenido del archivo seleccionado se muestra en el cuadro de texto](user-based-authorization-cs/_static/image23.png)](user-based-authorization-cs/_static/image22.png)

**Figura 8**: del contenido del archivo seleccionado The se muestra en el cuadro de texto ([haga clic aquí para ver la imagen a tamaño completo](user-based-authorization-cs/_static/image24.png))


> [!NOTE]
> Si ver el contenido de un archivo que contiene el marcado HTML y, a continuación, intente ver o eliminar un archivo, se le notificará una `HttpRequestValidationException` error. Esto ocurre porque en el postback contenido del cuadro de texto se envía al servidor web. De forma predeterminada, ASP.NET genera un `HttpRequestValidationException` error cuando se detecta contenido potencialmente peligroso de devolución de datos, como marcado HTML,. Para deshabilitar este error se produzca, desactivar la validación de solicitudes de la página mediante la adición de `ValidateRequest="false"` a la `@Page` directiva. Para obtener más información sobre las ventajas de la validación de solicitud como así como qué precauciones se debe efectuar cuando deshabilitarlo, lea [validación de solicitud: evitar los ataques de secuencia de comandos](https://asp.net/learn/whitepapers/request-validation/).


Finalmente, agregue un controlador de eventos con el código siguiente para la GridView [ `RowDeleting` eventos](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx):

[!code-csharp[Main](user-based-authorization-cs/samples/sample13.cs)]

El código muestra simplemente el nombre completo del archivo que desea eliminar en el `FileContents` cuadro de texto *sin* realmente eliminando el archivo.


[![Haga clic en el botón Eliminar no se eliminan realmente el archivo](user-based-authorization-cs/_static/image26.png)](user-based-authorization-cs/_static/image25.png)

**Figura 9**: haga clic en el eliminar botón no se eliminan realmente el archivo ([haga clic aquí para ver la imagen a tamaño completo](user-based-authorization-cs/_static/image27.png))


En el paso 1, hemos configurado las reglas de autorización de dirección URL para prohibir que los usuarios anónimos de ver las páginas en el `Membership` carpeta. Para presentar mejor autenticación concretos, vamos a permitir que los usuarios anónimos visitar la `UserBasedAuthorization.aspx` página, pero con una funcionalidad limitada. Para abrir esta página para tener acceso a todos los usuarios, agregue el siguiente `<location>` elemento a la `Web.config` un archivo en el `Membership` carpeta:

[!code-xml[Main](user-based-authorization-cs/samples/sample14.xml)]

Después de agregar esta `<location>` elemento, probar las nuevas reglas de autorización de dirección URL al cerrar sesión en el sitio. Como usuario anónimo debe permitirse para visitar la `UserBasedAuthorization.aspx` página.

Actualmente, cualquier usuario autenticado o anónimo puede optar por visitar el `UserBasedAuthorization.aspx` página y ver o eliminar archivos. Vamos a hacer para que sólo los usuarios autenticados pueden ver el contenido de un archivo y Tito sólo puede eliminar un archivo. Estas reglas de autorización concretos pueden aplicarse mediante declaración, mediante programación o mediante una combinación de ambos métodos. Vamos a usar el enfoque declarativo para limitar quién puede ver el contenido de un archivo; vamos a usar el enfoque de programación para limitar quién puede eliminar un archivo.

### <a name="using-the-loginview-control"></a>Uso del Control LoginView

Como hemos visto en tutoriales anteriores, el control LoginView resulta útil para mostrar de distintas interfaces para los usuarios autenticados y anónimos y ofrece una sencilla forma para ocultar la funcionalidad que no está accesible a usuarios anónimos. Puesto que los usuarios anónimos no pueden ver o eliminar archivos, solo es necesario mostrar la `FileContents` cuadro de texto cuando se visita la página por un usuario autenticado. Para ello, agregue un control LoginView a la página, asígnele el nombre `LoginViewForFileContentsTextBox`y mover el `FileContents` marcado declarativo del cuadro de texto en el control LoginView `LoggedInTemplate`.

[!code-aspx[Main](user-based-authorization-cs/samples/sample15.aspx)]

Los controles Web de plantillas de LoginView ya no sean directamente accesibles desde la clase de código subyacente. Por ejemplo, el `FilesGrid` de GridView `SelectedIndexChanged` y `RowDeleting` hacen referencia actualmente a controladores de eventos el `FileContents` control de cuadro de texto con código, como:

[!code-csharp[Main](user-based-authorization-cs/samples/sample16.cs)]

Sin embargo, este código ya no es válido. Moviendo la `FileContents` cuadro de texto en el `LoggedInTemplate` el cuadro de texto no son accesibles directamente. En su lugar, debemos usar la `FindControl("controlId")` método al que hace referencia al control mediante programación. Actualizar el `FilesGrid` controladores de eventos para hacer referencia al cuadro de texto de este modo:

[!code-csharp[Main](user-based-authorization-cs/samples/sample17.cs)]

Después de mover el cuadro de texto a la LoginView `LoggedInTemplate` y actualizando el código de la página de referencia del cuadro de texto utilizando la `FindControl("controlId")` de patrón, visite la página como usuario anónimo. Como se muestra en la figura 10, la `FileContents` no se muestra el cuadro de texto. Sin embargo, se sigue mostrando el LinkButton de vista.


[![El Control LoginView representa sólo el cuadro de texto FileContents para usuarios autenticados](user-based-authorization-cs/_static/image29.png)](user-based-authorization-cs/_static/image28.png)

**Figura 10**: The LoginView Control sólo presenta la `FileContents` cuadro de texto para los usuarios autenticados ([haga clic aquí para ver la imagen a tamaño completo](user-based-authorization-cs/_static/image30.png))


Es una manera de ocultar el botón de vista para los usuarios anónimos convertir el campo de GridView en TemplateField. Esto generará una plantilla que contiene el marcado declarativo para la vista LinkButton. A continuación, podemos agregar un control LoginView a TemplateField y coloque el LinkButton dentro el LoginView `LoggedInTemplate`, con lo que se oculta el botón de vista de visitantes anónimos. Para ello, haga clic en el vínculo Editar columnas etiqueta de GridView inteligente para iniciar el cuadro de diálogo de campos. A continuación, seleccione el botón de selección de la lista en la esquina inferior izquierda y, a continuación, haga clic en la función Convert este campo para un vínculo TemplateField. Si lo hace, se modificará el marcado declarativo del campo de:

[!code-aspx[Main](user-based-authorization-cs/samples/sample18.aspx)]

 A: 

[!code-aspx[Main](user-based-authorization-cs/samples/sample19.aspx)]

En este momento, podemos agregar una LoginView a TemplateField. El siguiente marcado muestra LinkButton vista solo para usuarios autenticados.

[!code-aspx[Main](user-based-authorization-cs/samples/sample20.aspx)]

Como se muestra en la figura 11, el resultado final no es que bastante como la vista de columna se sigue mostrando aunque estén ocultos el LinkButton vista dentro de la columna. Se consultará cómo ocultar toda la columna de GridView (y no solo la LinkButton) en la sección siguiente.


[![El Control LoginView oculta la LinkButton de vista para los visitantes anónimos](user-based-authorization-cs/_static/image32.png)](user-based-authorization-cs/_static/image31.png)

**Figura 11**: el Control LoginView oculta la LinkButton de vista para los visitantes anónimos ([haga clic aquí para ver la imagen a tamaño completo](user-based-authorization-cs/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Limitar la funcionalidad mediante programación

En algunas circunstancias, técnicas declarativas son insuficientes para limitar la funcionalidad a una página. Por ejemplo, la disponibilidad de cierta funcionalidad de página puede depender de criterios más allá de si el usuario visita la página es anónimo o autenticado. En tales casos, los distintos elementos de interfaz de usuario pueden mostrar u ocultos a través de medios mediante programación.

Con el fin de limitar la funcionalidad mediante programación, es preciso realizar dos tareas:

1. Determinar si el usuario visita la página puede tener acceso a la funcionalidad, y
2. Modificar mediante programación la interfaz de usuario en función de si el usuario tiene acceso a la funcionalidad en cuestión.

Para mostrar la aplicación de estas dos tareas, vamos a permitir solo Tito eliminar archivos de GridView. A continuación, es la primera tarea determinar si se trata de Tito visitar la página. Una vez que se ha determinado, es necesario ocultar columna de eliminación de GridView (o mostrar). Las columnas de GridView son accesibles a través de su `Columns` propiedad; solo se representa una columna si su `Visible` propiedad está establecida en `true` (valor predeterminado).

Agregue el código siguiente a la `Page_Load` controlador de eventos antes de enlazar los datos en GridView:

[!code-csharp[Main](user-based-authorization-cs/samples/sample21.cs)]

Como se explicó en la [ *una visión general de autenticación mediante formularios* ](../introduction/an-overview-of-forms-authentication-cs.md) tutorial, `User.Identity.Name` devuelve el nombre de identidad. Se corresponde con el nombre de usuario especificado en el control de inicio de sesión. Si es Tito visitar la página, segunda columna de GridView `Visible` propiedad está establecida en `true`; en caso contrario, se establece en `false`. El resultado neto es que cuando alguien distinto de Tito visita la página, otro usuario autenticado o un usuario anónimo, la columna de eliminación no se representa (Véase la figura 12). Sin embargo, cuando Tito visita la página, la columna de Delete está presente (Véase la figura 13).


[![Eliminar columna no es representa al visitado por alguien distinto de Tito (por ejemplo, Bruce)](user-based-authorization-cs/_static/image35.png)](user-based-authorization-cs/_static/image34.png)

**Figura 12**: eliminar la columna no es representa al visitado por alguien distinto de Tito (por ejemplo, Bruce) ([haga clic aquí para ver la imagen a tamaño completo](user-based-authorization-cs/_static/image36.png))


[![Eliminar columna se representa para Tito](user-based-authorization-cs/_static/image38.png)](user-based-authorization-cs/_static/image37.png)

**Figura 13**: eliminar la columna se representa para Tito ([haga clic aquí para ver la imagen a tamaño completo](user-based-authorization-cs/_static/image39.png))


## <a name="step-4-applying-authorization-rules-to-classes-and-methods"></a>Paso 4: Aplicar las reglas de autorización a las clases y métodos

En el paso 3 se no permiten a los usuarios anónimos pueda ver el contenido de un archivo y se prohíbe que eliminar los archivos de todos los usuarios pero Tito. Esto se lograba haciendo ocultar los elementos de la interfaz de usuario asociado para los visitantes no autorizados a través de técnicas de declaración y mediante programación. Para nuestro ejemplo simple, ocultar correctamente los elementos de la interfaz de usuario era sencillos, pero ¿qué hay de sitios más complejos donde puede haber muchas maneras diferentes para realizar la misma funcionalidad? Limitar esa funcionalidad a los usuarios no autorizados, ¿qué ocurre si se olvida ocultar o deshabilitar todos los elementos de interfaz de usuario aplicables?

Es una manera sencilla de asegurarse de que un usuario no autorizado no puede tener acceso a un elemento determinado de la funcionalidad decorar esa clase o método con el [ `PrincipalPermission` atributo](https://msdn.microsoft.com/en-us/library/system.security.permissions.principalpermissionattribute.aspx). Cuando el tiempo de ejecución de .NET utiliza una clase o ejecuta uno de sus métodos, comprueba para asegurarse de que el contexto de seguridad actual tiene permiso para utilizar la clase o el método execute. El `PrincipalPermission` atributo proporciona un mecanismo a través del cual podemos definir estas reglas.

Vamos a muestran cómo utilizar el `PrincipalPermission` atributo en la GridView `SelectedIndexChanged` y `RowDeleting` controladores de eventos para prohibir la ejecución, los usuarios anónimos y usuarios que no sean Tito, respectivamente. Lo que debemos hacer es agregar el atributo apropiado de encima de cada definición de función:

[!code-csharp[Main](user-based-authorization-cs/samples/sample22.cs)]

El atributo para el `SelectedIndexChanged` indica de controlador de eventos que solo los usuarios autenticados puede ejecutar el controlador de eventos, mientras que el atributo en el `RowDeleting` controlador de eventos limita la ejecución a Tito.

Si de algún modo, un usuario que no sea Tito intenta ejecutar la `RowDeleting` controlador de eventos o un usuario no autenticado intenta ejecutar la `SelectedIndexChanged` controlador de eventos, el tiempo de ejecución de .NET se producirá un `SecurityException`.


[![Si el contexto de seguridad no está autorizado para ejecutar el método, se produce una excepción de seguridad](user-based-authorization-cs/_static/image41.png)](user-based-authorization-cs/_static/image40.png)

**Figura 14**: si el contexto de seguridad no está autorizado para ejecutar el método, un `SecurityException` se produce ([haga clic aquí para ver la imagen a tamaño completo](user-based-authorization-cs/_static/image42.png))


> [!NOTE]
> Para permitir que varios contextos de seguridad tener acceso a una clase o método, decore la clase o método con un `PrincipalPermission` atributo para cada contexto de seguridad. Es decir, para permitir Tito y Bruce ejecutar el `RowDeleting` controlador de eventos, agregue *dos* `PrincipalPermission` atributos:


[!code-csharp[Main](user-based-authorization-cs/samples/sample23.cs)]

Además de las páginas ASP.NET, muchas aplicaciones también tienen una arquitectura que incluye varias capas, como la lógica de negocios y niveles de acceso a datos. Estos niveles normalmente se implementan como bibliotecas de clases y proporcionan clases y métodos para realizar la funcionalidad de relacionadas con datos y lógica empresarial. El `PrincipalPermission` atributo es útil para aplicar las reglas de autorización para estos niveles.

Para obtener más información sobre el uso de la `PrincipalPermission` atributo para definir las reglas de autorización sobre clases y métodos, consulte [Scott Guthrie](https://weblogs.asp.net/scottgu/)de entrada de blog [agregar reglas de autorización a empresarial y el uso de las capas de datos `PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Resumen

En este tutorial explicamos cómo aplicar las reglas de autorización basada en usuario. Comenzamos echando un vistazo a ASP. Marco de autorización de dirección URL de NET. Del cada solicitud, el motor ASP.NET `UrlAuthorizationModule` inspecciona las reglas de autorización de dirección URL definidas en la configuración de la aplicación para determinar si la identidad está autorizada para tener acceso al recurso solicitado. En resumen, autorización URL facilita especificar las reglas de autorización para una página específica o para todas las páginas de un directorio concreto.

El marco de autorización de dirección URL aplica las reglas de autorización en forma de página por página. Con la autorización de URL, ya sea la identidad del solicitante está autorizada para tener acceso a un recurso determinado o no. Sin embargo, muchos escenarios, llamar para las reglas de autorización de más concretos. En lugar de definir quién tiene acceso a una página, necesitamos podemos a todos los usuarios que tengan acceso a una página, pero mostrar datos diferentes o para ofrecer una funcionalidad diferente según el usuario visita la página. Autorización de nivel de página normalmente implica ocultar elementos de la interfaz de usuario específico para evitar que usuarios no autorizados tengan acceso a funcionalidad prohibida. Además, es posible usar atributos para restringir el acceso a las clases y la ejecución de sus métodos para determinados usuarios.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Agregar reglas de autorización a Business y capas de datos con`PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Autorización de ASP.NET](https://msdn.microsoft.com/en-us/library/wce3kxhd.aspx)
- [Cambios entre la seguridad de IIS 7 e IIS 6](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Configuración de archivos y subdirectorios específicos](https://msdn.microsoft.com/en-us/library/6hbkh9s7.aspx)
- [Limitar la funcionalidad de modificación de datos según el usuario](../../data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs.md)
- [Tutoriales del Control LoginView](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/loginview.aspx)
- [Descripción de autorizaciones de direcciones URL de IIS 7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)
- [`UrlAuthorizationModule`Documentación técnica](https://msdn.microsoft.com/en-us/library/system.web.security.urlauthorizationmodule.aspx)
- [Trabajar con datos en ASP.NET 2.0](../../data-access/index.md)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es  *[SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Puede ponerse en contacto Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Anterior](validating-user-credentials-against-the-membership-user-store-cs.md)
[Siguiente](storing-additional-user-information-cs.md)
