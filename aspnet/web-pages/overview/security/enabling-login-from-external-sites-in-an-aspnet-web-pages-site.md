---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Iniciar sesión en sitios externos en ASP.NET Web Pages (Razor) sitio | Microsoft Docs
author: tfitzmac
description: En este artículo se explica cómo iniciar sesión en el sitio de ASP.NET Web Pages (Razor) con Facebook, Twitter, Google, Yahoo y otros sitios, es decir, cómo admitir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: b161a890d11b1c0bf86669b819b97255c075075f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396895"
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="4a392-103">Iniciar sesión con sitios externos en un sitio Web de ASP.NET Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="4a392-103">Logging In Using External Sites in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="4a392-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4a392-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="4a392-105">En este artículo se explica cómo iniciar sesión en el sitio de ASP.NET Web Pages (Razor) con Facebook, Twitter, Google, Yahoo y otros sitios, es decir, cómo admitir OAuth y OpenID en su sitio.</span><span class="sxs-lookup"><span data-stu-id="4a392-105">This article explains how to log in to your ASP.NET Web Pages (Razor) site using Facebook, Google, Twitter, Yahoo, and other sites — that is, how to support OAuth and OpenID in your site.</span></span>
> 
> <span data-ttu-id="4a392-106">Lo que aprenderá:</span><span class="sxs-lookup"><span data-stu-id="4a392-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="4a392-107">Cómo habilitar el inicio de sesión desde otros sitios cuando se usa la plantilla de sitio de inicio de WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="4a392-107">How to enable login from other sites when you use the WebMatrix Starter Site template.</span></span>
> 
> <span data-ttu-id="4a392-108">Esta es la característica ASP.NET que se introdujo en el artículo:</span><span class="sxs-lookup"><span data-stu-id="4a392-108">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="4a392-109">El `OAuthWebSecurity` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="4a392-109">The `OAuthWebSecurity` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="4a392-110">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="4a392-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="4a392-111">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="4a392-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="4a392-112">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="4a392-112">WebMatrix 3</span></span>

