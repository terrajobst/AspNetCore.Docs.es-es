---
uid: web-pages/overview/security/16-adding-security-and-membership
title: Agregar seguridad y pertenencia a ASP.NET Web Pages (Razor) sitio | Microsoft Docs
author: tfitzmac
description: Este capítulo muestra cómo proteger su sitio Web para que algunas de las páginas solo están disponibles para las personas que inicie sesión. (También verá cómo crear páginas tha...
ms.author: riande
ms.date: 02/24/2014
ms.assetid: 7a77c2c0-deea-4290-a9c3-97958891758e
msc.legacyurl: /web-pages/overview/security/16-adding-security-and-membership
msc.type: authoredcontent
ms.openlocfilehash: ae574706ecd14f1cafdb2d8b6340477e50246a32
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835553"
---
<a name="adding-security-and-membership-to-an-aspnet-web-pages-razor-site"></a>Agregar seguridad y pertenencia a un sitio Web de ASP.NET Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo proteger un sitio Web de ASP.NET Web Pages (Razor) para que algunas de las páginas solo están disponibles para las personas que inicie sesión. (También verá cómo crear las páginas que cualquiera puede tener acceso.)
> 
> **Lo que aprenderá:** 
> 
> - Cómo crear un sitio Web que tiene una página de registro y una página de inicio de sesión de manera que para algunas páginas puede limitar el acceso a solo los miembros.
> - Cómo crear páginas públicas y miembro.
> - Cómo definir los roles, que son grupos que tienen permisos de seguridad diferente en su sitio, y cómo asignar usuarios a un rol.
> - Cómo usar CAPTCHA para evitar que programas automatizados (bots) de creación de cuentas de miembro.
>   
> 
> Estas son las características ASP.NET incorporadas en el artículo:
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
> - WebMatrix 3
> - ASP.NET Web Helpers Library


Puede configurar su sitio Web para que los usuarios pueden iniciar sesión en él &#8212; es decir, para que el sitio admite *pertenencia*. Esto puede ser útil por diversos motivos. Por ejemplo, el sitio puede tener páginas que deben estar disponibles solo a los miembros. En algunos casos, es posible que necesite los usuarios inicien sesión con el fin de enviar comentarios o deje un comentario.

Incluso si su sitio Web admite la pertenencia, los usuarios no son necesariamente necesarios para iniciar sesión antes de utilizar algunas de las páginas en el sitio. Los usuarios que no ha iniciado sesión en el que se conocen como *a los usuarios anónimos*.

Un usuario puede registrar en su sitio Web y, a continuación, puede iniciar sesión en el sitio. El sitio Web requiere un nombre de usuario (dirección de correo electrónico) y una contraseña para confirmar que los usuarios son quienes dicen ser. Este proceso de inicio de sesión y confirmar la identidad de un usuario se conoce como *autenticación*.

Puede establecer la seguridad y pertenencia de maneras diferentes:

- Si está usando WebMatrix, una forma sencilla consiste en crear como nuevo sitio basado en la **Starter Site** plantilla. Esta plantilla ya está configurada para la seguridad y pertenencia y ya tiene una página de registro, una página de inicio de sesión y así sucesivamente.

    El sitio creado mediante la plantilla también tiene una opción para permitir que los usuarios iniciar sesión con un sitio externo, como Facebook, Google o Twitter.
- Si desea agregar seguridad a un sitio existente, o si no desea utilizar el **Starter Site** plantilla, puede crear su propia página de registro, página de inicio de sesión y así sucesivamente.

En este artículo se centra en la primera opción &mdash; cómo agregar seguridad mediante la **Starter Site** plantilla. También proporciona información básica sobre cómo implementar su propia seguridad y, a continuación, se proporcionan vínculos para obtener más información sobre cómo hacerlo. También hay información sobre cómo habilitar los inicios de sesión externos, como se describe con más detalle en otro artículo.

## <a name="creating-website-security-using-the-starter-site-template"></a>Creación de seguridad de sitio Web mediante la plantilla de sitio de inicio

En WebMatrix, puede usar el **Starter Site** plantilla para crear un sitio Web que contiene lo siguiente:

- Una base de datos que se usa para almacenar los nombres de usuario y contraseñas para los miembros.
- Una página de registro donde pueden registrar los usuarios anónimos de (nuevos).
- Una página de inicio de sesión y cierre de sesión.
- Una página de restablecimiento y recuperación de contraseña.

El siguiente procedimiento describe cómo crear el sitio y configurarlo.

1. Inicie WebMatrix y en el **inicio rápido** página, seleccione **sitio desde plantilla**.
2. Seleccione el **Starter Site** plantilla y, a continuación, haga clic en **Aceptar**. WebMatrix crea un nuevo sitio.
3. En el panel izquierdo, haga clic en el **archivos** selector de área de trabajo.
4. En la carpeta raíz de su sitio Web, abra el  *\_AppStart.cshtml* archivo, que es un archivo especial que se usa para contener la configuración global. Contiene algunas instrucciones que están comentadas utilizando el `//` caracteres:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample1.cs)]

    Estas instrucciones se configuración el `WebMail` auxiliar, que se puede usar para enviar correo electrónico. El sistema de pertenencia puede utilizar el correo electrónico para enviar mensajes de confirmación cuando los usuarios registrar o cuando desee cambiar sus contraseñas. (Por ejemplo, una vez que los usuarios registrar, reciben un correo electrónico que incluye un vínculo que puede hacer clic con el fin de finalizar el proceso de registro.)

    Envío de correo electrónico requiere acceso a un servidor SMTP, como se describe en [agregando un correo electrónico a un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202899). Va a almacenar la configuración de correo electrónico en este central  *\_AppStart.cshtml* archivo para que no tiene que escriba el código varias veces en cada página que puede enviar correo electrónico. (No es necesario configurar SMTP para configurar una base de datos de registro; sólo necesita una configuración de SMTP si desea validar a los usuarios de sus alias de correo electrónico y permitir que los usuarios restablecer una contraseña olvidada).
