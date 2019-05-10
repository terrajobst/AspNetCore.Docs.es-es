---
title: Uso de la plantilla de proyecto de React con ASP.NET Core
author: SteveSandersonMS
description: Aprenda cómo comenzar a trabajar con la plantilla de proyecto de aplicación de página única (SPA) de ASP.NET Core para React y create-react-app.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 03/07/2019
uid: spa/react
ms.openlocfilehash: 91a71498574d6d96c2c06e896283fed801e8adb3
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64893702"
---
# <a name="use-the-react-project-template-with-aspnet-core"></a><span data-ttu-id="cc499-103">Uso de la plantilla de proyecto de React con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cc499-103">Use the React project template with ASP.NET Core</span></span>

<span data-ttu-id="cc499-104">La plantilla de proyecto actualizada de React ofrece un práctico punto de partida para las aplicaciones ASP.NET Core que usan React y las convenciones [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) para implementar una completa interfaz de usuario (UI) en el lado cliente.</span><span class="sxs-lookup"><span data-stu-id="cc499-104">The updated React project template provides a convenient starting point for ASP.NET Core apps using React and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="cc499-105">La plantilla es equivalente a crear un proyecto de ASP.NET Core para que funcione como un back-end de API y un proyecto de React de CRA estándar para que funcione como interfaz de usuario, pero con la comodidad de hospedar ambos en un único proyecto de aplicación que se puede compilar y publicar como una sola unidad.</span><span class="sxs-lookup"><span data-stu-id="cc499-105">The template is equivalent to creating both an ASP.NET Core project to act as an API backend, and a standard CRA React project to act as a UI, but with the convenience of hosting both in a single app project that can be built and published as a single unit.</span></span>

## <a name="create-a-new-app"></a><span data-ttu-id="cc499-106">Creación de una nueva aplicación</span><span class="sxs-lookup"><span data-stu-id="cc499-106">Create a new app</span></span>

<span data-ttu-id="cc499-107">Si tiene ASP.NET Core 2.1 instalado, no es necesario instalar la plantilla de proyecto de React.</span><span class="sxs-lookup"><span data-stu-id="cc499-107">If you have ASP.NET Core 2.1 installed, there's no need to install the React project template.</span></span>

<span data-ttu-id="cc499-108">En un símbolo del sistema, cree un nuevo proyecto con el comando `dotnet new react` en un directorio vacío.</span><span class="sxs-lookup"><span data-stu-id="cc499-108">Create a new project from a command prompt using the command `dotnet new react` in an empty directory.</span></span> <span data-ttu-id="cc499-109">Por ejemplo, los siguientes comandos crean la aplicación en un directorio *my-new-app* y cambian a ese directorio:</span><span class="sxs-lookup"><span data-stu-id="cc499-109">For example, the following commands create the app in a *my-new-app* directory and switch to that directory:</span></span>

```console
dotnet new react -o my-new-app
cd my-new-app
```

<span data-ttu-id="cc499-110">Ejecute la aplicación desde Visual Studio o la CLI de .NET Core:</span><span class="sxs-lookup"><span data-stu-id="cc499-110">Run the app from either Visual Studio or the .NET Core CLI:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cc499-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cc499-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="cc499-112">Abra el archivo *.csproj* generado y, desde ahí, ejecute la aplicación de la manera habitual.</span><span class="sxs-lookup"><span data-stu-id="cc499-112">Open the generated *.csproj* file, and run the app as normal from there.</span></span>

<span data-ttu-id="cc499-113">El proceso de compilación restaura las dependencias npm en la primera ejecución, lo que puede tardar varios minutos.</span><span class="sxs-lookup"><span data-stu-id="cc499-113">The build process restores npm dependencies on the first run, which can take several minutes.</span></span> <span data-ttu-id="cc499-114">Las compilaciones posteriores son mucho más rápidas.</span><span class="sxs-lookup"><span data-stu-id="cc499-114">Subsequent builds are much faster.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cc499-115">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="cc499-115">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="cc499-116">Asegúrese de tener una variable de entorno denominada `ASPNETCORE_Environment` con un valor de `Development`.</span><span class="sxs-lookup"><span data-stu-id="cc499-116">Ensure you have an environment variable called `ASPNETCORE_Environment` with value of `Development`.</span></span> <span data-ttu-id="cc499-117">En Windows (en los avisos que no son de PowerShell), ejecute `SET ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="cc499-117">On Windows (in non-PowerShell prompts), run `SET ASPNETCORE_Environment=Development`.</span></span> <span data-ttu-id="cc499-118">En Linux o macOS, ejecute `export ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="cc499-118">On Linux or macOS, run `export ASPNETCORE_Environment=Development`.</span></span>

