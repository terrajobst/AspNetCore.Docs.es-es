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
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="ee255-104">Tutorial: Introducción al servicio gRPC en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ee255-104">Tutorial: Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="ee255-105">Por [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="ee255-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="ee255-106">En este tutorial se enseñan los conceptos básicos de la compilación de un servicio gRPC en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ee255-106">This tutorial teaches the basics of building a gRPC service on ASP.NET Core.</span></span>

<span data-ttu-id="ee255-107">Al final, tendrá un servicio gRPC que repite saludos.</span><span class="sxs-lookup"><span data-stu-id="ee255-107">At the end, you'll have a gRPC service that echoes greetings.</span></span>

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

<span data-ttu-id="ee255-108">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="ee255-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ee255-109">Creará un servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="ee255-109">Create a gRPC service.</span></span>
> * <span data-ttu-id="ee255-110">Ejecute el servicio.</span><span class="sxs-lookup"><span data-stu-id="ee255-110">Run the service.</span></span>
> * <span data-ttu-id="ee255-111">Examinar los archivos de proyecto.</span><span class="sxs-lookup"><span data-stu-id="ee255-111">Examine the project files.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="ee255-112">Crear un servicio gRPC</span><span class="sxs-lookup"><span data-stu-id="ee255-112">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ee255-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ee255-113">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ee255-114">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="ee255-114">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="ee255-115">Cree una aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ee255-115">Create a new ASP.NET Core Web Application.</span></span>
  <span data-ttu-id="ee255-116">![Nueva aplicación web de ASP.NET Core](grpc-start/_static/np_3_0.1.png)</span><span class="sxs-lookup"><span data-stu-id="ee255-116">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.1.png)</span></span>