5. Quite las instrucciones quitando `//` delante de cada uno de ellos.

    Si no desea configurar correo electrónico de confirmación, puede omitir este paso y el paso siguiente. Si no se establecen los valores de SMTP, la nueva cuenta está inmediatamente disponible sin un correo electrónico de confirmación.
6. Modifique la siguiente configuración de correo electrónico en el código:

   - Establecer `WebMail.SmtpServer` en el nombre del servidor SMTP que tienen acceso a.
   - Deje `WebMail.EnableSsl` establecido en `true`. Esta configuración protege las credenciales que se envían al servidor SMTP mediante su cifrado.
   - Establecer `WebMail.UserName` al nombre de usuario de la cuenta del servidor SMTP.
   - Establecer `WebMail.Password` a la contraseña de la cuenta del servidor SMTP.
   - Establecer `WebMail.From` a su propia dirección de correo electrónico. Se trata de la dirección de correo electrónico que se envía el mensaje desde.

     > [!NOTE] 
     > 
     > **Sugerencia** para obtener más información sobre los valores de estas propiedades, vea [configuración de correo electrónico](https://go.microsoft.com/fwlink/?LinkID=202906#configuring_email_settings) en [personalizar el comportamiento de todo el sitio para ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkID=202906).
7. Guarde y cierre  *\_AppStart.cshtml*.
8. Ejecute el *Default.cshtml* página en un explorador.

    ![Security-suscripción-2](16-adding-security-and-membership/_static/image1.png)

   > [!NOTE]
   > Si ve un error que indica que una propiedad debe ser una instancia de `ExtendedMembershipProvider`, el sitio podría no estar configurado para usar el sistema de pertenencia de ASP.NET Web Pages (SimpleMembership). A veces esto puede ocurrir si el servidor de un proveedor de hospedaje está configurado de forma diferente a su servidor local. Para solucionar este problema, agregue el siguiente elemento a la carpeta del sitio *Web.config* archivo:
   > 
   > [!code-xml[Main](16-adding-security-and-membership/samples/sample2.xml)]
   > 
   > Agregue este elemento como elemento secundario de la `<configuration>` elemento y del mismo nivel que el `<system.web>` elemento.
9. En la esquina superior derecha de la página, haga clic en el **registrar** vínculo. El *Register.cshtml* se muestra la página.
10. Escriba un nombre de usuario y contraseña y, a continuación, haga clic en **registrar**.

    ![Security-pertenencia-3](16-adding-security-and-membership/_static/image2.png)

    Al crear el sitio Web desde el **Starter Site** plantilla, una base de datos denominada *StarterSite.sdf* se creó en el sitio *aplicación\_datos* carpeta. Durante el registro, la información de usuario se agrega a la base de datos. Si establece los valores de SMTP, se envía un mensaje a la dirección de correo electrónico que usó para finalizar el registro.

    ![Security-pertenencia-4](16-adding-security-and-membership/_static/image3.png)
11. Vaya a su programa de correo electrónico y busque el mensaje, que tendrá el código de confirmación y un hipervínculo al sitio.
12. Haga clic en el hipervínculo para activar su cuenta. El hipervínculo de confirmación, abre una página de confirmación de registro.

    ![Security-pertenencia-5](16-adding-security-and-membership/_static/image4.png)
13. Haga clic en el **inicio de sesión** vincular y, a continuación, inicie sesión con la cuenta que se ha registrado.

      Después de iniciar sesión, el **inicio de sesión** y **registrar** vínculos se reemplazan por un **Logout** vínculo. El nombre de inicio de sesión se muestra como un vínculo. (El vínculo le permite ir a una página donde puede cambiar la contraseña.)

      ![Security-pertenencia-6](16-adding-security-and-membership/_static/image5.png)

      > [!NOTE]
      > De forma predeterminada, las páginas web ASP.NET enviar credenciales al servidor en texto no cifrado (como texto legible). Un sitio de producción debe utilizar HTTP seguro (https://, también se conoce como el *capa de sockets seguros* o SSL) para cifrar información confidencial que se intercambia con el servidor. Puede que correo electrónico requiere los mensajes se envíen mediante SSL estableciendo `WebMail.EnableSsl=true` del ejemplo anterior. Para obtener más información acerca de SSL, consulte [asegurar las comunicaciones Web: certificados SSL y https://](https://go.microsoft.com/fwlink/?LinkId=208660).

## <a name="additional-membership-functionality-in-the-site"></a>Funcionalidad de pertenencia adicionales en el sitio

El sitio contiene otra funcionalidad que permite a los usuarios administrar sus cuentas. Los usuarios pueden hacer lo siguiente:

- Cambiar sus contraseñas. Después de iniciar sesión, pueden hacer clic en el nombre de usuario (que es un vínculo). Esto lleva a una página donde puede crear una nueva contraseña (*Account/ChangePassword.cshtml*).
- Recuperar una contraseña olvidada. En la página de inicio de sesión, hay un vínculo (**¿olvidó su contraseña?**) que lleva a los usuarios a una página (*Account/ForgotPassword.cshtml*) donde puede escribir una dirección de correo electrónico. El sitio envía un mensaje de correo electrónico que tiene un vínculo que puede hacer clic con el fin de establecer una contraseña nueva (*Account/PasswordReset.cshtml*).

También puede permitir que los usuarios también pueden inicien sesión mediante un sitio externo, como se explica más adelante.

<a id="Creating_a_Members-Only_Page"></a>
## <a name="creating-a-members-only-page"></a>Creación de una página solo para miembros

Por el momento, cualquier persona puede ir a cualquier página en su sitio Web. Pero puede ser conveniente tener páginas que están disponibles solo para las personas que han iniciado sesión (es decir, a los miembros). ASP.NET permite crear páginas que se pueden acceder únicamente por miembros que ha iniciado sesión. Normalmente, si los usuarios anónimos intentan tener acceso a una página solo para miembros, se redirige a la página de inicio de sesión.

En este procedimiento, creará una carpeta que contendrá las páginas que solo están disponibles para los usuarios conectados.

1. En la raíz del sitio, cree una nueva carpeta. (En la cinta de opciones, haga clic en la flecha situada debajo de **New** y, a continuación, elija **nueva carpeta**.)
2. Nombre de la nueva carpeta *miembros*.
3. Dentro de la *miembros* carpeta, cree una nueva página y lo ha denominado *MembersInformation.cshtml*.
4. Reemplace el contenido existente por el código y el marcado siguiente:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample3.cshtml)]

    Este código comprueba la `IsAuthenticated` propiedad de la `WebSecurity` object, que devuelve `true` si el usuario ha iniciado sesión. Si el usuario no ha iniciado sesión, el código llama a `Response.Redirect` para enviar al usuario el *Login.cshtml* página en el *cuenta* carpeta.

    La dirección URL de la redirección incluye un `returnUrl` consultar el valor de cadena que utiliza `Request.Url.LocalPath` para establecer la ruta de acceso de la página actual. Si establece la `returnUrl` valor en la cadena de consulta similar al siguiente (y si la dirección URL de retorno es una ruta de acceso local), la página de inicio de sesión devolverá los usuarios a esta página después de iniciar sesión.

    El código también establece  *\_SiteLayout.cshtml* página como su página de diseño. (Para obtener más información acerca de las páginas de diseño, vea [crear un diseño coherente en sitios de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202891).)
