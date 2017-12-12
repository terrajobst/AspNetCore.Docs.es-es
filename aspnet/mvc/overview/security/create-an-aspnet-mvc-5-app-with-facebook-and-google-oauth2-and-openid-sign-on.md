---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: "Crear MVC 5 aplicación con Facebook, Twitter, LinkedIn y Google OAuth2 Sign-on (C#) | Documentos de Microsoft"
author: Rick-Anderson
description: "Este tutorial muestra cómo crear una aplicación web de ASP.NET MVC 5 que permite a los usuarios inicien sesión con OAuth 2.0 con las credenciales de un authenti externo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2015
ms.topic: article
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: aaa061e61b9bab5b33083851624f0487b2cf6473
ms.sourcegitcommit: ccf08615ad59bc6f654560de33b93396113a2eb0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2017
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Crear una aplicación de ASP.NET MVC 5 con Facebook, Twitter, LinkedIn y Google OAuth2 Sign-on (C#)
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial muestra cómo crear una aplicación web de ASP.NET MVC 5 que permite a los usuarios iniciar sesión mediante [OAuth 2.0](http://oauth.net/2/) con credenciales de un proveedor de autenticación externo, como Facebook, Twitter, LinkedIn, Microsoft o Google. Por simplicidad, este tutorial se centra en trabajar con las credenciales de Google y Facebook.
> 
> Habilitar estas credenciales en los sitios web proporciona una ventaja importante porque millones de usuarios ya tienen cuentas con estos proveedores externos. Estos usuarios pueden ser más dispuestos a registrarse para el sitio si no tienen que crear y recordar a un nuevo conjunto de credenciales.
> 
> Vea también [aplicación de ASP.NET MVC 5 con SMS y correo electrónico de autenticación en dos fases](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).
> 
> El tutorial también muestra cómo agregar datos de perfil para el usuario y cómo usar la API de pertenencia para agregar roles. Este tutorial se escribió por [Rick Anderson](https://blogs.msdn.com/rickAndy) (me siga en Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="start"></a>
## <a name="getting-started"></a>Introducción

Empiece por instalar y ejecutar [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instalar Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o superior. Para obtener ayuda con Dropbox, GitHub, Linkedin, Instagram, búfer, salesforce, secuencia, pila de Exchange, Tripit, twitch, Twitter, Yahoo y mucho más, consulte este [integral guía](http://www.oauthforaspnet.com/).

> [!NOTE]
> Debe instalar Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o superior que se va a usar Google OAuth 2 como depurar localmente sin advertencias de SSL.


Haga clic en **nuevo proyecto** desde el **iniciar** página, o bien puede usar el menú y seleccione **archivo**y, a continuación, **nuevo proyecto**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a>Crear su primera aplicación

Haga clic en **nuevo proyecto**, a continuación, seleccione **Visual C#** a la izquierda, a continuación, **Web** y, a continuación, seleccione **aplicación Web ASP.NET**. Denomine el proyecto "MvcAuth" y, a continuación, haga clic en **Aceptar**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

En el **nuevo proyecto ASP.NET** cuadro de diálogo, haga clic en **MVC**. Si la autenticación no es **cuentas de usuario individuales**, haga clic en el **Cambiar autenticación** botón y seleccione **cuentas de usuario individuales**. Comprobando **Host en la nube**, la aplicación le resultará muy fácil de hospedar en Azure.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

Si seleccionó **Host en la nube**, complete el cuadro de diálogo Configurar.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>Use NuGet para actualizar a la último middleware OWIN

Usar el Administrador de paquetes de NuGet para actualizar la [middleware de OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md). Seleccione **actualizaciones** en el menú izquierdo. Puede hacer clic en el **actualizar todo** botón o se puede buscar solo los paquetes OWIN (que se muestra en la imagen siguiente):

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

En la imagen siguiente, se muestran solo los paquetes OWIN:

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

Desde la consola de Manager de paquete (PMC), puede escribir el `Update-Package` comando, que se actualizará todos los paquetes.

Presione **F5** o **CTRL+F5** para ejecutar la aplicación. En la imagen siguiente, el número de puerto es 1234. Al ejecutar la aplicación, verá un número de puerto diferente.

Según el tamaño de la ventana del explorador, tendrá que hacer clic en el icono de navegación para ver la **inicio**, **sobre**, **póngase en contacto con**, **registrar**y **sesión** vínculos.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>Cómo configurar SSL en el proyecto

Para conectarse a proveedores de autenticación como Google y Facebook, debe configurar IIS Express para usar SSL. Es importante seguir usando SSL después de iniciar sesión y no a HTTP, coloque otra vez, la cookie de inicio de sesión es simplemente como secreto como el nombre de usuario y la contraseña y sin utilizar SSL lo envía en texto no cifrado a través de la conexión. Además, ya se ha tomado el tiempo para realizar el protocolo de enlace y proteja el canal (que es la mayor parte de lo que hace más lenta que HTTP HTTPS) antes de ejecuta la canalización de MVC, por lo que volviendo a HTTP después de que ha iniciado sesión no realizar la solicitud actual o futuro solicitudes mucho más rápidas.

1. En **el Explorador de soluciones**, haga clic en el **MvcAuth** proyecto.
2. Presionar la tecla F4 para mostrar las propiedades del proyecto. Asimismo, desde el **vista** menú puede seleccionar **ventana propiedades**.
3. Cambio **se ha habilitado SSL** en True.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. Copie la dirección URL de SSL (que será `https://localhost:44300/` a menos que haya creado otros proyectos SSL).
5. En **el Explorador de soluciones**, haga clic con el **MvcAuth** de proyecto y seleccione **propiedades**.
6. Seleccione el **Web** ficha y, a continuación, pegue la dirección URL de SSL en el **dirección Url del proyecto** cuadro. Guarde el archivo (Ctl + S). Necesitará esta dirección URL para configurar aplicaciones de la autenticación de Google y Facebook.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. Agregar el [RequireHttps](https://msdn.microsoft.com/en-us/library/system.web.mvc.requirehttpsattribute.aspx) atribuir a la `Home` controlador para requerir todas las solicitudes debe usar HTTPS. Un enfoque más seguro consiste en agregar el [RequireHttps](https://msdn.microsoft.com/en-us/library/system.web.mvc.requirehttpsattribute.aspx) filtro a la aplicación. Vea la sección &quot;proteger la aplicación con SSL y el atributo autorizar&quot; en mi tutoral [crear una aplicación de MVC de ASP.NET con la autenticación y la base de datos SQL e implementar al servicio de aplicaciones de Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). A continuación se muestra una parte del controlador Home.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Presione CTRL+F5 para ejecutar la aplicación. Si ha instalado el certificado en el pasado, puede omitir el resto de esta sección y saltar a [crear una aplicación de Google para OAuth 2 y conectar la aplicación al proyecto](#goog), en caso contrario, siga las instrucciones para confiar en autofirmado certificado que IIS Express ha generado.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. Leer la **advertencia de seguridad** cuadro de diálogo y, a continuación, haga clic en **Sí** si desea instalar el certificado que representa el host local.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. Internet Explorer muestra la *inicio* página y no hay ninguna advertencia de SSL.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome también acepta el certificado y se mostrará el contenido HTTPS sin una advertencia. Firefox usa su propio almacén de certificados, por lo que mostrará una advertencia. Para nuestra aplicación puede hacer clic en **entiende los riesgos**.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>Crear una aplicación de Google para OAuth 2 y conectar la aplicación al proyecto

1. Navegue hasta la [Google Developers Console](https://console.developers.google.com/).
1. Si no ha creado un proyecto antes de, seleccione **credenciales** en la pestaña de la izquierda y, a continuación, seleccione **crear**.
1. En la pestaña de la izquierda, haga clic en **credenciales**.
1. Haga clic en **crear credenciales** , a continuación, **identificador de cliente OAuth**. 

    1. En el **crear ID de cliente** cuadro de diálogo, mantenga el valor predeterminado **aplicación Web** para el tipo de aplicación.
    2. Establecer el **autorizado JavaScript** orígenes a la dirección URL de SSL que usó anteriormente (`https://localhost:44300/` a menos que haya creado otros proyectos SSL)
    3. Establecer el **URI de redireccionamiento autorizados** para:  
         `https://localhost:44300/signin-google`
5. Haga clic en el elemento de menú de la pantalla de consentimiento de OAuth y, a continuación, configure su nombre de producto y de dirección de correo electrónico. Cuando haya completado el formulario, haga clic en **guardar**.
6. Haga clic en el elemento de menú de la biblioteca, buscar **API de Google +**, haga clic en él, a continuación, presione Enable.
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
 La imagen siguiente muestra las API habilitadas.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. Desde el Administrador de API de API de Google, visite la **credenciales** tab para obtener la **Id. de cliente**. Descarga para guardar un archivo JSON con secretos de la aplicación. Copie y pegue el **ClientId** y **ClientSecret** en el `UseGoogleAuthentication` método se encuentra en la *Startup.Auth.cs* un archivo en el *App_Start* carpeta. El **ClientId** y **ClientSecret** son ejemplos de valores que se muestran a continuación y no funcionan.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > Seguridad - nunca almacenar los datos confidenciales en el código fuente. La cuenta y las credenciales se agregan al código anterior para simplificar el ejemplo. Vea [las prácticas recomendadas para implementar las contraseñas y otros datos confidenciales en ASP.NET y el servicio de aplicación de Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
8. Presione **CTRL+F5** para compilar y ejecutar la aplicación. Haga clic en el **sesión** vínculo.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. En **utilice otro servicio para iniciar sesión**, haga clic en **Google**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > Si se salta cualquiera de los pasos anteriores le devolverá un error HTTP 401. Volver a comprobar los pasos anteriores. Si se salta un valor obligatorio (por ejemplo **nombre de producto**), agregue la falta de artículos y guardar, puede tardar unos minutos para que funcione la autenticación.
10. Se le redirigirá al sitio de google que deberá especificar sus credenciales.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. Después de escribir sus credenciales, se le pedirá que le asigne permisos a la aplicación web que acaba de crear:
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. Haga clic en **Aceptar**. Ahora redirigirá a la **registrar** página de la aplicación MvcAuth donde puede registrar su cuenta de Google. Tiene la opción de cambiar el nombre de registro de correo electrónico local utilizado para la cuenta de Gmail, pero generalmente desean mantener el alias de correo electrónico de manera predeterminada (es decir, la compañía utilizado para la autenticación). Haga clic en **Registrarse**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>Crear la aplicación de Facebook y conectar la aplicación al proyecto

Para la autenticación de Facebook OAuth2, debe copiar en el proyecto algunas opciones de configuración de una aplicación que cree en Facebook.

1. En el explorador, vaya a [https://developers.facebook.com/apps](https://developers.facebook.com/apps) e inicie sesión, escriba sus credenciales de Facebook.
2. Si ya no se registra como un programador de Facebook, haga clic en **registrar como desarrollador** y siga las instrucciones para registrar.
3. En el **aplicaciones** , haga clic en **crear una aplicación nueva**.

    ![Crear una nueva aplicación](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. Escriba un **nombre de la aplicación** y **categoría**, a continuación, haga clic en **crear aplicación**.

    Esto debe ser único en Facebook. El **aplicación Namespace** es la parte de la dirección URL que la aplicación utilizará para tener acceso a la aplicación de Facebook para la autenticación (por ejemplo, https://apps.facebook.com/ {aplicación Namespace}). Si no se especifica un **aplicación Namespace**, el **identificador de la aplicación** se usará para la dirección URL. El **Id. de aplicación** es un número long-generados por el sistema que se incluye en el paso siguiente.

    ![Crear cuadro de diálogo nueva aplicación](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. Enviar la comprobación de seguridad estándar.

    ![Comprobación de seguridad](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. Seleccione **configuración** de la barra de menú de la izquierda![ Barra de menús del programador de Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)
7. En el **básica** sección de configuración de la página Seleccionar **Agregar plataforma** para especificar que va a agregar una aplicación del sitio Web. ![Configuración básica](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)
8. Seleccione **sitio Web** entre las opciones de plataforma.  
  
    ![Opciones de plataforma](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. Tome nota de su **Id. de aplicación** y su **secreto de la aplicación** para que pueda agregar tanto en la aplicación de MVC más adelante en este tutorial. Además, agregue la dirección URL del sitio (`https://localhost:44300/`) para probar la aplicación MVC. Además, agregue un **correo electrónico de contacto**. A continuación, seleccione **guardar cambios**.   

    ![Página de detalles de una aplicación básica](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > Tenga en cuenta que sólo pueda autenticarse mediante el alias de correo electrónico que se ha registrado. Otros usuarios y cuentas de prueba no será capaz de registrar. Puede conceder acceso de otras cuentas de Facebook para la aplicación de Facebook **Roles de desarrollador** ficha.
10. En Visual Studio, abra *aplicación\_Start\Startup.Auth.cs*.
11. Copie y pegue el **AppId** y **secreto de la aplicación** en el `UseFacebookAuthentication` método. El **AppId** y **secreto de la aplicación** son ejemplos de valores que se muestran a continuación y no funcionará.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. Haga clic en **guardar cambios**.
13. Presione **CTRL+F5** para ejecutar la aplicación.


Seleccione **sesión** para mostrar la página de inicio de sesión. Haga clic en **Facebook** en **utilice otro servicio para iniciar sesión.**

Escriba sus credenciales de Facebook.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

Se le pedirá que conceda permiso para la aplicación tener acceso a su perfil público y una lista de confianza.

![Detalles de la aplicación de Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

Ahora que haya iniciado sesión. Ahora puede registrar esta cuenta con la aplicación.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

Al registrar, se agrega una entrada para el *usuarios* tabla de la base de datos de pertenencia.

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>Examinar los datos de pertenencia

En el **vista** menú, haga clic en **Explorador de servidores**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

Expanda **DefaultConnection (MvcAuth)**, expanda **tablas**, haga clic en **AspNetUsers** y haga clic en **mostrar datos de tabla**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![datos de la tabla de aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>Agregar datos de perfil para la clase de usuario

En esta sección agregará la fecha de nacimiento y ciudad natal a los datos de usuario durante el registro, como se muestra en la siguiente imagen.

![reg con ciudad natal y Cump.](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

Abra la *Models\IdentityModels.cs* de archivos y agregar propiedades de Pueblo de página principal y la fecha de nacimiento:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

Abra la *Models\AccountViewModels.cs* de archivos y el conjunto de propiedades de ciudad de fecha y de inicio en de nacimiento `ExternalLoginConfirmationViewModel`.

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

Abra la *Controllers\AccountController.cs* de archivos y agregue código para la ciudad de página principal y la fecha de nacimiento en la `ExternalLoginConfirmation` método de acción como se muestra:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

Agregar la fecha de nacimiento y ciudad natal a la *Views\Account\ExternalLoginConfirmation.cshtml* archivo:

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

Eliminar la base de datos de pertenencia para que pueda volver a registrar su cuenta de Facebook con la aplicación y compruebe que puede agregar la nueva fecha de nacimiento e información de perfil de ciudad natal.

De **el Explorador de soluciones**, haga clic en el **mostrar todos los archivos** icono y, a continuación, haga clic derecho *agregar\_Data\aspnet-MvcAuth -&lt;dateStamp&gt;.mdf* y haga clic en **eliminar**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

Desde el **herramientas** menú, haga clic en **Administrador de paquetes de NuGet**, a continuación, haga clic en **Package Manager Console** (PMC). Escriba los siguientes comandos en el PMC.

1. Enable-Migrations
2. Migración agregar Init
3. Actualizar base de datos

Ejecute la aplicación y usar Google y FaceBook para iniciar sesión y registrar algunos usuarios.

## <a name="examine-the-membership-data"></a>Examinar los datos de pertenencia

En el **vista** menú, haga clic en **Explorador de servidores**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

Haga clic en **AspNetUsers** y haga clic en **mostrar datos de tabla**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

El `HomeTown` y `BirthDate` a continuación se muestran los campos.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>Cerrar la aplicación de la sesión e iniciar sesión con otra cuenta

Si iniciar sesión en su aplicación con Facebook y, a continuación, cierre sesión y vuelva a intentar iniciar nuevo con una cuenta de Facebook diferente (con el mismo explorador), se inmediatamente grabará en a la cuenta de Facebook anterior que usa. Para usar otra cuenta, debe navegar a Facebook y cerrar la sesión en Facebook. La misma regla se aplica a cualquier otro proveedor de autenticación parte 3ª. Como alternativa, puede iniciar sesión con otra cuenta mediante un explorador diferente.

## <a name="next-steps"></a>Pasos siguientes

Vea [Introducción a los proveedores de seguridad de Yahoo y LinkedIn OAuth para OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) por Jerrie Pelser para obtener instrucciones de Yahoo y LinkedIn. Consulte del Jerrie queda botones de inicio de sesión social para ASP.NET MVC 5 obtener habilitar inicio de sesión social botones.

Siga el tutorial [crear una aplicación de MVC de ASP.NET con la autenticación y la base de datos SQL e implementar al servicio de aplicaciones de Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), que sigue este tutorial y muestra lo siguiente:

1. Cómo implementar la aplicación en Azure.
2. Cómo proteger las aplicaciones con los roles.
3. Cómo proteger la aplicación con el [RequireHttps](https://msdn.microsoft.com/en-us/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) y [Authorize](https://msdn.microsoft.com/en-us/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtros.
4. Describe cómo usar la API de pertenencia para agregar usuarios y roles.

Vota sobre cómo le gustó este tutorial y lo que podemos mejorar. También puede solicitar nuevos temas en [mostrar Me cómo con código](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code). Incluso puede solicitar y votar sobre las nuevas características que se agregarán a ASP.NET. Por ejemplo, puede votar por una herramienta para [crear y administrar usuarios y roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

Para una buena explicación de cómo funcionan los servicios de autenticación externos de ASP.NET, vea de Robert McMurray [servicios de autenticación externos](https://asp.net/web-api/overview/security/external-authentication-services). Artículo de Robert también entra en detalle para habilitar la autenticación de Microsoft y Twitter. Tom Dykstra del excelente [tutorial EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) muestra cómo trabajar con Entity Framework.