* <span data-ttu-id="ee255-117">Llame al proyecto **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="ee255-117">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="ee255-118">Es importante asignarle el nombre *GrpcGreeter* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="ee255-118">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="ee255-119">![Nueva aplicación web de ASP.NET Core](grpc-start/_static/np_3_0.2.png)</span><span class="sxs-lookup"><span data-stu-id="ee255-119">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.2.png)</span></span>
* <span data-ttu-id="ee255-120">Seleccione **.NET Core** y **ASP.NET Core 3.0** en el menú desplegable.</span><span class="sxs-lookup"><span data-stu-id="ee255-120">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown.</span></span> <span data-ttu-id="ee255-121">Elija la plantilla **Servicio gRPC**.</span><span class="sxs-lookup"><span data-stu-id="ee255-121">Choose the **gRPC Service** template.</span></span>

  <span data-ttu-id="ee255-122">Se crea el proyecto de inicio siguiente:</span><span class="sxs-lookup"><span data-stu-id="ee255-122">The following starter project is created:</span></span>

  ![Explorador de soluciones](grpc-start/_static/se3.0.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ee255-124">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ee255-124">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="ee255-125">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="ee255-125">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="ee255-126">Cambie los directorios (`cd`) a una carpeta que contenga el proyecto.</span><span class="sxs-lookup"><span data-stu-id="ee255-126">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="ee255-127">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="ee255-127">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="ee255-128">El comando `dotnet new` crea un nuevo servicio gRPC en la carpeta *GrpcGreeter*.</span><span class="sxs-lookup"><span data-stu-id="ee255-128">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="ee255-129">El comando `code` abre la carpeta *GrpcGreeter* en una nueva instancia de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ee255-129">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="ee255-130">Se muestra un cuadro de diálogo con el texto **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="ee255-130">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="ee255-131">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="ee255-131">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ee255-132">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="ee255-132">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="ee255-133">Desde un terminal, ejecute estos comandos:</span><span class="sxs-lookup"><span data-stu-id="ee255-133">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="ee255-134">Los comandos anteriores utilizan la [CLI de .NET Core](/dotnet/core/tools/dotnet) para crear un servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="ee255-134">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="ee255-135">Abrir el proyecto</span><span class="sxs-lookup"><span data-stu-id="ee255-135">Open the project</span></span>

<span data-ttu-id="ee255-136">En Visual Studio, seleccione **Archivo > Abrir** y elija el archivo *GrpcGreeter.sln*.</span><span class="sxs-lookup"><span data-stu-id="ee255-136">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="test-the-service"></a><span data-ttu-id="ee255-137">Probar el servicio</span><span class="sxs-lookup"><span data-stu-id="ee255-137">Test the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ee255-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ee255-138">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ee255-139">Asegúrese de que **GrpcGreeter.Server** se establece como proyecto de inicio y presione Ctrl+F5 para ejecutar el servicio gRPC sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="ee255-139">Ensure the **GrpcGreeter.Server** is set as the Startup Project and press Ctrl+F5 to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="ee255-140">Visual Studio ejecuta el servicio en un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="ee255-140">Visual Studio runs the service in a command prompt.</span></span> <span data-ttu-id="ee255-141">Los registros muestran que el servicio inició la escucha en `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="ee255-141">The logs show that the service started listening on `http://localhost:50051`.</span></span>

  ![Nueva aplicación web de ASP.NET Core](grpc-start/_static/server_start.png)

* <span data-ttu-id="ee255-143">Una vez que el servicio se encuentre en ejecución, establezca **GrpcGreeter.Client** como proyecto de inicio y presione Ctrl+F5 para ejecutar el cliente sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="ee255-143">Once the service is running, set the **GrpcGreeter.Client** is set as the Startup Project and press Ctrl+F5 to run the client without the debugger.</span></span>

  <span data-ttu-id="ee255-144">El cliente envía un saludo al servicio con un mensaje que contiene su nombre "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="ee255-144">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="ee255-145">El servicio enviará un mensaje "Hola GreeterClient" como respuesta que se muestra en el símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="ee255-145">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

  ![Nueva aplicación web de ASP.NET Core](grpc-start/_static/client.png)

  <span data-ttu-id="ee255-147">El servicio registra los detalles de la llamada correcta en los registros escritos en el símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="ee255-147">The service records the details of the successful call in the logs written to the command prompt.</span></span>

  ![Nueva aplicación web de ASP.NET Core](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="ee255-149">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="ee255-149">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="ee255-150">Ejecute el proyecto de servidor GrpcGreeter.Server desde la línea de comandos mediante `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="ee255-150">Run the Server project GrpcGreeter.Server from the command line using `dotnet run`.</span></span> <span data-ttu-id="ee255-151">Los registros muestran que el servicio inició la escucha en `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="ee255-151">The logs show that the service started listening on `http://localhost:50051`.</span></span>

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

* <span data-ttu-id="ee255-152">Ejecute el proyecto de cliente GrpcGreeter.Server desde la línea de comandos independiente mediante `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="ee255-152">Run the Client project GrpcGreeter.Client from the separate command line using `dotnet run`.</span></span>

<span data-ttu-id="ee255-153">El cliente envía un saludo al servicio con un mensaje que contiene su nombre "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="ee255-153">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="ee255-154">El servicio enviará un mensaje "Hola GreeterClient" como respuesta que se muestra en el símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="ee255-154">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="ee255-155">El servicio registra los detalles de la llamada correcta en los registros escritos en el símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="ee255-155">The service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="examine-the-project-files-of-the-grpc-project"></a><span data-ttu-id="ee255-156">Examinar los archivos de proyecto del proyecto gRPC</span><span class="sxs-lookup"><span data-stu-id="ee255-156">Examine the project files of the gRPC project</span></span>

<span data-ttu-id="ee255-157">Archivos GrpcGreeter.Server:</span><span class="sxs-lookup"><span data-stu-id="ee255-157">GrpcGreeter.Server files:</span></span>

* <span data-ttu-id="ee255-158">greet.proto: El archivo *Protos/greet.proto* define el gRPC `Greeter` y se usa para generar los recursos de servidor gRPC.</span><span class="sxs-lookup"><span data-stu-id="ee255-158">greet.proto: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="ee255-159">Para obtener más información, vea <xref:grpc/index>.</span><span class="sxs-lookup"><span data-stu-id="ee255-159">For more information, see <xref:grpc/index>.</span></span>
* <span data-ttu-id="ee255-160">Carpeta *Servicios*: contiene la implementación del servicio `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="ee255-160">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="ee255-161">*appSettings.json*: contiene datos de configuración, como el protocolo usado por Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ee255-161">*appSettings.json*:Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="ee255-162">Para obtener más información, vea <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="ee255-162">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="ee255-163">*Program.cs*: contiene el punto de entrada para el servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="ee255-163">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="ee255-164">Para obtener más información, vea <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="ee255-164">For more information, see <xref:fundamentals/host/web-host>.</span></span>
* <span data-ttu-id="ee255-165">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="ee255-165">Startup.cs</span></span>

<span data-ttu-id="ee255-166">Contiene código que configura el comportamiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ee255-166">Contains code that configures app behavior.</span></span> <span data-ttu-id="ee255-167">Para obtener más información, vea <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="ee255-167">For more information, see <xref:fundamentals/startup>.</span></span>

<span data-ttu-id="ee255-168">Archivo GrpcGreeter.Client de cliente gRPC:</span><span class="sxs-lookup"><span data-stu-id="ee255-168">gRPC client GrpcGreeter.Client file:</span></span>

<span data-ttu-id="ee255-169">*Program.cs* contiene el punto de entrada y la lógica para el cliente gRPC.</span><span class="sxs-lookup"><span data-stu-id="ee255-169">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="ee255-170">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="ee255-170">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ee255-171">Creado un servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="ee255-171">Created a gRPC service.</span></span>
> * <span data-ttu-id="ee255-172">Ejecutado el servicio y un cliente para probar el servicio.</span><span class="sxs-lookup"><span data-stu-id="ee255-172">Ran the service and a client to test the service.</span></span>
> * <span data-ttu-id="ee255-173">Examinado los archivo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="ee255-173">Examined the project files.</span></span>
