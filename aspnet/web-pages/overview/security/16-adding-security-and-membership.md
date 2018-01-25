---
uid: web-pages/overview/security/16-adding-security-and-membership
title: Agregar seguridad y pertenencia a ASP.NET Web Pages (Razor) sitio | Documentos de Microsoft
author: tfitzmac
description: "Este capítulo muestra cómo proteger su sitio Web para que estén disponibles sólo para las personas que inicien sesión en algunas de las páginas. (También verá cómo crear páginas tha..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/24/2014
ms.topic: article
ms.assetid: 7a77c2c0-deea-4290-a9c3-97958891758e
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/16-adding-security-and-membership
msc.type: authoredcontent
ms.openlocfilehash: af2eeb128cff554e7ae3d903e2117861087344e9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="adding-security-and-membership-to-an-aspnet-web-pages-razor-site"></a>Adición de seguridad y pertenencia a un sitio de ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artículo explica cómo proteger un sitio Web de ASP.NET Web Pages (Razor) para que estén disponibles sólo para las personas que inicien sesión en algunas de las páginas. (También verá cómo crear páginas que cualquiera puede tener acceso.)
> 
> **Lo que aprenderá:** 
> 
> - Cómo crear un sitio Web que tiene una página de registro y una página de inicio de sesión, por lo que para algunas páginas puede limitar el acceso a solo los miembros.
> - Cómo crear páginas públicas y miembro.
> - Cómo definir roles, que son grupos que tienen permisos de seguridad diferente en su sitio, y cómo asignar usuarios a un rol.
> - Describe cómo usar CAPTCHA para impedir que los programas automatizados (bots) crear cuentas de miembro.
>   
> 
> Estas son las características ASP.NET presentadas en el artículo:
> 
> - El WebMatrix **Starter Site** plantilla.
> - El `WebSecurity` auxiliar y `Roles` clase.
> - El `ReCaptcha` auxiliar.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - 3 de WebMatrix
> - ASP.NET Web Helpers Library


Puede configurar su sitio Web para que los usuarios pueden iniciar sesión en &#8212; es decir, para que el sitio admite *pertenencia*. Esto puede ser útil por diversos motivos. Por ejemplo, el sitio podría tener páginas que deben estar disponibles solo para miembros. En algunos casos, es posible que necesite los usuarios inicien sesión con el fin de enviar comentarios o deje un comentario.

Incluso si su sitio Web es compatible con la pertenencia, los usuarios no son necesariamente requeridos para iniciar sesión para usar algunas de las páginas en el sitio. Los usuarios que no inició sesión en se conocen como *a los usuarios anónimos*.

Un usuario puede registrar en el sitio Web y, a continuación, puede iniciar sesión en el sitio. El sitio Web requiere un nombre de usuario (una dirección de correo electrónico) y una contraseña para confirmar que los usuarios son quienes dicen ser. Este proceso de inicio de sesión y confirmar la identidad de un usuario se conoce como *autenticación*.

Puede establecer la seguridad y la pertenencia de maneras diferentes:

- Si usa WebMatrix, una forma sencilla consiste en crear como nuevo sitio basado en la **Starter Site** plantilla. Esta plantilla ya está configurada para la seguridad y la pertenencia y ya tiene una página de registro, una página de inicio de sesión y así sucesivamente.

    El sitio creado mediante la plantilla también tiene una opción para permitir a los usuarios iniciar sesión con un sitio externo como Facebook, Google, o Twitter.
- Si desea agregar seguridad a un sitio existente, o si no desea usar el **Starter Site** plantilla, puede crear su propia página de registro, página de inicio de sesión y así sucesivamente.

En este artículo se centra en la primera opción &mdash; cómo aumenta la seguridad mediante el uso de la **Starter Site** plantilla. También proporciona cierta información básica sobre cómo implementar su propia seguridad y, a continuación, se proporcionan vínculos para obtener más información acerca de cómo hacerlo. También hay información sobre cómo habilitar los inicios de sesión externos, que se describe con más detalle en otro artículo.

## <a name="creating-website-security-using-the-starter-site-template"></a>Creación de seguridad del sitio Web mediante la plantilla de sitio de inicio

En WebMatrix, puede usar el **Starter Site** plantilla para crear un sitio Web que contiene lo siguiente:

