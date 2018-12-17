---
title: Uso de SignalR de ASP.NET Core con TypeScript y Webpack
author: ssougnez
description: En este tutorial, se configura Webpack para agrupar y compilar una aplicación web de SignalR de ASP.NET Core cuyo cliente está escrito en TypeScript.
monikerRange: '>= aspnetcore-2.2'
ms.author: scaddie
ms.custom: mvc
ms.date: 11/30/2018
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: b2d59dfc449953cc2d747b507295c00ac0f652dd
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862257"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a><span data-ttu-id="4c23c-103">Uso de SignalR de ASP.NET Core con TypeScript y Webpack</span><span class="sxs-lookup"><span data-stu-id="4c23c-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="4c23c-104">Por [Sébastien Sougnez](https://twitter.com/ssougnez) y [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="4c23c-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="4c23c-105">[Webpack](https://webpack.js.org/) permite a los desarrolladores agrupar y compilar los recursos del lado cliente de una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="4c23c-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="4c23c-106">En este tutorial se describe el uso de Webpack en una aplicación web de SignalR de ASP.NET Core cuyo cliente está escrito en [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="4c23c-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="4c23c-107">En este tutorial aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="4c23c-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4c23c-108">Aplicación de scaffolding a una aplicación de inicio de SignalR de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4c23c-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="4c23c-109">Configuración del cliente TypeScript de SignalR</span><span class="sxs-lookup"><span data-stu-id="4c23c-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="4c23c-110">Configuración de una canalización de compilación mediante Webpack</span><span class="sxs-lookup"><span data-stu-id="4c23c-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="4c23c-111">Configuración del servidor de SignalR</span><span class="sxs-lookup"><span data-stu-id="4c23c-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="4c23c-112">Habilitar la comunicación entre cliente y servidor</span><span class="sxs-lookup"><span data-stu-id="4c23c-112">Enable communication between client and server</span></span>

<span data-ttu-id="4c23c-113">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4c23c-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-vs-vsc-2.2.md)]

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="4c23c-114">Creación de la aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4c23c-114">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4c23c-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4c23c-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4c23c-116">Configure Visual Studio para buscar npm en la variable de entorno *PATH*.</span><span class="sxs-lookup"><span data-stu-id="4c23c-116">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="4c23c-117">De forma predeterminada, Visual Studio usa la versión de npm que se encuentra en su directorio de instalación.</span><span class="sxs-lookup"><span data-stu-id="4c23c-117">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="4c23c-118">Siga estas instrucciones en Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="4c23c-118">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="4c23c-119">Vaya a **Herramientas** > **Opciones** > **Proyectos y soluciones** > **Administración de paquetes web** > **Herramientas web externas**.</span><span class="sxs-lookup"><span data-stu-id="4c23c-119">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="4c23c-120">Seleccione la entrada *$(PATH)* en la lista.</span><span class="sxs-lookup"><span data-stu-id="4c23c-120">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="4c23c-121">Haga clic en la flecha arriba para mover la entrada a la segunda posición de la lista.</span><span class="sxs-lookup"><span data-stu-id="4c23c-121">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Configuración de Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="4c23c-123">Se ha completado la configuración de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4c23c-123">Visual Studio configuration is completed.</span></span> <span data-ttu-id="4c23c-124">Es el momento de crear el proyecto.</span><span class="sxs-lookup"><span data-stu-id="4c23c-124">It's time to create the project.</span></span>

