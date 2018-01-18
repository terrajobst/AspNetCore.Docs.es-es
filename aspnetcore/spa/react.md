---
title: Utilice la plantilla de proyecto de reaccionar
author: SteveSandersonMS
description: "Obtenga información acerca de cómo empezar a trabajar con la plantilla de proyecto de aplicación de página principal de ASP.NET (SPA) versión candidata para reaccionar y aplicación reaccionar crear."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/06/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react
ms.openlocfilehash: a63d81633a0f37d24ad5e05de293e3c41004eba1
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2018
---
# <a name="use-the-react-project-template-release-candidate"></a><span data-ttu-id="504a9-103">Utilice la plantilla de proyecto de reaccionar (candidato de versión comercial)</span><span class="sxs-lookup"><span data-stu-id="504a9-103">Use the React project template (release candidate)</span></span>

> [!NOTE]
> <span data-ttu-id="504a9-104">Esta documentación no está sobre la plantilla de proyecto de reaccionar publicada.</span><span class="sxs-lookup"><span data-stu-id="504a9-104">This documentation is not about the released React project template.</span></span> <span data-ttu-id="504a9-105">**Esta documentación está sobre el candidato de versión de la plantilla de reaccionar.**</span><span class="sxs-lookup"><span data-stu-id="504a9-105">**This documentation is about the release candidate of the React template.**</span></span> <span data-ttu-id="504a9-106">Esperamos que enviar la versión de lanzamiento en 2018 temprano.</span><span class="sxs-lookup"><span data-stu-id="504a9-106">We hope to ship the released version in early 2018.</span></span>

