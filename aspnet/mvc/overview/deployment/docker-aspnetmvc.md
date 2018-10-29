---
uid: mvc/overview/deployment/docker
title: Migrar aplicaciones de ASP.NET MVC a contenedores de Windows
description: Aprenda a ejecutar una aplicación existente de ASP.NET MVC en un contenedor de Docker de Windows
keywords: Windows Containers,Docker,ASP.NET MVC
author: BillWagner
ms.author: wiwagn
ms.date: 02/01/2017
ms.assetid: c9f1d52c-b4bd-4b5d-b7f9-8f9ceaf778c4
ms.openlocfilehash: 7b34187747d3081998b8b60a72adae78cafe2c3e
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207971"
---
# <a name="migrating-aspnet-mvc-applications-to-windows-containers"></a>Migrar aplicaciones de ASP.NET MVC a contenedores de Windows

Ejecutar una aplicación existente basada en .NET Framework en un contenedor de Windows no requiere cambios en la aplicación. Para ejecutar la aplicación en un contenedor de Windows, crea una imagen de Docker que contenga la aplicación e inicie el contenedor. En este tema, se explican cómo realizar una [aplicación ASP.NET MVC](http://www.asp.net/mvc) existente e implementarla en un contenedor de Windows.

Comience con una aplicación existente de ASP.NET MVC y luego compile los recursos publicados mediante Visual Studio. Puede usar Docker para crear la imagen que contiene y ejecuta la aplicación. Podrá ir al sitio que se ejecuta en un contenedor de Windows y comprobar que la aplicación funciona.

En este artículo, se supone un conocimiento básico de Docker. Para más información sobre Docker, consulte [Docker Overview](https://docs.docker.com/engine/understanding-docker/) (Introducción a Docker).

La aplicación que ejecutará en un contenedor es un sitio web sencillo que responde a preguntas de forma aleatoria. Esta aplicación es una aplicación MVC básica sin autenticación ni almacenamiento de base de datos; esto le permite centrarse en mover la capa web a un contenedor. En temas futuros, se le mostrará cómo mover y administrar el almacenamiento persistente en aplicaciones en contenedor.

Al mover la aplicación, se incluyen los siguientes pasos:

1. [Crear una tarea de publicación para crear los recursos de una imagen.](#publish-script)
1. [Crear una imagen de Docker que ejecutará la aplicación.](#build-the-image)
1. [Iniciar un contenedor de Docker que ejecuta la imagen.](#start-a-container)
1. [Comprobar la aplicación mediante el explorador.](#verify-in-the-browser)

La [aplicación finalizada](https://github.com/dotnet/samples/tree/master/framework/docker/MVCRandomAnswerGenerator) se encuentra en GitHub.

## <a name="prerequisites"></a>Requisitos previos

El equipo de desarrollo debe tener el siguiente software:

- [Windows 10 Anniversary Update](https://www.microsoft.com/software-download/windows10/) (o posterior) o [Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (o posterior)
- [Docker para Windows](https://docs.docker.com/docker-for-windows/): versión 1.13.0 estable o 1.12 Beta 26 (o versiones más recientes)
- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

> [!IMPORTANT]
> Si usa Windows Server 2016, siga las instrucciones de [Implementación de host de contenedor - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).

Después de instalar e iniciar Docker, haga doble clic en el icono de bandeja y seleccione **cambiar a contenedores de Windows**. Esto es necesario para ejecutar imágenes de Docker basadas en Windows. Este comando tarda algunos segundos en ejecutarse:

![Contenedor de Windows][windows-container]

## <a name="publish-script"></a>Publicar script

Reúna en un mismo lugar todos los recursos que necesita cargar en una imagen de Docker. Puede usar el comando **Publicar** de Visual Studio para crear un perfil de publicación para la aplicación. Este perfil incluirá todos los recursos en un árbol de directorio que copia en la imagen de destino más adelante en este tutorial.

**Pasos de publicación**

1. Haga clic con el botón derecho en el proyecto web en Visual Studio y seleccione **Publicar**.
1. Haga clic en el **botón Perfil personalizado** y después seleccione **Sistema de archivos** como método.
1. Elija el directorio. Por convención, el ejemplo descargado usa `bin\Release\PublishOutput`.

![Conexión de publicación][publish-connection]

Abra la sección **Opciones de publicación de archivos** de la pestaña **Configuración**. Seleccione **Precompilar durante la publicación**. Esta optimización significa que compilará vistas en el contenedor de Docker, está copiando las vistas precompiladas.

![Configuración de publicación][publish-settings]

Haga clic en **Publicar** y Visual Studio copiará todos los recursos necesarios en la carpeta de destino.

## <a name="build-the-image"></a>Compilar la imagen

Defina la imagen de Docker en un Dockerfile. El Dockerfile que contenga instrucciones para la imagen base, componentes adicionales, la aplicación que quiere ejecutar y otras imágenes de configuración.  El Dockerfile es la entrada al comando `docker build`, que crea la imagen.

Creará una imagen basada en la imagen `microsoft/aspnet` que se encuentra en [Docker Hub](https://hub.docker.com/r/microsoft/aspnet/).
La imagen base, `microsoft/aspnet`, es una imagen de Windows Server. Contiene Windows Server Core, IIS y ASP.NET 4.7.2. Al ejecutar esta imagen en el contenedor, iniciará de forma automática IIS y los sitios web instalados.

El Dockerfile que crea la imagen tiene el siguiente aspecto:

```console
# The `FROM` instruction specifies the base image. You are
# extending the `microsoft/aspnet` image.

FROM microsoft/aspnet

# The final instruction copies the site you published earlier into the container.
COPY ./bin/Release/PublishOutput/ /inetpub/wwwroot
```

No hay ningún comando `ENTRYPOINT` en este Dockerfile. No se necesita ninguno. Cuando se ejecuta Windows Server con IIS, el proceso IIS es el punto de entrada, que está configurado para iniciarse en la imagen base aspnet.

Ejecute el comando de compilación Docker para crear la imagen que se ejecuta en la aplicación ASP.NET. Para ello, abra una ventana de PowerShell en el directorio del proyecto y escriba el siguiente comando en el directorio de la solución:

```console
docker build -t mvcrandomanswers .
```

Este comando para compilar la imagen nueva con las instrucciones del dockerfile y nomenclatura (-t etiquetado) la imagen como mvcrandomanswers. Esto puede incluir la extracción de la imagen base de [Docker Hub](http://hub.docker.com) y después agrega la aplicación a esa imagen.

Una vez que el comando finaliza, puede ejecutar el comando `docker images` para ver información sobre la nueva imagen:

```console
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mvcrandomanswers              latest              86838648aab6        2 minutes ago       10.1 GB
```

El IDENTIFICADOR DE LA IMAGEN será diferente en su equipo. Ahora, ejecutará la aplicación.

## <a name="start-a-container"></a>Iniciar un contenedor

Inicie un contenedor mediante la ejecución del siguiente comando `docker run`:

```console
docker run -d --name randomanswers mvcrandomanswers
```

El argumento `-d` indica a Docker que inicie la imagen en modo desasociado. Esto significa que la imagen de Docker se ejecuta desconectada del shell actual.

En muchos ejemplos de docker, es posible que vea -p para asignar los puertos de contenedor y el host. La imagen de aspnet predeterminada ya configuró el contenedor para escuchar en el puerto 80 y exponerla.

El argumento `--name randomanswers` da un nombre al contenedor en ejecución. Puede usar este nombre en lugar del identificador del contenedor en la mayoría de los comandos.

El argumento `mvcrandomanswers` es el nombre de la imagen que se iniciará.

## <a name="verify-in-the-browser"></a>Comprobar en el explorador

Una vez que se inicie el contenedor, conectar con el contenedor en ejecución mediante `http://localhost` en el ejemplo mostrado. Escriba esa dirección URL en el explorador y debería ver el sitio en ejecución.

> [!NOTE]
> Algún software de proxy o VPN puede impedir que explore su sitio.
> Puede deshabilitarlo de forma temporal para asegurarse de que el contenedor funciona.

El directorio de ejemplo en GitHub contiene un [script de PowerShell](https://github.com/dotnet/samples/blob/master/framework/docker/MVCRandomAnswerGenerator/run.ps1) que ejecuta estos comandos. Abra una ventana de PowerShell, cambie el directorio por el directorio de la solución y escriba:

```console
./run.ps1
```

El comando anterior crea la imagen, muestra la lista de imágenes del equipo e inicia un contenedor.

Para detener el contenedor, envíe un comando `docker stop`:

```console
docker stop randomanswers
```

Para quitar el contenedor, envíe un comando `docker rm`:

```console
docker rm randomanswers
```

[windows-container]: media/aspnetmvc/SwitchContainer.png "Cambiar al contenedor de Windows"
[publish-connection]: media/aspnetmvc/PublishConnection.png "Publicar en el sistema de archivos"
[publish-settings]: media/aspnetmvc/PublishSettings.png "Configuración de publicación"
