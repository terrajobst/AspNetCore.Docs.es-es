---
title: 'Tutorial: Creación de un cliente gRPC de .NET Core'
author: juntaoluo
description: Esta serie de tutoriales muestra cómo crear un servicio gRPC en ASP.NET Core. Aprenda a crear un proyecto de servicio gRPC, edite un archivo proto y agregue una llamada de streaming dúplex.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 4/10/2019
uid: tutorials/grpc/grpc-client
ms.openlocfilehash: ec6bf5072c76de640a78b2c3f13dd1fc552b9d04
ms.sourcegitcommit: b508b115107e0f8d7f62b25cfcc8ad45e1373459
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65212641"
---
# <a name="tutorial-create-a-net-core-grpc-client"></a>Tutorial: Creación de un cliente gRPC de .NET Core

Por [John Luo](https://github.com/juntaoluo)

En este tutorial se muestra cómo crear un cliente [gRPC](https://grpc.io/docs/guides/) de .NET Core que se puede comunicar con servicios gRPC.

Al final tendrá un cliente gRPC que se comunica con el servicio Greeter de gRPC.

[!INCLUDE[View or download sample code](~/includes/grpc/downloadClient.md)]

En este tutorial ha:

> [!div class="checklist"]
> * Crear un cliente gRPC.
> * Ejecutar el servicio con el servicio gRPC Greeter creado en el tutorial anterior.
> * Examinar los archivos de proyecto.

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-net-console-application"></a>Creación de una aplicación de consola .NET

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Siga las instrucciones que se indican [aquí](/dotnet/core/tutorials/with-visual-studio) para crear una aplicación de consola con el nombre *GrpcGreeterClient*.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Siga las instrucciones que se indican [aquí](/dotnet/core/tutorials/with-visual-studio-code) para crear una aplicación de consola con el nombre *GrpcGreeterClient*.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

Siga las instrucciones que se indican [aquí](/dotnet/core/tutorials/using-on-mac-vs-full-solution) para crear una aplicación de consola con el nombre *GrpcGreeterClient*.

<!-- End of VS tabs -->

---

## <a name="add-required-packages"></a>Adición de paquetes necesarios

Agregue los siguientes paquetes al proyecto de cliente gRPC:

* [Grpc.Core](https://www.nuget.org/packages/Grpc.Core), que contiene la API de C# para el cliente de C-core.
* [Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), que contiene API de mensajes protobuf para C#.
* [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), que contiene compatibilidad con herramientas de C# para archivos protobuf. El paquete de herramientas no es necesario en el runtime, de modo que la dependencia se marca con `PrivateAssets="All"`.

Se pueden agregar paquetes con los métodos siguientes:

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="pmc-option-to-install-packages"></a>Opción de PMC para instalar paquetes

* En Visual Studio, seleccione **Herramientas** > **Administrador de paquetes de NuGet** > **Consola del Administrador de paquetes**.
* En la ventana de la **Consola del Administrador de paquetes**, desplácese al directorio en el que se encuentra el archivo *GrpcGreeterClient.csproj*.
* Ejecute el siguiente comando:

    ```powershell
    Install-Package Grpc.Core
    ```

* Repita `Install-Package` para Google.Protobuf y Grpc.Tools.

<!-- Tutorials shouldn't have multiple options. Select what you think is the best approach. Recommend you removed this approach. -->

### <a name="manage-nuget-packages-option-to-install-packages"></a>Administración de la opción Paquetes NuGet para instalar paquetes

* Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** > **Administrar paquetes NuGet**.
* Establezca el **origen del paquete** en "nuget.org".
* Escriba "Grpc.Core" en el cuadro de búsqueda.
* Seleccione el paquete "Grpc.Core" en la pestaña **Examinar** y haga clic en **Instalar**.
* Repita este paso para Google.Protobuf y Grpc.Tools.

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Ejecute el siguiente comando en el **terminal integrado**:

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Core
```

Repita este paso para Google.Protobuf y Grpc.Tools.

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* Haga clic con el botón derecho en la carpeta *Paquetes* en **Panel de solución** > **Agregar paquetes...**
* Establezca el menú desplegable **Origen** de la ventana **Agregar paquetes** en "nuget.org".
* Escriba "Grpc.Core" en el cuadro de búsqueda.
* Seleccione el paquete "Grpc.Core" en el panel de resultados y haga clic en **Agregar paquete**.
* Repita este paso para Google.Protobuf y Grpc.Tools.

---

## <a name="add-the-greetproto-file"></a>Adición del archivo greet.proto

Copie el archivo **Protos\greet.proto** del servicio gRPC Greeter en el proyecto de cliente gRPC. Agregue el archivo **greet.proto** al grupo de elementos `<Protobuf>` del archivo del proyecto de GrpcGreeterClient:

```XML
<Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
```

> [!NOTE]
> Para abrir el archivo del proyecto de GrpcGreeterClient, haga clic con el botón derecho en el proyecto y seleccione la opción **Editar GrpcGreeterClient.csproj** en el menú desplegable.
>
> ![Nueva aplicación web de ASP.NET Core](grpc-start/_static/edit_csproj.png)
>
> También puede navegar al directorio de GrpcGreeterClient y editar `GrpcGreeterClient.csproj` con su editor favorito.

El atributo `GrpcServices="Client"` se agrega para que solo se generen recursos del cliente de C# para el archivo protobuf incluido. Compile el proyecto cliente para desencadenar la generación de los recursos del cliente de C#.

## <a name="create-a-greeterclient-and-invoke-the-sayhello-unary-call"></a>Creación de GreeterClient e invocación de la llamada unaria SayHello

Agregue el código siguiente al método `Main` del archivo `Program.cs` del proyecto de cliente gRPC:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet)]

Para acceder a los tipos requeridos, se necesitan las instrucciones siguientes:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=using)]

Se crea GreeterClient mediante la creación de una instancia de `Channel` que contiene la información para crear la conexión al servicio gRPC y usándola para construir `GreeterClient`:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-5)]

GreeterClient contiene la llamada unaria `SayHello`, que se puede invocar de forma asincrónica:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-8)]

Los resultados de la llamada `SayHello` se almacenan en `reply`, desde donde se pueden mostrar:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=9)]

El valor de `Channel` que usa el cliente se debe apagar cuando las operaciones hayan finalizado la liberación de todos los recursos:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=11)]

> [!NOTE]
> Deberá compilar el proyecto antes de que los tipos del espacio de nombres **Greeter** se puedan resolver. Estos tipos se generan automáticamente durante la compilación y no están disponibles antes de ejecutar una compilación.

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a>Prueba del cliente gRPC con el servicio gRPC Greeter

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Asegúrese de que el servicio Greeter creado en el tutorial anterior está en ejecución.

* Una vez que el servicio esté en ejecución, vuelva al proyecto **GrpcGreeterClient** y establézcalo como el proyecto de inicio. Presione Ctrl+F5 para ejecutar el cliente sin el depurador.

  El cliente envía un saludo al servicio con un mensaje que contiene su nombre "GreeterClient". El servicio enviará un mensaje "Hola GreeterClient" como respuesta que se muestra en el símbolo del sistema.

  ![Nueva aplicación web de ASP.NET Core](grpc-start/_static/client.png)

  El servicio registra los detalles de la llamada correcta en los registros escritos en el símbolo del sistema.

  ![Nueva aplicación web de ASP.NET Core](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio para Mac](#tab/visual-studio-code+visual-studio-mac)

* Asegúrese de que el servicio Greeter creado en el tutorial anterior está en ejecución.

* Ejecute el proyecto de cliente GrpcGreeter.Server desde la línea de comandos independiente mediante `dotnet run`.

El cliente envía un saludo al servicio con un mensaje que contiene su nombre "GreeterClient". El servicio enviará un mensaje "Hola GreeterClient" como respuesta que se muestra en el símbolo del sistema.

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

El servicio registra los detalles de la llamada correcta en los registros escritos en el símbolo del sistema.

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
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[2]
      Request finished in 194.5798ms 200 application/grpc
```

<!-- End of combined VS/Mac tabs -->

---

### <a name="examine-the-project-files-of-the-grpc-project"></a>Examinar los archivos de proyecto del proyecto gRPC

Archivo GrpcGreeterClient del cliente gRPC:

*Program.cs* contiene el punto de entrada y la lógica para el cliente gRPC.

## <a name="additional-resources"></a>Recursos adicionales

En este tutorial ha:

> [!div class="checklist"]
> * Crear un cliente gRPC.
> * Ejecutar el servicio con el servicio gRPC Greeter creado en el tutorial anterior.
> * Examinar los archivos de proyecto.

> [!div class="step-by-step"]
> [Anterior: Creación de un servicio gRPC Greeter](xref:tutorials/grpc/grpc-start)
