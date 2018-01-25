---
uid: web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs
title: "Información general sobre la autenticación de formularios (C#) | Documentos de Microsoft"
author: rick-anderson
description: Crear rutas personalizadas
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: de2d65b9-aadc-42ba-abe1-4e87e66521a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: d386a3b6328675fe21f989f8fd36bfc91fc08b32
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="an-overview-of-forms-authentication-c"></a>Información general sobre la autenticación de formularios (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_02_CS.zip) o [descarga de PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial02_FormsAuth_cs.pdf)

> En este tutorial se convertirá de discusión solo hecho a la aplicación; en concreto, veremos implementar autenticación de formularios. La aplicación web, empezamos a construir en este tutorial continuarán se basa en los tutoriales posteriores, tal y como se puede mover de autenticación de formularios simple a la pertenencia y roles.
> 
> Vea este vídeo para obtener más información acerca de este tema: [mediante la autenticación básica de formularios en ASP.NET](# "using-basic-forms-authentication-in-aspnet").


## <a name="introduction"></a>Introducción

En el [tutorial anterior](security-basics-and-asp-net-support-cs.md) analizamos las opciones de cuenta distintos de un usuario, la autenticación y autorización proporcionadas por ASP.NET. En este tutorial se convertirá de discusión solo hecho a la aplicación; en concreto, veremos implementar autenticación de formularios. La aplicación web, empezamos a construir en este tutorial continuarán se basa en los tutoriales posteriores, tal y como se puede mover de autenticación de formularios simple a la pertenencia y roles.

Este tutorial comienza con una información detallada sobre el flujo de trabajo de autenticación de formularios, un tema en, se indicó en el tutorial anterior. A continuación, se creará un sitio Web ASP.NET a través del cual demostrar los conceptos de autenticación de formularios. A continuación, se configurará el sitio para usar autenticación de formularios, cree una página de inicio de sesión simple y vea cómo determinar si un usuario está autenticado y, si es así, el nombre de usuario inició sesión en el código.

Descripción de los formularios de flujo de trabajo de autenticación, habilitarla en una aplicación web y la creación de las páginas de inicio de sesión y cierre de sesión son vitales todos los pasos en la creación de una aplicación ASP.NET que es compatible con cuentas de usuario y autentica a los usuarios a través de una página web. Por ello, y porque estos tutoriales se basan en otro - haría recomendamos trabajar a través de este tutorial en su totalidad antes de pasar al siguiente aunque ya haya tenido experiencia configurar la autenticación de formularios en los proyectos anteriores.

## <a name="understanding-the-forms-authentication-workflow"></a>Descripción del flujo de trabajo de autenticación de formularios

Cuando el tiempo de ejecución ASP.NET procesa una solicitud para un recurso ASP.NET, como una página ASP.NET o servicio Web de ASP.NET, la solicitud genera un número de eventos durante su ciclo de vida. No hay eventos generados al final muy inicial y parte de la solicitud, los que se produce cuando la solicitud es que se autentican y autoriza, un evento que se desencadena en el caso de una excepción no controlada y así sucesivamente. Para ver una lista completa de los eventos, consulte el [eventos del objeto HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication_events.aspx).

*Módulos HTTP* son clases administradas cuyo código se ejecuta en respuesta a un evento determinado en el ciclo de vida de la solicitud. ASP.NET se suministra con un número de módulos HTTP que realizan tareas esenciales en segundo plano. Dos módulos HTTP integrados que son especialmente importantes para nuestro análisis son:

- **[`FormsAuthenticationModule`](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)**: autentica al usuario inspeccionando el vale de autenticación de formularios, que normalmente se incluye en la colección de cookies del usuario. Si no está presente ningún vale de autenticación de formularios, el usuario es anónimo.
- **[`UrlAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)**: determina si el usuario actual está autorizado para tener acceso a la dirección URL solicitada. Este módulo determina la entidad al consultar las reglas de autorización especificadas en archivos de configuración de la aplicación. ASP.NET también incluye el [ `FileAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx) que determina la entidad consultando los archivos solicitados ACL.

El `FormsAuthenticationModule` intenta autenticar al usuario anteriores a la `UrlAuthorizationModule` (y `FileAuthorizationModule`) ejecutando. Si el usuario que realiza la solicitud no está autorizado para tener acceso al recurso solicitado, el módulo de autorización finaliza la solicitud y devuelve una [HTTP 401 no autorizado](http://www.checkupdown.com/status/E401.html) estado. En escenarios de autenticación de Windows, se devuelve el estado HTTP 401 al explorador. Este código de estado hace que el explorador solicitar al usuario sus credenciales a través de un cuadro de diálogo modal. Con autenticación de formularios, sin embargo, el estado HTTP 401 no autorizado nunca se envía al explorador porque FormsAuthenticationModule detecta este estado y modifica para redirigir al usuario a la página de inicio de sesión en su lugar (a través de un [HTTP 302 redirigir](http://www.checkupdown.com/status/E302.html) estado).

Es responsabilidad de la página de inicio de sesión determinar si las credenciales del usuario son válidas y, si es así, para crear un vale de autenticación de formularios y redirigir al usuario a la página estaban intentando visitar. El vale de autenticación se incluye en las solicitudes posteriores a las páginas en el sitio Web, que el `FormsAuthenticationModule` se utiliza para identificar al usuario.


![El flujo de trabajo de autenticación de formularios](an-overview-of-forms-authentication-cs/_static/image1.png)

**Figura 1**: el flujo de trabajo de autenticación de formularios


### <a name="remembering-the-authentication-ticket-across-page-visits"></a>Teniendo en cuenta el vale de autenticación a través de visitas de página

Después de iniciar sesión, se debe enviar el vale de autenticación de formularios al servidor web en cada solicitud para que el usuario continúa en la sesión en mientras exploran el sitio. Normalmente, esto se logra colocando el vale de autenticación en la colección de cookies del usuario. [Las cookies](http://en.wikipedia.org/wiki/HTTP_cookie) son pequeños archivos de texto que residen en el equipo del usuario y se transmiten en los encabezados HTTP en cada solicitud para el sitio Web que creó la cookie. Por lo tanto, una vez creado el vale de autenticación de formularios y almacenado en las cookies del explorador, cada visita posterior a ese sitio envía el vale de autenticación junto con la solicitud, con lo que se identifica al usuario.

Un aspecto de las cookies es su caducidad, que es la fecha y hora en que el explorador descarta la cookie. Cuando expira la cookie de autenticación de formularios, el usuario ya no se autentica y, por tanto, se convierten en anónimo. Cuando un usuario visita desde un terminal público, lo más probable es que deseen el vale de autenticación a punto de expirar cuando cierre su explorador. Cuando visita desde casa, sin embargo, ese mismo usuario que le interese el vale de autenticación se conserven entre los distintos reinicios de explorador para que no tienen que volver a iniciar sesión cada vez que visiten el sitio. Esta decisión se toma a menudo por el usuario en forma de un «recordar mi cuenta» casilla en la página de inicio de sesión. En el paso 3, examinaremos cómo implementar una casilla "Recordar cuenta" en la página de inicio de sesión. El tutorial siguiente trata los valores de tiempo de espera de vale de autenticación en detalle.

> [!NOTE]
> Es posible que el agente de usuario usado para iniciar sesión en el sitio Web no puede admitir cookies. En tal caso, ASP.NET puede usar los vales de autenticación de formularios sin cookies. En este modo, se codifica el vale de autenticación en la dirección URL. Veremos cuando se usan los vales de autenticación sin cookies y cómo se crean y administran en el tutorial siguiente.


### <a name="the-scope-of-forms-authentication"></a>El ámbito de autenticación de formularios

El `FormsAuthenticationModule` es el código administrado que forma parte de la ejecución de ASP.NET. Antes de la versión 7 de Microsoft [Internet Information Services (IIS)](https://www.iis.net/) servidor web, se produjo una barrera distinta entre la canalización HTTP de IIS y la canalización del runtime ASP.NET. En resumen, en IIS 6 y versiones anteriores, el `FormsAuthenticationModule` solo se ejecuta cuando se delega una solicitud de IIS para el tiempo de ejecución ASP.NET. De forma predeterminada, IIS procesa contenido estático, como páginas HTML y CSS y archivos de imagen: y entrega solo las solicitudes para el tiempo de ejecución ASP.NET cuando se solicita una página con la extensión .aspx, .asmx y .ashx.

Sin embargo, IIS 7, permite para IIS integrada y ASP.NET canalizaciones. Con algunos valores de configuración, puede configurar IIS 7 para invocar FormsAuthenticationModule para *todos los* las solicitudes. Además, con IIS 7 puede definir reglas de autorización de dirección URL para los archivos de cualquier tipo. Para obtener más información, consulte [cambios entre IIS 6 y la seguridad de IIS7](https://www.iis.net/learn/get-started/whats-new-in-iis-7/changes-in-security-between-iis-60-and-iis-7-and-above), [la seguridad de plataforma Web](https://www.iis.net/learn/get-started/whats-new-in-iis-7/iis7-and-above-security-improvements), y [autorizaciones de direcciones URL de IIS7 descripción](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization).

Artículo largo corto, en las versiones anteriores de IIS 7, sólo puede utilizar la autenticación de formularios para proteger los recursos manejados por el tiempo de ejecución ASP.NET. Del mismo modo, las reglas de autorización de dirección URL solo se aplican a recursos manejados por el tiempo de ejecución ASP.NET. Pero con IIS 7 es posible integrar el FormsAuthenticationModule y UrlAuthorizationModule en la canalización HTTP de IIS, ampliando así esta funcionalidad en todas las solicitudes.

## <a name="step-1-creating-an-aspnet-website-for-this-tutorial-series"></a>Paso 1: Crear un sitio Web ASP.NET para esta serie de tutoriales

Para llegar a la audiencia más amplia posible, el sitio Web ASP.NET se va a crear a lo largo de esta serie se creará con la versión gratuita de Microsoft de Visual Studio 2008, [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Implementaremos el `SqlMembershipProvider` almacén del usuario en un [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) base de datos. Si está utilizando Visual Studio 2005 o una edición diferente de Visual Studio 2008 o SQL Server, no se preocupe: los pasos serán casi idénticos y se indicará cualquier diferencia no trivial.

> [!NOTE]
> La aplicación web de demostración usada en cada tutorial está disponible como una descarga. Esta aplicación descargable se creó con Visual Web Developer 2008 destinado a la versión 3.5 de .NET Framework. Dado que la aplicación está destinada a .NET 3.5, su archivo Web.config incluye elementos de configuración adicionales, 3.5 específica. En resumen, si todavía tiene que instalar .NET 3.5 en el equipo, a continuación, la aplicación web que se pueden descargar no funcionará sin quitar primero el marcado específico del 3.5 de Web.config.


Antes de que podemos configurar autenticación de formularios, primero es necesario un sitio Web ASP.NET. Comience creando un nuevo archivo basado en sistema sitio Web de ASP.NET. Para ello, inicie Visual Web Developer y, a continuación, vaya al menú archivo y elija Nuevo sitio Web, mostrar el cuadro de diálogo nuevo sitio Web. Elija la plantilla de sitio Web ASP.NET, o establecer la lista de la lista desplegable de ubicación al sistema de archivos, elija una carpeta para colocar el sitio web y establecer el idioma en C#. Esto creará un nuevo sitio web con una página Default.aspx de ASP.NET, una aplicación\_carpeta de datos y un archivo Web.config.

> [!NOTE]
> Visual Studio admite dos modos de administración de proyectos: proyectos de sitios Web y proyectos de aplicación Web. Proyectos de sitios Web no tienen un archivo de proyecto, mientras que los proyectos de aplicación Web imitar la arquitectura de proyecto en Visual Studio .NET 2002/2003: incluir un archivo de proyecto y compilar código fuente del proyecto en un único ensamblado, que se coloca en la carpeta/bin. Visual Studio 2005 inicialmente solo compatibles proyectos de sitio Web, aunque el modelo de proyecto de aplicación Web se vuelve a producirse con Service Pack 1; Visual Studio 2008 ofrece dos modelos de proyecto. Visual Web Developer 2005 y 2008 ediciones, sin embargo, solo admiten proyectos de sitios Web. Va a utilizar el modelo de proyecto de sitio Web. Si está usando una edición no sean Express y desea utilizar el [modelo de proyecto de aplicación Web](https://msdn.microsoft.com/library/aa730880%28vs.80%29.aspx) en su lugar, no dude en hacerlo, pero tenga en cuenta que puede haber algunas discrepancias entre lo que ve en la pantalla y los pasos que debe seguir en comparación con el capturas de pantalla se muestra y las instrucciones proporcionadas en estos tutoriales.


[![Crear un nuevo archivo basado en sistema sitio Web](an-overview-of-forms-authentication-cs/_static/image3.png)](an-overview-of-forms-authentication-cs/_static/image2.png)

**Figura 2**: crear un sitio Web de New File System-Based ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-forms-authentication-cs/_static/image4.png))


### <a name="adding-a-master-page"></a>Agregar una página maestra

A continuación, agregue una nueva página maestra al sitio en el directorio raíz y denominado Site.master. [Páginas maestras](https://msdn.microsoft.com/library/wtxbf3hh.aspx) permiten a un desarrollador de la página Definir una plantilla de todo el sitio que se pueden aplicar a las páginas ASP.NET. La principal ventaja de las páginas maestras es que aspecto general del sitio se puede definir en una sola ubicación, por tanto, lo que facilita al actualizar o cambiar el diseño del sitio.


[![Agregar una página maestra denominada Site.master para el sitio Web](an-overview-of-forms-authentication-cs/_static/image6.png)](an-overview-of-forms-authentication-cs/_static/image5.png)

**Figura 3**: agregar un Site.master de con el nombre de página maestra al sitio Web ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-forms-authentication-cs/_static/image7.png))


Definir el diseño de página de todo el sitio aquí en la página maestra. Puede usar la vista de diseño y agregar los controles Web o de diseño necesarios, o puede agregar manualmente el marcado manualmente en la vista del origen. Estructura de diseño de la página maestra para imitar el diseño utilizado en mi  *[trabajar con datos en ASP.NET 2.0](../../data-access/index.md)*  serie de tutoriales (consulte la figura 4). Utiliza la página maestra [hojas de estilos en cascada](http://www.w3schools.com/css/default.asp) para colocar y estilos con las opciones de CSS definidas en el archivo Style.css (que se incluye en la descarga asociado de este tutorial). Mientras no se puede indicar en el marcado que se muestra a continuación, se definen las reglas de CSS que el panel de navegación &lt;div&gt;del contenido es una posición absoluta para que aparece a la izquierda y tienen un ancho fijo de 200 píxeles.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample1.aspx)]

Una página maestra define el diseño de página estáticos y las áreas que se pueden editar mediante las páginas ASP.NET que usan la página maestra. Estas regiones modificables contenidas se indican mediante el `ContentPlaceHolder` control, que puede verse dentro del contenido &lt;div&gt;. Esta página principal tiene una sola `ContentPlaceHolder` (MainContent), pero la página principal puede tener varios ContentPlaceHolders.

Con el marcado especificado con anterioridad, cambiar a la vista de diseño muestra el diseño de la página maestra. Todas las páginas ASP.NET que usan esta página principal tendrán este diseño uniforme, con la capacidad para especificar el marcado para la `MainContent` región.


[![La página principal, cuando se ven a través de la vista de diseño](an-overview-of-forms-authentication-cs/_static/image9.png)](an-overview-of-forms-authentication-cs/_static/image8.png)

**Figura 4**: la página maestra, al ver a través de la vista de diseño ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-forms-authentication-cs/_static/image10.png))


### <a name="creating-content-pages"></a>Crear páginas de contenido

En este momento tenemos una página Default.aspx en nuestro sitio Web, pero no usa la página maestra que acabamos de crear. Aunque es posible manipular el marcado declarativo de una página web que usa una página maestra, si la página no contiene ningún contenido aún es más fácil simplemente eliminar la página y vuelva a agregarlo al proyecto, especificar la página maestra que se usa. Por lo tanto, se inician eliminando Default.aspx desde el proyecto.

A continuación, haga doble clic en el nombre del proyecto en el Explorador de soluciones y elija Agregar un nuevo formulario Web denominado Default.aspx. En esta ocasión, active la casilla "Seleccionar la página principal" y elija la página maestra Site.master en la lista.


[![Agregue una nueva página Default.aspx eligiendo la opción de seleccionar una página maestra](an-overview-of-forms-authentication-cs/_static/image12.png)](an-overview-of-forms-authentication-cs/_static/image11.png)

**Figura 5**: agregar una nueva elección de página Default.aspx para seleccionar una página maestra ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-forms-authentication-cs/_static/image13.png))


![Use la página maestra Site.master](an-overview-of-forms-authentication-cs/_static/image14.png)

**Figura 6**: Use la página maestra Site.master


> [!NOTE]
> Si está utilizando el modelo de proyecto de aplicación Web el cuadro de diálogo Agregar nuevo elemento no incluye una casilla de verificación "Seleccionar la página principal". En su lugar, debe agregar un elemento del tipo "Web Form contenido". Después de elegir la opción "Formulario de contenido Web" y haga clic en Agregar, Visual Studio mostrará el mismo, seleccione un patrón de cuadro de diálogo se muestra en la figura 6.


Marcado declarativo de la página Default.aspx nueva incluye simplemente un @Page directiva especifica la ruta de acceso al maestro de página de archivos y un control de contenido MainContent ContentPlaceHolder de la página maestra.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample2.aspx)]

