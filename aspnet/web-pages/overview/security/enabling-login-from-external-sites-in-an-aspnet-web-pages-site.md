---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Iniciar sesión con sitios externos en un ASP.NET Web Pages (Razor) sitio | Documentos de Microsoft
author: tfitzmac
description: En este artículo se explica cómo iniciar sesión en el sitio de ASP.NET Web Pages (Razor) con Facebook, Google, Twitter, Yahoo y otros sitios, es decir, cómo admitir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 47d15686194b15b7b06a99d63125c19a41f91ed9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530174"
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a>Iniciar sesión con sitios externos en un sitio de ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo iniciar sesión en el sitio de ASP.NET Web Pages (Razor) con Facebook, Google, Twitter, Yahoo y otros sitios, es decir, cómo admitir OAuth y OpenID en su sitio.
> 
> Lo que aprenderá:
> 
> - Cómo habilitar el inicio de sesión desde otros sitios cuando se usa la plantilla de sitio de inicio de WebMatrix.
> 
> Se trata de la característica ASP.NET que se introdujo en el artículo:
> 
> - El `OAuthWebSecurity` auxiliar.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - 3 de WebMatrix

Las páginas Web ASP.NET incluye compatibilidad para [OAuth](http://oauth.net/) y [OpenID](http://openid.net/) proveedores. Con estos proveedores, puede dejar que los usuarios inicien sesión en el sitio con sus credenciales existentes de Facebook, Twitter, Google y Microsoft. Por ejemplo, para iniciar sesión con una cuenta de Facebook, los usuarios solo pueden elegir un icono de Facebook, que redirige a la página de inicio de sesión de Facebook en el que especificar su información de usuario. A continuación, puede asociar el inicio de sesión de Facebook con su cuenta en el sitio. Una mejora relacionada a las características de pertenencia de páginas Web es que los usuarios pueden asociar varios inicios de sesión (incluidos los inicios de sesión de los sitios de redes sociales) con una sola cuenta en el sitio Web.

Esta imagen muestra la página de inicio de sesión desde el **Starter Site** plantilla, donde un usuario puede elegir un icono de Facebook, Twitter, Google o Microsoft para habilitar el inicio de sesión con una cuenta externa:

![proveedores externos](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

Puede habilitar la pertenencia de OAuth y OpenID quitando el comentario de unas pocas líneas de código en el **Starter Site** plantilla. Los métodos y propiedades se usan para trabajar con el de OAuth y OpenID proveedores están en la `WebMatrix.Security.OAuthWebSecurity` clase. El **Starter Site** plantilla incluye una infraestructura completa de pertenencia, junto con una página de inicio de sesión, una base de datos de pertenencia y todo el código necesario permitir que los usuarios inicien sesión en el sitio con credenciales locales o los de otro sitio .

Esta sección proporciona un ejemplo de cómo permitir que los usuarios inicien sesión desde sitios externos a un sitio que se basa en el **Starter Site** plantilla. Después de crear un sitio de inicio, se ello (detalles):

- Para los sitios que usan un proveedor de OAuth (Facebook, Twitter y Microsoft), crear una aplicación en el sitio externo. Esto proporciona las claves de la aplicación que se necesita para invocar la característica de inicio de sesión para esos sitios.
- Para los sitios que usan un proveedor de OpenID (Google), no es necesario crear una aplicación. Para todos estos sitios, debe tener una cuenta para iniciar sesión y para crear aplicaciones de desarrollador.

    > [!NOTE]
    > Las aplicaciones de Microsoft solo aceptan una dirección URL en vivo para un sitio Web de trabajo, por lo que no puede usar una dirección URL del sitio Web local para probar los inicios de sesión.
- Editar algunos archivos en su sitio Web con el fin de especificar el proveedor de autenticación adecuado y enviar un inicio de sesión para el sitio que desea usar.

Este artículo proporciona instrucciones independientes para las siguientes tareas:

- [Habilitar los inicios de sesión de Google](#To_enable_Google_logins)
- [Habilitar los inicios de sesión de Facebook](#To_enable_Facebook_logins)
- [Habilitar los inicios de sesión de Twitter](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a>Habilitar los inicios de sesión de Google

1. Crear o abrir un sitio de ASP.NET Web Pages que se basa en la plantilla de sitio de inicio de WebMatrix.
2. Abra la  *\_AppStart.cshtml* página y elimine la línea siguiente de código. 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a>Probar el inicio de sesión de Google

1. Ejecute el *default.cshtml* página del sitio y elija la **sesión** botón.
2. En el *inicio de sesión* página, en la **utilice otro servicio para iniciar sesión** sección, elija el **Google** o **Yahoo** botón de envío. Este ejemplo utiliza el inicio de sesión de Google. 

    La página web se redirige la solicitud a la página de inicio de sesión de Google.

    ![Inicio de sesión de Google en](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. Escriba las credenciales para una cuenta de Google existente.
4. Si Google le pregunta si desea permitir *Localhost* para usar la información de la cuenta, haga clic en **permitir**.

    El código usa el token de Google para autenticar al usuario y, a continuación, vuelva a esta página en el sitio Web. Esta página permite a los usuarios asociar su inicio de sesión de Google con una cuenta existente en el sitio Web, o puede registrar una nueva cuenta en el sitio que desea asociar el inicio de sesión externo con.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. Elija la **asociar** botón. El explorador vuelve a la página principal de la aplicación.

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a>Habilitar los inicios de sesión de Facebook

1. Vaya a la [sitio de los desarrolladores de Facebook](https://developers.facebook.com/apps) (inicie sesión si todavía no ha iniciado sesión).
2. Elija la **crear una aplicación nueva** botón y, a continuación, siga las indicaciones para asignar un nombre y crear la nueva aplicación.
3. En la sección **seleccionar cómo se integrará la aplicación con Facebook**, elija la **sitio Web** sección.
4. Rellene la **dirección URL del sitio** campo con la dirección URL del sitio (por ejemplo, `http://www.example.com`). El **dominio** campo es opcional; puede utilizar esto para proporcionar autenticación para un dominio completo (como *ejemplo.com*). 

    > [!NOTE]
    > Si está ejecutando un sitio en el equipo local con una dirección URL como `http://localhost:12345` (donde el número es un número de puerto local), puede agregar este valor para el **dirección URL del sitio** campo para probar su sitio. Sin embargo, siempre que el número de puerto de los cambios en el sitio local, debe actualizar el **dirección URL del sitio** campo de la aplicación.
5. Elija la **guardar cambios** botón.
6. Elija la **aplicaciones** ficha nuevo y, a continuación, ver la página de inicio de la aplicación.
7. Copia la **Id. de aplicación** y **secreto de la aplicación** valores para la aplicación y péguelos en un archivo de texto temporal. En el código de sitio Web pasará estos valores para el proveedor de Facebook.
8. Salir del sitio para desarrolladores de Facebook.

Ahora realizar cambios en dos páginas en el sitio Web para que los usuarios podrán iniciar sesión en el sitio mediante sus cuentas de Facebook.

1. Crear o abrir un sitio de ASP.NET Web Pages que se basa en la plantilla de sitio de inicio de WebMatrix.
2. Abra la  *\_AppStart.cshtml* página y quite el código para el proveedor de Facebook OAuth. El bloque de código hace referencia es similar a lo siguiente: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. Copia la **Id. de aplicación** valor de la aplicación de Facebook como el valor de la `appId` parámetro (dentro de las comillas).
4. Copia **secreto de la aplicación** valor de la aplicación de Facebook como el `appSecret` el valor del parámetro.
5. Guarde y cierre el archivo.

### <a name="testing-facebook-login"></a>Probar el inicio de sesión de Facebook

1. Ejecute el sitio de Web *default.cshtml* página y elija la **inicio de sesión** botón.
2. En el *inicio de sesión* página, en la **utilice otro servicio para iniciar sesión** sección, elija el **Facebook** icono. 

    La página web se redirige la solicitud a la página de inicio de sesión de Facebook.

    ![OAuth-2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. Inicie sesión en una cuenta de Facebook. 

    El código usa el token de Facebook para autenticarle y, a continuación, vuelve a una página donde puede asociar su inicio de sesión de Facebook con inicio de sesión de su sitio. Su dirección de correo electrónico o nombre de usuario se rellena en el **correo electrónico** campo en el formulario.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. Elija la **asociar** botón. 

    El explorador vuelve a la página principal y que haya iniciado sesión.

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a>Habilitar los inicios de sesión de Twitter

1. Vaya a la [sitio de los desarrolladores de Twitter](https://dev.twitter.com/).
2. Elija la **crear una aplicación** vincular y, a continuación, inicie sesión en el sitio.
3. En el **crear una aplicación** forman, rellene el **nombre** y **descripción** campos.
4. En el **sitio Web** , escriba la dirección URL del sitio (por ejemplo, `http://www.example.com`). 

    > [!NOTE]
    > Si va a probar su sitio localmente (mediante una dirección URL como `http://localhost:12345`), Twitter podría no aceptar la dirección URL. Sin embargo, es posible que pueda usar la dirección IP de bucle invertido local (por ejemplo `http://127.0.0.1:12345`). Esto simplifica el proceso de probar la aplicación localmente. Sin embargo, cada vez que cambia el número de puerto del sitio local, debe actualizar el **sitio Web** campo de la aplicación.
5. En el **dirección URL de devolución de llamada** , escriba una dirección URL de la página en el sitio Web que desea que los usuarios para volver a tras iniciar sesión en Twitter. Por ejemplo, para enviar a los usuarios a la página principal del sitio de inicio (que reconozca su estado ha iniciado la sesión), escriba la misma dirección URL que escribió en el **sitio Web** campo.
6. Acepte los términos y elija la **crear su aplicación de Twitter** botón.
7. En el **mis aplicaciones** aterrizaje página, elija la aplicación que ha creado.
8. En el **detalles** ficha, desplácese hasta la parte inferior y elija la **crear mi Token de acceso** botón.
9. En el **detalles** ficha, copie el **clave de consumidor** y **secreto de consumidor** valores para la aplicación y péguelos en un archivo de texto temporal. Deberá pasar estos valores para el proveedor de Twitter en el código de sitio Web.
10. Salen del sitio de Twitter.

Ahora realizar cambios en dos páginas en el sitio Web para que los usuarios podrán iniciar sesión en el sitio con sus cuentas de Twitter.

1. Crear o abrir un sitio de ASP.NET Web Pages que se basa en la plantilla de sitio de inicio de WebMatrix.
2. Abra la  *\_AppStart.cshtml* página y quite el código para el proveedor de OAuth de Twitter. El bloque de código hace referencia tiene el siguiente aspecto: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. Copia la **clave de consumidor** valor de la aplicación de Twitter como el valor de la `consumerKey` parámetro (dentro de las comillas).
4. Copia la **secreto de consumidor** valor de la aplicación de Twitter como el valor de la `consumerSecret` parámetro.
5. Guarde y cierre el archivo.

### <a name="testing-twitter-login"></a>Probar el inicio de sesión de Twitter

1. Ejecute el *default.cshtml* página del sitio y elija la **inicio de sesión** botón.
2. En el *inicio de sesión* página, en la **utilice otro servicio para iniciar sesión** sección, elija el **Twitter** icono. 

    La página web se redirige la solicitud a una página de inicio de sesión de Twitter para la aplicación que ha creado.

    ![OAuth-4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. Inicie sesión en una cuenta de Twitter.
4. El código usa el token de Twitter para autenticar al usuario y, a continuación, vuelve a abrir una página donde puede asociar su inicio de sesión con su cuenta de sitio Web. El nombre o dirección de correo se rellena en el **correo electrónico** campo en el formulario.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. Elija la **asociar** botón. 

    El explorador vuelve a la página principal y que haya iniciado sesión.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales


- [Personalizar el comportamiento de todo el sitio](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Agregar seguridad y pertenencia a un sitio de páginas Web ASP.NET](https://go.microsoft.com/fwlink/?LinkID=202904)
