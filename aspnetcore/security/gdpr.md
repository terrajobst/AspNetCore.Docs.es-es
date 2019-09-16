---
title: Compatibilidad de Reglamento general de protección de datos (RGPD) en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo acceder a los puntos de extensión de RGPD en una aplicación Web de ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/11/2019
uid: security/gdpr
ms.openlocfilehash: 1086c22c2f3c27373d8cb779f4b1d8eb6792ec2e
ms.sourcegitcommit: 2fa0ffe82a47c7317efc9ea908365881cbcb8ed7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2019
ms.locfileid: "69572873"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a><span data-ttu-id="49608-103">Compatibilidad con Reglamento general de protección de datos de la Unión Europea (RGPD) en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="49608-103">EU General Data Protection Regulation (GDPR) support in ASP.NET Core</span></span>

<span data-ttu-id="49608-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="49608-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="49608-105">ASP.NET Core proporciona API y plantillas para ayudar a cumplir algunos de los requisitos de la Reglamento general de protección de datos de la [UE (RGPD)](https://www.eugdpr.org/) :</span><span class="sxs-lookup"><span data-stu-id="49608-105">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements:</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="49608-106">Las plantillas de proyecto incluyen puntos de extensión y marcado stub que puede reemplazar con la Directiva de privacidad y uso de cookies.</span><span class="sxs-lookup"><span data-stu-id="49608-106">The project templates include extension points and stubbed markup that you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="49608-107">La página *pages/privacy. cshtml* o views */Home/privacy. cshtml* proporciona una página para detallar la Directiva de privacidad de su sitio.</span><span class="sxs-lookup"><span data-stu-id="49608-107">The *Pages/Privacy.cshtml* page or *Views/Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span>

<span data-ttu-id="49608-108">Para habilitar la característica de consentimiento de cookies predeterminada, como la que se encuentra en las plantillas de ASP.NET Core 2,2 en una aplicación de ASP.NET Core 3,0 plantillas generadas:</span><span class="sxs-lookup"><span data-stu-id="49608-108">To enable the default cookie consent feature like that found in the ASP.NET Core 2.2 templates in an ASP.NET Core 3.0 template generated app:</span></span>

* <span data-ttu-id="49608-109">Agregue `using Microsoft.AspNetCore.Http` a la lista de directivas using.</span><span class="sxs-lookup"><span data-stu-id="49608-109">Add `using Microsoft.AspNetCore.Http` to the list of using directives.</span></span>
* <span data-ttu-id="49608-110">Agregue [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) a `Startup.ConfigureServices` y [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) a `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="49608-110">Add [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) to `Startup.ConfigureServices` and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) to `Startup.Configure`:</span></span>

  [!code-csharp[Main](gdpr/sample/RP3.0/Startup.cs?name=snippet1&highlight=12-19,38)]

* <span data-ttu-id="49608-111">Agregue el consentimiento de la cookie parcial al archivo *_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="49608-111">Add the cookie consent partial to the *_Layout.cshtml* file:</span></span>

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_Layout.cshtml?name=snippet&highlight=4)]

* <span data-ttu-id="49608-112">Agregue el  *\_archivo CookieConsentPartial. cshtml* al proyecto:</span><span class="sxs-lookup"><span data-stu-id="49608-112">Add the *\_CookieConsentPartial.cshtml* file to the project:</span></span>

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_CookieConsentPartial.cshtml)]

* <span data-ttu-id="49608-113">Seleccione la versión ASP.NET Core 2,2 de este artículo para obtener información acerca de la característica de consentimiento de cookies.</span><span class="sxs-lookup"><span data-stu-id="49608-113">Select the ASP.NET Core 2.2 version of this article to read about the cookie consent feature.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

* <span data-ttu-id="49608-114">Las plantillas de proyecto incluyen puntos de extensión y marcado stub que puede reemplazar con la Directiva de privacidad y uso de cookies.</span><span class="sxs-lookup"><span data-stu-id="49608-114">The project templates include extension points and stubbed markup that you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="49608-115">Una característica de consentimiento de cookies le permite solicitar (y realizar un seguimiento) el consentimiento de los usuarios para almacenar información personal.</span><span class="sxs-lookup"><span data-stu-id="49608-115">A cookie consent feature allows you to ask for (and track) consent from your users for storing personal information.</span></span> <span data-ttu-id="49608-116">Si un usuario no ha dado su consentimiento a la recopilación de datos y la aplicación tiene [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) establecido en `true`, las cookies no esenciales no se envían al explorador.</span><span class="sxs-lookup"><span data-stu-id="49608-116">If a user hasn't consented to data collection and the app has [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) set to `true`, non-essential cookies aren't sent to the browser.</span></span>
* <span data-ttu-id="49608-117">Las cookies se pueden marcar como esenciales.</span><span class="sxs-lookup"><span data-stu-id="49608-117">Cookies can be marked as essential.</span></span> <span data-ttu-id="49608-118">Las cookies esenciales se envían al explorador incluso cuando el usuario no ha dado su consentimiento y el seguimiento está deshabilitado.</span><span class="sxs-lookup"><span data-stu-id="49608-118">Essential cookies are sent to the browser even when the user hasn't consented and tracking is disabled.</span></span>
* <span data-ttu-id="49608-119">[TempData y las cookies de sesión](#tempdata) no funcionan cuando el seguimiento está deshabilitado.</span><span class="sxs-lookup"><span data-stu-id="49608-119">[TempData and Session cookies](#tempdata) aren't functional when tracking is disabled.</span></span>
* <span data-ttu-id="49608-120">La página de [Administración de identidades](#pd) proporciona un vínculo para descargar y eliminar los datos de usuario.</span><span class="sxs-lookup"><span data-stu-id="49608-120">The [Identity manage](#pd) page provides a link to download and delete user data.</span></span>

<span data-ttu-id="49608-121">La [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) permite probar la mayoría de los puntos de extensión RGPD y las API agregadas a las plantillas ASP.net Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="49608-121">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) allows you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span> <span data-ttu-id="49608-122">Vea el archivo [Léame](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) para obtener instrucciones de prueba.</span><span class="sxs-lookup"><span data-stu-id="49608-122">See the [ReadMe](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) file for testing instructions.</span></span>

<span data-ttu-id="49608-123">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="49608-123">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a><span data-ttu-id="49608-124">Compatibilidad de ASP.NET Core RGPD en el código generado por plantillas</span><span class="sxs-lookup"><span data-stu-id="49608-124">ASP.NET Core GDPR support in template-generated code</span></span>

<span data-ttu-id="49608-125">Los proyectos de Razor Pages y MVC creados con las plantillas de proyecto incluyen la siguiente compatibilidad con RGPD:</span><span class="sxs-lookup"><span data-stu-id="49608-125">Razor Pages and MVC projects created with the project templates include the following GDPR support:</span></span>

* <span data-ttu-id="49608-126">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) y [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) se establecen en la `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="49608-126">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) are set in the `Startup` class.</span></span>
* <span data-ttu-id="49608-127">[Vista parcial](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)de  *\_CookieConsentPartial. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="49608-127">The *\_CookieConsentPartial.cshtml* [partial view](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span> <span data-ttu-id="49608-128">En este archivo se incluye un botón **Aceptar** .</span><span class="sxs-lookup"><span data-stu-id="49608-128">An **Accept** button is included in this file.</span></span> <span data-ttu-id="49608-129">Cuando el usuario hace clic en el botón **Aceptar** , se proporciona el consentimiento para almacenar las cookies.</span><span class="sxs-lookup"><span data-stu-id="49608-129">When the user clicks the **Accept** button, consent to store cookies is provided.</span></span>
* <span data-ttu-id="49608-130">La página *pages/privacy. cshtml* o views */Home/privacy. cshtml* proporciona una página para detallar la Directiva de privacidad de su sitio.</span><span class="sxs-lookup"><span data-stu-id="49608-130">The *Pages/Privacy.cshtml* page or *Views/Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span> <span data-ttu-id="49608-131">El archivo CookieConsentPartial. cshtml genera un vínculo a la página de privacidad.  *\_*</span><span class="sxs-lookup"><span data-stu-id="49608-131">The *\_CookieConsentPartial.cshtml* file generates a link to the Privacy page.</span></span>
* <span data-ttu-id="49608-132">En el caso de las aplicaciones creadas con cuentas de usuario individuales, la página Administrar proporciona vínculos para descargar y eliminar los [datos personales](#pd)de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="49608-132">For apps created with individual user accounts, the Manage page provides links to download and delete [personal user data](#pd).</span></span>

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a><span data-ttu-id="49608-133">CookiePolicyOptions y UseCookiePolicy</span><span class="sxs-lookup"><span data-stu-id="49608-133">CookiePolicyOptions and UseCookiePolicy</span></span>

<span data-ttu-id="49608-134">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) se inicializan en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="49608-134">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) are initialized in `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

<span data-ttu-id="49608-135">Se llama a [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) en `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="49608-135">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) is called in `Startup.Configure`:</span></span>