- Una base de datos que se utiliza para almacenar los nombres de usuario y contraseñas de los miembros.
- Una página de registro en la que pueden registrar los usuarios anónimos de (nuevos).
- Una página de inicio de sesión y cierre de sesión.
- Una página de recuperación y restablecimiento de contraseña.

El siguiente procedimiento describe cómo crear el sitio y configurarlo.

1. Iniciar WebMatrix y en el **inicio rápido** página, seleccione **plantilla de sitio**.
2. Seleccione el **Starter Site** plantilla y, a continuación, haga clic en **Aceptar**. WebMatrix crea un nuevo sitio.
3. En el panel izquierdo, haga clic en el **archivos** selector de área de trabajo.
4. En la carpeta raíz de su sitio Web, abra el  *\_AppStart.cshtml* archivo, que es un archivo especial que se usa para contener la configuración global. Contiene algunas instrucciones que están comentados utilizando el `//` caracteres:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample1.cs)]

    Estas instrucciones configurar el `WebMail` auxiliar, que se puede usar para enviar correo electrónico. El sistema de pertenencia puede usar el correo electrónico para enviar mensajes de confirmación cuando un usuario registra o cuando desee cambiar sus contraseñas. (Por ejemplo, después de que los usuarios se registran, reciben un correo electrónico que incluye un vínculo que puede hacer clic con el fin de finalizar el proceso de registro.)

    Enviar correo electrónico requiere acceso a un servidor SMTP, como se describe en [Agregar correo electrónico a un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202899). Va a almacenar la configuración de correo electrónico en este centro  *\_AppStart.cshtml* de archivos para que no tengan que escriba el código varias veces en cada página que puede enviar correo electrónico. (No es necesario especificar la configuración de SMTP para configurar una base de datos de registro; solo necesita una configuración de SMTP si desea validar a los usuarios de sus alias de correo electrónico y permiten a los usuarios restablecer una contraseña olvidada).
5. Quite las instrucciones mediante la eliminación de `//` delante de cada uno de ellos.

    Si no desea configurar la confirmación por correo electrónico, puede omitir este paso y el paso siguiente. Si no se establecen los valores de SMTP, la nueva cuenta está inmediatamente disponible sin un correo electrónico de confirmación.