<span data-ttu-id="cc499-119">Ejecute [dotnet build](/dotnet/core/tools/dotnet-build) para comprobar que la aplicación se compila correctamente.</span><span class="sxs-lookup"><span data-stu-id="cc499-119">Run [dotnet build](/dotnet/core/tools/dotnet-build) to verify your app builds correctly.</span></span> <span data-ttu-id="cc499-120">En la primera ejecución, el proceso de compilación restaura las dependencias de npm, lo que puede tardar varios minutos.</span><span class="sxs-lookup"><span data-stu-id="cc499-120">On the first run, the build process restores npm dependencies, which can take several minutes.</span></span> <span data-ttu-id="cc499-121">Las compilaciones posteriores son mucho más rápidas.</span><span class="sxs-lookup"><span data-stu-id="cc499-121">Subsequent builds are much faster.</span></span>

<span data-ttu-id="cc499-122">Ejecute [dotnet run](/dotnet/core/tools/dotnet-run) para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="cc499-122">Run [dotnet run](/dotnet/core/tools/dotnet-run) to start the app.</span></span>

---

<span data-ttu-id="cc499-123">La plantilla de proyecto crea una aplicación ASP.NET Core y una aplicación de React.</span><span class="sxs-lookup"><span data-stu-id="cc499-123">The project template creates an ASP.NET Core app and a React app.</span></span> <span data-ttu-id="cc499-124">El uso previsto de la aplicación ASP.NET Core es el acceso a los datos, la autorización y otros problemas relativos al servidor.</span><span class="sxs-lookup"><span data-stu-id="cc499-124">The ASP.NET Core app is intended to be used for data access, authorization, and other server-side concerns.</span></span> <span data-ttu-id="cc499-125">La aplicación de React, que reside en el subdirectorio *ClientApp*, está diseñada para utilizarse para su uso con todos los problemas de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="cc499-125">The React app, residing in the *ClientApp* subdirectory, is intended to be used for all UI concerns.</span></span>

## <a name="add-pages-images-styles-modules-etc"></a><span data-ttu-id="cc499-126">Adición de páginas, imágenes, estilos, módulos, etc.</span><span class="sxs-lookup"><span data-stu-id="cc499-126">Add pages, images, styles, modules, etc.</span></span>

<span data-ttu-id="cc499-127">El directorio *ClientApp* es una aplicación de React de CRA estándar.</span><span class="sxs-lookup"><span data-stu-id="cc499-127">The *ClientApp* directory is a standard CRA React app.</span></span> <span data-ttu-id="cc499-128">Para más información, consulte la [documentación oficial de CRA](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md).</span><span class="sxs-lookup"><span data-stu-id="cc499-128">See the official [CRA documentation](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) for more information.</span></span>

<span data-ttu-id="cc499-129">Existe pequeñas diferencias entre la aplicación de React creada mediante este plantilla y la creada mediante CRA propiamente dicho; sin embargo, las funcionalidades de la aplicación permanecen sin cambios.</span><span class="sxs-lookup"><span data-stu-id="cc499-129">There are slight differences between the React app created by this template and the one created by CRA itself; however, the app's capabilities are unchanged.</span></span> <span data-ttu-id="cc499-130">La aplicación creada con la plantilla contiene un diseño basado en [arranque](https://getbootstrap.com/) y un ejemplo de enrutamiento básico.</span><span class="sxs-lookup"><span data-stu-id="cc499-130">The app created by the template contains a [Bootstrap](https://getbootstrap.com/)-based layout and a basic routing example.</span></span>

## <a name="install-npm-packages"></a><span data-ttu-id="cc499-131">Instalar paquetes de npm</span><span class="sxs-lookup"><span data-stu-id="cc499-131">Install npm packages</span></span>

