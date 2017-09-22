---
title: Host en un servicio de Windows
author: tdykstra
description: "Obtenga información acerca de cómo hospedar una aplicación de ASP.NET Core en un servicio de Windows."
keywords: Hospedaje de ASP.NET Core, servicio de Windows,
ms.author: tdykstra
manager: wpickett
ms.date: 03/30/2017
ms.topic: article
ms.assetid: d9a65066-d7cb-47df-b046-64629c4d2c6f
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/windows-service
ms.openlocfilehash: 5b54c77ff9e019b1d550aa687923077a3e9ba5c2
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2017
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a>Hospedar una aplicación ASP.NET básica en un servicio de Windows

Por [Tom Dykstra](https://github.com/tdykstra)

Es la manera recomendada para hospedar una aplicación de ASP.NET Core en Windows si no usa IIS para que se ejecute un [servicio de Windows](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications). De este modo puede iniciar automáticamente después de reiniciar el equipo y se bloquea, sin esperar a que alguien inicie sesión.

[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) consulte la [pasos](#next-steps) sección para obtener instrucciones sobre cómo ejecutarlo.

## <a name="prerequisites"></a>Requisitos previos

* Debe ejecutar la aplicación en el tiempo de ejecución de .NET framework.  En el *.csproj* de archivos, especifique los valores adecuados para [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) y [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog). Por ejemplo:

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  Al crear un proyecto en Visual Studio, utilice el **aplicación de ASP.NET Core (.NET Framework)** plantilla.

* Si la aplicación obtendrá las solicitudes de internet (no solo desde una red interna), debe utilizar el [WebListener](xref:fundamentals/servers/weblistener) servidor web en lugar de [Kestrel](xref:fundamentals/servers/kestrel).  Kestrel debe utilizarse con IIS para las implementaciones de borde.  Para más información, vea [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Cuándo se debe usar Kestrel con un proxy inverso).

## <a name="getting-started"></a>Introducción

En esta sección se explica los cambios mínimos necesarios para configurar un proyecto de ASP.NET Core existente para que se ejecute en un servicio.

* Instale el paquete NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

* Realice los cambios siguientes en `Program.Main`:
  
  * Llame a `host.RunAsService` en lugar de `host.Run`.
  
  * Si el código llama `UseContentRoot`, use una ruta de acceso a la ubicación de publicación en lugar de`Directory.GetCurrentDirectory()` 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* Publicar la aplicación en una carpeta.

  Use [publicar dotnet](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) o un [perfil de publicación de Visual Studio](xref:publishing/web-publishing-vs) que publica en una carpeta.

* Probar al crear e iniciar el servicio.

  Abra una ventana de símbolo del sistema de administrador para usar la [sc.exe](https://technet.microsoft.com/library/bb490995) herramienta de línea de comandos para crear e iniciar un servicio.  
  
  Si asigna un nombre al servicio MyService, publicar la aplicación en `c:\svc`y la propia aplicación se denomina AspNetCoreService, los comandos sería similar al siguiente:

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```
  El `binPath` valor es la ruta de acceso al ejecutable de la aplicación, incluido el nombre de archivo ejecutable propio.

  ![Ventana de la consola crear e iniciar el ejemplo](windows-service/_static/create-start.png)

  Cuando termine de estos comandos, que puede ir a la misma ruta que cuando ejecuta como una aplicación de consola (de forma predeterminada, `http://localhost:5000`)

  ![Ejecución en un servicio](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a>Proporcionan una manera para que se ejecute fuera de un servicio

Es más fácil de probar y depurar cuando se ejecuta fuera de un servicio, por lo que es habitual para agregar código que llama a `host.RunAsService` únicamente bajo ciertas condiciones.  Por ejemplo, podría ejecutar una aplicación de consola como si se produce un `--console` argumento de línea de comandos o si el depurador se adjunta.

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a>Controlar detener e iniciar los eventos

Si desea controlar `OnStarting`, `OnStarted`, y `OnStopping` eventos, realice los siguientes cambios adicionales:

* Cree una clase que derive de `WebHostService`.

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* Crear un método de extensión para `IWebHost` que pasa personalizado `WebHostService` a `ServiceBase.Run`.

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* En `Program.Main` cambio llamar al nuevo método de extensión en lugar de `host.RunAsService`.

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

Si su personalizado `WebHostService` código necesita obtener un servicio de inserción de dependencias (como un registrador), puede obtener desde la `Services` propiedad de `IWebHost`.

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a>Pasos siguientes

El [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) que muestra en este artículo es una aplicación web MVC sencilla que se ha modificado como se muestra en los anteriores ejemplos de código.  Para ejecutarlo en un servicio, realice los pasos siguientes:

* Publicar en *c:\svc*.

* Abra una ventana de administrador.

* Escriba los siguientes comandos:

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * En un explorador, vaya a http://localhost: 5000 para comprobar que se está ejecutando.

Si la aplicación no se inicia como se esperaba cuando se ejecuta en un servicio, es una forma rápida de hacer accesibles los mensajes de error agregar un proveedor de registro como el [proveedor de registro de eventos de Windows](xref:fundamentals/logging#eventlog).

## <a name="acknowledgments"></a>Confirmaciones

En este artículo se escribió con la Ayuda de los orígenes que ya se han publicado. La primera y más útiles de ellos fueron estos:

* [Hospedaje de ASP.NET Core como servicio de Windows](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [Cómo hospedar el núcleo de ASP.NET en un servicio de Windows](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
