---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Creación de MVC 5 App con Facebook, Twitter, LinkedIn y Google OAuth2 Sign-on (C#) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial muestra cómo crear una aplicación web de ASP.NET MVC 5 que permite a los usuarios inicien sesión con OAuth 2.0 con las credenciales de un authenti externo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2015
ms.topic: article
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 8a9528f76b0166175f950543b4b8a7250bdf5100
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388771"
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a><span data-ttu-id="6a494-103">Crear una aplicación ASP.NET MVC 5 con Facebook, Twitter, LinkedIn y Google OAuth2 Sign-on (C#)</span><span class="sxs-lookup"><span data-stu-id="6a494-103">Create an ASP.NET MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on (C#)</span></span>
====================
<span data-ttu-id="6a494-104">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="6a494-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="6a494-105">Este tutorial muestra cómo crear una aplicación web de ASP.NET MVC 5 que permite a los usuarios inicien sesión mediante [OAuth 2.0](http://oauth.net/2/) con las credenciales de un proveedor de autenticación externos, como Facebook, Twitter, LinkedIn, Microsoft o Google.</span><span class="sxs-lookup"><span data-stu-id="6a494-105">This tutorial shows you how to build an ASP.NET MVC 5 web application that enables users to log in using [OAuth 2.0](http://oauth.net/2/) with credentials from an external authentication provider, such as Facebook, Twitter, LinkedIn, Microsoft, or Google.</span></span> <span data-ttu-id="6a494-106">Por motivos de simplicidad, este tutorial se centra en trabajar con las credenciales de Facebook y Google.</span><span class="sxs-lookup"><span data-stu-id="6a494-106">For simplicity, this tutorial focuses on working with credentials from Facebook and Google.</span></span>
> 
> <span data-ttu-id="6a494-107">Habilitar estas credenciales en los sitios web proporciona una ventaja considerable porque millones de usuarios ya tienen cuentas con estos proveedores externos.</span><span class="sxs-lookup"><span data-stu-id="6a494-107">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="6a494-108">Estos usuarios pueden ser más propensos a suscribirse a su sitio si no tienen que crear y recordar un nuevo conjunto de credenciales.</span><span class="sxs-lookup"><span data-stu-id="6a494-108">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span>
> 
> <span data-ttu-id="6a494-109">Vea también [aplicación ASP.NET MVC 5 con SMS y correo electrónico de autenticación en dos fases](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="6a494-109">See also [ASP.NET MVC 5 app with SMS and email Two-Factor Authentication](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span></span>
> 
> <span data-ttu-id="6a494-110">El tutorial también muestra cómo agregar datos de perfil del usuario y cómo usar la API de pertenencia para agregar roles.</span><span class="sxs-lookup"><span data-stu-id="6a494-110">The tutorial also shows how to add profile data for the user, and how to use the Membership API to add roles.</span></span> <span data-ttu-id="6a494-111">En este tutorial se escribió por [Rick Anderson](https://blogs.msdn.com/rickAndy) (me siga en Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="6a494-111">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Please follow me on Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="start"></a>
## <a name="getting-started"></a><span data-ttu-id="6a494-112">Introducción</span><span class="sxs-lookup"><span data-stu-id="6a494-112">Getting Started</span></span>

<span data-ttu-id="6a494-113">Comience por instalar y ejecutar [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="6a494-113">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="6a494-114">Instalar Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o superior.</span><span class="sxs-lookup"><span data-stu-id="6a494-114">Install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span> <span data-ttu-id="6a494-115">Para obtener ayuda con Dropbox, GitHub, Linkedin, Instagram, búfer, Salesforce, vapor, Stack Exchange, Tripit, Twitch, Twitter, Yahoo! etc., consulte este [proyecto de ejemplo](https://github.com/matthewdunsdon/oauthforaspnet).</span><span class="sxs-lookup"><span data-stu-id="6a494-115">For help with Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo!, and more, see this [sample project](https://github.com/matthewdunsdon/oauthforaspnet).</span></span>

> [!NOTE]
> <span data-ttu-id="6a494-116">Debe instalar Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o posterior para usar Google OAuth 2 y depurar localmente sin recibir advertencias de SSL.</span><span class="sxs-lookup"><span data-stu-id="6a494-116">You must install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher to use Google OAuth 2 and to debug locally without SSL warnings.</span></span>


<span data-ttu-id="6a494-117">Haga clic en **nuevo proyecto** desde el **iniciar** página, o puede usar el menú y seleccione **archivo**y, a continuación, **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="6a494-117">Click **New Project** from the **Start** page, or you can use the menu and select **File**, and then **New Project**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a><span data-ttu-id="6a494-118">Crear su primera aplicación</span><span class="sxs-lookup"><span data-stu-id="6a494-118">Creating Your First Application</span></span>

<span data-ttu-id="6a494-119">Haga clic en **nuevo proyecto**, a continuación, seleccione **Visual C#** a la izquierda y, a continuación, **Web** y, a continuación, seleccione **aplicación Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="6a494-119">Click **New Project**, then select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="6a494-120">Nombre del proyecto "MvcAuth" y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="6a494-120">Name your project "MvcAuth" and then click **OK**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

<span data-ttu-id="6a494-121">En el **nuevo proyecto ASP.NET** cuadro de diálogo, haga clic en **MVC**.</span><span class="sxs-lookup"><span data-stu-id="6a494-121">In the **New ASP.NET Project** dialog, click **MVC**.</span></span> <span data-ttu-id="6a494-122">Si la autenticación no es **cuentas de usuario individuales**, haga clic en el **Cambiar autenticación** y seleccione **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="6a494-122">If the Authentication is not **Individual User Accounts**, click the **Change Authentication** button and select **Individual User Accounts**.</span></span> <span data-ttu-id="6a494-123">Comprobando **Host en la nube**, la aplicación será muy fácil hospedar en Azure.</span><span class="sxs-lookup"><span data-stu-id="6a494-123">By checking **Host in the cloud**, the app will be very easy to host in Azure.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

<span data-ttu-id="6a494-124">Si seleccionó **Host en la nube**, complete el cuadro de diálogo Configurar.</span><span class="sxs-lookup"><span data-stu-id="6a494-124">If you selected **Host in the cloud**, complete the configure dialog.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a><span data-ttu-id="6a494-125">Use NuGet para actualizar el middleware de OWIN más reciente</span><span class="sxs-lookup"><span data-stu-id="6a494-125">Use NuGet to update to the latest OWIN middleware</span></span>

<span data-ttu-id="6a494-126">Use el Administrador de paquetes de NuGet para actualizar el [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span><span class="sxs-lookup"><span data-stu-id="6a494-126">Use the NuGet package manager to update the [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span></span> <span data-ttu-id="6a494-127">Seleccione **actualizaciones** en el menú izquierdo.</span><span class="sxs-lookup"><span data-stu-id="6a494-127">Select **Updates** in the left menu.</span></span> <span data-ttu-id="6a494-128">Puede hacer clic en el **actualizar todo** botón, o bien puede buscar solo los paquetes OWIN (se muestra en la imagen siguiente):</span><span class="sxs-lookup"><span data-stu-id="6a494-128">You can click on the **Update All** button or you can search for only OWIN packages (shown in the next image):</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

<span data-ttu-id="6a494-129">En la imagen siguiente, se muestran solo los paquetes OWIN:</span><span class="sxs-lookup"><span data-stu-id="6a494-129">In the image below, only OWIN packages are shown:</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

<span data-ttu-id="6a494-130">Desde la consola de administrador de paquetes (PMC), puede escribir el `Update-Package` comando, que actualizará todos los paquetes.</span><span class="sxs-lookup"><span data-stu-id="6a494-130">From the Package Manager Console (PMC), you can enter the `Update-Package` command, which will update all packages.</span></span>

<span data-ttu-id="6a494-131">Presione **F5** o **CTRL+F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6a494-131">Press **F5** or **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="6a494-132">En la imagen siguiente, el número de puerto es 1234.</span><span class="sxs-lookup"><span data-stu-id="6a494-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="6a494-133">Al ejecutar la aplicación, verá un número de puerto diferente.</span><span class="sxs-lookup"><span data-stu-id="6a494-133">When you run the application, you'll see a different port number.</span></span>

<span data-ttu-id="6a494-134">Según el tamaño de la ventana del explorador, es posible que deba haga clic en el icono de navegación para ver el **inicio**, **sobre**, **póngase en contacto con**, **registrar**y **iniciarla** vínculos.</span><span class="sxs-lookup"><span data-stu-id="6a494-134">Depending on the size of your browser window, you might need to click the navigation icon to see the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a><span data-ttu-id="6a494-135">Configuración de SSL en el proyecto</span><span class="sxs-lookup"><span data-stu-id="6a494-135">Setting up SSL in the Project</span></span>

<span data-ttu-id="6a494-136">Para conectarse a los proveedores de autenticación, como Google y Facebook, deberá configurar IIS Express para usar SSL.</span><span class="sxs-lookup"><span data-stu-id="6a494-136">To connect to authentication providers like Google and Facebook, you will need to set up IIS-Express to use SSL.</span></span> <span data-ttu-id="6a494-137">Es importante mantener mediante SSL después de iniciar sesión y no cambiar a HTTP, la cookie de inicio de sesión es igual que el secreto como su nombre de usuario y contraseña y sin usar SSL, lo envía en texto sin cifrar a través de la conexión.</span><span class="sxs-lookup"><span data-stu-id="6a494-137">It's important to keep using SSL after login and not drop back to HTTP, your login cookie is just as secret as your username and password, and without using SSL you're sending it in clear-text across the wire.</span></span> <span data-ttu-id="6a494-138">Además, ya se ha tomado el tiempo para realizar el enlace y proteja el canal (que es la mayor parte de lo que hace que sea más lento que HTTP HTTPS) antes de ejecuta la canalización de MVC, por lo tanto volviendo a HTTP después de que ha iniciado sesión no realizar la solicitud actual o futuro solicitudes mucho más rápidas.</span><span class="sxs-lookup"><span data-stu-id="6a494-138">Besides, you've already taken the time to perform the handshake and secure the channel (which is the bulk of what makes HTTPS slower than HTTP) before the MVC pipeline is run, so redirecting back to HTTP after you're logged in won't make the current request or future requests much faster.</span></span>

1. <span data-ttu-id="6a494-139">En **el Explorador de soluciones**, haga clic en el **MvcAuth** proyecto.</span><span class="sxs-lookup"><span data-stu-id="6a494-139">In **Solution Explorer**, click the **MvcAuth** project.</span></span>
2. <span data-ttu-id="6a494-140">Presione la tecla F4 para mostrar las propiedades del proyecto.</span><span class="sxs-lookup"><span data-stu-id="6a494-140">Hit the F4 key to show the project properties.</span></span> <span data-ttu-id="6a494-141">Como alternativa, desde el **vista** menú puede seleccionar **ventana propiedades**.</span><span class="sxs-lookup"><span data-stu-id="6a494-141">Alternatively, from the **View** menu you can select **Properties Window**.</span></span>
3. <span data-ttu-id="6a494-142">Cambio **SSL habilitado** en True.</span><span class="sxs-lookup"><span data-stu-id="6a494-142">Change **SSL Enabled** to True.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. <span data-ttu-id="6a494-143">Copie la dirección URL de SSL (que será `https://localhost:44300/` a menos que haya creado a otros proyectos SSL).</span><span class="sxs-lookup"><span data-stu-id="6a494-143">Copy the SSL URL (which will be `https://localhost:44300/` unless you've created other SSL projects).</span></span>
5. <span data-ttu-id="6a494-144">En **el Explorador de soluciones**, haga clic en el **MvcAuth** del proyecto y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="6a494-144">In **Solution Explorer**, right click the **MvcAuth** project and select **Properties**.</span></span>
6. <span data-ttu-id="6a494-145">Seleccione el **Web** pestaña y, a continuación, pegue la dirección URL de SSL en el **dirección Url del proyecto** cuadro.</span><span class="sxs-lookup"><span data-stu-id="6a494-145">Select the **Web** tab, and then paste the SSL URL into the **Project Url** box.</span></span> <span data-ttu-id="6a494-146">Guarde el archivo (Ctl + S).</span><span class="sxs-lookup"><span data-stu-id="6a494-146">Save the file (Ctl+S).</span></span> <span data-ttu-id="6a494-147">Necesitará esta dirección URL para configurar aplicaciones de la autenticación de Facebook y Google.</span><span class="sxs-lookup"><span data-stu-id="6a494-147">You will need this URL to configure Facebook and Google authentication apps.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. <span data-ttu-id="6a494-148">Agregar el [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) atributo a la `Home` controlador requieren todas las solicitudes debe usar HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6a494-148">Add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attribute to the `Home` controller to require all requests must use HTTPS.</span></span> <span data-ttu-id="6a494-149">Un enfoque más seguro consiste en agregar el [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filtro a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6a494-149">A more secure approach is to add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter to the application.</span></span> <span data-ttu-id="6a494-150">Consulte la sección &quot;proteger la aplicación con SSL y el atributo Authorize&quot; en mi tutoral [crear una aplicación ASP.NET MVC con la autenticación y base de datos SQL e implementar en Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="6a494-150">See the section &quot;Protect the Application with SSL and the Authorize Attribute&quot; in my tutoral [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="6a494-151">A continuación se muestra una parte del controlador Home.</span><span class="sxs-lookup"><span data-stu-id="6a494-151">A portion of the Home controller is shown below.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. <span data-ttu-id="6a494-152">Presione CTRL+F5 para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6a494-152">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="6a494-153">Si ha instalado el certificado en el pasado, puede omitir el resto de esta sección y vaya a [creando una aplicación de Google para OAuth 2 y conectarse al proyecto de la aplicación](#goog), de lo contrario, siga las instrucciones para confiar en la firma automática certificado que ha generado IIS Express.</span><span class="sxs-lookup"><span data-stu-id="6a494-153">If you've installed the certificate in the past, you can skip the rest of this section and jump to [Creating a Google app for OAuth 2 and connecting the app to the project](#goog), otherwise, follow the instructions to trust the self-signed certificate that IIS Express has generated.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. <span data-ttu-id="6a494-154">Leer el **advertencia de seguridad** cuadro de diálogo y, a continuación, haga clic en **Sí** si desea instalar el certificado que representa el host local.</span><span class="sxs-lookup"><span data-stu-id="6a494-154">Read the **Security Warning** dialog and then click **Yes** if you want to install the certificate representing localhost.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. <span data-ttu-id="6a494-155">IE muestra la *inicio* página y no hay ninguna advertencia de SSL.</span><span class="sxs-lookup"><span data-stu-id="6a494-155">IE shows the *Home* page and there are no SSL warnings.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. <span data-ttu-id="6a494-156">Google Chrome también acepta el certificado y se mostrará el contenido HTTPS sin advertencia.</span><span class="sxs-lookup"><span data-stu-id="6a494-156">Google Chrome also accepts the certificate and will show HTTPS content without a warning.</span></span> <span data-ttu-id="6a494-157">Firefox usa su propio almacén de certificados, por lo que mostrará una advertencia.</span><span class="sxs-lookup"><span data-stu-id="6a494-157">Firefox uses its own certificate store, so it will display a warning.</span></span> <span data-ttu-id="6a494-158">Para nuestra aplicación de forma segura puede hacer clic **comprende los riesgos**.</span><span class="sxs-lookup"><span data-stu-id="6a494-158">For our application you can safely click **I Understand the Risks**.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a><span data-ttu-id="6a494-159">Creación de una aplicación de Google para OAuth 2 y conectarse al proyecto de la aplicación</span><span class="sxs-lookup"><span data-stu-id="6a494-159">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="6a494-160">Para obtener instrucciones de Google OAuth actuales, vea [Google de la configuración de autenticación en ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span><span class="sxs-lookup"><span data-stu-id="6a494-160">For current Google OAuth instructions, see [Configuring Google authentication in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span></span>

1. <span data-ttu-id="6a494-161">Navegue hasta la [Google Developers Console](https://console.developers.google.com/).</span><span class="sxs-lookup"><span data-stu-id="6a494-161">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span>
2. <span data-ttu-id="6a494-162">Si no ha creado un proyecto antes de, seleccione **credenciales** en la pestaña de la izquierda y, a continuación, seleccione **crear**.</span><span class="sxs-lookup"><span data-stu-id="6a494-162">If you haven't created a project before, select **Credentials** in the left tab, and then select **Create**.</span></span>
3. <span data-ttu-id="6a494-163">En el panel izquierdo, haga clic en **credenciales**.</span><span class="sxs-lookup"><span data-stu-id="6a494-163">In the left tab, click **Credentials**.</span></span>
4. <span data-ttu-id="6a494-164">Haga clic en **crear credenciales** , a continuación, **Id. de cliente de OAuth**.</span><span class="sxs-lookup"><span data-stu-id="6a494-164">Click **Create credentials** then **OAuth client ID**.</span></span> 

    1. <span data-ttu-id="6a494-165">En el **crear Id. de cliente** cuadro de diálogo, mantenga el valor predeterminado **aplicación Web** para el tipo de aplicación.</span><span class="sxs-lookup"><span data-stu-id="6a494-165">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
    2. <span data-ttu-id="6a494-166">Establecer el **autorizados de JavaScript** orígenes a la dirección URL de SSL que usó anteriormente (`https://localhost:44300/` a menos que haya creado a otros proyectos SSL)</span><span class="sxs-lookup"><span data-stu-id="6a494-166">Set the **Authorized JavaScript** origins to the SSL URL you used above (`https://localhost:44300/` unless you've created other SSL projects)</span></span>
    3. <span data-ttu-id="6a494-167">Establecer el **URI de redireccionamiento autorizado** para:</span><span class="sxs-lookup"><span data-stu-id="6a494-167">Set the **Authorized redirect URI** to:</span></span>  
         `https://localhost:44300/signin-google`
5. <span data-ttu-id="6a494-168">Haga clic en el elemento de menú de la pantalla de consentimiento de OAuth, Establece el nombre de producto y la dirección de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="6a494-168">Click the OAuth Consent screen menu item, then set your email address and product name.</span></span> <span data-ttu-id="6a494-169">Cuando haya completado el formulario, haga clic **guardar**.</span><span class="sxs-lookup"><span data-stu-id="6a494-169">When you have completed the form click **Save**.</span></span>
6. <span data-ttu-id="6a494-170">Haga clic en el elemento de menú de la biblioteca, buscar **Google + API**, haga clic en él, a continuación, presione Enable.</span><span class="sxs-lookup"><span data-stu-id="6a494-170">Click the Library menu item, search **Google+ API**, click on it then press Enable.</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   <span data-ttu-id="6a494-171">La siguiente imagen muestra la API habilitadas.</span><span class="sxs-lookup"><span data-stu-id="6a494-171">The image below shows the enabled APIs.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. <span data-ttu-id="6a494-172">Desde el Administrador de API de API de Google, visite la **credenciales** tab para obtener el **Id. de cliente**.</span><span class="sxs-lookup"><span data-stu-id="6a494-172">From the Google APIs API Manager, visit the **Credentials** tab to obtain the **Client ID**.</span></span> <span data-ttu-id="6a494-173">Descarga para guardar un archivo JSON con los secretos de aplicación.</span><span class="sxs-lookup"><span data-stu-id="6a494-173">Download to save a JSON file with application secrets.</span></span> <span data-ttu-id="6a494-174">Copie y pegue el **ClientId** y **ClientSecret** en el `UseGoogleAuthentication` método se encuentra en la *Startup.Auth.cs* de archivos en el *App_Start* carpeta.</span><span class="sxs-lookup"><span data-stu-id="6a494-174">Copy and paste the **ClientId** and **ClientSecret** into the `UseGoogleAuthentication` method found in the *Startup.Auth.cs* file in the *App_Start* folder.</span></span> <span data-ttu-id="6a494-175">El **ClientId** y **ClientSecret** son ejemplos de valores que se muestran a continuación y no funcionan.</span><span class="sxs-lookup"><span data-stu-id="6a494-175">The **ClientId** and **ClientSecret** values shown below are samples and don't work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > <span data-ttu-id="6a494-176">Seguridad: nunca almacene de datos confidenciales en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="6a494-176">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="6a494-177">La cuenta y las credenciales se agregan al código anterior para simplificar el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="6a494-177">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="6a494-178">Consulte [procedimientos recomendados para implementar contraseñas y otros datos confidenciales en ASP.NET y Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="6a494-178">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
8. <span data-ttu-id="6a494-179">Presione **CTRL+F5** para compilar y ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6a494-179">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="6a494-180">Haga clic en el **iniciarla** vínculo.</span><span class="sxs-lookup"><span data-stu-id="6a494-180">Click the **Log in** link.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. <span data-ttu-id="6a494-181">En **utilice otro servicio para iniciar sesión**, haga clic en **Google**.</span><span class="sxs-lookup"><span data-stu-id="6a494-181">Under **Use another service to log in**, click **Google**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > <span data-ttu-id="6a494-182">Si se salta cualquiera de los pasos anteriores obtendrá un error HTTP 401.</span><span class="sxs-lookup"><span data-stu-id="6a494-182">If you miss any of the steps above you will get a HTTP 401 error.</span></span> <span data-ttu-id="6a494-183">Revise los pasos anteriores.</span><span class="sxs-lookup"><span data-stu-id="6a494-183">Recheck your steps above.</span></span> <span data-ttu-id="6a494-184">Si se salta una configuración necesaria (por ejemplo **nombre de producto**), agregue el elemento que falta y guardar; puede tardar unos minutos para que funcione la autenticación.</span><span class="sxs-lookup"><span data-stu-id="6a494-184">If you miss a required setting (for example **product name**), add the missing item and save; it can take a few minutes for authentication to work.</span></span>
10. <span data-ttu-id="6a494-185">Se le redirigirá al sitio de Google donde especificará sus credenciales.</span><span class="sxs-lookup"><span data-stu-id="6a494-185">You will be redirected to the Google site where you will enter your credentials.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. <span data-ttu-id="6a494-186">Después de escribir sus credenciales, se le pedirá para conceder permisos a la aplicación web que acaba de crear:</span><span class="sxs-lookup"><span data-stu-id="6a494-186">After you enter your credentials, you will be prompted to give permissions to the web application you just created:</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. <span data-ttu-id="6a494-187">Haga clic en **acepte**.</span><span class="sxs-lookup"><span data-stu-id="6a494-187">Click **Accept**.</span></span> <span data-ttu-id="6a494-188">Se le redirigirá ahora a la **registrar** página de la aplicación MvcAuth donde puede registrar su cuenta de Google.</span><span class="sxs-lookup"><span data-stu-id="6a494-188">You will now be redirected back to the **Register** page of the MvcAuth application where you can register your Google account.</span></span> <span data-ttu-id="6a494-189">Tiene la opción de cambiar el nombre de registro de correo electrónico local utilizado para la cuenta de Gmail, pero generalmente deseará mantener el alias de correo electrónico predeterminada (es decir, lo que se usó para la autenticación).</span><span class="sxs-lookup"><span data-stu-id="6a494-189">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="6a494-190">Haga clic en **Registrarse**.</span><span class="sxs-lookup"><span data-stu-id="6a494-190">Click **Register**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a><span data-ttu-id="6a494-191">Creación de la aplicación de Facebook y conectarse al proyecto de la aplicación</span><span class="sxs-lookup"><span data-stu-id="6a494-191">Creating the app in Facebook and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="6a494-192">Para instrucciones actuales de autenticación de Facebook OAuth2, consulte [Facebook configurar autenticación](/aspnet/core/security/authentication/social/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="6a494-192">For current Facebook OAuth2 authentication instructions, see [Configuring Facebook authentication](/aspnet/core/security/authentication/social/facebook-logins)</span></span>

<span data-ttu-id="6a494-193">Para la autenticación de Facebook OAuth2, deberá copiar en el proyecto algunas opciones de configuración de una aplicación que cree en Facebook.</span><span class="sxs-lookup"><span data-stu-id="6a494-193">For Facebook OAuth2 authentication, you need to copy to your project some settings from an application that you create in Facebook.</span></span>

1. <span data-ttu-id="6a494-194">En el explorador, vaya a [ https://developers.facebook.com/apps ](https://developers.facebook.com/apps) e inicie sesión escribiendo sus credenciales de Facebook.</span><span class="sxs-lookup"><span data-stu-id="6a494-194">In your browser, navigate to [https://developers.facebook.com/apps](https://developers.facebook.com/apps) and log in by entering your Facebook credentials.</span></span>
2. <span data-ttu-id="6a494-195">Si no se ha registrado como un programador de Facebook, haga clic en **registrarse como desarrollador** y siga las instrucciones para registrar.</span><span class="sxs-lookup"><span data-stu-id="6a494-195">If you aren't already registered as a Facebook developer, click **Register as a Developer** and follow the directions to register.</span></span>
3. <span data-ttu-id="6a494-196">En el **aplicaciones** , haga clic **crear nueva aplicación**.</span><span class="sxs-lookup"><span data-stu-id="6a494-196">On the **Apps** tab, click **Create New App**.</span></span>

    ![Crear nueva aplicación](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. <span data-ttu-id="6a494-198">Escriba un **nombre de la aplicación** y **categoría**, a continuación, haga clic en **crear aplicación**.</span><span class="sxs-lookup"><span data-stu-id="6a494-198">Enter an **App Name** and **Category**, then click **Create App**.</span></span>

    <span data-ttu-id="6a494-199">El <strong>aplicación Namespace</strong> es la parte de la dirección URL que se usará para tener acceso a la aplicación de Facebook para la autenticación de la aplicación (por ejemplo, https\://apps.facebook.com/{App Namespace}).</span><span class="sxs-lookup"><span data-stu-id="6a494-199">The <strong>App Namespace</strong> is the part of the URL that your App will use to access the Facebook application for authentication (for example, https\://apps.facebook.com/{App Namespace}).</span></span> <span data-ttu-id="6a494-200">Si no especifica un <strong>aplicación Namespace</strong>, el <strong>Id. de aplicación</strong> se usará para la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="6a494-200">If you don't specify an <strong>App Namespace</strong>, the <strong>App ID</strong> will be used for the URL.</span></span> <span data-ttu-id="6a494-201">El <strong>Id. de aplicación</strong> es un número long generados por el sistema que verá en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="6a494-201">The <strong>App ID</strong> is a long system-generated number that you will see in the next step.</span></span>

    ![Crear el cuadro de diálogo nueva aplicación](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. <span data-ttu-id="6a494-203">Envíe la comprobación de seguridad estándar.</span><span class="sxs-lookup"><span data-stu-id="6a494-203">Submit the standard security check.</span></span>

    ![Comprobación de seguridad](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. <span data-ttu-id="6a494-205">Seleccione **configuración** de la barra de menú de la izquierda.![ Barra de menús del programador de Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="6a494-205">Select **Settings** for the left menu bar.![Facebook Developer's menu bar](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span></span>
7. <span data-ttu-id="6a494-206">En el **básica** sección de configuración de la página Seleccionar **Agregar plataforma** para especificar que va a agregar una aplicación del sitio Web.</span><span class="sxs-lookup"><span data-stu-id="6a494-206">On the **Basic** settings section of the page select **Add Platform** to specify that you are adding a website application.</span></span> <span data-ttu-id="6a494-207">![Configuración básica](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span><span class="sxs-lookup"><span data-stu-id="6a494-207">![Basic Settings](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span></span>
8. <span data-ttu-id="6a494-208">Seleccione **sitio Web** entre las opciones de plataforma.</span><span class="sxs-lookup"><span data-stu-id="6a494-208">Select **Website** from the platform choices.</span></span>  
  
    ![Opciones de plataforma](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. <span data-ttu-id="6a494-210">Tome nota de su **Id. de aplicación** y su **secreto de la aplicación** para que ambos en la aplicación MVC se agregan más adelante en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="6a494-210">Make a note of your **App ID** and your **App Secret** so that you can add both into your MVC application later in this tutorial.</span></span> <span data-ttu-id="6a494-211">Además, agregue la dirección URL del sitio (`https://localhost:44300/`) para probar la aplicación MVC.</span><span class="sxs-lookup"><span data-stu-id="6a494-211">Also, Add your Site URL (`https://localhost:44300/`) to test your MVC application.</span></span> <span data-ttu-id="6a494-212">Además, agregue un **correo electrónico de contacto**.</span><span class="sxs-lookup"><span data-stu-id="6a494-212">Also, add a **Contact Email**.</span></span> <span data-ttu-id="6a494-213">A continuación, seleccione **guardar cambios**.</span><span class="sxs-lookup"><span data-stu-id="6a494-213">Then, select **Save Changes**.</span></span>   

    ![Página de detalles de la aplicación básica](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > <span data-ttu-id="6a494-215">Tenga en cuenta que solo podrá autenticarse con el alias de correo electrónico que se ha registrado.</span><span class="sxs-lookup"><span data-stu-id="6a494-215">Note that you will only be able to authenticate using the email alias you have registered.</span></span> <span data-ttu-id="6a494-216">Otros usuarios y cuentas de prueba no será capaz de registrar.</span><span class="sxs-lookup"><span data-stu-id="6a494-216">Other users and test accounts will not be able to register.</span></span> <span data-ttu-id="6a494-217">Puede conceder acceso de otras cuentas de Facebook a la aplicación de Facebook **Developer Roles** ficha.</span><span class="sxs-lookup"><span data-stu-id="6a494-217">You can grant other Facebook accounts access to the application on the Facebook **Developer Roles** tab.</span></span>
10. <span data-ttu-id="6a494-218">En Visual Studio, abra *aplicación\_Start\Startup.Auth.cs*.</span><span class="sxs-lookup"><span data-stu-id="6a494-218">In Visual Studio, open *App\_Start\Startup.Auth.cs*.</span></span>
11. <span data-ttu-id="6a494-219">Copie y pegue el **AppId** y **secreto de la aplicación** en el `UseFacebookAuthentication` método.</span><span class="sxs-lookup"><span data-stu-id="6a494-219">Copy and paste the **AppId** and **App Secret** into the `UseFacebookAuthentication` method.</span></span> <span data-ttu-id="6a494-220">El **AppId** y **secreto de la aplicación** son ejemplos de valores que se muestran a continuación y no funcionará.</span><span class="sxs-lookup"><span data-stu-id="6a494-220">The **AppId** and **App Secret** values shown below are samples and will not work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. <span data-ttu-id="6a494-221">Haga clic en **guardar cambios**.</span><span class="sxs-lookup"><span data-stu-id="6a494-221">Click **Save Changes**.</span></span>
13. <span data-ttu-id="6a494-222">Presione **CTRL+F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6a494-222">Press **CTRL+F5** to run the application.</span></span>


<span data-ttu-id="6a494-223">Seleccione **iniciarla** para mostrar la página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="6a494-223">Select **Log in** to display the Login page.</span></span> <span data-ttu-id="6a494-224">Haga clic en **Facebook** en **utilice otro servicio para iniciar sesión.**</span><span class="sxs-lookup"><span data-stu-id="6a494-224">Click **Facebook** under **Use another service to log in.**</span></span>

<span data-ttu-id="6a494-225">Escriba sus credenciales de Facebook.</span><span class="sxs-lookup"><span data-stu-id="6a494-225">Enter your Facebook credentials.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

<span data-ttu-id="6a494-226">Se le pedirá que conceda permiso para la aplicación tener acceso a su perfil público y una lista de amigos.</span><span class="sxs-lookup"><span data-stu-id="6a494-226">You will be prompted to grant permission for the application to access your public profile and friend list.</span></span>

![Detalles de la aplicación de Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

<span data-ttu-id="6a494-228">Ahora se registran en.</span><span class="sxs-lookup"><span data-stu-id="6a494-228">You are now logged in.</span></span> <span data-ttu-id="6a494-229">Ahora puede registrar esta cuenta con la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6a494-229">You can now register this account with the application.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

<span data-ttu-id="6a494-230">Al registrar, se agrega una entrada para el *usuarios* tabla de la base de datos de pertenencia.</span><span class="sxs-lookup"><span data-stu-id="6a494-230">When you register, an entry is added to the *Users* table of the membership database.</span></span>

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a><span data-ttu-id="6a494-231">Examinar los datos de pertenencia</span><span class="sxs-lookup"><span data-stu-id="6a494-231">Examine the Membership Data</span></span>

<span data-ttu-id="6a494-232">En el **vista** menú, haga clic en **Explorador de servidores**.</span><span class="sxs-lookup"><span data-stu-id="6a494-232">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

<span data-ttu-id="6a494-233">Expanda **DefaultConnection (MvcAuth)**, expanda **tablas**, haga clic en **AspNetUsers** y haga clic en **mostrar datos de tabla**.</span><span class="sxs-lookup"><span data-stu-id="6a494-233">Expand **DefaultConnection (MvcAuth)**, expand **Tables**, right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![datos de la tabla aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a><span data-ttu-id="6a494-235">Agregar datos de perfil para la clase de usuario</span><span class="sxs-lookup"><span data-stu-id="6a494-235">Adding Profile Data to the User Class</span></span>

<span data-ttu-id="6a494-236">En esta sección agregará la fecha de nacimiento y ciudad natal a los datos de usuario durante el registro, como se muestra en la siguiente imagen.</span><span class="sxs-lookup"><span data-stu-id="6a494-236">In this section you'll add birth date and home town to the user data during registration, as shown in the following image.</span></span>

![reg con ciudad natal y Cump.](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

<span data-ttu-id="6a494-238">Abra el *Models\IdentityModels.cs* archivo y agregue las propiedades de ciudad principal y de fecha de nacimiento:</span><span class="sxs-lookup"><span data-stu-id="6a494-238">Open the *Models\IdentityModels.cs* file and add birth date and home town properties:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

<span data-ttu-id="6a494-239">Abra el *Models\AccountViewModels.cs* archivo y el conjunto de propiedades de población de fecha y principal en nacimiento `ExternalLoginConfirmationViewModel`.</span><span class="sxs-lookup"><span data-stu-id="6a494-239">Open the *Models\AccountViewModels.cs* file and the set birth date and home town properties in `ExternalLoginConfirmationViewModel`.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

<span data-ttu-id="6a494-240">Abra el *controllers\accountcontroller* archivo y agregue código para la ciudad de inicio y de fecha de nacimiento en la `ExternalLoginConfirmation` método de acción como se muestra:</span><span class="sxs-lookup"><span data-stu-id="6a494-240">Open the *Controllers\AccountController.cs* file and add code for birth date and home town in the `ExternalLoginConfirmation` action method as shown:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

<span data-ttu-id="6a494-241">Agregar la fecha de nacimiento y ciudad natal a la *Views\Account\ExternalLoginConfirmation.cshtml* archivo:</span><span class="sxs-lookup"><span data-stu-id="6a494-241">Add birth date and home town to the *Views\Account\ExternalLoginConfirmation.cshtml* file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

<span data-ttu-id="6a494-242">Para que pueda volver a registrar su cuenta de Facebook con su aplicación y compruebe que puede agregar la nueva fecha de nacimiento y la información de perfil de ciudad natal, elimine la base de datos de pertenencia.</span><span class="sxs-lookup"><span data-stu-id="6a494-242">Delete the membership database so you can again register your Facebook account with your application and verify you can add the new birth date and home town profile information.</span></span>

<span data-ttu-id="6a494-243">Desde **el Explorador de soluciones**, haga clic en el **mostrar todos los archivos** icono y, a continuación, haga clic derecho *agregar\_Data\aspnet-MvcAuth -&lt;dateStamp&gt;.mdf* y haga clic en **eliminar**.</span><span class="sxs-lookup"><span data-stu-id="6a494-243">From **Solution Explorer**, click the **Show All Files** icon, then right click *Add\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* and click **Delete**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

<span data-ttu-id="6a494-244">Desde el **herramientas** menú, haga clic en **Administrador de paquetes de NuGet**, a continuación, haga clic en **Package Manager Console** (PMC).</span><span class="sxs-lookup"><span data-stu-id="6a494-244">From the **Tools** menu, click **NuGet Package Manger**, then click **Package Manager Console** (PMC).</span></span> <span data-ttu-id="6a494-245">Escriba los siguientes comandos en la PMC.</span><span class="sxs-lookup"><span data-stu-id="6a494-245">Enter the following commands in the PMC.</span></span>

1. <span data-ttu-id="6a494-246">Enable-Migrations</span><span class="sxs-lookup"><span data-stu-id="6a494-246">Enable-Migrations</span></span>
2. <span data-ttu-id="6a494-247">Add-Migration Init</span><span class="sxs-lookup"><span data-stu-id="6a494-247">Add-Migration Init</span></span>
3. <span data-ttu-id="6a494-248">Actualizar base de datos</span><span class="sxs-lookup"><span data-stu-id="6a494-248">Update-Database</span></span>

<span data-ttu-id="6a494-249">Ejecute la aplicación y usar FaceBook y Google para iniciar sesión y registrar algunos usuarios.</span><span class="sxs-lookup"><span data-stu-id="6a494-249">Run the application and use FaceBook and Google to log in and register some users.</span></span>

## <a name="examine-the-membership-data"></a><span data-ttu-id="6a494-250">Examinar los datos de pertenencia</span><span class="sxs-lookup"><span data-stu-id="6a494-250">Examine the Membership Data</span></span>

<span data-ttu-id="6a494-251">En el **vista** menú, haga clic en **Explorador de servidores**.</span><span class="sxs-lookup"><span data-stu-id="6a494-251">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

<span data-ttu-id="6a494-252">Haga clic en **AspNetUsers** y haga clic en **mostrar datos de tabla**.</span><span class="sxs-lookup"><span data-stu-id="6a494-252">Right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

<span data-ttu-id="6a494-253">El `HomeTown` y `BirthDate` a continuación se muestran los campos.</span><span class="sxs-lookup"><span data-stu-id="6a494-253">The `HomeTown` and `BirthDate` fields are shown below.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a><span data-ttu-id="6a494-254">Cerrando la aplicación e iniciar sesión con otra cuenta</span><span class="sxs-lookup"><span data-stu-id="6a494-254">Logging off your App and Logging in With Another Account</span></span>

<span data-ttu-id="6a494-255">Si iniciar sesión en su aplicación con Facebook y, a continuación, cerrar la sesión y vuelva a iniciar sesión nuevo con otra cuenta de Facebook (con el mismo explorador), se inmediatamente registrará a la cuenta de Facebook anterior que usa.</span><span class="sxs-lookup"><span data-stu-id="6a494-255">If you log on to your app with Facebook,, and then log out and try to log in again with a different Facebook account (using the same browser), you will be immediately logged in to the previous Facebook account you used.</span></span> <span data-ttu-id="6a494-256">Para usar otra cuenta, deberá navegar a Facebook y cierre de sesión en Facebook.</span><span class="sxs-lookup"><span data-stu-id="6a494-256">In order to use another account, you need to navigate to Facebook and log out at Facebook.</span></span> <span data-ttu-id="6a494-257">La misma regla se aplica a cualquier otro proveedor de autenticación parte 3ª.</span><span class="sxs-lookup"><span data-stu-id="6a494-257">The same rule applies to any other 3rd party authentication provider.</span></span> <span data-ttu-id="6a494-258">Como alternativa, puede iniciar sesión con otra cuenta mediante el uso de un explorador diferente.</span><span class="sxs-lookup"><span data-stu-id="6a494-258">Alternatively, you can log in with another account by using a different browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6a494-259">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="6a494-259">Next Steps</span></span>

<span data-ttu-id="6a494-260">Consulte [Introducción a los proveedores de seguridad de Yahoo y LinkedIn OAuth de OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) por Jerrie Pelser para obtener instrucciones de Yahoo y LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="6a494-260">See [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions.</span></span> <span data-ttu-id="6a494-261">Consulte del Jerrie bastante botones de inicio de sesión social para ASP.NET MVC 5 obtener los botones de inicio de sesión social enable.</span><span class="sxs-lookup"><span data-stu-id="6a494-261">See Jerrie's Pretty social login buttons for ASP.NET MVC 5 to get enable social login buttons.</span></span>

<span data-ttu-id="6a494-262">Siga el tutorial [crear una aplicación ASP.NET MVC con la autenticación y base de datos SQL e implementar en Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), que sigue este tutorial y se muestra lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="6a494-262">Follow my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), which continues this tutorial and shows the following:</span></span>

1. <span data-ttu-id="6a494-263">Cómo implementar la aplicación en Azure.</span><span class="sxs-lookup"><span data-stu-id="6a494-263">How to deploy your app to Azure.</span></span>
2. <span data-ttu-id="6a494-264">Cómo proteger las aplicaciones con roles.</span><span class="sxs-lookup"><span data-stu-id="6a494-264">How to secure you app with roles.</span></span>
3. <span data-ttu-id="6a494-265">Cómo proteger su aplicación con el [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) y [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtros.</span><span class="sxs-lookup"><span data-stu-id="6a494-265">How to secure your app with the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) and [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filters.</span></span>
4. <span data-ttu-id="6a494-266">Cómo usar la API de pertenencia para agregar los usuarios y roles.</span><span class="sxs-lookup"><span data-stu-id="6a494-266">How to use the membership API to add users and roles.</span></span>

<span data-ttu-id="6a494-267">Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar.</span><span class="sxs-lookup"><span data-stu-id="6a494-267">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="6a494-268">También puede solicitar nuevos temas en [mostrar Me cómo con código](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span><span class="sxs-lookup"><span data-stu-id="6a494-268">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span> <span data-ttu-id="6a494-269">Incluso puede solicitar y votar sobre nuevas características que se agregarán a ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6a494-269">You can even ask for and vote on new features to be added to ASP.NET.</span></span> <span data-ttu-id="6a494-270">Por ejemplo, puede votar por una herramienta para [crear y administrar usuarios y roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span><span class="sxs-lookup"><span data-stu-id="6a494-270">For example, you can vote for a tool to [create and manage users and roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span></span>

<span data-ttu-id="6a494-271">Para una buena explicación de cómo funcionan los servicios externos de autenticación de ASP.NET, vea de Robert McMurray [servicios de autenticación externos](https://asp.net/web-api/overview/security/external-authentication-services).</span><span class="sxs-lookup"><span data-stu-id="6a494-271">For an good explanation of how ASP.NET External Authentication Services work, see Robert McMurray's [External Authentication Services](https://asp.net/web-api/overview/security/external-authentication-services).</span></span> <span data-ttu-id="6a494-272">El artículo de Robert también entra en detalle en habilitar la autenticación de Microsoft y Twitter.</span><span class="sxs-lookup"><span data-stu-id="6a494-272">Robert's article also goes into detail in enabling Microsoft and Twitter authentication.</span></span> <span data-ttu-id="6a494-273">La excelente Tom Dykstra [tutorial de EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) muestra cómo trabajar con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6a494-273">Tom Dykstra's excellent [EF/MVC tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) shows how to work with the Entity Framework.</span></span>