5. Ejecute el sitio Web. Si ha iniciado todavía, haga clic en el **Logout** situado en la parte superior de la página.
6. En el explorador, solicite la página */miembros/MembersInformation*. Por ejemplo, la dirección URL podría ser similar al siguiente:

    `http://localhost:38366/Members/MembersInformation`

    (El número de puerto (38366) probablemente será diferente en la dirección URL.)

    Se le redirigirá a la *Login.cshtml* página, porque no se inició sesión.
7. Inicie sesión con la cuenta que creó anteriormente. Se le redirigirá a la *MembersInformation* página. Dado que ha iniciado sesión, este tiempo verá el contenido de página.

Para proteger el acceso a varias páginas, puede hacer esto:

- Agregar la comprobación de seguridad en cada página.
- Crear un  *\_PageStart.cshtml* página en la carpeta donde tenga las páginas protegidas y agregar la comprobación de seguridad no existe. El  *\_PageStart.cshtml* página actúa como un tipo de página global para todas las páginas en la carpeta. Esta técnica se explica con más detalle en [personalizar el comportamiento de todo el sitio para ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Using__PageStart.cshtml_to_Restrict_Folder_Access).

## <a name="creating-security-for-groups-of-users-roles"></a>Creación de seguridad para grupos de usuarios (funciones)

