---
title: Visual Studio Tools para Docker con ASP.NET Core
author: spboyer
description: "Obtenga información sobre cómo usar las herramientas de Visual Studio 2017 y Docker para Windows para incluir en un contenedor una aplicación de ASP.NET Core."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: b2a3c369a22d50fcefdb96914f5bf84bfafab7cb
ms.sourcegitcommit: 6fa546140575b3eb279eabae12d9acad966f70e0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2018
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a><span data-ttu-id="44166-103">Visual Studio Tools para Docker con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="44166-103">Visual Studio Tools for Docker with ASP.NET Core</span></span>

<span data-ttu-id="44166-104">[Visual Studio de 2017](https://www.visualstudio.com/) permite compilar, depurar y ejecutar en contenedores ASP.NET Core aplicaciones destinadas a .NET Core.</span><span class="sxs-lookup"><span data-stu-id="44166-104">[Visual Studio 2017](https://www.visualstudio.com/) supports building, debugging, and running containerized ASP.NET Core apps targeting .NET Core.</span></span> <span data-ttu-id="44166-105">Se admiten contenedores de Windows y Linux.</span><span class="sxs-lookup"><span data-stu-id="44166-105">Both Windows and Linux containers are supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="44166-106">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="44166-106">Prerequisites</span></span>

* <span data-ttu-id="44166-107">[Visual Studio 2017](https://www.visualstudio.com/) con la carga de trabajo **Desarrollo multiplataforma de .NET Core**</span><span class="sxs-lookup"><span data-stu-id="44166-107">[Visual Studio 2017](https://www.visualstudio.com/) with the **.NET Core cross-platform development** workload</span></span>
* [<span data-ttu-id="44166-108">Docker para Windows</span><span class="sxs-lookup"><span data-stu-id="44166-108">Docker for Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a><span data-ttu-id="44166-109">Instalación y configuración</span><span class="sxs-lookup"><span data-stu-id="44166-109">Installation and setup</span></span>

<span data-ttu-id="44166-110">Para la instalación de Docker, revise la información de [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) (Docker para Windows: Información antes de realizar la instalación) e instale [Docker para Windows](https://docs.docker.com/docker-for-windows/install/).</span><span class="sxs-lookup"><span data-stu-id="44166-110">For Docker installation, review the information at [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) and install [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).</span></span>

<span data-ttu-id="44166-111">Las **[unidades compartidas](https://docs.docker.com/docker-for-windows/#shared-drives)** de Docker para Windows deben configurarse para admitir la asignación y la depuración de volúmenes.</span><span class="sxs-lookup"><span data-stu-id="44166-111">**[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** in Docker for Windows must be configured to support volume mapping and debugging.</span></span> <span data-ttu-id="44166-112">Haga clic en el icono de Docker de la bandeja del sistema, seleccione **configuración...** y seleccione **unidades compartidas**.</span><span class="sxs-lookup"><span data-stu-id="44166-112">Right-click the System Tray's Docker icon, select **Settings...**, and select **Shared Drives**.</span></span> <span data-ttu-id="44166-113">Seleccione la unidad donde almacena los archivos en Docker.</span><span class="sxs-lookup"><span data-stu-id="44166-113">Select the drive where Docker stores files.</span></span> <span data-ttu-id="44166-114">Seleccione **aplicar**.</span><span class="sxs-lookup"><span data-stu-id="44166-114">Select **Apply**.</span></span>

![Unidades compartidas](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> <span data-ttu-id="44166-116">Las versiones visuales Studio de 2017 15.6 y versiones posteriores cuando solicite **unidades compartidas** no están configurados.</span><span class="sxs-lookup"><span data-stu-id="44166-116">Visual Studio 2017 versions 15.6 and later prompt when **Shared Drives** aren't configured.</span></span>

## <a name="add-docker-support-to-an-app"></a><span data-ttu-id="44166-117">Agregar compatibilidad con Docker a una aplicación</span><span class="sxs-lookup"><span data-stu-id="44166-117">Add Docker support to an app</span></span>

<span data-ttu-id="44166-118">Para agregar compatibilidad de Docker a un proyecto de ASP.NET Core, el proyecto debe tener como destino .NET Core.</span><span class="sxs-lookup"><span data-stu-id="44166-118">In order to add Docker support to an ASP.NET Core project, the project must target .NET Core.</span></span> <span data-ttu-id="44166-119">Se admiten los contenedores de Linux y Windows.</span><span class="sxs-lookup"><span data-stu-id="44166-119">Both Linux and Windows containers are supported.</span></span>

<span data-ttu-id="44166-120">Al agregar compatibilidad con Docker a un proyecto, elija Windows o un contenedor de Linux.</span><span class="sxs-lookup"><span data-stu-id="44166-120">When adding Docker support to a project, choose either a Windows or a Linux container.</span></span> <span data-ttu-id="44166-121">El host de Docker debe ejecutar el mismo tipo de contenedor.</span><span class="sxs-lookup"><span data-stu-id="44166-121">The Docker host must be running the same container type.</span></span> <span data-ttu-id="44166-122">Para cambiar el tipo de contenedor en la instancia de Docker en ejecución, haga clic con el botón derecho en el icono de Docker en la bandeja del sistema y elija **Switch to Windows containers...** (Cambiar a contenedores Windows) o **Switch to Linux containers...** (Cambiar a contenedores Linux).</span><span class="sxs-lookup"><span data-stu-id="44166-122">To change the container type in the running Docker instance, right-click the System Tray's Docker icon and choose **Switch to Windows containers...** or **Switch to Linux containers...**.</span></span>

### <a name="new-app"></a><span data-ttu-id="44166-123">Nueva aplicación</span><span class="sxs-lookup"><span data-stu-id="44166-123">New app</span></span>

<span data-ttu-id="44166-124">Al crear una nueva aplicación con las plantillas de proyecto **Aplicación web ASP.NET Core**, active la casilla **Enable Docker Support** (Habilitar compatibilidad con Docker):</span><span class="sxs-lookup"><span data-stu-id="44166-124">When creating a new app with the **ASP.NET Core Web Application** project templates, select the **Enable Docker Support** checkbox:</span></span>

![Casilla Habilitar compatibilidad con Docker](visual-studio-tools-for-docker/_static/enable-docker-support-checkbox.png)

<span data-ttu-id="44166-126">Si la plataforma de destino es .NET Core, el **OS** desplegable permite la selección de un tipo de contenedor.</span><span class="sxs-lookup"><span data-stu-id="44166-126">If the target framework is .NET Core, the **OS** drop-down allows for the selection of a container type.</span></span>

### <a name="existing-app"></a><span data-ttu-id="44166-127">Aplicación existente</span><span class="sxs-lookup"><span data-stu-id="44166-127">Existing app</span></span>

<span data-ttu-id="44166-128">Visual Studio Tools para Docker no admite la adición de Docker a un proyecto de ASP.NET Core existente para .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="44166-128">The Visual Studio Tools for Docker don't support adding Docker to an existing ASP.NET Core project targeting .NET Framework.</span></span> <span data-ttu-id="44166-129">En los proyectos de ASP.NET Core para .NET Core, hay dos opciones para agregar compatibilidad con Docker mediante las herramientas.</span><span class="sxs-lookup"><span data-stu-id="44166-129">For ASP.NET Core projects targeting .NET Core, there are two options for adding Docker support via the tooling.</span></span> <span data-ttu-id="44166-130">Abra el proyecto en Visual Studio y elija una de las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="44166-130">Open the project in Visual Studio, and choose one of the following options:</span></span>

* <span data-ttu-id="44166-131">Seleccione **Compatibilidad con Docker** en el menú **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="44166-131">Select **Docker Support** from the **Project** menu.</span></span>
* <span data-ttu-id="44166-132">Haga clic con el botón derecho en el proyecto en el Explorador de soluciones y seleccione **Agregar** > **Compatibilidad con Docker**.</span><span class="sxs-lookup"><span data-stu-id="44166-132">Right-click the project in Solution Explorer and select **Add** > **Docker Support**.</span></span>

## <a name="docker-assets-overview"></a><span data-ttu-id="44166-133">Información general de los recursos de docker</span><span class="sxs-lookup"><span data-stu-id="44166-133">Docker assets overview</span></span>

<span data-ttu-id="44166-134">Visual Studio Tools para Docker agrega un proyecto *docker-compose* a la solución, que contiene lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="44166-134">The Visual Studio Tools for Docker add a *docker-compose* project to the solution, containing the following:</span></span>

* <span data-ttu-id="44166-135">*.dockerignore*: contiene una lista de patrones de archivos y directorios que se van a excluir al generar un contexto de compilación.</span><span class="sxs-lookup"><span data-stu-id="44166-135">*.dockerignore*: Contains a list of file and directory patterns to exclude when generating a build context.</span></span>
* <span data-ttu-id="44166-136">*docker-compose.yml*: archivo base de [Docker Compose](https://docs.docker.com/compose/overview/) usado para definir la colección de imágenes que se va a compilar y ejecutar con `docker-compose build` y `docker-compose run`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="44166-136">*docker-compose.yml*: The base [Docker Compose](https://docs.docker.com/compose/overview/) file used to define the collection of images to be built and run with `docker-compose build` and `docker-compose run`, respectively.</span></span>
* <span data-ttu-id="44166-137">*docker-compose.override.yml*: archivo opcional, leído por Docker Compose, que contiene las invalidaciones de configuración de los servicios.</span><span class="sxs-lookup"><span data-stu-id="44166-137">*docker-compose.override.yml*: An optional file, read by Docker Compose, containing configuration overrides for services.</span></span> <span data-ttu-id="44166-138">Visual Studio ejecuta `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` para combinar estos archivos.</span><span class="sxs-lookup"><span data-stu-id="44166-138">Visual Studio executes `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` to merge these files.</span></span>

<span data-ttu-id="44166-139">Se agrega un *Dockerfile*, la receta para crear una imagen de Docker final, a la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="44166-139">A *Dockerfile*, the recipe for creating a final Docker image, is added to the project root.</span></span> <span data-ttu-id="44166-140">Vea [Dockerfile reference (Referencia de Dockerfile)](https://docs.docker.com/engine/reference/builder/) para obtener una descripción de los comandos que contiene.</span><span class="sxs-lookup"><span data-stu-id="44166-140">Refer to [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for an understanding of the commands within it.</span></span> <span data-ttu-id="44166-141">Este *Dockerfile* concreto usa una [compilación de varias fases](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) con cuatro fases de compilación distintas con nombre:</span><span class="sxs-lookup"><span data-stu-id="44166-141">This particular *Dockerfile* uses a [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) containing four distinct, named build stages:</span></span>

[!code-dockerfile[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

<span data-ttu-id="44166-142">El *Dockerfile* se basa en la imagen [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore).</span><span class="sxs-lookup"><span data-stu-id="44166-142">The *Dockerfile* is based on the [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) image.</span></span> <span data-ttu-id="44166-143">Esta imagen base incluye los paquetes NuGet de ASP.NET Core, que se han precompilado para mejorar el rendimiento en el inicio.</span><span class="sxs-lookup"><span data-stu-id="44166-143">This base image includes the ASP.NET Core NuGet packages, which have been pre-jitted to improve startup performance.</span></span>

<span data-ttu-id="44166-144">El *docker compose.yml* archivo contiene el nombre de la imagen que se crea cuando se ejecuta el proyecto:</span><span class="sxs-lookup"><span data-stu-id="44166-144">The *docker-compose.yml* file contains the name of the image that's created when the project runs:</span></span>

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

<span data-ttu-id="44166-145">En el ejemplo anterior, `image: hellodockertools` genera la imagen `hellodockertools:dev` cuando se ejecuta la aplicación en modo de **depuración**.</span><span class="sxs-lookup"><span data-stu-id="44166-145">In the preceding example, `image: hellodockertools` generates the image `hellodockertools:dev` when the app runs in **Debug** mode.</span></span> <span data-ttu-id="44166-146">La imagen `hellodockertools:latest` se genera cuando se ejecuta la aplicación en modo de **versión**.</span><span class="sxs-lookup"><span data-stu-id="44166-146">The `hellodockertools:latest` image is generated when the app runs in **Release** mode.</span></span>

<span data-ttu-id="44166-147">Prefijo del nombre de la imagen con el [Docker Hub](https://hub.docker.com/) nombre de usuario (por ejemplo, `dockerhubusername/hellodockertools`) si la imagen se insertará en el registro.</span><span class="sxs-lookup"><span data-stu-id="44166-147">Prefix the image name with the [Docker Hub](https://hub.docker.com/) username (for example, `dockerhubusername/hellodockertools`) if the image will be pushed to the registry.</span></span> <span data-ttu-id="44166-148">Como alternativa, cambiar el nombre de la imagen para incluir la dirección URL del registro privada (por ejemplo, `privateregistry.domain.com/hellodockertools`) según la configuración.</span><span class="sxs-lookup"><span data-stu-id="44166-148">Alternatively, change the image name to include the private registry URL (for example, `privateregistry.domain.com/hellodockertools`) depending on the configuration.</span></span>

## <a name="debug"></a><span data-ttu-id="44166-149">Depuración</span><span class="sxs-lookup"><span data-stu-id="44166-149">Debug</span></span>

<span data-ttu-id="44166-150">Seleccione **Docker** en la lista desplegable de depuración de la barra de herramientas y empiece a depurar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="44166-150">Select **Docker** from the debug drop-down in the toolbar, and start debugging the app.</span></span> <span data-ttu-id="44166-151">La vista **Docker** de la ventana **Salida** muestra las acciones siguientes en curso:</span><span class="sxs-lookup"><span data-stu-id="44166-151">The **Docker** view of the **Output** window shows the following actions taking place:</span></span>

* <span data-ttu-id="44166-152">El *microsoft/aspnetcore* se adquiere la imagen en tiempo de ejecución (si aún no se encuentra en la memoria caché).</span><span class="sxs-lookup"><span data-stu-id="44166-152">The *microsoft/aspnetcore* runtime image is acquired (if not already in the cache).</span></span>
* <span data-ttu-id="44166-153">El *microsoft/aspnetcore-build* imagen y publicación de compilación se adquiere (si aún no se encuentra en la memoria caché).</span><span class="sxs-lookup"><span data-stu-id="44166-153">The *microsoft/aspnetcore-build* compile/publish image is acquired (if not already in the cache).</span></span>
* <span data-ttu-id="44166-154">La variable de entorno *ASPNETCORE_ENVIRONMENT* se establece en `Development` dentro del contenedor.</span><span class="sxs-lookup"><span data-stu-id="44166-154">The *ASPNETCORE_ENVIRONMENT* environment variable is set to `Development` within the container.</span></span>
* <span data-ttu-id="44166-155">Se expone el puerto 80 y se asigna a un puerto asignado dinámicamente para el host local.</span><span class="sxs-lookup"><span data-stu-id="44166-155">Port 80 is exposed and mapped to a dynamically-assigned port for localhost.</span></span> <span data-ttu-id="44166-156">El puerto viene determinado por el host de Docker y se puede consultar con el comando `docker ps`.</span><span class="sxs-lookup"><span data-stu-id="44166-156">The port is determined by the Docker host and can be queried with the `docker ps` command.</span></span>
* <span data-ttu-id="44166-157">La aplicación se copia en el contenedor.</span><span class="sxs-lookup"><span data-stu-id="44166-157">The app is copied to the container.</span></span>
* <span data-ttu-id="44166-158">Se inicia el explorador predeterminado con el depurador asociado al contenedor mediante el puerto asignado dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="44166-158">The default browser is launched with the debugger attached to the container using the dynamically-assigned port.</span></span> 

<span data-ttu-id="44166-159">La imagen de Docker resultante es el *dev* imagen de la aplicación con el *microsoft/aspnetcore* imágenes como la imagen base.</span><span class="sxs-lookup"><span data-stu-id="44166-159">The resulting Docker image is the *dev* image of the app with the *microsoft/aspnetcore* images as the base image.</span></span> <span data-ttu-id="44166-160">Ejecute el comando `docker images` en la ventana **Consola del Administrador de paquetes** (PMC).</span><span class="sxs-lookup"><span data-stu-id="44166-160">Run the `docker images` command in the **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="44166-161">Se muestran las imágenes en el equipo:</span><span class="sxs-lookup"><span data-stu-id="44166-161">The images on the machine are displayed:</span></span>

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> <span data-ttu-id="44166-162">La imagen de desarrollo no tiene el contenido de la aplicación, como **depurar** configuraciones utilizan el montaje de volumen para proporcionar la experiencia iterativa.</span><span class="sxs-lookup"><span data-stu-id="44166-162">The dev image lacks the app contents, as **Debug** configurations use volume mounting to provide the iterative experience.</span></span> <span data-ttu-id="44166-163">Para insertar una imagen, use la configuración de **versión**.</span><span class="sxs-lookup"><span data-stu-id="44166-163">To push an image, use the **Release** configuration.</span></span>

<span data-ttu-id="44166-164">Ejecute el comando `docker ps` en la PMC.</span><span class="sxs-lookup"><span data-stu-id="44166-164">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="44166-165">Tenga en cuenta que la aplicación se ejecuta mediante el contenedor:</span><span class="sxs-lookup"><span data-stu-id="44166-165">Notice the app is running using the container:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a><span data-ttu-id="44166-166">Editar y continuar</span><span class="sxs-lookup"><span data-stu-id="44166-166">Edit and continue</span></span>

<span data-ttu-id="44166-167">Los cambios en archivos estáticos y vistas de Razor se actualizan automáticamente sin necesidad de ningún paso de compilación.</span><span class="sxs-lookup"><span data-stu-id="44166-167">Changes to static files and Razor views are automatically updated without the need for a compilation step.</span></span> <span data-ttu-id="44166-168">Realice el cambio, guarde y actualice el explorador para ver la actualización.</span><span class="sxs-lookup"><span data-stu-id="44166-168">Make the change, save, and refresh the browser to view the update.</span></span>  

<span data-ttu-id="44166-169">Las modificaciones en archivos de código requieren compilación y un reinicio del Kestrel dentro del contenedor.</span><span class="sxs-lookup"><span data-stu-id="44166-169">Modifications to code files requires compiling and a restart of Kestrel within the container.</span></span> <span data-ttu-id="44166-170">Después de realizar la modificación, presione CTRL + F5 para realizar el proceso e iniciar la aplicación dentro del contenedor.</span><span class="sxs-lookup"><span data-stu-id="44166-170">After making the change, use CTRL + F5 to perform the process and start the app within the container.</span></span> <span data-ttu-id="44166-171">El contenedor Docker no vuelve a generar o detenido.</span><span class="sxs-lookup"><span data-stu-id="44166-171">The Docker container isn't rebuilt or stopped.</span></span> <span data-ttu-id="44166-172">Ejecute el comando `docker ps` en la PMC.</span><span class="sxs-lookup"><span data-stu-id="44166-172">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="44166-173">Observe que el contenedor original se está ejecutando desde hace 10 minutos:</span><span class="sxs-lookup"><span data-stu-id="44166-173">Notice the original container is still running as of 10 minutes ago:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a><span data-ttu-id="44166-174">Publicar imágenes de Docker</span><span class="sxs-lookup"><span data-stu-id="44166-174">Publish Docker images</span></span>

<span data-ttu-id="44166-175">Una vez que se completa el ciclo de desarrollo y depuración de la aplicación, Visual Studio Tools para Docker ayudará a crear la imagen de producción de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="44166-175">Once the develop and debug cycle of the app is completed, the Visual Studio Tools for Docker assist in creating the production image of the app.</span></span> <span data-ttu-id="44166-176">Cambie la lista desplegable de configuración a **Versión** y compile la aplicación.</span><span class="sxs-lookup"><span data-stu-id="44166-176">Change the configuration drop-down to **Release** and build the app.</span></span> <span data-ttu-id="44166-177">Las herramientas genera la imagen con el *más reciente* etiqueta, que se puede insertar en el registro privado o en Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="44166-177">The tooling produces the image with the *latest* tag, which can be pushed to the private registry or Docker Hub.</span></span> 

<span data-ttu-id="44166-178">Ejecute el comando `docker images` en la PMC para ver la lista de imágenes:</span><span class="sxs-lookup"><span data-stu-id="44166-178">Run the `docker images` command in PMC to see the list of images:</span></span>

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> <span data-ttu-id="44166-179">El comando `docker images` devuelve imágenes de intermediario con los nombres de repositorio y las etiquetas identificados como *\<ninguno >* (no mencionado anteriormente).</span><span class="sxs-lookup"><span data-stu-id="44166-179">The `docker images` command returns intermediary images with repository names and tags identified as *\<none>* (not listed above).</span></span> <span data-ttu-id="44166-180">Estas imágenes sin nombre son creadas por el *Dockerfile* de [compilación de varias fases](https://docs.docker.com/engine/userguide/eng-image/multistage-build/).</span><span class="sxs-lookup"><span data-stu-id="44166-180">These unnamed images are produced by the [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile*.</span></span> <span data-ttu-id="44166-181">Mejoran la eficacia de la compilación de la imagen final y solo se vuelven a compilar las capas necesarias cuando se producen cambios.</span><span class="sxs-lookup"><span data-stu-id="44166-181">They improve the efficiency of building the final image&mdash;only the necessary layers are rebuilt when changes occur.</span></span> <span data-ttu-id="44166-182">Cuando ya no son necesarias las imágenes de intermediarios, eliminarlos mediante la [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) comando.</span><span class="sxs-lookup"><span data-stu-id="44166-182">When the intermediary images are no longer needed, delete them using the [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) command.</span></span>

<span data-ttu-id="44166-183">Podría esperarse que la imagen de producción o versión fuera más pequeña que la imagen *dev*.</span><span class="sxs-lookup"><span data-stu-id="44166-183">There may be an expectation for the production or release image to be smaller in size by comparison to the *dev* image.</span></span> <span data-ttu-id="44166-184">Debido a la asignación de volumen, el depurador y la aplicación se ejecutan desde el equipo local y no dentro del contenedor.</span><span class="sxs-lookup"><span data-stu-id="44166-184">Because of the volume mapping, the debugger and app were running from the local machine and not within the container.</span></span> <span data-ttu-id="44166-185">La imagen *más reciente* ha empaquetado el código de aplicación necesario para ejecutar la aplicación en un equipo host.</span><span class="sxs-lookup"><span data-stu-id="44166-185">The *latest* image has packaged the necessary app code to run the app on a host machine.</span></span> <span data-ttu-id="44166-186">Por lo tanto, la diferencia es el tamaño del código de aplicación.</span><span class="sxs-lookup"><span data-stu-id="44166-186">Therefore, the delta is the size of the app code.</span></span>
