---
uid: web-pages/overview/releases/top-features-in-web-pages-2
title: Características de la parte superior de ASP.NET Web Pages 2 | Microsoft Docs
author: microsoft
description: En este tema se proporciona información general sobre las principales características nuevas de ASP.NET Web Pages 2, un marco de programación web ligero que se incluye con el WebMatr...
ms.author: aspnetcontent
ms.date: 02/13/2012
ms.assetid: cc712e72-c3d0-4e43-bc2d-28cc09cd8f71
msc.legacyurl: /web-pages/overview/releases/top-features-in-web-pages-2
msc.type: authoredcontent
ms.openlocfilehash: 6e20dedd19ae458b9881973570f23b5d77dda654
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827406"
---
<a name="the-top-features-in-aspnet-web-pages-2"></a>Las principales características de ASP.NET Web Pages 2
====================
por [Microsoft](https://github.com/microsoft)

> Este artículo proporciona información general de las principales características nuevas en ASP.NET Web Pages 2 RC, un marco de programación web ligero que se incluye con [Microsoft WebMatrix 2 RC](https://www.microsoft.com/web/).
> 
> **Lo que incluye:** 
> 
> - [Instalación de WebMatrix](#install)
> - [Características nuevas y mejoradas](#New_and_Enhanced_Features)
> 
>     - [Cambios para la versión RC](#Changes_for_the_RC_Version)
>     - [Cambios de la versión Beta](#Changes_for_the_Beta_Version)
>     - [Uso de las plantillas de sitio nuevas y actualizadas](#templates)
>     - [Validar la entrada del usuario](#validation)
>     - [Habilitar los inicios de sesión de Facebook y otros sitios con OAuth y OpenID](#oauthsetup)
>     - [Agregar asignaciones mediante la aplicación auxiliar de mapas](#maphelper)
>     - [Ejecutar aplicaciones en paralelo de las páginas Web](#sidebyside)
>     - [Representación de páginas para dispositivos móviles](#mobile)
> - [Recursos adicionales](#resources)
> 
> > [!NOTE]
> > En este tema se da por supuesto que está usando WebMatrix para trabajar con el código de ASP.NET Web Pages 2. Sin embargo, como con Web Pages 1, también puede crear sitios Web de Web Pages 2 mediante Visual Studio, que le permitirá mejorado las funcionalidades de IntelliSense y depuración. Para trabajar con páginas Web en Visual Studio, primero debe instalar Visual Studio 2010 SP1, Visual Web Developer Express 2010 SP1 o Visual Studio 11 Beta. A continuación, instale la versión Beta 4 de ASP.NET MVC, que incluye plantillas y herramientas para crear aplicaciones de ASP.NET MVC 4 y Web Pages 2 en Visual Studio.
> 
> 
> *Última actualización: 18 de junio de 2012*


<a id="install"></a>
## <a name="installing-webmatrix"></a>Instalación de WebMatrix

Para instalar las páginas Web, puede usar al instalador de plataforma Web de Microsoft, que es una aplicación gratuita que facilita la instalación y configuración de tecnologías relacionadas con la web. Se instalará la versión Beta 2 de WebMatrix, que incluye la versión Beta 2 de las páginas Web.

1. Vaya a la página de instalación de la versión más reciente del instalador de plataforma Web:

    [https://go.microsoft.com/fwlink/?LinkId=226883](https://go.microsoft.com/fwlink/?LinkId=226883)

    > [!NOTE]
    > Si ya tiene instalado el 1 de WebMatrix, esta instalación actualiza a la versión Beta 2 de WebMatrix. Puede ejecutar los sitios Web que se crearon con la versión 1 o 2 en el mismo equipo. Para obtener más información, vea la sección en [aplicaciones de páginas Web de ejecución en paralelo](#sidebyside).
2. Elija **instalar ahora**. 

    Si usa Internet Explorer, vaya al paso siguiente. Si utiliza un explorador diferente, como Mozilla Firefox o Google Chrome, deberá guardar los *Webmatrix.exe* a su equipo. Guarde el archivo y, a continuación, haga clic en él para iniciar al instalador.
3. Ejecute el instalador y elija el **instalar** botón. Esto instala WebMatrix y Web Pages.

## <a id="New_and_Enhanced_Features"></a>  Características nuevas y mejoradas

### <a id="Changes_for_the_RC_Version"></a>  Cambios de la versión RC (junio de 2012)

La versión RC en junio de 2012 tiene unos pocos cambios de la actualización de versión Beta que se lanzó en marzo de 2012. Estos cambios son:

- Un `Validation.AddFormError` método se agregó a la `Validation` auxiliar. Esto es útil si realiza una validación manualmente (por ejemplo, valida un valor que se pasa en la cadena de consulta) y desea agregar un mensaje de error que se puede mostrar mediante el `Html.ValidationSummary` método. Para obtener más información, consulte la sección [validar los datos que no proceden directamente de los usuarios](https://go.microsoft.com/fwlink/?LinkId=253002#Validating_Data_That_Doesnt_Come_Directly_from_Users) en [validar la entrada del usuario en sitios de ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253002).
- Se quitó la funcionalidad de unión y minificación de los ensamblados básicos de ASP.NET Web Pages 2. Como consecuencia, el `Assets` auxiliar aparece más adelante en este documento no está disponible. En su lugar, debe instalar la [optimización ASP.NET](http://nuget.org/packages/Microsoft.Web.Optimization/0.1) paquete NuGet. Para obtener más información, consulte [la unión y Minificar recursos en un sitio de ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=255373).
- Se han agregado los ensamblados adicionales para admitir ASP.NET Web Pages 2. El efecto de este cambio solo notable es que es posible que vea varios ensamblados en un sitio *bin* carpeta después de crear un sitio o implementar un sitio en un proveedor de hospedaje.

<a id="Changes_for_the_Beta_Version"></a>
### <a name="changes-for-the-beta-version-february-2012"></a>Cambios de la versión Beta (febrero de 2012)

La versión Beta, lanzada en febrero de 2012 tiene sólo unos pocos cambios de la versión Beta que se lanzó en diciembre de 2011. Estos cambios son:

- Razor ahora admite atributos condicionales. En un elemento HTML elemento, si establece un atributo en un valor que se resuelve en el código de servidor para `false` o `null`, ASP.NET no procesa el atributo. Por ejemplo, imagine que tiene el siguiente marcado para una casilla de verificación:

    [!code-html[Main](top-features-in-web-pages-2/samples/sample1.html)]

    Si el valor de `checked1` se resuelve como `false` o a `null`, el `checked` no se representa el atributo. Se trata de un cambio importante.
- El `Validation.GetHtml` método ha cambiado a `Validation.For`. Esto es un cambio brusco; `Validation.GetHtml` no funcionará en la versión Beta.
- Ahora puede incluir el `~` operador en el marcado para hacer referencia a la raíz del sitio sin utilizar el `Href` función. (Es decir, el analizador Razor ahora puede buscar y resolver el `~` operador sin necesidad de una llamada al método explícito `Href`.) El `Href` método todavía funciona, por lo que esto no es un cambio importante.

    Por ejemplo, si anteriormente tenía marcas como esta:

    `<a href="@Href("~/Default.cshtml")">Home</a>`

    Ahora puede usar marcas como esta:

    `<a href="~/Default.cshtml">Home</a>`
- El `Scripts` auxiliar para la administración de activos (recurso) se ha reemplazado por el `Assets` aplicación auxiliar, que tiene métodos ligeramente diferentes, como la siguiente:

  - Para `Scripts.Add`, usar `Assets.AddScript`
  - Para `Scripts.GetScriptTags`, usar `Assets.GetScripts`

    Esto es un cambio brusco; la `Scripts` clase no está disponible en la versión Beta. Se han actualizado los ejemplos de código en este documento que utilice Administración de activos con este cambio.

<a id="templates"></a>
### <a name="using-the-new-and-updated-site-templates"></a>Uso de las plantillas de sitio nuevas y actualizadas

El **Starter Site** plantilla se ha actualizado para que se ejecute en las páginas Web 2 de forma predeterminada. También incluye las siguientes capacidades nuevas:

- Representación de página móvil. Mediante el uso de estilos CSS y `@media` selector, el **Starter Site** proporciona una representación mejorada de páginas en pantallas más pequeñas, incluidas las pantallas de dispositivo móvil.
- Opciones mejoradas de pertenencia y autenticación. Puede permitir que los usuarios inicien sesión en su sitio con sus cuentas de otros sitios de redes sociales como Twitter, Facebook y Windows Live. Para obtener más información, consulte el [habilitar inicios de sesión de Facebook y otros sitios con OAuth y OpenID](#oauthsetup) sección.
- Elementos de HTML5.

El nuevo **sitio Personal** plantilla le permite crear un sitio Web que contiene un blog personal, una página de fotos y una página de Twitter. Puede personalizar un sitio basado en la **sitio Personal** plantilla haciendo lo siguiente:

- Cambiar el aspecto del sitio, edite el archivo de diseño (*\_SiteLayout.cshtml*) y el archivo de estilos (*Site.css*).
- Instalar paquetes de NuGet que agregan funcionalidad a su sitio. Para obtener información acerca de cómo instalar paquetes, incluidos ASP.NET Web Helpers Library, consulte el tutorial [instalar las aplicaciones auxiliares de](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers).

Para tener acceso a la **sitio Personal** plantilla, elija **plantillas** en el WebMatrix **inicio rápido** pantalla.

[![topseven-personalsite-1](top-features-in-web-pages-2/_static/image2.png)](top-features-in-web-pages-2/_static/image1.png)

En el **plantillas** diálogo cuadro, elija el **sitio Personal** plantilla.

[![topseven-personalsite-2](top-features-in-web-pages-2/_static/image4.png)](top-features-in-web-pages-2/_static/image3.png)

La página de aterrizaje de la **sitio Personal** plantilla le permite seguir los vínculos de configurar su blog, Twitter y fotos página.

[![topseven-personalsite-3](top-features-in-web-pages-2/_static/image6.png)](top-features-in-web-pages-2/_static/image5.png)

<a id="validation"></a>
### <a name="validating-user-input"></a>Validar la entrada del usuario

En Web Pages 1, para validar la entrada de usuario en los formularios de envío, se usa el `System.Web.WebPages.Html.ModelState` clase. (Esto se muestra en varios de los ejemplos de código en el tutorial de Web Pages 1 titulado [trabajar con datos](../data/5-working-with-data.md).) Todavía puede usar este enfoque en Web Pages 2. Sin embargo, Web Pages 2 también ofrece herramientas mejoradas para validar la entrada del usuario:

- Nuevas clases de validación, incluidas `System.Web.WebPages.ValidationHelper` y `System.Web.WebPages.Validator`, que le permiten realizar tareas de validación eficaz con unas pocas líneas de código.
- Opcionalmente, la validación del lado cliente, que ofrece comentarios inmediatos al usuario en lugar de requerir un ida y vuelta al servidor para comprobar errores de validación. (Por motivos de seguridad, validación se realiza en el servidor incluso si se han realizado las comprobaciones en el cliente con antelación).

Para usar las nuevas características de validación, haga lo siguiente:

En el código de la página, registrar un elemento que se valida mediante el uso de métodos de la `Validation` auxiliar: `Validation.RequireField`, `Validation.RequireFields` (para registrar varios elementos para que sea necesaria), o `Validation.Add`. El `Add` método le permite especificar otros tipos de comprobaciones de validación, como el tipo de datos comprobar, comparando las entradas de los distintos campos, las comprobaciones de longitud de cadena y patrones (mediante expresiones regulares). A continuación se muestran algunos ejemplos:

[!code-html[Main](top-features-in-web-pages-2/samples/sample2.html)]

Para mostrar un error específico del campo, llame a `Html.ValidationMessage` en el marcado para cada elemento que se está validando:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample3.cshtml)]

Para mostrar un resumen (`<ul>` lista) de todos los errores en la página, `Html.ValidationSummary` en el marcado:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample4.cshtml)]

Estos pasos son suficientes para implementar la validación del lado servidor. Si desea agregar la validación del lado cliente, siga este procedimiento además.

Agregue las siguientes referencias de archivo de script dentro de la `<head>` sección de una página web. Las primeras dos referencias de script de punto a archivos remotos en un servidor de content delivery network (CDN). El tercer referencia que apunta a un archivo de script local. Las aplicaciones de producción deben implementar una acción de reserva cuando la red CDN no está disponible. Probar la reserva.

[!code-html[Main](top-features-in-web-pages-2/samples/sample5.html)]

La manera más fácil de obtener una copia local de la *jquery.validate.unobtrusive.min.js* biblioteca consiste en crear un nuevo sitio de páginas Web basado en una de las plantillas de sitio (por ejemplo, el sitio de inicio). El sitio creado por la plantilla incluye *jquery.validate.unobtrusive.js* en su carpeta de Scripts, desde donde puede copiarla en su sitio.

Si el sitio Web usa un<em>\_SiteLayout</em> página para controlar el diseño de página, puede incluir estas referencias de script en esa página para que la validación está disponible en todas las páginas de contenido. Si desea realizar la validación solo en determinadas páginas, puede usar el Administrador de recursos para registrar los scripts en únicamente aquellas páginas. Para ello, llame a `Assets.AddScript(path)` en la página que desea validar y hacer referencia a cada uno de los archivos de script. A continuación, agregue una llamada a `Assets.GetScripts` en el  <em>\_SiteLayout</em> página con el fin de representar registrado `<script>` etiquetas. Para obtener más información, consulte la sección [registrar Scripts con el Administrador de recursos](#resmanagement).

En el marcado para un elemento individual, llame a la `Validation.For` método. Este método genera atributos que jQuery se puede enlazar con el fin de proporcionar la validación del lado cliente. Por ejemplo:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample6.cshtml)]

El ejemplo siguiente muestra una página que valida la entrada de usuario en un formulario. Para ejecutar y probar este código de validación, haga esto:

1. Crear un nuevo sitio web mediante una de las plantillas de sitio de WebMatrix 2 incluye un *Scripts* carpeta, como el **Starter Site** plantilla.
2. En el nuevo sitio, cree un nuevo *.cshtml* página y reemplace el contenido de la página con el código siguiente.
3. Ejecute la página en un explorador. Escriba los valores válidos y no válidos para ver los efectos de validación. Por ejemplo, deje en blanco un campo obligatorio o escriba una letra en el **créditos** campo.


[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample7.cshtml)]

Esta es la página cuando un usuario envía una entrada válida:

[![1 topSeven válido](top-features-in-web-pages-2/_static/image8.png)](top-features-in-web-pages-2/_static/image7.png)

Esta es la página cuando un usuario envía con un campo obligatorio que se deja en blanco:

[![2 topSeven válido](top-features-in-web-pages-2/_static/image10.png)](top-features-in-web-pages-2/_static/image9.png)

Esta es la página cuando un usuario envía con un valor distinto de un entero en el **créditos** campo:

[![3 topSeven válido](top-features-in-web-pages-2/_static/image12.png)](top-features-in-web-pages-2/_static/image11.png)

Para obtener más información, consulte las siguientes entradas de blog:

- [Actualiza la validación en Web Pages v2](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2344) Fundamentos de la incorporación de validación mediante el `Validation` auxiliar (servidor sólo)
- [Actualiza la validación en Web Pages v2, parte 2](http://www.mikepope.com/blog/DisplayBlog.aspx?permalink=2347) agregar la validación del lado cliente.
- [Actualiza la validación en Web Pages v2, parte 3](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2351) errores de validación de formato.

<a id="resmanagement"></a>
### <a name="registering-scripts-using-the-assets-manager"></a>Registrar Scripts mediante el Administrador de recursos

El Administrador de recursos es una característica nueva que puede usar en código del servidor para registrar y representar los scripts de cliente. Esta característica es útil cuando se trabaja con código de varios archivos (por ejemplo, las páginas de diseño, las páginas de contenido, las aplicaciones auxiliares, etc.) que se combinan en una sola página en tiempo de ejecución. El Administrador de recursos coordina para asegurarse de que se hace referencia correctamente a los archivos de script y eficaz en la página representada, independientemente de qué archivos de código se les llama desde o cuántas veces se denominan los archivos de origen. El Administrador de recursos que también presenta `<script>` etiquetas en el lugar correcto para que la página puede cargar rápidamente (sin tener que descargar los scripts durante el procesamiento) y evitar errores que pueden producirse si se llama a los scripts antes de la representación completada.

Por ejemplo, suponga que crea una aplicación auxiliar personalizada que llama a un archivo JavaScript y llamar a esta aplicación auxiliar en tres lugares diferentes en el código de la página de contenido. Si no usa el Administrador de recursos para registrar el script llama a en la aplicación auxiliar, tres diferentes `<script>` etiquetas que apunten al mismo archivo de script aparecerá en la página representada. Además, dependiendo de dónde el `<script>` etiquetas se insertan en la página representada, pueden producirse errores si el script intenta tener acceso a determinados elementos de página antes de la página se cargue completamente. Si usa el Administrador de recursos para registrar la secuencia de comandos, evitar estos problemas.

Puede registrar un script con el Administrador de recursos haciendo lo siguiente:

- En el código que necesita para hacer referencia a la secuencia de comandos, llame a la `Assets.AddScript` método.
- En un  *\_SiteLayout* página, llame a la `Assets.GetScripts` método para representar el `<script>` etiquetas. 

    > [!NOTE]
    > Coloque las llamadas a `Assets.GetScripts` como el último elemento de la `<body>` elemento de la  *\_SiteLayout* página. Esto ayuda a la página se cargan más rápido y puede ayudar a evitar errores de script.

El ejemplo siguiente muestra cómo funciona el Administrador de recursos. El código contiene los siguientes elementos:

- Una aplicación auxiliar personalizada denominada `MakeNote`. Esta aplicación auxiliar representa una cadena dentro de un cuadro ajustando un `div` elemento alrededor de ella que haya un estilo con un borde y agregando &quot;Nota:&quot; a él. La aplicación auxiliar también llama a un archivo JavaScript que agrega el comportamiento de tiempo de ejecución a la nota. En lugar de hacer referencia a la secuencia de comandos con un `<script>` etiqueta, la aplicación auxiliar registra el script mediante una llamada a `Assets.AddScript` .
- Un archivo JavaScript. Este es el archivo que se llama a la aplicación auxiliar y aumenta temporalmente el tamaño de fuente de las notas durante un `mouseover` eventos.
- Una página de contenido, que hace referencia a la<em>\_SiteLayout</em> representa algún contenido en el cuerpo de la página y, a continuación, llama a la `MakeNote` auxiliar.
- Un  *\_SiteLayout* página. Esta página proporciona un encabezado común y una estructura de diseño de página. También incluye una llamada a `Assets.GetScripts`, que es cómo el Administrador de recursos representa la secuencia de llamadas en una página.

Para ejecutar el ejemplo:

1. Crear un sitio Web vacío de Web Pages 2. Puede usar el WebMatrix **sitio vacío** plantilla para esto.
2. Cree una carpeta denominada *Scripts* en el sitio.
3. En el *Scripts* carpeta, cree un archivo denominado *Test.js*, copia el *Test.js* del ejemplo de contenido en él y guarde el archivo...
4. Cree una carpeta denominada *aplicación\_código* en el sitio.
5. En el *aplicación\_código* carpeta, cree un archivo denominado *Helpers.cshtml*, copie el código de ejemplo en él y guárdelo en una carpeta denominada *aplicación\_código*en la carpeta raíz.
6. En la carpeta raíz de la carpeta del sitio, cree un archivo denominado  *\_SiteLayout.cshtml,* copie el ejemplo en él y guarde el archivo.
7. En la raíz del sitio, cree un archivo denominado *ContentPage.cshtml*, agregue el código de ejemplo y guárdelo.
8. Ejecute *ContentPage* en un explorador. La cadena pasó a la `MakeNote` auxiliar se representa como una nota con conversión boxing.
9. Pasar el puntero del mouse a través de la nota. El script aumenta temporalmente el tamaño de fuente de la nota.
10. Ver el origen de la página representada. Debido a donde colocó la llamada a `Assets.GetScripts`, el texto representado `<script>` etiqueta a la que llama a *Test.js* es el último elemento en el cuerpo de la página.

*Test.js*

[!code-javascript[Main](top-features-in-web-pages-2/samples/sample8.js)]

*Helpers.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample9.cshtml)]

*\_SiteLayout.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample10.html)]

*ContentPage.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample11.cshtml)]

La siguiente captura de pantalla *ContentPage.cshtml* en un explorador cuando mantenga el puntero del mouse sobre la Nota:

[![topSeven-resmgr-1](top-features-in-web-pages-2/_static/image14.png)](top-features-in-web-pages-2/_static/image13.png)

<a id="oauthsetup"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Habilitar los inicios de sesión de Facebook y otros sitios con OAuth y OpenID

Web Pages 2 proporciona opciones mejoradas para la pertenencia y autenticación. La mejora de la principal es que hay nueva [OAuth](http://oauth.net/) y [OpenID](http://openid.net/) proveedores. Con estos proveedores, puede permitir que los usuarios inicien sesión en su sitio mediante sus credenciales de Facebook, Twitter, Windows Live, Google y Yahoo. Por ejemplo, para iniciar sesión con una cuenta de Facebook, los usuarios pueden elegir simplemente un icono de Facebook, que redirige a la página de inicio de sesión de Facebook donde que escriba su información de usuario. A continuación, puede asociar el inicio de sesión de Facebook con su cuenta en su sitio. Una mejora relacionada a las características de pertenencia de las páginas Web es que los usuarios pueden asociar varios inicios de sesión (incluidos los inicios de sesión de sitios de redes sociales) con una sola cuenta en su sitio Web.

Esta imagen muestra la página de inicio de sesión desde el **Starter Site** plantilla, donde un usuario puede elegir un icono de Facebook, Twitter o Windows Live para habilitar el inicio de sesión con una cuenta externa:

[![topSeven-oauth-1](top-features-in-web-pages-2/_static/image16.png)](top-features-in-web-pages-2/_static/image15.png)

Puede habilitar la pertenencia de OAuth y OpenID mediante unas pocas líneas de código. Los métodos y propiedades se utiliza para trabajar con el de OAuth y OpenID proveedores están en el `WebMatrix.Security.OAuthWebSecurity` clase.

Sin embargo, en lugar de usar código para habilitar los inicios de sesión desde otros sitios, una manera recomendada para empezar a trabajar con los nuevos proveedores es usar el nuevo **Starter Site** plantilla que se incluye con la versión Beta 2 de WebMatrix. El **Starter Site** plantilla incluye una infraestructura completa de pertenencia, junto con una página de inicio de sesión, una base de datos de pertenencia y todo el código necesario permitir que los usuarios inicien sesión en su sitio mediante credenciales locales o los de otro sitio .

#### <a name="how-to-enable-logins-using-the-oauth-and-openid-providers"></a>Cómo habilitar los inicios de sesión con los proveedores de OpenID y OAuth

Esta sección proporciona un ejemplo de cómo permitir que los usuarios inicien sesión desde sitios externos (Facebook, Twitter, Windows Live, Google o Yahoo) a un sitio que se basa en el **Starter Site** plantilla. Después de crear un sitio de inicio, siga este (detalles de seguimiento):

- Para los sitios que usan un proveedor de OAuth (Facebook, Twitter y Windows Live), cree una aplicación en el sitio externo. Esto proporciona las claves de aplicación que necesitará para invocar la característica de inicio de sesión para esos sitios. Para los sitios que usan un proveedor de OpenID (Google, Yahoo), no es necesario que crear una aplicación. Para todos estos sitios, debe tener una cuenta con el fin de iniciar sesión y crear aplicaciones de desarrollador. 

    > [!NOTE]
    > Las aplicaciones de Windows Live aceptan sólo una dirección URL en directo para un sitio Web en funcionamiento, por lo que no puede usar una dirección URL del sitio Web local para probar los inicios de sesión.
- Editar algunos archivos en su sitio Web con el fin de especificar el proveedor de autenticación adecuado y enviar un inicio de sesión en el sitio que desea usar.

**Para habilitar los inicios de sesión de Google y Yahoo**:

1. En su sitio Web, edite el  *\_AppStart.cshtml* página y agregue las siguientes dos líneas de código en el bloque de código de Razor después de llamar a la `WebSecurity.InitializeDatabaseConnection` método. Este código permite a los proveedores el OpenID Yahoo y Google. 

    [!code-css[Main](top-features-in-web-pages-2/samples/sample12.css)]
2. En el *~/Account/Login.cshtml* página, quite los comentarios de los siguientes `<fieldset>` bloque de marcado cerca del final de la página. Para quitar los comentarios del código, quite el `@*` caracteres que preceden y siguen el `<fieldset>` bloque. El bloque de código resultante tendrá este aspecto:

    [!code-html[Main](top-features-in-web-pages-2/samples/sample13.html)]
3. Agregar un `<input>` (elemento) para el proveedor de Google o Yahoo el `<fieldset>` grupo en el *~/Account/Login.cshtml* página. La actualización `<fieldset>` grupo con `<input>` elementos de Google y Yahoo parece similar al ejemplo siguiente: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample14.html)]
4. En el *~/Account/AssociateServiceAccount.cshtml* página, agregue `<input>` elementos de Google o Yahoo a la `<fieldset>` grupo cerca del final del archivo. Puede copiar el mismo `<input>` elementos que acaba de agregar a la `<fieldset>` sección la *~/Account/Login.cshtml* página. 

    El *~/Account/AssociateServiceAccount.cshtml* página en la plantilla de sitio inicial se puede usar si desea crear una página en el que los usuarios pueden asociar varios inicios de sesión desde otros sitios con una sola cuenta en su sitio Web.

Ahora puede probar los inicios de sesión de Google y Yahoo.

1. Ejecute el *default.cshtml* página del sitio y elija el **iniciarla** botón.
2. En el *inicio de sesión* página, en el **utilice otro servicio para iniciar sesión** sección, elija el **Google** o **Yahoo** botón de envío. Este ejemplo usa el inicio de sesión de Google. 

    La página web redirige la solicitud a la página de inicio de sesión de Google.

    [![topSeven-oauth-6](top-features-in-web-pages-2/_static/image18.png)](top-features-in-web-pages-2/_static/image17.png)
3. Escriba las credenciales para una cuenta de Google existente.
4. Si Google le pregunta si desea permitir Localhost para usar la información de la cuenta, haga clic en **permitir**.

    El código usa el token de Google para autenticar al usuario y, a continuación, vuelve a esta página en su sitio Web. Esta página permite a los usuarios asociar su inicio de sesión de Google con una cuenta existente en su sitio Web o puede registrar una nueva cuenta en su sitio para asociar con el inicio de sesión externo.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image20.png)](top-features-in-web-pages-2/_static/image19.png)
5. Elija la **asociar** botón. El explorador vuelve a la página principal de la aplicación.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image22.png)](top-features-in-web-pages-2/_static/image21.png)

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image24.png)](top-features-in-web-pages-2/_static/image23.png)