1. <span data-ttu-id="4c23c-125">Use la opción de menú **Archivo** > **Nuevo** > **Proyecto** y seleccione la plantilla **Aplicación web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="4c23c-125">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="4c23c-126">Asigne el nombre *SignalRWebPack* al proyecto y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="4c23c-126">Name the project *SignalRWebPack*, and select **OK**.</span></span>
1. <span data-ttu-id="4c23c-127">Seleccione *.NET Core* en la lista desplegable de plataforma de destino y *ASP.NET Core 2.2* en la lista desplegable del selector de plataforma.</span><span class="sxs-lookup"><span data-stu-id="4c23c-127">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.2* from the framework selector drop-down.</span></span> <span data-ttu-id="4c23c-128">Seleccione la plantilla **Vacía** y **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="4c23c-128">Select the **Empty** template, and select **OK**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4c23c-129">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4c23c-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="4c23c-130">Ejecute el comando siguiente en el **terminal integrado**:</span><span class="sxs-lookup"><span data-stu-id="4c23c-130">Run the following command in the **Integrated Terminal**:</span></span>

```console
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="4c23c-131">Se crea una aplicación web ASP.NET Core vacía, destinada a .NET Core, en el directorio *SignalRWebPack*.</span><span class="sxs-lookup"><span data-stu-id="4c23c-131">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="4c23c-132">Configuración de Webpack y TypeScript</span><span class="sxs-lookup"><span data-stu-id="4c23c-132">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="4c23c-133">Los pasos siguientes permiten configurar la conversión de TypeScript a JavaScript y la agrupación de los recursos del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="4c23c-133">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="4c23c-134">Ejecute el comando siguiente en la raíz del proyecto para crear un archivo *package.json*:</span><span class="sxs-lookup"><span data-stu-id="4c23c-134">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="4c23c-135">Agregue la propiedad resaltada al archivo *package.json*:</span><span class="sxs-lookup"><span data-stu-id="4c23c-135">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/snippets/package1.json?highlight=4)]

    <span data-ttu-id="4c23c-136">Si establece la propiedad `private` en `true`, evitará las advertencias de la instalación de paquetes en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="4c23c-136">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="4c23c-137">Instale los paquetes npm necesarios.</span><span class="sxs-lookup"><span data-stu-id="4c23c-137">Install the required npm packages.</span></span> <span data-ttu-id="4c23c-138">Ejecute el comando siguiente desde la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="4c23c-138">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@0.1.19 css-loader@0.28.11 html-webpack-plugin@3.2.0 mini-css-extract-plugin@0.4.0 ts-loader@4.4.1 typescript@2.9.2 webpack@4.12.0 webpack-cli@3.0.6
    ```

    <span data-ttu-id="4c23c-139">Algunos detalles del comando para tener en cuenta:</span><span class="sxs-lookup"><span data-stu-id="4c23c-139">Some command details to note:</span></span>

    * <span data-ttu-id="4c23c-140">En cada nombre de paquete, un número de versión sigue al signo `@`.</span><span class="sxs-lookup"><span data-stu-id="4c23c-140">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="4c23c-141">npm instala esas versiones de paquete específicas.</span><span class="sxs-lookup"><span data-stu-id="4c23c-141">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="4c23c-142">La opción `-E` deshabilita el comportamiento predeterminado de npm de escribir operadores de intervalo de [versionamiento semántico](https://semver.org/) en *package.json*.</span><span class="sxs-lookup"><span data-stu-id="4c23c-142">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="4c23c-143">Por ejemplo, se usa `"webpack": "4.12.0"` en lugar de `"webpack": "^4.12.0"`.</span><span class="sxs-lookup"><span data-stu-id="4c23c-143">For example, `"webpack": "4.12.0"` is used instead of `"webpack": "^4.12.0"`.</span></span> <span data-ttu-id="4c23c-144">Esta opción impide actualizaciones no deseadas a versiones más recientes del paquete.</span><span class="sxs-lookup"><span data-stu-id="4c23c-144">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="4c23c-145">Vea la documentación oficial de [npm-install](https://docs.npmjs.com/cli/install) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="4c23c-145">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="4c23c-146">Reemplace la propiedad `scripts` del archivo *package.json* por el fragmento de código siguiente:</span><span class="sxs-lookup"><span data-stu-id="4c23c-146">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="4c23c-147">Más detalles sobre los scripts:</span><span class="sxs-lookup"><span data-stu-id="4c23c-147">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="4c23c-148">`build`: agrupa los recursos del lado cliente en modo de desarrollo y supervisa los cambios del archivo.</span><span class="sxs-lookup"><span data-stu-id="4c23c-148">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="4c23c-149">El monitor de archivos hace que la agrupación se vuelva a generar cada vez que cambia un archivo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="4c23c-149">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="4c23c-150">La opción `mode` deshabilita las optimizaciones de producción, como la agitación del árbol y la minificación.</span><span class="sxs-lookup"><span data-stu-id="4c23c-150">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="4c23c-151">Use `build` únicamente durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="4c23c-151">Only use `build` in development.</span></span>
    * <span data-ttu-id="4c23c-152">`release`: agrupa los recursos del lado cliente en modo de producción.</span><span class="sxs-lookup"><span data-stu-id="4c23c-152">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="4c23c-153">`publish`: ejecuta el script `release` para agrupar los recursos del lado cliente en modo de producción.</span><span class="sxs-lookup"><span data-stu-id="4c23c-153">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="4c23c-154">Llama al comando [publish](/dotnet/core/tools/dotnet-publish) de la CLI de .NET Core para publicar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4c23c-154">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="4c23c-155">Cree un archivo denominado *webpack.config.js*, en la raíz del proyecto, con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="4c23c-155">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/webpack.config.js)]

    <span data-ttu-id="4c23c-156">El archivo anterior configura la compilación de Webpack.</span><span class="sxs-lookup"><span data-stu-id="4c23c-156">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="4c23c-157">Algunos detalles de configuración para tener en cuenta:</span><span class="sxs-lookup"><span data-stu-id="4c23c-157">Some configuration details to note:</span></span>

    * <span data-ttu-id="4c23c-158">La propiedad `output` invalida el valor predeterminado de *dist*.</span><span class="sxs-lookup"><span data-stu-id="4c23c-158">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="4c23c-159">En su lugar, la agrupación se genera en el directorio *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="4c23c-159">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="4c23c-160">La matriz `resolve.extensions` incluye *.js* para importar el código JavaScript cliente de SignalR.</span><span class="sxs-lookup"><span data-stu-id="4c23c-160">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="4c23c-161">Cree un directorio *src* en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="4c23c-161">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="4c23c-162">Su función es almacenar los activos del lado cliente del proyecto.</span><span class="sxs-lookup"><span data-stu-id="4c23c-162">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="4c23c-163">Cree *src/index.html* con el contenido siguiente.</span><span class="sxs-lookup"><span data-stu-id="4c23c-163">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/src/index.html)]

    <span data-ttu-id="4c23c-164">El código HTML anterior define el marcado reutilizable de la página principal.</span><span class="sxs-lookup"><span data-stu-id="4c23c-164">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="4c23c-165">Cree un directorio *src/css*.</span><span class="sxs-lookup"><span data-stu-id="4c23c-165">Create a new *src/css* directory.</span></span> <span data-ttu-id="4c23c-166">Su objetivo es almacenar los archivos *.css* del proyecto.</span><span class="sxs-lookup"><span data-stu-id="4c23c-166">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="4c23c-167">Cree *src/css/main.css* con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="4c23c-167">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/src/css/main.css)]

    <span data-ttu-id="4c23c-168">El archivo *main.css* anterior aplica estilo a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4c23c-168">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="4c23c-169">Cree *src/tsconfig.json* con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="4c23c-169">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/src/tsconfig.json)]

    <span data-ttu-id="4c23c-170">El código anterior configura el compilador de TypeScript para generar JavaScript compatible con [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5.</span><span class="sxs-lookup"><span data-stu-id="4c23c-170">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="4c23c-171">Cree *src/index.ts* con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="4c23c-171">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="4c23c-172">El elemento TypeScript anterior recupera las referencias a elementos DOM y adjunta dos controladores de eventos:</span><span class="sxs-lookup"><span data-stu-id="4c23c-172">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="4c23c-173">`keyup`: este evento se desencadena cuando el usuario escribe algo en el cuadro de texto identificado como `tbMessage`.</span><span class="sxs-lookup"><span data-stu-id="4c23c-173">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="4c23c-174">La función `send` se llama cuando el usuario presiona la tecla **Entrar**.</span><span class="sxs-lookup"><span data-stu-id="4c23c-174">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="4c23c-175">`click`: este evento se desencadena cuando el usuario clic en el botón **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="4c23c-175">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="4c23c-176">Se llama a la función `send`.</span><span class="sxs-lookup"><span data-stu-id="4c23c-176">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="4c23c-177">Configuración de la aplicación ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4c23c-177">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="4c23c-178">El código proporcionado en el método `Startup.Configure` muestra *Hello World!*.</span><span class="sxs-lookup"><span data-stu-id="4c23c-178">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="4c23c-179">Reemplace la llamada al método `app.Run` por las llamadas a [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) y [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="4c23c-179">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="4c23c-180">El código anterior permite que el servidor busque y proporcione el archivo *index.html*, con independencia de que el usuario escriba su dirección URL completa o la dirección URL raíz de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="4c23c-180">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="4c23c-181">Llame a [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) en el método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="4c23c-181">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="4c23c-182">Esto permite agregar los servicios SignalR al proyecto.</span><span class="sxs-lookup"><span data-stu-id="4c23c-182">It adds the SignalR services to your project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="4c23c-183">Asigne una ruta */hub* al concentrador `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="4c23c-183">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="4c23c-184">Agregue las líneas siguientes al final del método `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="4c23c-184">Add the following lines at the end of the `Startup.Configure` method:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseSignalR)]

1. <span data-ttu-id="4c23c-185">Cree un directorio denominado *Hubs* en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="4c23c-185">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="4c23c-186">Su objetivo es almacenar el concentrador de SignalR, que se crea en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="4c23c-186">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="4c23c-187">Cree el concentrador *Hubs/ChatHub.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="4c23c-187">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="4c23c-188">Agregue el código siguiente en la parte superior del archivo *Startup.cs* para resolver la referencia a `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="4c23c-188">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="4c23c-189">Habilitar la comunicación entre cliente y servidor</span><span class="sxs-lookup"><span data-stu-id="4c23c-189">Enable client and server communication</span></span>

<span data-ttu-id="4c23c-190">Actualmente, en la aplicación se muestra un formulario simple para enviar mensajes.</span><span class="sxs-lookup"><span data-stu-id="4c23c-190">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="4c23c-191">Al intentar hacer algo no sucede nada.</span><span class="sxs-lookup"><span data-stu-id="4c23c-191">Nothing happens when you try to do so.</span></span> <span data-ttu-id="4c23c-192">El servidor está escuchando en una ruta específica, pero no hace nada con los mensajes enviados.</span><span class="sxs-lookup"><span data-stu-id="4c23c-192">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="4c23c-193">Ejecute el comando siguiente en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="4c23c-193">Execute the following command at the project root:</span></span>

    ```console
    npm install @aspnet/signalr
    ```

    <span data-ttu-id="4c23c-194">El comando anterior instala el [cliente TypeScript de SignalR](https://www.npmjs.com/package/@aspnet/signalr), que permite al cliente enviar mensajes al servidor.</span><span class="sxs-lookup"><span data-stu-id="4c23c-194">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@aspnet/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="4c23c-195">Agregue el código resaltado al archivo *src/index.ts*:</span><span class="sxs-lookup"><span data-stu-id="4c23c-195">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="4c23c-196">El código anterior admite la recepción de mensajes desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="4c23c-196">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="4c23c-197">La clase `HubConnectionBuilder` crea un generador para configurar la conexión al servidor.</span><span class="sxs-lookup"><span data-stu-id="4c23c-197">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="4c23c-198">La función `withUrl` configura la dirección URL del concentrador.</span><span class="sxs-lookup"><span data-stu-id="4c23c-198">The `withUrl` function configures the hub URL.</span></span>

    <span data-ttu-id="4c23c-199">SignalR permite el intercambio de mensajes entre un cliente y un servidor.</span><span class="sxs-lookup"><span data-stu-id="4c23c-199">SignalR enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="4c23c-200">Cada mensaje tiene un nombre específico.</span><span class="sxs-lookup"><span data-stu-id="4c23c-200">Each message has a specific name.</span></span> <span data-ttu-id="4c23c-201">Por ejemplo, puede haber mensajes con el nombre `messageReceived` que ejecuten la lógica responsable de mostrar el mensaje nuevo en la zona de mensajes.</span><span class="sxs-lookup"><span data-stu-id="4c23c-201">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="4c23c-202">La escucha a un mensaje concreto se puede realizar mediante la función `on`.</span><span class="sxs-lookup"><span data-stu-id="4c23c-202">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="4c23c-203">Puede escuchar a cualquier número de nombres de mensaje.</span><span class="sxs-lookup"><span data-stu-id="4c23c-203">You can listen to any number of message names.</span></span> <span data-ttu-id="4c23c-204">También se pueden pasar parámetros al mensaje, como el nombre del autor y el contenido del mensaje recibido.</span><span class="sxs-lookup"><span data-stu-id="4c23c-204">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="4c23c-205">Una vez que el cliente recibe un mensaje, se crea un elemento `div` con el nombre del autor y el contenido del mensaje en su atributo `innerHTML`.</span><span class="sxs-lookup"><span data-stu-id="4c23c-205">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="4c23c-206">Se agrega al elemento `div` principal que muestra los mensajes.</span><span class="sxs-lookup"><span data-stu-id="4c23c-206">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="4c23c-207">Ahora que el cliente puede recibir mensajes, debe configurarlo para poder enviarlos.</span><span class="sxs-lookup"><span data-stu-id="4c23c-207">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="4c23c-208">Agregue el código resaltado al archivo *src/index.ts*:</span><span class="sxs-lookup"><span data-stu-id="4c23c-208">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/src/index.ts?highlight=34-35)]

    <span data-ttu-id="4c23c-209">El envío de mensajes a través de la conexión de WebSockets requiere llamar al método `send`.</span><span class="sxs-lookup"><span data-stu-id="4c23c-209">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="4c23c-210">El primer parámetro del método es el nombre del mensaje.</span><span class="sxs-lookup"><span data-stu-id="4c23c-210">The method's first parameter is the message name.</span></span> <span data-ttu-id="4c23c-211">Los datos del mensaje se encuentran en los otros parámetros.</span><span class="sxs-lookup"><span data-stu-id="4c23c-211">The message data inhabits the other parameters.</span></span> <span data-ttu-id="4c23c-212">En este ejemplo, se envía al servidor un mensaje identificado como `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="4c23c-212">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="4c23c-213">El mensaje está formado por el nombre de usuario y la entrada del usuario desde un cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="4c23c-213">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="4c23c-214">Si el envío funciona, se borra el valor del cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="4c23c-214">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="4c23c-215">Agregue el método resaltado a la clase `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="4c23c-215">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="4c23c-216">El código anterior difunde los mensajes recibidos a todos los usuarios conectados, una vez que el servidor los recibe.</span><span class="sxs-lookup"><span data-stu-id="4c23c-216">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="4c23c-217">No es necesario tener un método `on` genérico para recibir todos los mensajes.</span><span class="sxs-lookup"><span data-stu-id="4c23c-217">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="4c23c-218">Basta con un método que tenga el nombre del mensaje.</span><span class="sxs-lookup"><span data-stu-id="4c23c-218">A method named after the message name suffices.</span></span>

    <span data-ttu-id="4c23c-219">En este ejemplo, el cliente de TypeScript envía un mensaje que se identifica como `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="4c23c-219">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="4c23c-220">El método `NewMessage` de C# espera los datos enviados por el cliente.</span><span class="sxs-lookup"><span data-stu-id="4c23c-220">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="4c23c-221">Se realiza una llamada al método [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) de [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="4c23c-221">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="4c23c-222">Los mensajes recibidos se envían a todos los clientes conectados al concentrador.</span><span class="sxs-lookup"><span data-stu-id="4c23c-222">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="4c23c-223">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="4c23c-223">Test the app</span></span>

<span data-ttu-id="4c23c-224">Confirme que la aplicación funciona con los pasos siguientes.</span><span class="sxs-lookup"><span data-stu-id="4c23c-224">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4c23c-225">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4c23c-225">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="4c23c-226">Ejecute Webpack en modo *release*.</span><span class="sxs-lookup"><span data-stu-id="4c23c-226">Run Webpack in *release* mode.</span></span> <span data-ttu-id="4c23c-227">Desde la ventana **Consola del administrador de paquetes**, ejecute el comando siguiente en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="4c23c-227">Using the **Package Manager Console** window, execute the following command in the project root.</span></span> <span data-ttu-id="4c23c-228">Si no está en la raíz del proyecto, escriba `cd SignalRWebPack` antes de introducir el comando.</span><span class="sxs-lookup"><span data-stu-id="4c23c-228">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="4c23c-229">Seleccione **Depurar** > **Iniciar sin depurar** para iniciar la aplicación en un explorador sin adjuntar el depurador.</span><span class="sxs-lookup"><span data-stu-id="4c23c-229">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="4c23c-230">El archivo *wwwroot/index.html* se entrega en `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="4c23c-230">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="4c23c-231">Abra otra instancia del explorador (sirve cualquiera).</span><span class="sxs-lookup"><span data-stu-id="4c23c-231">Open another browser instance (any browser).</span></span> <span data-ttu-id="4c23c-232">Pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="4c23c-232">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="4c23c-233">Elija un explorador, escriba algo en el cuadro de texto **Mensaje** y haga clic en el botón **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="4c23c-233">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="4c23c-234">El nombre de usuario único y el mensaje se muestran en las dos páginas al instante.</span><span class="sxs-lookup"><span data-stu-id="4c23c-234">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4c23c-235">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4c23c-235">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="4c23c-236">Ejecute Webpack en modo *release* mediante la ejecución del comando siguiente en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="4c23c-236">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="4c23c-237">Compile y ejecute la aplicación mediante la ejecución del comando siguiente en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="4c23c-237">Build and run the app by executing the following command in the project root:</span></span>

    ```console
    dotnet run
    ```

    <span data-ttu-id="4c23c-238">El servidor web inicia la aplicación y hace que esté disponible en el host local.</span><span class="sxs-lookup"><span data-stu-id="4c23c-238">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="4c23c-239">Abra un explorador en `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="4c23c-239">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="4c23c-240">Se entrega el archivo *wwwroot/index.html*.</span><span class="sxs-lookup"><span data-stu-id="4c23c-240">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="4c23c-241">Copie la dirección URL de la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="4c23c-241">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="4c23c-242">Abra otra instancia del explorador (sirve cualquiera).</span><span class="sxs-lookup"><span data-stu-id="4c23c-242">Open another browser instance (any browser).</span></span> <span data-ttu-id="4c23c-243">Pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="4c23c-243">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="4c23c-244">Elija un explorador, escriba algo en el cuadro de texto **Mensaje** y haga clic en el botón **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="4c23c-244">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="4c23c-245">El nombre de usuario único y el mensaje se muestran en las dos páginas al instante.</span><span class="sxs-lookup"><span data-stu-id="4c23c-245">The unique user name and message are displayed on both pages instantly.</span></span>

---

![mensaje mostrado en las dos ventanas del explorador](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

## <a name="additional-resources"></a><span data-ttu-id="4c23c-247">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="4c23c-247">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