<span data-ttu-id="cc499-132">Para instalar paquetes de npm de otro fabricante, use un símbolo del sistema en el subdirectorio *ClientApp*.</span><span class="sxs-lookup"><span data-stu-id="cc499-132">To install third-party npm packages, use a command prompt in the *ClientApp* subdirectory.</span></span> <span data-ttu-id="cc499-133">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="cc499-133">For example:</span></span>

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a><span data-ttu-id="cc499-134">Publicación e implementación</span><span class="sxs-lookup"><span data-stu-id="cc499-134">Publish and deploy</span></span>

<span data-ttu-id="cc499-135">En el desarrollo, la aplicación se ejecuta en modo optimizado para comodidad del desarrollador.</span><span class="sxs-lookup"><span data-stu-id="cc499-135">In development, the app runs in a mode optimized for developer convenience.</span></span> <span data-ttu-id="cc499-136">Por ejemplo, agrupaciones de JavaScript contienen asignaciones de origen (de modo que durante la depuración, puede ver el código fuente original).</span><span class="sxs-lookup"><span data-stu-id="cc499-136">For example, JavaScript bundles include source maps (so that when debugging, you can see your original source code).</span></span> <span data-ttu-id="cc499-137">La aplicación inspecciona los cambios en los archivos de JavaScript, HTML y CSS en el disco y, automáticamente, realiza una nueva compilación y recarga cuando observa que esos archivos han cambiado.</span><span class="sxs-lookup"><span data-stu-id="cc499-137">The app watches JavaScript, HTML, and CSS file changes on disk and automatically recompiles and reloads when it sees those files change.</span></span>

<span data-ttu-id="cc499-138">En producción, use una versión de la aplicación que esté optimizada para el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="cc499-138">In production, serve a version of your app that's optimized for performance.</span></span> <span data-ttu-id="cc499-139">Esto se configura para que tenga lugar automáticamente.</span><span class="sxs-lookup"><span data-stu-id="cc499-139">This is configured to happen automatically.</span></span> <span data-ttu-id="cc499-140">Al publicar, la configuración de compilación emite una compilación transpilada reducida del código de cliente.</span><span class="sxs-lookup"><span data-stu-id="cc499-140">When you publish, the build configuration emits a minified, transpiled build of your client-side code.</span></span> <span data-ttu-id="cc499-141">A diferencia de la compilación de desarrollo, la compilación de producción no requiere la instalación de Node.js en el servidor.</span><span class="sxs-lookup"><span data-stu-id="cc499-141">Unlike the development build, the production build doesn't require Node.js to be installed on the server.</span></span>

<span data-ttu-id="cc499-142">Puede usar [métodos de implementación y hospedaje de ASP.NET Core](xref:host-and-deploy/index) estándar.</span><span class="sxs-lookup"><span data-stu-id="cc499-142">You can use standard [ASP.NET Core hosting and deployment methods](xref:host-and-deploy/index).</span></span>

## <a name="run-the-cra-server-independently"></a><span data-ttu-id="cc499-143">Ejecución del servidor de CRA de forma independiente</span><span class="sxs-lookup"><span data-stu-id="cc499-143">Run the CRA server independently</span></span>

<span data-ttu-id="cc499-144">El proyecto está configurado para iniciar su propia instancia del servidor de desarrollo de CRA en segundo plano cuando la aplicación ASP.NET Core se inicia en modo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="cc499-144">The project is configured to start its own instance of the CRA development server in the background when the ASP.NET Core app starts in development mode.</span></span> <span data-ttu-id="cc499-145">Esto resulta útil porque significa que no tiene que ejecutar manualmente un servidor independiente.</span><span class="sxs-lookup"><span data-stu-id="cc499-145">This is convenient because it means you don't have to run a separate server manually.</span></span>