Por ahora, deje en blanco Default.aspx. Volveremos a él más adelante en este tutorial para agregar contenido.

> [!NOTE]
> Esta página principal incluye una sección para un menú u otra interfaz de navegación. Dicha interfaz se creará en un tutorial posterior.

## <a name="step-2-enabling-forms-authentication"></a>Paso 2: Habilitar la autenticación mediante formularios

Con el sitio Web ASP.NET creado, la siguiente tarea consiste en habilitar la autenticación de formularios. Configuración de autenticación de la aplicación se especifica a través de la [ `<authentication>` elemento](https://msdn.microsoft.com/library/532aee0e.aspx) en el archivo Web.config. El `<authentication>` elemento contiene un atributo con el nombre de modo que especifica el modelo de autenticación utilizado por la aplicación. Este atributo puede tener uno de los siguientes cuatro valores:

- **Windows** : como se describe en el tutorial anterior, cuando una aplicación utiliza la autenticación de Windows es responsabilidad del servidor web para autenticar el visitante y esto normalmente se realiza a través de básica, implícita o integrada de Windows autenticación.
- **Formularios**: los usuarios se autentican a través de un formulario en una página web.
- **Passport**: los usuarios se autentican utilizando Passport Network de Microsoft.
- **Ninguno**– se utiliza ningún modelo de autenticación; todos los visitantes son anónimos.

De forma predeterminada, las aplicaciones ASP.NET utilizar la autenticación de Windows. Para cambiar el tipo de autenticación para la autenticación de formularios, a continuación, es necesario modificar el `<authentication>` atributo de modo del elemento a los formularios.

Si el proyecto aún no contiene un archivo Web.config, agregue uno ahora con el botón secundario en el nombre del proyecto en el Explorador de soluciones, elija Agregar nuevo elemento y, a continuación, agregar un archivo de configuración Web.


[![Si el proyecto no incluye todavía Web.config, agréguelo ahora](an-overview-of-forms-authentication-cs/_static/image16.png)](an-overview-of-forms-authentication-cs/_static/image15.png)

**Figura 7**: si su proyecto Does no todavía incluyen Web.config, agregue ahora ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-forms-authentication-cs/_static/image17.png))