6. Modificar las siguientes opciones relacionadas con el correo electrónico en el código:

    - Establecer `WebMail.SmtpServer` en el nombre del servidor SMTP que tienen acceso a.
    - Deje `WebMail.EnableSsl` establecido en `true`. Esta configuración protege las credenciales que se envían al servidor SMTP mediante su cifrado.
    - Establecer `WebMail.UserName` al nombre de usuario para la cuenta del servidor SMTP.
    - Establecer `WebMail.Password` como la contraseña para la cuenta del servidor SMTP.
    - Establecer `WebMail.From` a su propia dirección de correo electrónico. Se trata de la dirección de correo electrónico que se envía el mensaje desde.

    > [!NOTE] 
    > 
    > **Sugerencia** para obtener información adicional acerca de los valores de estas propiedades, vea [configurar opciones de correo electrónico](https://go.microsoft.com/fwlink/?LinkID=202906#configuring_email_settings) en [personalizar el comportamiento de todo el sitio para ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkID=202906).
7. Guarde y cierre  *\_AppStart.cshtml*.
8. Ejecute el *Default.cshtml* página en un explorador.

    ![seguridad-pertenencia-2](16-adding-security-and-membership/_static/image1.png)

    > [!NOTE]
    > Si ve un error que indica que una propiedad debe ser una instancia de `ExtendedMembershipProvider`, el sitio podría no estar configurado para usar el sistema de pertenencia de ASP.NET Web Pages (SimpleMembership). A veces, esto puede ocurrir si el servidor de un proveedor de hospedaje está configurado de manera diferente que el servidor local. Para solucionar este problema, agregue el siguiente elemento en el sitio *Web.config* archivo:
    > 
    > [!code-xml[Main](16-adding-security-and-membership/samples/sample2.xml)]
    > 
    > Agregue este elemento como elemento secundario de la `<configuration>` elemento y como un homólogo de la `<system.web>` elemento.
9. En la esquina superior derecha de la página, haga clic en el **registrar** vínculo. El *Register.cshtml* se muestra la página.
10. Escriba un nombre de usuario y una contraseña y, a continuación, haga clic en **registrar**.

    ![security-membership-3](16-adding-security-and-membership/_static/image2.png)

    Al crear el sitio Web desde el **Starter Site** plantilla, una base de datos denominada *StarterSite.sdf* creada en el sitio *aplicación\_datos* carpeta. Durante el registro, la información de usuario se agrega a la base de datos. Si establece los valores de SMTP, se envía un mensaje a la dirección de correo electrónico que usó para terminar de registrar.

    ![seguridad-pertenencia-4](16-adding-security-and-membership/_static/image3.png)
11. Vaya a su programa de correo electrónico y busque el mensaje, que tendrá el código de confirmación y un hipervínculo al sitio.
12. Haga clic en el hipervínculo para activar su cuenta. El hipervínculo de confirmación abre una página de confirmación de registro.

    ![seguridad-pertenencia-5](16-adding-security-and-membership/_static/image4.png)
- Haga clic en el **inicio de sesión** vincular y, a continuación, inicie sesión con la cuenta que haya registrado.

    Tras iniciar sesión, el **inicio de sesión** y **registrar** vínculos se reemplazan por un **Logout** vínculo. El nombre de inicio de sesión se muestra como un vínculo. (El vínculo le permite ir a una página donde puede cambiar la contraseña.)

    ![6 de pertenencia de seguridad](16-adding-security-and-membership/_static/image5.png)

    > [!NOTE]
    > De forma predeterminada, las páginas web ASP.NET enviar credenciales al servidor en texto no cifrado (como texto legible). Un sitio de producción debe usar HTTP seguro (https://, también conocida como la *capa de sockets seguros* o SSL) para cifrar información confidencial que se intercambia con el servidor. Puede que correo electrónico necesario para enviar mensajes mediante SSL estableciendo `WebMail.EnableSsl=true` como en el ejemplo anterior. Para obtener más información acerca de SSL, consulte [asegurar las comunicaciones Web: certificados SSL y https://](https://go.microsoft.com/fwlink/?LinkId=208660).

## <a name="additional-membership-functionality-in-the-site"></a>Funcionalidad de pertenencia adicionales en el sitio

El sitio contiene otra funcionalidad que permite a los usuarios administrar sus cuentas. Los usuarios pueden hacer lo siguiente:

- Cambiar las contraseñas. Después de iniciar sesión, pueden hacer clic en el nombre de usuario (que es un vínculo). Esto lleva a una página donde puede crear una nueva contraseña (*Account/ChangePassword.cshtml*).
- Recuperar una contraseña olvidada. En la página de inicio de sesión, hay un vínculo (**¿olvidó su contraseña?**) que los usuarios tienen acceso a una página (*Account/ForgotPassword.cshtml*) en el que pueden especificar una dirección de correo electrónico. El sitio envía un mensaje de correo electrónico que tiene un vínculo que puede hacer clic con el fin de establecer una contraseña nueva (*Account/PasswordReset.cshtml*).

También puede permitir que los usuarios también pueden inicien sesión mediante un sitio externo, como se explica más adelante.

<a id="Creating_a_Members-Only_Page"></a>
## <a name="creating-a-members-only-page"></a>Crear una página solo para miembros

Por el momento, cualquier usuario puede examinar cualquier página de su sitio Web. Pero desea tener páginas que están disponibles solo a las personas que han iniciado sesión (es decir, a los miembros). ASP.NET permite crear páginas que son accesibles solo por miembros que ha iniciado la sesión. Por lo general, si los usuarios anónimos intentan tener acceso a una página solo para miembros, se redirigirá a la página de inicio de sesión.

En este procedimiento, creará una carpeta que contendrá las páginas que solo están disponibles para los usuarios conectados.

1. En la raíz del sitio, cree una nueva carpeta. (En la cinta de opciones, haga clic en la flecha situada debajo de **New** y, a continuación, elija **nueva carpeta**.)
2. Nombre de la nueva carpeta *miembros*.
3. Dentro de la *miembros* carpeta, cree una nueva página y lo ha denominado *MembersInformation.cshtml*.
4. Reemplace el contenido existente por el código y el marcado siguiente:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample3.cshtml)]

    Este código comprueba el `IsAuthenticated` propiedad de la `WebSecurity` objeto, que devuelve `true` si el usuario ha iniciado sesión. Si el usuario no ha iniciado sesión, el código llama `Response.Redirect` para enviar al usuario el *Login.cshtml* página en el *cuenta* carpeta.

    La dirección URL de la redirección incluye un `returnUrl` consultar el valor de cadena que utiliza `Request.Url.LocalPath` para establecer la ruta de acceso de la página actual. Si establece la `returnUrl` valor en la cadena de consulta similar al siguiente (y si la dirección URL de retorno es una ruta de acceso local), la página de inicio de sesión devolverá los usuarios a esta página después de que inicie sesión.

    El código también establece  *\_SiteLayout.cshtml* página como su página de diseño. (Para obtener más información acerca de las páginas de diseño, vea [crear un diseño homogéneo en sitios de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202891).)
