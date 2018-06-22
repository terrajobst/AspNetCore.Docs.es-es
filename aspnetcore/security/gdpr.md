---
title: Admite la normativa de protección de datos generales (GDPR) en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de cómo obtener acceso a los puntos de extensión GDPR en una aplicación web de ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/29/2018
uid: security/gdpr
ms.openlocfilehash: c986eeca572eecb43e76d56dbc5cb872a9dff6b2
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277643"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a><span data-ttu-id="271b2-103">Compatibilidad de la UE General datos protección normativa (GDPR) en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="271b2-103">EU General Data Protection Regulation (GDPR) support in ASP.NET Core</span></span>

<span data-ttu-id="271b2-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="271b2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="271b2-105">ASP.NET Core proporciona las API y plantillas para ayudar a cumplir algunos de los [normativa General de protección de datos (GDPR) de la UE](https://www.eugdpr.org/) requisitos:</span><span class="sxs-lookup"><span data-stu-id="271b2-105">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements:</span></span>

* <span data-ttu-id="271b2-106">Las plantillas de proyecto incluyen puntos de extensión y marcado auxiliar que puede reemplazar por su privacidad y la directiva de uso de cookies.</span><span class="sxs-lookup"><span data-stu-id="271b2-106">The project templates include extension points and stubbed markup you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="271b2-107">Una característica de consentimiento de cookie permite pedir consentimiento (y realizar un seguimiento) de los usuarios para almacenar la información personal.</span><span class="sxs-lookup"><span data-stu-id="271b2-107">A cookie consent feature allows you to ask for (and track) consent from your users for storing personal information.</span></span> <span data-ttu-id="271b2-108">Si un usuario no ha dado su consentimiento para la recopilación de datos y la aplicación está configurada con [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) a `true`, las cookies no sean esenciales no se enviará al explorador.</span><span class="sxs-lookup"><span data-stu-id="271b2-108">If a user has not consented to data collection and the app is set with [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) to `true`, non-essential cookies will not be sent to the browser.</span></span>
* <span data-ttu-id="271b2-109">Las cookies se pueden marcar como esenciales.</span><span class="sxs-lookup"><span data-stu-id="271b2-109">Cookies can be marked as essential.</span></span> <span data-ttu-id="271b2-110">Las cookies esenciales se envían al explorador incluso cuando el usuario no ha dado su consentimiento y seguimiento está deshabilitado.</span><span class="sxs-lookup"><span data-stu-id="271b2-110">Essential cookies are sent to the browser even when the user has not consented and tracking is disabled.</span></span>
* <span data-ttu-id="271b2-111">[Las cookies de sesión y TempData](#tempdata) no son funcionales cuando el seguimiento está deshabilitado.</span><span class="sxs-lookup"><span data-stu-id="271b2-111">[TempData and Session cookies](#tempdata) are not functional when tracking is disabled.</span></span>
* <span data-ttu-id="271b2-112">El [administrar identidades](#pd) página proporciona un vínculo para descargar y eliminar datos de usuario.</span><span class="sxs-lookup"><span data-stu-id="271b2-112">The [Identity manage](#pd) page provides a link to download and delete user data.</span></span>

<span data-ttu-id="271b2-113">El [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) le permite probar la mayoría de las API que se agregará a las plantillas de ASP.NET Core 2.1 y puntos de extensión GDPR.</span><span class="sxs-lookup"><span data-stu-id="271b2-113">The [sample app](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) lets you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span> <span data-ttu-id="271b2-114">Consulte la [Léame](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) un archivo para probar las instrucciones.</span><span class="sxs-lookup"><span data-stu-id="271b2-114">See the [ReadMe](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) file for testing instructions.</span></span>

<span data-ttu-id="271b2-115">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="271b2-115">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a><span data-ttu-id="271b2-116">Código generado por el soporte técnico GDPR de ASP.NET Core en plantilla</span><span class="sxs-lookup"><span data-stu-id="271b2-116">ASP.NET Core GDPR support in template generated code</span></span>

<span data-ttu-id="271b2-117">Las páginas de Razor y MVC proyectos creados con las plantillas de proyecto incluyen la compatibilidad GDPR siguiente:</span><span class="sxs-lookup"><span data-stu-id="271b2-117">Razor Pages and MVC projects created with the project templates include the following GDPR support:</span></span>

* <span data-ttu-id="271b2-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) y [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) se establecen en `Startup`.</span><span class="sxs-lookup"><span data-stu-id="271b2-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) are set in `Startup`.</span></span>
* <span data-ttu-id="271b2-119">El *_CookieConsentPartial.cshtml* [vista parcial](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="271b2-119">The *_CookieConsentPartial.cshtml* [partial view](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span>
* <span data-ttu-id="271b2-120">El *Pages/Privacy.cshtml* o *Home/Privacy.cshtml* vista proporciona una página de detalle de la directiva de privacidad de su sitio.</span><span class="sxs-lookup"><span data-stu-id="271b2-120">The *Pages/Privacy.cshtml* or *Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span> <span data-ttu-id="271b2-121">El *_CookieConsentPartial.cshtml* archivo genera un vínculo a la página de privacidad.</span><span class="sxs-lookup"><span data-stu-id="271b2-121">The *_CookieConsentPartial.cshtml* file generates a link to the privacy page.</span></span>
* <span data-ttu-id="271b2-122">Para las aplicaciones creadas con cuentas de usuario individuales, la página de administración proporciona vínculos para descargar y eliminar [personal del usuario](#pd).</span><span class="sxs-lookup"><span data-stu-id="271b2-122">For applications created with individual user accounts, the manage page provides links to download and delete [personal user data](#pd).</span></span>

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a><span data-ttu-id="271b2-123">CookiePolicyOptions y UseCookiePolicy</span><span class="sxs-lookup"><span data-stu-id="271b2-123">CookiePolicyOptions and UseCookiePolicy</span></span>

<span data-ttu-id="271b2-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) se inicializan en el `Startup` clase `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="271b2-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) are initialized in the `Startup` class `ConfigureServices` method:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

<span data-ttu-id="271b2-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) se llama el `Startup` clase `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="271b2-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) is called in the `Startup` class `Configure` method:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=49)]

### <a name="cookieconsentpartialcshtml-partial-view"></a><span data-ttu-id="271b2-126">Vista parcial _CookieConsentPartial.cshtml</span><span class="sxs-lookup"><span data-stu-id="271b2-126">_CookieConsentPartial.cshtml partial view</span></span>

<span data-ttu-id="271b2-127">El *_CookieConsentPartial.cshtml* vista parcial:</span><span class="sxs-lookup"><span data-stu-id="271b2-127">The *_CookieConsentPartial.cshtml* partial view:</span></span>

[!code-html[Main](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

<span data-ttu-id="271b2-128">Este parcial:</span><span class="sxs-lookup"><span data-stu-id="271b2-128">This partial:</span></span>

* <span data-ttu-id="271b2-129">Obtiene el estado de seguimiento para el usuario.</span><span class="sxs-lookup"><span data-stu-id="271b2-129">Gets the state of tracking for the user.</span></span> <span data-ttu-id="271b2-130">Si la aplicación se configura para requerir el consentimiento que del usuario debe dar su consentimiento antes de que pueden realizar el seguimiento de las cookies.</span><span class="sxs-lookup"><span data-stu-id="271b2-130">If the application is configured to require consent the user must consent before cookies can be tracked.</span></span> <span data-ttu-id="271b2-131">Si se requiere consentimiento, el cromo de consentimiento de la cookie se fija en la barra de navegación que se creó en la parte superior del *Pages/Shared/_Layout.cshtml* archivo.</span><span class="sxs-lookup"><span data-stu-id="271b2-131">If consent is required, the cookie consent chrome is fixed on top of the navigation bar created in the *Pages/Shared/_Layout.cshtml* file.</span></span>
* <span data-ttu-id="271b2-132">Proporciona una etiqueta HTML `<p>` usar la directiva de elemento que se va a resumir su privacidad y cookies.</span><span class="sxs-lookup"><span data-stu-id="271b2-132">Provides an HTML `<p>` element to summarize your privacy and cookie use policy.</span></span>
* <span data-ttu-id="271b2-133">Proporciona un vínculo a *Pages/Privacy.cshtml* donde puede detallar política de privacidad de su sitio.</span><span class="sxs-lookup"><span data-stu-id="271b2-133">Provides a link to *Pages/Privacy.cshtml* where you can detail your site's privacy policy.</span></span>

## <a name="essential-cookies"></a><span data-ttu-id="271b2-134">Cookies esenciales</span><span class="sxs-lookup"><span data-stu-id="271b2-134">Essential cookies</span></span>

<span data-ttu-id="271b2-135">Si no se ha proporcionado el consentimiento, solo las cookies de marcado esenciales se envían al explorador.</span><span class="sxs-lookup"><span data-stu-id="271b2-135">If consent has not been given, only cookies marked essential are sent to the browser.</span></span> <span data-ttu-id="271b2-136">El código siguiente realiza una cookie esenciales:</span><span class="sxs-lookup"><span data-stu-id="271b2-136">The following code makes a cookie essential:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a><span data-ttu-id="271b2-137">Cookies de estado de sesión y el proveedor TempData no sean esenciales</span><span class="sxs-lookup"><span data-stu-id="271b2-137">Tempdata provider and session state cookies are not essential</span></span>

<span data-ttu-id="271b2-138">El [proveedor Tempdata](xref:fundamentals/app-state#tempdata) cookie no es esencial.</span><span class="sxs-lookup"><span data-stu-id="271b2-138">The [Tempdata provider](xref:fundamentals/app-state#tempdata) cookie is not essential.</span></span> <span data-ttu-id="271b2-139">Si se deshabilita el seguimiento, el proveedor Tempdata no es funcional.</span><span class="sxs-lookup"><span data-stu-id="271b2-139">If tracking is disabled, the Tempdata provider is not functional.</span></span> <span data-ttu-id="271b2-140">Para habilitar el proveedor Tempdata cuando el seguimiento está deshabilitado, marcar la cookie de TempData como esenciales en `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="271b2-140">To enable the Tempdata provider when tracking is disabled, mark the TempData cookie as essential in `ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

<span data-ttu-id="271b2-141">[Estado de sesión](xref:fundamentals/app-state) cookies no son esenciales.</span><span class="sxs-lookup"><span data-stu-id="271b2-141">[Session state](xref:fundamentals/app-state) cookies are not essential.</span></span> <span data-ttu-id="271b2-142">Estado de sesión no es funcional cuando se deshabilita el seguimiento.</span><span class="sxs-lookup"><span data-stu-id="271b2-142">Session state is not functional when tracking is disabled.</span></span>

<a name="pd"></a>

## <a name="personal-data"></a><span data-ttu-id="271b2-143">Datos personales</span><span class="sxs-lookup"><span data-stu-id="271b2-143">Personal data</span></span>

<span data-ttu-id="271b2-144">Las aplicaciones de ASP.NET Core creadas con cuentas de usuario individuales incluyen código para descargar y eliminar datos personales.</span><span class="sxs-lookup"><span data-stu-id="271b2-144">ASP.NET Core applications created with individual user accounts include code to download and delete personal data.</span></span>

<span data-ttu-id="271b2-145">Seleccione el nombre de usuario y, a continuación, seleccione **datos personales**:</span><span class="sxs-lookup"><span data-stu-id="271b2-145">Select the user name and then select **Personal data**:</span></span>

![Administrar la página de datos personales](gdpr/_static/pd.png)

<span data-ttu-id="271b2-147">Notas:</span><span class="sxs-lookup"><span data-stu-id="271b2-147">Notes:</span></span>

* <span data-ttu-id="271b2-148">Para generar el `Account/Manage` código, vea [Scaffold identidad](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="271b2-148">To generate the `Account/Manage` code, see [Scaffold Identity](xref:security/authentication/scaffold-identity).</span></span>
* <span data-ttu-id="271b2-149">Eliminar y descargar impacto solamente los datos de identidad de manera predeterminada.</span><span class="sxs-lookup"><span data-stu-id="271b2-149">Delete and download only impact the default identity data.</span></span> <span data-ttu-id="271b2-150">Los datos de usuario personalizado de creación de aplicaciones deben extenderse para delete/descargar los datos de usuario personalizada.</span><span class="sxs-lookup"><span data-stu-id="271b2-150">Apps the create custom user data must be extended to delete/download the custom user data.</span></span> <span data-ttu-id="271b2-151">Problema de GitHub [cómo agregar o eliminar datos de usuario personalizado a la identidad](https://github.com/aspnet/Docs/issues/6226) realiza un seguimiento de un artículo propuesto acerca de cómo crear personalizado/eliminar/descarga de datos de usuario personalizada.</span><span class="sxs-lookup"><span data-stu-id="271b2-151">GitHub issue [How to add/delete custom user data to Identity](https://github.com/aspnet/Docs/issues/6226) tracks a proposed article on creating custom/deleting/downloading custom user data.</span></span> <span data-ttu-id="271b2-152">Si le gustaría que ese tema un nivel de prioridad, deje un Pulgar hacia arriba reacción en el problema.</span><span class="sxs-lookup"><span data-stu-id="271b2-152">If you'd like to see that topic prioritized, leave a thumbs up reaction in the issue.</span></span>
* <span data-ttu-id="271b2-153">Guarda los tokens para el usuario que se almacenan en la tabla de base de datos de identidad `AspNetUserTokens` se eliminan cuando el usuario se elimina mediante el comportamiento de eliminación en cascada debido a la [clave externa](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="271b2-153">Saved tokens for the user that are stored in the Identity database table `AspNetUserTokens` are deleted when the user is deleted via the cascading delete behavior due to the [foreign key](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span></span>

## <a name="encryption-at-rest"></a><span data-ttu-id="271b2-154">Cifrado en reposo</span><span class="sxs-lookup"><span data-stu-id="271b2-154">Encryption at rest</span></span>

<span data-ttu-id="271b2-155">Algunas bases de datos y mecanismos de almacenamiento permiten para el cifrado en reposo.</span><span class="sxs-lookup"><span data-stu-id="271b2-155">Some databases and storage mechanisms allow for encryption at rest.</span></span> <span data-ttu-id="271b2-156">Cifrado en reposo:</span><span class="sxs-lookup"><span data-stu-id="271b2-156">Encryption at rest:</span></span>

* <span data-ttu-id="271b2-157">Cifra automáticamente los datos almacenados.</span><span class="sxs-lookup"><span data-stu-id="271b2-157">Encrypts stored data automatically.</span></span>
* <span data-ttu-id="271b2-158">Cifra sin configuración, programación u otras tareas para el software que tiene acceso a los datos.</span><span class="sxs-lookup"><span data-stu-id="271b2-158">Encrypts without configuration, programming, or other work for the software that accesses the data.</span></span>
* <span data-ttu-id="271b2-159">Es la opción más sencilla y más segura.</span><span class="sxs-lookup"><span data-stu-id="271b2-159">Is the easiest and safest option.</span></span>
* <span data-ttu-id="271b2-160">Permite que la base de datos administre las claves y el cifrado.</span><span class="sxs-lookup"><span data-stu-id="271b2-160">Lets the database manage keys and encryption.</span></span>

<span data-ttu-id="271b2-161">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="271b2-161">For example:</span></span>

* <span data-ttu-id="271b2-162">Microsoft SQL y SQL Azure proporcionan [cifrado de datos transparente](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span><span class="sxs-lookup"><span data-stu-id="271b2-162">Microsoft SQL and Azure SQL provide [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span></span>
* [<span data-ttu-id="271b2-163">SQL Azure cifra la base de datos de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="271b2-163">SQL Azure encrypts the database by default</span></span>](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* <span data-ttu-id="271b2-164">[Azure Blobs, archivos, tabla y cola de almacenamiento se cifran de forma predeterminada](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span><span class="sxs-lookup"><span data-stu-id="271b2-164">[Azure Blobs, Files, Table, and Queue Storage are encrypted by default](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span></span>

<span data-ttu-id="271b2-165">Para las bases de datos que no proporcionan cifrado integrado en reposo, es posible que pueda usar el cifrado del disco para proporcionar la misma protección.</span><span class="sxs-lookup"><span data-stu-id="271b2-165">For databases that don't provide built-in encryption at rest you may be able to use disk encryption to provide the same protection.</span></span> <span data-ttu-id="271b2-166">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="271b2-166">For example:</span></span>

* [<span data-ttu-id="271b2-167">BitLocker para Windows Server</span><span class="sxs-lookup"><span data-stu-id="271b2-167">BitLocker for Windows Server</span></span>](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* <span data-ttu-id="271b2-168">Linux:</span><span class="sxs-lookup"><span data-stu-id="271b2-168">Linux:</span></span>
  * [<span data-ttu-id="271b2-169">eCryptfs</span><span class="sxs-lookup"><span data-stu-id="271b2-169">eCryptfs</span></span>](https://launchpad.net/ecryptfs)
  * <span data-ttu-id="271b2-170">[EncFS](https://github.com/vgough/encfs).</span><span class="sxs-lookup"><span data-stu-id="271b2-170">[EncFS](https://github.com/vgough/encfs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="271b2-171">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="271b2-171">Additional Resources</span></span>

* [<span data-ttu-id="271b2-172">Microsoft.com/GDPR</span><span class="sxs-lookup"><span data-stu-id="271b2-172">Microsoft.com/GDPR</span></span>](https://www.microsoft.com/en-us/trustcenter/Privacy/GDPR)