A continuación, busque el `<authentication>` elemento y update para que use la autenticación de formularios. Después de este cambio, el marcado del archivo Web.config debe ser similar al siguiente:

[!code-xml[Main](an-overview-of-forms-authentication-cs/samples/sample3.xml)]

> [!NOTE]
> Puesto que el archivo Web.config es un archivo XML, mayúsculas y minúsculas son importante. Asegúrese de que establezca el atributo mode a Forms, con una letra mayúscula "F". Si usas un distinguen mayúsculas de minúsculas, por ejemplo, "forms", recibirá un error de configuración al visitar el sitio a través de un explorador.


El `<authentication>` elemento puede incluir opcionalmente un `<forms>` elemento secundario que contiene la configuración específica de la autenticación de formularios. Por ahora, vamos a utilizar la configuración de autenticación de formularios de forma predeterminada. A continuación se explican los `<forms>` elemento secundario con más detalle en el siguiente tutorial.

## <a name="step-3-building-the-login-page"></a>Paso 3: Creación de la página de inicio de sesión

Para admitir la autenticación de formularios nuestro sitio Web necesita una página de inicio de sesión. Como se describe en la sección "Understanding the Forms autenticación flujo de trabajo", la `FormsAuthenticationModule` automáticamente redirigir al usuario a la página de inicio de sesión si éstos intentan tener acceso a una página que no son autorizará para ver. También hay controles Web de ASP.NET que se mostrarán un vínculo a la página de inicio de sesión para los usuarios anónimos. Esto plantea la pregunta "¿Cuál es la dirección URL de la página de inicio de sesión?"

