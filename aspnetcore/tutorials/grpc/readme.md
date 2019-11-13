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
ms.sourcegitcommit: 9e85c2562df5e108d7933635c830297f484bb775
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73463050"
---
# <a name="create-a-grpc-client-and-server-in-aspnet-core-30-using-visual-studio"></a><span data-ttu-id="1a7b7-102">Creación de un servidor y un cliente gRPC en ASP.NET Core 3.0 con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1a7b7-102">Create a gRPC client and server in ASP.NET Core 3.0 using Visual Studio</span></span>

<span data-ttu-id="1a7b7-103">En este tutorial se muestra cómo crear un cliente [gRPC](https://grpc.io/docs/guides/) de .NET Core y un servidor gRPC de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-103">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="1a7b7-104">Al final tendrá un cliente gRPC que se comunica con el servicio Greeter de gRPC.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-104">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="1a7b7-105">Las tareas de este tutorial son:</span><span class="sxs-lookup"><span data-stu-id="1a7b7-105">In this tutorial, you;</span></span>

* <span data-ttu-id="1a7b7-106">Crear un servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-106">Create a gRPC Server.</span></span>
* <span data-ttu-id="1a7b7-107">Crear un cliente gRPC.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-107">Create a gRPC client.</span></span>
* <span data-ttu-id="1a7b7-108">Probar el servicio cliente gRPC con el servicio gRPC Greeter.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-108">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="create-a-grpc-service"></a><span data-ttu-id="1a7b7-109">Crear un servicio gRPC</span><span class="sxs-lookup"><span data-stu-id="1a7b7-109">Create a gRPC service</span></span>

* <span data-ttu-id="1a7b7-110">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-110">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="1a7b7-111">En el cuadro de diálogo **Crear un proyecto nuevo**, seleccione **Aplicación web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-111">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="1a7b7-112">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-112">Select **Next**</span></span>
* <span data-ttu-id="1a7b7-113">Llame al proyecto **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-113">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="1a7b7-114">Es importante asignarle el nombre *GrpcGreeter* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-114">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="1a7b7-115">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-115">Select **Create**</span></span>
* <span data-ttu-id="1a7b7-116">En el cuadro de diálogo **Crear una aplicación web ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="1a7b7-116">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="1a7b7-117">Seleccione **.NET Core** y **ASP.NET Core 3.0** en los menús desplegables.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-117">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="1a7b7-118">Seleccione la plantilla **Servicio gRPC**.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-118">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="1a7b7-119">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-119">Select **Create**</span></span>

### <a name="run-the-service"></a><span data-ttu-id="1a7b7-120">Ejecutar el servicio</span><span class="sxs-lookup"><span data-stu-id="1a7b7-120">Run the service</span></span>

* <span data-ttu-id="1a7b7-121">Presione `Ctrl+F5` para ejecutar el servicio gRPC sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-121">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="1a7b7-122">Visual Studio ejecuta el servicio en un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-122">Visual Studio runs the service in a command prompt.</span></span>

<span data-ttu-id="1a7b7-123">Los registros muestran que el servicio está escuchando en `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-123">The logs show the service listening on `https://localhost:5001`.</span></span>

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
> <span data-ttu-id="1a7b7-124">La plantilla gRPC está configurada para usar [Seguridad de la capa de transporte (TLS)](https://tools.ietf.org/html/rfc5246).</span><span class="sxs-lookup"><span data-stu-id="1a7b7-124">The gRPC template is configured to use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="1a7b7-125">Los clientes de gRPC necesitan usar HTTPS para llamar al servidor.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-125">gRPC clients need to use HTTPS to call the server.</span></span>
>
> <span data-ttu-id="1a7b7-126">macOS no admite gRPC de ASP.NET Core con TLS.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-126">macOS doesn't support ASP.NET Core gRPC with TLS.</span></span> <span data-ttu-id="1a7b7-127">Se requiere configuración adicional para ejecutar correctamente servicios gRPC en macOS.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-127">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="1a7b7-128">Para obtener más información, vea [gRPC y ASP.NET Core en macOS](xref:grpc/aspnetcore#grpc-and-aspnet-core-on-macos).</span><span class="sxs-lookup"><span data-stu-id="1a7b7-128">For more information, see [gRPC and ASP.NET Core on macOS](xref:grpc/aspnetcore#grpc-and-aspnet-core-on-macos).</span></span>

### <a name="examine-the-project-files"></a><span data-ttu-id="1a7b7-129">Examen de los archivo del proyecto</span><span class="sxs-lookup"><span data-stu-id="1a7b7-129">Examine the project files</span></span>

<span data-ttu-id="1a7b7-130">Archivos de proyecto de *GrpcGreeter*:</span><span class="sxs-lookup"><span data-stu-id="1a7b7-130">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="1a7b7-131">*greet.proto*: El archivo *Protos/greet.proto* define el gRPC `Greeter` y se usa para generar los recursos de servidor gRPC.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-131">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="1a7b7-132">Para obtener más información, vea [Introducción a gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="1a7b7-132">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="1a7b7-133">Carpeta *Servicios*: contiene la implementación del servicio `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-133">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="1a7b7-134">*appSettings.json*: contiene datos de configuración, como el protocolo usado por Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-134">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="1a7b7-135">Para más información, consulte <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-135">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="1a7b7-136">*Program.cs*: contiene el punto de entrada para el servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-136">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="1a7b7-137">Para más información, consulte <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-137">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="1a7b7-138">*Startup.cs*: Contiene código que configura el comportamiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-138">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="1a7b7-139">Para obtener más información, vea [Inicio de la aplicación](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="1a7b7-139">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="1a7b7-140">Creación del cliente gRPC en una aplicación de consola de .NET</span><span class="sxs-lookup"><span data-stu-id="1a7b7-140">Create the gRPC client in a .NET console app</span></span>

* <span data-ttu-id="1a7b7-141">Abra una segunda instancia de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-141">Open a second instance of Visual Studio.</span></span>
* <span data-ttu-id="1a7b7-142">Seleccione **Archivo** > **Nuevo** > **Proyecto** de la barra de menús.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-142">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="1a7b7-143">En el cuadro de diálogo **Crear un nuevo proyecto**, seleccione **Aplicación de consola (.NET Core)** .</span><span class="sxs-lookup"><span data-stu-id="1a7b7-143">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="1a7b7-144">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-144">Select **Next**</span></span>
* <span data-ttu-id="1a7b7-145">En el cuadro de texto **Nombre**, escriba "GrpcGreeterClient".</span><span class="sxs-lookup"><span data-stu-id="1a7b7-145">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="1a7b7-146">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-146">Select **Create**.</span></span>

### <a name="add-required-packages"></a><span data-ttu-id="1a7b7-147">Adición de paquetes necesarios</span><span class="sxs-lookup"><span data-stu-id="1a7b7-147">Add required packages</span></span>

<span data-ttu-id="1a7b7-148">El proyecto de cliente gRPC requiere los siguientes paquetes:</span><span class="sxs-lookup"><span data-stu-id="1a7b7-148">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="1a7b7-149">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), que contiene el cliente de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-149">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="1a7b7-150">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), que contiene API de mensajes protobuf para C#.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-150">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="1a7b7-151">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), que contiene compatibilidad con herramientas de C# para archivos protobuf.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-151">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="1a7b7-152">El paquete de herramientas no es necesario en el runtime, de modo que la dependencia se marca con `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-152">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

<span data-ttu-id="1a7b7-153">Instale los paquetes con la Consola del Administrador de paquetes (PMC) o mediante Administrar paquetes NuGet.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-153">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="1a7b7-154">Opción de PMC para instalar paquetes</span><span class="sxs-lookup"><span data-stu-id="1a7b7-154">PMC option to install packages</span></span>

* <span data-ttu-id="1a7b7-155">En Visual Studio, seleccione **Herramientas** > **Administrador de paquetes de NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-155">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="1a7b7-156">En la ventana de la **Consola del Administrador de paquetes**, desplácese al directorio en el que se encuentra el archivo *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-156">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="1a7b7-157">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="1a7b7-157">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="1a7b7-158">Administración de la opción Paquetes NuGet para instalar paquetes</span><span class="sxs-lookup"><span data-stu-id="1a7b7-158">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="1a7b7-159">Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** > **Administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-159">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="1a7b7-160">Seleccione la pestaña **Examinar**.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-160">Select the **Browse** tab.</span></span>
* <span data-ttu-id="1a7b7-161">Escriba **Grpc.Net.Client** en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-161">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="1a7b7-162">Seleccione el paquete **Grpc.Net.Client** en la pestaña **Examinar** y haga clic en **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-162">Select the **Grpc.Net.Client** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="1a7b7-163">Repita el proceso para `Google.Protobuf` y `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-163">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="add-greetproto"></a><span data-ttu-id="1a7b7-164">Adición de greet.proto</span><span class="sxs-lookup"><span data-stu-id="1a7b7-164">Add greet.proto</span></span>

* <span data-ttu-id="1a7b7-165">Cree una carpeta **Protos** en el proyecto de cliente gRPC.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-165">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="1a7b7-166">Copie el archivo **Protos\greet.proto** del servicio gRPC Greeter en el proyecto de cliente gRPC.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-166">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="1a7b7-167">Edite el archivo de proyecto *GrpcGreeterClient.csproj*:</span><span class="sxs-lookup"><span data-stu-id="1a7b7-167">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  <span data-ttu-id="1a7b7-168">Haga clic con el botón derecho en el proyecto y seleccione **Editar archivo del proyecto**.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-168">Right-click the project and select **Edit Project File**.</span></span>

* <span data-ttu-id="1a7b7-169">Agregue un grupo de elementos con un elemento `<Protobuf>` que hace referencia al archivo **greet.proto**:</span><span class="sxs-lookup"><span data-stu-id="1a7b7-169">Add an item group with a `<Protobuf>` element that refers to the **greet.proto** file:</span></span>

  ```xml
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="1a7b7-170">Creación del cliente de Greeter</span><span class="sxs-lookup"><span data-stu-id="1a7b7-170">Create the Greeter client</span></span>

<span data-ttu-id="1a7b7-171">Compile el proyecto para crear los tipos en el espacio de nombres `GrpcGreeter`.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-171">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="1a7b7-172">El proceso de compilación genera automáticamente los tipos `GrpcGreeter`.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-172">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="1a7b7-173">Actualice el archivo *Program.cs* del cliente gRPC con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="1a7b7-173">Update the gRPC client *Program.cs* file with the following code:</span></span>

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

<span data-ttu-id="1a7b7-174">*Program.cs* contiene el punto de entrada y la lógica para el cliente gRPC.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-174">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="1a7b7-175">El cliente de Greeter se crea mediante lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1a7b7-175">The Greeter client is created by:</span></span>

* <span data-ttu-id="1a7b7-176">Creación de una instancia de `GrpcChannel` que contiene la información para crear la conexión al servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-176">Instantiating a `GrpcChannel` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="1a7b7-177">Mediante `GrpcChannel` para construir el cliente de Greeter.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-177">Using the `GrpcChannel` to construct the Greeter client.</span></span>

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="1a7b7-178">Prueba del cliente gRPC con el servicio gRPC Greeter</span><span class="sxs-lookup"><span data-stu-id="1a7b7-178">Test the gRPC client with the gRPC Greeter service</span></span>

* <span data-ttu-id="1a7b7-179">En el servicio Greeter, presione `Ctrl+F5` para iniciar el servidor sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-179">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="1a7b7-180">En el proyecto `GrpcGreeterClient`, presione `Ctrl+F5` para iniciar el cliente sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-180">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the client without the debugger.</span></span>

<span data-ttu-id="1a7b7-181">El cliente envía un saludo al servicio con un mensaje que contiene su nombre "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="1a7b7-181">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="1a7b7-182">El servicio envía el mensaje "Hello GreeterClient" como respuesta.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-182">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="1a7b7-183">La respuesta "Hello GreeterClient" se muestra en el símbolo del sistema:</span><span class="sxs-lookup"><span data-stu-id="1a7b7-183">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="1a7b7-184">El servicio gRPC registra los detalles de la llamada correcta en los registros escritos en el símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="1a7b7-184">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="docs-help--next-steps-for-grpc"></a><span data-ttu-id="1a7b7-185">Ayuda de documentación y pasos siguientes para gRPC</span><span class="sxs-lookup"><span data-stu-id="1a7b7-185">Docs help & next steps for gRPC</span></span>

* [<span data-ttu-id="1a7b7-186">Introducción a gRPC en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1a7b7-186">Introduction to gRPC on ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/grpc/index?view=aspnetcore-3.0)
* [<span data-ttu-id="1a7b7-187">Servicios gRPC con C#</span><span class="sxs-lookup"><span data-stu-id="1a7b7-187">gRPC services with C#</span></span>](https://docs.microsoft.com/aspnet/core/grpc/basics?view=aspnetcore-3.0)
* [<span data-ttu-id="1a7b7-188">Migración de servicios gRPC de C-core a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1a7b7-188">Migrating gRPC services from C-core to ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/grpc/migration?view=aspnetcore-3.0)
