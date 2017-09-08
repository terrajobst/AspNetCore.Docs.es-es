---
title: Compilar proyectos con Yeoman en ASP.NET Core
author: spboyer
description: "Este artículo le guía por la creación de un núcleo de ASP.NET aplicación web mediante la Yeoman generador en macOS."
keywords: "Núcleo de ASP.NET, Yeoman, multiplataforma, yo aspnet"
ms.author: spboyer
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.assetid: fda0c2a8-1743-4505-be1a-7f8ceeef8647
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/yeoman
ms.openlocfilehash: 3a7cd83becc570d2f73014b356977fedb16f29bb
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-building-projects-with-yeoman-in-aspnet-core"></a>Introducción a la creación de proyectos con Yeoman en ASP.NET Core

[Yeoman](http://yeoman.io/) es un sistema de scaffolding de proyecto para crear muchos tipos de aplicaciones. El Yeoman generador para ASP.NET Core contiene una variedad de plantillas de proyecto para iniciar un nuevo sitio web, MVC o aplicación de consola.

## <a name="install-nodejs-npm-and-yeoman"></a>Instalar Node.js y npm, Yeoman

### <a name="prerequisites"></a>Requisitos previos

Se requiere para Yeoman Node.js y npm. Descargar desde [Node.js](https://nodejs.org/en/). El instalador incluye [Node.js](https://nodejs.org/en/) y [npm](https://www.npmjs.com/). Bower también es necesario para la instalación de marcos de interfaz de usuario como de arranque.

Para instalar Yeoman y Bower, ejecute el siguiente comando:

```console
npm install -g yo bower
```

>[!Note]
>Si se produce un error `npm ERR! Please try running this command again as root/Administrator.` en macOS, ejecute el siguiente comando mediante [sudo](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html):`sudo npm install -g yo bower`

Desde un símbolo del sistema, instalar el generador ASP.NET:

```console
npm install -g generator-aspnet
```

> [!NOTE]
> Si recibe un error de permiso, ejecute el comando en `sudo` tal y como se ha descrito anteriormente.

El `–g` marca instala el generador globalmente, de modo que se puede utilizar en cualquier ruta de acceso.

## <a name="create-an-aspnet-app"></a>Crear una aplicación ASP.NET

Ejecutar el generador de ASP.NET basado en Yeoman:

```console
yo aspnet
```

El generador de muestra un menú. Flecha hacia abajo hasta la **básicos de aplicación Web [sin pertenencia y autorización]** del proyecto y pulse **ENTRAR**:

![Ventana de comandos: ¿qué tipo de aplicación desea crear? Menú de tipos de aplicación](yeoman/_static/yeoman-yo-aspnet.png)

Seleccione Bootstrap como el marco de interfaz de usuario y pulse **ENTRAR**.

Use "**MyWebApp**" para el nombre de la aplicación y a continuación, puntee **ENTRAR**.

Yeoman se aplique la técnica scaffolding al proyecto y sus archivos auxiliares. También se proporcionan los pasos siguientes sugeridos en forma de comandos.

![Ventana de comandos: ¿cuál es el nombre de la aplicación ASP.NET? Símbolo del sistema](yeoman/_static/yeoman-yo-aspnet-created.png)

El [generador ASP.NET](https://www.npmjs.com/package/generator-aspnet) crea ASP.NET Core proyectos que se pueden cargar en el código de Visual Studio, Visual Studio, o ejecutar desde la línea de comandos.

## <a name="restore-build-and-run"></a>Restaurar, compilar y ejecutar

Siga los comandos sugeridos, cambie los directorios a la `MyWebApp` directory. A continuación, ejecute `dotnet restore`.

![Comandos (ventana)](yeoman/_static/dotnet-restore.png)

Compilar y ejecutar la aplicación con `dotnet build` y `dotnet run`:

![Comandos (ventana)](yeoman/_static/dotnet-build-run.png)

En este punto, puede navegar a la dirección URL se muestra para probar la aplicación de ASP.NET Core recién creado.

## <a name="client-side-packages"></a>Paquetes de cliente

Los recursos de front-end se proporcionan con las plantillas de la Yeoman generador utilizando el [Bower](xref:client-side/bower) Administrador de paquetes de cliente, agregar *bower.json* y *bowerrc* archivos para restaurar los paquetes de cliente mediante Bower.

El [BundlerMinifier](xref:client-side/bundling-and-minification) componente también se incluye de forma predeterminada para facilitar la concatenación (unión) y minificación de CSS, JavaScript y HTML.

## <a name="building-and-running-from-visual-studio"></a>Compilar y ejecutar desde Visual Studio

Puede cargar el proyecto web de ASP.NET Core generado directamente en Visual Studio, a continuación, compile y ejecute el proyecto a partir de ahí. Siga las instrucciones anteriores para aplicar una nueva aplicación de ASP.NET Core mediante Yeoman la técnica scaffolding. Esta vez, se selecciona **aplicación Web** en el menú y el nombre de la aplicación `MyWebApp`.

Abra Visual Studio. En el menú archivo, seleccione ‣ Abrir proyecto o solución.

En el cuadro de diálogo Abrir proyecto, vaya a la *.csproj* de archivo, selecciónelo y haga clic en el **abiertos** botón. En el Explorador de soluciones, el proyecto debe ser similar a la captura de pantalla siguiente.

![Archivos y carpetas de un nuevo proyecto en el Explorador de soluciones](yeoman/_static/yeoman-solution.png)

Yeoman scaffolds una aplicación web MVC, compatibilidad de compilación de servidor y cliente junto con ambos. Dependencias de servidor aparecen en la **dependencias/NuGet** nodo y las dependencias del lado cliente en el **dependencias/Bower** nodo del explorador de soluciones. Las dependencias se restauran automáticamente cuando se carga el proyecto.

![En el nodo dependencias en la vista de árbol del explorador de soluciones, la carpeta de Bower está abierta enumerar sus dependencias.](yeoman/_static/yeoman-loading-dependencies.png)

Cuando se restauran todas las dependencias, presione **F5** para ejecutar el proyecto. La página principal predeterminada se muestra en el explorador.

![Aplicación Web abierta en Microsoft Edge](yeoman/_static/yeoman-home-page.png)

## <a name="restoring-building-and-hosting-from-a-command-line"></a>Restauración, creación y hospedaje desde una línea de comandos

Puede preparar y hospedar la aplicación web mediante la CLI de .NET Core.

En un símbolo del sistema, cambie el directorio actual a la carpeta que contiene el proyecto (es decir, la carpeta que contiene el *.csproj* archivo):

```console
cd src\MyWebApp
```

Restaurar las dependencias de paquetes de NuGet del proyecto:

```console
dotnet restore
```

Ejecute la aplicación:

```console
dotnet run
```

La multiplataforma [Kestrel](xref:fundamentals/servers/kestrel) servidor web iniciará escuchando en el puerto 5000.

Abra un explorador web y navegue hasta `http://localhost:5000`.

![Aplicación Web abierta en Microsoft Edge](yeoman/_static/yeoman-home-page_5000.png)

## <a name="adding-to-your-project-with-sub-generators"></a>Agregar a un proyecto con los generadores de sub

Usando Yeoman [sub generadores](https://www.github.com/omnisharp/generator-aspnet#sub-generators), puede agregar un `nuget.config` o `web.config` después de crear el proyecto. Por ejemplo, ejecute el siguiente comando desde el directorio en el que se debe crear el archivo:

```console
yo aspnet:nugetconfig
```

El resultado es un archivo de configuración de NuGet denominado `nuget.config` con el siguiente contenido:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
 <packageSources>
    <!--To inherit the global NuGet package sources remove the <clear/> line below -->
    <clear />
 </packageSources>
</configuration>
```

## <a name="additional-resources"></a>Recursos adicionales

* [Servidores (Kestrel y WebListener)](xref:fundamentals/servers/index)
* [Conceptos básicos](xref:fundamentals/index)
