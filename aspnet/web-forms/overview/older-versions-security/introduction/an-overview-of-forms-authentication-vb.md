---
uid: web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
title: Información general sobre la autenticación de formularios (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial está se convertirá de explicación simple a la aplicación; en particular, veremos la implementación de autenticación de formularios. La anchura de la aplicación web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: 83267f7d-64d9-41ee-82cf-da91b1bf534d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: df8b0595cbc02a99a39c5b39be9ddb2e92bc34aa
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385896"
---
<a name="an-overview-of-forms-authentication-vb"></a>Información general sobre la autenticación de formularios (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_02_VB.zip) o [descargar PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial02_FormsAuth_vb.pdf)

> En este tutorial está se convertirá de explicación simple a la aplicación; en particular, veremos la implementación de autenticación de formularios. La aplicación web, que comenzamos a crear en este tutorial seguirán se basa en los tutoriales posteriores, conforme se mueve de la autenticación de formularios simples a la pertenencia y roles.
> 
> Vea este vídeo para obtener más información sobre este tema: [mediante la autenticación de formularios básicos en ASP.NET](# "using-basic-forms-authentication-in-aspnet").


## <a name="introduction"></a>Introducción

En el [tutorial anterior](security-basics-and-asp-net-support-vb.md) analizamos las opciones de cuenta distintos de un usuario, autenticación y autorización proporcionadas por ASP.NET. En este tutorial está se convertirá de explicación simple a la aplicación; en particular, veremos la implementación de autenticación de formularios. La aplicación web, que comenzamos a crear en este tutorial seguirán se basa en los tutoriales posteriores, conforme se mueve de la autenticación de formularios simples a la pertenencia y roles.

Este tutorial comienza con información detallada sobre el flujo de trabajo de autenticación de formularios, un tema que analizamos en el tutorial anterior. A continuación, creamos un sitio Web ASP.NET a través del cual los conceptos de autenticación de formularios de demostración. A continuación, configuraremos el sitio para usar autenticación por formularios, cree una página de inicio de sesión simple y vea cómo determinar si un usuario está autenticado y, si es así, el nombre de usuario ha iniciado sesión en el código.

Descripción de los formularios de flujo de trabajo de autenticación, habilitarlo en una aplicación web y la creación de las páginas de inicio de sesión y cierre de sesión son vitales en todos los pasos en la creación de una aplicación ASP.NET que es compatible con las cuentas de usuario y autentica a los usuarios a través de una página web. Debido a esto - y dado que estos tutoriales se basan en el otro, me gustaría incentivarlo trabajar a través de este tutorial en su totalidad antes de pasar a la siguiente aunque ya haya tenido la configuración de experiencia de autenticación de formularios en proyectos anteriores.

## <a name="understanding-the-forms-authentication-workflow"></a>Descripción del flujo de trabajo de autenticación de formularios

Cuando el tiempo de ejecución ASP.NET procesa una solicitud para un recurso ASP.NET, como una página ASP.NET o servicio Web de ASP.NET, la solicitud genera un número de eventos durante su ciclo de vida. Hay eventos provocados al final muy muy y a partir de la solicitud, que se genera cuando la solicitud es que se autentica y autoriza, un evento se genera en el caso de una excepción no controlada y así sucesivamente. Para ver una lista completa de los eventos, consulte el [eventos del objeto HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication_events.aspx).

*Los módulos HTTP* son clases administradas cuyo código se ejecuta en respuesta a un evento determinado en el ciclo de vida de la solicitud. ASP.NET incluye una serie de módulos HTTP que realizan tareas esenciales en segundo plano. Dos módulos HTTP integrados que son especialmente importantes para nuestro análisis son:

- **[FormsAuthenticationModule](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)**  -autentica al usuario inspeccionando el vale de autenticación de formularios, que normalmente se incluye en la colección de cookies del usuario. Si no está presente ningún vale de autenticación de formularios, el usuario es anónimo.
- **[UrlAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)**  -determina si el usuario actual está autorizado para tener acceso a la dirección URL solicitada. Este módulo determina la entidad consultando las reglas de autorización especificadas en archivos de configuración de la aplicación. ASP.NET también incluye el [FileAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx) que determina la entidad consultando la ACL de archivos solicitados.

FormsAuthenticationModule intenta autenticar al usuario antes de la UrlAuthorizationModule (y FileAuthorizationModule) que se ejecuta. Si el usuario que realiza la solicitud no está autorizado para acceder al recurso solicitado, el módulo de autorización finaliza la solicitud y devuelve un [HTTP 401 no autorizado](http://www.checkupdown.com/status/E401.html) estado. En escenarios de autenticación de Windows, se devuelve el estado HTTP 401 al explorador. Este código de estado hace que el explorador solicitar al usuario sus credenciales a través de un cuadro de diálogo modal. Con la autenticación de formularios, sin embargo, el estado HTTP 401 no autorizado nunca se envía al explorador porque FormsAuthenticationModule detecta este estado y lo modifica para redirigir al usuario a la página de inicio de sesión en su lugar (a través de un [HTTP 302 redirigir](http://www.checkupdown.com/status/E302.html) estado).

Es responsabilidad de la página de inicio de sesión determinar si las credenciales del usuario son válidas y, si es así, para crear un vale de autenticación de formularios y redirigir al usuario a la página estaban intentando visitar. El vale de autenticación se incluye en las solicitudes posteriores a las páginas en el sitio Web, que usa FormsAuthenticationModule para identificar al usuario.


[![El flujo de trabajo de autenticación de formularios](an-overview-of-forms-authentication-vb/_static/image2.png)](an-overview-of-forms-authentication-vb/_static/image1.png)

**Figura 01**: el flujo de trabajo de autenticación de formularios ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-forms-authentication-vb/_static/image3.png))


### <a name="remembering-the-authentication-ticket-across-page-visits"></a>Recordar el vale de autenticación a través de las visitas de la página

Después de iniciar sesión, se debe enviar el vale de autenticación de formularios al servidor web en cada solicitud para que el usuario permanece conectado mientras exploran el sitio. Normalmente, esto se logra colocando el vale de autenticación en la colección de cookies del usuario. [Las cookies](http://en.wikipedia.org/wiki/HTTP_cookie) son pequeños archivos de texto que residen en el equipo del usuario y se transmiten en los encabezados HTTP en cada solicitud al sitio Web que creó la cookie. Por lo tanto, una vez creado el vale de autenticación de formularios y almacenado en las cookies del explorador, cada vez que visito posteriores a ese sitio envía el vale de autenticación junto con la solicitud, con lo que se identifica al usuario.

> [!NOTE]
> La aplicación web de demostración usada en cada tutorial está disponible como descarga. Esta aplicación descargable se creó con Visual Web Developer 2008 destinados a .NET Framework versión 3.5. Puesto que la aplicación está destinada a .NET 3.5, su archivo Web.config incluye elementos de configuración adicionales, 3.5 específica. En resumen, si aún tiene que instalar .NET 3.5 en el equipo, a continuación, la aplicación web que se pueden descargar no funcionará sin quitar primero el marcado específico de 3.5 de Web.config.


Un aspecto de cookies es su caducidad, que es la fecha y hora en que el explorador descarta la cookie. Cuando expire la cookie de autenticación de formularios, el usuario ya no se autentica y, por tanto, se convierten en anónimo. Cuando un usuario está visitando desde un terminal público, lo más probable es que desean el vale de autenticación expire cuando cierre su explorador. Cuando se visita desde casa, sin embargo, ese mismo usuario podría recordar entre reinicios del explorador para que no tengan el vale de autenticación para volver a iniciar sesión cada vez que visitan el sitio. Esta decisión suelen tomarla por el usuario en forma de una casilla Recordar mi cuenta en la página de inicio de sesión. En el paso 3, examinaremos cómo implementar una casilla Recordar mi cuenta en la página de inicio de sesión. El siguiente tutorial aborda la configuración de tiempo de espera de vale de autenticación en detalle.

> [!NOTE]
> Es posible que el agente de usuario utilizado para iniciar sesión en el sitio Web puede no admitir cookies. En tal caso, ASP.NET puede usar los vales de autenticación de formularios sin cookies. En este modo, se codifica el vale de autenticación en la dirección URL. Veremos cuando se usan vales de autenticación sin cookies y cómo se crean y administran en el siguiente tutorial.


### <a name="the-scope-of-forms-authentication"></a>El ámbito de autenticación de formularios

Se administra FormsAuthenticationModule código que forma parte de la ejecución de ASP.NET. Antes de la versión 7 de Microsoft [Internet Information Services (IIS)](https://www.iis.net/) servidor web, se ha producido una barrera distinta entre la canalización HTTP de IIS y la canalización de la ejecución de ASP.NET. En resumen, en IIS 6 y versiones anteriores, FormsAuthenticationModule solo se ejecuta cuando se delega una solicitud de IIS para el tiempo de ejecución ASP.NET. De forma predeterminada, IIS procesa el contenido estático Sí -, como páginas HTML y CSS y archivos de imagen - y entrega solamente las solicitudes para el tiempo de ejecución ASP.NET cuando se solicita una página con la extensión .aspx, .asmx o .ashx.

IIS 7, sin embargo, permite integrada IIS y ASP.NET canalizaciones. Con algunos valores de configuración, puede configurar IIS 7 para invocar FormsAuthenticationModule para *todas* solicitudes. Además, puede definir reglas de autorización de dirección URL para archivos de cualquier tipo con IIS 7. Para obtener más información, consulte [cambios entre IIS6 e IIS7 seguridad](https://www.iis.net/learn/get-started/whats-new-in-iis-7/changes-in-security-between-iis-60-and-iis-7-and-above), [la seguridad de plataforma Web](https://www.iis.net/learn/get-started/whats-new-in-iis-7/iis7-and-above-security-improvements), y [autorización de URL de IIS7 descripción](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization).

Larga historia breve, en las versiones anteriores de IIS 7, sólo puede utilizar la autenticación de formularios para proteger los recursos que controla el tiempo de ejecución ASP.NET. Del mismo modo, solo se aplican las reglas de autorización de dirección URL a los recursos que controla el tiempo de ejecución ASP.NET. Pero con IIS 7 se pueden integrar la FormsAuthenticationModule y UrlAuthorizationModule en canalización HTTP de IIS, lo cual amplía esta funcionalidad en todas las solicitudes.

## <a name="step-1-creating-an-aspnet-website-for-this-tutorial-series"></a>Paso 1: Creación de un sitio Web ASP.NET para esta serie de tutoriales

Para llegar a la audiencia más amplia posible, se creará el sitio Web de ASP.NET se va a crear a lo largo de esta serie con la versión de Microsoft gratuita de Visual Studio 2008, [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Implementamos el almacén del usuario SqlMembershipProvider en un [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) base de datos. Si usa Visual Studio 2005 o una edición diferente de Visual Studio 2008 o SQL Server, no se preocupe: los pasos será casi idénticos y se indicará las diferencias no trivial.

Antes de que podemos configurar la autenticación de formularios, primero es necesario un sitio Web ASP.NET. Empiece por crear un nuevo archivo basado en el sistema sitio Web de ASP.NET. Para lograr esto, inicie Visual Web Developer y, a continuación, vaya al menú archivo y elija Nuevo sitio Web, mostrar el cuadro de diálogo nuevo sitio Web. Elija la plantilla de sitio Web de ASP.NET, Establece la lista desplegable de ubicación en el sistema de archivos, elija una carpeta para colocar el sitio web y establecer el idioma en VB. Esto creará un nuevo sitio web con una página Default.aspx de ASP.NET, una aplicación\_carpeta de datos y un archivo Web.config.

> [!NOTE]
> Visual Studio admite dos modos de administración de proyectos: proyectos de sitios Web y proyectos de aplicación Web. Proyectos de sitios Web carecen de un archivo de proyecto, mientras que los proyectos de aplicación Web imitar la arquitectura del proyecto en Visual Studio .NET 2002/2003: incluyen un archivo de proyecto y compilar código fuente del proyecto en un único ensamblado, que está situado en la carpeta/bin. Visual Studio 2005 inicialmente solo compatibles proyectos de sitio Web, aunque el modelo de proyecto de aplicación Web se vuelve a insertar con Service Pack 1. Visual Studio 2008 ofrece ambos modelos de proyecto. Visual Web Developer 2005 y 2008 ediciones, sin embargo, solo admiten proyectos de sitio Web. Va a utilizar el modelo de proyecto de sitio Web. Si está usando una edición no Express y desea utilizar el [modelo de proyecto de aplicación Web](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) en su lugar, no dude en hacerlo, pero tenga en cuenta que puede haber algunas discrepancias entre lo que ve en la pantalla y los pasos que debe seguir frente a la capturas de pantalla se muestra y las instrucciones proporcionadas en estos tutoriales.


[![Crear un nuevo archivo basado en el sistema sitio Web](an-overview-of-forms-authentication-vb/_static/image5.png)](an-overview-of-forms-authentication-vb/_static/image4.png)

**Figura 02**: crear un sitio Web de New File System-Based ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-forms-authentication-vb/_static/image6.png))


### <a name="adding-a-master-page"></a>Adición de una página maestra

A continuación, agregue una nueva página principal para el sitio en el directorio raíz denominado Site.master. [Páginas maestras](https://msdn.microsoft.com/library/wtxbf3hh.aspx) permiten a un desarrollador de la página Definir una plantilla de todo el sitio que se puede aplicar a las páginas ASP.NET. La principal ventaja de las páginas principales es que apariencia general de la carpeta del sitio se puede definir en una única ubicación, facilitando fáciles de actualizar o cambiar el diseño de la carpeta del sitio.


[![Agregar una página maestra denominada Site.master al sitio Web](an-overview-of-forms-authentication-vb/_static/image8.png)](an-overview-of-forms-authentication-vb/_static/image7.png)

**Figura 03**: agregar un Site.master de denominado de página maestra al sitio Web ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-forms-authentication-vb/_static/image9.png))


Definir el diseño de página de todo el sitio aquí en la página maestra. Puede usar la vista de diseño y agregar los controles Web o de diseño que necesita, o puede agregar manualmente el marcado de forma manual en la vista del origen. Estructuré el diseño de mi página maestra para imitar el diseño utilizado en mi *[trabajar con datos en ASP.NET 2.0](../../data-access/index.md)* serie de tutoriales (consulte la figura 4). La página maestra [hojas de estilos en cascada](http://www.w3schools.com/css/default.asp) para colocar y estilos con configuración CSS definidos en el archivo Style.css (que se incluye en la descarga asociados de este tutorial). Mientras que no se puede deducirlo del marcado que se muestra a continuación, se definen las reglas de CSS que el panel de navegación &lt;div&gt;del absolutamente se coloca el contenido por lo que aparece a la izquierda y tiene un ancho fijo de 200 píxeles.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample1.aspx)]

Una página principal define el diseño de página estática y las regiones que se pueden editar mediante las páginas ASP.NET que utilicen la página principal. Estas regiones contenidas editables se indican mediante el control ContentPlaceHolder, que se puede ver dentro del contenido &lt;div&gt;. Esta página principal tiene sólo un ContentPlaceHolder (MainContent), pero la página principal puede tener varios ContentPlaceHolders.

Con el marcado anterior, cambiar a la vista Diseño, muestra el diseño de la página maestra. Todas las páginas ASP.NET que usan esta página principal tendrán este diseño uniforme y con capacidad para especificar el marcado para la región MainContent.


[![La página maestra, cuando se ven a través de la vista de diseño](an-overview-of-forms-authentication-vb/_static/image11.png)](an-overview-of-forms-authentication-vb/_static/image10.png)

**Figura 04**: la página maestra, al ver a través de la vista de diseño ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-forms-authentication-vb/_static/image12.png))


### <a name="creating-content-pages"></a>Crear páginas de contenido

En este momento tenemos una página Default.aspx en nuestro sitio Web, pero no utiliza la página maestra que acabamos de crear. Aunque es posible manipular el marcado declarativo de una página web para usar una página maestra, si la página no contiene ningún contenido aún es más fácil simplemente eliminar la página y vuelva a agregarlo al proyecto, especificar la página maestra para usar. Por lo tanto, comience por eliminar Default.aspx del proyecto.

A continuación, haga doble clic en el nombre del proyecto en el Explorador de soluciones y elija Agregar un formulario Web nuevo llamado Default.aspx. Esta vez, active la casilla Seleccionar la página principal y elija la página maestra Site.master en la lista.


[![Agregue una nueva página Default.aspx elegir seleccionar una página maestra](an-overview-of-forms-authentication-vb/_static/image14.png)](an-overview-of-forms-authentication-vb/_static/image13.png)

**Figura 05**: agregar una nueva elección de página Default.aspx para seleccionar una página maestra ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-forms-authentication-vb/_static/image15.png))


