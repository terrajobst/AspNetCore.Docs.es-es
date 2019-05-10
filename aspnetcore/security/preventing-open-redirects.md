---
title: Evitar ataques de redireccionamiento abierto en ASP.NET Core
author: ardalis
description: Se muestra cómo evitar los ataques de redireccionamiento abierto en una aplicación ASP.NET Core
ms.author: riande
ms.date: 07/07/2017
uid: security/preventing-open-redirects
ms.openlocfilehash: 0896189d2caaccb19647eb7c6d57f29dfc0290dd
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64891622"
---
# <a name="prevent-open-redirect-attacks-in-aspnet-core"></a><span data-ttu-id="dfaf7-103">Evitar ataques de redireccionamiento abierto en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dfaf7-103">Prevent open redirect attacks in ASP.NET Core</span></span>

<span data-ttu-id="dfaf7-104">Una aplicación web que se redirige a una dirección URL que se especifica a través de la solicitud, como los datos de cadena de consulta o formulario potencialmente ser alterada para redirigir usuarios a una dirección URL externa, malintencionada.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-104">A web app that redirects to a URL that's specified via the request such as the querystring or form data can potentially be tampered with to redirect users to an external, malicious URL.</span></span> <span data-ttu-id="dfaf7-105">Esta modificación se llama a un ataque de redireccionamiento abierto.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-105">This tampering is called an open redirection attack.</span></span>

<span data-ttu-id="dfaf7-106">Cada vez que la lógica de aplicación redirige a una dirección URL especificada, debe comprobar que la dirección URL de redireccionamiento no se ha manipulado.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-106">Whenever your application logic redirects to a specified URL, you must verify that the redirection URL hasn't been tampered with.</span></span> <span data-ttu-id="dfaf7-107">ASP.NET Core tiene funcionalidad integrada para ayudar a proteger las aplicaciones frente a ataques de redireccionamiento abierto (también conocido como abierta redirección).</span><span class="sxs-lookup"><span data-stu-id="dfaf7-107">ASP.NET Core has built-in functionality to help protect apps from open redirect (also known as open redirection) attacks.</span></span>

## <a name="what-is-an-open-redirect-attack"></a><span data-ttu-id="dfaf7-108">¿Qué es un ataque de redireccionamiento abierto?</span><span class="sxs-lookup"><span data-stu-id="dfaf7-108">What is an open redirect attack?</span></span>

<span data-ttu-id="dfaf7-109">Las aplicaciones Web con frecuencia redirigen a los usuarios a una página de inicio de sesión cuando acceden a recursos que requieren autenticación.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-109">Web applications frequently redirect users to a login page when they access resources that require authentication.</span></span> <span data-ttu-id="dfaf7-110">La redirección normalmente incluye un `returnUrl` parámetro de cadena de consulta para que el usuario puede devolverse a la dirección URL solicitada originalmente después de que han iniciado sesión correctamente.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-110">The redirection typically includes a `returnUrl` querystring parameter so that the user can be returned to the originally requested URL after they have successfully logged in.</span></span> <span data-ttu-id="dfaf7-111">Después de que el usuario se autentica, le redirigirá a la dirección URL que tenían originalmente solicitada.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-111">After the user authenticates, they're redirected to the URL they had originally requested.</span></span>

<span data-ttu-id="dfaf7-112">Dado que la dirección URL de destino se especifica en el elemento querystring de la solicitud, un usuario malintencionado podría alterar la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-112">Because the destination URL is specified in the querystring of the request, a malicious user could tamper with the querystring.</span></span> <span data-ttu-id="dfaf7-113">Una cadena de consulta alterado podría permitir que el sitio redirigir al usuario a un sitio externo, malintencionado.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-113">A tampered querystring could allow the site to redirect the user to an external, malicious site.</span></span> <span data-ttu-id="dfaf7-114">Esta técnica se denomina un ataque de redirección (o la redirección) abierto.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-114">This technique is called an open redirect (or redirection) attack.</span></span>

### <a name="an-example-attack"></a><span data-ttu-id="dfaf7-115">Un ataque de ejemplo</span><span class="sxs-lookup"><span data-stu-id="dfaf7-115">An example attack</span></span>

