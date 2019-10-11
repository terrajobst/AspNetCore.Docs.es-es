---
title: Uso de SignalR de ASP.NET Core con TypeScript y Webpack
author: ssougnez
description: En este tutorial, se configura Webpack para agrupar y compilar una aplicación web de SignalR de ASP.NET Core cuyo cliente está escrito en TypeScript.
ms.author: bradyg
ms.custom: mvc
ms.date: 10/04/2019
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: 630e8cb5efe9c313479960626d3d864c4923cbd1
ms.sourcegitcommit: 3ffcd8cbff8b49128733842f72270bc58279de70
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2019
ms.locfileid: "71955934"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a><span data-ttu-id="392ae-103">Uso de SignalR de ASP.NET Core con TypeScript y Webpack</span><span class="sxs-lookup"><span data-stu-id="392ae-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="392ae-104">Por [Sébastien Sougnez](https://twitter.com/ssougnez) y [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="392ae-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="392ae-105">[Webpack](https://webpack.js.org/) permite a los desarrolladores agrupar y compilar los recursos del lado cliente de una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="392ae-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="392ae-106">En este tutorial se describe el uso de Webpack en una aplicación web de SignalR de ASP.NET Core cuyo cliente está escrito en [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="392ae-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="392ae-107">En este tutorial aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="392ae-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="392ae-108">Aplicación de scaffolding a una aplicación de inicio de SignalR de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="392ae-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="392ae-109">Configuración del cliente TypeScript de SignalR</span><span class="sxs-lookup"><span data-stu-id="392ae-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="392ae-110">Configuración de una canalización de compilación mediante Webpack</span><span class="sxs-lookup"><span data-stu-id="392ae-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="392ae-111">Configuración del servidor de SignalR</span><span class="sxs-lookup"><span data-stu-id="392ae-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="392ae-112">Habilitar la comunicación entre cliente y servidor</span><span class="sxs-lookup"><span data-stu-id="392ae-112">Enable communication between client and server</span></span>

<span data-ttu-id="392ae-113">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="392ae-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="392ae-114">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="392ae-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="392ae-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="392ae-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="392ae-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) con la carga de trabajo **ASP.NET y desarrollo web**</span><span class="sxs-lookup"><span data-stu-id="392ae-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="392ae-117">.NET Core SDK 3.0 o posterior</span><span class="sxs-lookup"><span data-stu-id="392ae-117">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="392ae-118">[Node.js](https://nodejs.org/) con [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="392ae-118">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="392ae-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="392ae-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="392ae-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="392ae-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="392ae-121">.NET Core SDK 3.0 o posterior</span><span class="sxs-lookup"><span data-stu-id="392ae-121">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="392ae-122">C# para Visual Studio Code versión 1.17.1 o posterior</span><span class="sxs-lookup"><span data-stu-id="392ae-122">C# for Visual Studio Code version 1.17.1 or later</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="392ae-123">[Node.js](https://nodejs.org/) con [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="392ae-123">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="392ae-124">Creación de la aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="392ae-124">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="392ae-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="392ae-125">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="392ae-126">Configure Visual Studio para buscar npm en la variable de entorno *PATH*.</span><span class="sxs-lookup"><span data-stu-id="392ae-126">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="392ae-127">De forma predeterminada, Visual Studio usa la versión de npm que se encuentra en su directorio de instalación.</span><span class="sxs-lookup"><span data-stu-id="392ae-127">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="392ae-128">Siga estas instrucciones en Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="392ae-128">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="392ae-129">Vaya a **Herramientas** > **Opciones** > **Proyectos y soluciones** > **Administración de paquetes web** > **Herramientas web externas**.</span><span class="sxs-lookup"><span data-stu-id="392ae-129">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="392ae-130">Seleccione la entrada *$(PATH)* en la lista.</span><span class="sxs-lookup"><span data-stu-id="392ae-130">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="392ae-131">Haga clic en la flecha arriba para mover la entrada a la segunda posición de la lista.</span><span class="sxs-lookup"><span data-stu-id="392ae-131">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Configuración de Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="392ae-133">Se ha completado la configuración de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="392ae-133">Visual Studio configuration is completed.</span></span> <span data-ttu-id="392ae-134">Es el momento de crear el proyecto.</span><span class="sxs-lookup"><span data-stu-id="392ae-134">It's time to create the project.</span></span>

1. <span data-ttu-id="392ae-135">Use la opción de menú **Archivo** > **Nuevo** > **Proyecto** y seleccione la plantilla **Aplicación web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="392ae-135">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="392ae-136">Asigne el nombre *SignalRWebPack* al proyecto y seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="392ae-136">Name the project *SignalRWebPack*, and select **Create**.</span></span>
1. <span data-ttu-id="392ae-137">Seleccione *.NET Core* en la lista desplegable de plataforma de destino y *ASP.NET Core 3.0* en la lista desplegable del selector de plataforma.</span><span class="sxs-lookup"><span data-stu-id="392ae-137">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 3.0* from the framework selector drop-down.</span></span> <span data-ttu-id="392ae-138">Seleccione la plantilla **Vacía** y **Crear**.</span><span class="sxs-lookup"><span data-stu-id="392ae-138">Select the **Empty** template, and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="392ae-139">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="392ae-139">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="392ae-140">Ejecute el comando siguiente en el **terminal integrado**:</span><span class="sxs-lookup"><span data-stu-id="392ae-140">Run the following command in the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="392ae-141">Se crea una aplicación web ASP.NET Core vacía, destinada a .NET Core, en el directorio *SignalRWebPack*.</span><span class="sxs-lookup"><span data-stu-id="392ae-141">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="392ae-142">Configuración de Webpack y TypeScript</span><span class="sxs-lookup"><span data-stu-id="392ae-142">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="392ae-143">Los pasos siguientes permiten configurar la conversión de TypeScript a JavaScript y la agrupación de los recursos del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="392ae-143">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="392ae-144">Ejecute el comando siguiente en la raíz del proyecto para crear un archivo *package.json*:</span><span class="sxs-lookup"><span data-stu-id="392ae-144">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="392ae-145">Agregue la propiedad resaltada al archivo *package.json*:</span><span class="sxs-lookup"><span data-stu-id="392ae-145">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/3.x/snippets/package1.json?highlight=4)]

    <span data-ttu-id="392ae-146">Si establece la propiedad `private` en `true`, evitará las advertencias de la instalación de paquetes en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="392ae-146">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="392ae-147">Instale los paquetes npm necesarios.</span><span class="sxs-lookup"><span data-stu-id="392ae-147">Install the required npm packages.</span></span> <span data-ttu-id="392ae-148">Ejecute el comando siguiente desde la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="392ae-148">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    <span data-ttu-id="392ae-149">Algunos detalles del comando para tener en cuenta:</span><span class="sxs-lookup"><span data-stu-id="392ae-149">Some command details to note:</span></span>

    * <span data-ttu-id="392ae-150">En cada nombre de paquete, un número de versión sigue al signo `@`.</span><span class="sxs-lookup"><span data-stu-id="392ae-150">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="392ae-151">npm instala esas versiones de paquete específicas.</span><span class="sxs-lookup"><span data-stu-id="392ae-151">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="392ae-152">La opción `-E` deshabilita el comportamiento predeterminado de npm de escribir operadores de intervalo de [versionamiento semántico](https://semver.org/) en *package.json*.</span><span class="sxs-lookup"><span data-stu-id="392ae-152">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="392ae-153">Por ejemplo, se usa `"webpack": "4.29.3"` en lugar de `"webpack": "^4.29.3"`.</span><span class="sxs-lookup"><span data-stu-id="392ae-153">For example, `"webpack": "4.29.3"` is used instead of `"webpack": "^4.29.3"`.</span></span> <span data-ttu-id="392ae-154">Esta opción impide actualizaciones no deseadas a versiones más recientes del paquete.</span><span class="sxs-lookup"><span data-stu-id="392ae-154">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="392ae-155">Vea la documentación oficial de [npm-install](https://docs.npmjs.com/cli/install) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="392ae-155">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="392ae-156">Reemplace la propiedad `scripts` del archivo *package.json* por el fragmento de código siguiente:</span><span class="sxs-lookup"><span data-stu-id="392ae-156">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="392ae-157">Más detalles sobre los scripts:</span><span class="sxs-lookup"><span data-stu-id="392ae-157">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="392ae-158">`build`: agrupa los recursos del lado cliente en modo de desarrollo y supervisa los cambios del archivo.</span><span class="sxs-lookup"><span data-stu-id="392ae-158">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="392ae-159">El monitor de archivos hace que la agrupación se vuelva a generar cada vez que cambia un archivo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="392ae-159">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="392ae-160">La opción `mode` deshabilita las optimizaciones de producción, como la agitación del árbol y la minificación.</span><span class="sxs-lookup"><span data-stu-id="392ae-160">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="392ae-161">Use `build` únicamente durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="392ae-161">Only use `build` in development.</span></span>
    * <span data-ttu-id="392ae-162">`release`: agrupa los recursos del lado cliente en modo de producción.</span><span class="sxs-lookup"><span data-stu-id="392ae-162">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="392ae-163">`publish`: ejecuta el script `release` para agrupar los recursos del lado cliente en modo de producción.</span><span class="sxs-lookup"><span data-stu-id="392ae-163">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="392ae-164">Llama al comando [publish](/dotnet/core/tools/dotnet-publish) de la CLI de .NET Core para publicar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="392ae-164">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="392ae-165">Cree un archivo denominado *webpack.config.js*, en la raíz del proyecto, con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="392ae-165">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/3.x/webpack.config.js)]

    <span data-ttu-id="392ae-166">El archivo anterior configura la compilación de Webpack.</span><span class="sxs-lookup"><span data-stu-id="392ae-166">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="392ae-167">Algunos detalles de configuración para tener en cuenta:</span><span class="sxs-lookup"><span data-stu-id="392ae-167">Some configuration details to note:</span></span>

    * <span data-ttu-id="392ae-168">La propiedad `output` invalida el valor predeterminado de *dist*.</span><span class="sxs-lookup"><span data-stu-id="392ae-168">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="392ae-169">En su lugar, la agrupación se genera en el directorio *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="392ae-169">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="392ae-170">La matriz `resolve.extensions` incluye *.js* para importar el código JavaScript cliente de SignalR.</span><span class="sxs-lookup"><span data-stu-id="392ae-170">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="392ae-171">Cree un directorio *src* en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="392ae-171">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="392ae-172">Su función es almacenar los activos del lado cliente del proyecto.</span><span class="sxs-lookup"><span data-stu-id="392ae-172">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="392ae-173">Cree *src/index.html* con el contenido siguiente.</span><span class="sxs-lookup"><span data-stu-id="392ae-173">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/3.x/src/index.html)]

    <span data-ttu-id="392ae-174">El código HTML anterior define el marcado reutilizable de la página principal.</span><span class="sxs-lookup"><span data-stu-id="392ae-174">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="392ae-175">Cree un directorio *src/css*.</span><span class="sxs-lookup"><span data-stu-id="392ae-175">Create a new *src/css* directory.</span></span> <span data-ttu-id="392ae-176">Su objetivo es almacenar los archivos *.css* del proyecto.</span><span class="sxs-lookup"><span data-stu-id="392ae-176">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="392ae-177">Cree *src/css/main.css* con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="392ae-177">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/3.x/src/css/main.css)]

    <span data-ttu-id="392ae-178">El archivo *main.css* anterior aplica estilo a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="392ae-178">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="392ae-179">Cree *src/tsconfig.json* con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="392ae-179">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/3.x/src/tsconfig.json)]

    <span data-ttu-id="392ae-180">El código anterior configura el compilador de TypeScript para generar JavaScript compatible con [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5.</span><span class="sxs-lookup"><span data-stu-id="392ae-180">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="392ae-181">Cree *src/index.ts* con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="392ae-181">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="392ae-182">El elemento TypeScript anterior recupera las referencias a elementos DOM y adjunta dos controladores de eventos:</span><span class="sxs-lookup"><span data-stu-id="392ae-182">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="392ae-183">`keyup`: este evento se desencadena cuando el usuario escribe algo en el cuadro de texto identificado como `tbMessage`.</span><span class="sxs-lookup"><span data-stu-id="392ae-183">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="392ae-184">La función `send` se llama cuando el usuario presiona la tecla **Entrar**.</span><span class="sxs-lookup"><span data-stu-id="392ae-184">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="392ae-185">`click`: este evento se desencadena cuando el usuario hace clic en el botón **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="392ae-185">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="392ae-186">Se llama a la función `send`.</span><span class="sxs-lookup"><span data-stu-id="392ae-186">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="392ae-187">Configuración de la aplicación ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="392ae-187">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="392ae-188">En el método `Startup.Configure`, agregue llamadas a [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) y [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="392ae-188">In the `Startup.Configure` method, add calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseStaticDefaultFiles&highlight=2-3)]

   <span data-ttu-id="392ae-189">El código anterior permite que el servidor busque y proporcione el archivo *index.html*, con independencia de que el usuario escriba su dirección URL completa o la dirección URL raíz de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="392ae-189">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="392ae-190">Al final del método `Startup.Configure`, asigne una ruta */hub* al centro `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="392ae-190">At the end of the `Startup.Configure` method, map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="392ae-191">Reemplace el código que muestra *Hola mundo*</span><span class="sxs-lookup"><span data-stu-id="392ae-191">Replace the code that displays *Hello World!*</span></span> <span data-ttu-id="392ae-192">por la línea siguiente:</span><span class="sxs-lookup"><span data-stu-id="392ae-192">with the following line:</span></span> 

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseSignalR&highlight=3)]

1. <span data-ttu-id="392ae-193">En el método `Startup.ConfigureServices`, llame a [AddSignalR ](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_).</span><span class="sxs-lookup"><span data-stu-id="392ae-193">In the `Startup.ConfigureServices` method, call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_).</span></span> <span data-ttu-id="392ae-194">Esto permite agregar los servicios SignalR al proyecto.</span><span class="sxs-lookup"><span data-stu-id="392ae-194">It adds the SignalR services to your project.</span></span>

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="392ae-195">Cree un directorio denominado *Hubs* en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="392ae-195">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="392ae-196">Su objetivo es almacenar el concentrador de SignalR, que se crea en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="392ae-196">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="392ae-197">Cree el concentrador *Hubs/ChatHub.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="392ae-197">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="392ae-198">Agregue el código siguiente en la parte superior del archivo *Startup.cs* para resolver la referencia a `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="392ae-198">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="392ae-199">Habilitar la comunicación entre cliente y servidor</span><span class="sxs-lookup"><span data-stu-id="392ae-199">Enable client and server communication</span></span>

