---
title: 'Tutorial: Introducción a gRPC en ASP.NET Core'
author: juntaoluo
description: Esta serie de tutoriales muestra cómo crear un servicio gRPC en ASP.NET Core. Aprenda a crear un proyecto de servicio gRPC, edite un archivo proto y agregue una llamada de streaming dúplex.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 2/26/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: a67050331cc8563b1baf5312b92910c1bbdfbd69
ms.sourcegitcommit: 57a974556acd09363a58f38c26f74dc21e0d4339
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59672572"
---
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a>Tutorial: Introducción al servicio gRPC en ASP.NET Core

Por [John Luo](https://github.com/juntaoluo)

En este tutorial se enseñan los conceptos básicos de la compilación de un servicio gRPC en ASP.NET Core.

Al final, tendrá un servicio gRPC que repite saludos.

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

En este tutorial hará lo siguiente:

> [!div class="checklist"]
> * Crear un servicio gRPC.
> * Ejecutar el servicio gRPC.
> * Examinar los archivos de proyecto.

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a>Crear un servicio gRPC

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.
* Cree una aplicación web de ASP.NET Core.
  ![Nueva aplicación web de ASP.NET Core](grpc-start/_static/np_3_0.1.png)
* Llame al proyecto **GrpcGreeter**. Es importante asignarle el nombre *GrpcGreeter* para que los espacios de nombres coincidan al copiar y pegar el código.
  ![Nueva aplicación web de ASP.NET Core](grpc-start/_static/np_3_0.2.png)
* Seleccione **.NET Core** y **ASP.NET Core 3.0** en el menú desplegable. Elija la plantilla **Servicio gRPC**.

  Se crea el proyecto de inicio siguiente:

  ![Explorador de soluciones](grpc-start/_static/se3.0.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Cambie los directorios (`cd`) a una carpeta que contenga el proyecto.
* Ejecute los comandos siguientes:

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * El comando `dotnet new` crea un nuevo servicio gRPC en la carpeta *GrpcGreeter*.
  * El comando `code` abre la carpeta *GrpcGreeter* en una nueva instancia de Visual Studio Code.

  Se muestra un cuadro de diálogo con el texto **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).
* Seleccione **Sí**.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

Desde un terminal, ejecute estos comandos:

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

Los comandos anteriores utilizan la [CLI de .NET Core](/dotnet/core/tools/dotnet) para crear un servicio gRPC.

### <a name="open-the-project"></a>Abrir el proyecto

En Visual Studio, seleccione **Archivo > Abrir** y elija el archivo *GrpcGreeter.sln*.

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a>Ejecutar el servicio

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Presione Ctrl+F5 para ejecutar el servicio gRPC sin el depurador.

  Visual Studio ejecuta el servicio en un símbolo del sistema. Los registros muestran que el servicio inició la escucha en `http://localhost:50051`.

  ![Nueva aplicación web de ASP.NET Core](grpc-start/_static/server_start.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio para Mac](#tab/visual-studio-code+visual-studio-mac)

* Ejecute el proyecto GrpcGreeter de gRPC Greeter desde la línea de comandos mediante `dotnet run`. Los registros muestran que el servicio inició la escucha en `http://localhost:50051`.

```console
dbug: Grpc.AspNetCore.Server.Internal.GrpcServiceBinder[1]
      Added gRPC method 'SayHello' to service 'Greet.Greeter'. Method type: 'Unary', route pattern: '/Greet.Greeter/SayHello'.
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\gh\Docs\aspnetcore\tutorials\grpc\grpc-start\samples\GrpcGreeter
```

<!-- End of combined VS/Mac tabs -->

---

En el siguiente tutorial se le enseñará cómo crear un cliente gRPC, que es necesario para probar el servicio Greeter.

### <a name="examine-the-project-files-of-the-grpc-project"></a>Examinar los archivos de proyecto del proyecto gRPC

Archivos de GrpcGreeter:

* greet.proto: El archivo *Protos/greet.proto* define el gRPC `Greeter` y se usa para generar los recursos de servidor gRPC. Para obtener más información, vea <xref:grpc/index>.
* Carpeta *Servicios*: contiene la implementación del servicio `Greeter`.
* *appSettings.json*: contiene datos de configuración, como el protocolo usado por Kestrel. Para obtener más información, vea <xref:fundamentals/configuration/index>.
* *Program.cs*: contiene el punto de entrada para el servicio gRPC. Para obtener más información, vea <xref:fundamentals/host/web-host>.
* Startup.cs: Contiene código que configura el comportamiento de la aplicación. Para obtener más información, vea <xref:fundamentals/startup>.

### <a name="test-the-service"></a>Probar el servicio

## <a name="additional-resources"></a>Recursos adicionales

En este tutorial ha:

> [!div class="checklist"]
> * Creado un servicio gRPC.
> * Ejecutado el servicio gRPC.
> * Examinado los archivo del proyecto.

> [!div class="step-by-step"]
> [Siguiente: Creación de un cliente gRPC de .NET Core](xref:tutorials/grpc/grpc-client)