<span data-ttu-id="504a9-107">La plantilla de proyecto de reaccionar actualizada proporciona un punto de partida cómodo para ASP.NET Core aplicaciones con reaccionar y [aplicación reaccionar crear](https://github.com/facebookincubator/create-react-app) convenciones (CRA) para implementar una interfaz de usuario de cliente enriquecido (UI).</span><span class="sxs-lookup"><span data-stu-id="504a9-107">The updated React project template provides a convenient starting point for ASP.NET Core apps using React and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="504a9-108">La plantilla es equivalente a crear un proyecto de ASP.NET Core para actuar como un API de back-end y un proyecto de reaccionar CRA estándar para actuar como una interfaz de usuario, pero con la comodidad de hospedarlos en un proyecto de aplicación único que se puedan generar y publicar como una sola unidad.</span><span class="sxs-lookup"><span data-stu-id="504a9-108">The template is equivalent to creating both an ASP.NET Core project to act as an API backend, and a standard CRA React project to act as a UI, but with the convenience of hosting both in a single app project that can be built and published as a single unit.</span></span>

## <a name="create-a-new-app"></a><span data-ttu-id="504a9-109">Crear una nueva aplicación</span><span class="sxs-lookup"><span data-stu-id="504a9-109">Create a new app</span></span>

<span data-ttu-id="504a9-110">Para empezar, asegúrese de que haya [instalar la plantilla de proyecto de reaccionar actualizada](xref:spa/index#installation).</span><span class="sxs-lookup"><span data-stu-id="504a9-110">To get started, ensure you've [installed the updated React project template](xref:spa/index#installation).</span></span> <span data-ttu-id="504a9-111">Estas instrucciones no son aplicables a la plantilla de proyecto reaccionar anterior que se incluye en el núcleo de .NET SDK 2.0. x.</span><span class="sxs-lookup"><span data-stu-id="504a9-111">These instructions don't apply to the previous React project template included in the .NET Core 2.0.x SDK.</span></span>

<span data-ttu-id="504a9-112">Crear un nuevo proyecto desde un símbolo del sistema mediante el comando `dotnet new react` en un directorio vacío.</span><span class="sxs-lookup"><span data-stu-id="504a9-112">Create a new project from a command prompt using the command `dotnet new react` in an empty directory.</span></span> <span data-ttu-id="504a9-113">Por ejemplo, los siguientes comandos crean la aplicación en un *mi aplicación nuevo* directorio y cambie a ese directorio:</span><span class="sxs-lookup"><span data-stu-id="504a9-113">For example, the following commands create the app in a *my-new-app* directory and switch to that directory:</span></span>

```console
dotnet new -o my-new-app
cd my-new-app
```

<span data-ttu-id="504a9-114">Ejecute la aplicación desde Visual Studio o en el núcleo de .NET CLI:</span><span class="sxs-lookup"><span data-stu-id="504a9-114">Run the app from either Visual Studio or the .NET Core CLI:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="504a9-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="504a9-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="504a9-116">Abra el archivo *.csproj* archivo y ejecutar la aplicación como normal desde allí.</span><span class="sxs-lookup"><span data-stu-id="504a9-116">Open the generated *.csproj* file, and run the app as normal from there.</span></span>

<span data-ttu-id="504a9-117">El proceso de compilación restaura npm dependencias en la primera ejecución, lo que puede tardar varios minutos.</span><span class="sxs-lookup"><span data-stu-id="504a9-117">The build process restores npm dependencies on the first run, which can take several minutes.</span></span> <span data-ttu-id="504a9-118">Las compilaciones posteriores son mucho más rápidas.</span><span class="sxs-lookup"><span data-stu-id="504a9-118">Subsequent builds are much faster.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="504a9-119">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="504a9-119">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="504a9-120">Asegúrese de que tiene una variable de entorno denominada `ASPNETCORE_Environment` con el valor de `Development`.</span><span class="sxs-lookup"><span data-stu-id="504a9-120">Ensure you have an environment variable called `ASPNETCORE_Environment` with value of `Development`.</span></span> <span data-ttu-id="504a9-121">En Windows (en PowerShell no solicita), ejecute `SET ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="504a9-121">On Windows (in non-PowerShell prompts), run `SET ASPNETCORE_Environment=Development`.</span></span> <span data-ttu-id="504a9-122">En Linux o Mac OS, ejecute `export ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="504a9-122">On Linux or macOS, run `export ASPNETCORE_Environment=Development`.</span></span>

<span data-ttu-id="504a9-123">Ejecutar `dotnet build` para comprobar la aplicación se compila correctamente.</span><span class="sxs-lookup"><span data-stu-id="504a9-123">Run `dotnet build` to verify your app builds correctly.</span></span> <span data-ttu-id="504a9-124">En la primera ejecución, el proceso de compilación restaura las dependencias de npm, que pueden tardar varios minutos.</span><span class="sxs-lookup"><span data-stu-id="504a9-124">On the first run, the build process restores npm dependencies, which can take several minutes.</span></span> <span data-ttu-id="504a9-125">Las compilaciones posteriores son mucho más rápidas.</span><span class="sxs-lookup"><span data-stu-id="504a9-125">Subsequent builds are much faster.</span></span>

<span data-ttu-id="504a9-126">Ejecute `dotnet run` para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="504a9-126">Run `dotnet run` to start the app.</span></span>

---

<span data-ttu-id="504a9-127">La plantilla de proyecto crea una aplicación de ASP.NET Core y una aplicación de reaccionar.</span><span class="sxs-lookup"><span data-stu-id="504a9-127">The project template creates an ASP.NET Core app and a React app.</span></span> <span data-ttu-id="504a9-128">La aplicación de ASP.NET Core está pensada para usarse para el acceso a datos, la autorización y otros problemas relativos a servidor.</span><span class="sxs-lookup"><span data-stu-id="504a9-128">The ASP.NET Core app is intended to be used for data access, authorization, and other server-side concerns.</span></span> <span data-ttu-id="504a9-129">La aplicación reaccionar, que reside en el *ClientApp* subdirectorio, está diseñada para utilizarse para todos los problemas de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="504a9-129">The React app, residing in the *ClientApp* subdirectory, is intended to be used for all UI concerns.</span></span>

## <a name="add-pages-images-styles-modules-etc"></a><span data-ttu-id="504a9-130">Agregar páginas, imágenes, estilos, módulos, etcetera.</span><span class="sxs-lookup"><span data-stu-id="504a9-130">Add pages, images, styles, modules, etc.</span></span>

<span data-ttu-id="504a9-131">El *ClientApp* directorio es una aplicación de reaccionar CRA estándar.</span><span class="sxs-lookup"><span data-stu-id="504a9-131">The *ClientApp* directory is a standard CRA React app.</span></span> <span data-ttu-id="504a9-132">Vea el oficial [documentación CRA](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="504a9-132">See the official [CRA documentation](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) for more information.</span></span>

<span data-ttu-id="504a9-133">Existen pequeñas diferencias entre la aplicación de reaccionar creados por esta plantilla y creado por CRA sí; Sin embargo, no se modifican las capacidades de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="504a9-133">There are slight differences between the React app created by this template and the one created by CRA itself; however, the app's capabilities are unchanged.</span></span> <span data-ttu-id="504a9-134">La aplicación creada por la plantilla contiene un [arranque](https://getbootstrap.com/)-según el diseño y ver un ejemplo de enrutamiento básico.</span><span class="sxs-lookup"><span data-stu-id="504a9-134">The app created by the template contains a [Bootstrap](https://getbootstrap.com/)-based layout and a basic routing example.</span></span>

## <a name="install-npm-packages"></a><span data-ttu-id="504a9-135">Instalar paquetes de npm</span><span class="sxs-lookup"><span data-stu-id="504a9-135">Install npm packages</span></span>

<span data-ttu-id="504a9-136">Para instalar paquetes de npm de otro fabricante, utilice un símbolo del sistema en el *ClientApp* subdirectorio.</span><span class="sxs-lookup"><span data-stu-id="504a9-136">To install third-party npm packages, use a command prompt in the *ClientApp* subdirectory.</span></span> <span data-ttu-id="504a9-137">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="504a9-137">For example:</span></span>

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a><span data-ttu-id="504a9-138">Publicación e implementación</span><span class="sxs-lookup"><span data-stu-id="504a9-138">Publish and deploy</span></span>

<span data-ttu-id="504a9-139">En el desarrollo, la aplicación se ejecuta en modo optimizado para comodidad del desarrollador.</span><span class="sxs-lookup"><span data-stu-id="504a9-139">In development, the app runs in a mode optimized for developer convenience.</span></span> <span data-ttu-id="504a9-140">Por ejemplo, agrupaciones de JavaScript contienen asignaciones de origen (de modo que durante la depuración, puede ver el código fuente original).</span><span class="sxs-lookup"><span data-stu-id="504a9-140">For example, JavaScript bundles include source maps (so that when debugging, you can see your original source code).</span></span> <span data-ttu-id="504a9-141">La aplicación supervisa JavaScript, HTML y CSS cambios de archivo en el disco y vuelve a compilar automáticamente y vuelve a cargar cuando ve cambiar esos archivos.</span><span class="sxs-lookup"><span data-stu-id="504a9-141">The app watches JavaScript, HTML, and CSS file changes on disk and automatically recompiles and reloads when it sees those files change.</span></span>

<span data-ttu-id="504a9-142">En producción, servir una versión de la aplicación que está optimizada para el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="504a9-142">In production, serve a version of your app that is optimized for performance.</span></span> <span data-ttu-id="504a9-143">Esto se configura para que se producen automáticamente.</span><span class="sxs-lookup"><span data-stu-id="504a9-143">This is configured to happen automatically.</span></span> <span data-ttu-id="504a9-144">Cuando se publica, la configuración de compilación emite una compilación transpiled reducida, del código de cliente.</span><span class="sxs-lookup"><span data-stu-id="504a9-144">When you publish, the build configuration emits a minified, transpiled build of your client-side code.</span></span> <span data-ttu-id="504a9-145">A diferencia de la compilación de desarrollo, la compilación de producción no requiere Node.js esté instalado en el servidor.</span><span class="sxs-lookup"><span data-stu-id="504a9-145">Unlike the development build, the production build doesn't require Node.js to be installed on the server.</span></span>

<span data-ttu-id="504a9-146">Puede usar estándar [métodos de implementación y hospedaje de ASP.NET Core](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="504a9-146">You can use standard [ASP.NET Core hosting and deployment methods](xref:host-and-deploy/index).</span></span>

## <a name="run-the-cra-server-independently"></a><span data-ttu-id="504a9-147">Ejecutar el servidor CRA independientemente</span><span class="sxs-lookup"><span data-stu-id="504a9-147">Run the CRA server independently</span></span>

<span data-ttu-id="504a9-148">El proyecto está configurado para iniciar su propia instancia del servidor de desarrollo CRA en segundo plano cuando la aplicación ASP.NET Core se inicia en modo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="504a9-148">The project is configured to start its own instance of the CRA development server in the background when the ASP.NET Core app starts in development mode.</span></span> <span data-ttu-id="504a9-149">Esto resulta útil porque significa que no tiene que ejecutar manualmente un servidor independiente.</span><span class="sxs-lookup"><span data-stu-id="504a9-149">This is convenient because it means you don't have to run a separate server manually.</span></span>

<span data-ttu-id="504a9-150">Hay un inconveniente de este programa de instalación de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="504a9-150">There is a drawback to this default setup.</span></span> <span data-ttu-id="504a9-151">Cada vez que modifique el código de C# y su aplicación necesita que se reinicie, de ASP.NET Core se reinicia el servidor CRA.</span><span class="sxs-lookup"><span data-stu-id="504a9-151">Each time you modify your C# code and your ASP.NET Core app needs to restart, the CRA server restarts.</span></span> <span data-ttu-id="504a9-152">Unos pocos segundos son necesarios para iniciar copia de seguridad.</span><span class="sxs-lookup"><span data-stu-id="504a9-152">A few seconds are required to start back up.</span></span> <span data-ttu-id="504a9-153">Si está realizando frecuentes modificaciones del código de C# y no desea esperar a que se reinicie el servidor CRA, ejecute el servidor CRA externamente, independientemente del proceso de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="504a9-153">If you're making frequent C# code edits and don't want to wait for the CRA server to restart, run the CRA server externally, independently of the ASP.NET Core process.</span></span> <span data-ttu-id="504a9-154">Para ello:</span><span class="sxs-lookup"><span data-stu-id="504a9-154">To do so:</span></span>

1. <span data-ttu-id="504a9-155">En un símbolo del sistema, cambie a la *ClientApp* subdirectorio e inicie el servidor de desarrollo de CRA:</span><span class="sxs-lookup"><span data-stu-id="504a9-155">In a command prompt, switch to the *ClientApp* subdirectory, and launch the CRA development server:</span></span>

    ```console
    cd ClientApp
    npm start
    ```

2. <span data-ttu-id="504a9-156">Modificar la aplicación de ASP.NET Core para usar la instancia del servidor CRA externa en lugar de iniciar una propia.</span><span class="sxs-lookup"><span data-stu-id="504a9-156">Modify your ASP.NET Core app to use the external CRA server instance instead of launching one of its own.</span></span> <span data-ttu-id="504a9-157">En su *inicio* clase, reemplace el `spa.UseReactDevelopmentServer` invocación con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="504a9-157">In your *Startup* class, replace the `spa.UseReactDevelopmentServer` invocation with the following:</span></span>

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

<span data-ttu-id="504a9-158">Al iniciar la aplicación de ASP.NET Core, no ejecutar un servidor CRA.</span><span class="sxs-lookup"><span data-stu-id="504a9-158">When you start your ASP.NET Core app, it won't launch a CRA server.</span></span> <span data-ttu-id="504a9-159">La instancia que iniciarse de forma manual se usa en su lugar.</span><span class="sxs-lookup"><span data-stu-id="504a9-159">The instance you started manually is used instead.</span></span> <span data-ttu-id="504a9-160">Esto le permite iniciar y reiniciar con mayor rapidez.</span><span class="sxs-lookup"><span data-stu-id="504a9-160">This enables it to start and restart faster.</span></span> <span data-ttu-id="504a9-161">Ya no está esperando la aplicación reaccionar a generar cada vez.</span><span class="sxs-lookup"><span data-stu-id="504a9-161">It's no longer waiting for your React app to rebuild each time.</span></span>