**Para habilitar los inicios de sesión de Facebook**:

1. Vaya a la [sitio de desarrolladores de Facebook](https://developers.facebook.com/apps) (inicie sesión si aún no ha iniciado sesión).
2. Elija la **crear nueva aplicación** botón y, a continuación, siga las indicaciones para nombrar y crear la nueva aplicación.
3. En la sección **Seleccione cómo se integrará su aplicación con Facebook**, elija el **sitio Web** sección.
4. Rellene el **dirección URL del sitio** campo con la dirección URL del sitio (por ejemplo, [ `http://www.example.com` ](http://www.example.com)). El **dominio** campo es opcional; puede usar esto para proporcionar autenticación para un dominio completo (como *ejemplo.com*). 

    > [!NOTE]
    > Si está ejecutando un sitio en el equipo local con una dirección URL como `http://localhost:12345` (donde el número es un número de puerto local), puede agregar este valor para el **dirección URL del sitio** campo para probar su sitio. Sin embargo, cada vez que el número de puerto de los cambios del sitio local, deberá actualizar el **dirección URL del sitio** campo de la aplicación.
5. Elija la **guardar cambios** botón.
6. Elija la **aplicaciones** pestaña nuevo y, a continuación, ver la página de inicio para la aplicación.
7. Copia el **Id. de aplicación** y **secreto de la aplicación** valores para la aplicación y péguelos en un archivo de texto temporal. En el código de sitio Web pasará estos valores para el proveedor de Facebook.
8. Salga del sitio para desarrolladores de Facebook.

Ahora realizar cambios en dos páginas en su sitio Web para que los usuarios podrán iniciar sesión en el sitio mediante sus cuentas de Facebook.

1. En su sitio Web, edite el  *\_AppStart.cshtml* página y quite los comentarios del código para el proveedor de Facebook OAuth. El bloque de código de marca de comentario tiene el siguiente aspecto: 

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample15.xml)]
2. Copia el **Id. de aplicación** valor de la aplicación de Facebook como el valor de la `consumerKey` parámetro (dentro de las comillas).
3. Copia **secreto de la aplicación** valor de la aplicación de Facebook como el `consumerSecret` el valor del parámetro.
4. Guarde y cierre el archivo.
5. Editar el *~/Account/Login.cshtml* página y quite los comentarios de la `<fieldset>` bloque cerca del final de la página. Para quitar los comentarios del código, quite el `@*` caracteres que preceden y siguen el `<fieldset>` bloque. El bloque de código con comentarios elimina es similar al siguiente: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample16.html)]
6. Guarde y cierre el archivo.

