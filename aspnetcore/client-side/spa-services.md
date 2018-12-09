---
title: Uso de JavaScriptServices para crear aplicaciones de página única en ASP.NET Core
author: scottaddie
description: Obtenga información sobre las ventajas del uso de JavaScriptServices para crear una aplicación de página única (SPA) respaldado por ASP.NET Core.
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
uid: client-side/spa-services
ms.openlocfilehash: ee772e67ef14608bcc6e3498ade00424ff6090e5
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121381"
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a><span data-ttu-id="26922-103">Uso de JavaScriptServices para crear aplicaciones de página única en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="26922-103">Use JavaScriptServices to Create Single Page Applications in ASP.NET Core</span></span>

<span data-ttu-id="26922-104">Por [Scott Addie](https://github.com/scottaddie) y [Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="26922-104">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="26922-105">Una aplicación de página única (SPA) es un tipo conocido de aplicación web debido a su experiencia de usuario completa e inherente.</span><span class="sxs-lookup"><span data-stu-id="26922-105">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="26922-106">La integración de marcos o bibliotecas SPA del lado cliente, como [Angular](https://angular.io/) o [React](https://facebook.github.io/react/), con marcos del lado servidor como ASP.NET Core puede ser difícil.</span><span class="sxs-lookup"><span data-stu-id="26922-106">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks like ASP.NET Core can be difficult.</span></span> <span data-ttu-id="26922-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) se desarrolló para reducir la fricción en el proceso de integración.</span><span class="sxs-lookup"><span data-stu-id="26922-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="26922-108">Permite el funcionamiento sin problemas entre los distintos componentes tecnológicos del lado servidor y cliente.</span><span class="sxs-lookup"><span data-stu-id="26922-108">It enables seamless operation between the different client and server technology stacks.</span></span>

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a><span data-ttu-id="26922-109">¿Qué es JavaScriptServices</span><span class="sxs-lookup"><span data-stu-id="26922-109">What is JavaScriptServices</span></span>

<span data-ttu-id="26922-110">JavaScriptServices es una colección de tecnologías de cliente para ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="26922-110">JavaScriptServices is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="26922-111">Su objetivo es a la posición de ASP.NET Core como plataforma de servidor preferido de los desarrolladores para la creación de spa.</span><span class="sxs-lookup"><span data-stu-id="26922-111">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="26922-112">JavaScriptServices consta de tres paquetes de NuGet distintos:</span><span class="sxs-lookup"><span data-stu-id="26922-112">JavaScriptServices consists of three distinct NuGet packages:</span></span>

* <span data-ttu-id="26922-113">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="26922-113">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="26922-114">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="26922-114">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>
* <span data-ttu-id="26922-115">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span><span class="sxs-lookup"><span data-stu-id="26922-115">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span></span>

<span data-ttu-id="26922-116">Estos paquetes son útiles si es:</span><span class="sxs-lookup"><span data-stu-id="26922-116">These packages are useful if you:</span></span>

* <span data-ttu-id="26922-117">Ejecutar JavaScript en el servidor</span><span class="sxs-lookup"><span data-stu-id="26922-117">Run JavaScript on the server</span></span>
* <span data-ttu-id="26922-118">Usa un marco o biblioteca de SPA</span><span class="sxs-lookup"><span data-stu-id="26922-118">Use a SPA framework or library</span></span>
* <span data-ttu-id="26922-119">Compile los recursos del lado cliente con Webpack</span><span class="sxs-lookup"><span data-stu-id="26922-119">Build client-side assets with Webpack</span></span>

<span data-ttu-id="26922-120">Gran parte el foco en este artículo se coloca sobre el uso del paquete SpaServices.</span><span class="sxs-lookup"><span data-stu-id="26922-120">Much of the focus in this article is placed on using the SpaServices package.</span></span>

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a><span data-ttu-id="26922-121">¿Qué es SpaServices</span><span class="sxs-lookup"><span data-stu-id="26922-121">What is SpaServices</span></span>

<span data-ttu-id="26922-122">SpaServices se creó para colocar ASP.NET Core como la plataforma del lado servidor preferida de los desarrolladores para compilar las SPA.</span><span class="sxs-lookup"><span data-stu-id="26922-122">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="26922-123">SpaServices no es necesario para desarrollar SPA con ASP.NET Core y no le obliga a usar un marco de cliente en particular.</span><span class="sxs-lookup"><span data-stu-id="26922-123">SpaServices isn't required to develop SPAs with ASP.NET Core, and it doesn't lock you into a particular client framework.</span></span>

<span data-ttu-id="26922-124">SpaServices proporciona infraestructura útil, como:</span><span class="sxs-lookup"><span data-stu-id="26922-124">SpaServices provides useful infrastructure such as:</span></span>

* [<span data-ttu-id="26922-125">Procesamiento previo del lado servidor</span><span class="sxs-lookup"><span data-stu-id="26922-125">Server-side prerendering</span></span>](#server-prerendering)
* [<span data-ttu-id="26922-126">Middleware de desarrollo de Webpack</span><span class="sxs-lookup"><span data-stu-id="26922-126">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="26922-127">Sustitución del módulo de acceso frecuente</span><span class="sxs-lookup"><span data-stu-id="26922-127">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="26922-128">Aplicaciones auxiliares de enrutamientos</span><span class="sxs-lookup"><span data-stu-id="26922-128">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="26922-129">De forma colectiva, estos componentes de infraestructura mejoran el flujo de trabajo de desarrollo y la experiencia en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="26922-129">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="26922-130">Los componentes pueden adoptar individualmente.</span><span class="sxs-lookup"><span data-stu-id="26922-130">The components can be adopted individually.</span></span>

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="26922-131">Requisitos previos para usar SpaServices</span><span class="sxs-lookup"><span data-stu-id="26922-131">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="26922-132">Para trabajar con SpaServices, instale lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="26922-132">To work with SpaServices, install the following:</span></span>

* <span data-ttu-id="26922-133">[Node.js](https://nodejs.org/) (versión 6 o posterior) con npm</span><span class="sxs-lookup"><span data-stu-id="26922-133">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>
  * <span data-ttu-id="26922-134">Para comprobar estos componentes se instalan y se pueden encontrar, ejecute lo siguiente desde la línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="26922-134">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

<span data-ttu-id="26922-135">Nota: Si va a implementar en un sitio web de Azure, no es necesario hacer nada aquí &mdash; Node.js está instalado y disponible en los entornos de servidor.</span><span class="sxs-lookup"><span data-stu-id="26922-135">Note: If you're deploying to an Azure web site, you don't need to do anything here &mdash; Node.js is installed and available in the server environments.</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * <span data-ttu-id="26922-136">Si se encuentra en Windows con Visual Studio 2017, el SDK se instala seleccionando el **desarrollo multiplataforma de .NET Core** carga de trabajo.</span><span class="sxs-lookup"><span data-stu-id="26922-136">If you're on Windows using Visual Studio 2017, the SDK is installed by selecting the **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="26922-137">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) paquete NuGet</span><span class="sxs-lookup"><span data-stu-id="26922-137">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a><span data-ttu-id="26922-138">Procesamiento previo del lado servidor</span><span class="sxs-lookup"><span data-stu-id="26922-138">Server-side prerendering</span></span>

<span data-ttu-id="26922-139">Una aplicación universal (también conocida como isomorfa) es una aplicación de JavaScript capaz de ejecutarse tanto en el servidor como en el cliente.</span><span class="sxs-lookup"><span data-stu-id="26922-139">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="26922-140">Angular, React y otros marcos populares proporcionan una plataforma universal para el desarrollo de este estilo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="26922-140">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="26922-141">La idea es representar primero los componentes del marco en el servidor a través de Node.js y, después, delegar el resto de la ejecución al cliente.</span><span class="sxs-lookup"><span data-stu-id="26922-141">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="26922-142">ASP.NET Core [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro) proporcionada por SpaServices simplificar la implementación de procesamiento previo de lado servidor mediante la invocación de las funciones de JavaScript en el servidor.</span><span class="sxs-lookup"><span data-stu-id="26922-142">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="26922-143">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="26922-143">Prerequisites</span></span>

<span data-ttu-id="26922-144">Instale el software siguiente:</span><span class="sxs-lookup"><span data-stu-id="26922-144">Install the following:</span></span>

* <span data-ttu-id="26922-145">[ASPNET-procesamiento previo](https://www.npmjs.com/package/aspnet-prerendering) paquete npm:</span><span class="sxs-lookup"><span data-stu-id="26922-145">[aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a><span data-ttu-id="26922-146">Configuración</span><span class="sxs-lookup"><span data-stu-id="26922-146">Configuration</span></span>

<span data-ttu-id="26922-147">Las aplicaciones auxiliares de etiquetas se hacen reconocibles a través del registro del espacio de nombres en el proyecto *_ViewImports.cshtml* archivo:</span><span class="sxs-lookup"><span data-stu-id="26922-147">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="26922-148">Estas aplicaciones auxiliares de etiquetas abstraer las complejidades de la comunicación directa con la API de bajo nivel mediante el aprovechamiento de una sintaxis similar a HTML dentro de la vista de Razor:</span><span class="sxs-lookup"><span data-stu-id="26922-148">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a><span data-ttu-id="26922-149">El `asp-prerender-module` aplicación auxiliar de etiquetas</span><span class="sxs-lookup"><span data-stu-id="26922-149">The `asp-prerender-module` Tag Helper</span></span>

<span data-ttu-id="26922-150">El `asp-prerender-module` ejecuta la aplicación auxiliar de etiquetas, utilizados en el ejemplo de código anterior, *ClientApp/dist/main-server.js* en el servidor a través de Node.js.</span><span class="sxs-lookup"><span data-stu-id="26922-150">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="26922-151">Para no complicarlo, *main server.js* archivo es un artefacto de la tarea de transpilación de TypeScript y JavaScript en el [Webpack](http://webpack.github.io/) proceso de compilación.</span><span class="sxs-lookup"><span data-stu-id="26922-151">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="26922-152">Webpack define un alias de punto de entrada de `main-server`; y comienza el cruce seguro de que el gráfico de dependencias para este alias en el *ClientApp, arranque-server.ts* archivo:</span><span class="sxs-lookup"><span data-stu-id="26922-152">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="26922-153">En el siguiente ejemplo Angular, el *ClientApp, arranque-server.ts* archivo utiliza el `createServerRenderer` función y `RenderResult` el tipo de la `aspnet-prerendering` paquete de npm para configurar la representación del servidor a través de Node.js.</span><span class="sxs-lookup"><span data-stu-id="26922-153">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="26922-154">El marcado HTML destinado para la representación del lado servidor se pasa a una llamada de función de resolución, que se ajusta en JavaScript fuertemente tipado `Promise` objeto.</span><span class="sxs-lookup"><span data-stu-id="26922-154">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="26922-155">El `Promise` es de importancia del objeto que proporciona asincrónicamente el marcado HTML para la página para la inserción en el elemento de DOM marcador de posición.</span><span class="sxs-lookup"><span data-stu-id="26922-155">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a><span data-ttu-id="26922-156">El `asp-prerender-data` aplicación auxiliar de etiquetas</span><span class="sxs-lookup"><span data-stu-id="26922-156">The `asp-prerender-data` Tag Helper</span></span>

<span data-ttu-id="26922-157">Cuando se combina con la `asp-prerender-module` aplicación auxiliar de etiquetas, la `asp-prerender-data` aplicación auxiliar de etiquetas se puede usar para pasar información contextual de la vista de Razor a JavaScript del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="26922-157">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="26922-158">Por ejemplo, el marcado siguiente pasa los datos de usuario en el `main-server` módulo:</span><span class="sxs-lookup"><span data-stu-id="26922-158">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="26922-159">Los datos recibidos `UserName` argumento se serializa utilizando el serializador JSON integrado y se almacena en la `params.data` objeto.</span><span class="sxs-lookup"><span data-stu-id="26922-159">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="26922-160">En el siguiente ejemplo Angular, los datos se utilizan para construir un saludo personalizado dentro de un `h1` elemento:</span><span class="sxs-lookup"><span data-stu-id="26922-160">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="26922-161">Nota: Los nombres de propiedad pasados en las aplicaciones auxiliares de etiquetas se representan con **PascalCase** notación.</span><span class="sxs-lookup"><span data-stu-id="26922-161">Note: Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="26922-162">Compare esto con JavaScript, donde se representan los mismos nombres de propiedad con **camelCase**.</span><span class="sxs-lookup"><span data-stu-id="26922-162">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="26922-163">La configuración predeterminada de serialización de JSON es responsable de esta diferencia.</span><span class="sxs-lookup"><span data-stu-id="26922-163">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="26922-164">Para ampliar el ejemplo de código anterior, datos pueden pasarse desde el servidor a la vista por hidratación el `globals` propiedad proporcionada a la `resolve` función:</span><span class="sxs-lookup"><span data-stu-id="26922-164">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="26922-165">El `postList` matriz definida dentro de la `globals` objeto está adjunto al explorador global `window` objeto.</span><span class="sxs-lookup"><span data-stu-id="26922-165">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="26922-166">Esta elevación de variables en ámbito global elimina la duplicación de esfuerzos, sobre todo lo que respecta a la carga de los mismos datos una vez en el servidor y de nuevo en el cliente.</span><span class="sxs-lookup"><span data-stu-id="26922-166">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![variable global de postList adjuntado al objeto de ventana](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a><span data-ttu-id="26922-168">Middleware de desarrollo de Webpack</span><span class="sxs-lookup"><span data-stu-id="26922-168">Webpack Dev Middleware</span></span>

<span data-ttu-id="26922-169">[Middleware de desarrollo de Webpack](https://webpack.github.io/docs/webpack-dev-middleware.html) incorpora un flujo de trabajo de desarrollo optimizado por el cual Webpack compila los recursos a petición.</span><span class="sxs-lookup"><span data-stu-id="26922-169">[Webpack Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="26922-170">El middleware automáticamente compila y sirve de recursos del cliente cuando se vuelve a cargar una página en el explorador.</span><span class="sxs-lookup"><span data-stu-id="26922-170">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="26922-171">El enfoque alternativo es invocar manualmente la Webpack a través de script de compilación del proyecto npm cuando cambia una dependencia de terceros o código personalizado.</span><span class="sxs-lookup"><span data-stu-id="26922-171">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="26922-172">Script de compilación de un npm el *package.json* archivo se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="26922-172">An npm build script in the *package.json* file is shown in the following example:</span></span>

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="prerequisites"></a><span data-ttu-id="26922-173">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="26922-173">Prerequisites</span></span>

<span data-ttu-id="26922-174">Instale el software siguiente:</span><span class="sxs-lookup"><span data-stu-id="26922-174">Install the following:</span></span>

* <span data-ttu-id="26922-175">[ASPNET webpack](https://www.npmjs.com/package/aspnet-webpack) paquete npm:</span><span class="sxs-lookup"><span data-stu-id="26922-175">[aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a><span data-ttu-id="26922-176">Configuración</span><span class="sxs-lookup"><span data-stu-id="26922-176">Configuration</span></span>

<span data-ttu-id="26922-177">Middleware de desarrollo de Webpack está registrado en la canalización de solicitudes HTTP mediante el siguiente código en el *Startup.cs* del archivo `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="26922-177">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_WebpackMiddlewareRegistration&highlight=4)]

<span data-ttu-id="26922-178">El `UseWebpackDevMiddleware` método de extensión debe llamarse antes [registrar archivos estáticos de hospedaje](xref:fundamentals/static-files) a través de la `UseStaticFiles` método de extensión.</span><span class="sxs-lookup"><span data-stu-id="26922-178">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="26922-179">Por motivos de seguridad, registre el middleware solo cuando la aplicación se ejecuta en modo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="26922-179">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="26922-180">El *webpack.config.js* del archivo `output.publicPath` propiedad indica el middleware para inspeccionar el `dist` carpeta para los cambios:</span><span class="sxs-lookup"><span data-stu-id="26922-180">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a><span data-ttu-id="26922-181">Sustitución del módulo de acceso frecuente</span><span class="sxs-lookup"><span data-stu-id="26922-181">Hot Module Replacement</span></span>

<span data-ttu-id="26922-182">Piense de Webpack [reemplazo en caliente módulo](https://webpack.js.org/concepts/hot-module-replacement/) característica (HMR) como una evolución de [Webpack Dev Middleware](#webpack-dev-middleware).</span><span class="sxs-lookup"><span data-stu-id="26922-182">Think of Webpack's [Hot Module Replacement](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="26922-183">HMR presenta las mismas ventajas, pero simplifica aún más el flujo de trabajo de desarrollo actualizando automáticamente el contenido de la página después de compilar los cambios.</span><span class="sxs-lookup"><span data-stu-id="26922-183">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="26922-184">No lo confunda con una actualización del explorador, lo que interferiría con el estado actual de en memoria y la sesión de depuración de la SPA.</span><span class="sxs-lookup"><span data-stu-id="26922-184">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="26922-185">Hay un vínculo directo entre el servicio de Webpack Dev Middleware y el explorador, lo que significa que los cambios se insertan en el explorador.</span><span class="sxs-lookup"><span data-stu-id="26922-185">There's a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="26922-186">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="26922-186">Prerequisites</span></span>

<span data-ttu-id="26922-187">Instale el software siguiente:</span><span class="sxs-lookup"><span data-stu-id="26922-187">Install the following:</span></span>

* <span data-ttu-id="26922-188">[middleware "hot" webpack](https://www.npmjs.com/package/webpack-hot-middleware) paquete npm:</span><span class="sxs-lookup"><span data-stu-id="26922-188">[webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a><span data-ttu-id="26922-189">Configuración</span><span class="sxs-lookup"><span data-stu-id="26922-189">Configuration</span></span>

<span data-ttu-id="26922-190">El componente HMR debe estar registrado en la canalización de solicitudes HTTP de MVC en el `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="26922-190">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="26922-191">Igual que ocurría con [Webpack Dev Middleware](#webpack-dev-middleware), `UseWebpackDevMiddleware` método de extensión debe llamarse antes el `UseStaticFiles` método de extensión.</span><span class="sxs-lookup"><span data-stu-id="26922-191">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="26922-192">Por motivos de seguridad, registre el middleware solo cuando la aplicación se ejecuta en modo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="26922-192">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="26922-193">El *webpack.config.js* archivo debe definir un `plugins` matriz, incluso si se deja vacío:</span><span class="sxs-lookup"><span data-stu-id="26922-193">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="26922-194">Después de cargar la aplicación en el explorador, pestaña de la consola de las herramientas de desarrollo proporciona la confirmación de la activación de HMR:</span><span class="sxs-lookup"><span data-stu-id="26922-194">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![Mensaje de conectado hot sustitución del módulo](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a><span data-ttu-id="26922-196">Aplicaciones auxiliares de enrutamientos</span><span class="sxs-lookup"><span data-stu-id="26922-196">Routing helpers</span></span>

<span data-ttu-id="26922-197">En la mayoría de las spa basada en ASP.NET Core, le conviene además del enrutamiento del servidor de enrutamiento del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="26922-197">In most ASP.NET Core-based SPAs, you'll want client-side routing in addition to server-side routing.</span></span> <span data-ttu-id="26922-198">Los sistemas de enrutamiento SPA y MVC pueden trabajar de forma independiente sin interferencias.</span><span class="sxs-lookup"><span data-stu-id="26922-198">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="26922-199">Hay, sin embargo, un borde case plantean desafíos: identificación de respuestas HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="26922-199">There's, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="26922-200">Considere el escenario en el que una ruta sin extensión de `/some/page` se utiliza.</span><span class="sxs-lookup"><span data-stu-id="26922-200">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="26922-201">Suponga que la solicitud no hacer coincidir el patrón una ruta de servidor, pero su patrón coincide con una ruta del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="26922-201">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="26922-202">Ahora considere una solicitud entrante para `/images/user-512.png`, por lo general que espera encontrar un archivo de imagen en el servidor.</span><span class="sxs-lookup"><span data-stu-id="26922-202">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="26922-203">Si esa ruta de acceso del recurso solicitado no coincide con ninguna ruta del lado servidor o un archivo estático, no es probable que la aplicación del lado cliente controlaría, por lo general desea devolver un código de estado HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="26922-203">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it — you generally want to return a 404 HTTP status code.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="26922-204">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="26922-204">Prerequisites</span></span>

<span data-ttu-id="26922-205">Instale el software siguiente:</span><span class="sxs-lookup"><span data-stu-id="26922-205">Install the following:</span></span>

* <span data-ttu-id="26922-206">El paquete de npm enrutamiento del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="26922-206">The client-side routing npm package.</span></span> <span data-ttu-id="26922-207">Uso de Angular como ejemplo:</span><span class="sxs-lookup"><span data-stu-id="26922-207">Using Angular as an example:</span></span>

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a><span data-ttu-id="26922-208">Configuración</span><span class="sxs-lookup"><span data-stu-id="26922-208">Configuration</span></span>

<span data-ttu-id="26922-209">Un método de extensión denominado `MapSpaFallbackRoute` se utiliza en el `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="26922-209">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_MvcRoutingTable&highlight=7-9)]

<span data-ttu-id="26922-210">Sugerencia: Las rutas se evalúan en el orden en el que se está configurado.</span><span class="sxs-lookup"><span data-stu-id="26922-210">Tip: Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="26922-211">Por lo tanto, el `default` ruta en el ejemplo de código anterior se usa primero para la coincidencia de patrones.</span><span class="sxs-lookup"><span data-stu-id="26922-211">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a><span data-ttu-id="26922-212">Crear un nuevo proyecto</span><span class="sxs-lookup"><span data-stu-id="26922-212">Creating a new project</span></span>

<span data-ttu-id="26922-213">JavaScriptServices proporciona plantillas de aplicaciones configuradas previamente.</span><span class="sxs-lookup"><span data-stu-id="26922-213">JavaScriptServices provides pre-configured application templates.</span></span> <span data-ttu-id="26922-214">SpaServices se usa en estas plantillas, junto con una diversidad de marcos y bibliotecas como Angular, React y Redux.</span><span class="sxs-lookup"><span data-stu-id="26922-214">SpaServices is used in these templates, in conjunction with different frameworks and libraries such as Angular, React, and Redux.</span></span>

<span data-ttu-id="26922-215">Estas plantillas se pueden instalar a través de la CLI de .NET Core, ejecute el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="26922-215">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="26922-216">Se muestra una lista de plantillas SPA disponibles:</span><span class="sxs-lookup"><span data-stu-id="26922-216">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="26922-217">Plantillas</span><span class="sxs-lookup"><span data-stu-id="26922-217">Templates</span></span>                                 | <span data-ttu-id="26922-218">Nombre corto</span><span class="sxs-lookup"><span data-stu-id="26922-218">Short Name</span></span> | <span data-ttu-id="26922-219">Lenguaje</span><span class="sxs-lookup"><span data-stu-id="26922-219">Language</span></span> | <span data-ttu-id="26922-220">Etiquetas</span><span class="sxs-lookup"><span data-stu-id="26922-220">Tags</span></span>        |
|:------------------------------------------|:-----------|:---------|:------------|
| <span data-ttu-id="26922-221">MVC de ASP.NET Core con Angular</span><span class="sxs-lookup"><span data-stu-id="26922-221">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="26922-222">angular</span><span class="sxs-lookup"><span data-stu-id="26922-222">angular</span></span>    | <span data-ttu-id="26922-223">[C#]</span><span class="sxs-lookup"><span data-stu-id="26922-223">[C#]</span></span>     | <span data-ttu-id="26922-224">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="26922-224">Web/MVC/SPA</span></span> |
| <span data-ttu-id="26922-225">MVC de ASP.NET Core con React.js</span><span class="sxs-lookup"><span data-stu-id="26922-225">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="26922-226">react</span><span class="sxs-lookup"><span data-stu-id="26922-226">react</span></span>      | <span data-ttu-id="26922-227">[C#]</span><span class="sxs-lookup"><span data-stu-id="26922-227">[C#]</span></span>     | <span data-ttu-id="26922-228">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="26922-228">Web/MVC/SPA</span></span> |
| <span data-ttu-id="26922-229">MVC ASP.NET Core con React.js y Redux</span><span class="sxs-lookup"><span data-stu-id="26922-229">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="26922-230">reactredux</span><span class="sxs-lookup"><span data-stu-id="26922-230">reactredux</span></span> | <span data-ttu-id="26922-231">[C#]</span><span class="sxs-lookup"><span data-stu-id="26922-231">[C#]</span></span>     | <span data-ttu-id="26922-232">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="26922-232">Web/MVC/SPA</span></span> |

<span data-ttu-id="26922-233">Para crear un nuevo proyecto con una de las plantillas SPA, incluya el **nombre corto** de la plantilla en el [dotnet nuevo](/dotnet/core/tools/dotnet-new) comando.</span><span class="sxs-lookup"><span data-stu-id="26922-233">To create a new project using one of the SPA templates, include the **Short Name** of the template in the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="26922-234">El siguiente comando crea una aplicación Angular con ASP.NET Core MVC configurada para el lado del servidor:</span><span class="sxs-lookup"><span data-stu-id="26922-234">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="26922-235">Establecer el modo de configuración en tiempo de ejecución</span><span class="sxs-lookup"><span data-stu-id="26922-235">Set the runtime configuration mode</span></span>

<span data-ttu-id="26922-236">Existen dos modos de configuración en tiempo de ejecución principal:</span><span class="sxs-lookup"><span data-stu-id="26922-236">Two primary runtime configuration modes exist:</span></span>

* <span data-ttu-id="26922-237">**Desarrollo**:</span><span class="sxs-lookup"><span data-stu-id="26922-237">**Development**:</span></span>
  * <span data-ttu-id="26922-238">Incluye mapas de código fuente para facilitar la depuración.</span><span class="sxs-lookup"><span data-stu-id="26922-238">Includes source maps to ease debugging.</span></span>
  * <span data-ttu-id="26922-239">No optimice el código del lado cliente para el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="26922-239">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="26922-240">**Producción**:</span><span class="sxs-lookup"><span data-stu-id="26922-240">**Production**:</span></span>
  * <span data-ttu-id="26922-241">Excluye los mapas de código fuente.</span><span class="sxs-lookup"><span data-stu-id="26922-241">Excludes source maps.</span></span>
  * <span data-ttu-id="26922-242">Optimiza el código del lado cliente a través de la unión y minificación.</span><span class="sxs-lookup"><span data-stu-id="26922-242">Optimizes the client-side code via bundling & minification.</span></span>

<span data-ttu-id="26922-243">ASP.NET Core usa una variable de entorno denominada `ASPNETCORE_ENVIRONMENT` para almacenar el modo de configuración.</span><span class="sxs-lookup"><span data-stu-id="26922-243">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="26922-244">Consulte **[establecer el entorno](xref:fundamentals/environments#set-the-environment)** para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="26922-244">See **[Set the environment](xref:fundamentals/environments#set-the-environment)** for more information.</span></span>

### <a name="running-with-net-core-cli"></a><span data-ttu-id="26922-245">Ejecutar con la CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="26922-245">Running with .NET Core CLI</span></span>

<span data-ttu-id="26922-246">Restaurar los paquetes de npm y NuGet necesario ejecutando el siguiente comando en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="26922-246">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="26922-247">Compilar y ejecutar la aplicación:</span><span class="sxs-lookup"><span data-stu-id="26922-247">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="26922-248">Se inicia la aplicación en el host local según el [modo de configuración en tiempo de ejecución](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="26922-248">The application starts on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> <span data-ttu-id="26922-249">Vaya a `http://localhost:5000` en el explorador muestra la página de inicio.</span><span class="sxs-lookup"><span data-stu-id="26922-249">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="running-with-visual-studio-2017"></a><span data-ttu-id="26922-250">Ejecución con Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="26922-250">Running with Visual Studio 2017</span></span>

<span data-ttu-id="26922-251">Abra el *.csproj* archivo generado por el [dotnet nuevo](/dotnet/core/tools/dotnet-new) comando.</span><span class="sxs-lookup"><span data-stu-id="26922-251">Open the *.csproj* file generated by the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="26922-252">Los paquetes de NuGet y npm necesarios se restauran automáticamente al proyecto abierto.</span><span class="sxs-lookup"><span data-stu-id="26922-252">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="26922-253">Este proceso de restauración puede tardar varios minutos, y la aplicación está lista para ejecutarse cuando se complete.</span><span class="sxs-lookup"><span data-stu-id="26922-253">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="26922-254">Haga clic en el botón verde de ejecución o presione `Ctrl + F5`, y el explorador se abre en la página de aterrizaje de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="26922-254">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="26922-255">La aplicación se ejecuta en localhost según la [modo de configuración en tiempo de ejecución](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="26922-255">The application runs on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span>

<a name="app-testing"></a>

## <a name="testing-the-app"></a><span data-ttu-id="26922-256">Probar la aplicación</span><span class="sxs-lookup"><span data-stu-id="26922-256">Testing the app</span></span>

<span data-ttu-id="26922-257">Las plantillas de SpaServices están preconfiguradas para ejecutar pruebas del lado cliente mediante [Karma](https://karma-runner.github.io/1.0/index.html) y [Jasmine](https://jasmine.github.io/).</span><span class="sxs-lookup"><span data-stu-id="26922-257">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="26922-258">Jasmine es un marco de pruebas unitarias popular para JavaScript, mientras que Karma es un ejecutor de esas pruebas.</span><span class="sxs-lookup"><span data-stu-id="26922-258">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="26922-259">Karma está configurado para funcionar con [Webpack Dev Middleware](#webpack-dev-middleware), de forma que el desarrollador no tenga que parar y ejecutar la prueba cada vez que se realicen cambios.</span><span class="sxs-lookup"><span data-stu-id="26922-259">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that the developer isn't required to stop and run the test every time changes are made.</span></span> <span data-ttu-id="26922-260">Tanto si se ejecuta el código en el caso de prueba como si se trata del propio caso de prueba, la prueba se ejecuta automáticamente.</span><span class="sxs-lookup"><span data-stu-id="26922-260">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="26922-261">Con la aplicación Angular como ejemplo, dos casos de prueba Jasmine ya se proporcionan para el `CounterComponent` en el *counter.component.spec.ts* archivo:</span><span class="sxs-lookup"><span data-stu-id="26922-261">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="26922-262">Abra el símbolo del sistema en el *ClientApp* directory.</span><span class="sxs-lookup"><span data-stu-id="26922-262">Open the command prompt in the *ClientApp* directory.</span></span> <span data-ttu-id="26922-263">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="26922-263">Run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="26922-264">El script inicia el ejecutor de pruebas Karma, que lee la configuración definida en el *karma.conf.js* archivo.</span><span class="sxs-lookup"><span data-stu-id="26922-264">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="26922-265">Entre otras opciones, el *karma.conf.js* identifica los archivos de prueba que se ejecuta a través de su `files` matriz:</span><span class="sxs-lookup"><span data-stu-id="26922-265">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a><span data-ttu-id="26922-266">Publicar la aplicación</span><span class="sxs-lookup"><span data-stu-id="26922-266">Publishing the application</span></span>

<span data-ttu-id="26922-267">Combinación de los recursos del lado cliente generados y los artefactos de ASP.NET Core publicados en un paquete listo para implementar puede resultar tedioso.</span><span class="sxs-lookup"><span data-stu-id="26922-267">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="26922-268">Afortunadamente, SpaServices organiza ese proceso de publicación completa con un destino de MSBuild personalizado denominado `RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="26922-268">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="26922-269">El destino de MSBuild tiene las siguientes responsabilidades:</span><span class="sxs-lookup"><span data-stu-id="26922-269">The MSBuild target has the following responsibilities:</span></span>

1. <span data-ttu-id="26922-270">Restaure los paquetes de npm</span><span class="sxs-lookup"><span data-stu-id="26922-270">Restore the npm packages</span></span>
1. <span data-ttu-id="26922-271">Crear una compilación de producción de los recursos de otros fabricantes, del lado cliente</span><span class="sxs-lookup"><span data-stu-id="26922-271">Create a production-grade build of the third-party, client-side assets</span></span>
1. <span data-ttu-id="26922-272">Crear una compilación de producción de los activos de cliente personalizadas</span><span class="sxs-lookup"><span data-stu-id="26922-272">Create a production-grade build of the custom client-side assets</span></span>
1. <span data-ttu-id="26922-273">Copiar los activos de Webpack generados en la carpeta de publicación</span><span class="sxs-lookup"><span data-stu-id="26922-273">Copy the Webpack-generated assets to the publish folder</span></span>

<span data-ttu-id="26922-274">El destino de MSBuild se invoca cuando se ejecuta:</span><span class="sxs-lookup"><span data-stu-id="26922-274">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="26922-275">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="26922-275">Additional resources</span></span>

* [<span data-ttu-id="26922-276">Docs angulares</span><span class="sxs-lookup"><span data-stu-id="26922-276">Angular Docs</span></span>](https://angular.io/docs)
