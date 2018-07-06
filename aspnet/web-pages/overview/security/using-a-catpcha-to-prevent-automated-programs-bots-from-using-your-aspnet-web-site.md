---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Usar un CAPTCHA para evitar que usen su Razor de ASP.NET Web Bots) sitio | Microsoft Docs
author: microsoft
description: En este artículo se explica cómo usar ReCaptcha (una medida de seguridad) para evitar que programas automatizados (bots) realizar tareas en una ASP.NET Web Pages (Razor) se...
ms.author: aspnetcontent
ms.date: 05/21/2012
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: f67eb60c23e0eec46089ceea9b04779492dfa15e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803076"
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a><span data-ttu-id="1155e-103">Sitio de usar un CAPTCHA para evitar que los Bots usen su Razor de ASP.NET Web)</span><span class="sxs-lookup"><span data-stu-id="1155e-103">Using a CAPTCHA to Prevent Bots from Using Your ASP.NET Web Razor) Site</span></span>
====================
<span data-ttu-id="1155e-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="1155e-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="1155e-105">En este artículo se explica cómo usar ReCaptcha (una medida de seguridad) para evitar que programas automatizados (bots) realizar tareas en un sitio Web de ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="1155e-105">This article explains how to use ReCaptcha (a security measure) to prevent automated programs (bots) from performing tasks in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="1155e-106">**Lo que aprenderá:**</span><span class="sxs-lookup"><span data-stu-id="1155e-106">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="1155e-107">Cómo agregar una prueba CAPTCHA para su sitio.</span><span class="sxs-lookup"><span data-stu-id="1155e-107">How to add a CAPTCHA test to your site.</span></span>
> 
> <span data-ttu-id="1155e-108">Estas son las características ASP.NET incorporadas en el artículo:</span><span class="sxs-lookup"><span data-stu-id="1155e-108">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="1155e-109">El `ReCaptcha` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="1155e-109">The `ReCaptcha` helper.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="1155e-110">La información de este artículo se aplica a las páginas Web de ASP.NET 1.0 y Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="1155e-110">The information in this article applies to ASP.NET Web Pages 1.0 and Web Pages 2.</span></span>


## <a name="about-captchas"></a><span data-ttu-id="1155e-111">Acerca de CAPTCHAs</span><span class="sxs-lookup"><span data-stu-id="1155e-111">About CAPTCHAs</span></span>

<span data-ttu-id="1155e-112">Cada vez que permite a los usuarios registrarse en su sitio, o incluso simplemente escriba un nombre y la dirección URL (como para un comentario en el blog), es posible que obtenga una avalancha de nombres falsos.</span><span class="sxs-lookup"><span data-stu-id="1155e-112">Any time you let people register in your site, or even just enter a name and URL (like for a blog comment), you might get a flood of fake names.</span></span> <span data-ttu-id="1155e-113">Esto se dejan a menudo por programas automatizados (bots) que intentan salir de las direcciones URL en cada sitio Web que puede encontrar.</span><span class="sxs-lookup"><span data-stu-id="1155e-113">These are often left by automated programs (bots) that try to leave URLs in every website they can find.</span></span> <span data-ttu-id="1155e-114">(Un motivo común es registrar las direcciones URL de productos para su venta).</span><span class="sxs-lookup"><span data-stu-id="1155e-114">(A common motivation is to post the URLs of products for sale.)</span></span>

<span data-ttu-id="1155e-115">Puede ayudar a asegurarse de que un usuario es una persona real y no un programa del equipo mediante el uso de un *CAPTCHA* para validar a los usuarios cuando registre o en caso contrario, escriba su nombre y el sitio.</span><span class="sxs-lookup"><span data-stu-id="1155e-115">You can help make sure that a user is real person and not a computer program by using a *CAPTCHA* to validate users when they register or otherwise enter their name and site.</span></span> <span data-ttu-id="1155e-116">CAPTCHA significa pruebas completamente automatizadas Turing pública indicar a los equipos y los seres humanos separados.</span><span class="sxs-lookup"><span data-stu-id="1155e-116">CAPTCHA stands for Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="1155e-117">Un CAPTCHA es un *desafío / respuesta* prueba en el que se pregunta al usuario hacer algo que sea fácil de una persona pero difícil para un programa automatizado hacer.</span><span class="sxs-lookup"><span data-stu-id="1155e-117">A CAPTCHA is a *challenge-response* test in which the user is asked to do something that is easy for a person to do but hard for an automated program to do.</span></span> <span data-ttu-id="1155e-118">El tipo más común de CAPTCHA es uno donde ver algunas letras distorsionada y se le pide que escribirlas.</span><span class="sxs-lookup"><span data-stu-id="1155e-118">The most common type of CAPTCHA is one where you see some distorted letters and are asked to type them.</span></span> <span data-ttu-id="1155e-119">(La distorsión se supone que resulte difícil para los bots descifrar las letras).</span><span class="sxs-lookup"><span data-stu-id="1155e-119">(The distortion is supposed to make it hard for bots to decipher the letters.)</span></span>

## <a name="adding-a-recaptcha-test"></a><span data-ttu-id="1155e-120">Agregar una prueba de ReCaptcha</span><span class="sxs-lookup"><span data-stu-id="1155e-120">Adding a ReCaptcha Test</span></span>