Si el sitio tiene muchos miembros, no es eficaz para comprobar los permisos para cada usuario individualmente antes de permitirles ver una página. En su lugar, lo que puede hacer es crear grupos, o *roles*, que pertenecen los miembros individuales. A continuación, puede comprobar los permisos basados en rol. En esta sección, creará un &quot;admin&quot; rol y, a continuación, cree una página que se puede acceder a los usuarios que están en (que pertenecen a) ese rol.

El sistema de pertenencia ASP.NET está configurado para admitir roles. Sin embargo, a diferencia de registro de pertenencia y el inicio de sesión, el **Starter Site** plantilla no contiene páginas que le ayudarán a administración roles. (Administración de roles es una tarea administrativa, en lugar de una tarea de usuario). Sin embargo, puede agregar grupos directamente en la base de datos de pertenencia en WebMatrix.

1. En WebMatrix, haga clic en el **bases de datos** selector de área de trabajo.
2. En el panel izquierdo, abra el *StarterSite.sdf* nodo, abra el **tablas** nodo y, a continuación, haga doble clic en el *páginas Web\_Roles* tabla.

    ![Security-pertenencia-7](16-adding-security-and-membership/_static/image6.png)
3. Agregue un rol denominado &quot;admin&quot;. El *RoleId* campo se rellena automáticamente. (Es la clave principal y se estableció para ser un campo de identificación, como se explica en [Introducción al trabajo con una base de datos en sitios de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202893).)
4. Tome nota de lo que es el valor para el *RoleId* campo. (Si se trata de la primera función que se va a definir, será 1.)

    ![Security-pertenencia-8](16-adding-security-and-membership/_static/image7.png)
5. Cerrar la *páginas Web\_Roles* tabla.
6. Abra el *UserProfile* tabla.
7. Tome nota de la *UserId* valor de uno o varios de los usuarios en la tabla y, luego, cierre la tabla.
8. Abra el *páginas Web\_UserInRoles* de tabla y escriba un *UserID* y un *RoleID* valor en la tabla. Por ejemplo, para colocar el usuario 2 en la &quot;admin&quot; rol, debe especificar estos valores:

    ![9 de pertenencia de seguridad](16-adding-security-and-membership/_static/image8.png)