[![Utilice la página maestra Site.master](an-overview-of-forms-authentication-vb/_static/image17.png)](an-overview-of-forms-authentication-vb/_static/image16.png)

**Figura 06**: Use la página maestra Site.master ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-forms-authentication-vb/_static/image18.png))


> [!NOTE]
> El cuadro de diálogo Agregar nuevo elemento no incluye una casilla de verificación Seleccionar la página principal si usa el modelo de proyecto de aplicación Web. En su lugar, deberá agregar un elemento del tipo de formulario de contenido Web. Después de elegir la opción de formulario de contenido Web y hacer clic en Agregar, Visual Studio mostrará el mismo, seleccione un patrón de cuadro de diálogo se muestra en la figura 6.


Marcado declarativo de la página Default.aspx nueva incluye simplemente un @Page directiva especifica la ruta de acceso al maestro de página MainContent ContentPlaceHolder de la página maestra archivo y un control de contenido.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample2.aspx)]

Por ahora, deje en blanco Default.aspx. Volveremos a él más adelante en este tutorial para agregar contenido.

> [!NOTE]
> Esta página principal incluye una sección de un menú u otra interfaz de navegación. Esta interfaz se creará en un futuro tutorial.


## <a name="step-2-enabling-forms-authentication"></a>Paso 2: Habilitar la autenticación de formularios

