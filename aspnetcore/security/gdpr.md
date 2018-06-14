---
title: Admite la normativa de protección de datos generales (GDPR) en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de cómo obtener acceso a los puntos de extensión GDPR en una aplicación web de ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/gdpr
ms.openlocfilehash: c3c8a3fcd4a303aea65c57ff6be2ff0434383f33
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341930"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a>Compatibilidad de la UE General datos protección normativa (GDPR) en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core proporciona las API y plantillas para ayudar a cumplir algunos de los [normativa General de protección de datos (GDPR) de la UE](https://www.eugdpr.org/) requisitos:

* Las plantillas de proyecto incluyen puntos de extensión y marcado auxiliar que puede reemplazar por su privacidad y la directiva de uso de cookies.
* Una característica de consentimiento de cookie permite pedir consentimiento (y realizar un seguimiento) de los usuarios para almacenar la información personal. Si un usuario no ha dado su consentimiento para la recopilación de datos y la aplicación está configurada con [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) a `true`, las cookies no sean esenciales no se enviará al explorador.
* Las cookies se pueden marcar como esenciales. Las cookies esenciales se envían al explorador incluso cuando el usuario no ha dado su consentimiento y seguimiento está deshabilitado.
* [Las cookies de sesión y TempData](#tempdata) no son funcionales cuando el seguimiento está deshabilitado.
* El [administrar identidades](#pd) página proporciona un vínculo para descargar y eliminar datos de usuario.

El [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) le permite probar la mayoría de las API que se agregará a las plantillas de ASP.NET Core 2.1 y puntos de extensión GDPR. Consulte la [Léame](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) un archivo para probar las instrucciones.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a>Código generado por el soporte técnico GDPR de ASP.NET Core en plantilla

Las páginas de Razor y MVC proyectos creados con las plantillas de proyecto incluyen la compatibilidad GDPR siguiente:

* [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) y [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) se establecen en `Startup`.
* El *_CookieConsentPartial.cshtml* [vista parcial](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).
* El *Pages/Privacy.cshtml* o *Home/Privacy.cshtml* vista proporciona una página de detalle de la directiva de privacidad de su sitio. El *_CookieConsentPartial.cshtml* archivo genera un vínculo a la página de privacidad.
* Para las aplicaciones creadas con cuentas de usuario individuales, la página de administración proporciona vínculos para descargar y eliminar [personal del usuario](#pd).

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a>CookiePolicyOptions y UseCookiePolicy

[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) se inicializan en el `Startup` clase `ConfigureServices` método:

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) se llama el `Startup` clase `Configure` método:

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=49)]

### <a name="cookieconsentpartialcshtml-partial-view"></a>Vista parcial _CookieConsentPartial.cshtml

El *_CookieConsentPartial.cshtml* vista parcial:

[!code-html[Main](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

Este parcial:

* Obtiene el estado de seguimiento para el usuario. Si la aplicación se configura para requerir el consentimiento que del usuario debe dar su consentimiento antes de que pueden realizar el seguimiento de las cookies. Si se requiere consentimiento, el cromo de consentimiento de la cookie se fija en la barra de navegación que se creó en la parte superior del *Pages/Shared/_Layout.cshtml* archivo.
* Proporciona una etiqueta HTML `<p>` usar la directiva de elemento que se va a resumir su privacidad y cookies.
* Proporciona un vínculo a *Pages/Privacy.cshtml* donde puede detallar política de privacidad de su sitio.

## <a name="essential-cookies"></a>Cookies esenciales

Si no se ha proporcionado el consentimiento, solo las cookies de marcado esenciales se envían al explorador. El código siguiente realiza una cookie esenciales:

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a>Cookies de estado de sesión y el proveedor TempData no sean esenciales

El [proveedor Tempdata](xref:fundamentals/app-state#tempdata) cookie no es esencial. Si se deshabilita el seguimiento, el proveedor Tempdata no es funcional. Para habilitar el proveedor Tempdata cuando el seguimiento está deshabilitado, marcar la cookie de TempData como esenciales en `ConfigureServices`:

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

[Estado de sesión](xref:fundamentals/app-state) cookies no son esenciales. Estado de sesión no es funcional cuando se deshabilita el seguimiento.

<a name="pd"></a>

## <a name="personal-data"></a>Datos personales

Las aplicaciones de ASP.NET Core creadas con cuentas de usuario individuales incluyen código para descargar y eliminar datos personales.

Seleccione el nombre de usuario y, a continuación, seleccione **datos personales**:

![Administrar la página de datos personales](gdpr/_static/pd.png)

Notas:

* Para generar el `Account/Manage` código, vea [Scaffold identidad](xref:security/authentication/scaffold-identity).
* Eliminar y descargar impacto solamente los datos de identidad de manera predeterminada. Los datos de usuario personalizado de creación de aplicaciones deben extenderse para delete/descargar los datos de usuario personalizada. Problema de GitHub [cómo agregar o eliminar datos de usuario personalizado a la identidad](https://github.com/aspnet/Docs/issues/6226) realiza un seguimiento de un artículo propuesto acerca de cómo crear personalizado/eliminar/descarga de datos de usuario personalizada. Si le gustaría que ese tema un nivel de prioridad, deje un Pulgar hacia arriba reacción en el problema.
* Guarda los tokens para el usuario que se almacenan en la tabla de base de datos de identidad `AspNetUserTokens` se eliminan cuando el usuario se elimina mediante el comportamiento de eliminación en cascada debido a la [clave externa](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).

## <a name="encryption-at-rest"></a>Cifrado en reposo

Algunas bases de datos y mecanismos de almacenamiento permiten para el cifrado en reposo. Cifrado en reposo:

* Cifra automáticamente los datos almacenados.
* Cifra sin configuración, programación u otras tareas para el software que tiene acceso a los datos.
* Es la opción más sencilla y más segura.
* Permite que la base de datos administre las claves y el cifrado.

Por ejemplo:

* Microsoft SQL y SQL Azure proporcionan [cifrado de datos transparente](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).
* [SQL Azure cifra la base de datos de forma predeterminada](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* [Azure Blobs, archivos, tabla y cola de almacenamiento se cifran de forma predeterminada](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).

Para las bases de datos que no proporcionan cifrado integrado en reposo, es posible que pueda usar el cifrado del disco para proporcionar la misma protección. Por ejemplo:

* [BitLocker para Windows Server](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* Linux:
  * [eCryptfs](https://launchpad.net/ecryptfs)
  * [EncFS](https://github.com/vgough/encfs).

## <a name="additional-resources"></a>Recursos adicionales

* [Microsoft.com/GDPR](https://www.microsoft.com/trustcenter/Privacy/GDPR)