De forma predeterminada, el sistema de autenticación de formularios espera la página de inicio de sesión se denomine Login.aspx y lo coloca en el directorio raíz de la aplicación web. Si desea usar una URL de la página de inicio de sesión diferente, puede hacerlo mediante la especificación en el archivo Web.config. Veremos cómo hacerlo en el tutorial posterior.

La página de inicio de sesión tiene tres responsabilidades:

1. Proporcionar una interfaz que permite el visitante que escriba sus credenciales.
2. Determinar si las credenciales presentadas son válidas.
3. "Inicie sesión" el usuario mediante la creación de los formularios de vale de autenticación.

### <a name="creating-the-login-pages-user-interface"></a>Crear la interfaz de usuario de la página de inicio de sesión

Vamos a empezar a trabajar con la primera tarea. Agregue una nueva página ASP.NET para el directorio raíz del sitio y denominado Login.aspx y asociarla a la página maestra Site.master.


[![Agregue una nueva página de ASP.NET denominada Login.aspx](an-overview-of-forms-authentication-cs/_static/image19.png)](an-overview-of-forms-authentication-cs/_static/image18.png)

**Figura 8**: agregar una página de ASP.NET nuevo denominado Login.aspx ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-forms-authentication-cs/_static/image20.png))


La interfaz de la página de inicio de sesión típico consta de dos cuadros de texto: uno para el nombre del usuario, uno para su contraseña y un botón para enviar el formulario. Sitios Web a menudo incluye una casilla «Recordar mi cuenta» que, si se activa, continúa el vale de autenticación resultante entre los distintos reinicios de explorador.

Agregar dos cuadros de texto al Login.aspx y establezca sus `ID` propiedades de nombre de usuario y una contraseña, respectivamente. Establecer la contraseña `TextMode` propiedad con contraseña. A continuación, agregue un control de casilla de verificación, establecer su `ID` propiedad RememberMe y su `Text` propiedad en "Recordar mi cuenta". A continuación, agregue un botón denominado LoginButton cuya `Text` propiedad se establece en "Iniciar sesión". Y por último, agregue un control Web Label y establezca su `ID` propiedad InvalidCredentialsMessage, su `Text` propiedad a "el nombre de usuario o la contraseña no es válido. Vuelva a intentarlo. ", sus `ForeColor` propiedad a rojo y su `Visible` propiedad en False.

En este momento, la pantalla debe ser similar a la pantalla en la figura 9 y la sintaxis declarativa de la página debe ser similar del siguiente:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample4.aspx)]


[![La página de inicio de sesión contiene dos cuadros de texto, una casilla de verificación, un botón y una etiqueta](an-overview-of-forms-authentication-cs/_static/image22.png)](an-overview-of-forms-authentication-cs/_static/image21.png)

**Figura 9**: el inicio de sesión página contiene dos cuadros de texto, una casilla de verificación, un botón y una etiqueta ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-forms-authentication-cs/_static/image23.png))


Por último, cree un controlador de eventos del LoginButton, haga clic en eventos. En el diseñador, simplemente haga doble clic en el control de botón para crear este controlador de eventos.

### <a name="determining-if-the-supplied-credentials-are-valid"></a>Determinar si las credenciales proporcionados no son válidos

Ahora necesitamos implementar tarea 2 de clic del botón controlador de eventos: determinar si las credenciales proporcionadas son válidas. Para ello debe haber un almacén de usuario que contiene todas las credenciales de los usuarios, por lo que podemos determinar si las credenciales proporcionadas coinciden con las credenciales conocidas.

Antes de ASP.NET 2.0, los desarrolladores son responsables de implementar ambos sus propios almacenes de usuario y escribe el código para validar las credenciales proporcionadas en el almacén. Mayoría de los desarrolladores implementaría el almacén del usuario en una base de datos, crear una tabla con el nombre de los usuarios con las columnas como nombre de usuario, contraseña, correo electrónico, LastLoginDate y así sucesivamente. Esta tabla, a continuación, tendría un registro por cada cuenta de usuario. Comprobar credenciales suministradas del usuario implicaría consultar la base de datos para un nombre de usuario correspondiente y, a continuación, asegurarse de que la contraseña en la base de datos se correspondía con la contraseña proporcionada.

Con ASP.NET 2.0, los desarrolladores deben utilizar uno de los proveedores de pertenencia para administrar el almacén de usuario. En esta serie de tutoriales usaremos SqlMembershipProvider, que utiliza una base de datos de SQL Server para el almacén del usuario. Cuando se usa el SqlMembershipProvider es necesario para implementar un esquema de base de datos específica que incluye tablas, vistas y procedimientos almacenados esperados por el proveedor. Examinaremos cómo implementar este esquema en el ***crear el esquema de pertenencia en SQL Server*** tutorial. Con el proveedor de pertenencia en su lugar, validar las credenciales del usuario es tan sencillo como llamar a la [clase de pertenencia](https://msdn.microsoft.com/library/system.web.security.membership.aspx)del [ValidateUser (*nombre de usuario*, *contraseña*) método](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx), que devuelve un valor booleano que indica si la validez de la *nombre de usuario* y *contraseña* combinación. Ver como aún no hemos implementado almacén del usuario del SqlMembershipProvider, no podemos usar pertenencia ValidateUser método de la clase en este momento.

En lugar de dedicar tiempo a crear nuestra propia tabla personalizada de la base de datos de los usuarios (que podría ser obsoleta, una vez que se implementa el SqlMembershipProvider), vamos a en su lugar, codificar de forma rígida las credenciales válidas en el inicio de sesión página propio. En la LoginButton haga clic en el controlador de eventos, agregue el código siguiente:

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample5.cs)]

Como puede ver, hay tres cuentas de usuario válidas: Scott, Jisun y Sam – y los tres también presentan la misma contraseña (""). El código recorre las matrices de usuarios y contraseñas busca una coincidencia de nombre de usuario y una contraseña válida. Si el nombre de usuario y la contraseña son válidos, es necesario iniciar sesión el usuario y, a continuación, redirigirá a la página apropiada. Si las credenciales son válidas, se muestra la etiqueta InvalidCredentialsMessage.

Cuando un usuario introduce credenciales válidas, he mencionado que, a continuación, se le redirige a la "página adecuada". ¿Qué es la página correspondiente, aunque? Recuerde que cuando un usuario visita una página que no están autorizados para ver, FormsAuthenticationModule automáticamente redirigirá a la página de inicio de sesión. Si lo hace, incluye la dirección URL solicitada en la cadena de consulta a través del parámetro ReturnUrl. Es decir, si un usuario ha intentado visitar ProtectedPage.aspx y que no tenían autorización para ello, FormsAuthenticationModule redirigiría puedan:

