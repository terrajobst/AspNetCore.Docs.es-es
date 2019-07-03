---
title: Autenticación en la nube con Azure Active Directory B2C en ASP.NET Core
author: camsoper
description: Descubra cómo configurar la autenticación de Azure Active Directory B2C con ASP.NET Core.
ms.author: casoper
ms.custom: mvc
ms.date: 02/27/2019
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: 3cb878aff7bf0c6c8efe7f3f0c0f06c74acef477
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538730"
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a><span data-ttu-id="68d94-103">Autenticación en la nube con Azure Active Directory B2C en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="68d94-103">Cloud authentication with Azure Active Directory B2C in ASP.NET Core</span></span>

<span data-ttu-id="68d94-104">Por [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="68d94-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="68d94-105">[Azure B2C de Active Directory](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) es una solución de administración de identidades de nube para aplicaciones web y móviles.</span><span class="sxs-lookup"><span data-stu-id="68d94-105">[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) is a cloud identity management solution for web and mobile apps.</span></span> <span data-ttu-id="68d94-106">El servicio proporciona autenticación para las aplicaciones hospedadas en la nube y locales.</span><span class="sxs-lookup"><span data-stu-id="68d94-106">The service provides authentication for apps hosted in the cloud and on-premises.</span></span> <span data-ttu-id="68d94-107">Tipos de autenticación incluyen cuentas individuales, las cuentas de redes sociales y federados cuentas de empresa.</span><span class="sxs-lookup"><span data-stu-id="68d94-107">Authentication types include individual accounts, social network accounts, and federated enterprise accounts.</span></span> <span data-ttu-id="68d94-108">Además, Azure AD B2C puede proporcionar la autenticación multifactor con una configuración mínima.</span><span class="sxs-lookup"><span data-stu-id="68d94-108">Additionally, Azure AD B2C can provide multi-factor authentication with minimal configuration.</span></span>

> [!TIP]
> <span data-ttu-id="68d94-109">Azure Active Directory (Azure AD) y Azure AD B2C son ofertas de producto independiente.</span><span class="sxs-lookup"><span data-stu-id="68d94-109">Azure Active Directory (Azure AD) and Azure AD B2C are separate product offerings.</span></span> <span data-ttu-id="68d94-110">Un inquilino de Azure AD representa una organización, mientras que un inquilino de Azure AD B2C representa una colección de identidades para su uso con las aplicaciones de confianza.</span><span class="sxs-lookup"><span data-stu-id="68d94-110">An Azure AD tenant represents an organization, while an Azure AD B2C tenant represents a collection of identities to be used with relying party applications.</span></span> <span data-ttu-id="68d94-111">Para obtener más información, consulte [Azure AD B2C: Preguntas más frecuentes (P+F)](/azure/active-directory-b2c/active-directory-b2c-faqs).</span><span class="sxs-lookup"><span data-stu-id="68d94-111">To learn more, see [Azure AD B2C: Frequently asked questions (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).</span></span>

<span data-ttu-id="68d94-112">En este tutorial, aprenderá cómo:</span><span class="sxs-lookup"><span data-stu-id="68d94-112">In this tutorial, learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="68d94-113">Crear a un inquilino de Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="68d94-113">Create an Azure Active Directory B2C tenant</span></span>
> * <span data-ttu-id="68d94-114">Registrar una aplicación en B2C de Azure AD</span><span class="sxs-lookup"><span data-stu-id="68d94-114">Register an app in Azure AD B2C</span></span>
> * <span data-ttu-id="68d94-115">Use Visual Studio para crear una aplicación web de ASP.NET Core configurada para usar al inquilino de Azure AD B2C para la autenticación</span><span class="sxs-lookup"><span data-stu-id="68d94-115">Use Visual Studio to create an ASP.NET Core web app configured to use the Azure AD B2C tenant for authentication</span></span>
> * <span data-ttu-id="68d94-116">Configurar directivas que controlan el comportamiento del inquilino de Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="68d94-116">Configure policies controlling the behavior of the Azure AD B2C tenant</span></span>

## <a name="prerequisites"></a><span data-ttu-id="68d94-117">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="68d94-117">Prerequisites</span></span>

<span data-ttu-id="68d94-118">Se requiere para este tutorial lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="68d94-118">The following are required for this walkthrough:</span></span>

* [<span data-ttu-id="68d94-119">Suscripción de Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="68d94-119">Microsoft Azure subscription</span></span>](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [<span data-ttu-id="68d94-120">Visual Studio 2019</span><span class="sxs-lookup"><span data-stu-id="68d94-120">Visual Studio 2019</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)

## <a name="create-the-azure-active-directory-b2c-tenant"></a><span data-ttu-id="68d94-121">Crear al inquilino de Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="68d94-121">Create the Azure Active Directory B2C tenant</span></span>

<span data-ttu-id="68d94-122">Crear un inquilino de Azure Active Directory B2C [tal como se describe en la documentación de](/azure/active-directory-b2c/active-directory-b2c-get-started).</span><span class="sxs-lookup"><span data-stu-id="68d94-122">Create an Azure Active Directory B2C tenant [as described in the documentation](/azure/active-directory-b2c/active-directory-b2c-get-started).</span></span> <span data-ttu-id="68d94-123">Cuando se le solicite, asociar al inquilino con una suscripción de Azure es opcional para este tutorial.</span><span class="sxs-lookup"><span data-stu-id="68d94-123">When prompted, associating the tenant with an Azure subscription is optional for this tutorial.</span></span>

## <a name="register-the-app-in-azure-ad-b2c"></a><span data-ttu-id="68d94-124">Registrar la aplicación en B2C de Azure AD</span><span class="sxs-lookup"><span data-stu-id="68d94-124">Register the app in Azure AD B2C</span></span>

<span data-ttu-id="68d94-125">En el inquilino de Azure AD B2C recién creado, registre la aplicación mediante [los pasos descritos en la documentación de](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) bajo el **registrar una aplicación web** sección.</span><span class="sxs-lookup"><span data-stu-id="68d94-125">In the newly created Azure AD B2C tenant, register your app using [the steps in the documentation](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) under the **Register a web app** section.</span></span> <span data-ttu-id="68d94-126">Detener el **crear un secreto de cliente de aplicación web** sección.</span><span class="sxs-lookup"><span data-stu-id="68d94-126">Stop at the **Create a web app client secret** section.</span></span> <span data-ttu-id="68d94-127">No es necesario un secreto de cliente para este tutorial.</span><span class="sxs-lookup"><span data-stu-id="68d94-127">A client secret isn't required for this tutorial.</span></span> 

<span data-ttu-id="68d94-128">Use los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="68d94-128">Use the following values:</span></span>

| <span data-ttu-id="68d94-129">Parámetro</span><span class="sxs-lookup"><span data-stu-id="68d94-129">Setting</span></span>                       | <span data-ttu-id="68d94-130">Valor</span><span class="sxs-lookup"><span data-stu-id="68d94-130">Value</span></span>                     | <span data-ttu-id="68d94-131">Notas</span><span class="sxs-lookup"><span data-stu-id="68d94-131">Notes</span></span>                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="68d94-132">**Name**</span><span class="sxs-lookup"><span data-stu-id="68d94-132">**Name**</span></span>                      | <span data-ttu-id="68d94-133">*&lt;Nombre de la aplicación&gt;*</span><span class="sxs-lookup"><span data-stu-id="68d94-133">*&lt;app name&gt;*</span></span>        | <span data-ttu-id="68d94-134">Escriba un **nombre** para la aplicación que describe la aplicación a los consumidores.</span><span class="sxs-lookup"><span data-stu-id="68d94-134">Enter a **Name** for the app that describes your app to consumers.</span></span>                                                                                                                                 |
| <span data-ttu-id="68d94-135">**Incluir aplicación web o API web**</span><span class="sxs-lookup"><span data-stu-id="68d94-135">**Include web app / web API**</span></span> | <span data-ttu-id="68d94-136">Sí</span><span class="sxs-lookup"><span data-stu-id="68d94-136">Yes</span></span>                       |                                                                                                                                                                                                    |
| <span data-ttu-id="68d94-137">**Permitir flujo implícito**</span><span class="sxs-lookup"><span data-stu-id="68d94-137">**Allow implicit flow**</span></span>       | <span data-ttu-id="68d94-138">Sí</span><span class="sxs-lookup"><span data-stu-id="68d94-138">Yes</span></span>                       |                                                                                                                                                                                                    |
| <span data-ttu-id="68d94-139">**Dirección URL de respuesta**</span><span class="sxs-lookup"><span data-stu-id="68d94-139">**Reply URL**</span></span>                 | `https://localhost:44300/signin-oidc` | <span data-ttu-id="68d94-140">Direcciones URL de respuesta son puntos de conexión donde Azure AD B2C devolverá los tokens que solicita la aplicación.</span><span class="sxs-lookup"><span data-stu-id="68d94-140">Reply URLs are endpoints where Azure AD B2C returns any tokens that your app requests.</span></span> <span data-ttu-id="68d94-141">Visual Studio proporciona la dirección URL de respuesta para usar.</span><span class="sxs-lookup"><span data-stu-id="68d94-141">Visual Studio provides the Reply URL to use.</span></span> <span data-ttu-id="68d94-142">Por ahora, escriba `https://localhost:44300/signin-oidc` para completar el formulario.</span><span class="sxs-lookup"><span data-stu-id="68d94-142">For now, enter `https://localhost:44300/signin-oidc` to complete the form.</span></span> |
| <span data-ttu-id="68d94-143">**URI de Id. de aplicación**</span><span class="sxs-lookup"><span data-stu-id="68d94-143">**App ID URI**</span></span>                | <span data-ttu-id="68d94-144">Deje en blanco</span><span class="sxs-lookup"><span data-stu-id="68d94-144">Leave blank</span></span>               | <span data-ttu-id="68d94-145">No es necesario para este tutorial.</span><span class="sxs-lookup"><span data-stu-id="68d94-145">Not required for this tutorial.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="68d94-146">**Incluir a cliente nativo**</span><span class="sxs-lookup"><span data-stu-id="68d94-146">**Include native client**</span></span>     | <span data-ttu-id="68d94-147">No</span><span class="sxs-lookup"><span data-stu-id="68d94-147">No</span></span>                        |                                                                                                                                                                                                    |

> [!WARNING]
> <span data-ttu-id="68d94-148">Si la configuración de una URL de respuesta que no sean localhost, ser consciente de la [restricciones en lo que se permite en la lista dirección URL de respuesta](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url).</span><span class="sxs-lookup"><span data-stu-id="68d94-148">If setting up a non-localhost Reply URL, be aware of the [constraints on what is allowed in the Reply URL list](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url).</span></span> 

<span data-ttu-id="68d94-149">Una vez registrada la aplicación, se muestra la lista de aplicaciones en el inquilino.</span><span class="sxs-lookup"><span data-stu-id="68d94-149">After the app is registered, the list of apps in the tenant is displayed.</span></span> <span data-ttu-id="68d94-150">Seleccione la aplicación que acaba de registrar.</span><span class="sxs-lookup"><span data-stu-id="68d94-150">Select the app that was just registered.</span></span> <span data-ttu-id="68d94-151">Seleccione el **copia** icono a la derecha de la **Id. de aplicación** campo para copiarlo en el Portapapeles.</span><span class="sxs-lookup"><span data-stu-id="68d94-151">Select the **Copy** icon to the right of the **Application ID** field to copy it to the clipboard.</span></span>

<span data-ttu-id="68d94-152">No hay nada más pueden configurarse en el inquilino de Azure AD B2C en este momento, pero deje abierta la ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="68d94-152">Nothing more can be configured in the Azure AD B2C tenant at this time, but leave the browser window open.</span></span> <span data-ttu-id="68d94-153">No hay más configuración después de crea la aplicación de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="68d94-153">There is more configuration after the ASP.NET Core app is created.</span></span>

## <a name="create-an-aspnet-core-app-in-visual-studio"></a><span data-ttu-id="68d94-154">Crear una aplicación ASP.NET Core en Visual Studio</span><span class="sxs-lookup"><span data-stu-id="68d94-154">Create an ASP.NET Core app in Visual Studio</span></span>

<span data-ttu-id="68d94-155">La plantilla de aplicación Web de Visual Studio puede configurarse para usar al inquilino de Azure AD B2C para la autenticación.</span><span class="sxs-lookup"><span data-stu-id="68d94-155">The Visual Studio Web Application template can be configured to use the Azure AD B2C tenant for authentication.</span></span>

<span data-ttu-id="68d94-156">En Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="68d94-156">In Visual Studio:</span></span>

1. <span data-ttu-id="68d94-157">Cree una aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="68d94-157">Create a new ASP.NET Core Web Application.</span></span> 
2. <span data-ttu-id="68d94-158">Seleccione **aplicación Web** en la lista de plantillas.</span><span class="sxs-lookup"><span data-stu-id="68d94-158">Select **Web Application** from the list of templates.</span></span>
3. <span data-ttu-id="68d94-159">Seleccione el **Cambiar autenticación** botón.</span><span class="sxs-lookup"><span data-stu-id="68d94-159">Select the **Change Authentication** button.</span></span>
    
    ![Botón de la autenticación de cambio](./azure-ad-b2c/_static/changeauth.png)

4. <span data-ttu-id="68d94-161">En el **Cambiar autenticación** cuadro de diálogo, seleccione **cuentas de usuario individuales**y, a continuación, seleccione **conectar a un almacén de usuario existente en la nube** en la lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="68d94-161">In the **Change Authentication** dialog, select **Individual User Accounts**, and then select **Connect to an existing user store in the cloud** in the dropdown.</span></span> 
    
    ![Cuadro de diálogo de autenticación de cambio](./azure-ad-b2c/_static/changeauthdialog.png)

5. <span data-ttu-id="68d94-163">Complete el formulario con los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="68d94-163">Complete the form with the following values:</span></span>
    
    | <span data-ttu-id="68d94-164">Parámetro</span><span class="sxs-lookup"><span data-stu-id="68d94-164">Setting</span></span>                       | <span data-ttu-id="68d94-165">Valor</span><span class="sxs-lookup"><span data-stu-id="68d94-165">Value</span></span>                                                 |
    |-------------------------------|-------------------------------------------------------|
    | <span data-ttu-id="68d94-166">**Nombre de dominio**</span><span class="sxs-lookup"><span data-stu-id="68d94-166">**Domain Name**</span></span>               | <span data-ttu-id="68d94-167">*&lt;el nombre de dominio del inquilino B2C&gt;*</span><span class="sxs-lookup"><span data-stu-id="68d94-167">*&lt;the domain name of your B2C tenant&gt;*</span></span>          |
    | <span data-ttu-id="68d94-168">**Id. de aplicación**</span><span class="sxs-lookup"><span data-stu-id="68d94-168">**Application ID**</span></span>            | <span data-ttu-id="68d94-169">*&lt;Pegue el identificador de aplicación desde el Portapapeles&gt;*</span><span class="sxs-lookup"><span data-stu-id="68d94-169">*&lt;paste the Application ID from the clipboard&gt;*</span></span> |
    | <span data-ttu-id="68d94-170">**Ruta de acceso de devolución de llamada**</span><span class="sxs-lookup"><span data-stu-id="68d94-170">**Callback Path**</span></span>             | <span data-ttu-id="68d94-171">*&lt;Use el valor predeterminado&gt;*</span><span class="sxs-lookup"><span data-stu-id="68d94-171">*&lt;use the default value&gt;*</span></span>                       |
    | <span data-ttu-id="68d94-172">**Directiva de registro o inicio de sesión**</span><span class="sxs-lookup"><span data-stu-id="68d94-172">**Sign-up or sign-in policy**</span></span> | `B2C_1_SiUpIn`                                        |
    | <span data-ttu-id="68d94-173">**Directiva de restablecimiento de contraseña**</span><span class="sxs-lookup"><span data-stu-id="68d94-173">**Reset password policy**</span></span>     | `B2C_1_SSPR`                                          |
    | <span data-ttu-id="68d94-174">**Editar directiva de perfil**</span><span class="sxs-lookup"><span data-stu-id="68d94-174">**Edit profile policy**</span></span>       | <span data-ttu-id="68d94-175">*&lt;Deje en blanco&gt;*</span><span class="sxs-lookup"><span data-stu-id="68d94-175">*&lt;leave blank&gt;*</span></span>                                 |
    
    <span data-ttu-id="68d94-176">Seleccione el **copia** junto al vínculo **URI de respuesta** para copiar el URI de respuesta en el Portapapeles.</span><span class="sxs-lookup"><span data-stu-id="68d94-176">Select the **Copy** link next to **Reply URI** to copy the Reply URI to the clipboard.</span></span> <span data-ttu-id="68d94-177">Seleccione **Aceptar** para cerrar el **Cambiar autenticación** cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="68d94-177">Select **OK** to close the **Change Authentication** dialog.</span></span> <span data-ttu-id="68d94-178">Seleccione **Aceptar** para crear la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="68d94-178">Select **OK** to create the web app.</span></span>

## <a name="finish-the-b2c-app-registration"></a><span data-ttu-id="68d94-179">Finalizar el registro de aplicación B2C</span><span class="sxs-lookup"><span data-stu-id="68d94-179">Finish the B2C app registration</span></span>

<span data-ttu-id="68d94-180">Volver a la ventana del explorador con las propiedades de la aplicación B2C sigue abiertas.</span><span class="sxs-lookup"><span data-stu-id="68d94-180">Return to the browser window with the B2C app properties still open.</span></span> <span data-ttu-id="68d94-181">Cambiar temporal **dirección URL de respuesta** especificado anteriormente para el valor copiado desde Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="68d94-181">Change the temporary **Reply URL** specified earlier to the value copied from Visual Studio.</span></span> <span data-ttu-id="68d94-182">Seleccione **guardar** en la parte superior de la ventana.</span><span class="sxs-lookup"><span data-stu-id="68d94-182">Select **Save** at the top of the window.</span></span>

> [!TIP]
> <span data-ttu-id="68d94-183">Si no los copió la URL de respuesta, use la dirección HTTPS desde la pestaña de depuración en las propiedades del proyecto web y anexe la **CallbackPath** valor desde *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="68d94-183">If you didn't copy the Reply URL, use the HTTPS address from the Debug tab in the web project properties, and append the **CallbackPath** value from *appsettings.json*.</span></span>

## <a name="configure-policies"></a><span data-ttu-id="68d94-184">Configuración de directivas</span><span class="sxs-lookup"><span data-stu-id="68d94-184">Configure policies</span></span>

<span data-ttu-id="68d94-185">Siga los pasos de la documentación de Azure AD B2C para [crear una directiva de registro o inicio de sesión](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy)y, a continuación, [crear una directiva de restablecimiento de contraseña](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy).</span><span class="sxs-lookup"><span data-stu-id="68d94-185">Use the steps in the Azure AD B2C documentation to [create a sign-up or sign-in policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy), and then [create a password reset policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy).</span></span> <span data-ttu-id="68d94-186">Use los valores de ejemplo proporcionados en la documentación de **proveedores de identidades**, **atributos de registro**, y **notificaciones de aplicación**.</span><span class="sxs-lookup"><span data-stu-id="68d94-186">Use the example values provided in the documentation for **Identity providers**, **Sign-up attributes**, and **Application claims**.</span></span> <span data-ttu-id="68d94-187">Mediante el **ejecutar ahora** para probar las directivas, como se describe en la documentación es opcional.</span><span class="sxs-lookup"><span data-stu-id="68d94-187">Using the **Run now** button to test the policies as described in the documentation is optional.</span></span>

> [!WARNING]
> <span data-ttu-id="68d94-188">Asegúrese de los nombres de directiva exactamente como se describe en la documentación de esas directivas utilizadas en el **Cambiar autenticación** cuadro de diálogo de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="68d94-188">Ensure the policy names are exactly as described in the documentation, as those policies were used in the **Change Authentication** dialog in Visual Studio.</span></span> <span data-ttu-id="68d94-189">Los nombres de directiva se pueden comprobar en *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="68d94-189">The policy names can be verified in *appsettings.json*.</span></span>

## <a name="configure-the-underlying-openidconnectoptionsjwtbearercookie-options"></a><span data-ttu-id="68d94-190">Configure las opciones de cookies o JwtBearer/OpenIdConnectOptions subyacentes</span><span class="sxs-lookup"><span data-stu-id="68d94-190">Configure the underlying OpenIdConnectOptions/JwtBearer/Cookie options</span></span>

<span data-ttu-id="68d94-191">Para configurar las opciones subyacentes directamente, usar la constante de esquema adecuado en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="68d94-191">To configure the underlying options directly, use the appropriate scheme constant in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<OpenIdConnectOptions>(
    AzureAD[B2C]Defaults.OpenIdScheme, options => 
    {
        // Omitted for brevity
    });

services.Configure<CookieAuthenticationOptions>(
    AzureAD[B2C]Defaults.CookieScheme, options => 
    {
        // Omitted for brevity
    });

services.Configure<JwtBearerOptions>(
    AzureAD[B2C]Defaults.JwtBearerAuthenticationScheme, options => 
    {
        // Omitted for brevity
    });
```

## <a name="run-the-app"></a><span data-ttu-id="68d94-192">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="68d94-192">Run the app</span></span>

<span data-ttu-id="68d94-193">En Visual Studio, presione **F5** para compilar y ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="68d94-193">In Visual Studio, press **F5** to build and run the app.</span></span> <span data-ttu-id="68d94-194">Una vez que inicie la aplicación web, seleccione **Accept** para aceptar el uso de cookies (si se le solicita) y, a continuación, seleccione **inicie sesión en**.</span><span class="sxs-lookup"><span data-stu-id="68d94-194">After the web app launches, select **Accept** to accept the use of cookies (if prompted), and then select **Sign in**.</span></span>

![Inicie sesión en la aplicación](./azure-ad-b2c/_static/signin.png)

<span data-ttu-id="68d94-196">El explorador se redirige a los inquilinos de Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="68d94-196">The browser redirects to the Azure AD B2C tenant.</span></span> <span data-ttu-id="68d94-197">Inicie sesión con una cuenta existente (si se ha creado uno probar las directivas) o seleccione **Suscríbase ahora** para crear una nueva cuenta.</span><span class="sxs-lookup"><span data-stu-id="68d94-197">Sign in with an existing account (if one was created testing the policies) or select **Sign up now** to create a new account.</span></span> <span data-ttu-id="68d94-198">El **¿olvidó su contraseña?** vínculo se usa para restablecer una contraseña olvidada.</span><span class="sxs-lookup"><span data-stu-id="68d94-198">The **Forgot your password?** link is used to reset a forgotten password.</span></span>

![Inicio de sesión de Azure AD B2C](./azure-ad-b2c/_static/b2csts.png)

<span data-ttu-id="68d94-200">Después de iniciar sesión correctamente, el explorador se redirige a la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="68d94-200">After successfully signing in, the browser redirects to the web app.</span></span>

![Correcto](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a><span data-ttu-id="68d94-202">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="68d94-202">Next steps</span></span>

<span data-ttu-id="68d94-203">En este tutorial ha aprendido a:</span><span class="sxs-lookup"><span data-stu-id="68d94-203">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="68d94-204">Crear a un inquilino de Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="68d94-204">Create an Azure Active Directory B2C tenant</span></span>
> * <span data-ttu-id="68d94-205">Registrar una aplicación en B2C de Azure AD</span><span class="sxs-lookup"><span data-stu-id="68d94-205">Register an app in Azure AD B2C</span></span>
> * <span data-ttu-id="68d94-206">Use Visual Studio para crear una aplicación Web de ASP.NET Core configurados para usar al inquilino de Azure AD B2C para la autenticación</span><span class="sxs-lookup"><span data-stu-id="68d94-206">Use Visual Studio to create an ASP.NET Core Web Application configured to use the Azure AD B2C tenant for authentication</span></span>
> * <span data-ttu-id="68d94-207">Configurar directivas que controlan el comportamiento del inquilino de Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="68d94-207">Configure policies controlling the behavior of the Azure AD B2C tenant</span></span>

<span data-ttu-id="68d94-208">Ahora que la aplicación de ASP.NET Core está configurada para usar Azure AD B2C para la autenticación, el [atributo Authorize](xref:security/authorization/simple) puede usarse para proteger la aplicación.</span><span class="sxs-lookup"><span data-stu-id="68d94-208">Now that the ASP.NET Core app is configured to use Azure AD B2C for authentication, the [Authorize attribute](xref:security/authorization/simple) can be used to secure your app.</span></span> <span data-ttu-id="68d94-209">Continuar desarrollando la aplicación Learning para:</span><span class="sxs-lookup"><span data-stu-id="68d94-209">Continue developing your app by learning to:</span></span>

* <span data-ttu-id="68d94-210">[Personalizar la interfaz de usuario de Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).</span><span class="sxs-lookup"><span data-stu-id="68d94-210">[Customize the Azure AD B2C user interface](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).</span></span>
* <span data-ttu-id="68d94-211">[Configurar los requisitos de complejidad de contraseña](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).</span><span class="sxs-lookup"><span data-stu-id="68d94-211">[Configure password complexity requirements](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).</span></span>
* <span data-ttu-id="68d94-212">[Habilitar la autenticación multifactor](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).</span><span class="sxs-lookup"><span data-stu-id="68d94-212">[Enable multi-factor authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).</span></span>
* <span data-ttu-id="68d94-213">Configurar proveedores de identidad adicional, como [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)y otros.</span><span class="sxs-lookup"><span data-stu-id="68d94-213">Configure additional identity providers, such as [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app), and others.</span></span>
* <span data-ttu-id="68d94-214">[Usar la API de Azure AD Graph](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) para recuperar información adicional del usuario, como la pertenencia al grupo, desde el inquilino de Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="68d94-214">[Use the Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) to retrieve additional user information, such as group membership, from the Azure AD B2C tenant.</span></span>
* <span data-ttu-id="68d94-215">[Proteger un núcleo de ASP.NET web API mediante Azure AD B2C](xref:security/authentication/azure-ad-b2c-webapi).</span><span class="sxs-lookup"><span data-stu-id="68d94-215">[Secure an ASP.NET Core web API using Azure AD B2C](xref:security/authentication/azure-ad-b2c-webapi).</span></span>
* <span data-ttu-id="68d94-216">[Llamar a una API web de .NET desde una aplicación web de .NET con Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).</span><span class="sxs-lookup"><span data-stu-id="68d94-216">[Call a .NET web API from a .NET web app using Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).</span></span>
