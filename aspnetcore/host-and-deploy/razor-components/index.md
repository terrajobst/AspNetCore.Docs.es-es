---
title: Hospedaje e implementación de Razor Components
author: guardrex
description: Descubra cómo hospedar e implementar aplicaciones de Blazor y Razor Components con ASP.NET Core, redes de entrega de contenido (CDN), servidores de archivos y GitHub Pages.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/22/2019
uid: host-and-deploy/razor-components/index
ms.openlocfilehash: 236e8da27b80dbdb3e0ea885413b6cfd563dde60
ms.sourcegitcommit: 7d6019f762fc5b8cbedcd69801e8310f51a17c18
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419425"
---
# <a name="host-and-deploy-razor-components"></a><span data-ttu-id="86274-103">Hospedaje e implementación de Razor Components</span><span class="sxs-lookup"><span data-stu-id="86274-103">Host and deploy Razor Components</span></span>

<span data-ttu-id="86274-104">Por [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) y [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="86274-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

## <a name="publish-the-app"></a><span data-ttu-id="86274-105">Publicar la aplicación</span><span class="sxs-lookup"><span data-stu-id="86274-105">Publish the app</span></span>

<span data-ttu-id="86274-106">Las aplicaciones se publican para la implementación en la configuración de lanzamiento con el comando [dotnet publish](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="86274-106">Apps are published for deployment in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="86274-107">Un IDE puede controlar la ejecución del comando `dotnet publish` automáticamente mediante sus características de publicación integradas, por lo que puede que no sea necesario ejecutar manualmente el comando desde un símbolo del sistema, en función de las herramientas de desarrollo que se usen.</span><span class="sxs-lookup"><span data-stu-id="86274-107">An IDE may handle executing the `dotnet publish` command automatically using its built-in publishing features, so it might not be necessary to manually execute the command from a command prompt depending on the development tools in use.</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="86274-108">`dotnet publish` desencadena una [restauración](/dotnet/core/tools/dotnet-restore) de las dependencias del proyecto y [compila](/dotnet/core/tools/dotnet-build) el proyecto antes de crear los recursos para la implementación.</span><span class="sxs-lookup"><span data-stu-id="86274-108">`dotnet publish` triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="86274-109">Como parte del proceso de compilación, se quitan los ensamblados y métodos que no se usan para reducir los tiempos de carga y el tamaño de descarga de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="86274-109">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span> <span data-ttu-id="86274-110">La implementación se crea en la carpeta */bin/Release/&lt;plataforma-de-destino&gt;/publish*.</span><span class="sxs-lookup"><span data-stu-id="86274-110">The deployment is created in the */bin/Release/&lt;target-framework&gt;/publish* folder.</span></span>

<span data-ttu-id="86274-111">Los recursos de la carpeta *publish* se implementan en el servidor web.</span><span class="sxs-lookup"><span data-stu-id="86274-111">The assets in the *publish* folder are deployed to the web server.</span></span> <span data-ttu-id="86274-112">La implementación puede ser un proceso manual o automatizado, en función de las herramientas de desarrollo que se usen.</span><span class="sxs-lookup"><span data-stu-id="86274-112">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="86274-113">Valores de configuración de host</span><span class="sxs-lookup"><span data-stu-id="86274-113">Host configuration values</span></span>

<span data-ttu-id="86274-114">Las aplicaciones de Razor Components que usan el [modelo de hospedaje del lado servidor](xref:razor-components/hosting-models#server-side-hosting-model) pueden aceptar [valores de configuración de host web](xref:fundamentals/host/web-host#host-configuration-values).</span><span class="sxs-lookup"><span data-stu-id="86274-114">Razor Components apps that use the [server-side hosting model](xref:razor-components/hosting-models#server-side-hosting-model) can accept [Web Host configuration values](xref:fundamentals/host/web-host#host-configuration-values).</span></span>

<span data-ttu-id="86274-115">Las aplicaciones de Blazor que usan el [modelo de hospedaje del lado cliente](xref:razor-components/hosting-models#client-side-hosting-model) pueden aceptar los siguientes valores de configuración de host como argumentos de línea de comandos en tiempo de ejecución en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="86274-115">Blazor apps that use the [client-side hosting model](xref:razor-components/hosting-models#client-side-hosting-model) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="86274-116">Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="86274-116">Content Root</span></span>

<span data-ttu-id="86274-117">El argumento `--contentroot` establece la ruta de acceso absoluta al directorio que incluye los archivos de contenido de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="86274-117">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files.</span></span>

* <span data-ttu-id="86274-118">Pase el argumento al ejecutar la aplicación de forma local en un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="86274-118">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="86274-119">En el directorio de la aplicación, ejecute lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="86274-119">From the app's directory, execute:</span></span>

  ```console
  dotnet run --contentroot=/<content-root>
  ```

* <span data-ttu-id="86274-120">Agregue una entrada al archivo *launchSettings.json* de la aplicación en el perfil **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="86274-120">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="86274-121">Este valor se selecciona al ejecutar la aplicación con el depurador de Visual Studio y al ejecutarla desde un símbolo del sistema con `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="86274-121">This setting is picked up when running the app with the Visual Studio Debugger and when running the app from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/<content-root>"
  ```

* <span data-ttu-id="86274-122">En Visual Studio, especifique el argumento en **Propiedades** > **Depuración** > **Argumentos de la aplicación**.</span><span class="sxs-lookup"><span data-stu-id="86274-122">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="86274-123">Al establecer el argumento en la página de propiedades de Visual Studio, se agrega el argumento al archivo *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="86274-123">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/<content-root>
  ```

### <a name="path-base"></a><span data-ttu-id="86274-124">Ruta de acceso base</span><span class="sxs-lookup"><span data-stu-id="86274-124">Path base</span></span>

<span data-ttu-id="86274-125">El argumento `--pathbase` establece la ruta de acceso base de la aplicación para una aplicación que se ejecuta localmente con una ruta de acceso virtual que no es raíz (el valor `href` de la etiqueta `<base>` se establece en una ruta de acceso que no sea `/` para ensayo y producción).</span><span class="sxs-lookup"><span data-stu-id="86274-125">The `--pathbase` argument sets the app base path for an app run locally with a non-root virtual path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="86274-126">Para obtener más información, vea la sección [Ruta de acceso base de la aplicación](#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="86274-126">For more information, see the [App base path](#app-base-path) section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="86274-127">A diferencia de la ruta de acceso proporcionada al valor `href` de la etiqueta `<base>`, no incluya una barra diagonal final (`/`) al pasar el valor del argumento `--pathbase`.</span><span class="sxs-lookup"><span data-stu-id="86274-127">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="86274-128">Si se proporciona la ruta de acceso base de la aplicación en la etiqueta `<base>` como `<base href="/CoolApp/" />` (se incluye una barra diagonal final), se pasa el valor del argumento de línea de comandos como `--pathbase=/CoolApp` (sin barra diagonal final).</span><span class="sxs-lookup"><span data-stu-id="86274-128">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/" />` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="86274-129">Pase el argumento al ejecutar la aplicación de forma local en un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="86274-129">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="86274-130">En el directorio de la aplicación, ejecute lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="86274-130">From the app's directory, execute:</span></span>

  ```console
  dotnet run --pathbase=/<virtual-path>
  ```

* <span data-ttu-id="86274-131">Agregue una entrada al archivo *launchSettings.json* de la aplicación en el perfil **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="86274-131">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="86274-132">Este valor se selecciona al ejecutar la aplicación con el depurador de Visual Studio y al ejecutarla desde un símbolo del sistema con `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="86274-132">This setting is picked up when running the app with the Visual Studio Debugger and when running the app from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/<virtual-path>"
  ```

* <span data-ttu-id="86274-133">En Visual Studio, especifique el argumento en **Propiedades** > **Depuración** > **Argumentos de la aplicación**.</span><span class="sxs-lookup"><span data-stu-id="86274-133">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="86274-134">Al establecer el argumento en la página de propiedades de Visual Studio, se agrega el argumento al archivo *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="86274-134">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/<virtual-path>
  ```

### <a name="urls"></a><span data-ttu-id="86274-135">Direcciones URL</span><span class="sxs-lookup"><span data-stu-id="86274-135">URLs</span></span>

<span data-ttu-id="86274-136">El argumento `--urls` indica las direcciones IP o las direcciones de host con los puertos y protocolos en los que escuchar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="86274-136">The `--urls` argument indicates the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="86274-137">Pase el argumento al ejecutar la aplicación de forma local en un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="86274-137">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="86274-138">En el directorio de la aplicación, ejecute lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="86274-138">From the app's directory, execute:</span></span>

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="86274-139">Agregue una entrada al archivo *launchSettings.json* de la aplicación en el perfil **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="86274-139">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="86274-140">Este valor se selecciona al ejecutar la aplicación con el depurador de Visual Studio y al ejecutarla desde un símbolo del sistema con `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="86274-140">This setting is picked up when running the app with the Visual Studio Debugger and when running the app from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="86274-141">En Visual Studio, especifique el argumento en **Propiedades** > **Depuración** > **Argumentos de la aplicación**.</span><span class="sxs-lookup"><span data-stu-id="86274-141">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="86274-142">Al establecer el argumento en la página de propiedades de Visual Studio, se agrega el argumento al archivo *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="86274-142">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="deploy-a-client-side-blazor-app"></a><span data-ttu-id="86274-143">Implementación de una aplicación Blazor del lado cliente</span><span class="sxs-lookup"><span data-stu-id="86274-143">Deploy a client-side Blazor app</span></span>

<span data-ttu-id="86274-144">Con el [modelo de hospedaje del lado cliente](xref:razor-components/hosting-models#client-side-hosting-model):</span><span class="sxs-lookup"><span data-stu-id="86274-144">With the [client-side hosting model](xref:razor-components/hosting-models#client-side-hosting-model):</span></span>

* <span data-ttu-id="86274-145">La aplicación Blazor, sus dependencias y el entorno de ejecución de .NET se descargan en el explorador.</span><span class="sxs-lookup"><span data-stu-id="86274-145">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="86274-146">La aplicación se ejecuta directamente en el subproceso de interfaz de usuario del explorador.</span><span class="sxs-lookup"><span data-stu-id="86274-146">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="86274-147">Se puede seguir cualquiera de las estrategias siguientes:</span><span class="sxs-lookup"><span data-stu-id="86274-147">Either of the following strategies is supported:</span></span>
  * <span data-ttu-id="86274-148">Una aplicación ASP.NET Core proporciona la aplicación Blazor.</span><span class="sxs-lookup"><span data-stu-id="86274-148">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="86274-149">Este proceso se trata en la sección [Implementación hospedada en Blazor del lado cliente con ASP.NET Core](#client-side-blazor-hosted-deployment-with-aspnet-core).</span><span class="sxs-lookup"><span data-stu-id="86274-149">Covered in the [Client-side Blazor hosted deployment with ASP.NET Core](#client-side-blazor-hosted-deployment-with-aspnet-core) section.</span></span>
  * <span data-ttu-id="86274-150">La aplicación Blazor se coloca en un servicio o servidor web de hospedaje estático, donde no se usa .NET para proporcionar la aplicación Blazor.</span><span class="sxs-lookup"><span data-stu-id="86274-150">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="86274-151">Este proceso se trata en la sección [Implementación independiente de Blazor del lado cliente](#client-side-blazor-standalone-deployment).</span><span class="sxs-lookup"><span data-stu-id="86274-151">Covered in the [Client-side Blazor standalone deployment](#client-side-blazor-standalone-deployment) section.</span></span>

### <a name="configure-the-linker"></a><span data-ttu-id="86274-152">Configurar el enlazador</span><span class="sxs-lookup"><span data-stu-id="86274-152">Configure the Linker</span></span>

<span data-ttu-id="86274-153">Blazor realiza la vinculación de lenguaje intermedio (IL) en cada compilación para quitar el IL innecesario de los ensamblados de salida.</span><span class="sxs-lookup"><span data-stu-id="86274-153">Blazor performs Intermediate Language (IL) linking on each build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="86274-154">Puede controlar la vinculación del ensamblado durante la compilación.</span><span class="sxs-lookup"><span data-stu-id="86274-154">You can control assembly linking on build.</span></span> <span data-ttu-id="86274-155">Para obtener más información, vea <xref:host-and-deploy/razor-components/configure-linker>.</span><span class="sxs-lookup"><span data-stu-id="86274-155">For more information, see <xref:host-and-deploy/razor-components/configure-linker>.</span></span>

### <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="86274-156">Reescritura de las URL para conseguir un enrutamiento correcto</span><span class="sxs-lookup"><span data-stu-id="86274-156">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="86274-157">Enrutar las solicitudes para los componentes de la página en una aplicación del lado cliente no es tan sencillo como enrutar las solicitudes a una aplicación hospedada del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="86274-157">Routing requests for page components in a client-side app isn't as simple as routing requests to a server-side, hosted app.</span></span> <span data-ttu-id="86274-158">Imagine que tiene una aplicación del lado cliente con dos páginas:</span><span class="sxs-lookup"><span data-stu-id="86274-158">Consider a client-side app with two pages:</span></span>

* <span data-ttu-id="86274-159">**_Main.cshtml_**: se carga en la raíz de la aplicación y contiene un vínculo a la página de información (`href="About"`).</span><span class="sxs-lookup"><span data-stu-id="86274-159">**_Main.cshtml_** &ndash; Loads at the root of the app and contains a link to the About page (`href="About"`).</span></span>
* <span data-ttu-id="86274-160">**_About.cshtml_**: página de información.</span><span class="sxs-lookup"><span data-stu-id="86274-160">**_About.cshtml_** &ndash; About page.</span></span>

<span data-ttu-id="86274-161">Cuando se solicita el documento predeterminado de la aplicación mediante la barra de direcciones del explorador (por ejemplo, `https://www.contoso.com/`):</span><span class="sxs-lookup"><span data-stu-id="86274-161">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="86274-162">El explorador realiza una solicitud.</span><span class="sxs-lookup"><span data-stu-id="86274-162">The browser makes a request.</span></span>
1. <span data-ttu-id="86274-163">Se devuelve la página predeterminada, que suele ser *index.html*.</span><span class="sxs-lookup"><span data-stu-id="86274-163">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="86274-164">*index.html* arranca la aplicación.</span><span class="sxs-lookup"><span data-stu-id="86274-164">*index.html* bootstraps the app.</span></span>
1. <span data-ttu-id="86274-165">Se carga el enrutador de Blazor y se muestra la página principal de Razor (*Main.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="86274-165">Blazor's router loads and the Razor Main page (*Main.cshtml*) is displayed.</span></span>

<span data-ttu-id="86274-166">En la página principal, al seleccionar el vínculo a la página de información, se carga la página de información.</span><span class="sxs-lookup"><span data-stu-id="86274-166">On the Main page, selecting the link to the About page loads the About page.</span></span> <span data-ttu-id="86274-167">Seleccionar el vínculo a la página de información funciona en el cliente porque el enrutador de Blazor impide que el explorador realice una solicitud en Internet a `www.contoso.com` sobre `About` y presenta la propia página de información.</span><span class="sxs-lookup"><span data-stu-id="86274-167">Selecting the link to the About page works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the About page itself.</span></span> <span data-ttu-id="86274-168">Todas las solicitudes de páginas internas *dentro de la aplicación del lado cliente* funcionan del mismo modo: Las solicitudes no desencadenan solicitudes basadas en el explorador a recursos hospedados en el servidor en Internet.</span><span class="sxs-lookup"><span data-stu-id="86274-168">All of the requests for internal pages *within the client-side app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="86274-169">El enrutador controla las solicitudes de forma interna.</span><span class="sxs-lookup"><span data-stu-id="86274-169">The router handles the requests internally.</span></span>

<span data-ttu-id="86274-170">Si se realiza una solicitud mediante la barra de direcciones del explorador para `www.contoso.com/About`, se produce un error.</span><span class="sxs-lookup"><span data-stu-id="86274-170">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="86274-171">Este recurso no existe en el host de Internet de la aplicación, por lo que se devuelve una respuesta *404 No encontrado*.</span><span class="sxs-lookup"><span data-stu-id="86274-171">No such resource exists on the app's Internet host, so a *404 Not found* response is returned.</span></span>

<span data-ttu-id="86274-172">Dado que los exploradores realizan solicitudes a hosts basados en Internet para las páginas del lado cliente, los servidores web y los servicios de hospedaje deben reescribir todas las solicitudes de recursos que no estén físicamente en el servidor a la página *index.html*.</span><span class="sxs-lookup"><span data-stu-id="86274-172">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="86274-173">Cuando se devuelve *index.html*, el enrutador de la aplicación del lado cliente se hace cargo y responde con el recurso correcto.</span><span class="sxs-lookup"><span data-stu-id="86274-173">When *index.html* is returned, the app's client-side router takes over and responds with the correct resource.</span></span>

### <a name="app-base-path"></a><span data-ttu-id="86274-174">Ruta de acceso base de la aplicación</span><span class="sxs-lookup"><span data-stu-id="86274-174">App base path</span></span>

<span data-ttu-id="86274-175">La ruta de acceso base de la aplicación es la ruta de acceso raíz de la aplicación virtual en el servidor.</span><span class="sxs-lookup"><span data-stu-id="86274-175">The app base path is the virtual app root path on the server.</span></span> <span data-ttu-id="86274-176">Por ejemplo, a una aplicación que reside en el servidor de Contoso, en una carpeta virtual de `/CoolApp/`, se accede desde `https://www.contoso.com/CoolApp`; su ruta de acceso base virtual es `/CoolApp/`.</span><span class="sxs-lookup"><span data-stu-id="86274-176">For example, an app that resides on the Contoso server in a virtual folder at `/CoolApp/` is reached at `https://www.contoso.com/CoolApp` and has a virtual base path of `/CoolApp/`.</span></span> <span data-ttu-id="86274-177">Al establecer la ruta de acceso base de la aplicación en `CoolApp/`, la aplicación sabe dónde reside virtualmente en el servidor.</span><span class="sxs-lookup"><span data-stu-id="86274-177">By setting the app base path to `CoolApp/`, the app is made aware of where it virtually resides on the server.</span></span> <span data-ttu-id="86274-178">La aplicación puede usar la ruta de acceso base de la aplicación para construir direcciones URL relativas a la raíz de la aplicación desde un componente que no se encuentre en el directorio raíz.</span><span class="sxs-lookup"><span data-stu-id="86274-178">The app can use the app base path to construct URLs relative to the app root from a component that isn't in the root directory.</span></span> <span data-ttu-id="86274-179">Esto permite a los componentes que existen en diferentes niveles de la estructura de directorios compilar vínculos a otros recursos en ubicaciones de toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="86274-179">This allows components that exist at different levels of the directory structure to build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="86274-180">La ruta de acceso base de la aplicación también se usa para interceptar clics en hipervínculos en los que el destino `href` del vínculo está dentro del espacio de URI de la ruta de acceso base de la aplicación y es el enrutador de Blazor quien controla la navegación interna.</span><span class="sxs-lookup"><span data-stu-id="86274-180">The app base path is also used to intercept hyperlink clicks where the `href` target of the link is within the app base path URI space&mdash;the Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="86274-181">En muchos escenarios de hospedaje, la ruta de acceso virtual del servidor a la aplicación es la raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="86274-181">In many hosting scenarios, the server's virtual path to the app is the root of the app.</span></span> <span data-ttu-id="86274-182">En estos casos, la ruta de acceso base de la aplicación es una barra diagonal (`<base href="/" />`), que es la configuración predeterminada para una aplicación.</span><span class="sxs-lookup"><span data-stu-id="86274-182">In these cases, the app base path is a forward slash (`<base href="/" />`), which is the default configuration for an app.</span></span> <span data-ttu-id="86274-183">En otros escenarios de hospedaje, como las subaplicaciones o los directorios virtuales de IIS y GitHub Pages, la ruta de acceso base de la aplicación debe establecerse en la ruta de acceso virtual del servidor a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="86274-183">In other hosting scenarios, such as GitHub Pages and IIS virtual directories or sub-applications, the app base path must be set to the server's virtual path to the app.</span></span> <span data-ttu-id="86274-184">Para establecer la ruta de acceso base de la aplicación, agregue o actualice la etiqueta `<base>` en *index.html*, que se encuentra dentro de los elementos de etiqueta `<head>`.</span><span class="sxs-lookup"><span data-stu-id="86274-184">To set the app's base path, add or update the `<base>` tag in *index.html* found within the `<head>` tag elements.</span></span> <span data-ttu-id="86274-185">Establezca el valor del atributo `href` en `<virtual-path>/` (la barra diagonal final es necesaria), donde `<virtual-path>/` es la ruta de acceso raíz de la aplicación virtual completa en el servidor para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="86274-185">Set the `href` attribute value to `<virtual-path>/` (the trailing slash is required), where `<virtual-path>/` is the full virtual app root path on the server for the app.</span></span> <span data-ttu-id="86274-186">En el ejemplo anterior, se establece la ruta de acceso virtual en `CoolApp/`: `<base href="CoolApp/" />`.</span><span class="sxs-lookup"><span data-stu-id="86274-186">In the preceding example, the virtual path is set to `CoolApp/`: `<base href="CoolApp/" />`.</span></span>

<span data-ttu-id="86274-187">En el caso de una aplicación con una ruta de acceso virtual que no es raíz configurada (por ejemplo, `<base href="CoolApp/" />`), la aplicación no puede encontrar sus recursos *cuando se ejecuta de forma local*.</span><span class="sxs-lookup"><span data-stu-id="86274-187">For an app with a non-root virtual path configured (for example, `<base href="CoolApp/" />`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="86274-188">Para solucionar este problema durante la fase de desarrollo y pruebas local, puede proporcionar un argumento de *ruta de acceso base* que coincida con el valor `href` de la etiqueta `<base>` en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="86274-188">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span>

<span data-ttu-id="86274-189">Para pasar el argumento de ruta de acceso base con la ruta de acceso raíz (`/`) al ejecutar la aplicación de forma local, ejecute el siguiente comando desde el directorio de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="86274-189">To pass the path base argument with the root path (`/`) when running the app locally, execute the following command from the app's directory:</span></span>

```console
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="86274-190">La aplicación responde de forma local en `http://localhost:port/CoolApp`.</span><span class="sxs-lookup"><span data-stu-id="86274-190">The app responds locally at `http://localhost:port/CoolApp`.</span></span>

<span data-ttu-id="86274-191">Para obtener más información, consulte la sección sobre el [valor de configuración de host de la ruta de acceso base](#path-base).</span><span class="sxs-lookup"><span data-stu-id="86274-191">For more information, see the [path base host configuration value](#path-base) section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="86274-192">Si una aplicación usa el [modelo de hospedaje del lado cliente](xref:razor-components/hosting-models#client-side-hosting-model) (basado en la plantilla de proyecto de **Blazor**) y se hospeda como una subaplicación de IIS en una aplicación ASP.NET Core, es importante deshabilitar el controlador del módulo de ASP.NET Core heredado o asegurarse de que la subaplicación no hereda la sección `<handlers>` de la aplicación raíz (principal) en el archivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="86274-192">If an app uses the [client-side hosting model](xref:razor-components/hosting-models#client-side-hosting-model) (based on the **Blazor** project template) and is hosted as an IIS sub-application in an ASP.NET Core app, it's important to disable the inherited ASP.NET Core Module handler or make sure the root (parent) app's `<handlers>` section in the *web.config* file isn't inherited by the sub-app.</span></span>
>
> <span data-ttu-id="86274-193">Para quitar el controlador del archivo *web.config* publicado de la aplicación, agregue una sección `<handlers>` al archivo:</span><span class="sxs-lookup"><span data-stu-id="86274-193">Remove the handler in the app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>
>
> ```xml
> <handlers>
>   <remove name="aspNetCore" />
> </handlers>
> ```
>
> <span data-ttu-id="86274-194">Como alternativa, deshabilite la herencia de la sección `<system.webServer>` de la aplicación raíz (principal) mediante un elemento `<location>` con `inheritInChildApplications` establecido en `false`:</span><span class="sxs-lookup"><span data-stu-id="86274-194">Alternatively, disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>
>
> ```xml
> <?xml version="1.0" encoding="utf-8"?>
> <configuration>
>  <location path="." inheritInChildApplications="false">
>    <system.webServer>
>      <handlers>
>        <add name="aspNetCore" ... />
>      </handlers>
>      <aspNetCore ... />
>    </system.webServer>
>  </location>
> </configuration>
> ```
>
> <span data-ttu-id="86274-195">Además de configurarse la ruta de acceso base de la aplicación, se quita el controlador o se deshabilita la herencia, como se describe en esta sección.</span><span class="sxs-lookup"><span data-stu-id="86274-195">Removing the handler or disabling inheritance is performed in addition to configuring the app's base path as described in this section.</span></span> <span data-ttu-id="86274-196">Establezca la ruta de acceso base de la aplicación en el archivo *index.html* de la aplicación en el alias de IIS que ha usado al configurar la subaplicación en IIS.</span><span class="sxs-lookup"><span data-stu-id="86274-196">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

### <a name="client-side-blazor-hosted-deployment-with-aspnet-core"></a><span data-ttu-id="86274-197">Implementación hospedada en Blazor del lado cliente con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="86274-197">Client-side Blazor hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="86274-198">Una *implementación hospedada* proporciona la aplicación Blazor del lado cliente a los exploradores desde una [aplicación ASP.NET Core](xref:index) que se ejecuta en un servidor.</span><span class="sxs-lookup"><span data-stu-id="86274-198">A *hosted deployment* serves the client-side Blazor app to browsers from an [ASP.NET Core app](xref:index) that runs on a server.</span></span>

<span data-ttu-id="86274-199">La aplicación Blazor se incluye con la aplicación ASP.NET Core en la salida publicada para que ambas se implementen juntas.</span><span class="sxs-lookup"><span data-stu-id="86274-199">The Blazor app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="86274-200">Se requiere un servidor web que pueda hospedar una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="86274-200">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="86274-201">En el caso de una implementación hospedada, Visual Studio incluye la plantilla de proyecto de **Blazor (hospedada en ASP.NET Core)** (la plantilla `blazorhosted` al usar el comando [dotnet new](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="86274-201">For a hosted deployment, Visual Studio includes the **Blazor (ASP.NET Core hosted)** project template (`blazorhosted` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<span data-ttu-id="86274-202">Para obtener más información sobre la implementación y el hospedaje de aplicaciones de ASP.NET Core, consulte <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="86274-202">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="86274-203">Para obtener información sobre cómo implementar en Azure App Service, consulte los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="86274-203">For information on deploying to Azure App Service, see the following topics:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>

<span data-ttu-id="86274-204">Obtenga información sobre cómo publicar una aplicación de ASP.NET Core en Azure App Service con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="86274-204">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

### <a name="client-side-blazor-standalone-deployment"></a><span data-ttu-id="86274-205">Implementación independiente de Blazor del lado cliente</span><span class="sxs-lookup"><span data-stu-id="86274-205">Client-side Blazor standalone deployment</span></span>

<span data-ttu-id="86274-206">Una *implementación independiente* proporciona la aplicación Blazor del lado cliente como un conjunto de archivos estáticos que los clientes solicitan directamente.</span><span class="sxs-lookup"><span data-stu-id="86274-206">A *standalone deployment* serves the client-side Blazor app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="86274-207">No se usa ningún servidor web para proporcionar la aplicación Blazor.</span><span class="sxs-lookup"><span data-stu-id="86274-207">A web server isn't used to serve the Blazor app.</span></span>

#### <a name="client-side-blazor-standalone-hosting-with-iis"></a><span data-ttu-id="86274-208">Hospedaje independiente de Blazor del lado cliente con IIS</span><span class="sxs-lookup"><span data-stu-id="86274-208">Client-side Blazor standalone hosting with IIS</span></span>

<span data-ttu-id="86274-209">IIS es un servidor de archivos estáticos compatible con las aplicaciones de Blazor.</span><span class="sxs-lookup"><span data-stu-id="86274-209">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="86274-210">Para configurar IIS para hospedar Blazor, vea [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis) (Compilación de un sitio web estático en IIS).</span><span class="sxs-lookup"><span data-stu-id="86274-210">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="86274-211">Los recursos publicados se crean en la carpeta *\\bin\\Release\\&lt;plataforma-de-destino&gt;\\publish*.</span><span class="sxs-lookup"><span data-stu-id="86274-211">Published assets are created in the *\\bin\\Release\\&lt;target-framework&gt;\\publish* folder.</span></span> <span data-ttu-id="86274-212">Hospede el contenido de la carpeta *publish* en el servidor web o el servicio de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="86274-212">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

<span data-ttu-id="86274-213">**web.config**</span><span class="sxs-lookup"><span data-stu-id="86274-213">**web.config**</span></span>

<span data-ttu-id="86274-214">Cuando se publica un proyecto de Blazor, se crea un archivo *web.config* con la siguiente configuración de IIS:</span><span class="sxs-lookup"><span data-stu-id="86274-214">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="86274-215">Se establecen los tipos MIME de las siguientes extensiones de archivo:</span><span class="sxs-lookup"><span data-stu-id="86274-215">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="86274-216">*\*.dll*: `application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="86274-216">*\*.dll*: `application/octet-stream`</span></span>
  * <span data-ttu-id="86274-217">*\*.json*: `application/json`</span><span class="sxs-lookup"><span data-stu-id="86274-217">*\*.json*: `application/json`</span></span>
  * <span data-ttu-id="86274-218">*\*.wasm*: `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="86274-218">*\*.wasm*: `application/wasm`</span></span>
  * <span data-ttu-id="86274-219">*\*.woff*: `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="86274-219">*\*.woff*: `application/font-woff`</span></span>
  * <span data-ttu-id="86274-220">*\*.woff2*: `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="86274-220">*\*.woff2*: `application/font-woff`</span></span>
* <span data-ttu-id="86274-221">Se habilita la compresión HTTP de los siguientes tipos MIME:</span><span class="sxs-lookup"><span data-stu-id="86274-221">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="86274-222">Se establecen reglas del módulo URL Rewrite:</span><span class="sxs-lookup"><span data-stu-id="86274-222">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="86274-223">Proporcionar el subdirectorio donde residen los recursos estáticos de la aplicación (*&lt;nombre_ensamblado&gt;\\dist\\&lt;ruta_acceso_solicitada&gt;*).</span><span class="sxs-lookup"><span data-stu-id="86274-223">Serve the sub-directory where the app's static assets reside (*&lt;assembly_name&gt;\\dist\\&lt;path_requested&gt;*).</span></span>
  * <span data-ttu-id="86274-224">Crear el enrutamiento de reserva de SPA para que las solicitudes de recursos que no sean archivos se redirijan al documento predeterminado de la aplicación en su carpeta de recursos estáticos (*&lt;nombre_ensamblado&gt;\\dist\\index.html*).</span><span class="sxs-lookup"><span data-stu-id="86274-224">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*&lt;assembly_name&gt;\\dist\\index.html*).</span></span>

<span data-ttu-id="86274-225">**Instalación del módulo URL Rewrite**</span><span class="sxs-lookup"><span data-stu-id="86274-225">**Install the URL Rewrite Module**</span></span>

<span data-ttu-id="86274-226">El [módulo URL Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite) es necesario para reescribir las URL.</span><span class="sxs-lookup"><span data-stu-id="86274-226">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="86274-227">El módulo no se instala de forma predeterminada y no está disponible para instalar como una característica de servicio de rol del servidor web (IIS).</span><span class="sxs-lookup"><span data-stu-id="86274-227">The module isn't installed by default, and it isn't available for install as an Web Server (IIS) role service feature.</span></span> <span data-ttu-id="86274-228">El módulo se debe descargar desde el sitio web de IIS.</span><span class="sxs-lookup"><span data-stu-id="86274-228">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="86274-229">Use el instalador de plataforma web para instalar el módulo:</span><span class="sxs-lookup"><span data-stu-id="86274-229">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="86274-230">De forma local, vaya a la [página de descargas del módulo URL Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span><span class="sxs-lookup"><span data-stu-id="86274-230">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="86274-231">En el caso de la versión en inglés, seleccione **WebPI** para descargar el instalador de WebPI.</span><span class="sxs-lookup"><span data-stu-id="86274-231">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="86274-232">En el caso de otros idiomas, seleccione la arquitectura adecuada del servidor (x86/x64) para descargar el instalador.</span><span class="sxs-lookup"><span data-stu-id="86274-232">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="86274-233">Copie el instalador en el servidor.</span><span class="sxs-lookup"><span data-stu-id="86274-233">Copy the installer to the server.</span></span> <span data-ttu-id="86274-234">Ejecute el instalador.</span><span class="sxs-lookup"><span data-stu-id="86274-234">Run the installer.</span></span> <span data-ttu-id="86274-235">Haga clic en el botón **Instalar** y acepte los términos de licencia.</span><span class="sxs-lookup"><span data-stu-id="86274-235">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="86274-236">No es necesario reiniciar el servidor al finalizar la instalación.</span><span class="sxs-lookup"><span data-stu-id="86274-236">A server restart isn't required after the install completes.</span></span>

<span data-ttu-id="86274-237">**Configuración del sitio web**</span><span class="sxs-lookup"><span data-stu-id="86274-237">**Configure the website**</span></span>

<span data-ttu-id="86274-238">Configure la **ruta de acceso física** del sitio web a la carpeta de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="86274-238">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="86274-239">La carpeta contiene:</span><span class="sxs-lookup"><span data-stu-id="86274-239">The folder contains:</span></span>

* <span data-ttu-id="86274-240">El archivo *web.config* que usa IIS para configurar el sitio web, incluidas las reglas de redireccionamiento y los tipos de contenido de archivos necesarios.</span><span class="sxs-lookup"><span data-stu-id="86274-240">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="86274-241">La carpeta de recursos estáticos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="86274-241">The app's static asset folder.</span></span>

<span data-ttu-id="86274-242">**Solución de problemas**</span><span class="sxs-lookup"><span data-stu-id="86274-242">**Troubleshooting**</span></span>

<span data-ttu-id="86274-243">Si se recibe un error *500 Error interno del servidor* y el administrador de IIS produce errores al intentar acceder a la configuración del sitio web, confirme que el módulo URL Rewrite esté instalado.</span><span class="sxs-lookup"><span data-stu-id="86274-243">If a *500 Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="86274-244">Si no lo está, IIS no puede analizar el archivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="86274-244">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="86274-245">Esto impide que el Administrador de IIS cargue la configuración del sitio web y que el sitio web proporcione los archivos estáticos de Blazor.</span><span class="sxs-lookup"><span data-stu-id="86274-245">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="86274-246">Para obtener más información sobre cómo solucionar problemas de las implementaciones en IIS, vea <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="86274-246">For more information on troubleshooting deployments to IIS, see <xref:host-and-deploy/iis/troubleshoot>.</span></span>

#### <a name="client-side-blazor-standalone-hosting-with-nginx"></a><span data-ttu-id="86274-247">Hospedaje independiente de Blazor del lado cliente con Nginx</span><span class="sxs-lookup"><span data-stu-id="86274-247">Client-side Blazor standalone hosting with Nginx</span></span>

<span data-ttu-id="86274-248">El siguiente archivo *nginx.conf* se ha simplificado para mostrar cómo configurar Nginx para enviar el archivo *Index.html* siempre que no pueda encontrar un archivo correspondiente en el disco.</span><span class="sxs-lookup"><span data-stu-id="86274-248">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *Index.html* file whenever it can't find a corresponding file on disk.</span></span>

```
events { }
http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            try_files $uri $uri/ /Index.html =404;
        }
    }
}
```

<span data-ttu-id="86274-249">Para obtener más información sobre la configuración del servidor web de producción de Nginx, consulte [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/) (Creación de archivos de configuración de NGINX y NGINX Plus).</span><span class="sxs-lookup"><span data-stu-id="86274-249">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

#### <a name="client-side-blazor-standalone-hosting-with-nginx-in-docker"></a><span data-ttu-id="86274-250">Hospedaje independiente de Blazor del lado cliente con Nginx en Docker</span><span class="sxs-lookup"><span data-stu-id="86274-250">Client-side Blazor standalone hosting with Nginx in Docker</span></span>

<span data-ttu-id="86274-251">Para hospedar Blazor en Docker mediante Nginx, configure el Dockerfile para usar la imagen de Nginx basada en Alpine.</span><span class="sxs-lookup"><span data-stu-id="86274-251">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="86274-252">Actualice el Dockerfile para copiar el archivo *nginx.config* en el contenedor.</span><span class="sxs-lookup"><span data-stu-id="86274-252">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="86274-253">Agregue una línea al Dockerfile, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="86274-253">Add one line to the Dockerfile, as shown in the following example:</span></span>

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

#### <a name="client-side-blazor-standalone-hosting-with-github-pages"></a><span data-ttu-id="86274-254">Hospedaje independiente de Blazor del lado cliente con GitHub Pages</span><span class="sxs-lookup"><span data-stu-id="86274-254">Client-side Blazor standalone hosting with GitHub Pages</span></span>

<span data-ttu-id="86274-255">Para controlar las reescrituras de URL, agregue un archivo *404.html* con un script que controle el redireccionamiento de la solicitud a la página *index.html*.</span><span class="sxs-lookup"><span data-stu-id="86274-255">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="86274-256">Para consultar una implementación de ejemplo que ha proporcionado la comunidad, vea [Single Page Apps for GitHub Pages](http://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)) (Aplicaciones de página única para GitHub Pages [rafrex/spa-github-pages en GitHub]).</span><span class="sxs-lookup"><span data-stu-id="86274-256">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](http://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="86274-257">Se puede ver un ejemplo del enfoque de la comunidad en [blazor-demo/blazor-demo.github.io en GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([sitio activo](https://blazor-demo.github.io/)).</span><span class="sxs-lookup"><span data-stu-id="86274-257">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="86274-258">Al usar un sitio de proyecto en lugar de un sitio de la organización, agregue o actualice la etiqueta `<base>` en *index.html*.</span><span class="sxs-lookup"><span data-stu-id="86274-258">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="86274-259">Establezca el valor del atributo `href` en `<repository-name>/`, donde `<repository-name>/` es el nombre del repositorio de GitHub.</span><span class="sxs-lookup"><span data-stu-id="86274-259">Set the `href` attribute value to `<repository-name>/`, where `<repository-name>/` is the GitHub repository name.</span></span>

## <a name="deploy-a-server-side-razor-components-app"></a><span data-ttu-id="86274-260">Implementación de una aplicación Razor Components del lado servidor</span><span class="sxs-lookup"><span data-stu-id="86274-260">Deploy a server-side Razor Components app</span></span>

<span data-ttu-id="86274-261">Con el [modelo de hospedaje del lado servidor](xref:razor-components/hosting-models#server-side-hosting-model), Razor Components se ejecuta en el servidor desde una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="86274-261">With the [server-side hosting model](xref:razor-components/hosting-models#server-side-hosting-model), Razor Components is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="86274-262">Las actualizaciones de la interfaz de usuario, el control de eventos y las llamadas de JavaScript se controlan mediante una conexión de SignalR.</span><span class="sxs-lookup"><span data-stu-id="86274-262">UI updates, event handling, and JavaScript calls are handled over a SignalR connection.</span></span>

<span data-ttu-id="86274-263">La aplicación se incluye con la aplicación ASP.NET Core en la salida publicada para que ambas se implementen juntas.</span><span class="sxs-lookup"><span data-stu-id="86274-263">The app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="86274-264">Se requiere un servidor web que pueda hospedar una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="86274-264">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="86274-265">En el caso de una implementación del lado servidor, Visual Studio incluye la plantilla de proyecto **Componentes de Razor** (la plantilla `razorcomponents` al usar el comando [dotnet new](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="86274-265">For a server-side deployment, Visual Studio includes the **Razor Components** project template (`razorcomponents` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

<span data-ttu-id="86274-266">Cuando se publica la aplicación ASP.NET Core, la aplicación Razor Components se incluye en la salida publicada para que ambas se puedan implementar juntas.</span><span class="sxs-lookup"><span data-stu-id="86274-266">When the ASP.NET Core app is published, the Razor Components app is included in the published output so that the ASP.NET Core app and the Razor Components app can be deployed together.</span></span> <span data-ttu-id="86274-267">Para obtener más información sobre la implementación y el hospedaje de aplicaciones de ASP.NET Core, consulte <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="86274-267">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="86274-268">Para obtener información sobre cómo implementar en Azure App Service, consulte los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="86274-268">For information on deploying to Azure App Service, see the following topics:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>

<span data-ttu-id="86274-269">Obtenga información sobre cómo publicar una aplicación de ASP.NET Core en Azure App Service con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="86274-269">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>