9. Cerrar la *páginas Web\_UsersInRoles* tabla.

    Ahora que tiene roles definidos, puede configurar una página que se puede acceder a los usuarios que están en ese rol.
10. En la carpeta raíz del sitio Web, cree una nueva página denominada *AdminError.cshtml* y reemplace el contenido existente por el código siguiente. Se trata de la página que se redirigen a usuarios si no se permiten el acceso a una página.

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample4.cshtml)]
11. En la carpeta raíz del sitio Web, cree una nueva página denominada *AdminOnly.cshtml* y reemplace el código existente por el código siguiente:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample5.cshtml)]

    El `Roles.IsUserInRole` devuelve del método `true` si el usuario actual es un miembro del rol especificado (en este caso, la función "admin").
12. Ejecute *Default.cshtml* en un explorador, pero no iniciar sesión. (Si ya ha iniciado sesión, cerrar sesión.)
13. En la barra de direcciones del explorador, agregue *AdminOnly* en la dirección URL. (En otras palabras, solicitar el *AdminOnly.cshtml* archivo.) Se le redirigirá a la *AdminError.cshtml* página, porque actualmente no se inició sesión como un usuario en el &quot;admin&quot; rol.
14. Vuelva a *Default.cshtml* e inicie sesión como el usuario ha agregado a la &quot;admin&quot; rol.
15. Vaya a *AdminOnly.cshtml* página. Esta vez verá la página.

## <a name="preventing-automated-programs-from-joining-your-website"></a>Impide que los programas automatizados unirse a su sitio Web

La página de inicio de sesión no detendrá programas automatizados (a veces se denomina *web robots* o *bots*) desde el registro con su sitio Web. Este procedimiento describe cómo habilitar una prueba de ReCaptcha para la página de registro.

![/media/38777/ch16securitymembership-18.jpg](16-adding-security-and-membership/_static/image1.jpg)

1. Registrar su sitio Web en ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Cuando haya completado el registro, obtendrá una clave pública y una clave privada.
2. Agregue la ASP.NET Web Helpers Library a su sitio Web, como se describe en [las aplicaciones auxiliares de la instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.
3. En el *cuenta* carpeta, abra el archivo denominado *Register.cshtml*.
4. En el código en la parte superior de la página, busque las siguientes líneas y quite la marca de comentario ellos mediante la eliminación de la `//` caracteres de comentario:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample6.cs)]
5. Reemplace `PRIVATE_KEY` con su propia clave privada de ReCaptcha.
6. En el marcado de la página, quite el `@*` y `*@` caracteres de comentarios de alrededor de las líneas siguientes en el marcado de página:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample7.cshtml)]
7. Reemplace `PUBLIC_KEY` con su clave.
8. Si aún no ha quitado ya, quite el `<div>` elemento que contiene el texto que comienza con "Para habilitar la comprobación CAPTCHA...". (Quitar toda la `<div>` elemento y su contenido.)

9. Ejecute *Default.cshtml* en un explorador. Si ha iniciado en el sitio, haga clic en el **Logout** vínculo.
10. Haga clic en el **registrar** vincular y probar el registro mediante la prueba CAPTCHA.

     ![seguridad y pertenencia-10](16-adding-security-and-membership/_static/image9.png)

Para obtener más información sobre la `ReCaptcha` auxiliares, consulte [utilizando un CATPCHA para evitar que programas automatizada (Bots) de uso de su sitio Web de ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251967).

<a id="Additional_Resources"></a>
## <a name="letting-users-log-in-using-an-external-site"></a>Permitir a los usuarios iniciar sesión con un sitio externo

El **Starter Site** plantilla incluye código y marcado que permite a los usuarios iniciar sesión con Facebook, Windows Live, Twitter, Google o Yahoo. De forma predeterminada, esta funcionalidad no está habilitada. El procedimiento general para el uso de permitir que los usuarios inicien sesión mediante estos proveedores externos es esto:

- Decida cuál de los sitios externos que desea admitir.
- Si es necesario, vaya a ese sitio y configurar una aplicación de inicio de sesión. (Por ejemplo, que debe hacer esto con el fin de permitir inicios de sesión de Facebook).
- En su sitio, configure el proveedor. En la mayoría de los casos, solo tiene comentarios de parte del código en el  *\_AppStart.cshtml* archivo.
- Agregue el marcado para la página de registro que permite a los usuarios vínculo al sitio externo para el registro. Normalmente, puede copiar el marcado que necesita y cambia ligeramente el texto.

Puede encontrar instrucciones paso a paso en el tema [habilitar inicio de sesión desde sitios externos en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251969).

Después de que un usuario inicia sesión desde otro sitio, el usuario vuelve a su sitio y *asocia* ese inicio de sesión con su sitio. De hecho, esto crea una entrada de pertenencia en el sitio para el inicio de sesión del usuario externo. Esto le permite usar las funciones normales de pertenencia (por ejemplo, los roles) con el inicio de sesión externo.

## <a name="adding-security-to-an-existing-website"></a>Adición de seguridad a un sitio Web existente

El procedimiento descrito anteriormente en este artículo se basa en el uso de la **Starter Site** plantilla como base para la seguridad del sitio Web. Si no resulta práctico para iniciar desde el **Starter Site** plantilla o para copiar las páginas relevantes de un sitio basado en esa plantilla, puede implementar el mismo tipo de seguridad en su propio sitio mediante la codificación por sí mismo. Crear los mismos tipos de páginas, registro, inicio de sesión etc. — y, a continuación, usar clases y las aplicaciones auxiliares para configurar la pertenencia.

El proceso básico que se describe en la entrada de blog [la manera más sencilla de implementar la seguridad de ASP.NET Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240). La mayoría del trabajo se realiza mediante los siguientes métodos y propiedades de la `WebSecurity` auxiliar:

- [WebSecurty.UserExists](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.userexists(v=vs.99).aspx), [WebSecurity.CreateUserAndAccount](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.createuserandaccount(v=vs.99).aspx). Estos métodos le permiten determinar si alguien ya está registrado y registrarlos.
- [WebSecurty.IsAuthenticated](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.isauthenticated(v=vs.99).aspx). Esta propiedad le permite determinar si el usuario actual ha iniciado sesión. Esto es útil para redirigir a los usuarios a una página de inicio de sesión si aún no han iniciado sesión.
- [WebSecurity.Login](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.login(v=vs.99).aspx), [WebSecurity.Logout](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.logout(v=vs.99).aspx). Estos métodos, inicie sesión un usuario o la reducción horizontal.
- [WebSecurity.CurrentUserName](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.currentusername(v=vs.99).aspx). Esta propiedad es útil para mostrar ha iniciado sesión nombre del usuario actual (si el usuario ha iniciado sesión).
- [WebSecurity.ConfirmAccount](https://msdn.microsoft.com/library/gg569286(v=vs.99).aspx). Este método es útil si configuró la confirmación por correo electrónico para el registro. (Los detalles se describen en la entrada de blog [usando la característica de confirmación para la seguridad de ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267).)

Para administrar roles, puede usar el [Roles](https://msdn.microsoft.com/library/gg538398(v=vs.99).aspx) y [pertenencia](https://msdn.microsoft.com/library/gg569035(v=vs.99).aspx) clases, como se describe en la entrada de blog.

## <a name="additional-resources"></a>Recursos adicionales

- [Personalizar el comportamiento de todo el sitio](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Protección de las comunicaciones Web: Certificados SSL y https://](https://go.microsoft.com/fwlink/?LinkId=208660)
- [LA manera más sencilla de implementar la seguridad de ASP.NET Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240) y [usando la característica de confirmación para la seguridad de ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267). Estos son entradas de blog que describen cómo implementar las características de pertenencia ASP.NET sin usar el **Starter Site** plantilla.
- [Habilitar el inicio de sesión desde sitios externos en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251969)
- [Referencia de API de clase WebSecurity](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity(v=vs.99)) (MSDN)
- [Referencia de API de clase SimpleRoleProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simpleroleprovider(v=vs.99)) (MSDN)
- [Referencia de API de clase SimpleMembershipProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simplemembershipprovider(v=vs.99)) (MSDN)