Con el sitio Web ASP.NET creado, la siguiente tarea es habilitar la autenticación de formularios. Configuración de autenticación de la aplicación se especifica a través de la [ &lt;autenticación&gt; elemento](https://msdn.microsoft.com/library/532aee0e.aspx) en el archivo Web.config. El &lt;autenticación&gt; elemento contiene un único atributo denominado modo que especifica el modelo de autenticación utilizado por la aplicación. Este atributo puede tener uno de los siguientes cuatro valores:

- **Windows** : como se describe en el tutorial anterior, cuando una aplicación utiliza la autenticación de Windows es responsabilidad del servidor web para autenticar el visitante y esto se hace normalmente mediante básica, implícita o integrada de Windows autenticación.
- **Formularios**-los usuarios se autentican a través de un formulario en una página web.
- **Passport**-los usuarios se autentican con Microsoft Passport Network.
- **Ninguno**-se utiliza ningún modelo de autenticación; todos los visitantes son anónimos.

De forma predeterminada, aplicaciones de ASP.NET usan la autenticación de Windows. Para cambiar el tipo de autenticación a autenticación de formularios, a continuación, necesitamos modificar la &lt;autenticación&gt; atributo de modo del elemento a los formularios.

Si el proyecto aún no contiene un archivo Web.config, agregue uno ahora en el botón secundario en el nombre del proyecto en el Explorador de soluciones, elija Agregar nuevo elemento y, a continuación, agregar un archivo de configuración Web.


[![Si el proyecto no tiene aún Web.config, agréguelo ahora](an-overview-of-forms-authentication-vb/_static/image20.png)](an-overview-of-forms-authentication-vb/_static/image19.png)

**Figura 07**: si su proyecto Does no todavía incluyen Web.config, agregue ahora ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-forms-authentication-vb/_static/image21.png))


A continuación, busque el &lt;autenticación&gt; elemento y actualización que use autenticación de formularios. Después de este cambio, el marcado del archivo Web.config debe ser similar al siguiente:

[!code-xml[Main](an-overview-of-forms-authentication-vb/samples/sample3.xml)]

> [!NOTE]
> Puesto que el archivo Web.config es un archivo XML, son importante mayúsculas y minúsculas. Asegúrese de establecer el atributo mode en formularios, con una letra mayúscula f el. Si utiliza una grafía diferente, como los formularios, recibirá un error de configuración cuando se visita el sitio mediante un explorador.


El &lt;autenticación&gt; elemento puede incluir opcionalmente un &lt;formularios&gt; elemento secundario que contiene la configuración específica de la autenticación de formularios. Por ahora, vamos a usar simplemente la configuración de autenticación de formularios de forma predeterminada. Exploraremos la &lt;formularios&gt; elemento secundario con más detalle en el siguiente tutorial.

## <a name="step-3-building-the-login-page"></a>Paso 3: Creación de la página de inicio de sesión

