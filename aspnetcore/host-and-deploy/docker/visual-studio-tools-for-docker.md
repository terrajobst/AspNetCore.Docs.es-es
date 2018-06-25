---
title: Visual Studio Tools para Docker con ASP.NET Core
author: spboyer
description: Obtenga información sobre cómo usar las herramientas de Visual Studio 2017 y Docker para Windows para incluir en un contenedor una aplicación de ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/12/2017
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: fd485416ff0fab2508ab8ffd3f0ad309be338723
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276859"
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a>Visual Studio Tools para Docker con ASP.NET Core

[Visual Studio 2017](https://www.visualstudio.com/) permite compilar, depurar y ejecutar aplicaciones ASP.NET Core en contenedor destinadas a .NET Core. Se admiten contenedores de Windows y Linux.

## <a name="prerequisites"></a>Requisitos previos

* [Visual Studio 2017](https://www.visualstudio.com/) con la carga de trabajo **Desarrollo multiplataforma de .NET Core**
* [Docker para Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a>Instalación y configuración

Para la instalación de Docker, revise la información de [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) (Docker para Windows: Información antes de realizar la instalación) e instale [Docker para Windows](https://docs.docker.com/docker-for-windows/install/).

Las **[unidades compartidas](https://docs.docker.com/docker-for-windows/#shared-drives)** de Docker para Windows deben configurarse para admitir la asignación y la depuración de volúmenes. Haga clic con el botón derecho en el icono de Docker en la bandeja del sistema, haga clic en **Configuración...** y seleccione **Unidades compartidas**. Seleccione la unidad donde los archivos se almacenan en Docker. Seleccione **Aplicar**.

![Unidades compartidas](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> Las versiones 15.6 y posteriores de Visual Studio 2017 le avisan si las **unidades compartidas** no están configuradas.

## <a name="add-docker-support-to-an-app"></a>Agregar compatibilidad con Docker a una aplicación

Para agregar compatibilidad con Docker a un proyecto de ASP.NET Core, el proyecto debe tener como destino .NET Core. Se admiten contenedores de Linux y Windows.

Al agregar compatibilidad con Docker a un proyecto, elija un contenedor de Linux o Windows. El host de Docker debe ejecutar el mismo tipo de contenedor. Para cambiar el tipo de contenedor en la instancia de Docker en ejecución, haga clic con el botón derecho en el icono de Docker en la bandeja del sistema y elija **Switch to Windows containers...** (Cambiar a contenedores Windows) o **Switch to Linux containers...** (Cambiar a contenedores Linux).

### <a name="new-app"></a>Nueva aplicación

Al crear una nueva aplicación con las plantillas de proyecto **Aplicación web ASP.NET Core**, active la casilla **Enable Docker Support** (Habilitar compatibilidad con Docker):

![Casilla Habilitar compatibilidad con Docker](visual-studio-tools-for-docker/_static/enable-docker-support-checkbox.png)

Si la plataforma de destino es .NET Core, la lista desplegable de **SO** permite la selección de un tipo de contenedor.

### <a name="existing-app"></a>Aplicación existente

Visual Studio Tools para Docker no admite la adición de Docker a un proyecto de ASP.NET Core existente para .NET Framework. En los proyectos de ASP.NET Core para .NET Core, hay dos opciones para agregar compatibilidad con Docker mediante las herramientas. Abra el proyecto en Visual Studio y elija una de las siguientes opciones:

* Seleccione **Compatibilidad con Docker** en el menú **Proyecto**.
* Haga clic con el botón derecho en el proyecto en el Explorador de soluciones y seleccione **Agregar** > **Compatibilidad con Docker**.

## <a name="docker-assets-overview"></a>Información general de los recursos de docker

Visual Studio Tools para Docker agrega un proyecto *docker-compose* a la solución, que contiene lo siguiente:

* *.dockerignore*: contiene una lista de patrones de archivos y directorios que se van a excluir al generar un contexto de compilación.
* *docker-compose.yml*: archivo base de [Docker Compose](https://docs.docker.com/compose/overview/) usado para definir la colección de imágenes que se va a compilar y ejecutar con `docker-compose build` y `docker-compose run`, respectivamente.
* *docker-compose.override.yml*: archivo opcional, leído por Docker Compose, que contiene las invalidaciones de configuración de los servicios. Visual Studio ejecuta `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` para combinar estos archivos.

Se agrega un *Dockerfile*, la receta para crear una imagen de Docker final, a la raíz del proyecto. Vea [Dockerfile reference (Referencia de Dockerfile)](https://docs.docker.com/engine/reference/builder/) para obtener una descripción de los comandos que contiene. Este *Dockerfile* concreto usa una [compilación de varias fases](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) con cuatro fases de compilación distintas con nombre:

[!code-dockerfile[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

El *Dockerfile* se basa en la imagen [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore). Esta imagen base incluye los paquetes NuGet de ASP.NET Core, que se han precompilado para mejorar el rendimiento en el inicio.

El archivo *docker-compose.yml* contiene el nombre de la imagen que se crea al ejecutar el proyecto:

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

En el ejemplo anterior, `image: hellodockertools` genera la imagen `hellodockertools:dev` cuando se ejecuta la aplicación en modo de **depuración**. La imagen `hellodockertools:latest` se genera cuando se ejecuta la aplicación en modo de **versión**.

Si planea colocar la imagen en el Registro, anteponga al nombre de imagen el nombre de usuario de [Docker Hub](https://hub.docker.com/) (por ejemplo, `dockerhubusername/hellodockertools`). También puede cambiar el nombre de la imagen para incluir la dirección URL del Registro privado (por ejemplo, `privateregistry.domain.com/hellodockertools`) según la configuración.

## <a name="debug"></a>Depuración

Seleccione **Docker** en la lista desplegable de depuración de la barra de herramientas y empiece a depurar la aplicación. La vista **Docker** de la ventana **Salida** muestra las acciones siguientes en curso:

* Se adquiere la imagen *microsoft/aspnetcore* en tiempo de ejecución (si todavía no está en la caché).
* Se adquiere la imagen de compilación o publicación *microsoft/aspnetcore-build* (si todavía no está en la caché).
* La variable de entorno *ASPNETCORE_ENVIRONMENT* se establece en `Development` dentro del contenedor.
* Se expone el puerto 80 y se asigna a un puerto asignado dinámicamente para el host local. El puerto viene determinado por el host de Docker y se puede consultar con el comando `docker ps`.
* La aplicación se copia en el contenedor.
* Se inicia el explorador predeterminado con el depurador asociado al contenedor, con el puerto asignado dinámicamente. 

La imagen de Docker resultante es la imagen *dev* de la aplicación, con las imágenes *microsoft/aspnetcore* como imagen base. Ejecute el comando `docker images` en la ventana **Consola del Administrador de paquetes** (PMC). Se muestran las imágenes en la máquina:

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> La imagen de desarrollo carece del contenido de la aplicación, ya que las configuraciones de **depuración** usan el montaje de volumen para proporcionar la experiencia iterativa. Para insertar una imagen, use la configuración de **versión**.

Ejecute el comando `docker ps` en la PMC. Tenga en cuenta que la aplicación se ejecuta mediante el contenedor:

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a>Editar y continuar

Los cambios en archivos estáticos y vistas de Razor se actualizan automáticamente sin necesidad de ningún paso de compilación. Realice el cambio, guarde y actualice el explorador para ver la actualización.  

Las modificaciones en archivos de código requieren compilación y un reinicio del Kestrel dentro del contenedor. Después de realizar la modificación, presione CTRL + F5 para realizar el proceso e iniciar la aplicación dentro del contenedor. El contenedor de Docker no se vuelve a compilar ni se detiene. Ejecute el comando `docker ps` en la PMC. Observe que el contenedor original se está ejecutando desde hace 10 minutos:

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a>Publicar imágenes de Docker

Una vez que se completa el ciclo de desarrollo y depuración de la aplicación, Visual Studio Tools para Docker ayuda a crear la imagen de producción de la aplicación. Cambie la lista desplegable de configuración a **Versión** y compile la aplicación. Las herramientas generan la imagen con la etiqueta *latest*, que puede insertar en el Registro privado o Docker Hub. 

Ejecute el comando `docker images` en la PMC para ver la lista de imágenes:

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> El comando `docker images` devuelve imágenes de intermediario con los nombres de repositorio y las etiquetas identificados como *\<ninguno >* (no mencionado anteriormente). Estas imágenes sin nombre son creadas por el *Dockerfile* de [compilación de varias fases](https://docs.docker.com/engine/userguide/eng-image/multistage-build/). Mejoran la eficacia de la compilación de la imagen final y solo se vuelven a compilar las capas necesarias cuando se producen cambios. Cuando las imágenes de intermediario ya no sean necesarias, elimínelas mediante el comando [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/).

Podría esperarse que la imagen de producción o versión fuera más pequeña que la imagen *dev*. Debido a la asignación de volumen, el depurador y la aplicación se han ejecutado desde la máquina local y no dentro del contenedor. La imagen *más reciente* ha empaquetado el código de aplicación necesario para ejecutar la aplicación en un equipo host. Por tanto, la diferencia es el tamaño del código de aplicación.