Login.aspx?ReturnUrl=ProtectedPage.aspx

Tras iniciar sesión correctamente, el usuario se debe redirigir a ProtectedPage.aspx. Como alternativa, los usuarios pueden visitar la página de inicio de sesión en su propios volition. En ese caso, después de iniciar sesión el usuario se deberían enviar a la página Default.aspx de la carpeta raíz.

### <a name="logging-in-the-user"></a>Inicio de sesión del usuario

Suponiendo que las credenciales proporcionadas son válidas, es necesario crear un vale de autenticación de formularios, inicio de sesión, por tanto, el usuario para el sitio. El [clase FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthentication.aspx) en el [espacio de nombres System.Web.Security](https://msdn.microsoft.com/library/system.web.security.aspx) proporciona métodos ordenados para el registro de inicio de sesión a los usuarios a través de los formularios y en el sistema de autenticación. Aunque existen varios métodos de la clase FormsAuthentication, son los tres que nos interesa en ese momento:

- [GetAuthCookie (*nombre de usuario*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.getauthcookie.aspx) – crea un vale de autenticación de formularios para el nombre proporcionado *nombre de usuario*. A continuación, este método crea y devuelve un objeto HttpCookie que incluye el contenido del vale de autenticación. Si *persistCookie* es true, se crea una cookie persistente.
- [SetAuthCookie (*nombre de usuario*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) : llama a la GetAuthCookie (*nombre de usuario*, *persistCookie*) método para generar la cookie de autenticación de formularios. Este método, a continuación, agrega la cookie devuelta por GetAuthCookie a la colección de Cookies (suponiendo que la autenticación de formularios basados en cookies se usa; en caso contrario, este método llama a una clase interna que controla la lógica de vale sin cookies).
- [RedirectFromLoginPage (*nombre de usuario*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.redirectfromloginpage.aspx) : llama a este método SetAuthCookie (*nombre de usuario*, *persistCookie* ) y, a continuación, redirige al usuario a la página apropiada.

GetAuthCookie resulta útil cuando necesita modificar el vale de autenticación antes de escribir la cookie a la colección de Cookies. SetAuthCookie es útil si desea crear los formularios de vale de autenticación y agregarlo a la colección de Cookies, pero no desea redirigir al usuario a la página apropiada. Es posible que desee mantenerlos en la página de inicio de sesión o enviarlos a alguna página alternativa.

Puesto que deseamos iniciar sesión en el usuario y redirigirá a la página adecuada, vamos a usar RedirectFromLoginPage. Actualización haga clic en del LoginButton controlador de eventos, reemplazando las dos líneas comentadas de tareas con la siguiente línea de código:

FormsAuthentication.RedirectFromLoginPage(UserName.Text, RememberMe.Checked);

Al crear los formularios de vale de autenticación usamos Text (propiedad) de UserName TextBox para el vale de autenticación de formularios *nombre de usuario* parámetro y el estado de activación de RememberMe CheckBox para el  *persistCookie* parámetro.

Para probar la página de inicio de sesión, visite en un explorador. Primero escriba credenciales no válidas, como un nombre de usuario de "Nope" y la contraseña de "error". Al hacer clic en el botón de inicio de sesión se realizará una devolución de datos y se mostrará la etiqueta InvalidCredentialsMessage.


[![La etiqueta de InvalidCredentialsMessage es muestran al escribir las credenciales no válidas](an-overview-of-forms-authentication-cs/_static/image25.png)](an-overview-of-forms-authentication-cs/_static/image24.png)

**Figura 10**: la etiqueta de InvalidCredentialsMessage es muestran al escribir las credenciales no válidas ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-forms-authentication-cs/_static/image26.png))


A continuación, escriba credenciales válidas y haga clic en el botón de inicio de sesión. Esta vez cuando la devolución de datos produce un vale de autenticación de formularios se crea y se le redirigirá automáticamente a Default.aspx. En este momento han iniciado sesión el sitio Web, si bien no hay ningún indicaciones visuales para indicar que han iniciado sesión. En el paso 4, que veremos cómo determinar mediante programación si un usuario se registra en o no, así como identificar el usuario visita la página.

Paso 5 examina las técnicas para registrar un usuario en el sitio Web.

### <a name="securing-the-login-page"></a>Protección de la página de inicio de sesión

Cuando el usuario escribe sus credenciales y envía el formulario de la página de inicio de sesión, las credenciales, incluida su contraseña, se transmiten a través de Internet al servidor web en *texto sin formato*. Esto significa que los piratas informáticos examen el tráfico de red pueden ver el nombre de usuario y la contraseña. Para evitar esto, es esencial para cifrar el tráfico de red mediante el uso de [capas de sockets seguros (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Esto garantizará que se cifra las credenciales (así como marcado HTML de la página completa) desde el momento en que dejan el explorador hasta que se reciben en el servidor web.

A menos que su sitio Web contiene información confidencial, sólo necesitará utilizar SSL en la página de inicio de sesión y en otras páginas donde la contraseña del usuario en caso contrario, se envía a través de la conexión en texto sin formato. No es necesario preocuparse de proteger los formularios de vale de autenticación porque, de forma predeterminada, que es tanto cifrado y firmado digitalmente (para evitar la manipulación). Una explicación más exhaustiva sobre la seguridad de vale de autenticación de formularios se presenta en el tutorial siguiente.

> [!NOTE]
> Muchos sitios Web financieros y médicos está configurados para utilizar SSL en *todos los* páginas puede tener acceso a los usuarios autentican. Si va a compilar un sitio Web de este tipo puede configurar el sistema de autenticación de formularios para que el vale de autenticación de formularios sólo se transmite a través de una conexión segura. Pasemos a ver las distintas opciones de configuración de autenticación de formularios en el siguiente tutorial,  *[configuración de autenticación de formularios y temas avanzados](forms-authentication-configuration-and-advanced-topics-cs.md)*.


## <a name="step-4-detecting-authenticated-visitors-and-determining-their-identity"></a>Paso 4: Detectar visitantes autenticados y determinar su identidad

En este punto hemos habilitado la autenticación de formularios y crea una página de inicio de sesión rudimentario, pero todavía tenemos que examinar cómo podemos determinar si un usuario está autenticado o anónimo. En ciertos escenarios podemos deseamos mostrar datos diferentes o información dependiendo de si un usuario autenticado o anónimo está visitando la página. Además, a menudo es necesario conocer la identidad del usuario autenticado.

Vamos a aumentar la página Default.aspx existente para ilustrar estas técnicas. En Default.aspx, agregue dos controles de Panel, un AuthenticatedMessagePanel con nombre y otra AnonymousMessagePanel con nombre. Agregue un control de etiqueta denominado WelcomeBackMessage en el primer Panel. En el segundo Panel, agregar un control de hipervínculo, establezca su propiedad Text a "Iniciar sesión" y su propiedad NavigateUrl en "~ / Login.aspx". En este momento el marcado declarativo para Default.aspx debe ser similar al siguiente:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample6.aspx)]

Como probablemente ha adivinado hasta ahora, la idea aquí es mostrar solo el AuthenticatedMessagePanel visitantes autenticados y simplemente la AnonymousMessagePanel a visitantes anónimos. Para ello, que es necesario establecer propiedades visibles de estos paneles dependiendo de si el usuario ha iniciado sesión o no.

El [Request.IsAuthenticated propiedad](https://msdn.microsoft.com/library/system.web.httprequest.isauthenticated.aspx) devuelve un valor booleano que indica si se autenticó la solicitud. Escriba el código siguiente en la página\_cargar el código del controlador de eventos:

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample7.cs)]

Con este código en su lugar, visite Default.aspx a través de un explorador. Suponiendo que aún no cuenta que iniciar sesión, verá un vínculo a la página de inicio de sesión (consulte la figura 11). Haga clic en este vínculo e inicie sesión en el sitio. Como vimos en el paso 3, después de escribir las credenciales se devolverán a Default.aspx, pero esta vez la página muestra el "Welcome volver!". mensaje (consulte la figura 12).


![Cuando visita forma anónima, un registro de vínculo se muestra](an-overview-of-forms-authentication-cs/_static/image27.png)

**Figura 11**: se muestra cuando visita forma anónima, un registro de vínculo


![Se muestran los usuarios autenticados el](an-overview-of-forms-authentication-cs/_static/image28.png)

**Figura 12**: usuarios autenticados se muestran el "Bienvenido de nuevo!" Mensaje


Podemos determinar identidad del usuario con sesión iniciada actualmente a través de la [objeto HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)del [propiedad de usuario](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx). El objeto HttpContext representa información sobre la solicitud actual y es el punto de partida para tales objetos comunes de ASP.NET como respuesta, la solicitud y la sesión, entre otros. La propiedad de usuario representa el contexto de seguridad de la solicitud HTTP actual y la implementa el [interfaz IPrincipal](https://msdn.microsoft.com/library/system.security.principal.iprincipal.aspx).

FormsAuthenticationModule establece la propiedad de usuario. En concreto, cuando FormsAuthenticationModule encuentra un vale de autenticación de formularios en la solicitud entrante, crea un nuevo objeto GenericPrincipal y lo asigna a la propiedad de usuario.

Objetos de entidad (por ejemplo, GenericPrincipal) proporcionan información sobre la identidad del usuario y los roles a los que pertenecen. La interfaz de IPrincipal define a dos miembros:

- [IsInRole (*roleName*)](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole.aspx) : un método que devuelve un valor booleano que indica si la entidad de seguridad pertenece al rol especificado.
- [Identidad](https://msdn.microsoft.com/library/system.security.principal.iprincipal.identity.aspx) : una propiedad que devuelve un objeto que implementa el [interfaz IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx). La interfaz IIdentity define tres propiedades: [AuthenticationType](https://msdn.microsoft.com/library/system.security.principal.iidentity.authenticationtype.aspx), [IsAuthenticated](https://msdn.microsoft.com/library/system.security.principal.iidentity.isauthenticated.aspx), y [nombre](https://msdn.microsoft.com/library/system.security.principal.iidentity.name.aspx).

Podemos determinar el nombre del visitante actual utilizando el código siguiente:

cadena currentUsersName = User.Identity.Name;

Cuando se utiliza autenticación mediante formularios, un [objeto FormsIdentity](https://msdn.microsoft.com/library/system.web.security.formsidentity.aspx) se crea para la propiedad de identidad de la clase GenericPrincipal. La clase FormsIdentity siempre devuelve la cadena "Forms" para su propiedad AuthenticationType y true para su propiedad IsAuthenticated. La propiedad Name devuelve el nombre de usuario especificado al crear los formularios de vale de autenticación. Además de estas tres propiedades FormsIdentity incluye el acceso para el vale de autenticación subyacente a través de su [propiedad de la ficha](https://msdn.microsoft.com/library/system.web.security.formsidentity.ticket.aspx). La propiedad Ticket devuelve un objeto de tipo [FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx), que tiene propiedades como la expiración, IsPersistent, IssueDate, nombre y así sucesivamente.

El punto importante que hay que considerar es que la *nombre de usuario* los parámetros especificados en la FormsAuthentication.GetAuthCookie (*nombre de usuario*, *persistCookie*), Habrá (*nombre de usuario*, *persistCookie*) y FormsAuthentication.RedirectFromLoginPage (*nombre de usuario*, *persistCookie*) métodos es el mismo valor devuelto por User.Identity.Name. Además, está disponible el vale de autenticación creado por estos métodos por User.Identity se convierte a un objeto FormsIdentity y, a continuación, obtener acceso a la propiedad de vale:

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample8.cs)]

Vamos a proporcionar un mensaje más personalizado en Default.aspx. Actualizar la página\_cargar el controlador de eventos para que la propiedad de texto de la etiqueta WelcomeBackMessage se le asigna la cadena "Bienvenido de nuevo, *nombre de usuario*!"

WelcomeBackMessage.Text = "¡Bienvenido de nuevo" + User.Identity.Name + "!";

Figura 13 muestra el efecto de esta modificación (al iniciar sesión como usuario Scott).


![El mensaje de bienvenida incluye tiene iniciada sesión en nombre del usuario](an-overview-of-forms-authentication-cs/_static/image29.png)

**Figura 13**: el mensaje de bienvenida incluye tiene iniciada sesión en nombre del usuario


### <a name="using-the-loginview-and-loginname-controls"></a>Usar los controles de LoginName y LoginView

Mostrar contenido diferente a los usuarios autenticados y anónimos es un requisito común; por lo que se están mostrando el nombre del usuario que ha iniciado sesión actualmente. Por esta razón, ASP.NET incluye dos controles Web que proporcionan la misma funcionalidad que se muestra en la figura 13, pero sin necesidad de escribir una sola línea de código.

El [control LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) es un control Web basadas en plantillas que facilita el proceso mostrar datos diferentes a los usuarios autenticados y anónimos. El LoginView incluye dos plantillas predefinidas:

- AnonymousTemplate: cualquier código que se agrega a esta plantilla solo se muestra para los visitantes anónimos.
- LoggedInTemplate: se muestra el marcado de la plantilla sólo a los usuarios autenticados.

Vamos a agregar el control LoginView a la página principal de nuestro sitio, Site.master. En lugar de agregar sólo para el control LoginView, sin embargo, vamos a agregar tanto un nuevo control ContentPlaceHolder y, a continuación, coloque el control LoginView dentro de ese ContentPlaceHolder nueva. El motivo de esta decisión serán evidente en breve.

> [!NOTE]
> Además del AnonymousTemplate y LoggedInTemplate, el control LoginView puede incluir plantillas de rol específico. Plantillas específicas de las funciones muestran marcado únicamente a aquellos usuarios que pertenecen a una función especificada. Examinaremos las características del control LoginView basada en roles en un tutorial posterior.


Empiece agregando un ContentPlaceHolder denominado LoginContent en la página maestra en el panel de navegación &lt;div&gt; elemento. Simplemente puede arrastrar un control ContentPlaceHolder del cuadro de herramientas en la vista del origen, colocar el marcado resultante justo encima de la "lista de tareas: menú va aquí..." text.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample9.aspx)]

