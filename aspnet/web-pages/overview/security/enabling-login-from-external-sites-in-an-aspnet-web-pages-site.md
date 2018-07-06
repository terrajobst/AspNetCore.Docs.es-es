---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Iniciar sesión en sitios externos en ASP.NET Web Pages (Razor) sitio | Microsoft Docs
author: tfitzmac
description: En este artículo se explica cómo iniciar sesión en el sitio de ASP.NET Web Pages (Razor) con Facebook, Twitter, Google, Yahoo y otros sitios, es decir, cómo admitir...
ms.author: aspnetcontent
ms.date: 02/21/2014
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: e6c5b578b4c74fd04136d31751d51b59ba9197ba
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825442"
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a>Iniciar sesión con sitios externos en un sitio Web de ASP.NET Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo iniciar sesión en el sitio de ASP.NET Web Pages (Razor) con Facebook, Twitter, Google, Yahoo y otros sitios, es decir, cómo admitir OAuth y OpenID en su sitio.
> 
> Lo que aprenderá:
> 
> - Cómo habilitar el inicio de sesión desde otros sitios cuando se usa la plantilla de sitio de inicio de WebMatrix.
> 
> Esta es la característica ASP.NET que se introdujo en el artículo:
> 
> - El `OAuthWebSecurity` auxiliar.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3

