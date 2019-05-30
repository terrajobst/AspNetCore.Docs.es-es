---
title: Hospedaje e implementación de Blazor del lado cliente
author: guardrex
description: Obtenga información sobre cómo hospedar e implementar una aplicación de Blazor con ASP.NET Core, redes de entrega de contenido (CDN), servidores de archivos y GitHub Pages.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/21/2019
uid: host-and-deploy/blazor/client-side
ms.openlocfilehash: b572067e688d7e7f7c654a7a25703009c1a7e855
ms.sourcegitcommit: e1623d8279b27ff83d8ad67a1e7ef439259decdf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2019
ms.locfileid: "66223185"
---
# <a name="host-and-deploy-blazor-client-side"></a><span data-ttu-id="54213-103">Hospedaje e implementación de Blazor del lado cliente</span><span class="sxs-lookup"><span data-stu-id="54213-103">Host and deploy Blazor client-side</span></span>

<span data-ttu-id="54213-104">Por [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) y [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="54213-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="54213-105">Valores de configuración de host</span><span class="sxs-lookup"><span data-stu-id="54213-105">Host configuration values</span></span>

<span data-ttu-id="54213-106">Las aplicaciones de Blazor que usan el [modelo de hospedaje del lado cliente](xref:blazor/hosting-models#client-side) pueden aceptar los siguientes valores de configuración de host como argumentos de línea de comandos en tiempo de ejecución en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="54213-106">Blazor apps that use the [client-side hosting model](xref:blazor/hosting-models#client-side) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="54213-107">Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="54213-107">Content Root</span></span>

<span data-ttu-id="54213-108">El argumento `--contentroot` establece la ruta de acceso absoluta al directorio que incluye los archivos de contenido de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="54213-108">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files.</span></span> <span data-ttu-id="54213-109">En los ejemplos siguientes, `/content-root-path` es la ruta de acceso raíz del contenido de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="54213-109">In the following examples, `/content-root-path` is the app's content root path.</span></span>

* <span data-ttu-id="54213-110">Pase el argumento al ejecutar la aplicación de forma local en un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="54213-110">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="54213-111">En el directorio de la aplicación, ejecute lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="54213-111">From the app's directory, execute:</span></span>

  ```console
  dotnet run --contentroot=/content-root-path
  ```

* <span data-ttu-id="54213-112">Agregue una entrada al archivo *launchSettings.json* de la aplicación en el perfil **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="54213-112">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="54213-113">Esta configuración se utiliza cuando se ejecuta la aplicación mediante el depurador de Visual Studio y desde un símbolo del sistema con `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="54213-113">This setting is used when the app is run with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* <span data-ttu-id="54213-114">En Visual Studio, especifique el argumento en **Propiedades** > **Depuración** > **Argumentos de la aplicación**.</span><span class="sxs-lookup"><span data-stu-id="54213-114">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="54213-115">Al establecer el argumento en la página de propiedades de Visual Studio, se agrega el argumento al archivo *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="54213-115">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a><span data-ttu-id="54213-116">Ruta de acceso base</span><span class="sxs-lookup"><span data-stu-id="54213-116">Path base</span></span>

<span data-ttu-id="54213-117">El argumento `--pathbase` establece la ruta de acceso base de la aplicación para una aplicación que se ejecuta localmente con una ruta de acceso virtual que no es raíz (el valor `href` de la etiqueta `<base>` se establece en una ruta de acceso que no sea `/` para ensayo y producción).</span><span class="sxs-lookup"><span data-stu-id="54213-117">The `--pathbase` argument sets the app base path for an app run locally with a non-root virtual path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="54213-118">En los ejemplos siguientes, `/virtual-path` es la ruta de acceso base de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="54213-118">In the following examples, `/virtual-path` is the app's path base.</span></span> <span data-ttu-id="54213-119">Para obtener más información, vea la sección [Ruta de acceso base de la aplicación](#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="54213-119">For more information, see the [App base path](#app-base-path) section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="54213-120">A diferencia de la ruta de acceso proporcionada al valor `href` de la etiqueta `<base>`, no incluya una barra diagonal final (`/`) al pasar el valor del argumento `--pathbase`.</span><span class="sxs-lookup"><span data-stu-id="54213-120">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="54213-121">Si se proporciona la ruta de acceso base de la aplicación en la etiqueta `<base>` como `<base href="/CoolApp/">` (se incluye una barra diagonal final), se pasa el valor del argumento de línea de comandos como `--pathbase=/CoolApp` (sin barra diagonal final).</span><span class="sxs-lookup"><span data-stu-id="54213-121">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/">` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="54213-122">Pase el argumento al ejecutar la aplicación de forma local en un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="54213-122">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="54213-123">En el directorio de la aplicación, ejecute lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="54213-123">From the app's directory, execute:</span></span>

  ```console
  dotnet run --pathbase=/virtual-path
  ```

* <span data-ttu-id="54213-124">Agregue una entrada al archivo *launchSettings.json* de la aplicación en el perfil **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="54213-124">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="54213-125">Esta configuración se utiliza cuando se ejecuta la aplicación mediante el depurador de Visual Studio y desde un símbolo del sistema con `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="54213-125">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/virtual-path"
  ```

* <span data-ttu-id="54213-126">En Visual Studio, especifique el argumento en **Propiedades** > **Depuración** > **Argumentos de la aplicación**.</span><span class="sxs-lookup"><span data-stu-id="54213-126">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="54213-127">Al establecer el argumento en la página de propiedades de Visual Studio, se agrega el argumento al archivo *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="54213-127">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/virtual-path
  ```

### <a name="urls"></a><span data-ttu-id="54213-128">Direcciones URL</span><span class="sxs-lookup"><span data-stu-id="54213-128">URLs</span></span>

<span data-ttu-id="54213-129">El argumento `--urls` establece las direcciones IP o las direcciones de host con los puertos y protocolos en los que escuchar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="54213-129">The `--urls` argument sets the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="54213-130">Pase el argumento al ejecutar la aplicación de forma local en un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="54213-130">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="54213-131">En el directorio de la aplicación, ejecute lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="54213-131">From the app's directory, execute:</span></span>

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="54213-132">Agregue una entrada al archivo *launchSettings.json* de la aplicación en el perfil **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="54213-132">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="54213-133">Esta configuración se utiliza cuando se ejecuta la aplicación mediante el depurador de Visual Studio y desde un símbolo del sistema con `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="54213-133">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="54213-134">En Visual Studio, especifique el argumento en **Propiedades** > **Depuración** > **Argumentos de la aplicación**.</span><span class="sxs-lookup"><span data-stu-id="54213-134">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="54213-135">Al establecer el argumento en la página de propiedades de Visual Studio, se agrega el argumento al archivo *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="54213-135">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="deployment"></a><span data-ttu-id="54213-136">Implementación</span><span class="sxs-lookup"><span data-stu-id="54213-136">Deployment</span></span>

<span data-ttu-id="54213-137">Con el [modelo de hospedaje del lado cliente](xref:blazor/hosting-models#client-side):</span><span class="sxs-lookup"><span data-stu-id="54213-137">With the [client-side hosting model](xref:blazor/hosting-models#client-side):</span></span>

* <span data-ttu-id="54213-138">La aplicación Blazor, sus dependencias y el entorno de ejecución de .NET se descargan en el explorador.</span><span class="sxs-lookup"><span data-stu-id="54213-138">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="54213-139">La aplicación se ejecuta directamente en el subproceso de interfaz de usuario del explorador.</span><span class="sxs-lookup"><span data-stu-id="54213-139">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="54213-140">Se puede seguir cualquiera de las estrategias siguientes:</span><span class="sxs-lookup"><span data-stu-id="54213-140">Either of the following strategies is supported:</span></span>
  * <span data-ttu-id="54213-141">Una aplicación ASP.NET Core proporciona la aplicación Blazor.</span><span class="sxs-lookup"><span data-stu-id="54213-141">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="54213-142">Esta estrategia se trata en la sección [Implementación hospedada con ASP.NET Core](#hosted-deployment-with-aspnet-core).</span><span class="sxs-lookup"><span data-stu-id="54213-142">This strategy is covered in the [Hosted deployment with ASP.NET Core](#hosted-deployment-with-aspnet-core) section.</span></span>
  * <span data-ttu-id="54213-143">La aplicación Blazor se coloca en un servicio o servidor web de hospedaje estático, donde no se usa .NET para proporcionar la aplicación Blazor.</span><span class="sxs-lookup"><span data-stu-id="54213-143">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="54213-144">Esta estrategia se trata en la sección [Implementación independiente](#standalone-deployment).</span><span class="sxs-lookup"><span data-stu-id="54213-144">This strategy is covered in the [Standalone deployment](#standalone-deployment) section.</span></span>

## <a name="configure-the-linker"></a><span data-ttu-id="54213-145">Configurar el enlazador</span><span class="sxs-lookup"><span data-stu-id="54213-145">Configure the Linker</span></span>

<span data-ttu-id="54213-146">Blazor realiza la vinculación de lenguaje intermedio (IL) en cada compilación para quitar el IL innecesario de los ensamblados de salida.</span><span class="sxs-lookup"><span data-stu-id="54213-146">Blazor performs Intermediate Language (IL) linking on each build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="54213-147">La vinculación de ensamblados puede controlarse en la compilación.</span><span class="sxs-lookup"><span data-stu-id="54213-147">Assembly linking can be controlled on build.</span></span> <span data-ttu-id="54213-148">Para obtener más información, vea <xref:host-and-deploy/blazor/configure-linker>.</span><span class="sxs-lookup"><span data-stu-id="54213-148">For more information, see <xref:host-and-deploy/blazor/configure-linker>.</span></span>

## <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="54213-149">Reescritura de las URL para conseguir un enrutamiento correcto</span><span class="sxs-lookup"><span data-stu-id="54213-149">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="54213-150">Enrutar las solicitudes para los componentes de la página en una aplicación del lado cliente no es tan sencillo como enrutar las solicitudes a una aplicación hospedada del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="54213-150">Routing requests for page components in a client-side app isn't as simple as routing requests to a server-side, hosted app.</span></span> <span data-ttu-id="54213-151">Imagine que tiene una aplicación del lado cliente con dos páginas:</span><span class="sxs-lookup"><span data-stu-id="54213-151">Consider a client-side app with two pages:</span></span>

* <span data-ttu-id="54213-152">**_Main.razor**: se carga en la raíz de la aplicación y contiene un vínculo a la página de información (`href="About"`).</span><span class="sxs-lookup"><span data-stu-id="54213-152">**_Main.razor** &ndash; Loads at the root of the app and contains a link to the About page (`href="About"`).</span></span>
* <span data-ttu-id="54213-153">**_About.Razor**: página Acerca de.</span><span class="sxs-lookup"><span data-stu-id="54213-153">**_About.razor** &ndash; About page.</span></span>

<span data-ttu-id="54213-154">Cuando se solicita el documento predeterminado de la aplicación mediante la barra de direcciones del explorador (por ejemplo, `https://www.contoso.com/`):</span><span class="sxs-lookup"><span data-stu-id="54213-154">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="54213-155">El explorador realiza una solicitud.</span><span class="sxs-lookup"><span data-stu-id="54213-155">The browser makes a request.</span></span>
1. <span data-ttu-id="54213-156">Se devuelve la página predeterminada, que suele ser *index.html*.</span><span class="sxs-lookup"><span data-stu-id="54213-156">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="54213-157">*index.html* arranca la aplicación.</span><span class="sxs-lookup"><span data-stu-id="54213-157">*index.html* bootstraps the app.</span></span>
1. <span data-ttu-id="54213-158">Se carga el enrutador de Blazor y se muestra la página principal de Razor (*Main.razor*).</span><span class="sxs-lookup"><span data-stu-id="54213-158">Blazor's router loads, and the Razor Main page (*Main.razor*) is displayed.</span></span>

<span data-ttu-id="54213-159">En la página principal, al seleccionar el vínculo a la página de información, se carga la página de información.</span><span class="sxs-lookup"><span data-stu-id="54213-159">On the Main page, selecting the link to the About page loads the About page.</span></span> <span data-ttu-id="54213-160">Seleccionar el vínculo a la página de información funciona en el cliente porque el enrutador de Blazor impide que el explorador realice una solicitud en Internet a `www.contoso.com` sobre `About` y presenta la propia página de información.</span><span class="sxs-lookup"><span data-stu-id="54213-160">Selecting the link to the About page works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the About page itself.</span></span> <span data-ttu-id="54213-161">Todas las solicitudes de páginas internas *dentro de la aplicación del lado cliente* funcionan del mismo modo: Las solicitudes no desencadenan solicitudes basadas en el explorador a recursos hospedados en el servidor en Internet.</span><span class="sxs-lookup"><span data-stu-id="54213-161">All of the requests for internal pages *within the client-side app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="54213-162">El enrutador controla las solicitudes de forma interna.</span><span class="sxs-lookup"><span data-stu-id="54213-162">The router handles the requests internally.</span></span>

<span data-ttu-id="54213-163">Si se realiza una solicitud mediante la barra de direcciones del explorador para `www.contoso.com/About`, se produce un error.</span><span class="sxs-lookup"><span data-stu-id="54213-163">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="54213-164">Este recurso no existe en el host de Internet de la aplicación, por lo que se devuelve una respuesta *404 No encontrado*.</span><span class="sxs-lookup"><span data-stu-id="54213-164">No such resource exists on the app's Internet host, so a *404 - Not Found* response is returned.</span></span>

<span data-ttu-id="54213-165">Dado que los exploradores realizan solicitudes a hosts basados en Internet para las páginas del lado cliente, los servidores web y los servicios de hospedaje deben reescribir todas las solicitudes de recursos que no estén físicamente en el servidor a la página *index.html*.</span><span class="sxs-lookup"><span data-stu-id="54213-165">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="54213-166">Cuando se devuelve *index.html*, el enrutador de la aplicación del lado cliente se hace cargo y responde con el recurso correcto.</span><span class="sxs-lookup"><span data-stu-id="54213-166">When *index.html* is returned, the app's client-side router takes over and responds with the correct resource.</span></span>

## <a name="app-base-path"></a><span data-ttu-id="54213-167">Ruta de acceso base de la aplicación</span><span class="sxs-lookup"><span data-stu-id="54213-167">App base path</span></span>

<span data-ttu-id="54213-168">La *ruta de acceso base de la aplicación* es la ruta de acceso raíz de la aplicación virtual en el servidor.</span><span class="sxs-lookup"><span data-stu-id="54213-168">The *app base path* is the virtual app root path on the server.</span></span> <span data-ttu-id="54213-169">Por ejemplo, a una aplicación que reside en el servidor de Contoso, en una carpeta virtual de `/CoolApp/`, se accede desde `https://www.contoso.com/CoolApp`; su ruta de acceso base virtual es `/CoolApp/`.</span><span class="sxs-lookup"><span data-stu-id="54213-169">For example, an app that resides on the Contoso server in a virtual folder at `/CoolApp/` is reached at `https://www.contoso.com/CoolApp` and has a virtual base path of `/CoolApp/`.</span></span> <span data-ttu-id="54213-170">Al establecer la ruta de acceso base de la aplicación en la ruta de acceso virtual (`<base href="/CoolApp/">`), la aplicación sabe dónde reside virtualmente en el servidor.</span><span class="sxs-lookup"><span data-stu-id="54213-170">By setting the app base path to the virtual path (`<base href="/CoolApp/">`), the app is made aware of where it virtually resides on the server.</span></span> <span data-ttu-id="54213-171">La aplicación puede usar la ruta de acceso base de la aplicación para construir direcciones URL relativas a la raíz de la aplicación desde un componente que no se encuentre en el directorio raíz.</span><span class="sxs-lookup"><span data-stu-id="54213-171">The app can use the app base path to construct URLs relative to the app root from a component that isn't in the root directory.</span></span> <span data-ttu-id="54213-172">Esto permite a los componentes que existen en diferentes niveles de la estructura de directorios compilar vínculos a otros recursos en ubicaciones de toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="54213-172">This allows components that exist at different levels of the directory structure to build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="54213-173">La ruta de acceso base de la aplicación también se usa para interceptar clics en hipervínculos en los que el destino `href` del vínculo está dentro del espacio de URI de la ruta de acceso base de la aplicación y es el enrutador de Blazor quien controla la navegación interna.</span><span class="sxs-lookup"><span data-stu-id="54213-173">The app base path is also used to intercept hyperlink clicks where the `href` target of the link is within the app base path URI space&mdash;the Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="54213-174">En muchos escenarios de hospedaje, la ruta de acceso virtual del servidor a la aplicación es la raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="54213-174">In many hosting scenarios, the server's virtual path to the app is the root of the app.</span></span> <span data-ttu-id="54213-175">En estos casos, la ruta de acceso base de la aplicación es una barra diagonal (`<base href="/" />`), que es la configuración predeterminada para una aplicación.</span><span class="sxs-lookup"><span data-stu-id="54213-175">In these cases, the app base path is a forward slash (`<base href="/" />`), which is the default configuration for an app.</span></span> <span data-ttu-id="54213-176">En otros escenarios de hospedaje, como las subaplicaciones o los directorios virtuales de IIS y GitHub Pages, la ruta de acceso base de la aplicación debe establecerse en la ruta de acceso virtual del servidor a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="54213-176">In other hosting scenarios, such as GitHub Pages and IIS virtual directories or sub-applications, the app base path must be set to the server's virtual path to the app.</span></span> <span data-ttu-id="54213-177">Para establecer la ruta de acceso base de la aplicación, actualice la etiqueta `<base>` dentro de los elementos de etiqueta `<head>` del archivo *wwwroot/index.HTML*.</span><span class="sxs-lookup"><span data-stu-id="54213-177">To set the app's base path, update the `<base>` tag within the `<head>` tag elements of the *wwwroot/index.html* file.</span></span> <span data-ttu-id="54213-178">Establezca el valor del atributo `href` en `/virtual-path/` (la barra diagonal final es necesaria), donde `/virtual-path/` es la ruta de acceso raíz de la aplicación virtual completa en el servidor para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="54213-178">Set the `href` attribute value to `/virtual-path/` (the trailing slash is required), where `/virtual-path/` is the full virtual app root path on the server for the app.</span></span> <span data-ttu-id="54213-179">En el ejemplo anterior, se establece la ruta de acceso virtual en `/CoolApp/`: `<base href="/CoolApp/">`.</span><span class="sxs-lookup"><span data-stu-id="54213-179">In the preceding example, the virtual path is set to `/CoolApp/`: `<base href="/CoolApp/">`.</span></span>

<span data-ttu-id="54213-180">En el caso de una aplicación con una ruta de acceso virtual que no es raíz configurada (por ejemplo, `<base href="/CoolApp/">`), la aplicación no puede encontrar sus recursos *cuando se ejecuta de forma local*.</span><span class="sxs-lookup"><span data-stu-id="54213-180">For an app with a non-root virtual path configured (for example, `<base href="/CoolApp/">`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="54213-181">Para solucionar este problema durante la fase de desarrollo y pruebas local, puede proporcionar un argumento de *ruta de acceso base* que coincida con el valor `href` de la etiqueta `<base>` en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="54213-181">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span>

<span data-ttu-id="54213-182">Para pasar el argumento de ruta de acceso base con la ruta de acceso raíz (`/`) al ejecutar la aplicación de forma local, ejecute el comando `dotnet run` desde el directorio de la aplicación con la opción `--pathbase`:</span><span class="sxs-lookup"><span data-stu-id="54213-182">To pass the path base argument with the root path (`/`) when running the app locally, execute the `dotnet run` command from the app's directory with the `--pathbase` option:</span></span>

```console
dotnet run --pathbase=/{Virtual Path (no trailing slash)}
```

<span data-ttu-id="54213-183">Para una aplicación con una ruta de acceso virtual base de `/CoolApp/` (`<base href="/CoolApp/">`), el comando es el siguiente:</span><span class="sxs-lookup"><span data-stu-id="54213-183">For an app with a virtual base path of `/CoolApp/` (`<base href="/CoolApp/">`), the command is:</span></span>

```console
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="54213-184">La aplicación responde de forma local en `http://localhost:port/CoolApp`.</span><span class="sxs-lookup"><span data-stu-id="54213-184">The app responds locally at `http://localhost:port/CoolApp`.</span></span>

<span data-ttu-id="54213-185">Para más información, vea la sección sobre el [valor de configuración de host de la ruta de acceso base](#path-base).</span><span class="sxs-lookup"><span data-stu-id="54213-185">For more information, see the section on the [path base host configuration value](#path-base).</span></span>

<span data-ttu-id="54213-186">Si una aplicación usa el [modelo de hospedaje del lado cliente](xref:blazor/hosting-models#client-side) (basado en la plantilla de proyecto de **Blazor**; la plantilla `blazor` al usar el comando [dotnet new](/dotnet/core/tools/dotnet-new)) y se hospeda como una subaplicación de IIS en una aplicación ASP.NET Core, es importante deshabilitar el controlador del módulo de ASP.NET Core heredado o asegurarse de que la subaplicación no hereda la sección `<handlers>` de la aplicación raíz (principal) en el archivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="54213-186">If an app uses the [client-side hosting model](xref:blazor/hosting-models#client-side) (based on the **Blazor** project template; the `blazor` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command) and is hosted as an IIS sub-application in an ASP.NET Core app, it's important to disable the inherited ASP.NET Core Module handler or make sure the root (parent) app's `<handlers>` section in the *web.config* file isn't inherited by the sub-app.</span></span>

<span data-ttu-id="54213-187">Para quitar el controlador del archivo *web.config* publicado de la aplicación, agregue una sección `<handlers>` al archivo:</span><span class="sxs-lookup"><span data-stu-id="54213-187">Remove the handler in the app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>

```xml
<handlers>
  <remove name="aspNetCore" />
</handlers>
```

<span data-ttu-id="54213-188">Como alternativa, deshabilite la herencia de la sección `<system.webServer>` de la aplicación raíz (principal) mediante un elemento `<location>` con `inheritInChildApplications` establecido en `false`:</span><span class="sxs-lookup"><span data-stu-id="54213-188">Alternatively, disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" ... />
      </handlers>
      <aspNetCore ... />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="54213-189">Además de configurarse la ruta de acceso base de la aplicación, se quita el controlador o se deshabilita la herencia, como se describe en esta sección.</span><span class="sxs-lookup"><span data-stu-id="54213-189">Removing the handler or disabling inheritance is performed in addition to configuring the app's base path as described in this section.</span></span> <span data-ttu-id="54213-190">Establezca la ruta de acceso base de la aplicación en el archivo *index.html* de la aplicación en el alias de IIS que ha usado al configurar la subaplicación en IIS.</span><span class="sxs-lookup"><span data-stu-id="54213-190">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

## <a name="hosted-deployment-with-aspnet-core"></a><span data-ttu-id="54213-191">Implementación hospedada con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="54213-191">Hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="54213-192">Una *implementación hospedada* proporciona la aplicación Blazor del lado cliente a los exploradores desde una [aplicación ASP.NET Core](xref:index) que se ejecuta en un servidor.</span><span class="sxs-lookup"><span data-stu-id="54213-192">A *hosted deployment* serves the client-side Blazor app to browsers from an [ASP.NET Core app](xref:index) that runs on a server.</span></span>

<span data-ttu-id="54213-193">La aplicación Blazor se incluye con la aplicación ASP.NET Core en la salida publicada para que ambas se implementen juntas.</span><span class="sxs-lookup"><span data-stu-id="54213-193">The Blazor app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="54213-194">Se requiere un servidor web que pueda hospedar una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="54213-194">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="54213-195">En el caso de una implementación hospedada, Visual Studio incluye la plantilla de proyecto de **Blazor (hospedada en ASP.NET Core)** (la plantilla `blazorhosted` al usar el comando [dotnet new](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="54213-195">For a hosted deployment, Visual Studio includes the **Blazor (ASP.NET Core hosted)** project template (`blazorhosted` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<span data-ttu-id="54213-196">Para obtener más información sobre la implementación y el hospedaje de aplicaciones de ASP.NET Core, consulte <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="54213-196">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="54213-197">Para obtener información sobre cómo implementar en Azure App Service, vea <xref:tutorials/publish-to-azure-webapp-using-vs>.</span><span class="sxs-lookup"><span data-stu-id="54213-197">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>

## <a name="standalone-deployment"></a><span data-ttu-id="54213-198">Implementación independiente</span><span class="sxs-lookup"><span data-stu-id="54213-198">Standalone deployment</span></span>

<span data-ttu-id="54213-199">Una *implementación independiente* proporciona la aplicación Blazor del lado cliente como un conjunto de archivos estáticos que los clientes solicitan directamente.</span><span class="sxs-lookup"><span data-stu-id="54213-199">A *standalone deployment* serves the client-side Blazor app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="54213-200">No se usa ningún servidor web para proporcionar la aplicación Blazor.</span><span class="sxs-lookup"><span data-stu-id="54213-200">A web server isn't used to serve the Blazor app.</span></span>

### <a name="iis"></a><span data-ttu-id="54213-201">IIS</span><span class="sxs-lookup"><span data-stu-id="54213-201">IIS</span></span>

<span data-ttu-id="54213-202">IIS es un servidor de archivos estáticos compatible con las aplicaciones de Blazor.</span><span class="sxs-lookup"><span data-stu-id="54213-202">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="54213-203">Para configurar IIS para hospedar Blazor, vea [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis) (Compilación de un sitio web estático en IIS).</span><span class="sxs-lookup"><span data-stu-id="54213-203">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="54213-204">Los recursos publicados se crean en la carpeta */bin/Release/{TARGET FRAMEWORK}/publish*.</span><span class="sxs-lookup"><span data-stu-id="54213-204">Published assets are created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span> <span data-ttu-id="54213-205">Hospede el contenido de la carpeta *publish* en el servidor web o el servicio de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="54213-205">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

#### <a name="webconfig"></a><span data-ttu-id="54213-206">web.config</span><span class="sxs-lookup"><span data-stu-id="54213-206">web.config</span></span>

<span data-ttu-id="54213-207">Cuando se publica un proyecto de Blazor, se crea un archivo *web.config* con la siguiente configuración de IIS:</span><span class="sxs-lookup"><span data-stu-id="54213-207">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="54213-208">Se establecen los tipos MIME de las siguientes extensiones de archivo:</span><span class="sxs-lookup"><span data-stu-id="54213-208">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="54213-209">*.dll* &ndash; `application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="54213-209">*.dll* &ndash; `application/octet-stream`</span></span>
  * <span data-ttu-id="54213-210">*.json* &ndash; `application/json`</span><span class="sxs-lookup"><span data-stu-id="54213-210">*.json* &ndash; `application/json`</span></span>
  * <span data-ttu-id="54213-211">*.wasm* &ndash; `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="54213-211">*.wasm* &ndash; `application/wasm`</span></span>
  * <span data-ttu-id="54213-212">*.woff* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="54213-212">*.woff* &ndash; `application/font-woff`</span></span>
  * <span data-ttu-id="54213-213">*.woff2* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="54213-213">*.woff2* &ndash; `application/font-woff`</span></span>
* <span data-ttu-id="54213-214">Se habilita la compresión HTTP de los siguientes tipos MIME:</span><span class="sxs-lookup"><span data-stu-id="54213-214">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="54213-215">Se establecen reglas del módulo URL Rewrite:</span><span class="sxs-lookup"><span data-stu-id="54213-215">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="54213-216">Proporcionar el subdirectorio donde residen los recursos estáticos de la aplicación ( *{ASSEMBLY NAME}/dist/{PATH REQUESTED}* ).</span><span class="sxs-lookup"><span data-stu-id="54213-216">Serve the sub-directory where the app's static assets reside (*{ASSEMBLY NAME}/dist/{PATH REQUESTED}*).</span></span>
  * <span data-ttu-id="54213-217">Crear el enrutamiento de reserva de SPA para que las solicitudes de recursos que no sean archivos se redirijan al documento predeterminado de la aplicación en su carpeta de recursos estáticos ( *{ASSEMBLY NAME}/dist/index.html*).</span><span class="sxs-lookup"><span data-stu-id="54213-217">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*{ASSEMBLY NAME}/dist/index.html*).</span></span>

#### <a name="install-the-url-rewrite-module"></a><span data-ttu-id="54213-218">Instalación del módulo URL Rewrite</span><span class="sxs-lookup"><span data-stu-id="54213-218">Install the URL Rewrite Module</span></span>

<span data-ttu-id="54213-219">El [módulo URL Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite) es necesario para reescribir las URL.</span><span class="sxs-lookup"><span data-stu-id="54213-219">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="54213-220">El módulo no se instala de forma predeterminada y no está disponible para instalar como una característica de servicio de rol del servidor web (IIS).</span><span class="sxs-lookup"><span data-stu-id="54213-220">The module isn't installed by default, and it isn't available for install as a Web Server (IIS) role service feature.</span></span> <span data-ttu-id="54213-221">El módulo se debe descargar desde el sitio web de IIS.</span><span class="sxs-lookup"><span data-stu-id="54213-221">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="54213-222">Use el instalador de plataforma web para instalar el módulo:</span><span class="sxs-lookup"><span data-stu-id="54213-222">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="54213-223">De forma local, vaya a la [página de descargas del módulo URL Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span><span class="sxs-lookup"><span data-stu-id="54213-223">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="54213-224">En el caso de la versión en inglés, seleccione **WebPI** para descargar el instalador de WebPI.</span><span class="sxs-lookup"><span data-stu-id="54213-224">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="54213-225">En el caso de otros idiomas, seleccione la arquitectura adecuada del servidor (x86/x64) para descargar el instalador.</span><span class="sxs-lookup"><span data-stu-id="54213-225">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="54213-226">Copie el instalador en el servidor.</span><span class="sxs-lookup"><span data-stu-id="54213-226">Copy the installer to the server.</span></span> <span data-ttu-id="54213-227">Ejecute el instalador.</span><span class="sxs-lookup"><span data-stu-id="54213-227">Run the installer.</span></span> <span data-ttu-id="54213-228">Haga clic en el botón **Instalar** y acepte los términos de licencia.</span><span class="sxs-lookup"><span data-stu-id="54213-228">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="54213-229">No es necesario reiniciar el servidor al finalizar la instalación.</span><span class="sxs-lookup"><span data-stu-id="54213-229">A server restart isn't required after the install completes.</span></span>

#### <a name="configure-the-website"></a><span data-ttu-id="54213-230">Configuración del sitio web</span><span class="sxs-lookup"><span data-stu-id="54213-230">Configure the website</span></span>

<span data-ttu-id="54213-231">Configure la **ruta de acceso física** del sitio web a la carpeta de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="54213-231">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="54213-232">La carpeta contiene:</span><span class="sxs-lookup"><span data-stu-id="54213-232">The folder contains:</span></span>

* <span data-ttu-id="54213-233">El archivo *web.config* que usa IIS para configurar el sitio web, incluidas las reglas de redireccionamiento y los tipos de contenido de archivos necesarios.</span><span class="sxs-lookup"><span data-stu-id="54213-233">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="54213-234">La carpeta de recursos estáticos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="54213-234">The app's static asset folder.</span></span>

#### <a name="troubleshooting"></a><span data-ttu-id="54213-235">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="54213-235">Troubleshooting</span></span>

<span data-ttu-id="54213-236">Si se recibe un error *500 Error interno del servidor* y el administrador de IIS produce errores al intentar acceder a la configuración del sitio web, confirme que el módulo URL Rewrite está instalado.</span><span class="sxs-lookup"><span data-stu-id="54213-236">If a *500 - Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="54213-237">Si no lo está, IIS no puede analizar el archivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="54213-237">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="54213-238">Esto impide que el Administrador de IIS cargue la configuración del sitio web y que el sitio web proporcione los archivos estáticos de Blazor.</span><span class="sxs-lookup"><span data-stu-id="54213-238">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="54213-239">Para obtener más información sobre cómo solucionar problemas de las implementaciones en IIS, vea <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="54213-239">For more information on troubleshooting deployments to IIS, see <xref:host-and-deploy/iis/troubleshoot>.</span></span>

### <a name="azure-storage"></a><span data-ttu-id="54213-240">Almacenamiento de Azure</span><span class="sxs-lookup"><span data-stu-id="54213-240">Azure Storage</span></span>

<span data-ttu-id="54213-241">El hospedaje de archivos estáticos de Azure Storage permite el hospedaje de aplicaciones Blazor sin servidor.</span><span class="sxs-lookup"><span data-stu-id="54213-241">Azure Storage static file hosting allows serverless Blazor app hosting.</span></span> <span data-ttu-id="54213-242">Se admiten nombres de dominio personalizados, Azure Content Delivery Network (CDN) y HTTPS.</span><span class="sxs-lookup"><span data-stu-id="54213-242">Custom domain names, the Azure Content Delivery Network (CDN), and HTTPS are supported.</span></span>

<span data-ttu-id="54213-243">Cuando el servicio de blob está habilitado para el hospedaje de sitios web estáticos en una cuenta de almacenamiento:</span><span class="sxs-lookup"><span data-stu-id="54213-243">When the blob service is enabled for static website hosting on a storage account:</span></span>

* <span data-ttu-id="54213-244">Establece el **nombre de documento de índice** en `index.html`.</span><span class="sxs-lookup"><span data-stu-id="54213-244">Set the **Index document name** to `index.html`.</span></span>
* <span data-ttu-id="54213-245">Establece la **ruta de acceso del documento de error** en `index.html`.</span><span class="sxs-lookup"><span data-stu-id="54213-245">Set the **Error document path** to `index.html`.</span></span> <span data-ttu-id="54213-246">Los componentes Razor y otros puntos de conexión que no son de archivo no residen en las rutas de acceso físicas del contenido estático almacenado por el servicio de blob.</span><span class="sxs-lookup"><span data-stu-id="54213-246">Razor Components and other non-file endpoints don't reside at physical paths in the static content stored by the blob service.</span></span> <span data-ttu-id="54213-247">Cuando se recibe una solicitud de uno de estos recursos que debe controlar el enrutador de Blazor, el error *404: no encontrado* generado por el servicio de blob enruta la solicitud a la **ruta de acceso del documento de error**.</span><span class="sxs-lookup"><span data-stu-id="54213-247">When a request for one of these resources is received that the Blazor router should handle, the *404 - Not Found* error generated by the blob service routes the request to the **Error document path**.</span></span> <span data-ttu-id="54213-248">Se devuelve el blob *index.html* y el enrutador de Blazor carga y procesa la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="54213-248">The *index.html* blob is returned, and the Blazor router loads and processes the path.</span></span>

<span data-ttu-id="54213-249">Para más información, consulte [Hospedaje de sitios web estáticos en Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span><span class="sxs-lookup"><span data-stu-id="54213-249">For more information, see [Static website hosting in Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span></span>

### <a name="nginx"></a><span data-ttu-id="54213-250">Nginx</span><span class="sxs-lookup"><span data-stu-id="54213-250">Nginx</span></span>

<span data-ttu-id="54213-251">El siguiente archivo *nginx.conf* se ha simplificado para mostrar cómo configurar Nginx para enviar el archivo *index.html* siempre que no pueda encontrar un archivo correspondiente en el disco.</span><span class="sxs-lookup"><span data-stu-id="54213-251">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *index.html* file whenever it can't find a corresponding file on disk.</span></span>

```
events { }
http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            try_files $uri $uri/ /index.html =404;
        }
    }
}
```

<span data-ttu-id="54213-252">Para obtener más información sobre la configuración del servidor web de producción de Nginx, consulte [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/) (Creación de archivos de configuración de NGINX y NGINX Plus).</span><span class="sxs-lookup"><span data-stu-id="54213-252">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

### <a name="nginx-in-docker"></a><span data-ttu-id="54213-253">Nginx en Docker</span><span class="sxs-lookup"><span data-stu-id="54213-253">Nginx in Docker</span></span>

<span data-ttu-id="54213-254">Para hospedar Blazor en Docker mediante Nginx, configure el Dockerfile para usar la imagen de Nginx basada en Alpine.</span><span class="sxs-lookup"><span data-stu-id="54213-254">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="54213-255">Actualice el Dockerfile para copiar el archivo *nginx.config* en el contenedor.</span><span class="sxs-lookup"><span data-stu-id="54213-255">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="54213-256">Agregue una línea al Dockerfile, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="54213-256">Add one line to the Dockerfile, as shown in the following example:</span></span>

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="github-pages"></a><span data-ttu-id="54213-257">GitHub Pages</span><span class="sxs-lookup"><span data-stu-id="54213-257">GitHub Pages</span></span>

<span data-ttu-id="54213-258">Para controlar las reescrituras de URL, agregue un archivo *404.html* con un script que controle el redireccionamiento de la solicitud a la página *index.html*.</span><span class="sxs-lookup"><span data-stu-id="54213-258">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="54213-259">Para consultar una implementación de ejemplo que ha proporcionado la comunidad, vea [Single Page Apps for GitHub Pages](http://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)) (Aplicaciones de página única para GitHub Pages [rafrex/spa-github-pages en GitHub]).</span><span class="sxs-lookup"><span data-stu-id="54213-259">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](http://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="54213-260">Se puede ver un ejemplo del enfoque de la comunidad en [blazor-demo/blazor-demo.github.io en GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([sitio activo](https://blazor-demo.github.io/)).</span><span class="sxs-lookup"><span data-stu-id="54213-260">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="54213-261">Al usar un sitio de proyecto en lugar de un sitio de la organización, agregue o actualice la etiqueta `<base>` en *index.html*.</span><span class="sxs-lookup"><span data-stu-id="54213-261">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="54213-262">Defina el valor del atributo `href` con el nombre del repositorio de GitHub con una barra diagonal final (por ejemplo, `my-repository/`).</span><span class="sxs-lookup"><span data-stu-id="54213-262">Set the `href` attribute value to the GitHub repository name with a trailing slash (for example, `my-repository/`.</span></span>