<span data-ttu-id="1155e-121">En las páginas ASP.NET, puede usar el `ReCaptcha` auxiliar para representar una prueba CAPTCHA que se basa en el servicio de ReCaptcha ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="1155e-121">In ASP.NET pages, you can use the `ReCaptcha` helper to render a CAPTCHA test that is based on the ReCaptcha service ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="1155e-122">El `ReCaptcha` auxiliar muestra una imagen de dos palabras distorsionadas que los usuarios deben especificar correctamente antes de que se valide la página.</span><span class="sxs-lookup"><span data-stu-id="1155e-122">The `ReCaptcha` helper displays an image of two distorted words that users have to enter correctly before the page is validated.</span></span> <span data-ttu-id="1155e-123">El servicio ReCaptcha.Net se valida la respuesta del usuario.</span><span class="sxs-lookup"><span data-stu-id="1155e-123">The user response is validated by the ReCaptcha.Net service.</span></span>

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. <span data-ttu-id="1155e-124">Registrar su sitio Web en ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="1155e-124">Register your website at ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="1155e-125">Cuando haya completado el registro, obtendrá una clave pública y una clave privada.</span><span class="sxs-lookup"><span data-stu-id="1155e-125">When you've completed registration, you'll get a public key and a private key.</span></span>
2. <span data-ttu-id="1155e-126">Agregue la ASP.NET Web Helpers Library a su sitio Web, como se describe en [las aplicaciones auxiliares de la instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.</span><span class="sxs-lookup"><span data-stu-id="1155e-126">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
3. <span data-ttu-id="1155e-127">Si aún no tiene un  *\_AppStart.cshtml* , en la carpeta raíz de un sitio Web, cree un archivo denominado  *\_AppStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1155e-127">If you don't already have a *\_AppStart.cshtml* file, in the root folder of a website create a file named *\_AppStart.cshtml*.</span></span>
4. <span data-ttu-id="1155e-128">Agregue el siguiente `Recaptcha` configuración de aplicación auxiliar en el  *\_AppStart.cshtml* archivo:</span><span class="sxs-lookup"><span data-stu-id="1155e-128">Add the following `Recaptcha` helper settings in the *\_AppStart.cshtml* file:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. <span data-ttu-id="1155e-129">Establecer el `PublicKey` y `PrivateKey` propiedades mediante sus propias claves públicas y privadas.</span><span class="sxs-lookup"><span data-stu-id="1155e-129">Set the `PublicKey` and `PrivateKey` properties using your own public and private keys.</span></span>
6. <span data-ttu-id="1155e-130">Guardar el  *\_AppStart.cshtml* de archivo y ciérrelo.</span><span class="sxs-lookup"><span data-stu-id="1155e-130">Save the *\_AppStart.cshtml* file and close it.</span></span>
7. <span data-ttu-id="1155e-131">En la carpeta raíz de un sitio Web, cree la nueva página denominada *Recaptcha.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1155e-131">In the root folder of a website, create new page named *Recaptcha.cshtml*.</span></span>
8. <span data-ttu-id="1155e-132">Reemplace el contenido existente por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1155e-132">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. <span data-ttu-id="1155e-133">Ejecute el *Recaptcha.cshtml* página en un explorador.</span><span class="sxs-lookup"><span data-stu-id="1155e-133">Run the *Recaptcha.cshtml* page in a browser.</span></span> <span data-ttu-id="1155e-134">Si el `PrivateKey` valor es válido, la página muestra el control de ReCaptcha y un botón.</span><span class="sxs-lookup"><span data-stu-id="1155e-134">If the `PrivateKey` value is valid, the page displays the ReCaptcha control and a button.</span></span> <span data-ttu-id="1155e-135">Si no se hubiera establecido las claves de forma global en  *\_AppStart.html*, la página mostrará un error.</span><span class="sxs-lookup"><span data-stu-id="1155e-135">If you had not set the keys globally in *\_AppStart.html*, the page would display an error.</span></span> 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. <span data-ttu-id="1155e-136">Escriba las palabras de la prueba.</span><span class="sxs-lookup"><span data-stu-id="1155e-136">Enter the words for the test.</span></span> <span data-ttu-id="1155e-137">Si se pasa la prueba de ReCaptcha, verá un mensaje a tal efecto.</span><span class="sxs-lookup"><span data-stu-id="1155e-137">If you pass the ReCaptcha test, you see a message to that effect.</span></span> <span data-ttu-id="1155e-138">En caso contrario, verá un mensaje de error y se vuelve a mostrar el control de ReCaptcha.</span><span class="sxs-lookup"><span data-stu-id="1155e-138">Otherwise you see an error message and the ReCaptcha control is redisplayed.</span></span>

> [!NOTE]
> <span data-ttu-id="1155e-139">Si el equipo está en un dominio que utiliza un servidor proxy, deberá configurar el `defaultproxy` elemento de la *Web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="1155e-139">If your computer is on a domain that uses proxy server, you might need to configure the `defaultproxy` element of the *Web.config* file.</span></span> <span data-ttu-id="1155e-140">El ejemplo siguiente se muestra un *Web.config* archivo con el `defaultproxy` elemento configurado para habilitar el servicio de ReCaptcha para que funcione.</span><span class="sxs-lookup"><span data-stu-id="1155e-140">The following example shows a *Web.config* file with the `defaultproxy` element configured to enable the ReCaptcha service to work.</span></span>
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="1155e-141">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="1155e-141">Additional Resources</span></span>


- [<span data-ttu-id="1155e-142">Personalizar el comportamiento de todo el sitio para sitios de ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="1155e-142">Customizing Site-Wide Behavior for ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="1155e-143">Sitio de ReCaptcha</span><span class="sxs-lookup"><span data-stu-id="1155e-143">ReCaptcha site</span></span>](https://www.google.com/recaptcha)