Para admitir la autenticación de formularios nuestro sitio Web necesita una página de inicio de sesión. Como se describe en la descripción de la sección de flujo de trabajo de autenticación de formularios, FormsAuthenticationModule le redireccionará automáticamente al usuario a la página de inicio de sesión si éstos intentan tener acceso a una página que no son autorizado para ver. También hay controles Web ASP.NET que se mostrarán un vínculo a la página de inicio de sesión a los usuarios anónimos. Esto plantea la pregunta, ¿qué es la dirección URL de la página de inicio de sesión?

De forma predeterminada, el sistema de autenticación de formularios espera que la página de inicio de sesión se denomine Login.aspx y lo coloca en el directorio raíz de la aplicación web. Si desea usar una dirección URL de página de inicio de sesión diferente, puede hacerlo especificando en el archivo Web.config. Vemos cómo hacer esto en el tutorial posterior.

La página de inicio de sesión tiene tres responsabilidades:

1. Proporcionar una interfaz que permite que el visitante que escriba sus credenciales.
2. Determina si las credenciales enviadas son válidas.
3. El usuario inicie sesión mediante la creación de los formularios de vale de autenticación.

### <a name="creating-the-login-pages-user-interface"></a>Creación de interfaz de usuario de la página de inicio de sesión

Vamos a empezar a trabajar con la primera tarea. Agregue una nueva página ASP.NET al directorio raíz de la carpeta del sitio llamado Login.aspx y asociarla a la página maestra Site.master.


[![Agregue una nueva página de ASP.NET denominada Login.aspx](an-overview-of-forms-authentication-vb/_static/image23.png)](an-overview-of-forms-authentication-vb/_static/image22.png)

**Figura 08**: agregar una página de ASP.NET nueva llamada Login.aspx ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-forms-authentication-vb/_static/image24.png))


La interfaz de la página de inicio de sesión típico consta de dos cuadros de texto: uno para el nombre del usuario, uno para su contraseña - y un botón para enviar el formulario. Los sitios Web a menudo, incluyen una casilla Recordar mi cuenta que, si está activada, se conserva el vale de autenticación resultante entre reinicios del explorador.

Agregar dos cuadros de texto a Login.aspx y establecer sus propiedades de identificador para el nombre de usuario y contraseña, respectivamente. También establece la propiedad TextMode de contraseña en contraseña. A continuación, agregue un control CheckBox, establecer su propiedad ID en RememberMe y su propiedad Text a Recordarme. A continuación, agregue un botón denominado LoginButton cuya propiedad de texto se establece en el inicio de sesión. Y por último, agregue un control Web de etiqueta y establezca su propiedad ID para InvalidCredentialsMessage, su propiedad Text a su nombre de usuario o contraseña no es válido. Vuelva a intentarlo., su propiedad de color de primer plano en rojo y su propiedad Visible en False.

En este momento, la pantalla debe ser similar a la pantalla en la figura 9, y la sintaxis declarativa de la página debe ser similar al siguiente:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample4.aspx)]


[![La página de inicio de sesión contiene dos cuadros de texto, una casilla de verificación, un botón y una etiqueta](an-overview-of-forms-authentication-vb/_static/image26.png)](an-overview-of-forms-authentication-vb/_static/image25.png)

**Figura 09**: el inicio de sesión página contiene dos cuadros de texto, una casilla de verificación, un botón y una etiqueta ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-forms-authentication-vb/_static/image27.png))


Por último, cree un controlador de eventos, haga clic en del LoginButton evento. En el diseñador, simplemente haga doble clic en el control de botón para crear este controlador de eventos.

### <a name="determining-if-the-supplied-credentials-are-valid"></a>Determinar si las credenciales proporcionados no son válidos

Ahora debemos implementar la tarea 2 Click del botón en el controlador de eventos: determinar si las credenciales proporcionadas son válidas. Para ello, debe haber un almacén de usuario que contiene todas las credenciales de los usuarios de modo que podemos determinar si las credenciales proporcionadas coinciden con las credenciales conocidas.

Antes de ASP.NET 2.0, los desarrolladores eran responsables de implementar ambos sus propios almacenes de usuario y escribir el código para validar las credenciales proporcionadas en el almacén. La mayoría de los desarrolladores implementaría el almacén del usuario en una base de datos, crear una tabla denominada a los usuarios con columnas como nombre de usuario, contraseña, correo electrónico, LastLoginDate y así sucesivamente. Esta tabla, a continuación, tendría un registro por cada cuenta de usuario. Comprobar las credenciales de un usuario proporcionado implicaría consultar la base de datos para un nombre de usuario coincidente y, a continuación, lo que garantiza que la contraseña en la base de datos correspondía a la contraseña proporcionada.

Con ASP.NET 2.0, los desarrolladores deben usar uno de los proveedores de pertenencia para administrar el almacén de usuario. En esta serie de tutoriales usaremos SqlMembershipProvider, que usa una base de datos de SQL Server para el almacén del usuario. Cuando se usa el SqlMembershipProvider debemos implementar un esquema de base de datos específica que incluye tablas, vistas y procedimientos almacenados esperados por el proveedor. Examinaremos cómo implementar este esquema en el *[crear el esquema de pertenencia en SQL Server](../membership/creating-the-membership-schema-in-sql-server-vb.md)* tutorial. Con el proveedor de pertenencia en su lugar, validar las credenciales del usuario es tan sencillo como llamar a la [clase de pertenencia](https://msdn.microsoft.com/library/system.web.security.membership.aspx)del [ValidateUser (*username*, *contraseña*) método](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx), que devuelve un valor booleano que indica si la validez de la *username* y *contraseña* combinación. Tomamos como que aún no hemos implementado almacén del usuario del SqlMembershipProvider, no podemos usamos pertenencia ValidateUser método de la clase en este momento.

En lugar de dedicar tiempo a crear nuestra propia tabla personalizado de la base de datos de los usuarios (que podría ser obsoleto, una vez que hemos implementado SqlMembershipProvider), vamos a en su lugar, codifique de forma rígida las credenciales válidas en el inicio de sesión página propia. En el LoginButton haga clic en el controlador de eventos, agregue el código siguiente:

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample5.vb)]

Como puede ver, hay tres cuentas de usuario válidas - Scott, Jisun y Sam, y los tres tienen la misma contraseña (contraseña). El código establece bucles en las matrices de usuarios y contraseñas busca una coincidencia de nombre de usuario y contraseña válida. Si el nombre de usuario y la contraseña son válidos, se debe iniciar sesión el usuario y, a continuación, redirigirlos a la página apropiada. Si las credenciales son válidas, a continuación, mostramos la etiqueta InvalidCredentialsMessage.

Cuando un usuario escribe credenciales válidas, mencioné que, a continuación, se redirige a la página adecuada. ¿Qué es la página adecuada, aunque? Recuerde que cuando un usuario visita una página que no están autorizados para ver, FormsAuthenticationModule lo redirige automáticamente a la página de inicio de sesión. En este modo, se incluye la dirección URL solicitada en la cadena de consulta a través del parámetro ReturnUrl. Es decir, si un usuario intentó visite ProtectedPage.aspx y que no tenían autorización para ello, FormsAuthenticationModule redirigiría les:

¿Login.aspx? ReturnUrl=ProtectedPage.aspx