<span data-ttu-id="392ae-200">Actualmente, en la aplicación se muestra un formulario simple para enviar mensajes.</span><span class="sxs-lookup"><span data-stu-id="392ae-200">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="392ae-201">Al intentar hacer algo no sucede nada.</span><span class="sxs-lookup"><span data-stu-id="392ae-201">Nothing happens when you try to do so.</span></span> <span data-ttu-id="392ae-202">El servidor está escuchando en una ruta específica, pero no hace nada con los mensajes enviados.</span><span class="sxs-lookup"><span data-stu-id="392ae-202">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="392ae-203">Ejecute el comando siguiente en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="392ae-203">Execute the following command at the project root:</span></span>

    ```console
    npm install @aspnet/signalr
    ```

    <span data-ttu-id="392ae-204">El comando anterior instala el [cliente TypeScript de SignalR](https://www.npmjs.com/package/@aspnet/signalr), que permite al cliente enviar mensajes al servidor.</span><span class="sxs-lookup"><span data-stu-id="392ae-204">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@aspnet/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="392ae-205">Agregue el código resaltado al archivo *src/index.ts*:</span><span class="sxs-lookup"><span data-stu-id="392ae-205">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="392ae-206">El código anterior admite la recepción de mensajes desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="392ae-206">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="392ae-207">La clase `HubConnectionBuilder` crea un generador para configurar la conexión al servidor.</span><span class="sxs-lookup"><span data-stu-id="392ae-207">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="392ae-208">La función `withUrl` configura la dirección URL del concentrador.</span><span class="sxs-lookup"><span data-stu-id="392ae-208">The `withUrl` function configures the hub URL.</span></span>

    <span data-ttu-id="392ae-209">SignalR permite el intercambio de mensajes entre un cliente y un servidor.</span><span class="sxs-lookup"><span data-stu-id="392ae-209">SignalR enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="392ae-210">Cada mensaje tiene un nombre específico.</span><span class="sxs-lookup"><span data-stu-id="392ae-210">Each message has a specific name.</span></span> <span data-ttu-id="392ae-211">Por ejemplo, puede haber mensajes con el nombre `messageReceived` que ejecuten la lógica responsable de mostrar el mensaje nuevo en la zona de mensajes.</span><span class="sxs-lookup"><span data-stu-id="392ae-211">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="392ae-212">La escucha a un mensaje concreto se puede realizar mediante la función `on`.</span><span class="sxs-lookup"><span data-stu-id="392ae-212">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="392ae-213">Puede escuchar a cualquier número de nombres de mensaje.</span><span class="sxs-lookup"><span data-stu-id="392ae-213">You can listen to any number of message names.</span></span> <span data-ttu-id="392ae-214">También se pueden pasar parámetros al mensaje, como el nombre del autor y el contenido del mensaje recibido.</span><span class="sxs-lookup"><span data-stu-id="392ae-214">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="392ae-215">Una vez que el cliente recibe un mensaje, se crea un elemento `div` con el nombre del autor y el contenido del mensaje en su atributo `innerHTML`.</span><span class="sxs-lookup"><span data-stu-id="392ae-215">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="392ae-216">Se agrega al elemento `div` principal que muestra los mensajes.</span><span class="sxs-lookup"><span data-stu-id="392ae-216">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="392ae-217">Ahora que el cliente puede recibir mensajes, debe configurarlo para poder enviarlos.</span><span class="sxs-lookup"><span data-stu-id="392ae-217">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="392ae-218">Agregue el código resaltado al archivo *src/index.ts*:</span><span class="sxs-lookup"><span data-stu-id="392ae-218">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/src/index.ts?highlight=34-35)]

    <span data-ttu-id="392ae-219">El envío de mensajes a través de la conexión de WebSockets requiere llamar al método `send`.</span><span class="sxs-lookup"><span data-stu-id="392ae-219">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="392ae-220">El primer parámetro del método es el nombre del mensaje.</span><span class="sxs-lookup"><span data-stu-id="392ae-220">The method's first parameter is the message name.</span></span> <span data-ttu-id="392ae-221">Los datos del mensaje se encuentran en los otros parámetros.</span><span class="sxs-lookup"><span data-stu-id="392ae-221">The message data inhabits the other parameters.</span></span> <span data-ttu-id="392ae-222">En este ejemplo, se envía al servidor un mensaje identificado como `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="392ae-222">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="392ae-223">El mensaje está formado por el nombre de usuario y la entrada del usuario desde un cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="392ae-223">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="392ae-224">Si el envío funciona, se borra el valor del cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="392ae-224">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="392ae-225">Agregue el método resaltado a la clase `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="392ae-225">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="392ae-226">El código anterior difunde los mensajes recibidos a todos los usuarios conectados, una vez que el servidor los recibe.</span><span class="sxs-lookup"><span data-stu-id="392ae-226">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="392ae-227">No es necesario tener un método `on` genérico para recibir todos los mensajes.</span><span class="sxs-lookup"><span data-stu-id="392ae-227">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="392ae-228">Basta con un método que tenga el nombre del mensaje.</span><span class="sxs-lookup"><span data-stu-id="392ae-228">A method named after the message name suffices.</span></span>

    <span data-ttu-id="392ae-229">En este ejemplo, el cliente de TypeScript envía un mensaje que se identifica como `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="392ae-229">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="392ae-230">El método `NewMessage` de C# espera los datos enviados por el cliente.</span><span class="sxs-lookup"><span data-stu-id="392ae-230">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="392ae-231">Se realiza una llamada al método [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) de [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="392ae-231">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="392ae-232">Los mensajes recibidos se envían a todos los clientes conectados al concentrador.</span><span class="sxs-lookup"><span data-stu-id="392ae-232">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="392ae-233">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="392ae-233">Test the app</span></span>

<span data-ttu-id="392ae-234">Confirme que la aplicación funciona con los pasos siguientes.</span><span class="sxs-lookup"><span data-stu-id="392ae-234">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="392ae-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="392ae-235">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="392ae-236">Ejecute Webpack en modo *release*.</span><span class="sxs-lookup"><span data-stu-id="392ae-236">Run Webpack in *release* mode.</span></span> <span data-ttu-id="392ae-237">Desde la ventana **Consola del administrador de paquetes**, ejecute el comando siguiente en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="392ae-237">Using the **Package Manager Console** window, execute the following command in the project root.</span></span> <span data-ttu-id="392ae-238">Si no está en la raíz del proyecto, escriba `cd SignalRWebPack` antes de introducir el comando.</span><span class="sxs-lookup"><span data-stu-id="392ae-238">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="392ae-239">Seleccione **Depurar** > **Iniciar sin depurar** para iniciar la aplicación en un explorador sin adjuntar el depurador.</span><span class="sxs-lookup"><span data-stu-id="392ae-239">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="392ae-240">El archivo *wwwroot/index.html* se entrega en `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="392ae-240">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

   <span data-ttu-id="392ae-241">Si obtiene errores de compilación, intente cerrar y volver a abrir la solución.</span><span class="sxs-lookup"><span data-stu-id="392ae-241">If you get compile errors, try closing and reopening the solution.</span></span> 

