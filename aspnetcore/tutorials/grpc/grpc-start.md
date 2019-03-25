---
title: 'Tutorial: Introducción a gRPC en ASP.NET Core'
author: juntaoluo
description: Esta serie de tutoriales muestra cómo crear un servicio gRPC en ASP.NET Core. Aprenda a crear un proyecto de servicio gRPC, edite un archivo proto y agregue una llamada de streaming dúplex.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 2/26/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 5f7a2f6b57804b3295b23c322dcbac553b05528b
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320061"
---
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a>Tutorial: Introducción al servicio gRPC en ASP.NET Core

Por [John Luo](https://github.com/juntaoluo)

En este tutorial se enseñan los conceptos básicos de la compilación de un servicio gRPC en ASP.NET Core.

Al final, tendrá un servicio gRPC que repite saludos.

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

En este tutorial ha:

> [!div class="checklist"]
> * Creará un servicio gRPC.
> * Ejecute el servicio.
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

### <a name="test-the-service"></a>Probar el servicio

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Asegúrese de que **GrpcGreeter.Server** se establece como proyecto de inicio y presione Ctrl+F5 para ejecutar el servicio gRPC sin el depurador.

  Visual Studio ejecuta el servicio en un símbolo del sistema. Los registros muestran que el servicio inició la escucha en `http://localhost:50051`.

  ![Nueva aplicación web de ASP.NET Core](grpc-start/_static/server_start.png)

* Una vez que el servicio se encuentre en ejecución, establezca **GrpcGreeter.Client** como proyecto de inicio y presione Ctrl+F5 para ejecutar el cliente sin el depurador.

  El cliente envía un saludo al servicio con un mensaje que contiene su nombre "GreeterClient". El servicio enviará un mensaje "Hola GreeterClient" como respuesta que se muestra en el símbolo del sistema.

  ![Nueva aplicación web de ASP.NET Core](grpc-start/_static/client.png)

  El servicio registra los detalles de la llamada correcta en los registros escritos en el símbolo del sistema.

  ![Nueva aplicación web de ASP.NET Core](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio para Mac](#tab/visual-studio-code+visual-studio-mac)

* Ejecute el proyecto de servidor GrpcGreeter.Server desde la línea de comandos mediante `dotnet run`. Los registros muestran que el servicio inició la escucha en `http://localhost:50051`.

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\example\GrpcGreeter\GrpcGreeter.Server
```

* Ejecute el proyecto de cliente GrpcGreeter.Server desde la línea de comandos independiente mediante `dotnet run`.

El cliente envía un saludo al servicio con un mensaje que contiene su nombre "GreeterClient". El servicio enviará un mensaje "Hola GreeterClient" como respuesta que se muestra en el símbolo del sistema.

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

El servicio registra los detalles de la llamada correcta en los registros escritos en el símbolo del sistema.

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\gh\tp\GrpcGreeter\GrpcGreeter.Server
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[2]
      Request finished in 107.46730000000001ms 200 application/grpc
```

<!-- End of combined VS/Mac tabs -->

---

### <a name="examine-the-project-files-of-the-grpc-project"></a>Examinar los archivos de proyecto del proyecto gRPC

Archivos GrpcGreeter.Server:

* greet.proto: El archivo *Protos/greet.proto* define el gRPC `Greeter` y se usa para generar los recursos de servidor gRPC. Para obtener más información, vea <xref:grpc/index>.
* Carpeta *Servicios*: contiene la implementación del servicio `Greeter`.
* *appSettings.json*: contiene datos de configuración, como el protocolo usado por Kestrel. Para obtener más información, vea <xref:fundamentals/configuration/index>.
* *Program.cs*: contiene el punto de entrada para el servicio gRPC. Para obtener más información, vea <xref:fundamentals/host/web-host>.
* Startup.cs

Contiene código que configura el comportamiento de la aplicación. Para obtener más información, vea <xref:fundamentals/startup>.

Archivo GrpcGreeter.Client de cliente gRPC:

*Program.cs* contiene el punto de entrada y la lógica para el cliente gRPC.

En este tutorial ha:

> [!div class="checklist"]
> * Creado un servicio gRPC.
> * Ejecutado el servicio y un cliente para probar el servicio.
> * Examinado los archivo del proyecto.
