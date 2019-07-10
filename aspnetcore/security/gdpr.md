---
title: Reglamento de protección de datos generales (RGPD) se admiten en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo obtener acceso a los puntos de extensión de RGPD en una aplicación web ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/11/2019
uid: security/gdpr
ms.openlocfilehash: 01d2f8943c0995c1400122b89c4ca7c459a85279
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724570"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a>Soporte técnico del Reglamento General de protección de datos (RGPD) de la UE en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core proporciona las API y plantillas para ayudar a cumplir algunos de los [Reglamento General de protección de datos (RGPD) de la UE](https://www.eugdpr.org/) requisitos:

::: moniker range=">= aspnetcore-3.0"

* Las plantillas de proyecto incluyen puntos de extensión y auxiliar marcado que se puede reemplazar con la privacidad y la directiva de uso de cookies.
* El *Pages/Privacy.cshtml* página o *Views/Home/Privacy.cshtml* vista proporciona una página para detallar política de privacidad de su sitio.

Para habilitar la característica de consentimiento de cookie predeterminado que se encuentra en las plantillas de ASP.NET Core 2.2 en una aplicación de ASP.NET Core 3.0 plantilla generada:

* Agregar [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) demasiado `Startup.ConfigureServices` y [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) a `Startup.Configure`:

  [!code-csharp[Main](gdpr/sample/RP3.0/Startup.cs?name=snippet1&highlight=12-19,38)]

* Agregar la parcial de consentimiento de cookie para la *_Layout.cshtml* archivo:

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_Layout.cshtml?name=snippet&highlight=4)]

* Agregar el  *\_CookieConsentPartial.cshtml* archivo al proyecto:

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_CookieConsentPartial.cshtml)]

* Seleccione la versión de ASP.NET Core 2.2 de este artículo para obtener información acerca de la característica de consentimiento de la cookie.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