Ahora puede probar el inicio de sesión de Facebook.

1. Ejecute el sitio *default.cshtml* página y elija el **inicio de sesión** botón.
2. En el *inicio de sesión* página, en el **utilice otro servicio para iniciar sesión** sección, elija el **Facebook** icono. 

    La página web redirige la solicitud a la página de inicio de sesión de Facebook.

    [![topSeven-oauth-2](top-features-in-web-pages-2/_static/image26.png)](top-features-in-web-pages-2/_static/image25.png)
3. Inicie sesión en una cuenta de Facebook. 

    El código usa el token de Facebook para la autenticación y, a continuación, vuelve a una página donde puede asociar el inicio de sesión de Facebook con inicio de sesión de su sitio. Su dirección de correo electrónico o nombre de usuario se rellena en el **correo electrónico** campo en el formulario.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image28.png)](top-features-in-web-pages-2/_static/image27.png)
4. Elija la **asociar** botón. 

    El explorador vuelve a la página principal y que haya iniciado sesión.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image30.png)](top-features-in-web-pages-2/_static/image29.png)

**Para habilitar los inicios de sesión de Twitter:** 

1. Vaya a la [sitio de desarrolladores de Twitter](https://dev.twitter.com/).
2. Elija la **crear una aplicación** vincular y, a continuación, inicie sesión en el sitio.
3. En el **crear una aplicación** forman, rellene el **nombre** y **descripción** campos.
4. En el **sitio Web** , escriba la dirección URL del sitio (por ejemplo, [ `http://www.example.com` ](http://www.example.com)). 

    > [!NOTE]
    > Si va a probar su sitio local (mediante una dirección URL como `http://localhost:12345`), Twitter no puede aceptar la dirección URL. Sin embargo, es posible que pueda usar la dirección IP de bucle invertido local (por ejemplo `http://127.0.0.1:12345`). Esto simplifica el proceso de probar la aplicación localmente. Sin embargo, cada vez que cambia el número de puerto del sitio local, deberá actualizar el **sitio Web** campo de la aplicación.
5. En el **dirección URL de devolución de llamada** , escriba una dirección URL de la página en su sitio Web que desea que los usuarios vuelvan a tras iniciar sesión en Twitter. Por ejemplo, para enviar a los usuarios a la página principal del sitio de inicio (que reconozca su estado ha iniciado sesión), escriba la misma dirección URL que escribió en el **sitio Web** campo.
6. Acepte los términos y elija el **crear aplicación de Twitter** botón.
7. En el **My Applications** aterrizaje de la página, elija la aplicación que creó.
8. En el **detalles** pestaña, desplácese hacia abajo y elija el **crear mi Token de acceso** botón.
9. En el **detalles** pestaña, copie el **clave de consumidor** y **secreto de consumidor** valores para la aplicación y péguelos en un archivo de texto temporal. Pasará estos valores para el proveedor de Twitter en el código de sitio Web.
10. Salga del sitio de Twitter.

Ahora realizar cambios en dos páginas en su sitio Web para que los usuarios podrán iniciar sesión en el sitio con sus cuentas de Twitter.

1. En su sitio Web, edite el  *\_AppStart.cshtml* página y quite los comentarios del código para el proveedor de OAuth de Twitter. El bloque de marca de comentario de código tiene este aspecto: 

    [!code-csharp[Main](top-features-in-web-pages-2/samples/sample17.cs)]
2. Copia el **clave de consumidor** valor de la aplicación de Twitter como el valor de la `consumerKey` parámetro (dentro de las comillas).
3. Copia el **secreto de consumidor** valor de la aplicación de Twitter como el valor de la `consumerSecret` parámetro.
4. Guarde y cierre el archivo.
5. Editar el *~/Account/Login.cshtml* página y quite los comentarios de la `<fieldset>` bloque cerca del final de la página. Para quitar los comentarios del código, quite el `@*` caracteres que preceden y siguen el `<fieldset>` bloque. El bloque de código con comentarios elimina es similar al siguiente: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample18.html)]
6. Guarde y cierre el archivo.

Ahora puede probar el inicio de sesión de Twitter.

1. Ejecute el *default.cshtml* página del sitio y elija el **inicio de sesión** botón.
2. En el *inicio de sesión* página, en el **utilice otro servicio para iniciar sesión** sección, elija el **Twitter** icono. 

    La página web redirige la solicitud a una página de inicio de sesión de Twitter para la aplicación que creó.

    [![topSeven-oauth-4](top-features-in-web-pages-2/_static/image32.png)](top-features-in-web-pages-2/_static/image31.png)
3. Inicie sesión en una cuenta de Twitter.
4. El código usa el token de Twitter para autenticar al usuario y, a continuación, vuelve a una página donde se puede asociar el inicio de sesión con su cuenta de sitio Web. Su dirección de correo electrónico o nombre se rellena en el **correo electrónico** campo en el formulario.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image34.png)](top-features-in-web-pages-2/_static/image33.png)
5. Elija la **asociar** botón. 

    El explorador vuelve a la página principal y que haya iniciado sesión.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image36.png)](top-features-in-web-pages-2/_static/image35.png)