<span data-ttu-id="dfaf7-116">Un usuario malintencionado puede desarrollar un ataque diseñado para permitir el acceso de usuarios malintencionados a las credenciales de un usuario o información confidencial.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-116">A malicious user can develop an attack intended to allow the malicious user access to a user's credentials or sensitive information.</span></span> <span data-ttu-id="dfaf7-117">Para iniciar el ataque, el usuario malintencionado convence al usuario que haga clic en un vínculo a la página de inicio de sesión de su sitio con un `returnUrl` querystring agregado valor a la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-117">To begin the attack, the malicious user convinces the user to click a link to your site's login page with a `returnUrl` querystring value added to the URL.</span></span> <span data-ttu-id="dfaf7-118">Por ejemplo, considere la posibilidad de una aplicación en `contoso.com` que incluye una página de inicio de sesión en `http://contoso.com/Account/LogOn?returnUrl=/Home/About`.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-118">For example, consider an app at `contoso.com` that includes a login page at `http://contoso.com/Account/LogOn?returnUrl=/Home/About`.</span></span> <span data-ttu-id="dfaf7-119">El ataque sigue estos pasos:</span><span class="sxs-lookup"><span data-stu-id="dfaf7-119">The attack follows these steps:</span></span>

1. <span data-ttu-id="dfaf7-120">El usuario hace clic en un vínculo malintencionado a `http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn` (la segunda dirección URL es "contoso**1**.com", no "contoso.com").</span><span class="sxs-lookup"><span data-stu-id="dfaf7-120">The user clicks a malicious link to `http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn` (the second URL is "contoso**1**.com", not "contoso.com").</span></span>
2. <span data-ttu-id="dfaf7-121">El usuario inicia sesión correctamente.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-121">The user logs in successfully.</span></span>
3. <span data-ttu-id="dfaf7-122">Se redirige al usuario (por el sitio) a `http://contoso1.com/Account/LogOn` (un sitio malintencionado que es exactamente igual que un sitio real).</span><span class="sxs-lookup"><span data-stu-id="dfaf7-122">The user is redirected (by the site) to `http://contoso1.com/Account/LogOn` (a malicious site that looks exactly like real site).</span></span>
4. <span data-ttu-id="dfaf7-123">El usuario inicia sesión de nuevo (lo que ofrece malintencionado sus credenciales de sitio) y se redirige al sitio real.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-123">The user logs in again (giving malicious site their credentials) and is redirected back to the real site.</span></span>

<span data-ttu-id="dfaf7-124">El usuario es probable que se considera que no se pudo su primer intento de iniciar sesión y que su segundo intento se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-124">The user likely believes that their first attempt to log in failed and that their second attempt is successful.</span></span> <span data-ttu-id="dfaf7-125">Más probable es que el usuario sigue siendo consciente de que están en peligro sus credenciales.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-125">The user most likely remains unaware that their credentials are compromised.</span></span>

![Proceso de ataques de redireccionamiento abierto](preventing-open-redirects/_static/open-redirection-attack-process.png)

