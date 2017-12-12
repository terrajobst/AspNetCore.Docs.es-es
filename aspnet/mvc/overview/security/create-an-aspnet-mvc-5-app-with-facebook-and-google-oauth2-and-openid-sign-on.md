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
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a><span data-ttu-id="b9488-103">Crear una aplicación de ASP.NET MVC 5 con Facebook, Twitter, LinkedIn y Google OAuth2 Sign-on (C#)</span><span class="sxs-lookup"><span data-stu-id="b9488-103">Create an ASP.NET MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on (C#)</span></span>
====================
<span data-ttu-id="b9488-104">Por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="b9488-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="b9488-105">Este tutorial muestra cómo crear una aplicación web de ASP.NET MVC 5 que permite a los usuarios iniciar sesión mediante [OAuth 2.0](http://oauth.net/2/) con credenciales de un proveedor de autenticación externo, como Facebook, Twitter, LinkedIn, Microsoft o Google.</span><span class="sxs-lookup"><span data-stu-id="b9488-105">This tutorial shows you how to build an ASP.NET MVC 5 web application that enables users to log in using [OAuth 2.0](http://oauth.net/2/) with credentials from an external authentication provider, such as Facebook, Twitter, LinkedIn, Microsoft, or Google.</span></span> <span data-ttu-id="b9488-106">Por simplicidad, este tutorial se centra en trabajar con las credenciales de Google y Facebook.</span><span class="sxs-lookup"><span data-stu-id="b9488-106">For simplicity, this tutorial focuses on working with credentials from Facebook and Google.</span></span>
> 
> <span data-ttu-id="b9488-107">Habilitar estas credenciales en los sitios web proporciona una ventaja importante porque millones de usuarios ya tienen cuentas con estos proveedores externos.</span><span class="sxs-lookup"><span data-stu-id="b9488-107">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="b9488-108">Estos usuarios pueden ser más dispuestos a registrarse para el sitio si no tienen que crear y recordar a un nuevo conjunto de credenciales.</span><span class="sxs-lookup"><span data-stu-id="b9488-108">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span>
> 
> <span data-ttu-id="b9488-109">Vea también [aplicación de ASP.NET MVC 5 con SMS y correo electrónico de autenticación en dos fases](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="b9488-109">See also [ASP.NET MVC 5 app with SMS and email Two-Factor Authentication](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span></span>
> 
> <span data-ttu-id="b9488-110">El tutorial también muestra cómo agregar datos de perfil para el usuario y cómo usar la API de pertenencia para agregar roles.</span><span class="sxs-lookup"><span data-stu-id="b9488-110">The tutorial also shows how to add profile data for the user, and how to use the Membership API to add roles.</span></span> <span data-ttu-id="b9488-111">Este tutorial se escribió por [Rick Anderson](https://blogs.msdn.com/rickAndy) (me siga en Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="b9488-111">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Please follow me on Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="start"></a>
## <a name="getting-started"></a><span data-ttu-id="b9488-112">Introducción</span><span class="sxs-lookup"><span data-stu-id="b9488-112">Getting Started</span></span>

<span data-ttu-id="b9488-113">Empiece por instalar y ejecutar [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="b9488-113">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="b9488-114">Instalar Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o superior.</span><span class="sxs-lookup"><span data-stu-id="b9488-114">Install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span> <span data-ttu-id="b9488-115">Para obtener ayuda con Dropbox, GitHub, Linkedin, Instagram, búfer, salesforce, secuencia, pila de Exchange, Tripit, twitch, Twitter, Yahoo y mucho más, consulte este [integral guía](http://www.oauthforaspnet.com/).</span><span class="sxs-lookup"><span data-stu-id="b9488-115">For help with Dropbox, GitHub, Linkedin, Instagram, buffer, salesforce, STEAM, Stack Exchange, Tripit, twitch, Twitter, Yahoo and more, see this [one stop guide](http://www.oauthforaspnet.com/).</span></span>

> [!NOTE]
> <span data-ttu-id="b9488-116">Debe instalar Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o superior que se va a usar Google OAuth 2 como depurar localmente sin advertencias de SSL.</span><span class="sxs-lookup"><span data-stu-id="b9488-116">You must install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher to use Google OAuth 2 and to debug locally without SSL warnings.</span></span>


<span data-ttu-id="b9488-117">Haga clic en **nuevo proyecto** desde el **iniciar** página, o bien puede usar el menú y seleccione **archivo**y, a continuación, **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="b9488-117">Click **New Project** from the **Start** page, or you can use the menu and select **File**, and then **New Project**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a><span data-ttu-id="b9488-118">Crear su primera aplicación</span><span class="sxs-lookup"><span data-stu-id="b9488-118">Creating Your First Application</span></span>

<span data-ttu-id="b9488-119">Haga clic en **nuevo proyecto**, a continuación, seleccione **Visual C#** a la izquierda, a continuación, **Web** y, a continuación, seleccione **aplicación Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="b9488-119">Click **New Project**, then select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="b9488-120">Denomine el proyecto "MvcAuth" y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="b9488-120">Name your project "MvcAuth" and then click **OK**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

<span data-ttu-id="b9488-121">En el **nuevo proyecto ASP.NET** cuadro de diálogo, haga clic en **MVC**.</span><span class="sxs-lookup"><span data-stu-id="b9488-121">In the **New ASP.NET Project** dialog, click **MVC**.</span></span> <span data-ttu-id="b9488-122">Si la autenticación no es **cuentas de usuario individuales**, haga clic en el **Cambiar autenticación** botón y seleccione **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="b9488-122">If the Authentication is not **Individual User Accounts**, click the **Change Authentication** button and select **Individual User Accounts**.</span></span> <span data-ttu-id="b9488-123">Comprobando **Host en la nube**, la aplicación le resultará muy fácil de hospedar en Azure.</span><span class="sxs-lookup"><span data-stu-id="b9488-123">By checking **Host in the cloud**, the app will be very easy to host in Azure.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

<span data-ttu-id="b9488-124">Si seleccionó **Host en la nube**, complete el cuadro de diálogo Configurar.</span><span class="sxs-lookup"><span data-stu-id="b9488-124">If you selected **Host in the cloud**, complete the configure dialog.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a><span data-ttu-id="b9488-125">Use NuGet para actualizar a la último middleware OWIN</span><span class="sxs-lookup"><span data-stu-id="b9488-125">Use NuGet to update to the latest OWIN middleware</span></span>

<span data-ttu-id="b9488-126">Usar el Administrador de paquetes de NuGet para actualizar la [middleware de OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span><span class="sxs-lookup"><span data-stu-id="b9488-126">Use the NuGet package manager to update the [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span></span> <span data-ttu-id="b9488-127">Seleccione **actualizaciones** en el menú izquierdo.</span><span class="sxs-lookup"><span data-stu-id="b9488-127">Select **Updates** in the left menu.</span></span> <span data-ttu-id="b9488-128">Puede hacer clic en el **actualizar todo** botón o se puede buscar solo los paquetes OWIN (que se muestra en la imagen siguiente):</span><span class="sxs-lookup"><span data-stu-id="b9488-128">You can click on the **Update All** button or you can search for only OWIN packages (shown in the next image):</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

<span data-ttu-id="b9488-129">En la imagen siguiente, se muestran solo los paquetes OWIN:</span><span class="sxs-lookup"><span data-stu-id="b9488-129">In the image below, only OWIN packages are shown:</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

<span data-ttu-id="b9488-130">Desde la consola de Manager de paquete (PMC), puede escribir el `Update-Package` comando, que se actualizará todos los paquetes.</span><span class="sxs-lookup"><span data-stu-id="b9488-130">From the Package Manager Console (PMC), you can enter the `Update-Package` command, which will update all packages.</span></span>

<span data-ttu-id="b9488-131">Presione **F5** o **CTRL+F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b9488-131">Press **F5** or **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="b9488-132">En la imagen siguiente, el número de puerto es 1234.</span><span class="sxs-lookup"><span data-stu-id="b9488-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="b9488-133">Al ejecutar la aplicación, verá un número de puerto diferente.</span><span class="sxs-lookup"><span data-stu-id="b9488-133">When you run the application, you'll see a different port number.</span></span>

<span data-ttu-id="b9488-134">Según el tamaño de la ventana del explorador, tendrá que hacer clic en el icono de navegación para ver la **inicio**, **sobre**, **póngase en contacto con**, **registrar**y **sesión** vínculos.</span><span class="sxs-lookup"><span data-stu-id="b9488-134">Depending on the size of your browser window, you might need to click the navigation icon to see the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a><span data-ttu-id="b9488-135">Cómo configurar SSL en el proyecto</span><span class="sxs-lookup"><span data-stu-id="b9488-135">Setting up SSL in the Project</span></span>

<span data-ttu-id="b9488-136">Para conectarse a proveedores de autenticación como Google y Facebook, debe configurar IIS Express para usar SSL.</span><span class="sxs-lookup"><span data-stu-id="b9488-136">To connect to authentication providers like Google and Facebook, you will need to set up IIS-Express to use SSL.</span></span> <span data-ttu-id="b9488-137">Es importante seguir usando SSL después de iniciar sesión y no a HTTP, coloque otra vez, la cookie de inicio de sesión es simplemente como secreto como el nombre de usuario y la contraseña y sin utilizar SSL lo envía en texto no cifrado a través de la conexión.</span><span class="sxs-lookup"><span data-stu-id="b9488-137">It's important to keep using SSL after login and not drop back to HTTP, your login cookie is just as secret as your username and password, and without using SSL you're sending it in clear-text across the wire.</span></span> <span data-ttu-id="b9488-138">Además, ya se ha tomado el tiempo para realizar el protocolo de enlace y proteja el canal (que es la mayor parte de lo que hace más lenta que HTTP HTTPS) antes de ejecuta la canalización de MVC, por lo que volviendo a HTTP después de que ha iniciado sesión no realizar la solicitud actual o futuro solicitudes mucho más rápidas.</span><span class="sxs-lookup"><span data-stu-id="b9488-138">Besides, you've already taken the time to perform the handshake and secure the channel (which is the bulk of what makes HTTPS slower than HTTP) before the MVC pipeline is run, so redirecting back to HTTP after you're logged in won't make the current request or future requests much faster.</span></span>

1. <span data-ttu-id="b9488-139">En **el Explorador de soluciones**, haga clic en el **MvcAuth** proyecto.</span><span class="sxs-lookup"><span data-stu-id="b9488-139">In **Solution Explorer**, click the **MvcAuth** project.</span></span>
2. <span data-ttu-id="b9488-140">Presionar la tecla F4 para mostrar las propiedades del proyecto.</span><span class="sxs-lookup"><span data-stu-id="b9488-140">Hit the F4 key to show the project properties.</span></span> <span data-ttu-id="b9488-141">Asimismo, desde el **vista** menú puede seleccionar **ventana propiedades**.</span><span class="sxs-lookup"><span data-stu-id="b9488-141">Alternatively, from the **View** menu you can select **Properties Window**.</span></span>
3. <span data-ttu-id="b9488-142">Cambio **se ha habilitado SSL** en True.</span><span class="sxs-lookup"><span data-stu-id="b9488-142">Change **SSL Enabled** to True.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. <span data-ttu-id="b9488-143">Copie la dirección URL de SSL (que será `https://localhost:44300/` a menos que haya creado otros proyectos SSL).</span><span class="sxs-lookup"><span data-stu-id="b9488-143">Copy the SSL URL (which will be `https://localhost:44300/` unless you've created other SSL projects).</span></span>
5. <span data-ttu-id="b9488-144">En **el Explorador de soluciones**, haga clic con el **MvcAuth** de proyecto y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="b9488-144">In **Solution Explorer**, right click the **MvcAuth** project and select **Properties**.</span></span>
6. <span data-ttu-id="b9488-145">Seleccione el **Web** ficha y, a continuación, pegue la dirección URL de SSL en el **dirección Url del proyecto** cuadro.</span><span class="sxs-lookup"><span data-stu-id="b9488-145">Select the **Web** tab, and then paste the SSL URL into the **Project Url** box.</span></span> <span data-ttu-id="b9488-146">Guarde el archivo (Ctl + S).</span><span class="sxs-lookup"><span data-stu-id="b9488-146">Save the file (Ctl+S).</span></span> <span data-ttu-id="b9488-147">Necesitará esta dirección URL para configurar aplicaciones de la autenticación de Google y Facebook.</span><span class="sxs-lookup"><span data-stu-id="b9488-147">You will need this URL to configure Facebook and Google authentication apps.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. <span data-ttu-id="b9488-148">Agregar el [RequireHttps](https://msdn.microsoft.com/en-us/library/system.web.mvc.requirehttpsattribute.aspx) atribuir a la `Home` controlador para requerir todas las solicitudes debe usar HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b9488-148">Add the [RequireHttps](https://msdn.microsoft.com/en-us/library/system.web.mvc.requirehttpsattribute.aspx) attribute to the `Home` controller to require all requests must use HTTPS.</span></span> <span data-ttu-id="b9488-149">Un enfoque más seguro consiste en agregar el [RequireHttps](https://msdn.microsoft.com/en-us/library/system.web.mvc.requirehttpsattribute.aspx) filtro a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b9488-149">A more secure approach is to add the [RequireHttps](https://msdn.microsoft.com/en-us/library/system.web.mvc.requirehttpsattribute.aspx) filter to the application.</span></span> <span data-ttu-id="b9488-150">Vea la sección &quot;proteger la aplicación con SSL y el atributo autorizar&quot; en mi tutoral [crear una aplicación de MVC de ASP.NET con la autenticación y la base de datos SQL e implementar al servicio de aplicaciones de Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="b9488-150">See the section &quot;Protect the Application with SSL and the Authorize Attribute&quot; in my tutoral [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="b9488-151">A continuación se muestra una parte del controlador Home.</span><span class="sxs-lookup"><span data-stu-id="b9488-151">A portion of the Home controller is shown below.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. <span data-ttu-id="b9488-152">Presione CTRL+F5 para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b9488-152">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="b9488-153">Si ha instalado el certificado en el pasado, puede omitir el resto de esta sección y saltar a [crear una aplicación de Google para OAuth 2 y conectar la aplicación al proyecto](#goog), en caso contrario, siga las instrucciones para confiar en autofirmado certificado que IIS Express ha generado.</span><span class="sxs-lookup"><span data-stu-id="b9488-153">If you've installed the certificate in the past, you can skip the rest of this section and jump to [Creating a Google app for OAuth 2 and connecting the app to the project](#goog), otherwise, follow the instructions to trust the self-signed certificate that IIS Express has generated.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. <span data-ttu-id="b9488-154">Leer la **advertencia de seguridad** cuadro de diálogo y, a continuación, haga clic en **Sí** si desea instalar el certificado que representa el host local.</span><span class="sxs-lookup"><span data-stu-id="b9488-154">Read the **Security Warning** dialog and then click **Yes** if you want to install the certificate representing localhost.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. <span data-ttu-id="b9488-155">Internet Explorer muestra la *inicio* página y no hay ninguna advertencia de SSL.</span><span class="sxs-lookup"><span data-stu-id="b9488-155">IE shows the *Home* page and there are no SSL warnings.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. <span data-ttu-id="b9488-156">Google Chrome también acepta el certificado y se mostrará el contenido HTTPS sin una advertencia.</span><span class="sxs-lookup"><span data-stu-id="b9488-156">Google Chrome also accepts the certificate and will show HTTPS content without a warning.</span></span> <span data-ttu-id="b9488-157">Firefox usa su propio almacén de certificados, por lo que mostrará una advertencia.</span><span class="sxs-lookup"><span data-stu-id="b9488-157">Firefox uses its own certificate store, so it will display a warning.</span></span> <span data-ttu-id="b9488-158">Para nuestra aplicación puede hacer clic en **entiende los riesgos**.</span><span class="sxs-lookup"><span data-stu-id="b9488-158">For our application you can safely click **I Understand the Risks**.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a><span data-ttu-id="b9488-159">Crear una aplicación de Google para OAuth 2 y conectar la aplicación al proyecto</span><span class="sxs-lookup"><span data-stu-id="b9488-159">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>

1. <span data-ttu-id="b9488-160">Navegue hasta la [Google Developers Console](https://console.developers.google.com/).</span><span class="sxs-lookup"><span data-stu-id="b9488-160">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span>
1. <span data-ttu-id="b9488-161">Si no ha creado un proyecto antes de, seleccione **credenciales** en la pestaña de la izquierda y, a continuación, seleccione **crear**.</span><span class="sxs-lookup"><span data-stu-id="b9488-161">If you haven't created a project before, select **Credentials** in the left tab, and then select **Create**.</span></span>
1. <span data-ttu-id="b9488-162">En la pestaña de la izquierda, haga clic en **credenciales**.</span><span class="sxs-lookup"><span data-stu-id="b9488-162">In the left tab, click **Credentials**.</span></span>
1. <span data-ttu-id="b9488-163">Haga clic en **crear credenciales** , a continuación, **identificador de cliente OAuth**.</span><span class="sxs-lookup"><span data-stu-id="b9488-163">Click **Create credentials** then **OAuth client ID**.</span></span> 

    1. <span data-ttu-id="b9488-164">En el **crear ID de cliente** cuadro de diálogo, mantenga el valor predeterminado **aplicación Web** para el tipo de aplicación.</span><span class="sxs-lookup"><span data-stu-id="b9488-164">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
    2. <span data-ttu-id="b9488-165">Establecer el **autorizado JavaScript** orígenes a la dirección URL de SSL que usó anteriormente (`https://localhost:44300/` a menos que haya creado otros proyectos SSL)</span><span class="sxs-lookup"><span data-stu-id="b9488-165">Set the **Authorized JavaScript** origins to the SSL URL you used above (`https://localhost:44300/` unless you've created other SSL projects)</span></span>
    3. <span data-ttu-id="b9488-166">Establecer el **URI de redireccionamiento autorizados** para:</span><span class="sxs-lookup"><span data-stu-id="b9488-166">Set the **Authorized redirect URI** to:</span></span>  
         `https://localhost:44300/signin-google`
5. <span data-ttu-id="b9488-167">Haga clic en el elemento de menú de la pantalla de consentimiento de OAuth y, a continuación, configure su nombre de producto y de dirección de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="b9488-167">Click the OAuth Consent screen menu item, then set your email address and product name.</span></span> <span data-ttu-id="b9488-168">Cuando haya completado el formulario, haga clic en **guardar**.</span><span class="sxs-lookup"><span data-stu-id="b9488-168">When you have completed the form click **Save**.</span></span>
6. <span data-ttu-id="b9488-169">Haga clic en el elemento de menú de la biblioteca, buscar **API de Google +**, haga clic en él, a continuación, presione Enable.</span><span class="sxs-lookup"><span data-stu-id="b9488-169">Click the Library menu item, search **Google+ API**, click on it then press Enable.</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
 <span data-ttu-id="b9488-170">La imagen siguiente muestra las API habilitadas.</span><span class="sxs-lookup"><span data-stu-id="b9488-170">The image below shows the enabled APIs.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. <span data-ttu-id="b9488-171">Desde el Administrador de API de API de Google, visite la **credenciales** tab para obtener la **Id. de cliente**.</span><span class="sxs-lookup"><span data-stu-id="b9488-171">From the Google APIs API Manager, visit the **Credentials** tab to obtain the **Client ID**.</span></span> <span data-ttu-id="b9488-172">Descarga para guardar un archivo JSON con secretos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b9488-172">Download to save a JSON file with application secrets.</span></span> <span data-ttu-id="b9488-173">Copie y pegue el **ClientId** y **ClientSecret** en el `UseGoogleAuthentication` método se encuentra en la *Startup.Auth.cs* un archivo en el *App_Start* carpeta.</span><span class="sxs-lookup"><span data-stu-id="b9488-173">Copy and paste the **ClientId** and **ClientSecret** into the `UseGoogleAuthentication` method found in the *Startup.Auth.cs* file in the *App_Start* folder.</span></span> <span data-ttu-id="b9488-174">El **ClientId** y **ClientSecret** son ejemplos de valores que se muestran a continuación y no funcionan.</span><span class="sxs-lookup"><span data-stu-id="b9488-174">The **ClientId** and **ClientSecret** values shown below are samples and don't work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > <span data-ttu-id="b9488-175">Seguridad - nunca almacenar los datos confidenciales en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="b9488-175">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="b9488-176">La cuenta y las credenciales se agregan al código anterior para simplificar el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b9488-176">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="b9488-177">Vea [las prácticas recomendadas para implementar las contraseñas y otros datos confidenciales en ASP.NET y el servicio de aplicación de Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="b9488-177">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
8. <span data-ttu-id="b9488-178">Presione **CTRL+F5** para compilar y ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b9488-178">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="b9488-179">Haga clic en el **sesión** vínculo.</span><span class="sxs-lookup"><span data-stu-id="b9488-179">Click the **Log in** link.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. <span data-ttu-id="b9488-180">En **utilice otro servicio para iniciar sesión**, haga clic en **Google**.</span><span class="sxs-lookup"><span data-stu-id="b9488-180">Under **Use another service to log in**, click **Google**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > <span data-ttu-id="b9488-181">Si se salta cualquiera de los pasos anteriores le devolverá un error HTTP 401.</span><span class="sxs-lookup"><span data-stu-id="b9488-181">If you miss any of the steps above you will get a HTTP 401 error.</span></span> <span data-ttu-id="b9488-182">Volver a comprobar los pasos anteriores.</span><span class="sxs-lookup"><span data-stu-id="b9488-182">Recheck your steps above.</span></span> <span data-ttu-id="b9488-183">Si se salta un valor obligatorio (por ejemplo **nombre de producto**), agregue la falta de artículos y guardar, puede tardar unos minutos para que funcione la autenticación.</span><span class="sxs-lookup"><span data-stu-id="b9488-183">If you miss a required setting (for example **product name**), add the missing item and save, it can take a few minutes for authentication to work.</span></span>
10. <span data-ttu-id="b9488-184">Se le redirigirá al sitio de google que deberá especificar sus credenciales.</span><span class="sxs-lookup"><span data-stu-id="b9488-184">You will be redirected to the google site where you will enter your credentials.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. <span data-ttu-id="b9488-185">Después de escribir sus credenciales, se le pedirá que le asigne permisos a la aplicación web que acaba de crear:</span><span class="sxs-lookup"><span data-stu-id="b9488-185">After you enter your credentials, you will be prompted to give permissions to the web application you just created:</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. <span data-ttu-id="b9488-186">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="b9488-186">Click **Accept**.</span></span> <span data-ttu-id="b9488-187">Ahora redirigirá a la **registrar** página de la aplicación MvcAuth donde puede registrar su cuenta de Google.</span><span class="sxs-lookup"><span data-stu-id="b9488-187">You will now be redirected back to the **Register** page of the MvcAuth application where you can register your Google account.</span></span> <span data-ttu-id="b9488-188">Tiene la opción de cambiar el nombre de registro de correo electrónico local utilizado para la cuenta de Gmail, pero generalmente desean mantener el alias de correo electrónico de manera predeterminada (es decir, la compañía utilizado para la autenticación).</span><span class="sxs-lookup"><span data-stu-id="b9488-188">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="b9488-189">Haga clic en **Registrarse**.</span><span class="sxs-lookup"><span data-stu-id="b9488-189">Click **Register**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a><span data-ttu-id="b9488-190">Crear la aplicación de Facebook y conectar la aplicación al proyecto</span><span class="sxs-lookup"><span data-stu-id="b9488-190">Creating the app in Facebook and connecting the app to the project</span></span>

<span data-ttu-id="b9488-191">Para la autenticación de Facebook OAuth2, debe copiar en el proyecto algunas opciones de configuración de una aplicación que cree en Facebook.</span><span class="sxs-lookup"><span data-stu-id="b9488-191">For Facebook OAuth2 authentication, you need to copy to your project some settings from an application that you create in Facebook.</span></span>

1. <span data-ttu-id="b9488-192">En el explorador, vaya a [https://developers.facebook.com/apps](https://developers.facebook.com/apps) e inicie sesión, escriba sus credenciales de Facebook.</span><span class="sxs-lookup"><span data-stu-id="b9488-192">In your browser, navigate to [https://developers.facebook.com/apps](https://developers.facebook.com/apps) and log in by entering your Facebook credentials.</span></span>
2. <span data-ttu-id="b9488-193">Si ya no se registra como un programador de Facebook, haga clic en **registrar como desarrollador** y siga las instrucciones para registrar.</span><span class="sxs-lookup"><span data-stu-id="b9488-193">If you aren't already registered as a Facebook developer, click **Register as a Developer** and follow the directions to register.</span></span>
3. <span data-ttu-id="b9488-194">En el **aplicaciones** , haga clic en **crear una aplicación nueva**.</span><span class="sxs-lookup"><span data-stu-id="b9488-194">On the **Apps** tab, click **Create New App**.</span></span>

    ![Crear una nueva aplicación](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. <span data-ttu-id="b9488-196">Escriba un **nombre de la aplicación** y **categoría**, a continuación, haga clic en **crear aplicación**.</span><span class="sxs-lookup"><span data-stu-id="b9488-196">Enter an **App Name** and **Category**, then click **Create App**.</span></span>

    <span data-ttu-id="b9488-197">Esto debe ser único en Facebook.</span><span class="sxs-lookup"><span data-stu-id="b9488-197">This must be unique across Facebook.</span></span> <span data-ttu-id="b9488-198">El **aplicación Namespace** es la parte de la dirección URL que la aplicación utilizará para tener acceso a la aplicación de Facebook para la autenticación (por ejemplo, https://apps.facebook.com/ {aplicación Namespace}).</span><span class="sxs-lookup"><span data-stu-id="b9488-198">The **App Namespace** is the part of the URL that your App will use to access the Facebook application for authentication (for example, https://apps.facebook.com/{App Namespace}).</span></span> <span data-ttu-id="b9488-199">Si no se especifica un **aplicación Namespace**, el **identificador de la aplicación** se usará para la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="b9488-199">If you don't specify an **App Namespace**, the **App ID** will be used for the URL.</span></span> <span data-ttu-id="b9488-200">El **Id. de aplicación** es un número long-generados por el sistema que se incluye en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="b9488-200">The **App ID** is a long system-generated number that you will see in the next step.</span></span>

    ![Crear cuadro de diálogo nueva aplicación](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. <span data-ttu-id="b9488-202">Enviar la comprobación de seguridad estándar.</span><span class="sxs-lookup"><span data-stu-id="b9488-202">Submit the standard security check.</span></span>

    ![Comprobación de seguridad](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. <span data-ttu-id="b9488-204">Seleccione **configuración** de la barra de menú de la izquierda![ Barra de menús del programador de Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="b9488-204">Select **Settings** for the left menu bar.![Facebook Developer's menu bar](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span></span>
7. <span data-ttu-id="b9488-205">En el **básica** sección de configuración de la página Seleccionar **Agregar plataforma** para especificar que va a agregar una aplicación del sitio Web.</span><span class="sxs-lookup"><span data-stu-id="b9488-205">On the **Basic** settings section of the page select **Add Platform** to specify that you are adding a website application.</span></span> <span data-ttu-id="b9488-206">![Configuración básica](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span><span class="sxs-lookup"><span data-stu-id="b9488-206">![Basic Settings](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span></span>
8. <span data-ttu-id="b9488-207">Seleccione **sitio Web** entre las opciones de plataforma.</span><span class="sxs-lookup"><span data-stu-id="b9488-207">Select **Website** from the platform choices.</span></span>  
  
    ![Opciones de plataforma](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. <span data-ttu-id="b9488-209">Tome nota de su **Id. de aplicación** y su **secreto de la aplicación** para que pueda agregar tanto en la aplicación de MVC más adelante en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="b9488-209">Make a note of your **App ID** and your **App Secret** so that you can add both into your MVC application later in this tutorial.</span></span> <span data-ttu-id="b9488-210">Además, agregue la dirección URL del sitio (`https://localhost:44300/`) para probar la aplicación MVC.</span><span class="sxs-lookup"><span data-stu-id="b9488-210">Also, Add your Site URL (`https://localhost:44300/`) to test your MVC application.</span></span> <span data-ttu-id="b9488-211">Además, agregue un **correo electrónico de contacto**.</span><span class="sxs-lookup"><span data-stu-id="b9488-211">Also, add a **Contact Email**.</span></span> <span data-ttu-id="b9488-212">A continuación, seleccione **guardar cambios**.</span><span class="sxs-lookup"><span data-stu-id="b9488-212">Then, select **Save Changes**.</span></span>   

    ![Página de detalles de una aplicación básica](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > <span data-ttu-id="b9488-214">Tenga en cuenta que sólo pueda autenticarse mediante el alias de correo electrónico que se ha registrado.</span><span class="sxs-lookup"><span data-stu-id="b9488-214">Note that you will only be able to authenticate using the email alias you have registered.</span></span> <span data-ttu-id="b9488-215">Otros usuarios y cuentas de prueba no será capaz de registrar.</span><span class="sxs-lookup"><span data-stu-id="b9488-215">Other users and test accounts will not be able to register.</span></span> <span data-ttu-id="b9488-216">Puede conceder acceso de otras cuentas de Facebook para la aplicación de Facebook **Roles de desarrollador** ficha.</span><span class="sxs-lookup"><span data-stu-id="b9488-216">You can grant other Facebook accounts access to the application on the Facebook **Developer Roles** tab.</span></span>
10. <span data-ttu-id="b9488-217">En Visual Studio, abra *aplicación\_Start\Startup.Auth.cs*.</span><span class="sxs-lookup"><span data-stu-id="b9488-217">In Visual Studio, open *App\_Start\Startup.Auth.cs*.</span></span>
11. <span data-ttu-id="b9488-218">Copie y pegue el **AppId** y **secreto de la aplicación** en el `UseFacebookAuthentication` método.</span><span class="sxs-lookup"><span data-stu-id="b9488-218">Copy and paste the **AppId** and **App Secret** into the `UseFacebookAuthentication` method.</span></span> <span data-ttu-id="b9488-219">El **AppId** y **secreto de la aplicación** son ejemplos de valores que se muestran a continuación y no funcionará.</span><span class="sxs-lookup"><span data-stu-id="b9488-219">The **AppId** and **App Secret** values shown below are samples and will not work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. <span data-ttu-id="b9488-220">Haga clic en **guardar cambios**.</span><span class="sxs-lookup"><span data-stu-id="b9488-220">Click **Save Changes**.</span></span>
13. <span data-ttu-id="b9488-221">Presione **CTRL+F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b9488-221">Press **CTRL+F5** to run the application.</span></span>


<span data-ttu-id="b9488-222">Seleccione **sesión** para mostrar la página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="b9488-222">Select **Log in** to display the Login page.</span></span> <span data-ttu-id="b9488-223">Haga clic en **Facebook** en **utilice otro servicio para iniciar sesión.**</span><span class="sxs-lookup"><span data-stu-id="b9488-223">Click **Facebook** under **Use another service to log in.**</span></span>

<span data-ttu-id="b9488-224">Escriba sus credenciales de Facebook.</span><span class="sxs-lookup"><span data-stu-id="b9488-224">Enter your Facebook credentials.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

<span data-ttu-id="b9488-225">Se le pedirá que conceda permiso para la aplicación tener acceso a su perfil público y una lista de confianza.</span><span class="sxs-lookup"><span data-stu-id="b9488-225">You will be prompted to grant permission for the application to access your public profile and friend list.</span></span>

![Detalles de la aplicación de Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

<span data-ttu-id="b9488-227">Ahora que haya iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="b9488-227">You are now logged in.</span></span> <span data-ttu-id="b9488-228">Ahora puede registrar esta cuenta con la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b9488-228">You can now register this account with the application.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

<span data-ttu-id="b9488-229">Al registrar, se agrega una entrada para el *usuarios* tabla de la base de datos de pertenencia.</span><span class="sxs-lookup"><span data-stu-id="b9488-229">When you register, an entry is added to the *Users* table of the membership database.</span></span>

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a><span data-ttu-id="b9488-230">Examinar los datos de pertenencia</span><span class="sxs-lookup"><span data-stu-id="b9488-230">Examine the Membership Data</span></span>

<span data-ttu-id="b9488-231">En el **vista** menú, haga clic en **Explorador de servidores**.</span><span class="sxs-lookup"><span data-stu-id="b9488-231">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

<span data-ttu-id="b9488-232">Expanda **DefaultConnection (MvcAuth)**, expanda **tablas**, haga clic en **AspNetUsers** y haga clic en **mostrar datos de tabla**.</span><span class="sxs-lookup"><span data-stu-id="b9488-232">Expand **DefaultConnection (MvcAuth)**, expand **Tables**, right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![datos de la tabla de aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a><span data-ttu-id="b9488-234">Agregar datos de perfil para la clase de usuario</span><span class="sxs-lookup"><span data-stu-id="b9488-234">Adding Profile Data to the User Class</span></span>

<span data-ttu-id="b9488-235">En esta sección agregará la fecha de nacimiento y ciudad natal a los datos de usuario durante el registro, como se muestra en la siguiente imagen.</span><span class="sxs-lookup"><span data-stu-id="b9488-235">In this section you'll add birth date and home town to the user data during registration, as shown in the following image.</span></span>

![reg con ciudad natal y Cump.](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

<span data-ttu-id="b9488-237">Abra la *Models\IdentityModels.cs* de archivos y agregar propiedades de Pueblo de página principal y la fecha de nacimiento:</span><span class="sxs-lookup"><span data-stu-id="b9488-237">Open the *Models\IdentityModels.cs* file and add birth date and home town properties:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

<span data-ttu-id="b9488-238">Abra la *Models\AccountViewModels.cs* de archivos y el conjunto de propiedades de ciudad de fecha y de inicio en de nacimiento `ExternalLoginConfirmationViewModel`.</span><span class="sxs-lookup"><span data-stu-id="b9488-238">Open the *Models\AccountViewModels.cs* file and the set birth date and home town properties in `ExternalLoginConfirmationViewModel`.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

<span data-ttu-id="b9488-239">Abra la *Controllers\AccountController.cs* de archivos y agregue código para la ciudad de página principal y la fecha de nacimiento en la `ExternalLoginConfirmation` método de acción como se muestra:</span><span class="sxs-lookup"><span data-stu-id="b9488-239">Open the *Controllers\AccountController.cs* file and add code for birth date and home town in the `ExternalLoginConfirmation` action method as shown:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

<span data-ttu-id="b9488-240">Agregar la fecha de nacimiento y ciudad natal a la *Views\Account\ExternalLoginConfirmation.cshtml* archivo:</span><span class="sxs-lookup"><span data-stu-id="b9488-240">Add birth date and home town to the *Views\Account\ExternalLoginConfirmation.cshtml* file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

<span data-ttu-id="b9488-241">Eliminar la base de datos de pertenencia para que pueda volver a registrar su cuenta de Facebook con la aplicación y compruebe que puede agregar la nueva fecha de nacimiento e información de perfil de ciudad natal.</span><span class="sxs-lookup"><span data-stu-id="b9488-241">Delete the membership database so you can again register your Facebook account with your application and verify you can add the new birth date and home town profile information.</span></span>

<span data-ttu-id="b9488-242">De **el Explorador de soluciones**, haga clic en el **mostrar todos los archivos** icono y, a continuación, haga clic derecho *agregar\_Data\aspnet-MvcAuth -&lt;dateStamp&gt;.mdf* y haga clic en **eliminar**.</span><span class="sxs-lookup"><span data-stu-id="b9488-242">From **Solution Explorer**, click the **Show All Files** icon, then right click *Add\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* and click **Delete**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

<span data-ttu-id="b9488-243">Desde el **herramientas** menú, haga clic en **Administrador de paquetes de NuGet**, a continuación, haga clic en **Package Manager Console** (PMC).</span><span class="sxs-lookup"><span data-stu-id="b9488-243">From the **Tools** menu, click **NuGet Package Manger**, then click **Package Manager Console** (PMC).</span></span> <span data-ttu-id="b9488-244">Escriba los siguientes comandos en el PMC.</span><span class="sxs-lookup"><span data-stu-id="b9488-244">Enter the following commands in the PMC.</span></span>

1. <span data-ttu-id="b9488-245">Enable-Migrations</span><span class="sxs-lookup"><span data-stu-id="b9488-245">Enable-Migrations</span></span>
2. <span data-ttu-id="b9488-246">Migración agregar Init</span><span class="sxs-lookup"><span data-stu-id="b9488-246">Add-Migration Init</span></span>
3. <span data-ttu-id="b9488-247">Actualizar base de datos</span><span class="sxs-lookup"><span data-stu-id="b9488-247">Update-Database</span></span>

<span data-ttu-id="b9488-248">Ejecute la aplicación y usar Google y FaceBook para iniciar sesión y registrar algunos usuarios.</span><span class="sxs-lookup"><span data-stu-id="b9488-248">Run the application and use FaceBook and Google to log in and register some users.</span></span>

## <a name="examine-the-membership-data"></a><span data-ttu-id="b9488-249">Examinar los datos de pertenencia</span><span class="sxs-lookup"><span data-stu-id="b9488-249">Examine the Membership Data</span></span>

<span data-ttu-id="b9488-250">En el **vista** menú, haga clic en **Explorador de servidores**.</span><span class="sxs-lookup"><span data-stu-id="b9488-250">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

<span data-ttu-id="b9488-251">Haga clic en **AspNetUsers** y haga clic en **mostrar datos de tabla**.</span><span class="sxs-lookup"><span data-stu-id="b9488-251">Right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

<span data-ttu-id="b9488-252">El `HomeTown` y `BirthDate` a continuación se muestran los campos.</span><span class="sxs-lookup"><span data-stu-id="b9488-252">The `HomeTown` and `BirthDate` fields are shown below.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a><span data-ttu-id="b9488-253">Cerrar la aplicación de la sesión e iniciar sesión con otra cuenta</span><span class="sxs-lookup"><span data-stu-id="b9488-253">Logging off your App and Logging in With Another Account</span></span>

<span data-ttu-id="b9488-254">Si iniciar sesión en su aplicación con Facebook y, a continuación, cierre sesión y vuelva a intentar iniciar nuevo con una cuenta de Facebook diferente (con el mismo explorador), se inmediatamente grabará en a la cuenta de Facebook anterior que usa.</span><span class="sxs-lookup"><span data-stu-id="b9488-254">If you log on to your app with Facebook,, and then log out and try to log in again with a different Facebook account (using the same browser), you will be immediately logged in to the previous Facebook account you used.</span></span> <span data-ttu-id="b9488-255">Para usar otra cuenta, debe navegar a Facebook y cerrar la sesión en Facebook.</span><span class="sxs-lookup"><span data-stu-id="b9488-255">In order to use another account, you need to navigate to Facebook and log out at Facebook.</span></span> <span data-ttu-id="b9488-256">La misma regla se aplica a cualquier otro proveedor de autenticación parte 3ª.</span><span class="sxs-lookup"><span data-stu-id="b9488-256">The same rule applies to any other 3rd party authentication provider.</span></span> <span data-ttu-id="b9488-257">Como alternativa, puede iniciar sesión con otra cuenta mediante un explorador diferente.</span><span class="sxs-lookup"><span data-stu-id="b9488-257">Alternatively, you can log in with another account by using a different browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b9488-258">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="b9488-258">Next Steps</span></span>

<span data-ttu-id="b9488-259">Vea [Introducción a los proveedores de seguridad de Yahoo y LinkedIn OAuth para OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) por Jerrie Pelser para obtener instrucciones de Yahoo y LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="b9488-259">See [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions.</span></span> <span data-ttu-id="b9488-260">Consulte del Jerrie queda botones de inicio de sesión social para ASP.NET MVC 5 obtener habilitar inicio de sesión social botones.</span><span class="sxs-lookup"><span data-stu-id="b9488-260">See Jerrie's Pretty social login buttons for ASP.NET MVC 5 to get enable social login buttons.</span></span>

<span data-ttu-id="b9488-261">Siga el tutorial [crear una aplicación de MVC de ASP.NET con la autenticación y la base de datos SQL e implementar al servicio de aplicaciones de Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), que sigue este tutorial y muestra lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b9488-261">Follow my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), which continues this tutorial and shows the following:</span></span>

1. <span data-ttu-id="b9488-262">Cómo implementar la aplicación en Azure.</span><span class="sxs-lookup"><span data-stu-id="b9488-262">How to deploy your app to Azure.</span></span>
2. <span data-ttu-id="b9488-263">Cómo proteger las aplicaciones con los roles.</span><span class="sxs-lookup"><span data-stu-id="b9488-263">How to secure you app with roles.</span></span>
3. <span data-ttu-id="b9488-264">Cómo proteger la aplicación con el [RequireHttps](https://msdn.microsoft.com/en-us/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) y [Authorize](https://msdn.microsoft.com/en-us/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtros.</span><span class="sxs-lookup"><span data-stu-id="b9488-264">How to secure your app with the [RequireHttps](https://msdn.microsoft.com/en-us/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) and [Authorize](https://msdn.microsoft.com/en-us/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filters.</span></span>
4. <span data-ttu-id="b9488-265">Describe cómo usar la API de pertenencia para agregar usuarios y roles.</span><span class="sxs-lookup"><span data-stu-id="b9488-265">How to use the membership API to add users and roles.</span></span>

<span data-ttu-id="b9488-266">Vota sobre cómo le gustó este tutorial y lo que podemos mejorar.</span><span class="sxs-lookup"><span data-stu-id="b9488-266">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="b9488-267">También puede solicitar nuevos temas en [mostrar Me cómo con código](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span><span class="sxs-lookup"><span data-stu-id="b9488-267">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span> <span data-ttu-id="b9488-268">Incluso puede solicitar y votar sobre las nuevas características que se agregarán a ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b9488-268">You can even ask for and vote on new features to be added to ASP.NET.</span></span> <span data-ttu-id="b9488-269">Por ejemplo, puede votar por una herramienta para [crear y administrar usuarios y roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span><span class="sxs-lookup"><span data-stu-id="b9488-269">For example, you can vote for a tool to [create and manage users and roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span></span>

<span data-ttu-id="b9488-270">Para una buena explicación de cómo funcionan los servicios de autenticación externos de ASP.NET, vea de Robert McMurray [servicios de autenticación externos](https://asp.net/web-api/overview/security/external-authentication-services).</span><span class="sxs-lookup"><span data-stu-id="b9488-270">For an good explanation of how ASP.NET External Authentication Services work, see Robert McMurray's [External Authentication Services](https://asp.net/web-api/overview/security/external-authentication-services).</span></span> <span data-ttu-id="b9488-271">Artículo de Robert también entra en detalle para habilitar la autenticación de Microsoft y Twitter.</span><span class="sxs-lookup"><span data-stu-id="b9488-271">Robert's article also goes into detail in enabling Microsoft and Twitter authentication.</span></span> <span data-ttu-id="b9488-272">Tom Dykstra del excelente [tutorial EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) muestra cómo trabajar con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b9488-272">Tom Dykstra's excellent [EF/MVC tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) shows how to work with the Entity Framework.</span></span>