Tras iniciar sesión correctamente, se redirigirá al usuario volver a ProtectedPage.aspx. Como alternativa, los usuarios pueden visitar la página de inicio de sesión en su propio volition. En ese caso, después de iniciar sesión el usuario debe ser enviados a la página Default.aspx de la carpeta raíz.

### <a name="logging-in-the-user"></a>Sesión del usuario

Suponiendo que las credenciales proporcionadas son válidas, es necesario crear un vale de autenticación de formularios, con lo que iniciar sesión en el usuario al sitio. El [clase FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthentication.aspx) en el [espacio de nombres System.Web.Security](https://msdn.microsoft.com/library/system.web.security.aspx) proporciona diversas métodos para el registro de inicio de sesión a los usuarios a través de los formularios y en el sistema de autenticación. Aunque existen varios métodos en la clase FormsAuthentication, son los tres que nos interesan en ese momento:

- [GetAuthCookie (*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.getauthcookie.aspx) -crea un vale de autenticación de formularios para el nombre proporcionado *username*. A continuación, este método crea y devuelve un objeto HttpCookie que contiene el contenido del vale de autenticación. Si *persistCookie* es True, se crea una cookie persistente.
- [SetAuthCookie (*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) -llama a la GetAuthCookie (*username*, *persistCookie*) método para generar la cookie de autenticación de formularios. Este método, a continuación, agrega la cookie devuelta por GetAuthCookie a la colección de Cookies (suponiendo que la autenticación de formularios basada en cookies se utiliza; en caso contrario, este método llama a una clase interna que controla la lógica de vale sin cookies).
- [RedirectFromLoginPage (*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.redirectfromloginpage.aspx) -llama a este método SetAuthCookie (*username*, *persistCookie*) y, a continuación, redirige al usuario a la página adecuada.

GetAuthCookie es útil cuando se necesita modificar el vale de autenticación antes de escribir la cookie a la colección de Cookies. SetAuthCookie es útil si desea crear los formularios de vale de autenticación y agregarlo a la colección de Cookies, pero no desea redirigir al usuario a la página adecuada. Es posible que desee conservarlos en la página de inicio de sesión o enviarlos a una página determinada alternativa.

Puesto que deseamos que el usuario inicie sesión y redirigirlas a la página adecuada, vamos a usar RedirectFromLoginPage. Actualización haga clic en del LoginButton el controlador de eventos, reemplazando las dos líneas comentadas de TODO con la siguiente línea de código:

FormsAuthentication.RedirectFromLoginPage (UserName.Text, RememberMe.Checked)

Al crear los formularios de vale de autenticación se usa la propiedad de texto de UserName TextBox para el vale de autenticación de formularios *username* parámetro y el estado activado de RememberMe CheckBox para el  *persistCookie* parámetro.

Para probar la página de inicio de sesión, visite en un explorador. Iniciar, escriba las credenciales no válidas, por ejemplo, un nombre de usuario de Nope y una contraseña incorrecta. Al hacer clic en el botón de inicio de sesión se producirá una devolución de datos y se mostrará la etiqueta InvalidCredentialsMessage.


[![La etiqueta InvalidCredentialsMessage es muestran al escribir las credenciales no válidas](an-overview-of-forms-authentication-vb/_static/image29.png)](an-overview-of-forms-authentication-vb/_static/image28.png)

**Figura 10**: la etiqueta de InvalidCredentialsMessage es muestran al escribir las credenciales no válidas ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-forms-authentication-vb/_static/image30.png))


A continuación, escriba credenciales válidas y haga clic en el botón de inicio de sesión. Esta vez cuando la devolución de datos produce un vale de autenticación de formularios, se crea y se le redirigirá automáticamente a Default.aspx. En este momento han iniciado sesión el sitio Web, aunque no hay ningún indicaciones visuales para indicar que actualmente se registran. En el paso 4, que veremos cómo determinar mediante programación si un usuario se registra en o no, así como también identificar al usuario visitar la página.

Paso 5 examina las técnicas para registrar un usuario cierre el sitio Web.

### <a name="securing-the-login-page"></a>Protección de la página de inicio de sesión

Cuando el usuario escribe sus credenciales y envía el formulario de la página de inicio de sesión, las credenciales (incluidos con su contraseña) se transmiten a través de Internet al servidor web en *texto sin formato*. Esto significa que los hackers examen el tráfico de red pueden ver el nombre de usuario y la contraseña. Para evitar esto, es esencial para cifrar el tráfico de red mediante [capas de sockets seguros (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Esto garantizará que se cifra las credenciales (así como marcado HTML de la página completa) desde el momento en que dejan el explorador hasta que se reciben por el servidor web.

A menos que su sitio Web contiene información confidencial, solo deberá utilizar SSL en la página de inicio de sesión y en otras páginas donde la contraseña del usuario en caso contrario, se envía a través de la red en texto sin formato. No es necesario preocuparse por proteger los formularios de vale de autenticación, ya que, de forma predeterminada, es tanto cifrarse y firmarse digitalmente (para evitar la manipulación). En el siguiente tutorial se presenta un análisis más detallado sobre la seguridad de vale de autenticación de formularios.

> [!NOTE]
> Muchos sitios Web financieros y médicos están configurados para utilizar SSL en *todas* páginas accesibles a los usuarios autentican. Si está creando un sitio Web de este tipo puede configurar el sistema de autenticación de formularios para que el vale de autenticación de formularios sólo se transmite a través de una conexión segura. Asimismo, estudiaremos las distintas opciones de configuración de autenticación de formularios en el siguiente tutorial,  *[configuración de autenticación de formularios y temas avanzados](../membership/creating-the-membership-schema-in-sql-server-vb.md)*.


## <a name="step-4-detecting-authenticated-visitors-and-determining-their-identity"></a>Paso 4: Detectar visitantes autenticados y determinar su identidad

En este punto hemos habilitado la autenticación de formularios y creado una página de inicio de sesión rudimentaria, pero aún tenemos que examinar cómo podemos determinar si un usuario está autenticado o anónimo. En ciertos escenarios podemos queremos mostrar distintos datos o información dependiendo de si un usuario autenticado o anónimo está visitando la página. Además, a menudo es necesario conocer la identidad del usuario autenticado.

Vamos a aumentar la página Default.aspx existente para ilustrar estas técnicas. En Default.aspx, agregue dos controles de Panel AuthenticatedMessagePanel con una y otra AnonymousMessagePanel con nombre. Agregue un control de etiqueta denominado WelcomeBackMessage en el primer Panel. En el Panel de segundo, agregue un control de hipervínculo, establezca su propiedad Text para iniciar sesión y la propiedad NavigateUrl en ~ / Login.aspx. En este momento el marcado declarativo de Default.aspx debe ser similar al siguiente:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample6.aspx)]

Como probablemente habrá imaginado en este momento, la idea aquí es mostrar simplemente el AuthenticatedMessagePanel a los visitantes autenticados y simplemente la AnonymousMessagePanel a los visitantes anónimos. Para lograr esto es necesario establecer propiedades visibles de estos paneles dependiendo de si el usuario inició sesión o no.

El [Request.IsAuthenticated propiedad](https://msdn.microsoft.com/library/system.web.httprequest.isauthenticated.aspx) devuelve un valor booleano que indica si se ha autenticado la solicitud. Escriba el código siguiente en la página\_cargar código del controlador de eventos:

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample7.vb)]

Con este código en su lugar, visite Default.aspx a través de un explorador. Suponiendo que todavía tiene que iniciar sesión, verá un vínculo a la página de inicio de sesión (consulte la figura 11). Haga clic en este vínculo e inicie sesión en el sitio. Como hemos visto en el paso 3, después de escribir las credenciales se devolverán a Default.aspx, pero esta vez la página muestra el Bienvenido de nuevo. mensaje (consulte la figura 12).


[![Cuando visite forma anónima, un vínculo de registro se muestra](an-overview-of-forms-authentication-vb/_static/image32.png)](an-overview-of-forms-authentication-vb/_static/image31.png)

**Figura 11**: aparece cuando se visita forma anónima, un vínculo de registro ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-forms-authentication-vb/_static/image33.png))