<a id="maphelper"></a>
### <a name="adding-maps-using-the-maps-helper"></a>Agregar asignaciones mediante la aplicación auxiliar de mapas

Web Pages 2 incluye adiciones a ASP.NET Web Helpers Library, que es un paquete de complementos para un sitio Web Pages. Uno de ellos es un componente de asignación proporcionado por el `Microsoft.Web.Helpers.Maps` clase. Puede usar el `Maps` clase para generar asignaciones basadas en una dirección o en un conjunto de coordenadas de longitud y latitud. La `Maps` clase permite llamar a directamente en los motores de mapa populares como Bing, Google, Yahoo y MapQuest.

Para utilizar el nuevo `Maps` clase en su sitio Web, primero debe instalar la versión 2 de la biblioteca de aplicaciones auxiliares de Web. Para ello, vaya a las instrucciones para instalar la versión actualmente publicada de la [ASP.NET Web Helpers Library](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers) e instalar la versión 2.

Los pasos para agregar la asignación a una página son el mismo independientemente de cuál de los motores de mapa que llamar a. Solo tiene que agregar una referencia a la página de asignación del archivo de JavaScript y, a continuación, agregue una llamada que representa el `<script>` etiquetas en la página. A continuación, en la página de asignación, llamar al motor de mapa que desea usar.