[!code-csharp[](gdpr/sample/Startup.cs?name=snippet1&highlight=51)]

### <a name="_cookieconsentpartialcshtml-partial-view"></a><span data-ttu-id="49608-136">\_Vista parcial de CookieConsentPartial. cshtml</span><span class="sxs-lookup"><span data-stu-id="49608-136">\_CookieConsentPartial.cshtml partial view</span></span>

<span data-ttu-id="49608-137">La vista parcial de  *\_CookieConsentPartial. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="49608-137">The *\_CookieConsentPartial.cshtml* partial view:</span></span>

[!code-html[](gdpr/sample/RP2.2/Pages/Shared/_CookieConsentPartial.cshtml)]

<span data-ttu-id="49608-138">Este:</span><span class="sxs-lookup"><span data-stu-id="49608-138">This partial:</span></span>

* <span data-ttu-id="49608-139">Obtiene el estado de seguimiento del usuario.</span><span class="sxs-lookup"><span data-stu-id="49608-139">Obtains the state of tracking for the user.</span></span> <span data-ttu-id="49608-140">Si la aplicación está configurada para requerir el consentimiento, el usuario debe dar su consentimiento antes de que se pueda realizar el seguimiento de las cookies.</span><span class="sxs-lookup"><span data-stu-id="49608-140">If the app is configured to require consent, the user must consent before cookies can be tracked.</span></span> <span data-ttu-id="49608-141">Si se requiere el consentimiento, el panel de consentimiento de cookies se fija en la parte superior de la barra de navegación creada por el  *\_archivo layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="49608-141">If consent is required, the cookie consent panel is fixed at top of the navigation bar created by the *\_Layout.cshtml* file.</span></span>
* <span data-ttu-id="49608-142">Proporciona un elemento `<p>` HTML para resumir la privacidad y la Directiva de uso de cookies.</span><span class="sxs-lookup"><span data-stu-id="49608-142">Provides an HTML `<p>` element to summarize your privacy and cookie use policy.</span></span>
* <span data-ttu-id="49608-143">Proporciona un vínculo a la página de privacidad o vista donde puede obtener detalles de la Directiva de privacidad del sitio.</span><span class="sxs-lookup"><span data-stu-id="49608-143">Provides a link to Privacy page or view where you can detail your site's privacy policy.</span></span>