5. Ejecute el sitio Web. Si ha iniciado sesión todavía, haga clic en el **Logout** situado en la parte superior de la página.
6. En el explorador, solicite la página */miembros/MembersInformation*. Por ejemplo, la dirección URL podría ser similar al siguiente:

    `http://localhost:38366/Members/MembersInformation`

    (El número de puerto (38366) probablemente será diferente en la dirección URL.)

    Se le redirigirá a la *Login.cshtml* página, porque no inició sesión.
- Inicie sesión con la cuenta que creó anteriormente. Se le redirigirá a la *MembersInformation* página. Dado que ha iniciado sesión, este tiempo se mostrará el contenido de página.

Para proteger el acceso a varias páginas, puede hacer esto:

- Agregar la comprobación de seguridad en cada página.
- Crear un  *\_PageStart.cshtml* página en la carpeta donde tenga páginas protegidas y agregar la comprobación de seguridad no existe. El  *\_PageStart.cshtml* página actúa como un tipo de página global para todas las páginas en la carpeta. Esta técnica se explica con más detalle en [personalizar el comportamiento de todo el sitio para ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Using__PageStart.cshtml_to_Restrict_Folder_Access).

## <a name="creating-security-for-groups-of-users-roles"></a>Creación de seguridad para grupos de usuarios (funciones)

Si el sitio tiene una gran cantidad de miembros, no es eficaz para comprobar el permiso para cada usuario individualmente antes de permitirles vea una página. En su lugar, lo que puede hacer es crear grupos, o *roles*, que pertenecen los miembros individuales. A continuación, puede comprobar los permisos basados en rol. En esta sección, creará un &quot;administración&quot; rol y, a continuación, cree una página que sea accesible para los usuarios que están en (que pertenecen a) ese rol.

El sistema de pertenencia ASP.NET está configurado para admitir roles. Sin embargo, a diferencia de registro de pertenencia y el inicio de sesión, el **sitio de inicio** la plantilla no contiene páginas que le ayudarán a administración roles. (Administración de roles es una tarea administrativa, en lugar de una tarea de usuario). Sin embargo, puede agregar grupos directamente en la base de datos de pertenencia en WebMatrix.

1. En WebMatrix, haga clic en el **bases de datos** selector de área de trabajo.
2. En el panel izquierdo, abra el *StarterSite.sdf* nodo, abra el **tablas** nodo y, a continuación, haga doble clic en el *páginas Web\_Roles* tabla.

    ![7 de pertenencia de seguridad](16-adding-security-and-membership/_static/image6.png)
3. Agregar un rol denominado &quot;administración&quot;. El *RoleId* campo se rellena automáticamente. (Es la clave principal y se ha establecido en ser un campo de identificación, como se explica en [Introducción a trabajar con una base de datos en sitios de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202893).)
4. Tome nota de qué es el valor para el *RoleId* campo. (Si se trata del primer rol que se está definiendo, será 1.)

    ![seguridad-pertenencia-8](16-adding-security-and-membership/_static/image7.png)
5. Cerrar la *páginas Web\_Roles* tabla.
6. Abra la *UserProfile* tabla.
7. Tome nota de la *UserId* valor de uno o varios de los usuarios en la tabla y, a continuación, cierre la tabla.
8. Abra la *páginas Web\_UserInRoles* de tabla y escriba un *UserID* y un *RoleID* valor en la tabla. Por ejemplo, para colocar el usuario 2 en la &quot;administración&quot; rol, escribiría estos valores:

    ![9 de pertenencia de seguridad](16-adding-security-and-membership/_static/image8.png)