[![¡Los usuarios autenticados se muestran el Bienvenido de nuevo! Mensaje](an-overview-of-forms-authentication-vb/_static/image35.png)](an-overview-of-forms-authentication-vb/_static/image34.png)

**Figura 12**: usuarios autenticados se muestran el Bienvenido de nuevo! Mensaje ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-forms-authentication-vb/_static/image36.png))


Podemos determinar identidad del usuario ha iniciado sesión actualmente a través de la [objeto HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)del [propiedad usuario](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx). El objeto HttpContext representa información sobre la solicitud actual y es la página principal para objetos ASP.NET comunes como respuesta, la solicitud y la sesión, entre otros. La propiedad de usuario representa el contexto de seguridad de la solicitud HTTP actual y la implementa el [interfaz IPrincipal](https://msdn.microsoft.com/library/system.security.principal.iprincipal.aspx).

FormsAuthenticationModule establece la propiedad de usuario. En concreto, cuando FormsAuthenticationModule encuentra un vale de autenticación de formularios en la solicitud entrante, crea un nuevo objeto GenericPrincipal y lo asigna a la propiedad de usuario.

Objetos de entidad (como GenericPrincipal) proporcionan información sobre la identidad del usuario y los roles a los que pertenecen. La interfaz de IPrincipal define a dos miembros:

- [IsInRole (*roleName*)](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole.aspx) -un método que devuelve un valor booleano que indica si la entidad de seguridad pertenece al rol especificado.
- [Identidad](https://msdn.microsoft.com/library/system.security.principal.iprincipal.identity.aspx) -una propiedad que devuelve un objeto que implementa el [interfaz IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx). La interfaz de IIdentity define tres propiedades: [AuthenticationType](https://msdn.microsoft.com/library/system.security.principal.iidentity.authenticationtype.aspx), [IsAuthenticated](https://msdn.microsoft.com/library/system.security.principal.iidentity.isauthenticated.aspx), y [nombre](https://msdn.microsoft.com/library/system.security.principal.iidentity.name.aspx).

Podemos determinar el nombre del visitante actual utilizando el código siguiente:

Dim currentUsersName As String = User.Identity.Name

Cuando el uso de autenticación mediante formularios, un [objeto FormsIdentity](https://msdn.microsoft.com/library/system.web.security.formsidentity.aspx) se crea para la propiedad de identidad de GenericPrincipal. La clase FormsIdentity siempre devuelve los formularios de cadena para su propiedad AuthenticationType y True para su propiedad IsAuthenticated. La propiedad Name devuelve el nombre de usuario que especificó al crear los formularios de vale de autenticación. Además de estas tres propiedades, FormsIdentity incluye el acceso al vale de autenticación subyacente a través de su [vale propiedad](https://msdn.microsoft.com/library/system.web.security.formsidentity.ticket.aspx). La propiedad Ticket devuelve un objeto de tipo [FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx), que tiene propiedades, como la expiración, IsPersistent, IssueDate, nombre y así sucesivamente.

Lo más importante que hay que considerar aquí es que la *username* los parámetros especificados en la FormsAuthentication.GetAuthCookie (*username*, *persistCookie*), Habrá (*username*, *persistCookie*) y FormsAuthentication.RedirectFromLoginPage (*username*, *persistCookie*) métodos es el mismo valor devuelto por User.Identity.Name. Además, está disponible el vale de autenticación creado por estos métodos convirtiendo User.Identity en un objeto FormsIdentity y, a continuación, obtener acceso a la propiedad de vale:

Dim ident como FormsIdentity = CType (User.Identity, FormsIdentity)

Dim authTicket como FormsAuthenticationTicket = ident. Vale

Vamos a proporcionar un mensaje más personalizado en Default.aspx. Actualizar la página\_cargar el controlador de eventos para que la propiedad de texto de la etiqueta WelcomeBackMessage se asigna la cadena de bienvenida, *username*!

WelcomeBackMessage.Text = "Bienvenido de nuevo," &amp; User.Identity.Name &amp; "!"

Figura 13 muestra el efecto de esta modificación (al iniciar sesión como usuario Scott).


[![El mensaje de bienvenida incluye actualmente ha iniciado en el nombre del usuario](an-overview-of-forms-authentication-vb/_static/image38.png)](an-overview-of-forms-authentication-vb/_static/image37.png)

**Figura 13**: el mensaje de bienvenida incluye nombre del usuario en el ha iniciado actualmente ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-forms-authentication-vb/_static/image39.png))


### <a name="using-the-loginview-and-loginname-controls"></a>Utilizando los controles de LoginName y LoginView

Mostrar contenido diferente a los usuarios autenticados y anónimos es un requisito común; por lo que muestra el nombre del usuario que ha iniciado sesión actualmente. Por ese motivo, ASP.NET incluye dos controles Web que proporcionan la misma funcionalidad que se muestra en la figura 13, pero sin necesidad de escribir una sola línea de código.

El [control LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) es un control Web basadas en plantillas que facilita la tarea mostrar datos diferentes a los usuarios autenticados y anónimos. El LoginView incluye dos plantillas predefinidas:

- AnonymousTemplate - cualquier marcado agregado a esta plantilla solo se muestra a los visitantes anónimos.
- LoggedInTemplate - se muestra el marcado de la plantilla sólo a los usuarios autenticados.

Vamos a agregar el control LoginView a la página principal de nuestro sitio, Site.master. En lugar de agregar simplemente el control LoginView, sin embargo, vamos a agregar tanto un nuevo control ContentPlaceHolder y, a continuación, coloque el control LoginView dentro de ese nuevo ContentPlaceHolder. El motivo de esta decisión se harán patente en breve.

> [!NOTE]
> Además del AnonymousTemplate y LoggedInTemplate, el control LoginView puede incluir plantillas específicas de funciones. Plantillas específicas de funciones mostrar marcado únicamente a aquellos usuarios que pertenecen a un rol especificado. Examinaremos las características basadas en rol del control LoginView en un futuro tutorial.


Empiece agregando un ContentPlaceHolder denominado LoginContent en la página maestra en el panel de navegación &lt;div&gt; elemento. Simplemente puede arrastrar un control ContentPlaceHolder desde el cuadro de herramientas en la vista del origen, colocar el marcado resultante justo arriba de la lista de tareas: menú insertará texto aquí.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample8.aspx)]

