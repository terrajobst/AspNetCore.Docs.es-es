---
title: Aplicar una directiva de seguridad de contenido para ASP.NET Core Blazor
author: guardrex
description: Aprenda a usar una directiva de seguridad de contenido (CSP) con ASP.NET Core Blazor aplicaciones para ayudar a protegerse frente a ataques de scripting entre sitios (XSS).
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/02/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/content-security-policy
ms.openlocfilehash: 1cfebf7b3d3bbb98a671b6f2db7c6518cda74b65
ms.sourcegitcommit: 51c86c003ab5436598dbc42f26ea4a83a795fd6e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2020
ms.locfileid: "78964539"
---
# <a name="enforce-a-content-security-policy-for-aspnet-core-opno-locblazor"></a><span data-ttu-id="fb8ca-103">Aplicar una directiva de seguridad de contenido para ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="fb8ca-103">Enforce a Content Security Policy for ASP.NET Core Blazor</span></span>

<span data-ttu-id="fb8ca-104">Por [Javier Calvarro Nelson](https://github.com/javiercn) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fb8ca-104">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="fb8ca-105">El [scripting entre sitios (XSS)](xref:security/cross-site-scripting) es una vulnerabilidad de seguridad en la que un atacante coloca uno o varios scripts del lado cliente malintencionados en el contenido representado de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-105">[Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) is a security vulnerability where an attacker places one or more malicious client-side scripts into an app's rendered content.</span></span> <span data-ttu-id="fb8ca-106">Una directiva de seguridad de contenido (CSP) ayuda a protegerse contra ataques XSS informando al explorador de válido:</span><span class="sxs-lookup"><span data-stu-id="fb8ca-106">A Content Security Policy (CSP) helps protect against XSS attacks by informing the browser of valid:</span></span>

* <span data-ttu-id="fb8ca-107">Orígenes para contenido cargado, incluidos scripts, hojas de estilo e imágenes.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-107">Sources for loaded content, including scripts, stylesheets, and images.</span></span>
* <span data-ttu-id="fb8ca-108">Acciones realizadas por una página que especifican los destinos de URL permitidos de los formularios.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-108">Actions taken by a page, specifying permitted URL targets of forms.</span></span>
* <span data-ttu-id="fb8ca-109">Complementos que se pueden cargar.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-109">Plugins that can be loaded.</span></span>

<span data-ttu-id="fb8ca-110">Para aplicar un CSP a una aplicación, el desarrollador especifica varias *directivas* de seguridad de contenido de CSP en uno o varios encabezados de `Content-Security-Policy` o etiquetas de `<meta>`.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-110">To apply a CSP to an app, the developer specifies several CSP content security *directives* in one or more `Content-Security-Policy` headers or `<meta>` tags.</span></span>

<span data-ttu-id="fb8ca-111">El explorador evalúa las directivas mientras se carga una página.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-111">Policies are evaluated by the browser while a page is loading.</span></span> <span data-ttu-id="fb8ca-112">El explorador inspecciona los orígenes de la página y determina si cumplen los requisitos de las directivas de seguridad de contenido.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-112">The browser inspects the page's sources and determines if they meet the requirements of the content security directives.</span></span> <span data-ttu-id="fb8ca-113">Cuando no se cumplen las directivas de directiva para un recurso, el explorador no carga el recurso.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-113">When policy directives aren't met for a resource, the browser doesn't load the resource.</span></span> <span data-ttu-id="fb8ca-114">Por ejemplo, considere una directiva que no permita scripts de terceros.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-114">For example, consider a policy that doesn't allow third-party scripts.</span></span> <span data-ttu-id="fb8ca-115">Cuando una página contiene una etiqueta de `<script>` con un origen de terceros en el atributo `src`, el explorador evita que se cargue el script.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-115">When a page contains a `<script>` tag with a third-party origin in the `src` attribute, the browser prevents the script from loading.</span></span>

<span data-ttu-id="fb8ca-116">CSP se admite en la mayoría de los exploradores de escritorio y móviles modernos, como Chrome, Edge, Firefox, opera y Safari.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-116">CSP is supported in most modern desktop and mobile browsers, including Chrome, Edge, Firefox, Opera, and Safari.</span></span> <span data-ttu-id="fb8ca-117">CSP se recomienda para las aplicaciones de Blazor.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-117">CSP is recommended for Blazor apps.</span></span>

## <a name="policy-directives"></a><span data-ttu-id="fb8ca-118">Directivas de Directiva</span><span class="sxs-lookup"><span data-stu-id="fb8ca-118">Policy directives</span></span>

<span data-ttu-id="fb8ca-119">Como mínimo, especifique las siguientes directivas y orígenes para Blazor aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-119">Minimally, specify the following directives and sources for Blazor apps.</span></span> <span data-ttu-id="fb8ca-120">Agregue directivas y orígenes adicionales según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-120">Add additional directives and sources as needed.</span></span> <span data-ttu-id="fb8ca-121">Las directivas siguientes se usan en la sección [aplicar la Directiva](#apply-the-policy) de este artículo, donde se proporcionan las directivas de seguridad de ejemplo para Blazor webassembly y Blazor Server:</span><span class="sxs-lookup"><span data-stu-id="fb8ca-121">The following directives are used in the [Apply the policy](#apply-the-policy) section of this article, where example security policies for Blazor WebAssembly and Blazor Server are provided:</span></span>

* <span data-ttu-id="fb8ca-122">el [URI de base](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/base-uri) &ndash; restringe las direcciones URL de la etiqueta de `<base>` de una página.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-122">[base-uri](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/base-uri) &ndash; Restricts the URLs for a page's `<base>` tag.</span></span> <span data-ttu-id="fb8ca-123">Especifique `self` para indicar que el origen de la aplicación, incluido el esquema y el número de puerto, es un origen válido.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-123">Specify `self` to indicate that the app's origin, including the scheme and port number, is a valid source.</span></span>
* <span data-ttu-id="fb8ca-124">[Block-All-Mixed-content](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/block-all-mixed-content) &ndash; impide la carga de contenido http y https mixto.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-124">[block-all-mixed-content](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/block-all-mixed-content) &ndash; Prevents loading mixed HTTP and HTTPS content.</span></span>
* <span data-ttu-id="fb8ca-125">[default-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/default-src) &ndash; indica una reserva para las directivas de código fuente que no se especifican explícitamente en la Directiva.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-125">[default-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/default-src) &ndash; Indicates a fallback for source directives that aren't explicitly specified by the policy.</span></span> <span data-ttu-id="fb8ca-126">Especifique `self` para indicar que el origen de la aplicación, incluido el esquema y el número de puerto, es un origen válido.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-126">Specify `self` to indicate that the app's origin, including the scheme and port number, is a valid source.</span></span>
* <span data-ttu-id="fb8ca-127">[img-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/img-src) &ndash; indica orígenes válidos para las imágenes.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-127">[img-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/img-src) &ndash; Indicates valid sources for images.</span></span>
  * <span data-ttu-id="fb8ca-128">Especifique `data:` para permitir la carga de imágenes de `data:` direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-128">Specify `data:` to permit loading images from `data:` URLs.</span></span>
  * <span data-ttu-id="fb8ca-129">Especifique `https:` para permitir la carga de imágenes desde puntos de conexión HTTPS.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-129">Specify `https:` to permit loading images from HTTPS endpoints.</span></span>
* <span data-ttu-id="fb8ca-130">[Object-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/object-src) &ndash; indica orígenes válidos para las etiquetas `<object>`, `<embed>`y `<applet>`.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-130">[object-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/object-src) &ndash; Indicates valid sources for the `<object>`, `<embed>`, and `<applet>` tags.</span></span> <span data-ttu-id="fb8ca-131">Especifique `none` para evitar todos los orígenes de direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-131">Specify `none` to prevent all URL sources.</span></span>
* <span data-ttu-id="fb8ca-132">[script-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/script-src) &ndash; indica orígenes válidos para scripts.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-132">[script-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/script-src) &ndash; Indicates valid sources for scripts.</span></span>
  * <span data-ttu-id="fb8ca-133">Especifique el origen de host de `https://stackpath.bootstrapcdn.com/` para los scripts de arranque.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-133">Specify the `https://stackpath.bootstrapcdn.com/` host source for Bootstrap scripts.</span></span>
  * <span data-ttu-id="fb8ca-134">Especifique `self` para indicar que el origen de la aplicación, incluido el esquema y el número de puerto, es un origen válido.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-134">Specify `self` to indicate that the app's origin, including the scheme and port number, is a valid source.</span></span>
  * <span data-ttu-id="fb8ca-135">En una aplicación Blazor webassembly:</span><span class="sxs-lookup"><span data-stu-id="fb8ca-135">In a Blazor WebAssembly app:</span></span>
    * <span data-ttu-id="fb8ca-136">Especifique los siguientes valores hash para permitir la carga de los scripts en línea Blazor webassembly necesarios:</span><span class="sxs-lookup"><span data-stu-id="fb8ca-136">Specify the following hashes to permit the required Blazor WebAssembly inline scripts to load:</span></span>
      * `sha256-v8ZC9OgMhcnEQ/Me77/R9TlJfzOBqrMTW8e1KuqLaqc=`
      * `sha256-If//FtbPc03afjLezvWHnC3Nbu4fDM04IIzkPaf3pH0=`
      * `sha256-v8v3RKRPmN4odZ1CWM5gw80QKPCCWMcpNeOmimNL2AA=`
    * <span data-ttu-id="fb8ca-137">Especifique `unsafe-eval` para usar `eval()` y métodos para crear código a partir de cadenas.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-137">Specify `unsafe-eval` to use `eval()` and methods for creating code from strings.</span></span>
  * <span data-ttu-id="fb8ca-138">En una aplicación de Blazor Server, especifique el hash de `sha256-34WLX60Tw3aG6hylk0plKbZZFXCuepeQ6Hu7OqRf8PI=` para el script en línea que realiza la detección de reserva para las hojas de estilos.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-138">In a Blazor Server app, specify the `sha256-34WLX60Tw3aG6hylk0plKbZZFXCuepeQ6Hu7OqRf8PI=` hash for the inline script that performs fallback detection for stylesheets.</span></span>
* <span data-ttu-id="fb8ca-139">[Style: src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/style-src) &ndash; indica orígenes válidos para las hojas de estilos.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-139">[style-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/style-src) &ndash; Indicates valid sources for stylesheets.</span></span>
  * <span data-ttu-id="fb8ca-140">Especifique el origen de host de `https://stackpath.bootstrapcdn.com/` para las hojas de estilos de arranque.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-140">Specify the `https://stackpath.bootstrapcdn.com/` host source for Bootstrap stylesheets.</span></span>
  * <span data-ttu-id="fb8ca-141">Especifique `self` para indicar que el origen de la aplicación, incluido el esquema y el número de puerto, es un origen válido.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-141">Specify `self` to indicate that the app's origin, including the scheme and port number, is a valid source.</span></span>
  * <span data-ttu-id="fb8ca-142">Especifique `unsafe-inline` para permitir el uso de estilos alineados.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-142">Specify `unsafe-inline` to allow the use of inline styles.</span></span> <span data-ttu-id="fb8ca-143">La declaración en línea es necesaria para que la interfaz de usuario de Blazor aplicaciones de servidor para volver a conectar el cliente y el servidor después de la solicitud inicial.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-143">The inline declaration is required for the UI in Blazor Server apps for reconnecting the client and server after the initial request.</span></span> <span data-ttu-id="fb8ca-144">En una versión futura, es posible que se eliminen los estilos en línea para que ya no sea necesario `unsafe-inline`.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-144">In a future release, inline styling might be removed so that `unsafe-inline` is no longer required.</span></span>
* <span data-ttu-id="fb8ca-145">[upgrade-insecure-requests](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/upgrade-insecure-requests) &ndash; indica que las direcciones URL de contenido de orígenes no seguros (http) deben adquirirse de forma segura a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-145">[upgrade-insecure-requests](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/upgrade-insecure-requests) &ndash; Indicates that content URLs from insecure (HTTP) sources should be acquired securely over HTTPS.</span></span>

<span data-ttu-id="fb8ca-146">Todos los exploradores, excepto Microsoft Internet Explorer, admiten las directivas anteriores.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-146">The preceding directives are supported by all browsers except Microsoft Internet Explorer.</span></span>

<span data-ttu-id="fb8ca-147">Para obtener los algoritmos hash SHA para scripts en línea adicionales:</span><span class="sxs-lookup"><span data-stu-id="fb8ca-147">To obtain SHA hashes for additional inline scripts:</span></span>

* <span data-ttu-id="fb8ca-148">Aplique el CSP mostrado en la sección [aplicar la Directiva](#apply-the-policy) .</span><span class="sxs-lookup"><span data-stu-id="fb8ca-148">Apply the CSP shown in the [Apply the policy](#apply-the-policy) section.</span></span>
* <span data-ttu-id="fb8ca-149">Acceda a la consola de herramientas de desarrollo del explorador mientras ejecuta la aplicación localmente.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-149">Access the browser's developer tools console while running the app locally.</span></span> <span data-ttu-id="fb8ca-150">El explorador calcula y muestra los valores hash para los scripts bloqueados cuando está presente un encabezado CSP o una etiqueta de `meta`.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-150">The browser calculates and displays hashes for blocked scripts when a CSP header or `meta` tag is present.</span></span>
* <span data-ttu-id="fb8ca-151">Copie los valores hash proporcionados por el explorador en los orígenes de `script-src`.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-151">Copy the hashes provided by the browser to the `script-src` sources.</span></span> <span data-ttu-id="fb8ca-152">Use comillas simples alrededor de cada hash.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-152">Use single quotes around each hash.</span></span>

<span data-ttu-id="fb8ca-153">Para obtener una matriz de compatibilidad con exploradores de nivel 2 de directiva de seguridad de contenido, consulte [puedo usar: Directiva de seguridad de contenido nivel 2](https://www.caniuse.com/#feat=contentsecuritypolicy2).</span><span class="sxs-lookup"><span data-stu-id="fb8ca-153">For a Content Security Policy Level 2 browser support matrix, see [Can I use: Content Security Policy Level 2](https://www.caniuse.com/#feat=contentsecuritypolicy2).</span></span>

## <a name="apply-the-policy"></a><span data-ttu-id="fb8ca-154">Aplicación de la directiva</span><span class="sxs-lookup"><span data-stu-id="fb8ca-154">Apply the policy</span></span>

<span data-ttu-id="fb8ca-155">Use una etiqueta de `<meta>` para aplicar la Directiva:</span><span class="sxs-lookup"><span data-stu-id="fb8ca-155">Use a `<meta>` tag to apply the policy:</span></span>

* <span data-ttu-id="fb8ca-156">Establezca el valor del atributo `http-equiv` en `Content-Security-Policy`.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-156">Set the value of the `http-equiv` attribute to `Content-Security-Policy`.</span></span>
* <span data-ttu-id="fb8ca-157">Coloque las directivas en el valor del atributo `content`.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-157">Place the directives in the `content` attribute value.</span></span> <span data-ttu-id="fb8ca-158">Separe las directivas con un punto y coma (`;`).</span><span class="sxs-lookup"><span data-stu-id="fb8ca-158">Separate directives with a semicolon (`;`).</span></span>
* <span data-ttu-id="fb8ca-159">Coloque siempre el `meta` etiqueta en el contenido de la `<head>`.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-159">Always place the `meta` tag in the `<head>` content.</span></span>

<span data-ttu-id="fb8ca-160">En las secciones siguientes se muestran las directivas de ejemplo para Blazor webassembly y Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-160">The following sections show example policies for Blazor WebAssembly and Blazor Server.</span></span> <span data-ttu-id="fb8ca-161">En estos ejemplos se tiene una versión de este artículo para cada versión de Blazor.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-161">These examples are versioned with this article for each release of Blazor.</span></span> <span data-ttu-id="fb8ca-162">Para usar una versión adecuada para su versión, seleccione la versión del documento con el selector desplegable **versión** en esta página web.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-162">To use a version appropriate for your release, select the document version with the **Version** drop down selector on this webpage.</span></span>

### <a name="opno-locblazor-webassembly"></a>Blazor<span data-ttu-id="fb8ca-163"> WebAssembly</span><span class="sxs-lookup"><span data-stu-id="fb8ca-163"> WebAssembly</span></span>

<span data-ttu-id="fb8ca-164">En el `<head>` contenido de la página host de *wwwroot/index.html* , aplique las directivas descritas en la sección [directivas de directiva](#policy-directives) :</span><span class="sxs-lookup"><span data-stu-id="fb8ca-164">In the `<head>` content of the *wwwroot/index.html* host page, apply the directives described in the [Policy directives](#policy-directives) section:</span></span>

```html
<meta http-equiv="Content-Security-Policy" 
      content="base-uri 'self';
               block-all-mixed-content;
               default-src 'self';
               img-src data: https:;
               object-src 'none';
               script-src https://stackpath.bootstrapcdn.com/ 
                          'self' 
                          'sha256-v8ZC9OgMhcnEQ/Me77/R9TlJfzOBqrMTW8e1KuqLaqc=' 
                          'sha256-If//FtbPc03afjLezvWHnC3Nbu4fDM04IIzkPaf3pH0=' 
                          'sha256-v8v3RKRPmN4odZ1CWM5gw80QKPCCWMcpNeOmimNL2AA=' 
                          'unsafe-eval';
               style-src https://stackpath.bootstrapcdn.com/
                         'self'
                         'unsafe-inline';
               upgrade-insecure-requests;">
```

### <a name="opno-locblazor-server"></a><span data-ttu-id="fb8ca-165">Servidor de Blazor</span><span class="sxs-lookup"><span data-stu-id="fb8ca-165">Blazor Server</span></span>

<span data-ttu-id="fb8ca-166">En el `<head>` contenido de la página de host *pages/_Host. cshtml* , aplique las directivas descritas en la sección directivas de [Directiva](#policy-directives) :</span><span class="sxs-lookup"><span data-stu-id="fb8ca-166">In the `<head>` content of the *Pages/_Host.cshtml* host page, apply the directives described in the [Policy directives](#policy-directives) section:</span></span>

```cshtml
<meta http-equiv="Content-Security-Policy" 
      content="base-uri 'self';
               block-all-mixed-content;
               default-src 'self';
               img-src data: https:;
               object-src 'none';
               script-src https://stackpath.bootstrapcdn.com/ 
                          'self' 
                          'sha256-34WLX60Tw3aG6hylk0plKbZZFXCuepeQ6Hu7OqRf8PI=';
               style-src https://stackpath.bootstrapcdn.com/
                         'self' 
                         'unsafe-inline';
               upgrade-insecure-requests;">
```

## <a name="meta-tag-limitations"></a><span data-ttu-id="fb8ca-167">Limitaciones de la etiqueta meta</span><span class="sxs-lookup"><span data-stu-id="fb8ca-167">Meta tag limitations</span></span>

<span data-ttu-id="fb8ca-168">Una directiva de `<meta>` etiqueta no admite las siguientes directivas:</span><span class="sxs-lookup"><span data-stu-id="fb8ca-168">A `<meta>` tag policy doesn't support the following directives:</span></span>

* [<span data-ttu-id="fb8ca-169">antecesores de marco</span><span class="sxs-lookup"><span data-stu-id="fb8ca-169">frame-ancestors</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/frame-ancestors)
* [<span data-ttu-id="fb8ca-170">notificar a</span><span class="sxs-lookup"><span data-stu-id="fb8ca-170">report-to</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-to)
* [<span data-ttu-id="fb8ca-171">URI de informe</span><span class="sxs-lookup"><span data-stu-id="fb8ca-171">report-uri</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-uri)
* [<span data-ttu-id="fb8ca-172">aislados</span><span class="sxs-lookup"><span data-stu-id="fb8ca-172">sandbox</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/sandbox)

<span data-ttu-id="fb8ca-173">Para admitir las directivas anteriores, utilice un encabezado denominado `Content-Security-Policy`.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-173">To support the preceding directives, use a header named `Content-Security-Policy`.</span></span> <span data-ttu-id="fb8ca-174">La cadena de directiva es el valor del encabezado.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-174">The directive string is the header's value.</span></span>

## <a name="test-a-policy-and-receive-violation-reports"></a><span data-ttu-id="fb8ca-175">Probar una directiva y recibir informes de infracción</span><span class="sxs-lookup"><span data-stu-id="fb8ca-175">Test a policy and receive violation reports</span></span>

<span data-ttu-id="fb8ca-176">La prueba ayuda a confirmar que los scripts de terceros no se bloquean accidentalmente al crear una directiva inicial.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-176">Testing helps confirm that third-party scripts aren't inadvertently blocked when building an initial policy.</span></span>

<span data-ttu-id="fb8ca-177">Para probar una directiva a lo largo de un período de tiempo sin aplicar las directivas de Directiva, establezca `<meta>` el atributo `http-equiv` o el nombre de encabezado de una directiva basada en encabezado en `Content-Security-Policy-Report-Only`.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-177">To test a policy over a period of time without enforcing the policy directives, set the `<meta>` tag's `http-equiv` attribute or header name of a header-based policy to `Content-Security-Policy-Report-Only`.</span></span> <span data-ttu-id="fb8ca-178">Los informes de errores se envían como documentos JSON a una dirección URL especificada.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-178">Failure reports are sent as JSON documents to a specified URL.</span></span> <span data-ttu-id="fb8ca-179">Para obtener más información, vea [MDN web docs: Content-Security-Policy-Report-Report-Only](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy-Report-Only).</span><span class="sxs-lookup"><span data-stu-id="fb8ca-179">For more information, see [MDN web docs: Content-Security-Policy-Report-Only](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy-Report-Only).</span></span>

<span data-ttu-id="fb8ca-180">Para obtener información sobre las infracciones mientras una directiva está activa, consulte los siguientes artículos:</span><span class="sxs-lookup"><span data-stu-id="fb8ca-180">For reporting on violations while a policy is active, see the following articles:</span></span>

* [<span data-ttu-id="fb8ca-181">notificar a</span><span class="sxs-lookup"><span data-stu-id="fb8ca-181">report-to</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-to)
* [<span data-ttu-id="fb8ca-182">URI de informe</span><span class="sxs-lookup"><span data-stu-id="fb8ca-182">report-uri</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-uri)

<span data-ttu-id="fb8ca-183">Aunque ya no se recomienda el uso de `report-uri`, se deben usar ambas directivas hasta que se admitan `report-to` en todos los exploradores principales.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-183">Although `report-uri` is no longer recommended for use, both directives should be used until `report-to` is supported by all of the major browsers.</span></span> <span data-ttu-id="fb8ca-184">No utilice exclusivamente `report-uri` porque la compatibilidad con `report-uri` está sujeta a la eliminación *en cualquier momento* desde exploradores.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-184">Don't exclusively use `report-uri` because support for `report-uri` is subject to being dropped *at any time* from browsers.</span></span> <span data-ttu-id="fb8ca-185">Quite la compatibilidad con `report-uri` en las directivas cuando `report-to` sea totalmente compatible.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-185">Remove support for `report-uri` in your policies when `report-to` is fully supported.</span></span> <span data-ttu-id="fb8ca-186">Para realizar un seguimiento de la adopción de `report-to`, consulte [puedo usar: Report-to](https://caniuse.com/#feat=mdn-http_headers_csp_content-security-policy_report-to).</span><span class="sxs-lookup"><span data-stu-id="fb8ca-186">To track adoption of `report-to`, see [Can I use: report-to](https://caniuse.com/#feat=mdn-http_headers_csp_content-security-policy_report-to).</span></span>

<span data-ttu-id="fb8ca-187">Probar y actualizar la Directiva de una aplicación en cada versión.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-187">Test and update an app's policy every release.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="fb8ca-188">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="fb8ca-188">Troubleshoot</span></span>

* <span data-ttu-id="fb8ca-189">Los errores aparecen en la consola de herramientas de desarrollo del explorador.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-189">Errors appear in the browser's developer tools console.</span></span> <span data-ttu-id="fb8ca-190">Los exploradores proporcionan información acerca de:</span><span class="sxs-lookup"><span data-stu-id="fb8ca-190">Browsers provide information about:</span></span>
  * <span data-ttu-id="fb8ca-191">Elementos que no cumplen la Directiva.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-191">Elements that don't comply with the policy.</span></span>
  * <span data-ttu-id="fb8ca-192">Cómo modificar la Directiva para permitir un elemento bloqueado.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-192">How to modify the policy to allow for a blocked item.</span></span>
* <span data-ttu-id="fb8ca-193">Una directiva solo es totalmente efectiva cuando el explorador del cliente admite todas las directivas incluidas.</span><span class="sxs-lookup"><span data-stu-id="fb8ca-193">A policy is only completely effective when the client's browser supports all of the included directives.</span></span> <span data-ttu-id="fb8ca-194">Para obtener una matriz de compatibilidad de explorador actual, vea [¿puedo usar: Content-Security-Policy](https://caniuse.com/#search=Content-Security-Policy).</span><span class="sxs-lookup"><span data-stu-id="fb8ca-194">For a current browser support matrix, see [Can I use: Content-Security-Policy](https://caniuse.com/#search=Content-Security-Policy).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fb8ca-195">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="fb8ca-195">Additional resources</span></span>

* [<span data-ttu-id="fb8ca-196">Documentos web de MDN: Content-Security-Policy</span><span class="sxs-lookup"><span data-stu-id="fb8ca-196">MDN web docs: Content-Security-Policy</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy)
* [<span data-ttu-id="fb8ca-197">Nivel 2 de directiva de seguridad de contenido</span><span class="sxs-lookup"><span data-stu-id="fb8ca-197">Content Security Policy Level 2</span></span>](https://www.w3.org/TR/CSP2/)
* [<span data-ttu-id="fb8ca-198">Evaluador de CSP de Google</span><span class="sxs-lookup"><span data-stu-id="fb8ca-198">Google CSP Evaluator</span></span>](https://csp-evaluator.withgoogle.com/)
