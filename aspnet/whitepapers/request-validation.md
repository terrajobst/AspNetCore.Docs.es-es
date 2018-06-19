---
uid: whitepapers/request-validation
title: Solicitar validación - prevención de ataques de Script | Documentos de Microsoft
author: rick-anderson
description: Este documento describe la característica de validación de solicitud de ASP.NET donde, de forma predeterminada, la aplicación no puede procesar enviar contenido de HTML sin codificar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 0b24fe2193d2c7a858667505bad9ed0b1d70a328
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
ms.locfileid: "28883560"
---
<a name="request-validation---preventing-script-attacks"></a><span data-ttu-id="aa8d2-103">La validación - prevención de ataques de secuencia de comandos de solicitudes</span><span class="sxs-lookup"><span data-stu-id="aa8d2-103">Request Validation - Preventing Script Attacks</span></span>
====================
> <span data-ttu-id="aa8d2-104">Este documento describe la característica de validación de solicitud de ASP.NET donde, de forma predeterminada, la aplicación no puede procesar el contenido HTML sin codificar enviado al servidor.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-104">This paper describes the request validation feature of ASP.NET where, by default, the application is prevented from processing unencoded HTML content submitted to the server.</span></span> <span data-ttu-id="aa8d2-105">Esta característica de validación de solicitud puede estar deshabilitada cuando la aplicación se ha diseñado para procesar datos HTML de forma segura.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-105">This request validation feature can be disabled when the application has been designed to safely process HTML data.</span></span>
> 
> <span data-ttu-id="aa8d2-106">Se aplica a ASP.NET 1.1 y ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-106">Applies to ASP.NET 1.1 and ASP.NET 2.0.</span></span>


<span data-ttu-id="aa8d2-107">Validación de solicitudes, una característica de ASP.NET desde la versión 1.1, impide que el servidor acepte contenido HTML sin codificar que lo contiene.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-107">Request validation, a feature of ASP.NET since version 1.1, prevents the server from accepting content containing un-encoded HTML.</span></span> <span data-ttu-id="aa8d2-108">Esta característica está diseñada para ayudar a evitar algunos ataques de inyección de script mediante el cual código de script de cliente o HTML puede ser sin saberlo enviada a un servidor, almacena y, a continuación, se presentan a otros usuarios.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-108">This feature is designed to help prevent some script-injection attacks whereby client script code or HTML can be unknowingly submitted to a server, stored, and then presented to other users.</span></span> <span data-ttu-id="aa8d2-109">Se recomienda encarecidamente seguir que validar todos los datos recibidos y codificación HTML que cuando corresponda.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-109">We still strongly recommend that you validate all input data and HTML encode it when appropriate.</span></span>

<span data-ttu-id="aa8d2-110">Por ejemplo, cree una página Web que solicita la dirección de correo electrónico de un usuario y, a continuación, almacena ese correo electrónico en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-110">For example, you create a Web page that requests a user's email address and then stores that email address in a database.</span></span> <span data-ttu-id="aa8d2-111">Si el usuario escribe &lt;SCRIPT&gt;alerta ("Hola desde un script")&lt;/SCRIPT&gt; en lugar de una dirección de correo electrónico válida, cuando esos datos se presentación, esta secuencia de comandos se puede ejecutar si el contenido no se codificó correctamente.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-111">If the user enters &lt;SCRIPT&gt;alert("hello from script")&lt;/SCRIPT&gt; instead of a valid email address, when that data is presented, this script can be executed if the content was not properly encoded.</span></span> <span data-ttu-id="aa8d2-112">La característica de validación de solicitud de ASP.NET evita que esto suceda.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-112">The request validation feature of ASP.NET prevents this from happening.</span></span>

## <a name="why-this-feature-is-useful"></a><span data-ttu-id="aa8d2-113">¿Por qué esta característica es útil</span><span class="sxs-lookup"><span data-stu-id="aa8d2-113">Why this feature is useful</span></span>

<span data-ttu-id="aa8d2-114">Muchos sitios no son conscientes de que estén abiertos a ataques de inyección de script simple.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-114">Many sites are not aware that they are open to simple script injection attacks.</span></span> <span data-ttu-id="aa8d2-115">Si el propósito de este tipo de ataques es a estropear el sitio mostrando HTML o potencialmente ejecutar script de cliente para redirigir al usuario al sitio de un intruso, ataques de inyección de script son un problema que los desarrolladores Web deben lidiar con.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-115">Whether the purpose of these attacks is to deface the site by displaying HTML, or to potentially execute client script to redirect the user to a hacker's site, script injection attacks are a problem that Web developers must contend with.</span></span>

