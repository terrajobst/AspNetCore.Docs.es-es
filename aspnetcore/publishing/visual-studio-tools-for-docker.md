---
title: Visual Studio Tools para Docker con ASP.NET Core
description: "En este artículo se describe cómo usar herramientas de Visual Studio de 2017 y Docker para Windows para incluir en un contenedor una aplicación de ASP.NET Core."
keywords: Docker,ASP.NET Core,Visual Studio,contenedor
author: spboyer
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.prod: asp.net-core
ms.technology: aspnet
ms.assetid: 1f3b9a68-4dea-4b60-8cb3-f46164eedbbf
uid: publishing/vs-tools-for-docker
ms.openlocfilehash: 2d8e337141ae4e0d0258f1d7546510b0ab077e39
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="visual-studio-tools-for-docker"></a><span data-ttu-id="995e3-104">Visual Studio Tools para Docker</span><span class="sxs-lookup"><span data-stu-id="995e3-104">Visual Studio Tools for Docker</span></span>

<span data-ttu-id="995e3-105">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) con [Docker para Windows](https://docs.docker.com/docker-for-windows/install/) admite la compilación, depuración y ejecución de aplicaciones de consola y web de .NET Framework y .NET Core con contenedores de Windows y Linux.</span><span class="sxs-lookup"><span data-stu-id="995e3-105">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) with [Docker for Windows](https://docs.docker.com/docker-for-windows/install/) supports building, debugging, and running .NET Framework and .NET Core web and console applications using Windows and Linux containers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="995e3-106">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="995e3-106">Prerequisites</span></span>

- <span data-ttu-id="995e3-107">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) con la carga de trabajo de .NET Core</span><span class="sxs-lookup"><span data-stu-id="995e3-107">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) with .NET Core workload</span></span>
- [<span data-ttu-id="995e3-108">Docker para Windows</span><span class="sxs-lookup"><span data-stu-id="995e3-108">Docker for Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a><span data-ttu-id="995e3-109">Instalación y configuración</span><span class="sxs-lookup"><span data-stu-id="995e3-109">Installation and setup</span></span>

<span data-ttu-id="995e3-110">Instale [Microsoft Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) con la carga de trabajo de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="995e3-110">Install [Microsoft Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) with the .NET Core workload.</span></span> <span data-ttu-id="995e3-111">Si ya tiene instalado Visual Studio, puede [modificar la instalación de Visual Studio](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) para agregar la carga de trabajo de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="995e3-111">If you already have Visual Studio installed, you can [modify your Visual Studio installation](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) to add the .NET Core workload.</span></span>

<span data-ttu-id="995e3-112">Para la instalación de Docker, revise la información de [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) (Docker para Windows: Información antes de realizar la instalación) e instale [Docker para Windows](https://docs.docker.com/docker-for-windows/install/).</span><span class="sxs-lookup"><span data-stu-id="995e3-112">For Docker installation, review the information at [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) and install [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).</span></span>

<span data-ttu-id="995e3-113">Una configuración necesaria es configurar **[unidades compartidas](https://docs.docker.com/docker-for-windows/#shared-drives)** en Docker para Windows.</span><span class="sxs-lookup"><span data-stu-id="995e3-113">A required configuration is to setup **[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** in Docker for Windows.</span></span> <span data-ttu-id="995e3-114">La configuración es necesaria para la compatibilidad de asignación y depuración de los volúmenes.</span><span class="sxs-lookup"><span data-stu-id="995e3-114">The setting is required for the volume mapping and debugging support.</span></span>

<span data-ttu-id="995e3-115">Haga clic con el botón derecho en el icono de Docker en la bandeja del sistema, haga clic en **Configuración** y seleccione **Shared Drives** (Unidades compartidas).</span><span class="sxs-lookup"><span data-stu-id="995e3-115">Right-click the Docker icon in the System Tray, click **Settings**, and select **Shared Drives**.</span></span> <span data-ttu-id="995e3-116">Seleccione la unidad donde Docker almacenará los archivos y aplique los cambios.</span><span class="sxs-lookup"><span data-stu-id="995e3-116">Select the drive where Docker will store your files and apply changes.</span></span>

![Unidades compartidas](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

## <a name="create-an-aspnet-web-application-and-add-docker-support"></a><span data-ttu-id="995e3-118">Creación de una aplicación web ASP.NET y adición de la compatibilidad con Docker</span><span class="sxs-lookup"><span data-stu-id="995e3-118">Create an ASP.NET Web Application and add Docker Support</span></span>

<span data-ttu-id="995e3-119">Con Visual Studio, cree una nueva aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="995e3-119">Using Visual Studio, create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="995e3-120">Cuando se cargue la aplicación, seleccione **Add Docker Support** (Agregar compatibilidad con Docker) desde el **menú de proyecto**, o bien haga clic con el botón derecho en el proyecto desde el Explorador de soluciones y seleccione **Agregar** > **Compatibilidad con Docker**.</span><span class="sxs-lookup"><span data-stu-id="995e3-120">When the application is loaded, either select **Add Docker Support** from the **Project Menu** or right-click the project from the Solution Explorer and select **Add** > **Docker Support**.</span></span>

<span data-ttu-id="995e3-121">*Menú de proyecto*</span><span class="sxs-lookup"><span data-stu-id="995e3-121">*Project Menu*</span></span>

![Agregar compatibilidad con Docker del proyecto](./visual-studio-tools-for-docker/_static/project-add-docker-support.png)

<span data-ttu-id="995e3-123">*Menú contextual del proyecto*</span><span class="sxs-lookup"><span data-stu-id="995e3-123">*Project Context Menu*</span></span>

![Hacer clic con el botón derecho en Agregar compatibilidad con Docker](./visual-studio-tools-for-docker/_static/right-click-add-docker-support.png)

<span data-ttu-id="995e3-125">Cuando agregue compatibilidad con Docker al proyecto, puede elegir contenedores Windows o Linux.</span><span class="sxs-lookup"><span data-stu-id="995e3-125">When you add Docker support to your project, you can choose either Windows or Linux containers.</span></span> <span data-ttu-id="995e3-126">(El host de Docker debe ejecutar el mismo tipo de contenedor.</span><span class="sxs-lookup"><span data-stu-id="995e3-126">(The Docker host must be running the same container type.</span></span> <span data-ttu-id="995e3-127">Si necesita cambiar el tipo de contenedor en la instancia de Docker en ejecución, haga clic con el botón derecho en el icono de **Docker** en la bandeja del sistema y elija**Switch to Windows containers** (Cambiar a contenedores Windows) o **Switch to Linux containers** (Cambiar a contenedores Linux)).</span><span class="sxs-lookup"><span data-stu-id="995e3-127">If you need change the container type in the running Docker instance, right-click the **Docker** icon in the System Tray, and choose **Switch to Windows containers** or **Switch to Linux containers**.)</span></span> 

<span data-ttu-id="995e3-128">También se agregan al proyecto los archivos siguientes:</span><span class="sxs-lookup"><span data-stu-id="995e3-128">The following files are added to the project:</span></span>

- <span data-ttu-id="995e3-129">**Dockerfile**: el archivo de Docker para las aplicaciones de ASP.NET Core se basa en la imagen [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore).</span><span class="sxs-lookup"><span data-stu-id="995e3-129">**Dockerfile**: the Docker file for ASP.NET Core applications is based on the [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) image.</span></span> <span data-ttu-id="995e3-130">Esta imagen incluye los paquetes NuGet de ASP.NET Core que se hayan precompilado, con lo que se mejora el rendimiento en el inicio.</span><span class="sxs-lookup"><span data-stu-id="995e3-130">This image includes the ASP.NET Core NuGet packages, which have been pre-jitted improving startup performance.</span></span> <span data-ttu-id="995e3-131">Al compilar aplicaciones de consola de .NET Core, el archivo Dockerfile de origen hará referencia a la última imagen [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet).</span><span class="sxs-lookup"><span data-stu-id="995e3-131">When building .NET Core Console Applications, the Dockerfile FROM will reference the most recent [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet) image.</span></span>   
- <span data-ttu-id="995e3-132">**docker-compose.yml**: archivo de Docker Compose de base utilizado para definir la colección de imágenes que se van a compilar y ejecutar con docker-compose build/run.</span><span class="sxs-lookup"><span data-stu-id="995e3-132">**docker-compose.yml**: base Docker Compose file used to define the collection of images to be built and run with docker-compose build/run.</span></span>   
- <span data-ttu-id="995e3-133">**docker-compose.dev.debug.yml**: archivo docker-compose adicional para cambios iterativos cuando se establece la configuración en depuración.</span><span class="sxs-lookup"><span data-stu-id="995e3-133">**docker-compose.dev.debug.yml**: additional docker-compose file with for iterative changes when your configuration is set to debug.</span></span> <span data-ttu-id="995e3-134">Visual Studio llamará a -f docker-compose.yml -f docker-compose.dev.debug.yml para combinarlos.</span><span class="sxs-lookup"><span data-stu-id="995e3-134">Visual Studio will call -f docker-compose.yml -f docker-compose.dev.debug.yml to merge these together.</span></span> <span data-ttu-id="995e3-135">Las herramientas de desarrollo de Visual Studio usarán este archivo compuesto.</span><span class="sxs-lookup"><span data-stu-id="995e3-135">This compose file is used by Visual Studio development tools.</span></span>   
- <span data-ttu-id="995e3-136">**docker-compose.dev.release.yml**: archivo de Docker Compose adicional para depurar la definición de versión.</span><span class="sxs-lookup"><span data-stu-id="995e3-136">**docker-compose.dev.release.yml**: additional Docker Compose file to debug your release definition.</span></span> <span data-ttu-id="995e3-137">Montará el volumen del depurador, por lo que no cambia el contenido de la imagen de producción.</span><span class="sxs-lookup"><span data-stu-id="995e3-137">It will volume mount the debugger so it doesn't change the contents of the production image.</span></span>  

<span data-ttu-id="995e3-138">El archivo *docker-compose.yml* contiene el nombre de la imagen que se crea al ejecutar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="995e3-138">The *docker-compose.yml* file contains the name of the image that is created when project is run.</span></span> 

```
version '2'

services:
  hellodockertools:
    image:  user/hellodockertools${TAG}
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "80"
``` 

<span data-ttu-id="995e3-139">En este ejemplo, `image: user/hellodockertools${TAG}` genera la imagen `user/hellodockertools:dev` cuando se ejecuta la aplicación en modo **Depuración** y `user/hellodockertools:latest` en modo **Lanzamiento**, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="995e3-139">In this example, `image: user/hellodockertools${TAG}` generates the image `user/hellodockertools:dev` when the application is run in **Debug** mode and `user/hellodockertools:latest` in **Release** mode respectively.</span></span> 

<span data-ttu-id="995e3-140">Tal vez le interese cambiar el `user` a su nombre de usuario de [Docker Hub](https://hub.docker.com/) si va a insertar la imagen en el Registro.</span><span class="sxs-lookup"><span data-stu-id="995e3-140">You will want to change the `user` to your [Docker Hub](https://hub.docker.com/) username if you plan to push the image to the registry.</span></span> <span data-ttu-id="995e3-141">Por ejemplo, `spboyer/hellodockertools`, o la dirección URL del Registro privada `privateregistry.domain.com/`, en función de la configuración.</span><span class="sxs-lookup"><span data-stu-id="995e3-141">For example, `spboyer/hellodockertools`, or change to your private registry URL `privateregistry.domain.com/` depending on your configuration.</span></span>

### <a name="debugging"></a><span data-ttu-id="995e3-142">Depuración</span><span class="sxs-lookup"><span data-stu-id="995e3-142">Debugging</span></span>

<span data-ttu-id="995e3-143">Seleccione **Docker** en la lista desplegable de depuración en la barra de herramientas y use F5 para empezar a depurar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="995e3-143">Select **Docker** from the debug drop-down in the toolbar and use F5 to start debugging the application.</span></span> 

- <span data-ttu-id="995e3-144">Se adquiere la imagen *microsoft/aspnetcore* (si todavía no está en la memoria caché).</span><span class="sxs-lookup"><span data-stu-id="995e3-144">The *microsoft/aspnetcore* image is acquired (if not already in your cache)</span></span>
- <span data-ttu-id="995e3-145">*ASPNETCORE_ENVIRONMENT* se establece en Desarrollo dentro del contenedor.</span><span class="sxs-lookup"><span data-stu-id="995e3-145">*ASPNETCORE_ENVIRONMENT* is set to Development within the container</span></span>
- <span data-ttu-id="995e3-146">Se EXPONE el PUERTO 80 y se asigna a un puerto dinámicamente asignado para el host local.</span><span class="sxs-lookup"><span data-stu-id="995e3-146">PORT 80 is EXPOSED and mapped to a dynamically assigned port for localhost.</span></span> <span data-ttu-id="995e3-147">El puerto viene determinado por el host de docker y se puede consultar con docker ps.</span><span class="sxs-lookup"><span data-stu-id="995e3-147">The port is determined by the docker host and can be queried with docker ps.</span></span> 
- <span data-ttu-id="995e3-148">La aplicación se copia en el contenedor.</span><span class="sxs-lookup"><span data-stu-id="995e3-148">Your application is copied to the container</span></span>
- <span data-ttu-id="995e3-149">Se inicia el explorador predeterminado con el depurador asociado al contenedor, utilizando el puerto asignado dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="995e3-149">Default browser is launched with the debugger attached to the container, using the dynamically assigned port.</span></span> 

<span data-ttu-id="995e3-150">La imagen de Docker resultante compilada es la imagen *dev* de la aplicación con las imágenes *microsoft/aspnetcore* como la imagen base.</span><span class="sxs-lookup"><span data-stu-id="995e3-150">The resulting Docker image built is the *dev* image of your application with the *microsoft/aspnetcore* images as the base image.</span></span>

<span data-ttu-id="995e3-151">**Nota:** La imagen de desarrollo está vacía de contenido de aplicación, ya que las configuraciones de depuración usan el montaje del volumen para proporcionar la experiencia iterativa.</span><span class="sxs-lookup"><span data-stu-id="995e3-151">**Note:** The dev image is empty of your app contents as Debug configurations use volume mounting to provide the iterative experience.</span></span> <span data-ttu-id="995e3-152">Para insertar una imagen, use la configuración de lanzamiento.</span><span class="sxs-lookup"><span data-stu-id="995e3-152">To push an image, use the Release configuration.</span></span>

```console
REPOSITORY                  TAG         IMAGE ID            CREATED         SIZE
spboyer/hellodockertools    dev         0b6e2e44b3df        4 minutes ago   268.9 MB
microsoft/aspnetcore        1.0.1       189ad4312ce7        5 days ago      268.9 MB
```

<span data-ttu-id="995e3-153">La aplicación se ejecuta con el contenedor. Puede verlo si ejecuta el comando `docker ps`.</span><span class="sxs-lookup"><span data-stu-id="995e3-153">The application is running using the container, which you can see by running the `docker ps` command.</span></span>

```console
CONTAINER ID        IMAGE                          COMMAND               CREATED             STATUS              PORTS                   NAMES
3f240cf686c9        spboyer/hellodockertools:dev   "tail -f /dev/null"   4 minutes ago       Up 4 minutes        0.0.0.0:32769->80/tcp   hellodockertools_hellodockertools_1
```

### <a name="edit-and-continue"></a><span data-ttu-id="995e3-154">Editar y continuar</span><span class="sxs-lookup"><span data-stu-id="995e3-154">Edit and Continue</span></span>

<span data-ttu-id="995e3-155">Los cambios en los archivos estáticos o archivos de plantilla Razor (*.cshtml*) se actualizan automáticamente sin necesidad de un paso de compilación.</span><span class="sxs-lookup"><span data-stu-id="995e3-155">Changes to static files and/or razor template files (*.cshtml*) are automatically updated without the need of a compilation step.</span></span> <span data-ttu-id="995e3-156">Realice el cambio, guarde y pulse Actualizar en el explorador para ver la actualización.</span><span class="sxs-lookup"><span data-stu-id="995e3-156">Make the change, save, and tap refresh in the browser to view the update.</span></span>  

<span data-ttu-id="995e3-157">Las modificaciones en los archivos de código requieren compilación y un reinicio del Kestrel dentro del contenedor.</span><span class="sxs-lookup"><span data-stu-id="995e3-157">Modifications to code files require compiling and a restart of Kestrel within the container.</span></span> <span data-ttu-id="995e3-158">Después de realizar la modificación, use CTRL + F5 para realizar el proceso e iniciar la aplicación dentro del contenedor.</span><span class="sxs-lookup"><span data-stu-id="995e3-158">After making the change, use CTRL + F5 to perform the process and start the application within the container.</span></span> <span data-ttu-id="995e3-159">El contenedor de Docker no se vuelve a compilar ni se detiene. Si usa `docker ps` en la línea de comandos, puede ver que el contenedor original todavía se está ejecutando desde hace 10 minutos.</span><span class="sxs-lookup"><span data-stu-id="995e3-159">The Docker container is not rebuilt or stopped; using `docker ps` in the command line, you can see that the original container is still running as of 10 minutes ago.</span></span> 

```console
CONTAINER ID        IMAGE                          COMMAND               CREATED             STATUS              PORTS                   NAMES
3f240cf686c9        spboyer/hellodockertools:dev   "tail -f /dev/null"   10 minutes ago      Up 10 minutes       0.0.0.0:32769->80/tcp   hellodockertools_hellodockertools_1
```

### <a name="publishing-docker-images"></a><span data-ttu-id="995e3-160">Publicación de imágenes de Docker</span><span class="sxs-lookup"><span data-stu-id="995e3-160">Publishing Docker images</span></span>

<span data-ttu-id="995e3-161">Una vez completado el ciclo de desarrollo y depuración de la aplicación, Visual Studio Tools para Docker le ayudará a crear la imagen de producción de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="995e3-161">Once you have completed the develop and debug cycle of your application, the Visual Studio Tools for Docker will help you create the production image of your application.</span></span> <span data-ttu-id="995e3-162">Cambie la lista desplegable de depuración a **Lanzamiento** y compile la aplicación.</span><span class="sxs-lookup"><span data-stu-id="995e3-162">Change the debug dropdown to **Release** and build the application.</span></span> <span data-ttu-id="995e3-163">Las herramientas producirán la imagen con la etiqueta `:latest` que puede insertar en el registro privado o en Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="995e3-163">The tooling will produce the image with the `:latest` tag which you can push to your private registry or Docker Hub.</span></span> 

<span data-ttu-id="995e3-164">Mediante el comando `docker images`, puede ver la lista de imágenes.</span><span class="sxs-lookup"><span data-stu-id="995e3-164">Using the `docker images` command, you can see the list of images.</span></span>

```console
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
spboyer/hellodockertools   latest              8184ae38ba91        5 seconds ago       278.4 MB
spboyer/hellodockertools   dev                 0b6e2e44b3df        About an hour ago   268.9 MB
microsoft/aspnetcore       1.0.1               189ad4312ce7        5 days ago          268.9 MB
```

<span data-ttu-id="995e3-165">Cabe esperar que la imagen de producción o lanzamiento sea de menor tamaño en comparación con la imagen de **desarrollo**. A pesar de ello, mediante el uso de la asignación de volumen, el depurador y la aplicación se estaban ejecutando realmente desde la máquina local, y no dentro del contenedor.</span><span class="sxs-lookup"><span data-stu-id="995e3-165">There may be an expectation for the production or release image to be smaller in size by comparison to the **dev** image; however, through the use of the volume mapping, the debugger and application were actually being run from your local machine and not within the container.</span></span> <span data-ttu-id="995e3-166">La **última** imagen ha empaquetado el código de aplicación completo necesario para ejecutar la aplicación en una máquina host, por lo tanto, la diferencia es el tamaño del código de aplicación.</span><span class="sxs-lookup"><span data-stu-id="995e3-166">The **latest** image has packaged the entire application code needed to run the application on a host machine, therefore the delta is the size of your application code.</span></span>