A continuación, agregue un control LoginView dentro el LoginContent ContentPlaceHolder. Se consideran contenido que aparece en los controles de la página maestra ContentPlaceHolder *contenido predeterminado* para el control ContentPlaceHolder. Es decir, las páginas ASP.NET que usan esta página principal pueden especificar su propio contenido para cada ContentPlaceHolder o usar el contenido de la página maestra predeterminada.

El LoginView y otros controles relacionados con el inicio de sesión se encuentran en la pestaña de inicio de sesión del cuadro de herramientas.


[![El Control LoginView en el cuadro de herramientas](an-overview-of-forms-authentication-vb/_static/image41.png)](an-overview-of-forms-authentication-vb/_static/image40.png)

**Figura 14**: el LoginView Control en el cuadro de herramientas ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-forms-authentication-vb/_static/image42.png))


A continuación, agregue dos &lt;br /&gt; elementos inmediatamente después del control LoginView, pero todavía en el ContentPlaceHolder. En este momento, el panel de navegación &lt;div&gt; marcado del elemento debe ser similar al siguiente:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample9.aspx)]

Plantillas de LoginView se pueden definir desde el diseñador o el marcado declarativo. Desde el Diseñador de Visual Studio, expanda la etiqueta inteligente de LoginView, que enumera las plantillas preconfiguradas en una lista desplegable. Escriba el texto Hello, desconocido en AnonymousTemplate; a continuación, agregue un control de hipervínculo y establecer sus propiedades Text y NavigateUrl para iniciar sesión y ~ / Login.aspx, respectivamente.

Después de configurar AnonymousTemplate, cambie a LoggedInTemplate y escriba el texto, "Bienvenido de nuevo,". A continuación, arrastre un control LoginName del cuadro de herramientas en LoggedInTemplate, coloca inmediatamente después de la "Welcome," texto. El [control LoginName](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginname.aspx), como su nombre implica, muestra el nombre del usuario con sesión iniciada actualmente. Internamente, el control LoginName simplemente envía la propiedad User.Identity.Name

Después de realizar estas adiciones a las plantillas de LoginView, el marcado debe tener un aspecto similar al siguiente:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample10.aspx)]

Con esta adición a la página maestra Site.master, cada página en nuestro sitio Web mostrará un mensaje diferente dependiendo de si el usuario está autenticado. Figura 15 muestra la página Default.aspx al usuario Jisun visitado mediante un explorador. La parte posterior bienvenida, Jisun mensaje se repite dos veces: una vez en la sección de exploración de la página principal de la izquierda (mediante el control LoginView que acabamos de agregar) y una vez en el Default.aspx de contenido (a través de controles de Panel y la lógica de programación).


[![El Asistente para la muestra del Control LoginView back Jisun.](an-overview-of-forms-authentication-vb/_static/image44.png)](an-overview-of-forms-authentication-vb/_static/image43.png)

**Figura 15**: The LoginView Control muestra Bienvenido de nuevo, Jisun. ([Haga clic aquí para ver imagen en tamaño completo](an-overview-of-forms-authentication-vb/_static/image45.png))


Como el LoginView se han agregado a la página maestra, puede aparecer en cada página en nuestro sitio. Sin embargo, puede haber páginas web donde no queremos que aparezca este mensaje. Una página es la página de inicio de sesión, puesto que parece un vínculo a la página de inicio de sesión fuera de lugar allí. Puesto que el control LoginView se coloca en un ContentPlaceHolder en la página principal, es posible invalidar este marcado de forma predeterminada en nuestra página de contenido. Abra Login.aspx y vaya al diseñador. Puesto que no hemos definido explícitamente un control de contenido en Login.aspx para el LoginContent ContentPlaceHolder en la página principal, la página de inicio de sesión mostrará marcado de la página maestra predeterminada para este ContentPlaceHolder. Puede ver esto a través del diseñador - el LoginContent ContentPlaceHolder muestra el marcado predeterminado (el control LoginView).


[![La página de inicio de sesión muestra el valor predeterminado el contenido de LoginContent ContentPlaceHolder la página maestra](an-overview-of-forms-authentication-vb/_static/image47.png)](an-overview-of-forms-authentication-vb/_static/image46.png)

**Figura 16**: la página de inicio de sesión muestra el contenido predeterminado para LoginContent ContentPlaceHolder's Page the Master ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-forms-authentication-vb/_static/image48.png))


Para invalidar el marcado predeterminado para el LoginContent ContentPlaceHolder, simplemente haga doble clic en la región en el diseñador y elija la opción de crear contenido personalizado en el menú contextual. (Cuando se usa el control ContentPlaceHolder de Visual Studio 2008 incluye un de etiquetas inteligentes que, cuando se selecciona, ofrece la misma opción.) Esto agrega un nuevo control de contenido para marcado de la página y, por tanto, nos permite definir contenido personalizado de esta página. Puede agregar un mensaje personalizado en este caso, por ejemplo, inicie sesión, pero vamos a déjelo en blanco.

> [!NOTE]
> En Visual Studio 2005, crear contenido personalizado crea un control en la página ASP.NET de contenido. En Visual Studio 2008, sin embargo, crear contenido personalizado copia contenido de la página maestra predeterminada en el control de contenido recién creado. Si usa Visual Studio 2008, a continuación, después de crear el nuevo control de contenido Asegúrese de que borrar el contenido copiado a través de la página maestra.


Figura 17 se muestra la página Login.aspx cuando visita desde un explorador después de realizar este cambio. Tenga en cuenta que no hay ningún Hello, aquí está otra vez, o un extraño *username* mensaje en el panel de navegación izquierdo &lt;div&gt; porque no hay cuando se visita Default.aspx.


[![La página de inicio de sesión oculta el marcado de LoginContent ContentPlaceHolder el valor predeterminado](an-overview-of-forms-authentication-vb/_static/image50.png)](an-overview-of-forms-authentication-vb/_static/image49.png)

**Figura 17**: la página de inicio de sesión oculta marcado del valor predeterminado LoginContent ContentPlaceHolder ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-forms-authentication-vb/_static/image51.png))


## <a name="step-5-logging-out"></a>Paso 5: El cierre de sesión

En el paso 3 analizamos la creación de una página de inicio de sesión para iniciar sesión un usuario en el sitio, pero aún tenemos que ver cómo un usuario cierre la sesión. Además de métodos para registrar un usuario, la clase FormsAuthentication también proporciona un [método SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx). El método de cierre de sesión simplemente destruye el vale de autenticación de formularios, registro, por tanto, el usuario fuera del sitio.

Ofrece que un vínculo de cierre de sesión es una característica tan común que ASP.NET incluye un control diseñado específicamente para un usuario cierre la sesión. El [control LoginStatus](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginstatus.aspx) muestra un control LinkButton de inicio de sesión o Logout LinkButton, según el estado de la autenticación del usuario. Se representa un control LinkButton de inicio de sesión para usuarios anónimos, mientras que un Logout LinkButton se muestra a los usuarios autenticados. El texto para el inicio de sesión y Logout LinkButtons puede configurarse a través de la LoginStatus LoginText y LogoutText propiedades.