Las páginas Web ASP.NET incluye compatibilidad para [OAuth](http://oauth.net/) y [OpenID](http://openid.net/) proveedores. Con estos proveedores, puede permitir que los usuarios inicien sesión en su sitio mediante sus credenciales de Google, Facebook, Twitter y Microsoft. Por ejemplo, para iniciar sesión con una cuenta de Facebook, los usuarios pueden elegir simplemente un icono de Facebook, que redirige a la página de inicio de sesión de Facebook donde que escriba su información de usuario. A continuación, puede asociar el inicio de sesión de Facebook con su cuenta en su sitio. Una mejora relacionada a las características de pertenencia de las páginas Web es que los usuarios pueden asociar varios inicios de sesión (incluidos los inicios de sesión de sitios de redes sociales) con una sola cuenta en su sitio Web.

Esta imagen muestra la página de inicio de sesión desde el **Starter Site** plantilla, donde un usuario puede elegir un icono de Facebook, Twitter, Google o Microsoft para habilitar el inicio de sesión con una cuenta externa:

![proveedores externos](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

Para habilitar la pertenencia de OAuth y OpenID quitando los comentarios unas pocas líneas de código en el **Starter Site** plantilla. Los métodos y propiedades se utiliza para trabajar con el de OAuth y OpenID proveedores están en el `WebMatrix.Security.OAuthWebSecurity` clase. El **Starter Site** plantilla incluye una infraestructura completa de pertenencia, junto con una página de inicio de sesión, una base de datos de pertenencia y todo el código necesario permitir que los usuarios inicien sesión en su sitio mediante credenciales locales o los de otro sitio .

Esta sección proporciona un ejemplo de cómo permitir que los usuarios iniciar sesión desde sitios externos en un sitio que se basa en el **Starter Site** plantilla. Después de crear un sitio de inicio, siga este (detalles de seguimiento):

- Para los sitios que usan un proveedor de OAuth (Facebook, Twitter y Microsoft), crear una aplicación en el sitio externo. Esto proporciona las claves de aplicación que necesitará para invocar la característica de inicio de sesión para esos sitios.
- Para los sitios que usan un proveedor de OpenID (Google), no es necesario que crear una aplicación. Para todos estos sitios, debe tener una cuenta con el fin de iniciar sesión y crear aplicaciones de desarrollador.

    > [!NOTE]
    > Las aplicaciones de Microsoft solo aceptan una dirección URL en directo para un sitio Web en funcionamiento, por lo que no puede usar una dirección URL del sitio Web local para probar los inicios de sesión.
- Editar algunos archivos en su sitio Web con el fin de especificar el proveedor de autenticación adecuado y enviar un inicio de sesión en el sitio que desea usar.

En este artículo proporciona instrucciones independientes para las siguientes tareas:

- [Habilitar los inicios de sesión de Google](#To_enable_Google_logins)
- [Habilitar los inicios de sesión de Facebook](#To_enable_Facebook_logins)
- [Habilitar los inicios de sesión de Twitter](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a>Habilitar los inicios de sesión de Google

1. Cree o abra un sitio de ASP.NET Web Pages que se basa en la plantilla de sitio de inicio de WebMatrix.
2. Abra el  *\_AppStart.cshtml* página y elimine la siguiente línea de código. 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a>Probar el inicio de sesión de Google

1. Ejecute el *default.cshtml* página del sitio y elija el **iniciarla** botón.
2. En el *inicio de sesión* página, en el **utilice otro servicio para iniciar sesión** sección, elija el **Google** o **Yahoo** botón de envío. Este ejemplo usa el inicio de sesión de Google. 

    La página web redirige la solicitud a la página de inicio de sesión de Google.

    ![Inicio de sesión de Google](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. Escriba las credenciales para una cuenta de Google existente.
4. Si Google le pregunta si desea permitir *Localhost* para usar la información de la cuenta, haga clic en **permitir**.

    El código usa el token de Google para autenticar al usuario y, a continuación, vuelve a esta página en su sitio Web. Esta página permite a los usuarios asociar su inicio de sesión de Google con una cuenta existente en su sitio Web o puede registrar una nueva cuenta en su sitio para asociar con el inicio de sesión externo.

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. Elija la **asociar** botón. El explorador vuelve a la página principal de la aplicación.

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a>Habilitar los inicios de sesión de Facebook

1. Vaya a la [sitio de desarrolladores de Facebook](https://developers.facebook.com/apps) (inicie sesión si aún no ha iniciado sesión).
2. Elija la **crear nueva aplicación** botón y, a continuación, siga las indicaciones para nombrar y crear la nueva aplicación.
3. En la sección **Seleccione cómo se integrará su aplicación con Facebook**, elija el **sitio Web** sección.
4. Rellene el **dirección URL del sitio** campo con la dirección URL del sitio (por ejemplo, `http://www.example.com`). El **dominio** campo es opcional; puede usar esto para proporcionar autenticación para un dominio completo (como *ejemplo.com*). 

    > [!NOTE]
    > Si está ejecutando un sitio en el equipo local con una dirección URL como `http://localhost:12345` (donde el número es un número de puerto local), puede agregar este valor para el **dirección URL del sitio** campo para probar su sitio. Sin embargo, cada vez que el número de puerto de los cambios del sitio local, deberá actualizar el **dirección URL del sitio** campo de la aplicación.
5. Elija la **guardar cambios** botón.
6. Elija la **aplicaciones** pestaña nuevo y, a continuación, ver la página de inicio para la aplicación.
7. Copia el **Id. de aplicación** y **secreto de la aplicación** valores para la aplicación y péguelos en un archivo de texto temporal. En el código de sitio Web pasará estos valores para el proveedor de Facebook.
8. Salga del sitio para desarrolladores de Facebook.

Ahora realizar cambios en dos páginas en su sitio Web para que los usuarios podrán iniciar sesión en el sitio mediante sus cuentas de Facebook.

1. Cree o abra un sitio de ASP.NET Web Pages que se basa en la plantilla de sitio de inicio de WebMatrix.
2. Abra el  *\_AppStart.cshtml* página y quite los comentarios del código para el proveedor de Facebook OAuth. El bloque de código de marca de comentario tiene el siguiente aspecto: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. Copia el **Id. de aplicación** valor de la aplicación de Facebook como el valor de la `appId` parámetro (dentro de las comillas).
4. Copia **secreto de la aplicación** valor de la aplicación de Facebook como el `appSecret` el valor del parámetro.
5. Guarde y cierre el archivo.

### <a name="testing-facebook-login"></a>Probar el inicio de sesión de Facebook

1. Ejecute el sitio *default.cshtml* página y elija el **inicio de sesión** botón.
2. En el *inicio de sesión* página, en el **utilice otro servicio para iniciar sesión** sección, elija el **Facebook** icono. 

    La página web redirige la solicitud a la página de inicio de sesión de Facebook.

    ![OAuth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. Inicie sesión en una cuenta de Facebook. 

    El código usa el token de Facebook para la autenticación y, a continuación, vuelve a una página donde puede asociar el inicio de sesión de Facebook con inicio de sesión de su sitio. Su dirección de correo electrónico o nombre de usuario se rellena en el **correo electrónico** campo en el formulario.

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. Elija la **asociar** botón. 

    El explorador vuelve a la página principal y que haya iniciado sesión.

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a>Habilitar los inicios de sesión de Twitter

1. Vaya a la [sitio de desarrolladores de Twitter](https://dev.twitter.com/).
2. Elija la **crear una aplicación** vincular y, a continuación, inicie sesión en el sitio.
3. En el **crear una aplicación** forman, rellene el **nombre** y **descripción** campos.
4. En el **sitio Web** , escriba la dirección URL del sitio (por ejemplo, `http://www.example.com`). 

    > [!NOTE]
    > Si va a probar su sitio local (mediante una dirección URL como `http://localhost:12345`), Twitter no puede aceptar la dirección URL. Sin embargo, es posible que pueda usar la dirección IP de bucle invertido local (por ejemplo `http://127.0.0.1:12345`). Esto simplifica el proceso de probar la aplicación localmente. Sin embargo, cada vez que cambia el número de puerto del sitio local, deberá actualizar el **sitio Web** campo de la aplicación.
5. En el **dirección URL de devolución de llamada** , escriba una dirección URL de la página en su sitio Web que desea que los usuarios vuelvan a tras iniciar sesión en Twitter. Por ejemplo, para enviar a los usuarios a la página principal del sitio de inicio (que reconozca su estado ha iniciado sesión), escriba la misma dirección URL que escribió en el **sitio Web** campo.
6. Acepte los términos y elija el **crear aplicación de Twitter** botón.
7. En el **My Applications** aterrizaje de la página, elija la aplicación que creó.
8. En el **detalles** pestaña, desplácese hacia abajo y elija el **crear mi Token de acceso** botón.
9. En el **detalles** pestaña, copie el **clave de consumidor** y **secreto de consumidor** valores para la aplicación y péguelos en un archivo de texto temporal. Pasará estos valores para el proveedor de Twitter en el código de sitio Web.
10. Salga del sitio de Twitter.

Ahora realizar cambios en dos páginas en su sitio Web para que los usuarios podrán iniciar sesión en el sitio con sus cuentas de Twitter.

1. Cree o abra un sitio de ASP.NET Web Pages que se basa en la plantilla de sitio de inicio de WebMatrix.
2. Abra el  *\_AppStart.cshtml* página y quite los comentarios del código para el proveedor de OAuth de Twitter. El bloque de marca de comentario de código tiene este aspecto: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. Copia el **clave de consumidor** valor de la aplicación de Twitter como el valor de la `consumerKey` parámetro (dentro de las comillas).
4. Copia el **secreto de consumidor** valor de la aplicación de Twitter como el valor de la `consumerSecret` parámetro.
5. Guarde y cierre el archivo.

### <a name="testing-twitter-login"></a>Probar el inicio de sesión de Twitter

1. Ejecute el *default.cshtml* página del sitio y elija el **inicio de sesión** botón.
2. En el *inicio de sesión* página, en el **utilice otro servicio para iniciar sesión** sección, elija el **Twitter** icono. 

    La página web redirige la solicitud a una página de inicio de sesión de Twitter para la aplicación que creó.

    ![OAuth 4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. Inicie sesión en una cuenta de Twitter.
4. El código usa el token de Twitter para autenticar al usuario y, a continuación, vuelve a una página donde se puede asociar el inicio de sesión con su cuenta de sitio Web. Su dirección de correo electrónico o nombre se rellena en el **correo electrónico** campo en el formulario.

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. Elija la **asociar** botón. 

    El explorador vuelve a la página principal y que haya iniciado sesión.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales


- [Personalizar el comportamiento de todo el sitio](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Agregar seguridad y pertenencia a un sitio ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkID=202904)
