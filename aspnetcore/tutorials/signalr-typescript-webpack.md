---
title: Uso de ASP.NET Core SignalR con TypeScript and Webpack
author: ssougnez
description: En este tutorial, configurará Webpack para agrupar y compilar una aplicación web ASP.NET Core SignalR cuyo cliente está escrito en TypeScript.
ms.author: bradyg
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- SignalR
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: 9094a1d391c087a6f58aa9dd66e3697a79f4af86
ms.sourcegitcommit: ef1720cb733908f36a54825d84c3461c5280bdbe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2020
ms.locfileid: "75737521"
---
# <a name="use-aspnet-core-opno-locsignalr-with-typescript-and-webpack"></a><span data-ttu-id="ec537-103">Uso de ASP.NET Core SignalR con TypeScript and Webpack</span><span class="sxs-lookup"><span data-stu-id="ec537-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="ec537-104">Por [Sébastien Sougnez](https://twitter.com/ssougnez) y [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="ec537-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="ec537-105">[Webpack](https://webpack.js.org/) permite a los desarrolladores agrupar y compilar los recursos del lado cliente de una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="ec537-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="ec537-106">En este tutorial se describe el uso de Webpack en una aplicación web ASP.NET Core SignalR cuyo cliente está escrito en [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="ec537-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="ec537-107">En este tutorial aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="ec537-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ec537-108">Agregar scaffold a una aplicación ASP.NET Core SignalR de inicio</span><span class="sxs-lookup"><span data-stu-id="ec537-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="ec537-109">Configurar el cliente de TypeScript para SignalR</span><span class="sxs-lookup"><span data-stu-id="ec537-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="ec537-110">Configuración de una canalización de compilación mediante Webpack</span><span class="sxs-lookup"><span data-stu-id="ec537-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="ec537-111">Configurar el servidor SignalR</span><span class="sxs-lookup"><span data-stu-id="ec537-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="ec537-112">Habilitar la comunicación entre cliente y servidor</span><span class="sxs-lookup"><span data-stu-id="ec537-112">Enable communication between client and server</span></span>