El ejemplo siguiente muestra cómo crear una página que representa una asignación basada en una dirección y otra página que representa un mapa en función de las coordenadas de longitud y latitud. El ejemplo de asignación de dirección usa Google Maps y, en el ejemplo de asignación coordinada utiliza Bing Maps. Tenga en cuenta los siguientes elementos en el código:

- La llamada a `Assets.AddScript` en la parte superior de las dos páginas de asignación. Este método agrega una referencia a la *jquery 1.6.2.min.js* archivo que se incluye con el **Starter Site** plantilla y que requiere la `Maps` clase.
- La llamada a la `Assets.GetScripts` método en el archivo de diseño. Este método representa el `<script>` de etiquetas en las dos páginas de asignación.
- La llamada a la `@Maps.GetGoogleHtml` y `@Maps.GetBingHtml` métodos en las páginas de asignación. Para asignar una dirección, debe pasar una cadena de dirección. Para asignar las coordenadas, debe pasar la longitud y latitud coordenadas. Para el motor de Bing Maps, también se debe pasar una clave (de forma gratuita obtenerlo registrándose en el [sitio para desarrolladores de Bing Maps](https://www.microsoft.com/maps/developers/web.aspx)). Los métodos para los otros motores de mapa funcionan de manera similar (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).

Para crear páginas de asignación:

1. Crear un sitio Web basado en la **Starter Site** plantilla.
2. Cree un archivo denominado *MapAddress.cshtml* en la raíz del sitio. Esta página generará una asignación basada en una dirección que se pasa a él.
3. Copie el código siguiente en el archivo, sobrescribiendo el contenido existente. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample19.cshtml)]
4. Cree un archivo denominado  *\_MapLayout.cshtml* en la raíz del sitio. Esta página será la página de diseño para las dos páginas de asignación.
5. Copie el código siguiente en el archivo, sobrescribiendo el contenido existente. 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample20.html)]
6. Cree un archivo denominado *MapCoordinates.cshtml* en la raíz del sitio. Esta página generará un mapa según un conjunto de coordenadas que se pasan a él.
7. Copie el código siguiente en el archivo, sobrescribiendo el contenido existente. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample21.cshtml)]