* Las plantillas de proyecto incluyen puntos de extensión y auxiliar marcado que se puede reemplazar con la privacidad y la directiva de uso de cookies.
* Una característica de consentimiento de cookie permite pedir consentimiento (y realizar un seguimiento) de los usuarios para almacenar información personal. Si un usuario no ha dado su consentimiento para la recopilación de datos y la aplicación tiene [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) establecido en `true`, las cookies no esenciales no se envían al explorador.
* Las cookies se pueden marcar como esenciales. Esencial cookies se envían al explorador incluso cuando el usuario no ha dado su consentimiento y seguimiento está deshabilitado.
* [Las cookies de TempData y Session](#tempdata) no funcionan cuando se deshabilita el seguimiento.
* El [administrar identidades](#pd) página proporciona un vínculo para descargar y eliminar datos de usuario.

El [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) permite probar la mayoría de los puntos de extensión de RGPD y las API agregadas a las plantillas de ASP.NET Core 2.1. Consulte la [Léame](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) archivo para las pruebas de instrucciones.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a>ASP.NET Core RGPD se admiten en código generado por plantilla

Las páginas de Razor y MVC los proyectos creados con las plantillas de proyecto incluyen la compatibilidad de RGPD siguiente:

* [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) y [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) se establecen en el `Startup` clase.
* El  *\_CookieConsentPartial.cshtml* [vista parcial](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper). Un **Accept** botón se incluye en este archivo. Cuando el usuario hace clic en el **Accept** botón, da su consentimiento para almacenar las cookies se proporciona.
* El *Pages/Privacy.cshtml* página o *Views/Home/Privacy.cshtml* vista proporciona una página para detallar política de privacidad de su sitio. El  *\_CookieConsentPartial.cshtml* archivo genera un vínculo a la página de privacidad.
* Las aplicaciones creadas con cuentas de usuario individuales, la página de administración proporciona vínculos para descargar y eliminar [personal del usuario](#pd).

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a>CookiePolicyOptions y UseCookiePolicy

[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) se inicializan en `Startup.ConfigureServices`:

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) se denomina en `Startup.Configure`:

[!code-csharp[](gdpr/sample/Startup.cs?name=snippet1&highlight=51)]

### <a name="cookieconsentpartialcshtml-partial-view"></a>\_Vista parcial CookieConsentPartial.cshtml

El  *\_CookieConsentPartial.cshtml* vista parcial:

[!code-html[](gdpr/sample/RP2.2/Pages/Shared/_CookieConsentPartial.cshtml)]

Este parcial:

* Obtiene el estado de seguimiento para el usuario. Si la aplicación está configurada para requerir consentimiento, el usuario debe dar su consentimiento antes de que pueden realizar el seguimiento de las cookies. Si se requiere el consentimiento, el panel de consentimiento de la cookie se fija en la parte superior de la barra de navegación creada por el  *\_Layout.cshtml* archivo.
* Proporciona una etiqueta HTML `<p>` usar la directiva de elemento que se va a resumir su privacidad y cookies.
* Proporciona un vínculo a la página de privacidad o vista donde puede detallan la directiva de privacidad de su sitio.

## <a name="essential-cookies"></a>Cookies esenciales

Si su consentimiento almacenar las cookies no ha facilitado, solo las cookies de marcado esencial se envían al explorador. El código siguiente realiza una cookie esenciales:

[!code-csharp[Main](gdpr/sample/RP2.2/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

### <a name="tempdata-provider-and-session-state-cookies-arent-essential"></a>Cookies de estado de sesión y el proveedor TempData no son esenciales

El [proveedor TempData](xref:fundamentals/app-state#tempdata) cookie no es esencial. Si se deshabilita el seguimiento, el proveedor TempData no funcionará. Para habilitar el proveedor TempData cuando se deshabilita el seguimiento, marcar la cookie de TempData como esencial en `Startup.ConfigureServices`:

[!code-csharp[Main](gdpr/sample/RP2.2/Startup.cs?name=snippet1)]

[Estado de sesión](xref:fundamentals/app-state) las cookies no son esenciales. Estado de sesión no funcionará cuando se deshabilita el seguimiento. El código siguiente hace que las cookies de sesión esenciales:

[!code-csharp[](gdpr/sample/RP2.2/Startup.cs?name=snippet2)]

<a name="pd"></a>

## <a name="personal-data"></a>Datos personales

Las aplicaciones de ASP.NET Core creadas con cuentas de usuario individuales incluyen código para descargar y eliminar datos personales.

Seleccione el nombre de usuario y, a continuación, seleccione **datos personales**:

![Página de datos personales de administración](gdpr/_static/pd.png)

Notas:

* Para generar el `Account/Manage` código, vea [Scaffold identidad](xref:security/authentication/scaffold-identity).
* El **eliminar** y **descargar** vínculos solo actúan sobre los datos de identidad predeterminada. Las aplicaciones que crean datos de usuario personalizada deben ampliarse para eliminación y descarga los datos de usuario personalizada. Para obtener más información, consulte [Add, descargar y eliminar datos de usuario personalizada para identidad](xref:security/authentication/add-user-data).
* Guarda los tokens para el usuario que se almacenan en la tabla de base de datos de identidad `AspNetUserTokens` se eliminan cuando el usuario se elimina mediante el comportamiento de eliminación en cascada debido a la [clave externa](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).
* [Autenticación de proveedor externo](xref:security/authentication/social/index), como Facebook y Google, no está disponible antes de que se acepte la directiva.

::: moniker-end

## <a name="encryption-at-rest"></a>Cifrado en reposo

Algunas bases de datos y mecanismos de almacenamiento permiten para el cifrado en reposo. Cifrado en reposo:

* Cifra automáticamente los datos almacenados.
* Cifra sin configuración, programación u otros trabajos para el software que tiene acceso a los datos.
* Es la opción más sencilla y segura.
* Permite administrar las claves y cifrado de la base de datos.

Por ejemplo:

* Microsoft SQL y SQL de Azure proporcionan [cifrado de datos transparente](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).
* [SQL Azure cifra la base de datos de forma predeterminada](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* [Azure Blobs, archivos, Table y Queue Storage se cifran de forma predeterminada](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).

Para las bases de datos que no proporcionan cifrado integrado en reposo, puede usar el cifrado de disco para proporcionar la misma protección. Por ejemplo:

* [BitLocker para Windows Server](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* Linux:
  * [eCryptfs](https://launchpad.net/ecryptfs)
  * [EncFS](https://github.com/vgough/encfs).

## <a name="additional-resources"></a>Recursos adicionales

* [Microsoft.com/GDPR](https://www.microsoft.com/trustcenter/Privacy/GDPR)