<span data-ttu-id="ec537-113">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ec537-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="ec537-114">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="ec537-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ec537-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ec537-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ec537-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) con la carga de trabajo **ASP.NET y desarrollo web**</span><span class="sxs-lookup"><span data-stu-id="ec537-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="ec537-117">.NET Core SDK 3.0 o posterior</span><span class="sxs-lookup"><span data-stu-id="ec537-117">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="ec537-118">[Node.js](https://nodejs.org/) con [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="ec537-118">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ec537-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ec537-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="ec537-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ec537-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="ec537-121">.NET Core SDK 3.0 o posterior</span><span class="sxs-lookup"><span data-stu-id="ec537-121">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="ec537-122">C# para Visual Studio Code versión 1.17.1 o posterior</span><span class="sxs-lookup"><span data-stu-id="ec537-122">C# for Visual Studio Code version 1.17.1 or later</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="ec537-123">[Node.js](https://nodejs.org/) con [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="ec537-123">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="ec537-124">Creación de la aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ec537-124">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ec537-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ec537-125">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ec537-126">Configure Visual Studio para buscar npm en la variable de entorno *PATH*.</span><span class="sxs-lookup"><span data-stu-id="ec537-126">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="ec537-127">De forma predeterminada, Visual Studio usa la versión de npm que se encuentra en su directorio de instalación.</span><span class="sxs-lookup"><span data-stu-id="ec537-127">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="ec537-128">Siga estas instrucciones en Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="ec537-128">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="ec537-129">Vaya a **Herramientas** > **Opciones** > **Proyectos y soluciones** > **Administración de paquetes web** > **Herramientas web externas**.</span><span class="sxs-lookup"><span data-stu-id="ec537-129">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="ec537-130">Seleccione la entrada *$(PATH)* en la lista.</span><span class="sxs-lookup"><span data-stu-id="ec537-130">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="ec537-131">Haga clic en la flecha arriba para mover la entrada a la segunda posición de la lista.</span><span class="sxs-lookup"><span data-stu-id="ec537-131">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Configuración de Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="ec537-133">Se ha completado la configuración de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ec537-133">Visual Studio configuration is completed.</span></span> <span data-ttu-id="ec537-134">Es el momento de crear el proyecto.</span><span class="sxs-lookup"><span data-stu-id="ec537-134">It's time to create the project.</span></span>

1. <span data-ttu-id="ec537-135">Use la opción de menú **Archivo** > **Nuevo** > **Proyecto** y seleccione la plantilla **Aplicación web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ec537-135">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="ec537-136">Asigne el nombre *SignalRWebPack* al proyecto y seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="ec537-136">Name the project *SignalRWebPack*, and select **Create**.</span></span>
1. <span data-ttu-id="ec537-137">Seleccione *.NET Core* en la lista desplegable de plataforma de destino y *ASP.NET Core 3.0* en la lista desplegable del selector de plataforma.</span><span class="sxs-lookup"><span data-stu-id="ec537-137">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 3.0* from the framework selector drop-down.</span></span> <span data-ttu-id="ec537-138">Seleccione la plantilla **Vacía** y **Crear**.</span><span class="sxs-lookup"><span data-stu-id="ec537-138">Select the **Empty** template, and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ec537-139">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ec537-139">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ec537-140">Ejecute el comando siguiente en el **terminal integrado**:</span><span class="sxs-lookup"><span data-stu-id="ec537-140">Run the following command in the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="ec537-141">Se crea una aplicación web ASP.NET Core vacía, destinada a .NET Core, en el directorio *SignalRWebPack*.</span><span class="sxs-lookup"><span data-stu-id="ec537-141">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="ec537-142">Configuración de Webpack y TypeScript</span><span class="sxs-lookup"><span data-stu-id="ec537-142">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="ec537-143">Los pasos siguientes permiten configurar la conversión de TypeScript a JavaScript y la agrupación de los recursos del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="ec537-143">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="ec537-144">Ejecute el comando siguiente en la raíz del proyecto para crear un archivo *package.json*:</span><span class="sxs-lookup"><span data-stu-id="ec537-144">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="ec537-145">Agregue la propiedad resaltada al archivo *package.json*:</span><span class="sxs-lookup"><span data-stu-id="ec537-145">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/3.x/snippets/package1.json?highlight=4)]

    <span data-ttu-id="ec537-146">Si establece la propiedad `private` en `true`, evitará las advertencias de la instalación de paquetes en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="ec537-146">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="ec537-147">Instale los paquetes npm necesarios.</span><span class="sxs-lookup"><span data-stu-id="ec537-147">Install the required npm packages.</span></span> <span data-ttu-id="ec537-148">Ejecute el comando siguiente desde la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="ec537-148">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    <span data-ttu-id="ec537-149">Algunos detalles del comando para tener en cuenta:</span><span class="sxs-lookup"><span data-stu-id="ec537-149">Some command details to note:</span></span>

    * <span data-ttu-id="ec537-150">En cada nombre de paquete, un número de versión sigue al signo `@`.</span><span class="sxs-lookup"><span data-stu-id="ec537-150">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="ec537-151">npm instala esas versiones de paquete específicas.</span><span class="sxs-lookup"><span data-stu-id="ec537-151">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="ec537-152">La opción `-E` deshabilita el comportamiento predeterminado de npm de escribir operadores de intervalo de [versionamiento semántico](https://semver.org/) en *package.json*.</span><span class="sxs-lookup"><span data-stu-id="ec537-152">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="ec537-153">Por ejemplo, se usa `"webpack": "4.29.3"` en lugar de `"webpack": "^4.29.3"`.</span><span class="sxs-lookup"><span data-stu-id="ec537-153">For example, `"webpack": "4.29.3"` is used instead of `"webpack": "^4.29.3"`.</span></span> <span data-ttu-id="ec537-154">Esta opción impide actualizaciones no deseadas a versiones más recientes del paquete.</span><span class="sxs-lookup"><span data-stu-id="ec537-154">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="ec537-155">Vea la documentación oficial de [npm-install](https://docs.npmjs.com/cli/install) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="ec537-155">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="ec537-156">Reemplace la propiedad `scripts` del archivo *package.json* por el fragmento de código siguiente:</span><span class="sxs-lookup"><span data-stu-id="ec537-156">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="ec537-157">Más detalles sobre los scripts:</span><span class="sxs-lookup"><span data-stu-id="ec537-157">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="ec537-158">`build`: agrupa los recursos del lado cliente en modo de desarrollo y supervisa los cambios del archivo.</span><span class="sxs-lookup"><span data-stu-id="ec537-158">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="ec537-159">El monitor de archivos hace que la agrupación se vuelva a generar cada vez que cambia un archivo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="ec537-159">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="ec537-160">La opción `mode` deshabilita las optimizaciones de producción, como la agitación del árbol y la minificación.</span><span class="sxs-lookup"><span data-stu-id="ec537-160">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="ec537-161">Use `build` únicamente durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="ec537-161">Only use `build` in development.</span></span>
    * <span data-ttu-id="ec537-162">`release`: agrupa los recursos del lado cliente en modo de producción.</span><span class="sxs-lookup"><span data-stu-id="ec537-162">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="ec537-163">`publish`: ejecuta el script `release` para agrupar los recursos del lado cliente en modo de producción.</span><span class="sxs-lookup"><span data-stu-id="ec537-163">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="ec537-164">Llama al comando [publish](/dotnet/core/tools/dotnet-publish) de la CLI de .NET Core para publicar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ec537-164">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="ec537-165">Cree un archivo denominado *webpack.config.js*, en la raíz del proyecto, con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="ec537-165">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/3.x/webpack.config.js)]

    <span data-ttu-id="ec537-166">El archivo anterior configura la compilación de Webpack.</span><span class="sxs-lookup"><span data-stu-id="ec537-166">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="ec537-167">Algunos detalles de configuración para tener en cuenta:</span><span class="sxs-lookup"><span data-stu-id="ec537-167">Some configuration details to note:</span></span>

    * <span data-ttu-id="ec537-168">La propiedad `output` invalida el valor predeterminado de *dist*.</span><span class="sxs-lookup"><span data-stu-id="ec537-168">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="ec537-169">En su lugar, la agrupación se genera en el directorio *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="ec537-169">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="ec537-170">La matriz `resolve.extensions` incluye *.js* para importar el código JavaScript del cliente de SignalR.</span><span class="sxs-lookup"><span data-stu-id="ec537-170">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="ec537-171">Cree un directorio *src* en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="ec537-171">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="ec537-172">Su función es almacenar los activos del lado cliente del proyecto.</span><span class="sxs-lookup"><span data-stu-id="ec537-172">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="ec537-173">Cree *src/index.html* con el contenido siguiente.</span><span class="sxs-lookup"><span data-stu-id="ec537-173">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/3.x/src/index.html)]

    <span data-ttu-id="ec537-174">El código HTML anterior define el marcado reutilizable de la página principal.</span><span class="sxs-lookup"><span data-stu-id="ec537-174">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="ec537-175">Cree un directorio *src/css*.</span><span class="sxs-lookup"><span data-stu-id="ec537-175">Create a new *src/css* directory.</span></span> <span data-ttu-id="ec537-176">Su objetivo es almacenar los archivos *.css* del proyecto.</span><span class="sxs-lookup"><span data-stu-id="ec537-176">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="ec537-177">Cree *src/css/main.css* con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="ec537-177">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/3.x/src/css/main.css)]

    <span data-ttu-id="ec537-178">El archivo *main.css* anterior aplica estilo a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ec537-178">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="ec537-179">Cree *src/tsconfig.json* con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="ec537-179">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/3.x/src/tsconfig.json)]

    <span data-ttu-id="ec537-180">El código anterior configura el compilador de TypeScript para generar JavaScript compatible con [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5.</span><span class="sxs-lookup"><span data-stu-id="ec537-180">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="ec537-181">Cree *src/index.ts* con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="ec537-181">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="ec537-182">El elemento TypeScript anterior recupera las referencias a elementos DOM y adjunta dos controladores de eventos:</span><span class="sxs-lookup"><span data-stu-id="ec537-182">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="ec537-183">`keyup`: este evento se desencadena cuando el usuario escribe algo en el cuadro de texto identificado como `tbMessage`.</span><span class="sxs-lookup"><span data-stu-id="ec537-183">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="ec537-184">La función `send` se llama cuando el usuario presiona la tecla **Entrar**.</span><span class="sxs-lookup"><span data-stu-id="ec537-184">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="ec537-185">`click`: este evento se desencadena cuando el usuario hace clic en el botón **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="ec537-185">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="ec537-186">Se llama a la función `send`.</span><span class="sxs-lookup"><span data-stu-id="ec537-186">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="ec537-187">Configuración de la aplicación ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ec537-187">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="ec537-188">En el método `Startup.Configure`, agregue llamadas a [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) y [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="ec537-188">In the `Startup.Configure` method, add calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseStaticDefaultFiles&highlight=2-3)]

   <span data-ttu-id="ec537-189">El código anterior permite que el servidor busque y proporcione el archivo *index.html*, con independencia de que el usuario escriba su dirección URL completa o la dirección URL raíz de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="ec537-189">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="ec537-190">Al final del método `Startup.Configure`, asigne una ruta */hub* al centro `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="ec537-190">At the end of the `Startup.Configure` method, map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="ec537-191">Reemplace el código que muestra *Hola mundo*</span><span class="sxs-lookup"><span data-stu-id="ec537-191">Replace the code that displays *Hello World!*</span></span> <span data-ttu-id="ec537-192">por la línea siguiente:</span><span class="sxs-lookup"><span data-stu-id="ec537-192">with the following line:</span></span> 

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseSignalR&highlight=3)]

1. <span data-ttu-id="ec537-193">En el método `Startup.ConfigureServices`, llame a [AddSignalR ](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_).</span><span class="sxs-lookup"><span data-stu-id="ec537-193">In the `Startup.ConfigureServices` method, call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_).</span></span> <span data-ttu-id="ec537-194">Esta acción agrega los servicios SignalR al proyecto.</span><span class="sxs-lookup"><span data-stu-id="ec537-194">It adds the SignalR services to your project.</span></span>

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="ec537-195">Cree un directorio denominado *Hubs* en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="ec537-195">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="ec537-196">Su objetivo es almacenar el concentrador de SignalR, que se crea en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="ec537-196">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="ec537-197">Cree el concentrador *Hubs/ChatHub.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="ec537-197">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="ec537-198">Agregue el código siguiente en la parte superior del archivo *Startup.cs* para resolver la referencia a `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="ec537-198">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="ec537-199">Habilitar la comunicación entre cliente y servidor</span><span class="sxs-lookup"><span data-stu-id="ec537-199">Enable client and server communication</span></span>

<span data-ttu-id="ec537-200">Actualmente, en la aplicación se muestra un formulario simple para enviar mensajes.</span><span class="sxs-lookup"><span data-stu-id="ec537-200">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="ec537-201">Al intentar hacer algo no sucede nada.</span><span class="sxs-lookup"><span data-stu-id="ec537-201">Nothing happens when you try to do so.</span></span> <span data-ttu-id="ec537-202">El servidor está escuchando en una ruta específica, pero no hace nada con los mensajes enviados.</span><span class="sxs-lookup"><span data-stu-id="ec537-202">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="ec537-203">Ejecute el comando siguiente en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="ec537-203">Execute the following command at the project root:</span></span>

    ```console
    npm install @microsoft/signalr
    ```

    <span data-ttu-id="ec537-204">El comando anterior instala el [cliente TypeScript de SignalR](https://www.npmjs.com/package/@microsoft/signalr), que permite al cliente enviar mensajes al servidor.</span><span class="sxs-lookup"><span data-stu-id="ec537-204">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@microsoft/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="ec537-205">Agregue el código resaltado al archivo *src/index.ts*:</span><span class="sxs-lookup"><span data-stu-id="ec537-205">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="ec537-206">El código anterior admite la recepción de mensajes desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="ec537-206">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="ec537-207">La clase `HubConnectionBuilder` crea un generador para configurar la conexión al servidor.</span><span class="sxs-lookup"><span data-stu-id="ec537-207">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="ec537-208">La función `withUrl` configura la dirección URL del concentrador.</span><span class="sxs-lookup"><span data-stu-id="ec537-208">The `withUrl` function configures the hub URL.</span></span>

    SignalR<span data-ttu-id="ec537-209"> permite el intercambio de mensajes entre un cliente y un servidor.</span><span class="sxs-lookup"><span data-stu-id="ec537-209"> enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="ec537-210">Cada mensaje tiene un nombre específico.</span><span class="sxs-lookup"><span data-stu-id="ec537-210">Each message has a specific name.</span></span> <span data-ttu-id="ec537-211">Por ejemplo, puede haber mensajes con el nombre `messageReceived` que ejecuten la lógica responsable de mostrar el mensaje nuevo en la zona de mensajes.</span><span class="sxs-lookup"><span data-stu-id="ec537-211">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="ec537-212">La escucha a un mensaje concreto se puede realizar mediante la función `on`.</span><span class="sxs-lookup"><span data-stu-id="ec537-212">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="ec537-213">Puede escuchar a cualquier número de nombres de mensaje.</span><span class="sxs-lookup"><span data-stu-id="ec537-213">You can listen to any number of message names.</span></span> <span data-ttu-id="ec537-214">También se pueden pasar parámetros al mensaje, como el nombre del autor y el contenido del mensaje recibido.</span><span class="sxs-lookup"><span data-stu-id="ec537-214">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="ec537-215">Una vez que el cliente recibe un mensaje, se crea un elemento `div` con el nombre del autor y el contenido del mensaje en su atributo `innerHTML`.</span><span class="sxs-lookup"><span data-stu-id="ec537-215">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="ec537-216">Se agrega al elemento `div` principal que muestra los mensajes.</span><span class="sxs-lookup"><span data-stu-id="ec537-216">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="ec537-217">Ahora que el cliente puede recibir mensajes, debe configurarlo para poder enviarlos.</span><span class="sxs-lookup"><span data-stu-id="ec537-217">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="ec537-218">Agregue el código resaltado al archivo *src/index.ts*:</span><span class="sxs-lookup"><span data-stu-id="ec537-218">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/src/index.ts?highlight=34-35)]

    <span data-ttu-id="ec537-219">El envío de mensajes a través de la conexión de WebSockets requiere llamar al método `send`.</span><span class="sxs-lookup"><span data-stu-id="ec537-219">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="ec537-220">El primer parámetro del método es el nombre del mensaje.</span><span class="sxs-lookup"><span data-stu-id="ec537-220">The method's first parameter is the message name.</span></span> <span data-ttu-id="ec537-221">Los datos del mensaje se encuentran en los otros parámetros.</span><span class="sxs-lookup"><span data-stu-id="ec537-221">The message data inhabits the other parameters.</span></span> <span data-ttu-id="ec537-222">En este ejemplo, se envía al servidor un mensaje identificado como `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="ec537-222">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="ec537-223">El mensaje está formado por el nombre de usuario y la entrada del usuario desde un cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="ec537-223">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="ec537-224">Si el envío funciona, se borra el valor del cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="ec537-224">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="ec537-225">Agregue el método resaltado a la clase `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="ec537-225">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="ec537-226">El código anterior difunde los mensajes recibidos a todos los usuarios conectados, una vez que el servidor los recibe.</span><span class="sxs-lookup"><span data-stu-id="ec537-226">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="ec537-227">No es necesario tener un método `on` genérico para recibir todos los mensajes.</span><span class="sxs-lookup"><span data-stu-id="ec537-227">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="ec537-228">Basta con un método que tenga el nombre del mensaje.</span><span class="sxs-lookup"><span data-stu-id="ec537-228">A method named after the message name suffices.</span></span>

    <span data-ttu-id="ec537-229">En este ejemplo, el cliente de TypeScript envía un mensaje que se identifica como `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="ec537-229">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="ec537-230">El método `NewMessage` de C# espera los datos enviados por el cliente.</span><span class="sxs-lookup"><span data-stu-id="ec537-230">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="ec537-231">Se realiza una llamada al método [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) de [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="ec537-231">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="ec537-232">Los mensajes recibidos se envían a todos los clientes conectados al concentrador.</span><span class="sxs-lookup"><span data-stu-id="ec537-232">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="ec537-233">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="ec537-233">Test the app</span></span>

<span data-ttu-id="ec537-234">Confirme que la aplicación funciona con los pasos siguientes.</span><span class="sxs-lookup"><span data-stu-id="ec537-234">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ec537-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ec537-235">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="ec537-236">Ejecute Webpack en modo *release*.</span><span class="sxs-lookup"><span data-stu-id="ec537-236">Run Webpack in *release* mode.</span></span> <span data-ttu-id="ec537-237">Desde la ventana **Consola del administrador de paquetes**, ejecute el comando siguiente en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="ec537-237">Using the **Package Manager Console** window, execute the following command in the project root.</span></span> <span data-ttu-id="ec537-238">Si no está en la raíz del proyecto, escriba `cd SignalRWebPack` antes de introducir el comando.</span><span class="sxs-lookup"><span data-stu-id="ec537-238">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="ec537-239">Seleccione **Depurar** > **Iniciar sin depurar** para iniciar la aplicación en un explorador sin adjuntar el depurador.</span><span class="sxs-lookup"><span data-stu-id="ec537-239">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="ec537-240">El archivo *wwwroot/index.html* se entrega en `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="ec537-240">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

   <span data-ttu-id="ec537-241">Si obtiene errores de compilación, intente cerrar y volver a abrir la solución.</span><span class="sxs-lookup"><span data-stu-id="ec537-241">If you get compile errors, try closing and reopening the solution.</span></span> 