Para probar las páginas de asignación:

1. Ejecute la página *MapAddress.cshtml* archivo.
2. Escriba una cadena de dirección completa, incluida una dirección postal, estado o provincia y código postal y, a continuación, elija el **Map It** botón. La página representa un mapa de Google Maps: 

    [![topseven-maphelper-1](top-features-in-web-pages-2/_static/image38.png)](top-features-in-web-pages-2/_static/image37.png)
3. Buscar un conjunto de coordenadas de latitud y longitud de una ubicación específica.
4. Ejecute la página *MapCoordinates.cshtml*. Escriba las coordenadas y, a continuación, elija el **Map It** botón. La página representa un mapa de Bing Maps: 

    [![topseven-maphelper-2](top-features-in-web-pages-2/_static/image40.png)](top-features-in-web-pages-2/_static/image39.png)

<a id="sidebyside"></a>
### <a name="running-web-pages-applications-side-by-side"></a>Ejecutar aplicaciones en paralelo de las páginas Web

Web Pages 2 agrega la capacidad para ejecutar aplicaciones en paralelo. Esto le permite continuar para ejecutar las aplicaciones de Web Pages 1, crear nuevas aplicaciones de Web Pages 2 y ejecutar todas ellas en el mismo equipo.

Estas son algunas cosas que recordar cuando se instala la versión Beta 2 de las páginas Web con WebMatrix:

- De forma predeterminada, las aplicaciones existentes de las páginas Web se ejecutarán como aplicaciones de la versión 2 en el equipo. (Los ensamblados para la versión 2 se instalan en la GAC y se usarán automáticamente).
- Si desea ejecutar un sitio mediante Web Pages versión 1 (en lugar del predeterminado, como se muestra en el punto anterior), puede configurar el sitio para hacerlo. Si el sitio ya no tiene un *web.config* de archivos en la raíz del sitio, cree uno y copie el siguiente código XML en él, sobrescribiendo el contenido existente. Si el sitio ya contiene un *web.config* , agregue un `<appSettings>` elemento como el siguiente a la `<configuration>` sección.

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample22.xml)]
  '-Si no especifica ninguna versión en el *web.config* archivo, un sitio se implementa como un sitio de la versión 2. (Los ensamblados de la versión 2 se copian en el *bin* carpeta en el sitio implementado.)
- Nuevas aplicaciones que se crean con las plantillas de sitio en la versión de Web Matrix Beta 2 se incluyen los ensamblados de la versión 2 de las páginas Web en el sitio *bin* carpeta.

En general, siempre puede controlar qué versión de las páginas Web para usar con su sitio mediante el uso de NuGet para instalar los ensamblados correspondientes en la carpeta del sitio *bin* carpeta. Para buscar paquetes, visite [NuGet.org](http://NuGet.org).

<a id="mobile"></a>
### <a name="rendering-pages-for-mobile-devices"></a>Representación de páginas para dispositivos móviles

Web Pages 2 le permite crear pantallas personalizadas para representar el contenido en dispositivos móviles o de otros dispositivos.

El `System.Web.WebPages` espacio de nombres contiene las siguientes clases que permiten trabajar con los modos de presentación: `DefaultDisplayMode`, `DisplayInfo`, y `DisplayModes`. Puede utilizar estas clases directamente y escribir código que presente la salida derecha para dispositivos específicos.

Como alternativa, puede crear páginas específicas del dispositivo mediante un patrón de nomenclatura de archivos similar al siguiente: <em>nombre de archivo.</em> <em>Mobile</em><em>.cshtml</em>. Por ejemplo, puede crear dos versiones de una página, uno denominado <em>MyFile.cshtml</em> y otro llamado <em>MyFile.Mobile.cshtml</em>. En tiempo de ejecución, cuando se solicita un dispositivo móvil <em>MyFile.cshtml</em>, páginas Web presenta el contenido de <em>MyFile.Mobile.cshtml</em>. En caso contrario, <em>MyFile.cshtml</em> se representa.

El ejemplo siguiente muestra cómo habilitar la representación de dispositivos móvil mediante la adición de una página de contenido para dispositivos móviles. *Page1.cshtml* contiene contenido además de una barra lateral de navegación. *Page1.Mobile.cshtml* contiene el mismo contenido, pero omite la barra lateral.

Para compilar y ejecutar el ejemplo de código:

1. En un sitio Web Pages, cree un archivo denominado *Page1.cshtml* y copie el *Page1.cshtml* contenido en él en el ejemplo.
2. Cree un archivo denominado *Page1.Mobile.cshtml* y copie el *Page1.Mobile.cshtml* contenido en él en el ejemplo. Tenga en cuenta que la versión móvil de la página omite la sección de exploración para representar mejor en una pantalla más pequeña.
3. Ejecute un explorador de escritorio y vaya a *Page1.cshtml*.
4. Ejecute un explorador móvil (o un emulador de dispositivos móviles) y vaya a *Page1.cshtml*. Tenga en cuenta que esta vez páginas Web presenta la versión móvil de la página. 

    > [!NOTE]
    > Para probar las páginas móviles, puede usar un simulador de dispositivos móviles que se ejecuta en un equipo de escritorio. Esta herramienta le permite probar las páginas web tal como aparecen en los dispositivos móviles (es decir, normalmente con mucho menor Mostrar área). Un ejemplo de un simulador es el [complemento modificador del agente de usuario](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/) para Mozilla Firefox, que le permite emular varios exploradores móviles desde una versión de escritorio de Firefox.

*Page1.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample23.html)]