<span data-ttu-id="aa8d2-116">Ataques de inyección de script son un problema de todos los desarrolladores web, independientemente de que están utilizando ASP.NET, ASP u otras tecnologías de desarrollo de web.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-116">Script injection attacks are a concern of all web developers, whether they are using ASP.NET, ASP, or other web development technologies.</span></span>

<span data-ttu-id="aa8d2-117">La característica de validación de solicitud ASP.NET proactivamente impide estos ataques al no permitir el contenido sobre HTML sin codificar ser procesados por el servidor a menos que el desarrollador decide permitir que el contenido.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-117">The ASP.NET request validation feature proactively prevents these attacks by not allowing unencoded HTML content to be processed by the server unless the developer decides to allow that content.</span></span>

## <a name="what-to-expect-error-page"></a><span data-ttu-id="aa8d2-118">Qué esperar: página de Error</span><span class="sxs-lookup"><span data-stu-id="aa8d2-118">What to expect: Error Page</span></span>

<span data-ttu-id="aa8d2-119">La captura de pantalla siguiente muestra ejemplos de código de ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="aa8d2-119">The screen shot below shows some sample ASP.NET code:</span></span>

![](request-validation/_static/image1.png)

<span data-ttu-id="aa8d2-120">Resultados de este código se ejecuta en una página sencilla que le permite escribir texto en el cuadro de texto, haga clic en el botón y muestra el texto en el control de etiqueta:</span><span class="sxs-lookup"><span data-stu-id="aa8d2-120">Running this code results in a simple page that allows you to enter some text in the textbox, click the button, and display the text in the label control:</span></span>

![](request-validation/_static/image2.png)

<span data-ttu-id="aa8d2-121">Sin embargo, eran JavaScript, como `<script>alert("hello!")</script>` que se especifique y envían obtendríamos una excepción:</span><span class="sxs-lookup"><span data-stu-id="aa8d2-121">However, were JavaScript, such as `<script>alert("hello!")</script>` to be entered and submitted we would get an exception:</span></span>

![](request-validation/_static/image3.png)

<span data-ttu-id="aa8d2-122">El mensaje de error indica que un 'potencialmente peligroso Request.Form detectó el valor' y se proporcionan más detalles en la descripción en cuanto a exactamente lo que ha sucedido y cómo cambiar el comportamiento.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-122">The error message states that a 'potentially dangerous Request.Form value was detected' and provides more details in the description as to exactly what occurred and how to change the behavior.</span></span> <span data-ttu-id="aa8d2-123">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="aa8d2-123">For example:</span></span>

<span data-ttu-id="aa8d2-124">Validación de solicitudes ha detectado un valor de entrada de cliente potencialmente peligrosos y ha anulado el procesamiento de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-124">Request validation has detected a potentially dangerous client input value, and processing of the request has been aborted.</span></span> <span data-ttu-id="aa8d2-125">Este valor puede indicar un intento de poner en peligro la seguridad de la aplicación, como un ataque XSS.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-125">This value may indicate an attempt to compromise the security of your application, such as a cross-site scripting attack.</span></span> <span data-ttu-id="aa8d2-126">Puede deshabilitar la validación de solicitudes estableciendo `validateRequest=false` en la directiva de página o en la sección de configuración.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-126">You can disable request validation by setting `validateRequest=false` in the Page directive or in the configuration section.</span></span> <span data-ttu-id="aa8d2-127">Sin embargo, se recomienda encarecidamente que su aplicación compruebe explícitamente todas las entradas en este caso.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-127">However, it is strongly recommended that your application explicitly check all inputs in this case.</span></span>

## <a name="disabling-request-validation-on-a-page"></a><span data-ttu-id="aa8d2-128">Cómo deshabilitar la validación de solicitud en una página</span><span class="sxs-lookup"><span data-stu-id="aa8d2-128">Disabling request validation on a page</span></span>

<span data-ttu-id="aa8d2-129">Para deshabilitar la validación de solicitud en una página que se debe establecer el `validateRequest` atributo de la directiva de página a `false`:</span><span class="sxs-lookup"><span data-stu-id="aa8d2-129">To disable request validation on a page you must set the `validateRequest` attribute of the Page directive to `false`:</span></span>

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> <span data-ttu-id="aa8d2-130">Cuando se deshabilita la validación de solicitudes, el contenido se puede enviar a una página; es responsabilidad del programador de la página para asegurarse de que el contenido se codifican o se procesan correctamente.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-130">When request validation is disabled, content can be submitted to a page; it is the responsibility of the page developer to ensure that content is properly encoded or processed.</span></span>