<span data-ttu-id="cc499-146">Sin embargo, esta configuración predeterminada tiene un inconveniente.</span><span class="sxs-lookup"><span data-stu-id="cc499-146">There's a drawback to this default setup.</span></span> <span data-ttu-id="cc499-147">Cada vez que modifica el código de C# y la aplicación ASP.NET Core debe reiniciarse, el servidor de CRA se reinicia.</span><span class="sxs-lookup"><span data-stu-id="cc499-147">Each time you modify your C# code and your ASP.NET Core app needs to restart, the CRA server restarts.</span></span> <span data-ttu-id="cc499-148">Se necesitan unos segundos para iniciar la copia de seguridad.</span><span class="sxs-lookup"><span data-stu-id="cc499-148">A few seconds are required to start back up.</span></span> <span data-ttu-id="cc499-149">Sin realiza frecuentes modificaciones en el código de C# y no quiere esperar a que se reinicie el servidor de CRA, ejecute el servidor de CRA externamente, con independencia del proceso de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cc499-149">If you're making frequent C# code edits and don't want to wait for the CRA server to restart, run the CRA server externally, independently of the ASP.NET Core process.</span></span> <span data-ttu-id="cc499-150">Para ello:</span><span class="sxs-lookup"><span data-stu-id="cc499-150">To do so:</span></span>

1. <span data-ttu-id="cc499-151">Agregar un *.env* del archivo a la *ClientApp* subdirectorio con la siguiente configuración:</span><span class="sxs-lookup"><span data-stu-id="cc499-151">Add a *.env* file to the *ClientApp* subdirectory with the following setting:</span></span>

    ```
    BROWSER=none
    ```

    <span data-ttu-id="cc499-152">Esto impedirá que el explorador web al abrir al iniciar el servidor CRA externamente.</span><span class="sxs-lookup"><span data-stu-id="cc499-152">This will prevent your web browser from opening when starting the CRA server externally.</span></span>

2. <span data-ttu-id="cc499-153">En un símbolo del sistema, cambie al subdirectorio *ClientApp* e inicie el servidor de desarrollo de CRA:</span><span class="sxs-lookup"><span data-stu-id="cc499-153">In a command prompt, switch to the *ClientApp* subdirectory, and launch the CRA development server:</span></span>

    ```console
    cd ClientApp
    npm start
    ```

3. <span data-ttu-id="cc499-154">Modifique la aplicación ASP.NET Core para usar la instancia del servidor de CRA externo en lugar de iniciar una de las suyas.</span><span class="sxs-lookup"><span data-stu-id="cc499-154">Modify your ASP.NET Core app to use the external CRA server instance instead of launching one of its own.</span></span> <span data-ttu-id="cc499-155">En la clase *Startup*, reemplace la invocación de `spa.UseReactDevelopmentServer` por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="cc499-155">In your *Startup* class, replace the `spa.UseReactDevelopmentServer` invocation with the following:</span></span>

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

<span data-ttu-id="cc499-156">Cuando inicie la aplicación ASP.NET Core, no se iniciará un servidor de CRA.</span><span class="sxs-lookup"><span data-stu-id="cc499-156">When you start your ASP.NET Core app, it won't launch a CRA server.</span></span> <span data-ttu-id="cc499-157">En su lugar, se usa la instancia que inició manualmente.</span><span class="sxs-lookup"><span data-stu-id="cc499-157">The instance you started manually is used instead.</span></span> <span data-ttu-id="cc499-158">Esto le permite iniciar y reiniciar con mayor rapidez.</span><span class="sxs-lookup"><span data-stu-id="cc499-158">This enables it to start and restart faster.</span></span> <span data-ttu-id="cc499-159">Ya no tiene que esperar a que la aplicación de React se recompile de una vez a otra.</span><span class="sxs-lookup"><span data-stu-id="cc499-159">It's no longer waiting for your React app to rebuild each time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cc499-160">"Representación del lado servidor" no es una característica compatible con esta plantilla.</span><span class="sxs-lookup"><span data-stu-id="cc499-160">"Server-side rendering" is not a supported feature of this template.</span></span> <span data-ttu-id="cc499-161">Nuestro objetivo con esta plantilla es satisfacer la paridad con "create de react-app".</span><span class="sxs-lookup"><span data-stu-id="cc499-161">Our goal with this template is to meet parity with "create-react-app".</span></span> <span data-ttu-id="cc499-162">Por lo tanto, escenarios y características no incluidas en un proyecto de "creación de react-app" (por ejemplo, el SSR) no se admiten y se dejan como un ejercicio para el usuario.</span><span class="sxs-lookup"><span data-stu-id="cc499-162">As such, scenarios and features not included in a "create-react-app" project (such as SSR) are not supported and are left as an exercise for the user.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cc499-163">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="cc499-163">Additional resources</span></span>

* <xref:security/authentication/identity/spa>