## <a name="essential-cookies"></a><span data-ttu-id="49608-144">Cookies esenciales</span><span class="sxs-lookup"><span data-stu-id="49608-144">Essential cookies</span></span>

<span data-ttu-id="49608-145">Si no se ha proporcionado el consentimiento para almacenar las cookies, solo se envían al explorador las cookies marcadas como esenciales.</span><span class="sxs-lookup"><span data-stu-id="49608-145">If consent to store cookies hasn't been provided, only cookies marked essential are sent to the browser.</span></span> <span data-ttu-id="49608-146">El código siguiente hace que una cookie sea esencial:</span><span class="sxs-lookup"><span data-stu-id="49608-146">The following code makes a cookie essential:</span></span>

[!code-csharp[Main](gdpr/sample/RP2.2/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

### <a name="tempdata-provider-and-session-state-cookies-arent-essential"></a><span data-ttu-id="49608-147">Las cookies del proveedor TempData y del estado de sesión no son esenciales</span><span class="sxs-lookup"><span data-stu-id="49608-147">TempData provider and session state cookies aren't essential</span></span>

<span data-ttu-id="49608-148">La cookie del [proveedor TempData](xref:fundamentals/app-state#tempdata) no es esencial.</span><span class="sxs-lookup"><span data-stu-id="49608-148">The [TempData provider](xref:fundamentals/app-state#tempdata) cookie isn't essential.</span></span> <span data-ttu-id="49608-149">Si el seguimiento está deshabilitado, el proveedor TempData no funciona.</span><span class="sxs-lookup"><span data-stu-id="49608-149">If tracking is disabled, the TempData provider isn't functional.</span></span> <span data-ttu-id="49608-150">Para habilitar el proveedor TempData cuando el seguimiento está deshabilitado, marque la cookie TempData `Startup.ConfigureServices`como esencial en:</span><span class="sxs-lookup"><span data-stu-id="49608-150">To enable the TempData provider when tracking is disabled, mark the TempData cookie as essential in `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/RP2.2/Startup.cs?name=snippet1)]

<span data-ttu-id="49608-151">Las cookies de [Estado de sesión](xref:fundamentals/app-state) no son esenciales.</span><span class="sxs-lookup"><span data-stu-id="49608-151">[Session state](xref:fundamentals/app-state) cookies are not essential.</span></span> <span data-ttu-id="49608-152">El estado de sesión no funciona cuando el seguimiento está deshabilitado.</span><span class="sxs-lookup"><span data-stu-id="49608-152">Session state isn't functional when tracking is disabled.</span></span> <span data-ttu-id="49608-153">El código siguiente hace que las cookies de sesión sean esenciales:</span><span class="sxs-lookup"><span data-stu-id="49608-153">The following code makes session cookies essential:</span></span>

[!code-csharp[](gdpr/sample/RP2.2/Startup.cs?name=snippet2)]

<a name="pd"></a>

## <a name="personal-data"></a><span data-ttu-id="49608-154">Datos personales</span><span class="sxs-lookup"><span data-stu-id="49608-154">Personal data</span></span>

<span data-ttu-id="49608-155">ASP.NET Core aplicaciones creadas con cuentas de usuario individuales incluyen código para descargar y eliminar los datos personales.</span><span class="sxs-lookup"><span data-stu-id="49608-155">ASP.NET Core apps created with individual user accounts include code to download and delete personal data.</span></span>

<span data-ttu-id="49608-156">Seleccione el nombre de usuario y, a continuación, seleccione **datos personales**:</span><span class="sxs-lookup"><span data-stu-id="49608-156">Select the user name and then select **Personal data**:</span></span>

![Página administrar datos personales](gdpr/_static/pd.png)

<span data-ttu-id="49608-158">Notas:</span><span class="sxs-lookup"><span data-stu-id="49608-158">Notes:</span></span>

* <span data-ttu-id="49608-159">Para generar el `Account/Manage` código, vea [scaffolding Identity](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="49608-159">To generate the `Account/Manage` code, see [Scaffold Identity](xref:security/authentication/scaffold-identity).</span></span>
* <span data-ttu-id="49608-160">Los vínculos de **eliminación** y **descarga** solo actúan sobre los datos de identidad predeterminados.</span><span class="sxs-lookup"><span data-stu-id="49608-160">The **Delete** and **Download** links only act on the default identity data.</span></span> <span data-ttu-id="49608-161">Las aplicaciones que crean datos de usuario personalizados deben extenderse para eliminar o descargar los datos de usuario personalizados.</span><span class="sxs-lookup"><span data-stu-id="49608-161">Apps that create custom user data must be extended to delete/download the custom user data.</span></span> <span data-ttu-id="49608-162">Para obtener más información, vea [Agregar, descargar y eliminar datos de usuario personalizados a Identity](xref:security/authentication/add-user-data).</span><span class="sxs-lookup"><span data-stu-id="49608-162">For more information, see [Add, download, and delete custom user data to Identity](xref:security/authentication/add-user-data).</span></span>
* <span data-ttu-id="49608-163">Los tokens guardados para el usuario que se almacenan en la `AspNetUserTokens` tabla de la base de datos de identidad se eliminan cuando se elimina el usuario mediante el comportamiento de eliminación en cascada debido a la [clave externa](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="49608-163">Saved tokens for the user that are stored in the Identity database table `AspNetUserTokens` are deleted when the user is deleted via the cascading delete behavior due to the [foreign key](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span></span>
* <span data-ttu-id="49608-164">La [autenticación de proveedor externo](xref:security/authentication/social/index), como Facebook y Google, no está disponible antes de que se acepte la Directiva de cookies.</span><span class="sxs-lookup"><span data-stu-id="49608-164">[External provider authentication](xref:security/authentication/social/index), such as Facebook and Google, isn't available before the cookie policy is accepted.</span></span>

::: moniker-end

## <a name="encryption-at-rest"></a><span data-ttu-id="49608-165">Cifrado en reposo</span><span class="sxs-lookup"><span data-stu-id="49608-165">Encryption at rest</span></span>

<span data-ttu-id="49608-166">Algunas bases de datos y mecanismos de almacenamiento permiten el cifrado en reposo.</span><span class="sxs-lookup"><span data-stu-id="49608-166">Some databases and storage mechanisms allow for encryption at rest.</span></span> <span data-ttu-id="49608-167">Cifrado en reposo:</span><span class="sxs-lookup"><span data-stu-id="49608-167">Encryption at rest:</span></span>

* <span data-ttu-id="49608-168">Cifra automáticamente los datos almacenados.</span><span class="sxs-lookup"><span data-stu-id="49608-168">Encrypts stored data automatically.</span></span>
* <span data-ttu-id="49608-169">Cifra sin configuración, programación u otro trabajo para el software que tiene acceso a los datos.</span><span class="sxs-lookup"><span data-stu-id="49608-169">Encrypts without configuration, programming, or other work for the software that accesses the data.</span></span>
* <span data-ttu-id="49608-170">Es la opción más sencilla y más segura.</span><span class="sxs-lookup"><span data-stu-id="49608-170">Is the easiest and safest option.</span></span>
* <span data-ttu-id="49608-171">Permite a la base de datos administrar las claves y el cifrado.</span><span class="sxs-lookup"><span data-stu-id="49608-171">Allows the database to manage keys and encryption.</span></span>

<span data-ttu-id="49608-172">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="49608-172">For example:</span></span>

* <span data-ttu-id="49608-173">Microsoft SQL y Azure SQL proporcionan [cifrado de datos transparente](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span><span class="sxs-lookup"><span data-stu-id="49608-173">Microsoft SQL and Azure SQL provide [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span></span>
* [<span data-ttu-id="49608-174">SQL Azure cifra la base de datos de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="49608-174">SQL Azure encrypts the database by default</span></span>](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* <span data-ttu-id="49608-175">[Los blobs de Azure, los archivos, las tablas y los Queue Storage están cifrados de forma predeterminada](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span><span class="sxs-lookup"><span data-stu-id="49608-175">[Azure Blobs, Files, Table, and Queue Storage are encrypted by default](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span></span>

<span data-ttu-id="49608-176">En el caso de las bases de datos que no proporcionan cifrado integrado en reposo, es posible que pueda usar el cifrado de disco para proporcionar la misma protección.</span><span class="sxs-lookup"><span data-stu-id="49608-176">For databases that don't provide built-in encryption at rest, you may be able to use disk encryption to provide the same protection.</span></span> <span data-ttu-id="49608-177">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="49608-177">For example:</span></span>

* [<span data-ttu-id="49608-178">BitLocker para Windows Server</span><span class="sxs-lookup"><span data-stu-id="49608-178">BitLocker for Windows Server</span></span>](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* <span data-ttu-id="49608-179">Linux:</span><span class="sxs-lookup"><span data-stu-id="49608-179">Linux:</span></span>
  * [<span data-ttu-id="49608-180">eCryptfs</span><span class="sxs-lookup"><span data-stu-id="49608-180">eCryptfs</span></span>](https://launchpad.net/ecryptfs)
  * <span data-ttu-id="49608-181">[EncFS](https://github.com/vgough/encfs).</span><span class="sxs-lookup"><span data-stu-id="49608-181">[EncFS](https://github.com/vgough/encfs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="49608-182">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="49608-182">Additional resources</span></span>

* [<span data-ttu-id="49608-183">Microsoft.com/GDPR</span><span class="sxs-lookup"><span data-stu-id="49608-183">Microsoft.com/GDPR</span></span>](https://www.microsoft.com/trustcenter/Privacy/GDPR)
* [<span data-ttu-id="49608-184">RGPD: agregar un botón revocar consentimiento en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="49608-184">GDPR - Adding a Revoke Consent Button in ASP.NET Core</span></span>](https://www.joeaudette.com/blog/2018/08/28/gdpr---adding-a-revoke-consent-button-in-aspnet-core)