## <a name="disabling-request-validation-for-your-application"></a><span data-ttu-id="aa8d2-131">Cómo deshabilitar la validación de solicitud para la aplicación</span><span class="sxs-lookup"><span data-stu-id="aa8d2-131">Disabling request validation for your application</span></span>

<span data-ttu-id="aa8d2-132">Para deshabilitar la validación de solicitudes en la aplicación, debe modificar o crear un archivo Web.config para la aplicación y establezca el atributo validateRequest de la `<pages />` sección a `false`:</span><span class="sxs-lookup"><span data-stu-id="aa8d2-132">To disable request validation for your application, you must modify or create a Web.config file for your application and set the validateRequest attribute of the `<pages />` section to `false`:</span></span>

[!code-xml[Main](request-validation/samples/sample2.xml)]

<span data-ttu-id="aa8d2-133">Si desea deshabilitar la validación de solicitudes para todas las aplicaciones en el servidor, puede realizar esta modificación en el archivo Machine.config.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-133">If you wish to disable request validation for all applications on your server, you can make this modification to your Machine.config file.</span></span>

> [!CAUTION]
> <span data-ttu-id="aa8d2-134">Cuando se deshabilita la validación de solicitudes, el contenido se puede enviar a la aplicación; es responsabilidad del desarrollador de la aplicación para asegurarse de que el contenido se codifica correctamente o se procese.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-134">When request validation is disabled, content can be submitted to your application; it is the responsibility of the application developer to ensure that content is properly encoded or processed.</span></span>

<span data-ttu-id="aa8d2-135">El código siguiente se modifica para desactivar la validación de solicitud:</span><span class="sxs-lookup"><span data-stu-id="aa8d2-135">The code below is modified to turn off request validation:</span></span>

![](request-validation/_static/image4.png)

<span data-ttu-id="aa8d2-136">Ahora, si se ha escrito el código JavaScript siguiente en el cuadro de texto `<script>alert("hello!")</script>` el resultado sería:</span><span class="sxs-lookup"><span data-stu-id="aa8d2-136">Now if the following JavaScript was entered into the textbox `<script>alert("hello!")</script>` the result would be:</span></span>

![](request-validation/_static/image5.png)

<span data-ttu-id="aa8d2-137">Para evitar que esto ocurra, con validación de solicitudes está desactivada, se necesitamos, en HTML, codificar el contenido.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-137">To prevent this from happening, with request validation turned off, we need to HTML encode the content.</span></span>

## <a name="how-to-html-encode-content"></a><span data-ttu-id="aa8d2-138">Cómo a HTML codificar contenido</span><span class="sxs-lookup"><span data-stu-id="aa8d2-138">How to HTML encode content</span></span>

<span data-ttu-id="aa8d2-139">Si deshabilita la validación de solicitudes, es recomendable contenido codificar en HTML que se almacenará para un uso futuro.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-139">If you have disabled request validation, it is good practice to HTML-encode content that will be stored for future use.</span></span> <span data-ttu-id="aa8d2-140">Codificación HTML reemplazará automáticamente cualquier '&lt;'o'&gt;' (junto con otros símbolos) con su correspondiente HTML representación con codificación.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-140">HTML encoding will automatically replace any ‘&lt;' or ‘&gt;' (together with several other symbols) with their corresponding HTML encoded representation.</span></span> <span data-ttu-id="aa8d2-141">Por ejemplo, '&lt;'se reemplaza por'&amp;lt;' y '&gt;'se reemplaza por'&amp;gt;'.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-141">For example, ‘&lt;' is replaced by ‘&amp;lt;' and ‘&gt;' is replaced by ‘&amp;gt;'.</span></span> <span data-ttu-id="aa8d2-142">Los exploradores utilizan estos códigos especiales para mostrar el '&lt;'o'&gt;' en el explorador.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-142">Browsers use these special codes to display the ‘&lt;' or ‘&gt;' in the browser.</span></span>

<span data-ttu-id="aa8d2-143">Contenido puede ser fácilmente codificada en HTML en el servidor mediante el `Server.HtmlEncode(string)` API.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-143">Content can be easily HTML-encoded on the server using the `Server.HtmlEncode(string)` API.</span></span> <span data-ttu-id="aa8d2-144">Contenido también puede ser fácilmente descodificados para HTML, es decir, vuelto a usar el estándar HTML la `Server.HtmlDecode(string)` método.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-144">Content can also be easily HTML-decoded, that is, reverted back to standard HTML using the `Server.HtmlDecode(string)` method.</span></span>

![](request-validation/_static/image6.png)

<span data-ttu-id="aa8d2-145">Lo que:</span><span class="sxs-lookup"><span data-stu-id="aa8d2-145">Resulting in:</span></span>

![](request-validation/_static/image7.png)
