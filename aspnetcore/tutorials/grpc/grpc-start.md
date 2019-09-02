---
title: Creación de un servidor y un cliente gRPC en ASP.NET Core
author: juntaoluo
description: En este tutorial se le mostrará cómo crear un servicio gRPC y un cliente gRPC en ASP.NET Core. Aprenda a crear un proyecto de servicio gRPC, edite un archivo proto y agregue una llamada de streaming dúplex.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 8/26/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: ffe0034f1d33fb9282b39c030421b9b307413cdb
ms.sourcegitcommit: de17150e5ec7507d7114dde0e5dbc2e45a66ef53
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70113277"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="de63d-104">Tutorial: Crear un servidor y un cliente gRPC en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="de63d-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="de63d-105">Por [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="de63d-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="de63d-106">En este tutorial se muestra cómo crear un cliente [gRPC](https://grpc.io/docs/guides/) de .NET Core y un servidor gRPC de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="de63d-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="de63d-107">Al final tendrá un cliente gRPC que se comunica con el servicio Greeter de gRPC.</span><span class="sxs-lookup"><span data-stu-id="de63d-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="de63d-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="de63d-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="de63d-109">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="de63d-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="de63d-110">Crear un servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="de63d-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="de63d-111">Crear un cliente gRPC.</span><span class="sxs-lookup"><span data-stu-id="de63d-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="de63d-112">Probar el servicio cliente gRPC con el servicio gRPC Greeter.</span><span class="sxs-lookup"><span data-stu-id="de63d-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="de63d-113">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="de63d-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="de63d-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de63d-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="de63d-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de63d-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="de63d-116">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="de63d-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-grpc-service"></a><span data-ttu-id="de63d-117">Crear un servicio gRPC</span><span class="sxs-lookup"><span data-stu-id="de63d-117">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="de63d-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de63d-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="de63d-119">Inicie Visual Studio y seleccione **Crear un proyecto**.</span><span class="sxs-lookup"><span data-stu-id="de63d-119">Start Visual Studio and select **Create a new project**.</span></span> <span data-ttu-id="de63d-120">Alternativamente, en el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="de63d-120">Alternatively, from the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="de63d-121">En el cuadro de diálogo **Crear un proyecto**, seleccione **Servicio gRPC** y seleccione **Siguiente**:</span><span class="sxs-lookup"><span data-stu-id="de63d-121">In the **Create a new project** dialog, select **gPRC Service** and select **Next**:</span></span>

  ![Cuadro de diálogo **Crear un proyecto**](~/tutorials/grpc/grpc-start/static/cnp.png)

* <span data-ttu-id="de63d-123">Llame al proyecto **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="de63d-123">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="de63d-124">Es importante asignarle el nombre *GrpcGreeter* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="de63d-124">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="de63d-125">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="de63d-125">Select **Create**.</span></span>
* <span data-ttu-id="de63d-126">En el cuadro de diálogo **Crear un servicio gRPC**:</span><span class="sxs-lookup"><span data-stu-id="de63d-126">In the **Create a new gRPC service** dialog:</span></span>
  * <span data-ttu-id="de63d-127">Se selecciona la plantilla **Servicio gRPC**.</span><span class="sxs-lookup"><span data-stu-id="de63d-127">The **gRPC Service** template is selected.</span></span>
  * <span data-ttu-id="de63d-128">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="de63d-128">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="de63d-129">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de63d-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="de63d-130">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="de63d-130">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="de63d-131">Cambie los directorios (`cd`) a una carpeta que contenga el proyecto.</span><span class="sxs-lookup"><span data-stu-id="de63d-131">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="de63d-132">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="de63d-132">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="de63d-133">El comando `dotnet new` crea un nuevo servicio gRPC en la carpeta *GrpcGreeter*.</span><span class="sxs-lookup"><span data-stu-id="de63d-133">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="de63d-134">El comando `code` abre la carpeta *GrpcGreeter* en una nueva instancia de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="de63d-134">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="de63d-135">Se muestra un cuadro de diálogo con el texto **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="de63d-135">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="de63d-136">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="de63d-136">Select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="de63d-137">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="de63d-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="de63d-138">Desde un terminal, ejecute estos comandos:</span><span class="sxs-lookup"><span data-stu-id="de63d-138">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="de63d-139">Los comandos anteriores utilizan la [CLI de .NET Core](/dotnet/core/tools/dotnet) para crear un servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="de63d-139">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="de63d-140">Abrir el proyecto</span><span class="sxs-lookup"><span data-stu-id="de63d-140">Open the project</span></span>

<span data-ttu-id="de63d-141">En Visual Studio, seleccione **Archivo** > **Abrir** y seleccione el archivo *GrpcGreeter.sln*.</span><span class="sxs-lookup"><span data-stu-id="de63d-141">From Visual Studio, select **File** > **Open**, and then select the *GrpcGreeter.sln* file.</span></span>

---

### <a name="run-the-service"></a><span data-ttu-id="de63d-142">Ejecutar el servicio</span><span class="sxs-lookup"><span data-stu-id="de63d-142">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="de63d-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de63d-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="de63d-144">Presione `Ctrl+F5` para ejecutar el servicio gRPC sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="de63d-144">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="de63d-145">Visual Studio ejecuta el servicio en un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="de63d-145">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="de63d-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de63d-146">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="de63d-147">Ejecute el proyecto *GrpcGreeter* de gRPC Greeter desde la línea de comandos mediante `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="de63d-147">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="de63d-148">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="de63d-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="de63d-149">Ejecute el proyecto *GrpcGreeter* de gRPC Greeter desde la línea de comandos mediante `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="de63d-149">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

---

<span data-ttu-id="de63d-150">Los registros muestran que el servicio está escuchando en `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="de63d-150">The logs show the service listening on `https://localhost:5001`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

> [!NOTE]
> <span data-ttu-id="de63d-151">La plantilla gRPC está configurada para usar [Seguridad de la capa de transporte (TLS)](https://tools.ietf.org/html/rfc5246).</span><span class="sxs-lookup"><span data-stu-id="de63d-151">The gRPC template is configured to use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="de63d-152">Los clientes de gRPC necesitan usar HTTPS para llamar al servidor.</span><span class="sxs-lookup"><span data-stu-id="de63d-152">gRPC clients need to use HTTPS to call the server.</span></span>
>
> <span data-ttu-id="de63d-153">macOS no admite gRPC de ASP.NET Core con TLS.</span><span class="sxs-lookup"><span data-stu-id="de63d-153">macOS doesn't support ASP.NET Core gRPC with TLS.</span></span> <span data-ttu-id="de63d-154">Se requiere configuración adicional para ejecutar correctamente servicios gRPC en macOS.</span><span class="sxs-lookup"><span data-stu-id="de63d-154">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="de63d-155">Para obtener más información, vea [No se puede iniciar la aplicación gRPC de ASP.NET Core en macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span><span class="sxs-lookup"><span data-stu-id="de63d-155">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

### <a name="examine-the-project-files"></a><span data-ttu-id="de63d-156">Examen de los archivo del proyecto</span><span class="sxs-lookup"><span data-stu-id="de63d-156">Examine the project files</span></span>

<span data-ttu-id="de63d-157">Archivos de proyecto de *GrpcGreeter*:</span><span class="sxs-lookup"><span data-stu-id="de63d-157">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="de63d-158">*greet.proto* &ndash; el archivo *Protos/greet.proto* define el gRPC `Greeter` y se usa para generar los recursos de servidor gRPC.</span><span class="sxs-lookup"><span data-stu-id="de63d-158">*greet.proto* &ndash; The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="de63d-159">Para obtener más información, vea [Introducción a gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="de63d-159">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="de63d-160">Carpeta *Servicios*: contiene la implementación del servicio `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="de63d-160">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="de63d-161">*appSettings.json* &ndash; contiene datos de configuración, como el protocolo usado por Kestrel.</span><span class="sxs-lookup"><span data-stu-id="de63d-161">*appSettings.json* &ndash; Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="de63d-162">Para más información, consulte <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="de63d-162">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="de63d-163">*Program.cs* &ndash; contiene el punto de entrada para el servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="de63d-163">*Program.cs* &ndash; Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="de63d-164">Para más información, consulte <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="de63d-164">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="de63d-165">*Startup.cs* &ndash; contiene código que configura el comportamiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="de63d-165">*Startup.cs* &ndash; Contains code that configures app behavior.</span></span> <span data-ttu-id="de63d-166">Para obtener más información, vea [Inicio de la aplicación](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="de63d-166">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="de63d-167">Creación del cliente gRPC en una aplicación de consola de .NET</span><span class="sxs-lookup"><span data-stu-id="de63d-167">Create the gRPC client in a .NET console app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="de63d-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de63d-168">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="de63d-169">Abra una segunda instancia de Visual Studio y seleccione **Crear un proyecto**.</span><span class="sxs-lookup"><span data-stu-id="de63d-169">Open a second instance of Visual Studio and select **Create a new project**.</span></span>
* <span data-ttu-id="de63d-170">En el cuadro de diálogo **Crear un proyecto**, seleccione **Aplicación de consola (.NET Core)** y elija **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="de63d-170">In the **Create a new project** dialog, select **Console App (.NET Core)** and select **Next**.</span></span>
* <span data-ttu-id="de63d-171">En el cuadro de texto **Nombre**, escriba **GrpcGreeterClient** y seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="de63d-171">In the **Name** text box, enter **GrpcGreeterClient** and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="de63d-172">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de63d-172">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="de63d-173">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="de63d-173">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="de63d-174">Cambie los directorios (`cd`) a una carpeta que contenga el proyecto.</span><span class="sxs-lookup"><span data-stu-id="de63d-174">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="de63d-175">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="de63d-175">Run the following commands:</span></span>

  ```console
  dotnet new console -o GrpcGreeterClient
  code -r GrpcGreeterClient
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="de63d-176">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="de63d-176">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="de63d-177">Siga las instrucciones de [Creación de una solución completa de .NET Core en macOS con Visual Studio para Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution) para crear una aplicación de consola con el nombre *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="de63d-177">Follow the instructions in [Building a complete .NET Core solution on macOS using Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

---

### <a name="add-required-packages"></a><span data-ttu-id="de63d-178">Adición de paquetes necesarios</span><span class="sxs-lookup"><span data-stu-id="de63d-178">Add required packages</span></span>

<span data-ttu-id="de63d-179">El proyecto de cliente gRPC requiere los siguientes paquetes:</span><span class="sxs-lookup"><span data-stu-id="de63d-179">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="de63d-180">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), que contiene el cliente de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="de63d-180">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="de63d-181">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), que contiene API de mensajes protobuf para C#.</span><span class="sxs-lookup"><span data-stu-id="de63d-181">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="de63d-182">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), que contiene compatibilidad con herramientas de C# para archivos protobuf.</span><span class="sxs-lookup"><span data-stu-id="de63d-182">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="de63d-183">El paquete de herramientas no es necesario en el runtime, de modo que la dependencia se marca con `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="de63d-183">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="de63d-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de63d-184">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="de63d-185">Instale los paquetes con la Consola del Administrador de paquetes (PMC) o mediante Administrar paquetes NuGet.</span><span class="sxs-lookup"><span data-stu-id="de63d-185">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="de63d-186">Opción de PMC para instalar paquetes</span><span class="sxs-lookup"><span data-stu-id="de63d-186">PMC option to install packages</span></span>

* <span data-ttu-id="de63d-187">En Visual Studio, seleccione **Herramientas** > **Administrador de paquetes de NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="de63d-187">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="de63d-188">En la ventana **Consola del Administrador de paquetes**, ejecute `cd GrpcGreeterClient` para cambiar los directorios a la carpeta que contiene los archivos *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="de63d-188">From the **Package Manager Console** window, run `cd GrpcGreeterClient` to change directories to the folder containing the *GrpcGreeterClient.csproj* files.</span></span>
* <span data-ttu-id="de63d-189">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="de63d-189">Run the following commands:</span></span>

  ```powershell
  Install-Package Grpc.Net.Client -prerelease
  Install-Package Google.Protobuf -prerelease
  Install-Package Grpc.Tools -prerelease
  ```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="de63d-190">Administración de la opción Paquetes NuGet para instalar paquetes</span><span class="sxs-lookup"><span data-stu-id="de63d-190">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="de63d-191">Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** > **Administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="de63d-191">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="de63d-192">Seleccione la pestaña **Examinar**.</span><span class="sxs-lookup"><span data-stu-id="de63d-192">Select the **Browse** tab.</span></span>
* <span data-ttu-id="de63d-193">Escriba **Grpc.Net.Client** en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="de63d-193">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="de63d-194">Seleccione el paquete **Grpc.Net.Client** en la pestaña **Examinar** y haga clic en **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="de63d-194">Select the **Grpc.Net.Client** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="de63d-195">Repita el proceso para `Google.Protobuf` y `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="de63d-195">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="de63d-196">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de63d-196">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="de63d-197">Ejecute los siguientes comandos en el **terminal integrado**:</span><span class="sxs-lookup"><span data-stu-id="de63d-197">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="de63d-198">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="de63d-198">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="de63d-199">Haga clic con el botón derecho en la carpeta **Paquetes**, en **Panel de solución** > **Agregar paquetes**.</span><span class="sxs-lookup"><span data-stu-id="de63d-199">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="de63d-200">Escriba **Grpc.Net.Client** en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="de63d-200">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="de63d-201">Seleccione el paquete **Grpc.Net.Client** en el panel de resultados y seleccione **Agregar paquete**.</span><span class="sxs-lookup"><span data-stu-id="de63d-201">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="de63d-202">Repita el proceso para `Google.Protobuf` y `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="de63d-202">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="de63d-203">Adición de greet.proto</span><span class="sxs-lookup"><span data-stu-id="de63d-203">Add greet.proto</span></span>

* <span data-ttu-id="de63d-204">Cree una carpeta *Protos* en el proyecto de cliente gRPC.</span><span class="sxs-lookup"><span data-stu-id="de63d-204">Create a *Protos* folder in the gRPC client project.</span></span>
* <span data-ttu-id="de63d-205">Copie el archivo *Protos\greet.proto* del servicio gRPC Greeter en el proyecto de cliente gRPC.</span><span class="sxs-lookup"><span data-stu-id="de63d-205">Copy the *Protos\greet.proto* file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="de63d-206">Edite el archivo de proyecto *GrpcGreeterClient.csproj*:</span><span class="sxs-lookup"><span data-stu-id="de63d-206">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="de63d-207">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de63d-207">Visual Studio</span></span>](#tab/visual-studio)

  <span data-ttu-id="de63d-208">Haga clic con el botón derecho en el proyecto y seleccione **Editar archivo del proyecto**.</span><span class="sxs-lookup"><span data-stu-id="de63d-208">Right-click the project and select **Edit Project File**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="de63d-209">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de63d-209">Visual Studio Code</span></span>](#tab/visual-studio-code)

  <span data-ttu-id="de63d-210">Seleccione el archivo *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="de63d-210">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="de63d-211">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="de63d-211">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="de63d-212">Haga clic con el botón derecho en el proyecto y seleccione **Herramientas** > **Editar archivo**.</span><span class="sxs-lookup"><span data-stu-id="de63d-212">Right-click the project and select **Tools** > **Edit File**.</span></span>

  ---

* <span data-ttu-id="de63d-213">Agregue un grupo de elementos con un elemento `<Protobuf>` que hace referencia al archivo *greet.proto*:</span><span class="sxs-lookup"><span data-stu-id="de63d-213">Add an item group with a `<Protobuf>` element that refers to the *greet.proto* file:</span></span>

  ```xml
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="de63d-214">Creación del cliente de Greeter</span><span class="sxs-lookup"><span data-stu-id="de63d-214">Create the Greeter client</span></span>

<span data-ttu-id="de63d-215">Compile el proyecto para crear los tipos en el espacio de nombres `GrpcGreeter`.</span><span class="sxs-lookup"><span data-stu-id="de63d-215">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="de63d-216">El proceso de compilación genera automáticamente los tipos `GrpcGreeter`.</span><span class="sxs-lookup"><span data-stu-id="de63d-216">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="de63d-217">Actualice el archivo *Program.cs* del cliente gRPC con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="de63d-217">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="de63d-218">*Program.cs* contiene el punto de entrada y la lógica para el cliente gRPC.</span><span class="sxs-lookup"><span data-stu-id="de63d-218">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="de63d-219">El cliente de Greeter se crea mediante lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="de63d-219">The Greeter client is created by:</span></span>

* <span data-ttu-id="de63d-220">Creación de una instancia de `HttpClient` que contiene la información para crear la conexión al servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="de63d-220">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="de63d-221">Uso de `HttpClient` para construir el cliente de Greeter:</span><span class="sxs-lookup"><span data-stu-id="de63d-221">Using the `HttpClient` to construct the Greeter client:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="de63d-222">El cliente de Greeter realiza una llamada al método `SayHello` asincrónico.</span><span class="sxs-lookup"><span data-stu-id="de63d-222">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="de63d-223">Se muestra el resultado de la llamada a `SayHello`:</span><span class="sxs-lookup"><span data-stu-id="de63d-223">The result of the `SayHello` call is displayed:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-9)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="de63d-224">Prueba del cliente gRPC con el servicio gRPC Greeter</span><span class="sxs-lookup"><span data-stu-id="de63d-224">Test the gRPC client with the gRPC Greeter service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="de63d-225">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de63d-225">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="de63d-226">En el servicio Greeter, presione `Ctrl+F5` para iniciar el servidor sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="de63d-226">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="de63d-227">En el proyecto `GrpcGreeterClient`, presione `Ctrl+F5` para iniciar el cliente sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="de63d-227">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the client without the debugger.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="de63d-228">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de63d-228">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="de63d-229">Inicie el servicio Greeter.</span><span class="sxs-lookup"><span data-stu-id="de63d-229">Start the Greeter service.</span></span>
* <span data-ttu-id="de63d-230">Inicie el cliente.</span><span class="sxs-lookup"><span data-stu-id="de63d-230">Start the client.</span></span>


# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="de63d-231">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="de63d-231">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="de63d-232">Inicie el servicio Greeter.</span><span class="sxs-lookup"><span data-stu-id="de63d-232">Start the Greeter service.</span></span>
* <span data-ttu-id="de63d-233">Inicie el cliente.</span><span class="sxs-lookup"><span data-stu-id="de63d-233">Start the client.</span></span>

---

<span data-ttu-id="de63d-234">El cliente envía un saludo al servicio con un mensaje que contiene su nombre *GreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="de63d-234">The client sends a greeting to the service with a message containing its name, *GreeterClient*.</span></span> <span data-ttu-id="de63d-235">El servicio envía el mensaje "Hello GreeterClient" como respuesta.</span><span class="sxs-lookup"><span data-stu-id="de63d-235">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="de63d-236">La respuesta "Hello GreeterClient" se muestra en el símbolo del sistema:</span><span class="sxs-lookup"><span data-stu-id="de63d-236">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="de63d-237">El servicio gRPC registra los detalles de la llamada correcta en los registros escritos en el símbolo del sistema:</span><span class="sxs-lookup"><span data-stu-id="de63d-237">The gRPC service records the details of the successful call in the logs written to the command prompt:</span></span>

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

> [!NOTE]
> <span data-ttu-id="de63d-238">El código de este artículo requiere el certificado de desarrollo de .NET Core HTTPS para proteger el servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="de63d-238">The code in this article requires the ASP.NET Core HTTPS development certificate to secure the gRPC service.</span></span> <span data-ttu-id="de63d-239">Si se produce un error con el mensaje `The remote certificate is invalid according to the validation procedure.` en el cliente, el certificado de desarrollo no es de confianza.</span><span class="sxs-lookup"><span data-stu-id="de63d-239">If the client fails with the message `The remote certificate is invalid according to the validation procedure.`, the development certificate is not trusted.</span></span> <span data-ttu-id="de63d-240">Para instrucciones sobre cómo corregir este problema, consulte [Confiar en el certificado de desarrollo de ASP.NET Core HTTPS en Windows y macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span><span class="sxs-lookup"><span data-stu-id="de63d-240">For instructions to fix this issue, see [Trust the ASP.NET Core HTTPS development certificate on Windows and macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span></span>

### <a name="next-steps"></a><span data-ttu-id="de63d-241">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="de63d-241">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