*Page1.Mobile.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample24.html)]

*Page1.cshtml* representados en un explorador de escritorio:

[![topseven-displaymodes-1](top-features-in-web-pages-2/_static/image42.png)](top-features-in-web-pages-2/_static/image41.png)

*Page1.Mobile.cshtml* muestra en una vista de simulador de iPhone de Apple en el explorador Firefox. Aunque la solicitud es *Page1.cshtml*, las representaciones de la aplicación *Page1.Mobile.cshtml*.

[![topseven-displaymodes-2](top-features-in-web-pages-2/_static/image44.png)](top-features-in-web-pages-2/_static/image43.png)

<a id="resources"></a>
## <a name="additional-resources"></a>Recursos adicionales

### <a name="aspnet-web-pages-1-resources"></a>Recursos de ASP.NET Web Pages 1

> [!NOTE]
> La mayoría de los recursos de la API y programación de Web Pages 1 todavía se aplican a las páginas Web 2.

- [Introducción a ASP.NET Web Pages de programación](https://go.microsoft.com/fwlink/?LinkId=202890)

### <a name="webmatrix-resources"></a>Recursos de WebMatrix

- [WebMatrix 2 Novedades](http://webmatrix.com/next)
- [Sitio de Microsoft WebMatrix](https://go.microsoft.com/fwlink/?LinkID=195076)
- [Iniciar el desarrollo de Web con Microsoft WebMatrix](https://msdn.microsoft.com/en-us/library/hh145669(v=VS.99).aspx)(incluye una aplicación de páginas Web de ejemplo completos)