1. <span data-ttu-id="ec537-242">Abra otra instancia del explorador (sirve cualquiera).</span><span class="sxs-lookup"><span data-stu-id="ec537-242">Open another browser instance (any browser).</span></span> <span data-ttu-id="ec537-243">Pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="ec537-243">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="ec537-244">Elija un explorador, escriba algo en el cuadro de texto **Mensaje** y haga clic en el botón **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="ec537-244">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="ec537-245">El nombre de usuario único y el mensaje se muestran en las dos páginas al instante.</span><span class="sxs-lookup"><span data-stu-id="ec537-245">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ec537-246">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ec537-246">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="ec537-247">Ejecute Webpack en modo *release* mediante la ejecución del comando siguiente en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="ec537-247">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="ec537-248">Compile y ejecute la aplicación mediante la ejecución del comando siguiente en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="ec537-248">Build and run the app by executing the following command in the project root:</span></span>

    ```dotnetcli
    dotnet run
    ```

    <span data-ttu-id="ec537-249">El servidor web inicia la aplicación y hace que esté disponible en el host local.</span><span class="sxs-lookup"><span data-stu-id="ec537-249">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="ec537-250">Abra un explorador en `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="ec537-250">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="ec537-251">Se entrega el archivo *wwwroot/index.html*.</span><span class="sxs-lookup"><span data-stu-id="ec537-251">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="ec537-252">Copie la dirección URL de la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="ec537-252">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="ec537-253">Abra otra instancia del explorador (sirve cualquiera).</span><span class="sxs-lookup"><span data-stu-id="ec537-253">Open another browser instance (any browser).</span></span> <span data-ttu-id="ec537-254">Pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="ec537-254">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="ec537-255">Elija un explorador, escriba algo en el cuadro de texto **Mensaje** y haga clic en el botón **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="ec537-255">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="ec537-256">El nombre de usuario único y el mensaje se muestran en las dos páginas al instante.</span><span class="sxs-lookup"><span data-stu-id="ec537-256">The unique user name and message are displayed on both pages instantly.</span></span>

---

![mensaje mostrado en las dos ventanas del explorador](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="ec537-258">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="ec537-258">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ec537-259">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ec537-259">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ec537-260">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) con la carga de trabajo **ASP.NET y desarrollo web**</span><span class="sxs-lookup"><span data-stu-id="ec537-260">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="ec537-261">.NET Core SDK 2.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="ec537-261">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="ec537-262">[Node.js](https://nodejs.org/) con [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="ec537-262">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ec537-263">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ec537-263">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="ec537-264">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ec537-264">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="ec537-265">.NET Core SDK 2.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="ec537-265">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="ec537-266">C# para Visual Studio Code versión 1.17.1 o posterior</span><span class="sxs-lookup"><span data-stu-id="ec537-266">C# for Visual Studio Code version 1.17.1 or later</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="ec537-267">[Node.js](https://nodejs.org/) con [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="ec537-267">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="ec537-268">Creación de la aplicación web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ec537-268">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ec537-269">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ec537-269">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ec537-270">Configure Visual Studio para buscar npm en la variable de entorno *PATH*.</span><span class="sxs-lookup"><span data-stu-id="ec537-270">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="ec537-271">De forma predeterminada, Visual Studio usa la versión de npm que se encuentra en su directorio de instalación.</span><span class="sxs-lookup"><span data-stu-id="ec537-271">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="ec537-272">Siga estas instrucciones en Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="ec537-272">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="ec537-273">Vaya a **Herramientas** > **Opciones** > **Proyectos y soluciones** > **Administración de paquetes web** > **Herramientas web externas**.</span><span class="sxs-lookup"><span data-stu-id="ec537-273">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="ec537-274">Seleccione la entrada *$(PATH)* en la lista.</span><span class="sxs-lookup"><span data-stu-id="ec537-274">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="ec537-275">Haga clic en la flecha arriba para mover la entrada a la segunda posición de la lista.</span><span class="sxs-lookup"><span data-stu-id="ec537-275">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Configuración de Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="ec537-277">Se ha completado la configuración de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ec537-277">Visual Studio configuration is completed.</span></span> <span data-ttu-id="ec537-278">Es el momento de crear el proyecto.</span><span class="sxs-lookup"><span data-stu-id="ec537-278">It's time to create the project.</span></span>

1. <span data-ttu-id="ec537-279">Use la opción de menú **Archivo** > **Nuevo** > **Proyecto** y seleccione la plantilla **Aplicación web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ec537-279">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="ec537-280">Asigne el nombre *SignalRWebPack* al proyecto y seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="ec537-280">Name the project *SignalRWebPack*, and select **Create**.</span></span>
1. <span data-ttu-id="ec537-281">Seleccione *.NET Core* en la lista desplegable de plataforma de destino y *ASP.NET Core 2.2* en la lista desplegable del selector de plataforma.</span><span class="sxs-lookup"><span data-stu-id="ec537-281">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.2* from the framework selector drop-down.</span></span> <span data-ttu-id="ec537-282">Seleccione la plantilla **Vacía** y **Crear**.</span><span class="sxs-lookup"><span data-stu-id="ec537-282">Select the **Empty** template, and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ec537-283">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ec537-283">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ec537-284">Ejecute el comando siguiente en el **terminal integrado**:</span><span class="sxs-lookup"><span data-stu-id="ec537-284">Run the following command in the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="ec537-285">Se crea una aplicación web ASP.NET Core vacía, destinada a .NET Core, en el directorio *SignalRWebPack*.</span><span class="sxs-lookup"><span data-stu-id="ec537-285">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="ec537-286">Configuración de Webpack y TypeScript</span><span class="sxs-lookup"><span data-stu-id="ec537-286">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="ec537-287">Los pasos siguientes permiten configurar la conversión de TypeScript a JavaScript y la agrupación de los recursos del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="ec537-287">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="ec537-288">Ejecute el comando siguiente en la raíz del proyecto para crear un archivo *package.json*:</span><span class="sxs-lookup"><span data-stu-id="ec537-288">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="ec537-289">Agregue la propiedad resaltada al archivo *package.json*:</span><span class="sxs-lookup"><span data-stu-id="ec537-289">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/2.x/snippets/package1.json?highlight=4)]

    <span data-ttu-id="ec537-290">Si establece la propiedad `private` en `true`, evitará las advertencias de la instalación de paquetes en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="ec537-290">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="ec537-291">Instale los paquetes npm necesarios.</span><span class="sxs-lookup"><span data-stu-id="ec537-291">Install the required npm packages.</span></span> <span data-ttu-id="ec537-292">Ejecute el comando siguiente desde la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="ec537-292">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    <span data-ttu-id="ec537-293">Algunos detalles del comando para tener en cuenta:</span><span class="sxs-lookup"><span data-stu-id="ec537-293">Some command details to note:</span></span>

    * <span data-ttu-id="ec537-294">En cada nombre de paquete, un número de versión sigue al signo `@`.</span><span class="sxs-lookup"><span data-stu-id="ec537-294">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="ec537-295">npm instala esas versiones de paquete específicas.</span><span class="sxs-lookup"><span data-stu-id="ec537-295">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="ec537-296">La opción `-E` deshabilita el comportamiento predeterminado de npm de escribir operadores de intervalo de [versionamiento semántico](https://semver.org/) en *package.json*.</span><span class="sxs-lookup"><span data-stu-id="ec537-296">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="ec537-297">Por ejemplo, se usa `"webpack": "4.29.3"` en lugar de `"webpack": "^4.29.3"`.</span><span class="sxs-lookup"><span data-stu-id="ec537-297">For example, `"webpack": "4.29.3"` is used instead of `"webpack": "^4.29.3"`.</span></span> <span data-ttu-id="ec537-298">Esta opción impide actualizaciones no deseadas a versiones más recientes del paquete.</span><span class="sxs-lookup"><span data-stu-id="ec537-298">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="ec537-299">Vea la documentación oficial de [npm-install](https://docs.npmjs.com/cli/install) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="ec537-299">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="ec537-300">Reemplace la propiedad `scripts` del archivo *package.json* por el fragmento de código siguiente:</span><span class="sxs-lookup"><span data-stu-id="ec537-300">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="ec537-301">Más detalles sobre los scripts:</span><span class="sxs-lookup"><span data-stu-id="ec537-301">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="ec537-302">`build`: agrupa los recursos del lado cliente en modo de desarrollo y supervisa los cambios del archivo.</span><span class="sxs-lookup"><span data-stu-id="ec537-302">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="ec537-303">El monitor de archivos hace que la agrupación se vuelva a generar cada vez que cambia un archivo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="ec537-303">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="ec537-304">La opción `mode` deshabilita las optimizaciones de producción, como la agitación del árbol y la minificación.</span><span class="sxs-lookup"><span data-stu-id="ec537-304">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="ec537-305">Use `build` únicamente durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="ec537-305">Only use `build` in development.</span></span>
    * <span data-ttu-id="ec537-306">`release`: agrupa los recursos del lado cliente en modo de producción.</span><span class="sxs-lookup"><span data-stu-id="ec537-306">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="ec537-307">`publish`: ejecuta el script `release` para agrupar los recursos del lado cliente en modo de producción.</span><span class="sxs-lookup"><span data-stu-id="ec537-307">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="ec537-308">Llama al comando [publish](/dotnet/core/tools/dotnet-publish) de la CLI de .NET Core para publicar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ec537-308">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="ec537-309">Cree un archivo denominado *webpack.config.js*, en la raíz del proyecto, con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="ec537-309">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/2.x/webpack.config.js)]

    <span data-ttu-id="ec537-310">El archivo anterior configura la compilación de Webpack.</span><span class="sxs-lookup"><span data-stu-id="ec537-310">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="ec537-311">Algunos detalles de configuración para tener en cuenta:</span><span class="sxs-lookup"><span data-stu-id="ec537-311">Some configuration details to note:</span></span>

    * <span data-ttu-id="ec537-312">La propiedad `output` invalida el valor predeterminado de *dist*.</span><span class="sxs-lookup"><span data-stu-id="ec537-312">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="ec537-313">En su lugar, la agrupación se genera en el directorio *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="ec537-313">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="ec537-314">La matriz `resolve.extensions` incluye *.js* para importar el código JavaScript del cliente de SignalR.</span><span class="sxs-lookup"><span data-stu-id="ec537-314">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="ec537-315">Cree un directorio *src* en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="ec537-315">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="ec537-316">Su función es almacenar los activos del lado cliente del proyecto.</span><span class="sxs-lookup"><span data-stu-id="ec537-316">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="ec537-317">Cree *src/index.html* con el contenido siguiente.</span><span class="sxs-lookup"><span data-stu-id="ec537-317">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/2.x/src/index.html)]

    <span data-ttu-id="ec537-318">El código HTML anterior define el marcado reutilizable de la página principal.</span><span class="sxs-lookup"><span data-stu-id="ec537-318">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="ec537-319">Cree un directorio *src/css*.</span><span class="sxs-lookup"><span data-stu-id="ec537-319">Create a new *src/css* directory.</span></span> <span data-ttu-id="ec537-320">Su objetivo es almacenar los archivos *.css* del proyecto.</span><span class="sxs-lookup"><span data-stu-id="ec537-320">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="ec537-321">Cree *src/css/main.css* con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="ec537-321">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/2.x/src/css/main.css)]

    <span data-ttu-id="ec537-322">El archivo *main.css* anterior aplica estilo a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ec537-322">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="ec537-323">Cree *src/tsconfig.json* con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="ec537-323">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/2.x/src/tsconfig.json)]

    <span data-ttu-id="ec537-324">El código anterior configura el compilador de TypeScript para generar JavaScript compatible con [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5.</span><span class="sxs-lookup"><span data-stu-id="ec537-324">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="ec537-325">Cree *src/index.ts* con el contenido siguiente:</span><span class="sxs-lookup"><span data-stu-id="ec537-325">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="ec537-326">El elemento TypeScript anterior recupera las referencias a elementos DOM y adjunta dos controladores de eventos:</span><span class="sxs-lookup"><span data-stu-id="ec537-326">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="ec537-327">`keyup`: este evento se desencadena cuando el usuario escribe algo en el cuadro de texto identificado como `tbMessage`.</span><span class="sxs-lookup"><span data-stu-id="ec537-327">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="ec537-328">La función `send` se llama cuando el usuario presiona la tecla **Entrar**.</span><span class="sxs-lookup"><span data-stu-id="ec537-328">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="ec537-329">`click`: este evento se desencadena cuando el usuario hace clic en el botón **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="ec537-329">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="ec537-330">Se llama a la función `send`.</span><span class="sxs-lookup"><span data-stu-id="ec537-330">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="ec537-331">Configuración de la aplicación ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ec537-331">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="ec537-332">El código proporcionado en el método `Startup.Configure` muestra *Hello World!* .</span><span class="sxs-lookup"><span data-stu-id="ec537-332">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="ec537-333">Reemplace la llamada al método `app.Run` por las llamadas a [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) y [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="ec537-333">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="ec537-334">El código anterior permite que el servidor busque y proporcione el archivo *index.html*, con independencia de que el usuario escriba su dirección URL completa o la dirección URL raíz de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="ec537-334">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="ec537-335">Llame a [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) en el método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ec537-335">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="ec537-336">Esta acción agrega los servicios SignalR al proyecto.</span><span class="sxs-lookup"><span data-stu-id="ec537-336">It adds the SignalR services to your project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="ec537-337">Asigne una ruta */hub* al concentrador `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="ec537-337">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="ec537-338">Agregue las líneas siguientes al final del método `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="ec537-338">Add the following lines at the end of the `Startup.Configure` method:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseSignalR)]

1. <span data-ttu-id="ec537-339">Cree un directorio denominado *Hubs* en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="ec537-339">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="ec537-340">Su objetivo es almacenar el concentrador de SignalR, que se crea en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="ec537-340">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="ec537-341">Cree el concentrador *Hubs/ChatHub.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="ec537-341">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="ec537-342">Agregue el código siguiente en la parte superior del archivo *Startup.cs* para resolver la referencia a `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="ec537-342">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="ec537-343">Habilitar la comunicación entre cliente y servidor</span><span class="sxs-lookup"><span data-stu-id="ec537-343">Enable client and server communication</span></span>

