---
page_type: sample
description: En este tutorial se le mostrará cómo crear un servicio gRPC y un cliente gRPC en ASP.NET Core.
languages:
- csharp
products:
- dotnet-core
- aspnet-core
- vs
urlFragment: create-grpc-client
ms.openlocfilehash: b9feb9eed62177358fffc0d7da582f625a431e32
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "78648161"
---
# <a name="create-a-grpc-client-and-server-in-aspnet-core-30-using-visual-studio"></a>Creación de un servidor y un cliente gRPC en ASP.NET Core 3.0 con Visual Studio

En este tutorial se muestra cómo crear un cliente [gRPC](https://grpc.io/docs/guides/) de .NET Core y un servidor gRPC de ASP.NET Core.

Al final tendrá un cliente gRPC que se comunica con el servicio Greeter de gRPC.

Las tareas de este tutorial son:

* Crear un servicio gRPC.
* Crear un cliente gRPC.
* Probar el servicio cliente gRPC con el servicio gRPC Greeter.

## <a name="create-a-grpc-service"></a>Crear un servicio gRPC

* En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.
* En el cuadro de diálogo **Crear un proyecto nuevo**, seleccione **Aplicación web ASP.NET Core**.
* Seleccione **Siguiente**.
* Llame al proyecto **GrpcGreeter**. Es importante asignarle el nombre *GrpcGreeter* para que los espacios de nombres coincidan al copiar y pegar el código.
* Seleccione **Crear**.
* En el cuadro de diálogo **Crear una aplicación web ASP.NET Core**:
  * Seleccione **.NET Core** y **ASP.NET Core 3.0** en los menús desplegables. 
  * Seleccione la plantilla **Servicio gRPC**.
  * Seleccione **Crear**.

### <a name="run-the-service"></a>Ejecutar el servicio

* Presione `Ctrl+F5` para ejecutar el servicio gRPC sin el depurador.

  Visual Studio ejecuta el servicio en un símbolo del sistema.

Los registros muestran que el servicio está escuchando en `https://localhost:5001`.

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

> [!NOTE]
> La plantilla gRPC está configurada para usar [Seguridad de la capa de transporte (TLS)](https://tools.ietf.org/html/rfc5246). Los clientes de gRPC necesitan usar HTTPS para llamar al servidor.
>
> macOS no admite gRPC de ASP.NET Core con TLS. Se requiere configuración adicional para ejecutar correctamente servicios gRPC en macOS. Para obtener más información, vea [gRPC y ASP.NET Core en macOS](xref:grpc/aspnetcore#grpc-and-aspnet-core-on-macos).

### <a name="examine-the-project-files"></a>Examen de los archivo del proyecto

Archivos de proyecto de *GrpcGreeter*:

* *greet.proto*: El archivo *Protos/greet.proto* define el gRPC `Greeter` y se usa para generar los recursos de servidor gRPC. Para obtener más información, vea [Introducción a gRPC](xref:grpc/index).
* Carpeta *Servicios*: contiene la implementación del servicio `Greeter`.
* *appSettings.json*: contiene datos de configuración, como el protocolo usado por Kestrel. Para obtener más información, vea <xref:fundamentals/configuration/index>.
* *Program.cs*: contiene el punto de entrada para el servicio gRPC. Para obtener más información, vea <xref:fundamentals/host/generic-host>.
* *Startup.cs*: Contiene código que configura el comportamiento de la aplicación. Para obtener más información, vea [Inicio de la aplicación](xref:fundamentals/startup).

## <a name="create-the-grpc-client-in-a-net-console-app"></a>Creación del cliente gRPC en una aplicación de consola de .NET

* Abra una segunda instancia de Visual Studio.
* Seleccione **Archivo** > **Nuevo** > **Proyecto** de la barra de menús.
* En el cuadro de diálogo **Crear un nuevo proyecto**, seleccione **Aplicación de consola (.NET Core)** .
* Seleccione **Siguiente**.
* En el cuadro de texto **Nombre**, escriba "GrpcGreeterClient".
* Seleccione **Crear**.

### <a name="add-required-packages"></a>Adición de paquetes necesarios

El proyecto de cliente gRPC requiere los siguientes paquetes:

* [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), que contiene el cliente de .NET Core.
* [Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), que contiene API de mensajes protobuf para C#.
* [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), que contiene compatibilidad con herramientas de C# para archivos protobuf. El paquete de herramientas no es necesario en el runtime, de modo que la dependencia se marca con `PrivateAssets="All"`.

Instale los paquetes con la Consola del Administrador de paquetes (PMC) o mediante Administrar paquetes NuGet.

#### <a name="pmc-option-to-install-packages"></a>Opción de PMC para instalar paquetes

* En Visual Studio, seleccione **Herramientas** > **Administrador de paquetes de NuGet** > **Consola del Administrador de paquetes**.
* En la ventana de la **Consola del Administrador de paquetes**, desplácese al directorio en el que se encuentra el archivo *GrpcGreeterClient.csproj*.
* Ejecute los comandos siguientes:

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a>Administración de la opción Paquetes NuGet para instalar paquetes

* Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** > **Administrar paquetes NuGet**.
* Seleccione la pestaña **Examinar**.
* Escriba **Grpc.Net.Client** en el cuadro de búsqueda.
* Seleccione el paquete **Grpc.Net.Client** en la pestaña **Examinar** y haga clic en **Instalar**.
* Repita el proceso para `Google.Protobuf` y `Grpc.Tools`.

### <a name="add-greetproto"></a>Adición de greet.proto

* Cree una carpeta **Protos** en el proyecto de cliente gRPC.
* Copie el archivo **Protos\greet.proto** del servicio gRPC Greeter en el proyecto de cliente gRPC.
* Edite el archivo de proyecto *GrpcGreeterClient.csproj*:

  Haga clic con el botón derecho en el proyecto y seleccione **Editar archivo del proyecto**.

* Agregue un grupo de elementos con un elemento `<Protobuf>` que hace referencia al archivo **greet.proto**:

  ```xml
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a>Creación del cliente de Greeter

Compile el proyecto para crear los tipos en el espacio de nombres `GrpcGreeter`. El proceso de compilación genera automáticamente los tipos `GrpcGreeter`.

Actualice el archivo *Program.cs* del cliente gRPC con el código siguiente:

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;
using GrpcGreeter;
using Grpc.Net.Client;

namespace GrpcGreeterClient
{
    class Program
    {
        static async Task Main(string[] args)
        {
            // The port number(5001) must match the port of the gRPC server.
            var channel = GrpcChannel.ForAddress("https://localhost:5001");
            var client = new Greeter.GreeterClient(channel);
            var reply = await client.SayHelloAsync(
                              new HelloRequest { Name = "GreeterClient" });
            Console.WriteLine("Greeting: " + reply.Message);
            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

*Program.cs* contiene el punto de entrada y la lógica para el cliente gRPC.

El cliente de Greeter se crea mediante lo siguiente:

* Creación de una instancia de `GrpcChannel` que contiene la información para crear la conexión al servicio gRPC.
* Mediante `GrpcChannel` para construir el cliente de Greeter.

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a>Prueba del cliente gRPC con el servicio gRPC Greeter

* En el servicio Greeter, presione `Ctrl+F5` para iniciar el servidor sin el depurador.
* En el proyecto `GrpcGreeterClient`, presione `Ctrl+F5` para iniciar el cliente sin el depurador.

El cliente envía un saludo al servicio con un mensaje que contiene su nombre "GreeterClient". El servicio envía el mensaje "Hello GreeterClient" como respuesta. La respuesta "Hello GreeterClient" se muestra en el símbolo del sistema:

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

El servicio gRPC registra los detalles de la llamada correcta en los registros escritos en el símbolo del sistema.

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\GH\aspnet\docs\4\Docs\aspnetcore\tutorials\grpc\grpc-start\sample\GrpcGreeter
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST https://localhost:5001/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 78.32260000000001ms 200 application/grpc
```

### <a name="docs-help--next-steps-for-grpc"></a>Ayuda de documentación y pasos siguientes para gRPC

* [Introducción a gRPC en ASP.NET Core](https://docs.microsoft.com/aspnet/core/grpc/index?view=aspnetcore-3.0)
* [Servicios gRPC con C#](https://docs.microsoft.com/aspnet/core/grpc/basics?view=aspnetcore-3.0)
* [Migración de servicios gRPC de C-core a ASP.NET Core](https://docs.microsoft.com/aspnet/core/grpc/migration?view=aspnetcore-3.0)