Al hacer clic en LinkButton inicio de sesión, se produce un postback, desde el que se emite una redirección a la página de inicio de sesión. Al hacer clic en Logout LinkButton hace que el control LoginStatus invocar el método FormsAuthentication.SignOff y, a continuación, redirige al usuario a una página. La página que ha iniciado los usuarios se redirige a depende de la propiedad LogoutAction, que puede asignarse a uno de los tres valores siguientes:

- Actualización: el valor predeterminado; redirige al usuario a la página que se encontraban simplemente. Si la página que se encontraban simplemente no permite a los usuarios anónimos, FormsAuthenticationModule automáticamente redirigirá al usuario a la página de inicio de sesión.

Es posible que curiosidad sobre por qué se realiza un redireccionamiento por aquí. Si el usuario desea permanecer en la misma página, ¿por qué la necesidad de la redirección explícita? La razón es que cuando se hace clic en Logoff LinkButton, el usuario todavía tiene el vale de autenticación de formularios en su colección de cookies. Por lo tanto, la solicitud de devolución de datos es una solicitud autenticada. El control LoginStatus llama al método de cierre de sesión, pero eso ocurre después FormsAuthenticationModule ha autenticado al usuario. Por lo tanto, un redireccionamiento explícito hace que el explorador volver a solicitar la página. En el momento en que el explorador solicita volver a la página, se ha quitado el vale de autenticación de formularios y, por tanto, la solicitud entrante no es anónima.

- Redirect - el usuario se redirige a la dirección URL especificada por la propiedad del LoginStatus LogoutPageUrl.
- RedirectToLoginPage - el usuario se redirige a la página de inicio de sesión.

Vamos a agregar un control LoginStatus a la página maestra y configúrelo para utilizar la opción redirigir a enviar al usuario a una página que muestra un mensaje que confirma que ha cerrado. Empiece por crear una página en el directorio raíz denominado Logout.aspx. No se olvide de asociar esta página a la página maestra Site.master. A continuación, escriba un mensaje en el marcado de la página explicar al usuario que ha cerrado.

A continuación, volver a la página maestra Site.master y agregue un control LoginStatus debajo el LoginView en el LoginContent ContentPlaceHolder. Establezca la propiedad del control LoginStatus LogoutAction redirección y su propiedad LogoutPageUrl en ~/Logout.aspx.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample11.aspx)]

Puesto que LoginStatus está fuera del control LoginView, aparecerá para los usuarios autenticados y anónimos, pero eso está bien porque LoginStatus mostrarán correctamente un inicio de sesión o Logout LinkButton. Con la adición del control LoginStatus, el registro en el hipervínculo en AnonymousTemplate es superfluo, así que eliminarlo.

Figura 18 se muestra Default.aspx cuando visita Jisun. Tenga en cuenta que la columna izquierda muestra el mensaje, Bienvenido de nuevo, Jisun junto con un vínculo para cerrar sesión. Al hacer clic en el registro de salida LinkButton produce un postback, cierra sesión en el sistema Jisun y redirige a ella a Logout.aspx. Como se muestra en la figura 19, en el momento en que llega a Jisun Logout.aspx que ya se ha cerrado y, por tanto, es anónimo. Por lo tanto, la columna izquierda muestra el texto de bienvenida, extrañas y un vínculo a la página de inicio de sesión.


[![Default.aspx se muestra aquí está otra vez, Jisun junto con un control LinkButton de cierre de sesión](an-overview-of-forms-authentication-vb/_static/image53.png)](an-overview-of-forms-authentication-vb/_static/image52.png)

**Figura 18**: Default.aspx se muestra aquí está otra vez, Jisun junto con un Logout LinkButton ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-forms-authentication-vb/_static/image54.png))


[![Logout.aspx muestra bienvenida, desconocido, junto con un control LinkButton de inicio de sesión](an-overview-of-forms-authentication-vb/_static/image56.png)](an-overview-of-forms-authentication-vb/_static/image55.png)

**Figura 19**: Logout.aspx muestra bienvenida, desconocido, junto con un control LinkButton de inicio de sesión ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-forms-authentication-vb/_static/image57.png))


> [!NOTE]
> Le animo a personalizar la página Logout.aspx para ocultar LoginContent ContentPlaceHolder la página maestra (como hicimos para Login.aspx en el paso 4). La razón es que el inicio de sesión LinkButton representada por el control LoginStatus (lo debajo de Hello, desconocido) envía al usuario a la página de inicio de sesión pasa la dirección URL actual en el parámetro de cadena de consulta ReturnUrl. En pocas palabras, si un usuario que ha cerrado la sesión hace clic en LinkButton de inicio de sesión de este LoginStatus y, a continuación, inicia sesión, le redirigirá a Logout.aspx, lo que podría confundir fácilmente el usuario.


## <a name="summary"></a>Resumen

En este tutorial se inicia con un examen del flujo de trabajo de autenticación de formularios y, a continuación, Considerando implementar autenticación de formularios en una aplicación de ASP.NET. Autenticación de formularios cuenta con la tecnología FormsAuthenticationModule, que tiene dos responsabilidades: identificar usuarios según su vale de autenticación de formularios y redirigir los usuarios no autorizados a la página de inicio de sesión.

Clase FormsAuthentication de .NET Framework incluye métodos para crear, inspeccionar y eliminar los vales de autenticación de formularios. La propiedad Request.IsAuthenticated y el objeto de usuario proporcionan soporte técnico de programación adicional para determinar si una solicitud está autenticada y obtener información acerca de la identidad del usuario. También hay controles LoginView, LoginStatus, LoginName Web y, que proporcionan a los desarrolladores una manera rápida y sin código para llevar a cabo muchas tareas comunes relacionadas con el inicio de sesión. Examinaremos estos y otros controles Web relacionados con el inicio de sesión con más detalle en próximos tutoriales.

Este tutorial proporciona una introducción rápida de autenticación de formularios. No se examinar las opciones de configuración diversas, examine cookieless cómo funcionan los formularios de autenticación vales o explorar cómo ASP.NET protege el contenido del vale de autenticación de formularios. Trataremos estos temas y mucho más en el [siguiente tutorial](forms-authentication-configuration-and-advanced-topics-vb.md).

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Cambios entre IIS6 y la seguridad de IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Controles de inicio de sesión ASP.NET](https://msdn.microsoft.com/library/d51ttbhx.aspx)
- [Professional ASP.NET 2.0 seguridad, la pertenencia y administración de roles](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [El &lt;autenticación&gt; elemento](https://msdn.microsoft.com/library/532aee0e.aspx)
- [El &lt;formularios&gt; (elemento) para &lt;autenticación&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>En los temas contenidos en este Tutorial en vídeo

- [Usar la autenticación de formularios básicos en ASP.NET](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha estado trabajando con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es *[SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Puede ponerse en contacto Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial incluyen Alicja Maziarz, John Suru y Teresa Murphy. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Anterior](security-basics-and-asp-net-support-vb.md)
> [Siguiente](forms-authentication-configuration-and-advanced-topics-vb.md)
