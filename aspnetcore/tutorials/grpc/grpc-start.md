---
title: Creación de un servidor y un cliente gRPC en ASP.NET Core
author: juntaoluo
description: En este tutorial se le mostrará cómo crear un servicio gRPC y un cliente gRPC en ASP.NET Core. Aprenda a crear un proyecto de servicio gRPC, edite un archivo proto y agregue una llamada de streaming dúplex.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 06/05/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 71e3321819eb7169f0896abe3e07849f59ea6fc7
ms.sourcegitcommit: 5dd2ce9709c9e41142771e652d1a4bd0b5248cec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2019
ms.locfileid: "66692531"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a>Tutorial: Crear un servidor y un cliente gRPC en ASP.NET Core

Por [John Luo](https://github.com/juntaoluo)

En este tutorial se muestra cómo crear un cliente [gRPC](https://grpc.io/docs/guides/) de .NET Core y un servidor gRPC de ASP.NET Core.

Al final tendrá un cliente gRPC que se comunica con el servicio Greeter de gRPC.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample)).

En este tutorial ha:

> [!div class="checklist"]
> * Crear un servicio gRPC.
> * Crear un cliente gRPC.
> * Probar el servicio cliente gRPC con el servicio gRPC Greeter.

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a>Crear un servicio gRPC

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.
* En el cuadro de diálogo **Crear un proyecto nuevo**, seleccione **Aplicación web ASP.NET Core**.
* Seleccione **Siguiente**.
* Llame al proyecto **GrpcGreeter**. Es importante asignarle el nombre *GrpcGreeter* para que los espacios de nombres coincidan al copiar y pegar el código.
* Seleccione **Crear**.
* En el cuadro de diálogo **Crear una aplicación web ASP.NET Core**:
  * Seleccione **.NET Core** y **ASP.NET Core 3.0** en los menús desplegables. 
  * Seleccione la plantilla **Servicio gRPC**.
  * Seleccione **Crear**.

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

  Visual Studio ejecuta el servicio en un símbolo del sistema.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio para Mac](#tab/visual-studio-code+visual-studio-mac)

* Ejecute el proyecto GrpcGreeter de gRPC Greeter desde la línea de comandos mediante `dotnet run`.

<!-- End of combined VS/Mac tabs -->

---

Los registros muestran que el servicio está escuchando en `http://localhost:50051`.

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

### <a name="examine-the-project-files"></a>Examen de los archivo del proyecto

Archivos de GrpcGreeter:

* *greet.proto*: El archivo *Protos/greet.proto* define el gRPC `Greeter` y se usa para generar los recursos de servidor gRPC. Para obtener más información, vea [Introducción a gRPC](xref:grpc/index).
* Carpeta *Servicios*: contiene la implementación del servicio `Greeter`.
* *appSettings.json*: contiene datos de configuración, como el protocolo usado por Kestrel. Para obtener más información, vea <xref:fundamentals/configuration/index>.
* *Program.cs*: contiene el punto de entrada para el servicio gRPC. Para obtener más información, vea <xref:fundamentals/host/web-host>.
* *Startup.cs*: Contiene código que configura el comportamiento de la aplicación. Para obtener más información, vea [Inicio de la aplicación](xref:fundamentals/startup).

## <a name="create-the-grpc-client-in-a-net-console-app"></a>Creación del cliente gRPC en una aplicación de consola de .NET

## <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Seleccione **Archivo** > **Nuevo** > **Proyecto** de la barra de menús.
* En el cuadro de diálogo **Crear un nuevo proyecto**, seleccione **Aplicación de consola (.NET Core)** .
* Seleccione **Siguiente**.
* En el cuadro de texto **Nombre**, escriba "GrpcGreeterClient".
* Seleccione **Crear**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Cambie los directorios (`cd`) a una carpeta que contenga el proyecto.
* Ejecute los comandos siguientes:

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

Siga las instrucciones que se indican [aquí](/dotnet/core/tutorials/using-on-mac-vs-full-solution) para crear una aplicación de consola con el nombre *GrpcGreeterClient*.

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a>Adición de paquetes necesarios

Agregue los siguientes paquetes al proyecto de cliente gRPC:

* [Grpc.Core](https://www.nuget.org/packages/Grpc.Core), que contiene la API de C# para el cliente de C-core.
* [Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), que contiene API de mensajes protobuf para C#.
* [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), que contiene compatibilidad con herramientas de C# para archivos protobuf. El paquete de herramientas no es necesario en el runtime, de modo que la dependencia se marca con `PrivateAssets="All"`.

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Instalación de los paquetes con la Consola del Administrador de paquetes (PMC) o mediante Administrar paquete de NuGet

#### <a name="pmc-option-to-install-packages"></a>Opción de PMC para instalar paquetes

* En Visual Studio, seleccione **Herramientas** > **Administrador de paquetes de NuGet** > **Consola del Administrador de paquetes**.
* En la ventana de la **Consola del Administrador de paquetes**, desplácese al directorio en el que se encuentra el archivo *GrpcGreeterClient.csproj*.
* Ejecute los comandos siguientes:

 ```powershell
Install-Package Grpc.Core
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a>Administración de la opción Paquetes NuGet para instalar paquetes

* Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** > **Administrar paquetes NuGet**.
* Seleccione la pestaña **Examinar**.
* Escriba **Grpc.Core** en el cuadro de búsqueda.
* Seleccione el paquete **Grpc.Core** en la pestaña **Examinar** y haga clic en **Instalar**.
* Repita el proceso para `Google.Protobuf` y `Grpc.Tools`.

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Ejecute los siguientes comandos en el **terminal integrado**:

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Core
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* Haga clic con el botón derecho en la carpeta **Paquetes**, en **Panel de solución** > **Agregar paquetes**.
* Escriba **Grpc.Core** en el cuadro de búsqueda.
* Seleccione el paquete **Grpc.Core** en el panel de resultados y haga clic en **Agregar paquete**.
* Repita el proceso para `Google.Protobuf` y `Grpc.Tools`.

---

### <a name="add-greetproto"></a>Adición de greet.proto

* Cree una carpeta **Protos** en el proyecto de cliente gRPC.
* Copie el archivo **Protos\greet.proto** del servicio gRPC Greeter en el proyecto de cliente gRPC.
* Edite el archivo de proyecto *GrpcGreeterClient.csproj*:

  # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

  Haga clic con el botón derecho en el proyecto y seleccione **Editar GrpcGreeterClient.csproj**.

  # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code) 

  Seleccione el archivo *GrpcGreeterClient.csproj*.

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

  Haga clic con el botón derecho en el proyecto y seleccione **Herramientas > Editar archivo**.

  ---

* Agregue el archivo **greet.proto** al grupo de elementos `<Protobuf>` del archivo del proyecto de GrpcGreeterClient:

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

Compile el proyecto cliente para desencadenar la generación de los recursos del cliente de C#.

### <a name="create-the-greeter-client"></a>Creación del cliente de Greeter

Compile el proyecto para crear los tipos en el espacio de nombres **Greeter**. El proceso de compilación genera automáticamente los tipos `Greeter`.

Actualice el archivo *Program.cs* del cliente gRPC con el código siguiente:

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

*Program.cs* contiene el punto de entrada y la lógica para el cliente gRPC.

El cliente de Greeter se crea mediante lo siguiente:

* Creación de una instancia de `Channel` que contiene la información para crear la conexión al servicio gRPC.
* Uso de `Channel` para construir el cliente de Greeter:

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-6)]

El cliente de Greeter realiza una llamada al método `SayHello` asincrónico. Se muestra el resultado de la llamada a `SayHello`:

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-9)]

Apague el parámetro `Channel` que usa el cliente cuando las operaciones hayan finalizado la liberación de todos los recursos.

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a>Prueba del cliente gRPC con el servicio gRPC Greeter

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* En el servicio Greeter, pulse Ctrl+F5 para iniciar el servidor sin el depurador.
* En el proyecto GrpcGreeterClient, pulse Ctrl+F5 para iniciar el servidor sin el depurador.

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio para Mac](#tab/visual-studio-code+visual-studio-mac)

* Inicie el servicio Greeter.
* Inicie el cliente.

<!-- End of combined VS/Mac tabs -->

---

El cliente envía un saludo al servicio con un mensaje que contiene su nombre "GreeterClient". El servicio envía el mensaje "Hello GreeterClient" como respuesta. La respuesta "Hello GreeterClient" se muestra en el símbolo del sistema:

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

El servicio gRPC registra los detalles de la llamada correcta en los registros escritos en el símbolo del sistema.

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\GH\aspnet\docs\4\Docs\aspnetcore\tutorials\grpc\grpc-start\sample\GrpcGreeter
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 78.32260000000001ms 200 application/grpc
```

### <a name="next-steps"></a>Pasos siguientes

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