9. Cerrar la *páginas Web\_UsersInRoles* tabla.

    Ahora que tiene roles definidos, puede configurar una página que sea accesible a los usuarios que están en ese rol.
10. En la carpeta raíz del sitio Web, cree una nueva página denominada *AdminError.cshtml* y reemplace el contenido existente por el código siguiente. Se trata de la página que se redirigen a los usuarios si no se permiten el acceso a una página.

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample4.cshtml)]
11. En la carpeta raíz del sitio Web, cree una nueva página denominada *AdminOnly.cshtml* y reemplace el código existente por el código siguiente:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample5.cshtml)]

    El `Roles.IsUserInRole` método `true` si el usuario actual es un miembro del rol especificado (en este caso, la función "admin").
12. Ejecutar *Default.cshtml* en un explorador, pero no iniciar sesión. (Si ya ha iniciado sesión, cierre sesión.)
13. En la barra de direcciones del explorador, agregue *AdminOnly* en la dirección URL. (En otras palabras, solicitar la *AdminOnly.cshtml* archivo.) Se le redirigirá a la *AdminError.cshtml* página, porque actualmente no inició sesión como un usuario en el &quot;administración&quot; rol.
14. Vuelva a *Default.cshtml* e inicie sesión como el usuario que haya agregado a la &quot;administración&quot; rol.
15. Vaya a *AdminOnly.cshtml* página. Esta vez verá la página.

## <a name="preventing-automated-programs-from-joining-your-website"></a>Impide que los programas automatizados unirse a su sitio Web

La página de inicio de sesión no detendrá programas automatizados (a veces se denomina *web robots* o *bots*) desde el registro con su sitio Web. Este procedimiento describe cómo habilitar una prueba de ReCaptcha para la página de registro.

![/media/38777/ch16securitymembership-18.jpg](16-adding-security-and-membership/_static/image1.jpg)

1. Registrar su sitio Web en ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Cuando haya completado el registro, obtendrá una clave pública y una clave privada.
2. Agregar ASP.NET Web Helpers Library a su sitio Web, como se describe en [aplicaciones auxiliares de instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.
3. En el *cuenta* carpeta, abra el archivo denominado *Register.cshtml*.
4. En el código en la parte superior de la página, busque las siguientes líneas y quite la marca de comentario ellos mediante la eliminación de la `//` caracteres de comentario:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample6.cs)]
5. Reemplace `PRIVATE_KEY` con su propia clave privada de ReCaptcha.
6. En el marcado de la página, quite el `@*` y `*@` caracteres de comentarios de alrededor de las líneas siguientes en el marcado de la página:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample7.cshtml)]
7. Reemplace `PUBLIC_KEY` con su clave.
8. Si aún no lo ha quitado ya, quitar el `<div>` elemento que contiene el texto que empieza por "Para habilitar la comprobación CAPTCHA...". (Elimina toda la `<div>` elemento y su contenido.)

1. Ejecutar *Default.cshtml* en un explorador. Si ha iniciado sesión en el sitio, haga clic en el **Logout** vínculo.
2. Haga clic en el **registrar** vincular y probar el registro mediante la prueba CAPTCHA.

    ![security-membership-10](16-adding-security-and-membership/_static/image9.png)

Para obtener más información sobre la `ReCaptcha` auxiliar, vea [mediante un CATPCHA para evitar que los programas automatizada (Bots) de uso de su sitio Web de ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251967).

<a id="Additional_Resources"></a>
## <a name="letting-users-log-in-using-an-external-site"></a>Permite que los usuarios iniciar sesión con un sitio externo

El **Starter Site** plantilla incluye código y marcado que permite a los usuarios iniciar sesión con Facebook, Windows Live, Twitter, Google o Yahoo. De forma predeterminada, esta funcionalidad no está habilitada. El procedimiento general para el uso de permitir que los usuarios inicien sesión mediante estos proveedores externos es:

- Decidir cuál de los sitios externos que desea admitir.
- Si es necesario, vaya a ese sitio y configurar una aplicación de inicio de sesión. (Por ejemplo, se debe hacer esto para permitir los inicios de sesión de Facebook).
- En el sitio, configure el proveedor. En la mayoría de los casos, sólo tiene que quite la parte del código en el  *\_AppStart.cshtml* archivo.
- Agregar marcas a la página de registro que permite que personas vínculo al sitio externo de inicio de sesión. Normalmente, puede copiar el marcado que necesita y cambia el texto ligeramente.

Encontrará instrucciones paso a paso en el tema [habilitar inicio de sesión de sitios externos en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251969).

Después de que un usuario inicia sesión desde otro sitio, el usuario vuelve a su sitio y *asocia* ese inicio de sesión con su sitio. En efecto, esto crea una entrada de pertenencia en su sitio para el inicio de sesión del usuario externo. Esto le permite utilizar las funciones normales de pertenencia (por ejemplo, los roles) con el inicio de sesión externo.

## <a name="adding-security-to-an-existing-website"></a>Adición de seguridad a un sitio Web existente

El procedimiento descrito anteriormente en este artículo se basa en el uso del **Starter Site** plantilla como base para la seguridad del sitio Web. Si no resulta muy práctico para que pueda iniciar desde el **Starter Site** plantilla o para copiar las páginas relevantes de un sitio basado en esa plantilla, puede implementar el mismo tipo de seguridad en su propio sitio codificando usted mismo. Crear los mismos tipos de páginas, registro, inicio de sesión y así sucesivamente y, a continuación, usar aplicaciones auxiliares y las clases para configurar la pertenencia.

El proceso básico que se describe en la entrada de blog [la manera más sencilla de implementar la seguridad de ASP.NET Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240). Realiza la mayor parte del trabajo mediante los siguientes métodos y propiedades de la `WebSecurity` auxiliar:

- [WebSecurty.UserExists](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.userexists(v=vs.99).aspx), [WebSecurity.CreateUserAndAccount](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.createuserandaccount(v=vs.99).aspx). Estos métodos le permiten determinar si alguien ya está registrado y registrarlos.
- [WebSecurty.IsAuthenticated](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.isauthenticated(v=vs.99).aspx). Esta propiedad le permite determinar si el usuario actual ha iniciado sesión. Esto es útil para redirigir a los usuarios a una página de inicio de sesión si aún no han iniciado sesión.
- [WebSecurity.Login](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.login(v=vs.99).aspx), [WebSecurity.Logout](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.logout(v=vs.99).aspx). Estos métodos, inicie sesión un usuario o alejar.
- [WebSecurity.CurrentUserName](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.currentusername(v=vs.99).aspx). Esta propiedad es útil para mostrar ha iniciado la sesión nombre del usuario actual (si el usuario ha iniciado sesión).
- [WebSecurity.ConfirmAccount](https://msdn.microsoft.com/library/gg569286(v=vs.99).aspx). Este método es útil si configuras confirmación por correo electrónico para el registro. (En la entrada de blog se proporcionan detalles [mediante la característica de confirmación para la seguridad de ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267).)

Para administrar roles, puede usar el [Roles](https://msdn.microsoft.com/library/gg538398(v=vs.99).aspx) y [pertenencia](https://msdn.microsoft.com/library/gg569035(v=vs.99).aspx) clases, como se describe en la entrada de blog.

## <a name="additional-resources"></a>Recursos adicionales

- [Personalizar el comportamiento de todo el sitio](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Proteger las comunicaciones Web: Https://, SSL y certificados](https://go.microsoft.com/fwlink/?LinkId=208660)
- [LA manera más sencilla de implementar la seguridad de ASP.NET Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240) y [mediante la característica de confirmación para la seguridad de ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267). Se trata de entradas de blog que describen cómo implementar las características de pertenencia ASP.NET sin usar la **Starter Site** plantilla.
- [Habilitar el inicio de sesión desde sitios externos en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251969)
- [Referencia de la API de clase WebSecurity](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity(v=vs.99)) (MSDN)
- [Referencia de la API de la clase de SimpleRoleProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simpleroleprovider(v=vs.99)) (MSDN)
- [Referencia de la API de la clase de SimpleMembershipProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simplemembershipprovider(v=vs.99)) (MSDN)