1. <span data-ttu-id="392ae-242">Abra otra instancia del explorador (sirve cualquiera).</span><span class="sxs-lookup"><span data-stu-id="392ae-242">Open another browser instance (any browser).</span></span> <span data-ttu-id="392ae-243">Pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="392ae-243">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="392ae-244">Elija un explorador, escriba algo en el cuadro de texto **Mensaje** y haga clic en el botón **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="392ae-244">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="392ae-245">El nombre de usuario único y el mensaje se muestran en las dos páginas al instante.</span><span class="sxs-lookup"><span data-stu-id="392ae-245">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="392ae-246">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="392ae-246">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="392ae-247">Ejecute Webpack en modo *release* mediante la ejecución del comando siguiente en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="392ae-247">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="392ae-248">Compile y ejecute la aplicación mediante la ejecución del comando siguiente en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="392ae-248">Build and run the app by executing the following command in the project root:</span></span>

    ```dotnetcli
    dotnet run
    ```

    <span data-ttu-id="392ae-249">El servidor web inicia la aplicación y hace que esté disponible en el host local.</span><span class="sxs-lookup"><span data-stu-id="392ae-249">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="392ae-250">Abra un explorador en `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="392ae-250">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="392ae-251">Se entrega el archivo *wwwroot/index.html*.</span><span class="sxs-lookup"><span data-stu-id="392ae-251">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="392ae-252">Copie la dirección URL de la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="392ae-252">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="392ae-253">Abra otra instancia del explorador (sirve cualquiera).</span><span class="sxs-lookup"><span data-stu-id="392ae-253">Open another browser instance (any browser).</span></span> <span data-ttu-id="392ae-254">Pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="392ae-254">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="392ae-255">Elija un explorador, escriba algo en el cuadro de texto **Mensaje** y haga clic en el botón **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="392ae-255">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="392ae-256">El nombre de usuario único y el mensaje se muestran en las dos páginas al instante.</span><span class="sxs-lookup"><span data-stu-id="392ae-256">The unique user name and message are displayed on both pages instantly.</span></span>

---

![mensaje mostrado en las dos ventanas del explorador](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="392ae-258">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="392ae-258">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="392ae-259">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) con la carga de trabajo **ASP.NET y desarrollo web**</span><span class="sxs-lookup"><span data-stu-id="392ae-259">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="392ae-260">.NET Core SDK 2.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="392ae-260">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="392ae-261">[Node.js](https://nodejs.org/) con [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="392ae-261">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="392ae-262">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="392ae-262">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="392ae-263">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="392ae-263">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="392ae-264">.NET Core SDK 2.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="392ae-264">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="392ae-265">C# para Visual Studio Code versión 1.17.1 o posterior</span><span class="sxs-lookup"><span data-stu-id="392ae-265">C# for Visual Studio Code version 1.17.1 or later</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="392ae-266">[Node.js](https://nodejs.org/) con [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="392ae-266">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="392ae-267">Creación de la aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="392ae-267">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="392ae-268">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="392ae-268">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="392ae-269">Configure Visual Studio para buscar npm en la variable de entorno *PATH*.</span><span class="sxs-lookup"><span data-stu-id="392ae-269">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="392ae-270">De forma predeterminada, Visual Studio usa la versión de npm que se encuentra en su directorio de instalación.</span><span class="sxs-lookup"><span data-stu-id="392ae-270">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="392ae-271">Siga estas instrucciones en Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="392ae-271">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="392ae-272">Vaya a **Herramientas** > **Opciones** > **Proyectos y soluciones** > **Administración de paquetes web** > **Herramientas web externas**.</span><span class="sxs-lookup"><span data-stu-id="392ae-272">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="392ae-273">Seleccione la entrada *$(PATH)* en la lista.</span><span class="sxs-lookup"><span data-stu-id="392ae-273">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="392ae-274">Haga clic en la flecha arriba para mover la entrada a la segunda posición de la lista.</span><span class="sxs-lookup"><span data-stu-id="392ae-274">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Configuración de Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="392ae-276">Se ha completado la configuración de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="392ae-276">Visual Studio configuration is completed.</span></span> <span data-ttu-id="392ae-277">Es el momento de crear el proyecto.</span><span class="sxs-lookup"><span data-stu-id="392ae-277">It's time to create the project.</span></span>

1. <span data-ttu-id="392ae-278">Use la opción de menú **Archivo** > **Nuevo** > **Proyecto** y seleccione la plantilla **Aplicación web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="392ae-278">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="392ae-279">Asigne el nombre *SignalRWebPack* al proyecto y seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="392ae-279">Name the project *SignalRWebPack*, and select **Create**.</span></span>
1. <span data-ttu-id="392ae-280">Seleccione *.NET Core* en la lista desplegable de plataforma de destino y *ASP.NET Core 2.2* en la lista desplegable del selector de plataforma.</span><span class="sxs-lookup"><span data-stu-id="392ae-280">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.2* from the framework selector drop-down.</span></span> <span data-ttu-id="392ae-281">Seleccione la plantilla **Vacía** y **Crear**.</span><span class="sxs-lookup"><span data-stu-id="392ae-281">Select the **Empty** template, and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="392ae-282">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="392ae-282">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="392ae-283">Ejecute el comando siguiente en el **terminal integrado**:</span><span class="sxs-lookup"><span data-stu-id="392ae-283">Run the following command in the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="392ae-284">Se crea una aplicación web ASP.NET Core vacía, destinada a .NET Core, en el directorio *SignalRWebPack*.</span><span class="sxs-lookup"><span data-stu-id="392ae-284">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="392ae-285">Configuración de Webpack y TypeScript</span><span class="sxs-lookup"><span data-stu-id="392ae-285">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="392ae-286">Los pasos siguientes permiten configurar la conversión de TypeScript a JavaScript y la agrupación de los recursos del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="392ae-286">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="392ae-287">Ejecute el comando siguiente en la raíz del proyecto para crear un archivo *package.json*:</span><span class="sxs-lookup"><span data-stu-id="392ae-287">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="392ae-288">Agregue la propiedad resaltada al archivo *package.json*:</span><span class="sxs-lookup"><span data-stu-id="392ae-288">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/2.x/snippets/package1.json?highlight=4)]

    <span data-ttu-id="392ae-289">Si establece la propiedad `private` en `true`, evitará las advertencias de la instalación de paquetes en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="392ae-289">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="392ae-290">Instale los paquetes npm necesarios.</span><span class="sxs-lookup"><span data-stu-id="392ae-290">Install the required npm packages.</span></span> <span data-ttu-id="392ae-291">Ejecute el comando siguiente desde la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="392ae-291">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    <span data-ttu-id="392ae-292">Algunos detalles del comando para tener en cuenta:</span><span class="sxs-lookup"><span data-stu-id="392ae-292">Some command details to note:</span></span>

    * <span data-ttu-id="392ae-293">En cada nombre de paquete, un número de versión sigue al signo `@`.</span><span class="sxs-lookup"><span data-stu-id="392ae-293">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="392ae-294">npm instala esas versiones de paquete específicas.</span><span class="sxs-lookup"><span data-stu-id="392ae-294">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="392ae-295">La opción `-E` deshabilita el comportamiento predeterminado de npm de escribir operadores de intervalo de [versionamiento semántico](https://semver.org/) en *package.json*.</span><span class="sxs-lookup"><span data-stu-id="392ae-295">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="392ae-296">Por ejemplo, se usa `"webpack": "4.29.3"` en lugar de `"webpack": "^4.29.3"`.</span><span class="sxs-lookup"><span data-stu-id="392ae-296">For example, `"webpack": "4.29.3"` is used instead of `"webpack": "^4.29.3"`.</span></span> <span data-ttu-id="392ae-297">Esta opción impide actualizaciones no deseadas a versiones más recientes del paquete.</span><span class="sxs-lookup"><span data-stu-id="392ae-297">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="392ae-298">Vea la documentación oficial de [npm-install](https://docs.npmjs.com/cli/install) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="392ae-298">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="392ae-299">Reemplace la propiedad `scripts` del archivo *package.json* por el fragmento de código siguiente:</span><span class="sxs-lookup"><span data-stu-id="392ae-299">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="392ae-300">Más detalles sobre los scripts:</span><span class="sxs-lookup"><span data-stu-id="392ae-300">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="392ae-301">`build`: agrupa los recursos del lado cliente en modo de desarrollo y supervisa los cambios del archivo.</span><span class="sxs-lookup"><span data-stu-id="392ae-301">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="392ae-302">El monitor de archivos hace que la agrupación se vuelva a generar cada vez que cambia un archivo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="392ae-302">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="392ae-303">La opción `mode` deshabilita las optimizaciones de producción, como la agitación del árbol y la minificación.</span><span class="sxs-lookup"><span data-stu-id="392ae-303">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="392ae-304">Use `build` únicamente durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="392ae-304">Only use `build` in development.</span></span>
    * <span data-ttu-id="392ae-305">`release`: agrupa los recursos del lado cliente en modo de producción.</span><span class="sxs-lookup"><span data-stu-id="392ae-305">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="392ae-306">`publish`: ejecuta el script `release` para agrupar los recursos del lado cliente en modo de producción.</span><span class="sxs-lookup"><span data-stu-id="392ae-306">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="392ae-307">Llama al comando [publish](/dotnet/core/tools/dotnet-publish) de la CLI de .NET Core para publicar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="392ae-307">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="392ae-308">Cree un archivo denominado *webpack.config.js*, en la raíz del proyecto, con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="392ae-308">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/2.x/webpack.config.js)]

    <span data-ttu-id="392ae-309">El archivo anterior configura la compilación de Webpack.</span><span class="sxs-lookup"><span data-stu-id="392ae-309">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="392ae-310">Algunos detalles de configuración para tener en cuenta:</span><span class="sxs-lookup"><span data-stu-id="392ae-310">Some configuration details to note:</span></span>

    * <span data-ttu-id="392ae-311">La propiedad `output` invalida el valor predeterminado de *dist*.</span><span class="sxs-lookup"><span data-stu-id="392ae-311">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="392ae-312">En su lugar, la agrupación se genera en el directorio *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="392ae-312">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="392ae-313">La matriz `resolve.extensions` incluye *.js* para importar el código JavaScript cliente de SignalR.</span><span class="sxs-lookup"><span data-stu-id="392ae-313">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="392ae-314">Cree un directorio *src* en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="392ae-314">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="392ae-315">Su función es almacenar los activos del lado cliente del proyecto.</span><span class="sxs-lookup"><span data-stu-id="392ae-315">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="392ae-316">Cree *src/index.html* con el contenido siguiente.</span><span class="sxs-lookup"><span data-stu-id="392ae-316">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/2.x/src/index.html)]

    <span data-ttu-id="392ae-317">El código HTML anterior define el marcado reutilizable de la página principal.</span><span class="sxs-lookup"><span data-stu-id="392ae-317">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="392ae-318">Cree un directorio *src/css*.</span><span class="sxs-lookup"><span data-stu-id="392ae-318">Create a new *src/css* directory.</span></span> <span data-ttu-id="392ae-319">Su objetivo es almacenar los archivos *.css* del proyecto.</span><span class="sxs-lookup"><span data-stu-id="392ae-319">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="392ae-320">Cree *src/css/main.css* con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="392ae-320">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/2.x/src/css/main.css)]

    <span data-ttu-id="392ae-321">El archivo *main.css* anterior aplica estilo a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="392ae-321">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="392ae-322">Cree *src/tsconfig.json* con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="392ae-322">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/2.x/src/tsconfig.json)]

    <span data-ttu-id="392ae-323">El código anterior configura el compilador de TypeScript para generar JavaScript compatible con [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5.</span><span class="sxs-lookup"><span data-stu-id="392ae-323">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="392ae-324">Cree *src/index.ts* con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="392ae-324">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="392ae-325">El elemento TypeScript anterior recupera las referencias a elementos DOM y adjunta dos controladores de eventos:</span><span class="sxs-lookup"><span data-stu-id="392ae-325">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="392ae-326">`keyup`: este evento se desencadena cuando el usuario escribe algo en el cuadro de texto identificado como `tbMessage`.</span><span class="sxs-lookup"><span data-stu-id="392ae-326">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="392ae-327">La función `send` se llama cuando el usuario presiona la tecla **Entrar**.</span><span class="sxs-lookup"><span data-stu-id="392ae-327">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="392ae-328">`click`: este evento se desencadena cuando el usuario hace clic en el botón **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="392ae-328">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="392ae-329">Se llama a la función `send`.</span><span class="sxs-lookup"><span data-stu-id="392ae-329">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="392ae-330">Configuración de la aplicación ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="392ae-330">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="392ae-331">El código proporcionado en el método `Startup.Configure` muestra *Hello World!* .</span><span class="sxs-lookup"><span data-stu-id="392ae-331">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="392ae-332">Reemplace la llamada al método `app.Run` por las llamadas a [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) y [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="392ae-332">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="392ae-333">El código anterior permite que el servidor busque y proporcione el archivo *index.html*, con independencia de que el usuario escriba su dirección URL completa o la dirección URL raíz de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="392ae-333">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="392ae-334">Llame a [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) en el método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="392ae-334">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="392ae-335">Esto permite agregar los servicios SignalR al proyecto.</span><span class="sxs-lookup"><span data-stu-id="392ae-335">It adds the SignalR services to your project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="392ae-336">Asigne una ruta */hub* al concentrador `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="392ae-336">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="392ae-337">Agregue las líneas siguientes al final del método `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="392ae-337">Add the following lines at the end of the `Startup.Configure` method:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseSignalR)]

1. <span data-ttu-id="392ae-338">Cree un directorio denominado *Hubs* en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="392ae-338">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="392ae-339">Su objetivo es almacenar el concentrador de SignalR, que se crea en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="392ae-339">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="392ae-340">Cree el concentrador *Hubs/ChatHub.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="392ae-340">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="392ae-341">Agregue el código siguiente en la parte superior del archivo *Startup.cs* para resolver la referencia a `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="392ae-341">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="392ae-342">Habilitar la comunicación entre cliente y servidor</span><span class="sxs-lookup"><span data-stu-id="392ae-342">Enable client and server communication</span></span>