<span data-ttu-id="ec537-344">Actualmente, en la aplicación se muestra un formulario simple para enviar mensajes.</span><span class="sxs-lookup"><span data-stu-id="ec537-344">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="ec537-345">Al intentar hacer algo no sucede nada.</span><span class="sxs-lookup"><span data-stu-id="ec537-345">Nothing happens when you try to do so.</span></span> <span data-ttu-id="ec537-346">El servidor está escuchando en una ruta específica, pero no hace nada con los mensajes enviados.</span><span class="sxs-lookup"><span data-stu-id="ec537-346">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="ec537-347">Ejecute el comando siguiente en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="ec537-347">Execute the following command at the project root:</span></span>

    ```console
    npm install @aspnet/signalr
    ```

    <span data-ttu-id="ec537-348">El comando anterior instala el [cliente TypeScript de SignalR](https://www.npmjs.com/package/@microsoft/signalr), que permite al cliente enviar mensajes al servidor.</span><span class="sxs-lookup"><span data-stu-id="ec537-348">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@microsoft/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="ec537-349">Agregue el código resaltado al archivo *src/index.ts*:</span><span class="sxs-lookup"><span data-stu-id="ec537-349">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="ec537-350">El código anterior admite la recepción de mensajes desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="ec537-350">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="ec537-351">La clase `HubConnectionBuilder` crea un generador para configurar la conexión al servidor.</span><span class="sxs-lookup"><span data-stu-id="ec537-351">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="ec537-352">La función `withUrl` configura la dirección URL del concentrador.</span><span class="sxs-lookup"><span data-stu-id="ec537-352">The `withUrl` function configures the hub URL.</span></span>

    SignalR<span data-ttu-id="ec537-353"> permite el intercambio de mensajes entre un cliente y un servidor.</span><span class="sxs-lookup"><span data-stu-id="ec537-353"> enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="ec537-354">Cada mensaje tiene un nombre específico.</span><span class="sxs-lookup"><span data-stu-id="ec537-354">Each message has a specific name.</span></span> <span data-ttu-id="ec537-355">Por ejemplo, puede haber mensajes con el nombre `messageReceived` que ejecuten la lógica responsable de mostrar el mensaje nuevo en la zona de mensajes.</span><span class="sxs-lookup"><span data-stu-id="ec537-355">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="ec537-356">La escucha a un mensaje concreto se puede realizar mediante la función `on`.</span><span class="sxs-lookup"><span data-stu-id="ec537-356">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="ec537-357">Puede escuchar a cualquier número de nombres de mensaje.</span><span class="sxs-lookup"><span data-stu-id="ec537-357">You can listen to any number of message names.</span></span> <span data-ttu-id="ec537-358">También se pueden pasar parámetros al mensaje, como el nombre del autor y el contenido del mensaje recibido.</span><span class="sxs-lookup"><span data-stu-id="ec537-358">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="ec537-359">Una vez que el cliente recibe un mensaje, se crea un elemento `div` con el nombre del autor y el contenido del mensaje en su atributo `innerHTML`.</span><span class="sxs-lookup"><span data-stu-id="ec537-359">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="ec537-360">Se agrega al elemento `div` principal que muestra los mensajes.</span><span class="sxs-lookup"><span data-stu-id="ec537-360">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="ec537-361">Ahora que el cliente puede recibir mensajes, debe configurarlo para poder enviarlos.</span><span class="sxs-lookup"><span data-stu-id="ec537-361">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="ec537-362">Agregue el código resaltado al archivo *src/index.ts*:</span><span class="sxs-lookup"><span data-stu-id="ec537-362">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/src/index.ts?highlight=34-35)]

    <span data-ttu-id="ec537-363">El envío de mensajes a través de la conexión de WebSockets requiere llamar al método `send`.</span><span class="sxs-lookup"><span data-stu-id="ec537-363">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="ec537-364">El primer parámetro del método es el nombre del mensaje.</span><span class="sxs-lookup"><span data-stu-id="ec537-364">The method's first parameter is the message name.</span></span> <span data-ttu-id="ec537-365">Los datos del mensaje se encuentran en los otros parámetros.</span><span class="sxs-lookup"><span data-stu-id="ec537-365">The message data inhabits the other parameters.</span></span> <span data-ttu-id="ec537-366">En este ejemplo, se envía al servidor un mensaje identificado como `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="ec537-366">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="ec537-367">El mensaje está formado por el nombre de usuario y la entrada del usuario desde un cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="ec537-367">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="ec537-368">Si el envío funciona, se borra el valor del cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="ec537-368">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="ec537-369">Agregue el método resaltado a la clase `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="ec537-369">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="ec537-370">El código anterior difunde los mensajes recibidos a todos los usuarios conectados, una vez que el servidor los recibe.</span><span class="sxs-lookup"><span data-stu-id="ec537-370">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="ec537-371">No es necesario tener un método `on` genérico para recibir todos los mensajes.</span><span class="sxs-lookup"><span data-stu-id="ec537-371">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="ec537-372">Basta con un método que tenga el nombre del mensaje.</span><span class="sxs-lookup"><span data-stu-id="ec537-372">A method named after the message name suffices.</span></span>

    <span data-ttu-id="ec537-373">En este ejemplo, el cliente de TypeScript envía un mensaje que se identifica como `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="ec537-373">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="ec537-374">El método `NewMessage` de C# espera los datos enviados por el cliente.</span><span class="sxs-lookup"><span data-stu-id="ec537-374">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="ec537-375">Se realiza una llamada al método [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) de [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="ec537-375">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="ec537-376">Los mensajes recibidos se envían a todos los clientes conectados al concentrador.</span><span class="sxs-lookup"><span data-stu-id="ec537-376">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="ec537-377">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="ec537-377">Test the app</span></span>

<span data-ttu-id="ec537-378">Confirme que la aplicación funciona con los pasos siguientes.</span><span class="sxs-lookup"><span data-stu-id="ec537-378">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ec537-379">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ec537-379">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="ec537-380">Ejecute Webpack en modo *release*.</span><span class="sxs-lookup"><span data-stu-id="ec537-380">Run Webpack in *release* mode.</span></span> <span data-ttu-id="ec537-381">Desde la ventana **Consola del administrador de paquetes**, ejecute el comando siguiente en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="ec537-381">Using the **Package Manager Console** window, execute the following command in the project root.</span></span> <span data-ttu-id="ec537-382">Si no está en la raíz del proyecto, escriba `cd SignalRWebPack` antes de introducir el comando.</span><span class="sxs-lookup"><span data-stu-id="ec537-382">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="ec537-383">Seleccione **Depurar** > **Iniciar sin depurar** para iniciar la aplicación en un explorador sin adjuntar el depurador.</span><span class="sxs-lookup"><span data-stu-id="ec537-383">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="ec537-384">El archivo *wwwroot/index.html* se entrega en `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="ec537-384">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="ec537-385">Abra otra instancia del explorador (sirve cualquiera).</span><span class="sxs-lookup"><span data-stu-id="ec537-385">Open another browser instance (any browser).</span></span> <span data-ttu-id="ec537-386">Pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="ec537-386">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="ec537-387">Elija un explorador, escriba algo en el cuadro de texto **Mensaje** y haga clic en el botón **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="ec537-387">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="ec537-388">El nombre de usuario único y el mensaje se muestran en las dos páginas al instante.</span><span class="sxs-lookup"><span data-stu-id="ec537-388">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ec537-389">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ec537-389">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="ec537-390">Ejecute Webpack en modo *release* mediante la ejecución del comando siguiente en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="ec537-390">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="ec537-391">Compile y ejecute la aplicación mediante la ejecución del comando siguiente en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="ec537-391">Build and run the app by executing the following command in the project root:</span></span>

    ```dotnetcli
    dotnet run
    ```

    <span data-ttu-id="ec537-392">El servidor web inicia la aplicación y hace que esté disponible en el host local.</span><span class="sxs-lookup"><span data-stu-id="ec537-392">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="ec537-393">Abra un explorador en `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="ec537-393">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="ec537-394">Se entrega el archivo *wwwroot/index.html*.</span><span class="sxs-lookup"><span data-stu-id="ec537-394">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="ec537-395">Copie la dirección URL de la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="ec537-395">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="ec537-396">Abra otra instancia del explorador (sirve cualquiera).</span><span class="sxs-lookup"><span data-stu-id="ec537-396">Open another browser instance (any browser).</span></span> <span data-ttu-id="ec537-397">Pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="ec537-397">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="ec537-398">Elija un explorador, escriba algo en el cuadro de texto **Mensaje** y haga clic en el botón **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="ec537-398">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="ec537-399">El nombre de usuario único y el mensaje se muestran en las dos páginas al instante.</span><span class="sxs-lookup"><span data-stu-id="ec537-399">The unique user name and message are displayed on both pages instantly.</span></span>

---

![mensaje mostrado en las dos ventanas del explorador](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="ec537-401">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ec537-401">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