A continuación, agregue un control LoginView dentro de la LoginContent ContentPlaceHolder. Se consideran contenido que aparece en los controles de la página maestra ContentPlaceHolder *contenido predeterminado* para el control ContentPlaceHolder. Es decir, las páginas ASP.NET que usan esta página principal pueden especificar su propio contenido para cada ContentPlaceHolder o usar el contenido de la página maestra predeterminada.

El LoginView y otros controles relacionados con el inicio de sesión se encuentran en la pestaña de inicio de sesión del cuadro de herramientas.


![El Control LoginView en el cuadro de herramientas](an-overview-of-forms-authentication-cs/_static/image30.png)

**Figura 14**: el Control LoginView en el cuadro de herramientas


A continuación, agregue dos &lt;br /&gt; elementos inmediatamente después del control LoginView, pero aún dentro de la ContentPlaceHolder. En este momento, el panel de navegación &lt;div&gt; marcado del elemento debe ser similar al siguiente:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample10.aspx)]

Plantillas de LoginView pueden definirse en el diseñador o en el marcado declarativo. Desde el Diseñador de Visual Studio, expanda las etiquetas inteligentes de LoginView, que enumeran las plantillas configuradas en una lista desplegable. Escribir el texto "Hola, stranger" en AnonymousTemplate; a continuación, agregue un control de hipervínculo y establezca sus propiedades Text y NavigateUrl para "Iniciar sesión" y "~ / Login.aspx", respectivamente.