<span data-ttu-id="4a392-113">Las páginas Web ASP.NET incluye compatibilidad para [OAuth](http://oauth.net/) y [OpenID](http://openid.net/) proveedores.</span><span class="sxs-lookup"><span data-stu-id="4a392-113">ASP.NET Web Pages includes support for [OAuth](http://oauth.net/) and [OpenID](http://openid.net/) providers.</span></span> <span data-ttu-id="4a392-114">Con estos proveedores, puede permitir que los usuarios inicien sesión en su sitio mediante sus credenciales de Google, Facebook, Twitter y Microsoft.</span><span class="sxs-lookup"><span data-stu-id="4a392-114">Using these providers, you can let users log into your site using their existing credentials from Facebook, Twitter, Microsoft, and Google.</span></span> <span data-ttu-id="4a392-115">Por ejemplo, para iniciar sesión con una cuenta de Facebook, los usuarios pueden elegir simplemente un icono de Facebook, que redirige a la página de inicio de sesión de Facebook donde que escriba su información de usuario.</span><span class="sxs-lookup"><span data-stu-id="4a392-115">For example, to log in using a Facebook account, users can just choose a Facebook icon, which redirects them to the Facebook login page where they enter their user information.</span></span> <span data-ttu-id="4a392-116">A continuación, puede asociar el inicio de sesión de Facebook con su cuenta en su sitio.</span><span class="sxs-lookup"><span data-stu-id="4a392-116">They can then associate the Facebook login with their account on your site.</span></span> <span data-ttu-id="4a392-117">Una mejora relacionada a las características de pertenencia de las páginas Web es que los usuarios pueden asociar varios inicios de sesión (incluidos los inicios de sesión de sitios de redes sociales) con una sola cuenta en su sitio Web.</span><span class="sxs-lookup"><span data-stu-id="4a392-117">A related enhancement to the Web Pages membership features is that users can associate multiple logins (including logins from social networking sites) with a single account on your website.</span></span>

<span data-ttu-id="4a392-118">Esta imagen muestra la página de inicio de sesión desde el **Starter Site** plantilla, donde un usuario puede elegir un icono de Facebook, Twitter, Google o Microsoft para habilitar el inicio de sesión con una cuenta externa:</span><span class="sxs-lookup"><span data-stu-id="4a392-118">This image shows the Login page from the **Starter Site** template, where a user can choose a Facebook, Twitter, Google or Microsoft icon to enable logging in with an external account:</span></span>

![proveedores externos](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

<span data-ttu-id="4a392-120">Para habilitar la pertenencia de OAuth y OpenID quitando los comentarios unas pocas líneas de código en el **Starter Site** plantilla.</span><span class="sxs-lookup"><span data-stu-id="4a392-120">You can enable OAuth and OpenID membership by uncommenting a few lines of code in the **Starter Site** template.</span></span> <span data-ttu-id="4a392-121">Los métodos y propiedades se utiliza para trabajar con el de OAuth y OpenID proveedores están en el `WebMatrix.Security.OAuthWebSecurity` clase.</span><span class="sxs-lookup"><span data-stu-id="4a392-121">The methods and properties you use to work with the OAuth and OpenID providers are in the `WebMatrix.Security.OAuthWebSecurity` class.</span></span> <span data-ttu-id="4a392-122">El **Starter Site** plantilla incluye una infraestructura completa de pertenencia, junto con una página de inicio de sesión, una base de datos de pertenencia y todo el código necesario permitir que los usuarios inicien sesión en su sitio mediante credenciales locales o los de otro sitio .</span><span class="sxs-lookup"><span data-stu-id="4a392-122">The **Starter Site** template includes a full membership infrastructure, complete with a login page, a membership database, and all the code you need to let users log into your site using either local credentials or those from another site.</span></span>

<span data-ttu-id="4a392-123">Esta sección proporciona un ejemplo de cómo permitir que los usuarios iniciar sesión desde sitios externos en un sitio que se basa en el **Starter Site** plantilla.</span><span class="sxs-lookup"><span data-stu-id="4a392-123">This section provides an example of how to let users log in from external sites to a site that's based on the **Starter Site** template.</span></span> <span data-ttu-id="4a392-124">Después de crear un sitio de inicio, siga este (detalles de seguimiento):</span><span class="sxs-lookup"><span data-stu-id="4a392-124">After creating a starter site, you do this (details follow):</span></span>

- <span data-ttu-id="4a392-125">Para los sitios que usan un proveedor de OAuth (Facebook, Twitter y Microsoft), crear una aplicación en el sitio externo.</span><span class="sxs-lookup"><span data-stu-id="4a392-125">For the sites that use an OAuth provider (Facebook, Twitter, and Microsoft), you create an application on the external site.</span></span> <span data-ttu-id="4a392-126">Esto proporciona las claves de aplicación que necesitará para invocar la característica de inicio de sesión para esos sitios.</span><span class="sxs-lookup"><span data-stu-id="4a392-126">This gives you application keys that you'll need in order to invoke the login feature for those sites.</span></span>
- <span data-ttu-id="4a392-127">Para los sitios que usan un proveedor de OpenID (Google), no es necesario que crear una aplicación.</span><span class="sxs-lookup"><span data-stu-id="4a392-127">For sites that use an OpenID provider (Google), you do not have to create an application.</span></span> <span data-ttu-id="4a392-128">Para todos estos sitios, debe tener una cuenta con el fin de iniciar sesión y crear aplicaciones de desarrollador.</span><span class="sxs-lookup"><span data-stu-id="4a392-128">For all of these sites, you must have an account in order to log in and to create developer applications.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4a392-129">Las aplicaciones de Microsoft solo aceptan una dirección URL en directo para un sitio Web en funcionamiento, por lo que no puede usar una dirección URL del sitio Web local para probar los inicios de sesión.</span><span class="sxs-lookup"><span data-stu-id="4a392-129">Microsoft applications only accept a live URL for a working website, so you cannot use a local website URL for testing logins.</span></span>
- <span data-ttu-id="4a392-130">Editar algunos archivos en su sitio Web con el fin de especificar el proveedor de autenticación adecuado y enviar un inicio de sesión en el sitio que desea usar.</span><span class="sxs-lookup"><span data-stu-id="4a392-130">Edit a few files in your website in order to specify the appropriate authentication provider and to submit a login to the site you want to use.</span></span>

<span data-ttu-id="4a392-131">En este artículo proporciona instrucciones independientes para las siguientes tareas:</span><span class="sxs-lookup"><span data-stu-id="4a392-131">This article provides separate instructions for the following tasks:</span></span>

- [<span data-ttu-id="4a392-132">Habilitar los inicios de sesión de Google</span><span class="sxs-lookup"><span data-stu-id="4a392-132">Enabling Google logins</span></span>](#To_enable_Google_logins)
- [<span data-ttu-id="4a392-133">Habilitar los inicios de sesión de Facebook</span><span class="sxs-lookup"><span data-stu-id="4a392-133">Enabling Facebook logins</span></span>](#To_enable_Facebook_logins)
- [<span data-ttu-id="4a392-134">Habilitar los inicios de sesión de Twitter</span><span class="sxs-lookup"><span data-stu-id="4a392-134">Enabling Twitter logins</span></span>](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a><span data-ttu-id="4a392-135">Habilitar los inicios de sesión de Google</span><span class="sxs-lookup"><span data-stu-id="4a392-135">Enabling Google Logins</span></span>

1. <span data-ttu-id="4a392-136">Cree o abra un sitio de ASP.NET Web Pages que se basa en la plantilla de sitio de inicio de WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="4a392-136">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="4a392-137">Abra el  *\_AppStart.cshtml* página y elimine la siguiente línea de código.</span><span class="sxs-lookup"><span data-stu-id="4a392-137">Open the *\_AppStart.cshtml* page and uncomment the following line of code.</span></span> 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a><span data-ttu-id="4a392-138">Probar el inicio de sesión de Google</span><span class="sxs-lookup"><span data-stu-id="4a392-138">Testing Google login</span></span>

1. <span data-ttu-id="4a392-139">Ejecute el *default.cshtml* página del sitio y elija el **iniciarla** botón.</span><span class="sxs-lookup"><span data-stu-id="4a392-139">Run the *default.cshtml* page of your site and choose the **Log in** button.</span></span>
2. <span data-ttu-id="4a392-140">En el *inicio de sesión* página, en el **utilice otro servicio para iniciar sesión** sección, elija el **Google** o **Yahoo** botón de envío.</span><span class="sxs-lookup"><span data-stu-id="4a392-140">On the *Login* page, in the **Use another service to log in** section, choose either the **Google** or **Yahoo** submit button.</span></span> <span data-ttu-id="4a392-141">Este ejemplo usa el inicio de sesión de Google.</span><span class="sxs-lookup"><span data-stu-id="4a392-141">This example uses the Google login.</span></span> 

    <span data-ttu-id="4a392-142">La página web redirige la solicitud a la página de inicio de sesión de Google.</span><span class="sxs-lookup"><span data-stu-id="4a392-142">The web page redirects the request to the Google login page.</span></span>

    ![Inicio de sesión de Google](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. <span data-ttu-id="4a392-144">Escriba las credenciales para una cuenta de Google existente.</span><span class="sxs-lookup"><span data-stu-id="4a392-144">Enter credentials for an existing Google account.</span></span>
4. <span data-ttu-id="4a392-145">Si Google le pregunta si desea permitir *Localhost* para usar la información de la cuenta, haga clic en **permitir**.</span><span class="sxs-lookup"><span data-stu-id="4a392-145">If Google asks you whether you want to allow *Localhost* to use information from the account, click **Allow**.</span></span>

    <span data-ttu-id="4a392-146">El código usa el token de Google para autenticar al usuario y, a continuación, vuelve a esta página en su sitio Web.</span><span class="sxs-lookup"><span data-stu-id="4a392-146">The code uses the Google token to authenticate the user, and then returns to this page on your website.</span></span> <span data-ttu-id="4a392-147">Esta página permite a los usuarios asociar su inicio de sesión de Google con una cuenta existente en su sitio Web o puede registrar una nueva cuenta en su sitio para asociar con el inicio de sesión externo.</span><span class="sxs-lookup"><span data-stu-id="4a392-147">This page lets users associate their Google login with an existing account on your website, or they can register a new account on your site to associate the external login with.</span></span>

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. <span data-ttu-id="4a392-149">Elija la **asociar** botón.</span><span class="sxs-lookup"><span data-stu-id="4a392-149">Choose the **Associate** button.</span></span> <span data-ttu-id="4a392-150">El explorador vuelve a la página principal de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4a392-150">The browser returns to your application's home page.</span></span>

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a><span data-ttu-id="4a392-151">Habilitar los inicios de sesión de Facebook</span><span class="sxs-lookup"><span data-stu-id="4a392-151">Enabling Facebook Logins</span></span>

1. <span data-ttu-id="4a392-152">Vaya a la [sitio de desarrolladores de Facebook](https://developers.facebook.com/apps) (inicie sesión si aún no ha iniciado sesión).</span><span class="sxs-lookup"><span data-stu-id="4a392-152">Go to the [Facebook developers site](https://developers.facebook.com/apps) (log in if you're not already logged in).</span></span>
2. <span data-ttu-id="4a392-153">Elija la **crear nueva aplicación** botón y, a continuación, siga las indicaciones para nombrar y crear la nueva aplicación.</span><span class="sxs-lookup"><span data-stu-id="4a392-153">Choose the **Create New App** button, and then follow the prompts to name and create the new application.</span></span>
3. <span data-ttu-id="4a392-154">En la sección **Seleccione cómo se integrará su aplicación con Facebook**, elija el **sitio Web** sección.</span><span class="sxs-lookup"><span data-stu-id="4a392-154">In the section **Select how your app will integrate with Facebook**, choose the **Website** section.</span></span>
4. <span data-ttu-id="4a392-155">Rellene el **dirección URL del sitio** campo con la dirección URL del sitio (por ejemplo, `http://www.example.com`).</span><span class="sxs-lookup"><span data-stu-id="4a392-155">Fill in the **Site URL** field with the URL of your site (for example, `http://www.example.com`).</span></span> <span data-ttu-id="4a392-156">El **dominio** campo es opcional; puede usar esto para proporcionar autenticación para un dominio completo (como *ejemplo.com*).</span><span class="sxs-lookup"><span data-stu-id="4a392-156">The **Domain** field is optional; you can use this to provide authentication for an entire domain (such as *example.com*).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="4a392-157">Si está ejecutando un sitio en el equipo local con una dirección URL como `http://localhost:12345` (donde el número es un número de puerto local), puede agregar este valor para el **dirección URL del sitio** campo para probar su sitio.</span><span class="sxs-lookup"><span data-stu-id="4a392-157">If you are running a site on your local computer with a URL like `http://localhost:12345` (where the number is a local port number), you can add this value to the **Site URL** field for testing your site.</span></span> <span data-ttu-id="4a392-158">Sin embargo, cada vez que el número de puerto de los cambios del sitio local, deberá actualizar el **dirección URL del sitio** campo de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4a392-158">However, any time the port number of your local site changes, you will need to update the **Site URL** field of your application.</span></span>
5. <span data-ttu-id="4a392-159">Elija la **guardar cambios** botón.</span><span class="sxs-lookup"><span data-stu-id="4a392-159">Choose the **Save Changes** button.</span></span>
6. <span data-ttu-id="4a392-160">Elija la **aplicaciones** pestaña nuevo y, a continuación, ver la página de inicio para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4a392-160">Choose the **Apps** tab again, and then view the start page for your application.</span></span>
7. <span data-ttu-id="4a392-161">Copia el **Id. de aplicación** y **secreto de la aplicación** valores para la aplicación y péguelos en un archivo de texto temporal.</span><span class="sxs-lookup"><span data-stu-id="4a392-161">Copy the **App ID** and **App Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="4a392-162">En el código de sitio Web pasará estos valores para el proveedor de Facebook.</span><span class="sxs-lookup"><span data-stu-id="4a392-162">You will pass these values to the Facebook provider in your website code.</span></span>
8. <span data-ttu-id="4a392-163">Salga del sitio para desarrolladores de Facebook.</span><span class="sxs-lookup"><span data-stu-id="4a392-163">Exit the Facebook developer site.</span></span>

<span data-ttu-id="4a392-164">Ahora realizar cambios en dos páginas en su sitio Web para que los usuarios podrán iniciar sesión en el sitio mediante sus cuentas de Facebook.</span><span class="sxs-lookup"><span data-stu-id="4a392-164">Now you make changes to two pages in your website so that users will able to log into the site using their Facebook accounts.</span></span>

1. <span data-ttu-id="4a392-165">Cree o abra un sitio de ASP.NET Web Pages que se basa en la plantilla de sitio de inicio de WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="4a392-165">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="4a392-166">Abra el  *\_AppStart.cshtml* página y quite los comentarios del código para el proveedor de Facebook OAuth.</span><span class="sxs-lookup"><span data-stu-id="4a392-166">Open the *\_AppStart.cshtml* page and uncomment the code for the Facebook OAuth provider.</span></span> <span data-ttu-id="4a392-167">El bloque de código de marca de comentario tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="4a392-167">The uncommented code block looks like the following:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. <span data-ttu-id="4a392-168">Copia el **Id. de aplicación** valor de la aplicación de Facebook como el valor de la `appId` parámetro (dentro de las comillas).</span><span class="sxs-lookup"><span data-stu-id="4a392-168">Copy the **App ID** value from the Facebook application as the value of the `appId` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="4a392-169">Copia **secreto de la aplicación** valor de la aplicación de Facebook como el `appSecret` el valor del parámetro.</span><span class="sxs-lookup"><span data-stu-id="4a392-169">Copy **App Secret** value from the Facebook application as the `appSecret` parameter value.</span></span>
5. <span data-ttu-id="4a392-170">Guarde y cierre el archivo.</span><span class="sxs-lookup"><span data-stu-id="4a392-170">Save and close the file.</span></span>

### <a name="testing-facebook-login"></a><span data-ttu-id="4a392-171">Probar el inicio de sesión de Facebook</span><span class="sxs-lookup"><span data-stu-id="4a392-171">Testing Facebook login</span></span>

1. <span data-ttu-id="4a392-172">Ejecute el sitio *default.cshtml* página y elija el **inicio de sesión** botón.</span><span class="sxs-lookup"><span data-stu-id="4a392-172">Run the site's *default.cshtml* page and choose the **Login** button.</span></span>
2. <span data-ttu-id="4a392-173">En el *inicio de sesión* página, en el **utilice otro servicio para iniciar sesión** sección, elija el **Facebook** icono.</span><span class="sxs-lookup"><span data-stu-id="4a392-173">On the *Login* page, in the **Use another service to log in** section, choose the **Facebook** icon.</span></span> 

    <span data-ttu-id="4a392-174">La página web redirige la solicitud a la página de inicio de sesión de Facebook.</span><span class="sxs-lookup"><span data-stu-id="4a392-174">The web page redirects the request to the Facebook login page.</span></span>

    ![OAuth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. <span data-ttu-id="4a392-176">Inicie sesión en una cuenta de Facebook.</span><span class="sxs-lookup"><span data-stu-id="4a392-176">Log into a Facebook account.</span></span> 

    <span data-ttu-id="4a392-177">El código usa el token de Facebook para la autenticación y, a continuación, vuelve a una página donde puede asociar el inicio de sesión de Facebook con inicio de sesión de su sitio.</span><span class="sxs-lookup"><span data-stu-id="4a392-177">The code uses the Facebook token to authenticate you and then returns to a page where you can associate your Facebook login with your site's login.</span></span> <span data-ttu-id="4a392-178">Su dirección de correo electrónico o nombre de usuario se rellena en el **correo electrónico** campo en el formulario.</span><span class="sxs-lookup"><span data-stu-id="4a392-178">Your user name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. <span data-ttu-id="4a392-180">Elija la **asociar** botón.</span><span class="sxs-lookup"><span data-stu-id="4a392-180">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="4a392-181">El explorador vuelve a la página principal y que haya iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="4a392-181">The browser returns to the home page and you are logged in.</span></span>

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a><span data-ttu-id="4a392-182">Habilitar los inicios de sesión de Twitter</span><span class="sxs-lookup"><span data-stu-id="4a392-182">Enabling Twitter Logins</span></span>

1. <span data-ttu-id="4a392-183">Vaya a la [sitio de desarrolladores de Twitter](https://dev.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="4a392-183">Browse to the [Twitter developers site](https://dev.twitter.com/).</span></span>
2. <span data-ttu-id="4a392-184">Elija la **crear una aplicación** vincular y, a continuación, inicie sesión en el sitio.</span><span class="sxs-lookup"><span data-stu-id="4a392-184">Choose the **Create an App** link and then log into the site.</span></span>
3. <span data-ttu-id="4a392-185">En el **crear una aplicación** forman, rellene el **nombre** y **descripción** campos.</span><span class="sxs-lookup"><span data-stu-id="4a392-185">On the **Create an Application** form, fill in the **Name** and **Description** fields.</span></span>
4. <span data-ttu-id="4a392-186">En el **sitio Web** , escriba la dirección URL del sitio (por ejemplo, `http://www.example.com`).</span><span class="sxs-lookup"><span data-stu-id="4a392-186">In the **WebSite** field, enter the URL of your site (for example, `http://www.example.com`).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="4a392-187">Si va a probar su sitio local (mediante una dirección URL como `http://localhost:12345`), Twitter no puede aceptar la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="4a392-187">If you're testing your site locally (using a URL like `http://localhost:12345`), Twitter might not accept the URL.</span></span> <span data-ttu-id="4a392-188">Sin embargo, es posible que pueda usar la dirección IP de bucle invertido local (por ejemplo `http://127.0.0.1:12345`).</span><span class="sxs-lookup"><span data-stu-id="4a392-188">However, you might be able to use the local loopback IP address (for example `http://127.0.0.1:12345`).</span></span> <span data-ttu-id="4a392-189">Esto simplifica el proceso de probar la aplicación localmente.</span><span class="sxs-lookup"><span data-stu-id="4a392-189">This simplifies the process of testing your application locally.</span></span> <span data-ttu-id="4a392-190">Sin embargo, cada vez que cambia el número de puerto del sitio local, deberá actualizar el **sitio Web** campo de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4a392-190">However, every time the port number of your local site changes, you'll need to update the **WebSite** field of your application.</span></span>
5. <span data-ttu-id="4a392-191">En el **dirección URL de devolución de llamada** , escriba una dirección URL de la página en su sitio Web que desea que los usuarios vuelvan a tras iniciar sesión en Twitter.</span><span class="sxs-lookup"><span data-stu-id="4a392-191">In the **Callback URL** field, enter a URL for the page in your website that you want users to return to after logging into Twitter.</span></span> <span data-ttu-id="4a392-192">Por ejemplo, para enviar a los usuarios a la página principal del sitio de inicio (que reconozca su estado ha iniciado sesión), escriba la misma dirección URL que escribió en el **sitio Web** campo.</span><span class="sxs-lookup"><span data-stu-id="4a392-192">For example, to send users to the home page of the Starter Site (which will recognize their logged-in status), enter the same URL that you entered in the **WebSite** field.</span></span>
6. <span data-ttu-id="4a392-193">Acepte los términos y elija el **crear aplicación de Twitter** botón.</span><span class="sxs-lookup"><span data-stu-id="4a392-193">Accept the terms and choose the **Create your Twitter application** button.</span></span>
7. <span data-ttu-id="4a392-194">En el **My Applications** aterrizaje de la página, elija la aplicación que creó.</span><span class="sxs-lookup"><span data-stu-id="4a392-194">On the **My Applications** landing page, choose the application you created.</span></span>
8. <span data-ttu-id="4a392-195">En el **detalles** pestaña, desplácese hacia abajo y elija el **crear mi Token de acceso** botón.</span><span class="sxs-lookup"><span data-stu-id="4a392-195">On the **Details** tab, scroll to the bottom and choose the **Create My Access Token** button.</span></span>
9. <span data-ttu-id="4a392-196">En el **detalles** pestaña, copie el **clave de consumidor** y **secreto de consumidor** valores para la aplicación y péguelos en un archivo de texto temporal.</span><span class="sxs-lookup"><span data-stu-id="4a392-196">On the **Details** tab, copy the **Consumer Key** and **Consumer Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="4a392-197">Pasará estos valores para el proveedor de Twitter en el código de sitio Web.</span><span class="sxs-lookup"><span data-stu-id="4a392-197">You'll pass these values to the Twitter provider in your website code.</span></span>
10. <span data-ttu-id="4a392-198">Salga del sitio de Twitter.</span><span class="sxs-lookup"><span data-stu-id="4a392-198">Exit the Twitter site.</span></span>

<span data-ttu-id="4a392-199">Ahora realizar cambios en dos páginas en su sitio Web para que los usuarios podrán iniciar sesión en el sitio con sus cuentas de Twitter.</span><span class="sxs-lookup"><span data-stu-id="4a392-199">Now you make changes to two pages in your website so that users will be able to log into the site using their Twitter accounts.</span></span>

1. <span data-ttu-id="4a392-200">Cree o abra un sitio de ASP.NET Web Pages que se basa en la plantilla de sitio de inicio de WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="4a392-200">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="4a392-201">Abra el  *\_AppStart.cshtml* página y quite los comentarios del código para el proveedor de OAuth de Twitter.</span><span class="sxs-lookup"><span data-stu-id="4a392-201">Open the *\_AppStart.cshtml* page and uncomment the code for the Twitter OAuth provider.</span></span> <span data-ttu-id="4a392-202">El bloque de marca de comentario de código tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="4a392-202">The uncommented code block looks like this:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. <span data-ttu-id="4a392-203">Copia el **clave de consumidor** valor de la aplicación de Twitter como el valor de la `consumerKey` parámetro (dentro de las comillas).</span><span class="sxs-lookup"><span data-stu-id="4a392-203">Copy the **Consumer Key** value from the Twitter application as the value of the `consumerKey` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="4a392-204">Copia el **secreto de consumidor** valor de la aplicación de Twitter como el valor de la `consumerSecret` parámetro.</span><span class="sxs-lookup"><span data-stu-id="4a392-204">Copy the **Consumer Secret** value from the Twitter application as the value of the `consumerSecret` parameter.</span></span>
5. <span data-ttu-id="4a392-205">Guarde y cierre el archivo.</span><span class="sxs-lookup"><span data-stu-id="4a392-205">Save and close the file.</span></span>

### <a name="testing-twitter-login"></a><span data-ttu-id="4a392-206">Probar el inicio de sesión de Twitter</span><span class="sxs-lookup"><span data-stu-id="4a392-206">Testing Twitter login</span></span>

1. <span data-ttu-id="4a392-207">Ejecute el *default.cshtml* página del sitio y elija el **inicio de sesión** botón.</span><span class="sxs-lookup"><span data-stu-id="4a392-207">Run the *default.cshtml* page of your site and choose the **Login** button.</span></span>
2. <span data-ttu-id="4a392-208">En el *inicio de sesión* página, en el **utilice otro servicio para iniciar sesión** sección, elija el **Twitter** icono.</span><span class="sxs-lookup"><span data-stu-id="4a392-208">On the *Login* page, in the **Use another service to log in** section, choose the **Twitter** icon.</span></span> 

    <span data-ttu-id="4a392-209">La página web redirige la solicitud a una página de inicio de sesión de Twitter para la aplicación que creó.</span><span class="sxs-lookup"><span data-stu-id="4a392-209">The web page redirects the request to a Twitter login page for the application you created.</span></span>

    ![OAuth 4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. <span data-ttu-id="4a392-211">Inicie sesión en una cuenta de Twitter.</span><span class="sxs-lookup"><span data-stu-id="4a392-211">Log into a Twitter account.</span></span>
4. <span data-ttu-id="4a392-212">El código usa el token de Twitter para autenticar al usuario y, a continuación, vuelve a una página donde se puede asociar el inicio de sesión con su cuenta de sitio Web.</span><span class="sxs-lookup"><span data-stu-id="4a392-212">The code uses the Twitter token to authenticate the user and then returns you to a page where you can associate your login with your website account.</span></span> <span data-ttu-id="4a392-213">Su dirección de correo electrónico o nombre se rellena en el **correo electrónico** campo en el formulario.</span><span class="sxs-lookup"><span data-stu-id="4a392-213">Your name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. <span data-ttu-id="4a392-215">Elija la **asociar** botón.</span><span class="sxs-lookup"><span data-stu-id="4a392-215">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="4a392-216">El explorador vuelve a la página principal y que haya iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="4a392-216">The browser returns to the home page and you are logged in.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="4a392-217">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="4a392-217">Additional Resources</span></span>


- [<span data-ttu-id="4a392-218">Personalizar el comportamiento de todo el sitio</span><span class="sxs-lookup"><span data-stu-id="4a392-218">Customizing Site-Wide Behavior</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="4a392-219">Agregar seguridad y pertenencia a un sitio ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="4a392-219">Adding Security and Membership to an ASP.NET Web Pages Site</span></span>](https://go.microsoft.com/fwlink/?LinkID=202904)