<span data-ttu-id="392ae-343">Actualmente, en la aplicación se muestra un formulario simple para enviar mensajes.</span><span class="sxs-lookup"><span data-stu-id="392ae-343">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="392ae-344">Al intentar hacer algo no sucede nada.</span><span class="sxs-lookup"><span data-stu-id="392ae-344">Nothing happens when you try to do so.</span></span> <span data-ttu-id="392ae-345">El servidor está escuchando en una ruta específica, pero no hace nada con los mensajes enviados.</span><span class="sxs-lookup"><span data-stu-id="392ae-345">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="392ae-346">Ejecute el comando siguiente en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="392ae-346">Execute the following command at the project root:</span></span>

    ```console
    npm install @aspnet/signalr
    ```

    <span data-ttu-id="392ae-347">El comando anterior instala el [cliente TypeScript de SignalR](https://www.npmjs.com/package/@aspnet/signalr), que permite al cliente enviar mensajes al servidor.</span><span class="sxs-lookup"><span data-stu-id="392ae-347">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@aspnet/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="392ae-348">Agregue el código resaltado al archivo *src/index.ts*:</span><span class="sxs-lookup"><span data-stu-id="392ae-348">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="392ae-349">El código anterior admite la recepción de mensajes desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="392ae-349">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="392ae-350">La clase `HubConnectionBuilder` crea un generador para configurar la conexión al servidor.</span><span class="sxs-lookup"><span data-stu-id="392ae-350">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="392ae-351">La función `withUrl` configura la dirección URL del concentrador.</span><span class="sxs-lookup"><span data-stu-id="392ae-351">The `withUrl` function configures the hub URL.</span></span>

    <span data-ttu-id="392ae-352">SignalR permite el intercambio de mensajes entre un cliente y un servidor.</span><span class="sxs-lookup"><span data-stu-id="392ae-352">SignalR enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="392ae-353">Cada mensaje tiene un nombre específico.</span><span class="sxs-lookup"><span data-stu-id="392ae-353">Each message has a specific name.</span></span> <span data-ttu-id="392ae-354">Por ejemplo, puede haber mensajes con el nombre `messageReceived` que ejecuten la lógica responsable de mostrar el mensaje nuevo en la zona de mensajes.</span><span class="sxs-lookup"><span data-stu-id="392ae-354">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="392ae-355">La escucha a un mensaje concreto se puede realizar mediante la función `on`.</span><span class="sxs-lookup"><span data-stu-id="392ae-355">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="392ae-356">Puede escuchar a cualquier número de nombres de mensaje.</span><span class="sxs-lookup"><span data-stu-id="392ae-356">You can listen to any number of message names.</span></span> <span data-ttu-id="392ae-357">También se pueden pasar parámetros al mensaje, como el nombre del autor y el contenido del mensaje recibido.</span><span class="sxs-lookup"><span data-stu-id="392ae-357">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="392ae-358">Una vez que el cliente recibe un mensaje, se crea un elemento `div` con el nombre del autor y el contenido del mensaje en su atributo `innerHTML`.</span><span class="sxs-lookup"><span data-stu-id="392ae-358">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="392ae-359">Se agrega al elemento `div` principal que muestra los mensajes.</span><span class="sxs-lookup"><span data-stu-id="392ae-359">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="392ae-360">Ahora que el cliente puede recibir mensajes, debe configurarlo para poder enviarlos.</span><span class="sxs-lookup"><span data-stu-id="392ae-360">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="392ae-361">Agregue el código resaltado al archivo *src/index.ts*:</span><span class="sxs-lookup"><span data-stu-id="392ae-361">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/src/index.ts?highlight=34-35)]

    <span data-ttu-id="392ae-362">El envío de mensajes a través de la conexión de WebSockets requiere llamar al método `send`.</span><span class="sxs-lookup"><span data-stu-id="392ae-362">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="392ae-363">El primer parámetro del método es el nombre del mensaje.</span><span class="sxs-lookup"><span data-stu-id="392ae-363">The method's first parameter is the message name.</span></span> <span data-ttu-id="392ae-364">Los datos del mensaje se encuentran en los otros parámetros.</span><span class="sxs-lookup"><span data-stu-id="392ae-364">The message data inhabits the other parameters.</span></span> <span data-ttu-id="392ae-365">En este ejemplo, se envía al servidor un mensaje identificado como `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="392ae-365">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="392ae-366">El mensaje está formado por el nombre de usuario y la entrada del usuario desde un cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="392ae-366">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="392ae-367">Si el envío funciona, se borra el valor del cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="392ae-367">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="392ae-368">Agregue el método resaltado a la clase `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="392ae-368">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="392ae-369">El código anterior difunde los mensajes recibidos a todos los usuarios conectados, una vez que el servidor los recibe.</span><span class="sxs-lookup"><span data-stu-id="392ae-369">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="392ae-370">No es necesario tener un método `on` genérico para recibir todos los mensajes.</span><span class="sxs-lookup"><span data-stu-id="392ae-370">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="392ae-371">Basta con un método que tenga el nombre del mensaje.</span><span class="sxs-lookup"><span data-stu-id="392ae-371">A method named after the message name suffices.</span></span>

    <span data-ttu-id="392ae-372">En este ejemplo, el cliente de TypeScript envía un mensaje que se identifica como `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="392ae-372">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="392ae-373">El método `NewMessage` de C# espera los datos enviados por el cliente.</span><span class="sxs-lookup"><span data-stu-id="392ae-373">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="392ae-374">Se realiza una llamada al método [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) de [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="392ae-374">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="392ae-375">Los mensajes recibidos se envían a todos los clientes conectados al concentrador.</span><span class="sxs-lookup"><span data-stu-id="392ae-375">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="392ae-376">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="392ae-376">Test the app</span></span>

<span data-ttu-id="392ae-377">Confirme que la aplicación funciona con los pasos siguientes.</span><span class="sxs-lookup"><span data-stu-id="392ae-377">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="392ae-378">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="392ae-378">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="392ae-379">Ejecute Webpack en modo *release*.</span><span class="sxs-lookup"><span data-stu-id="392ae-379">Run Webpack in *release* mode.</span></span> <span data-ttu-id="392ae-380">Desde la ventana **Consola del administrador de paquetes**, ejecute el comando siguiente en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="392ae-380">Using the **Package Manager Console** window, execute the following command in the project root.</span></span> <span data-ttu-id="392ae-381">Si no está en la raíz del proyecto, escriba `cd SignalRWebPack` antes de introducir el comando.</span><span class="sxs-lookup"><span data-stu-id="392ae-381">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="392ae-382">Seleccione **Depurar** > **Iniciar sin depurar** para iniciar la aplicación en un explorador sin adjuntar el depurador.</span><span class="sxs-lookup"><span data-stu-id="392ae-382">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="392ae-383">El archivo *wwwroot/index.html* se entrega en `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="392ae-383">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="392ae-384">Abra otra instancia del explorador (sirve cualquiera).</span><span class="sxs-lookup"><span data-stu-id="392ae-384">Open another browser instance (any browser).</span></span> <span data-ttu-id="392ae-385">Pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="392ae-385">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="392ae-386">Elija un explorador, escriba algo en el cuadro de texto **Mensaje** y haga clic en el botón **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="392ae-386">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="392ae-387">El nombre de usuario único y el mensaje se muestran en las dos páginas al instante.</span><span class="sxs-lookup"><span data-stu-id="392ae-387">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="392ae-388">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="392ae-388">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="392ae-389">Ejecute Webpack en modo *release* mediante la ejecución del comando siguiente en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="392ae-389">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="392ae-390">Compile y ejecute la aplicación mediante la ejecución del comando siguiente en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="392ae-390">Build and run the app by executing the following command in the project root:</span></span>

    ```dotnetcli
    dotnet run
    ```

    <span data-ttu-id="392ae-391">El servidor web inicia la aplicación y hace que esté disponible en el host local.</span><span class="sxs-lookup"><span data-stu-id="392ae-391">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="392ae-392">Abra un explorador en `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="392ae-392">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="392ae-393">Se entrega el archivo *wwwroot/index.html*.</span><span class="sxs-lookup"><span data-stu-id="392ae-393">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="392ae-394">Copie la dirección URL de la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="392ae-394">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="392ae-395">Abra otra instancia del explorador (sirve cualquiera).</span><span class="sxs-lookup"><span data-stu-id="392ae-395">Open another browser instance (any browser).</span></span> <span data-ttu-id="392ae-396">Pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="392ae-396">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="392ae-397">Elija un explorador, escriba algo en el cuadro de texto **Mensaje** y haga clic en el botón **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="392ae-397">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="392ae-398">El nombre de usuario único y el mensaje se muestran en las dos páginas al instante.</span><span class="sxs-lookup"><span data-stu-id="392ae-398">The unique user name and message are displayed on both pages instantly.</span></span>

---

![mensaje mostrado en las dos ventanas del explorador](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="392ae-400">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="392ae-400">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