Después de configurar AnonymousTemplate, cambie a LoggedInTemplate y escriba el texto, "Aquí está otra vez,". A continuación, arrastre un control LoginName del cuadro de herramientas en LoggedInTemplate, colocándolo inmediatamente después de la "Welcome," texto. El [control LoginName](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginname.aspx), como su nombre implica, muestra el nombre del usuario que actualmente ha iniciado sesión. Internamente, el control LoginName simplemente genera la propiedad User.Identity.Name

Después de realizar estas adiciones a las plantillas de LoginView, el marcado debe ser similar al siguiente:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample11.aspx)]

Con esta adición a la página maestra Site.master, cada página en nuestro sitio Web mostrará un mensaje diferente dependiendo de si el usuario está autenticado. Figura 15 muestra la página Default.aspx al usuario Jisun visitado a través de un explorador. El mensaje "Welcome, Jisun" se repite dos veces: una vez en la sección de exploración de la página principal de la izquierda (mediante el control LoginView que acabamos de agregar) y una vez en el Default.aspx contenido área (a través de controles de Panel y lógica de programación).


![Muestra el Control LoginView](an-overview-of-forms-authentication-cs/_static/image31.png)

**Figura 15**: la muestra de Control LoginView "Bienvenido de nuevo, Jisun."


Porque agregamos la LoginView a la página maestra, puede aparecer en cada página en nuestro sitio. Sin embargo, puede haber páginas web donde no queremos mostrar este mensaje. Una página es la página de inicio de sesión, ya que parece un vínculo a la página de inicio de sesión fuera de lugar no existe. Puesto que el control LoginView se coloca en un control ContentPlaceHolder en la página maestra, es posible invalidar este marcado de forma predeterminada en nuestra página de contenido. Abra Login.aspx y vaya hasta el diseñador. Puesto que no hemos definido explícitamente un control de contenido en Login.aspx para el LoginContent ContentPlaceHolder en la página maestra, la página de inicio de sesión mostrará marcado de la página principal predeterminada para este ContentPlaceHolder. Puede ver esto a través del diseñador: el LoginContent ContentPlaceHolder muestra el marcado de forma predeterminada (el control LoginView).


[![La página de inicio de sesión muestra el valor predeterminado el contenido de LoginContent ContentPlaceHolder la página maestra](an-overview-of-forms-authentication-cs/_static/image33.png)](an-overview-of-forms-authentication-cs/_static/image32.png)

**Figura 16**: la página de inicio de sesión muestra el contenido predeterminado para LoginContent ContentPlaceHolder's Page the Master ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-forms-authentication-cs/_static/image34.png))


Para reemplazar el código predeterminado para el LoginContent ContentPlaceHolder, simplemente haga doble clic en la región en el diseñador y elija la opción de crear contenido personalizado en el menú contextual. (Cuando se usa Visual Studio 2008 el ContentPlaceHolder incluye una etiqueta inteligente que, cuando se selecciona, ofrece la misma opción.) Esto agrega un nuevo control de contenido para el marcado de la página y, por tanto, nos permite definir contenido personalizado de esta página. Puede agregar un mensaje personalizado en este caso, como "Inicie sesión en...", pero vamos a déjelo en blanco.

> [!NOTE]
> En Visual Studio 2005, crear contenido personalizado crea vacío de contenido de control en la página ASP.NET. En Visual Studio 2008, sin embargo, crear contenido personalizado copia contenido de la página maestra predeterminada en el control de contenido recién creado. Si usa Visual Studio 2008, a continuación, después de crear el nuevo control de contenido Asegúrese borrar el contenido que se copia a través de la página maestra.


Figura 17 muestra la página Login.aspx cuando visita desde un explorador después de realizar este cambio. Tenga en cuenta que no hay ninguna "Hola, stranger" o "Bienvenido de nuevo, *nombre de usuario*" mensaje en el panel de navegación izquierdo &lt;div&gt; como cuando visita a Default.aspx.


[![La página de inicio de sesión oculta marcado de LoginContent ContentPlaceHolder el valor predeterminado](an-overview-of-forms-authentication-cs/_static/image36.png)](an-overview-of-forms-authentication-cs/_static/image35.png)

**Figura 17**: la página de inicio de sesión oculta marcado del valor predeterminado LoginContent ContentPlaceHolder ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-forms-authentication-cs/_static/image37.png))


## <a name="step-5-logging-out"></a>Paso 5: Cerrar la sesión

En el paso 3 se buscan en la creación de una página de inicio de sesión para iniciar sesión un usuario en el sitio, pero todavía tenemos que ver cómo cerrar la sesión un usuario. Además de métodos para un usuario en el registro, la clase FormsAuthentication también proporciona un [método SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx). El método de cierre de sesión simplemente destruye el vale de autenticación de formularios, con lo que el registro del usuario en el sitio.

Ofrece que una vínculo de la sesión es una característica tan comunes que ASP.NET incluye un control diseñado específicamente para cerrar la sesión un usuario. El [control LoginStatus](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginstatus.aspx) muestra un LinkButton "Iniciar sesión" o un LinkButton "Cierre de sesión", según el estado de autenticación del usuario. Un LinkButton "Inicio de sesión" se representa para los usuarios anónimos, mientras que un LinkButton "Cierre de sesión" se muestra a los usuarios autenticados. El texto para el "Inicio de sesión" y "Cierre de sesión" LinkButton puede configurarse a través de la LoginStatus LoginText y LogoutText propiedades.