<span data-ttu-id="dfaf7-127">Además de las páginas de inicio de sesión, algunos sitios proporcionan páginas de redireccionamiento o los puntos de conexión.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-127">In addition to login pages, some sites provide redirect pages or endpoints.</span></span> <span data-ttu-id="dfaf7-128">Imagine que su aplicación tiene una página con una redirección abierta, `/Home/Redirect`.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-128">Imagine your app has a page with an open redirect, `/Home/Redirect`.</span></span> <span data-ttu-id="dfaf7-129">Un atacante podría crear, por ejemplo, un vínculo en un correo electrónico que se dirige a `[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-129">An attacker could create, for example, a link in an email that goes to `[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`.</span></span> <span data-ttu-id="dfaf7-130">Un usuario típico consultará la dirección URL y que empieza con el nombre del sitio.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-130">A typical user will look at the URL and see it begins with your site name.</span></span> <span data-ttu-id="dfaf7-131">Confiar en, hará clic en el vínculo.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-131">Trusting that, they will click the link.</span></span> <span data-ttu-id="dfaf7-132">El redireccionamiento abierto, a continuación, enviaría el usuario para el sitio de suplantación de identidad, que es idéntico a la suya, y probablemente lo haría el usuario inicie sesión en lo creemos que es su sitio.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-132">The open redirect would then send the user to the phishing site, which looks identical to yours, and the user would likely login to what they believe is your site.</span></span>

## <a name="protecting-against-open-redirect-attacks"></a><span data-ttu-id="dfaf7-133">Protección contra ataques de redireccionamiento abierto</span><span class="sxs-lookup"><span data-stu-id="dfaf7-133">Protecting against open redirect attacks</span></span>

<span data-ttu-id="dfaf7-134">Al desarrollar aplicaciones web, tratar todos los datos proporcionados por el usuario como no es de confianza.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-134">When developing web applications, treat all user-provided data as untrustworthy.</span></span> <span data-ttu-id="dfaf7-135">Si la aplicación tiene la funcionalidad que se redirige al usuario según el contenido de la dirección URL, asegúrese de que estas redirecciones solo se realizan localmente dentro de la aplicación (o a una dirección URL conocida, no en cualquier dirección URL que se puede proporcionar en la cadena de consulta).</span><span class="sxs-lookup"><span data-stu-id="dfaf7-135">If your application has functionality that redirects the user based on the contents of the URL,  ensure that such redirects are only done locally within your app (or to a known URL, not any URL that may be supplied in the querystring).</span></span>

### <a name="localredirect"></a><span data-ttu-id="dfaf7-136">LocalRedirect</span><span class="sxs-lookup"><span data-stu-id="dfaf7-136">LocalRedirect</span></span>

<span data-ttu-id="dfaf7-137">Use la `LocalRedirect` método auxiliar de la base de `Controller` clase:</span><span class="sxs-lookup"><span data-stu-id="dfaf7-137">Use the `LocalRedirect` helper method from the base `Controller` class:</span></span>

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

<span data-ttu-id="dfaf7-138">`LocalRedirect` se iniciará una excepción si se especifica una URL no locales.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-138">`LocalRedirect` will throw an exception if a non-local URL is specified.</span></span> <span data-ttu-id="dfaf7-139">En caso contrario, se comporta igual que el `Redirect` método.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-139">Otherwise, it behaves just like the `Redirect` method.</span></span>

### <a name="islocalurl"></a><span data-ttu-id="dfaf7-140">IsLocalUrl</span><span class="sxs-lookup"><span data-stu-id="dfaf7-140">IsLocalUrl</span></span>

<span data-ttu-id="dfaf7-141">Use la [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) método para probar las direcciones URL antes de redirigir:</span><span class="sxs-lookup"><span data-stu-id="dfaf7-141">Use the [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) method to test URLs before redirecting:</span></span>

<span data-ttu-id="dfaf7-142">El ejemplo siguiente muestra cómo comprobar si una dirección URL es local antes de redirigir.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-142">The following example shows how to check whether a URL is local before redirecting.</span></span>

```csharp
private IActionResult RedirectToLocal(string returnUrl)
{
    if (Url.IsLocalUrl(returnUrl))
    {
        return Redirect(returnUrl);
    }
    else
    {
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }
}
```

<span data-ttu-id="dfaf7-143">El `IsLocalUrl` método protege a los usuarios sin darse cuenta que se redirija a un sitio malintencionado.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-143">The `IsLocalUrl` method protects users from being inadvertently redirected to a malicious site.</span></span> <span data-ttu-id="dfaf7-144">Puede registrar los detalles de la dirección URL que se proporcionó cuando se proporciona una dirección URL de no local en una situación donde se espera una dirección URL local.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-144">You can log the details of the URL that was provided when a non-local URL is supplied in a situation where you expected a local URL.</span></span> <span data-ttu-id="dfaf7-145">Registro redirigir direcciones URL pueden ayudar a diagnosticar los ataques de redireccionamiento.</span><span class="sxs-lookup"><span data-stu-id="dfaf7-145">Logging redirect URLs may help in diagnosing redirection attacks.</span></span>