Al hacer clic en "Iniciar sesión" LinkButton provoca una devolución de datos, desde el que se emite una redirección a la página de inicio de sesión. Al hacer clic en LinkButton "Cierre de sesión" hace que el control LoginStatus invocar el método FormsAuthentication.SignOff y, a continuación, redirige al usuario a una página. La página ha iniciado los usuarios se redirigen a depende de la propiedad LogoutAction, que puede asignarse a uno de los tres valores siguientes:

- Actualización: el valor predeterminado; redirige al usuario a la página que solo se visitan. Si la página que se encontraban simplemente no permite a los usuarios anónimos, FormsAuthenticationModule automáticamente redirigirá al usuario a la página de inicio de sesión.

Es posible que tiene curiosidad por qué se realiza un redireccionamiento aquí. Si el usuario desea permanecer en la misma página, ¿por qué la necesidad de la redirección explícita? La razón es que cuando se hace clic en "Cerrar sesión" LinkButton, el usuario todavía tiene el vale de autenticación de formularios en su colección de cookies. Por lo tanto, la solicitud de devolución de datos es una solicitud autenticada. El control LoginStatus llama al método de cierre de sesión, pero que se produce una vez FormsAuthenticationModule ha autenticado al usuario. Por lo tanto, un redireccionamiento explícito hace que el explorador volverá a solicitar la página. En el momento en que el explorador solicita volver a la página, se ha quitado el vale de autenticación de formularios y, por tanto, la solicitud entrante es anónima.

- Redirigir: el usuario se redirige a la dirección URL especificada por la propiedad de LogoutPageUrl del LoginStatus.
- RedirectToLoginPage: el usuario se redirige a la página de inicio de sesión.

Vamos a agregar un control LoginStatus a la página maestra y configúrelo para utilizar la opción de redirección para enviar al usuario a una página que muestra un mensaje que confirma que ha cerrado la sesión. Empiece por crear una página en el directorio raíz denominado Logout.aspx. No olvide asociar esta página a la página maestra Site.master. A continuación, escriba un mensaje en el marcado de la página que explique al usuario que se han registrado out.

A continuación, volver a la página maestra Site.master y agregar un control LoginStatus bajo el LoginView en el LoginContent ContentPlaceHolder. Establezca la LoginStatus propiedad del control LogoutAction como redirección y su propiedad LogoutPageUrl en "~ / Logout.aspx".

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample12.aspx)]

Dado que la LoginStatus está fuera del control LoginView, aparecerá para los usuarios autenticados y anónimos, pero eso está bien porque el LoginStatus mostrarán correctamente un LinkButton "Cierre de sesión" o "Inicio de sesión". Con la adición del control LoginStatus, el hipervínculo "Inicio de sesión" en AnonymousTemplate es superfluo, por lo que quitarlo.

La figura 18 muestra Default.aspx cuando visita Jisun. Tenga en cuenta que la columna izquierda muestra el mensaje, "Bienvenido de nuevo, Jisun" junto con un vínculo para cerrar la sesión. Al hacer clic en el registro de salida LinkButton provoca una devolución de datos, Jisun se cerrará si el sistema y, a continuación, redirige llevó a Logout.aspx. Como se muestra en figura 19, en el momento que jisun alcanza Logout.aspx que ya se ha cerrado la sesión y, por tanto, es anónimo. Por lo tanto, la columna izquierda muestra el texto "Welcome stranger" y un vínculo a la página de inicio de sesión.


[![Se muestra en default.aspx](an-overview-of-forms-authentication-cs/_static/image39.png)](an-overview-of-forms-authentication-cs/_static/image38.png)

**Figura 18**: Default.aspx muestra "Bienvenido de nuevo, Jisun" junto con un LinkButton "Cierre de sesión" ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-forms-authentication-cs/_static/image40.png))


[![Muestra Logout.aspx](an-overview-of-forms-authentication-cs/_static/image42.png)](an-overview-of-forms-authentication-cs/_static/image41.png)

**Figura 19**: Logout.aspx muestra "Welcome stranger" junto con un LinkButton "Login" ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-forms-authentication-cs/_static/image43.png))


> [!NOTE]
> Le animo a personalizar la página Logout.aspx para ocultar LoginContent ContentPlaceHolder la página maestra (al igual que hicimos para Login.aspx en el paso 4). La razón es que la LinkButton "Login" representada por el control LoginStatus (uno por debajo de "Hola, stranger") envía al usuario a la página de inicio de sesión pasa la dirección URL actual en el parámetro de cadena de consulta ReturnUrl. En resumen, si hace clic en un usuario que haya cerrado la sesión este LoginStatus LinkButton "Login" y a continuación, registra en, redirigirá a Logout.aspx, lo que podría confundir fácilmente el usuario.


## <a name="summary"></a>Resumen

En este tutorial se inicia con un examen del flujo de trabajo de autenticación de formularios y, a continuación, se activa para implementar la autenticación mediante formularios en una aplicación ASP.NET. Autenticación de formularios se realiza gracias FormsAuthenticationModule, que tiene dos responsabilidades: identificar a los usuarios en función de su vale de autenticación de formularios y la redirección de los usuarios no autorizados a la página de inicio de sesión.

Clase de FormsAuthentication de .NET Framework incluye métodos para crear, inspeccionar y eliminar vales de autenticación de formularios. La propiedad Request.IsAuthenticated y el objeto de usuario proporcionan soporte de programación adicional para determinar si una solicitud está autenticada y obtener información acerca de la identidad del usuario. También son los controles de LoginView y LoginStatus, LoginName Web, que proporcionan a los desarrolladores una manera rápida y sin código para llevar a cabo muchas tareas comunes relacionadas con el inicio de sesión. Examinaremos estos y otros controles de Web relacionados con el inicio de sesión con más detalle en tutoriales futuros.

Este tutorial proporciona una introducción rápida de autenticación de formularios. No se examinar las opciones de configuración ordenados, buscar en el trabajo de vales de autenticación de formularios cómo cookieless o explorar cómo ASP.NET protege el contenido del vale de autenticación de formularios. Trataremos estos temas y mucho más en la [siguiente tutorial](forms-authentication-configuration-and-advanced-topics-cs.md).

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Cambios entre la seguridad de IIS 7 e IIS 6](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Controles ASP.NET de inicio de sesión](https://msdn.microsoft.com/library/d51ttbhx.aspx)
- [Professional ASP.NET 2.0 seguridad, la pertenencia y la administración de roles](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [El `<authentication>` elemento](https://msdn.microsoft.com/library/532aee0e.aspx)
- [El `<forms>` (elemento) para`<authentication>`](https://msdn.microsoft.com/library/1d3t3c61.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Aprendizaje mediante vídeo sobre los temas incluidos en este Tutorial

- [Usar la autenticación de formularios básicos en ASP.NET](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a...

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era este tutorial serie se revisó por varios revisores útiles. Los revisores iniciales para este tutorial incluyen Alicja Maziarz, John Suru y Teresa Murphy. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](security-basics-and-asp-net-support-cs.md)
[Siguiente](forms-authentication-configuration-and-advanced-topics-cs.md)
