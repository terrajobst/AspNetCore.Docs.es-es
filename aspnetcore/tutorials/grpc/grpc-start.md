---
title: Creación de un servidor y un cliente gRPC en ASP.NET Core
author: juntaoluo
description: En este tutorial se le mostrará cómo crear un servicio gRPC y un cliente gRPC en ASP.NET Core. Aprenda a crear un proyecto de servicio gRPC, edite un archivo proto y agregue una llamada de streaming dúplex.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 06/12/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 8507c3dcefeb61a4dd34a1ff967c082d287cf646
ms.sourcegitcommit: d6e51c60439f03a8992bda70cc982ddb15d3f100
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555894"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="f8e27-104">Tutorial: Crear un servidor y un cliente gRPC en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f8e27-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="f8e27-105">Por [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="f8e27-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="f8e27-106">En este tutorial se muestra cómo crear un cliente [gRPC](https://grpc.io/docs/guides/) de .NET Core y un servidor gRPC de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f8e27-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="f8e27-107">Al final tendrá un cliente gRPC que se comunica con el servicio Greeter de gRPC.</span><span class="sxs-lookup"><span data-stu-id="f8e27-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="f8e27-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="f8e27-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="f8e27-109">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="f8e27-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f8e27-110">Crear un servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="f8e27-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="f8e27-111">Crear un cliente gRPC.</span><span class="sxs-lookup"><span data-stu-id="f8e27-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="f8e27-112">Probar el servicio cliente gRPC con el servicio gRPC Greeter.</span><span class="sxs-lookup"><span data-stu-id="f8e27-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f8e27-113">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="f8e27-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f8e27-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8e27-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f8e27-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f8e27-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f8e27-116">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="f8e27-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-grpc-service"></a><span data-ttu-id="f8e27-117">Crear un servicio gRPC</span><span class="sxs-lookup"><span data-stu-id="f8e27-117">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f8e27-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8e27-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f8e27-119">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="f8e27-119">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f8e27-120">En el cuadro de diálogo **Crear un proyecto nuevo**, seleccione **Aplicación web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="f8e27-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="f8e27-121">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="f8e27-121">Select **Next**</span></span>
* <span data-ttu-id="f8e27-122">Llame al proyecto **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="f8e27-122">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="f8e27-123">Es importante asignarle el nombre *GrpcGreeter* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="f8e27-123">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="f8e27-124">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="f8e27-124">Select **Create**</span></span>
* <span data-ttu-id="f8e27-125">En el cuadro de diálogo **Crear una aplicación web ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="f8e27-125">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="f8e27-126">Seleccione **.NET Core** y **ASP.NET Core 3.0** en los menús desplegables.</span><span class="sxs-lookup"><span data-stu-id="f8e27-126">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="f8e27-127">Seleccione la plantilla **Servicio gRPC**.</span><span class="sxs-lookup"><span data-stu-id="f8e27-127">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="f8e27-128">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="f8e27-128">Select **Create**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f8e27-129">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f8e27-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f8e27-130">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="f8e27-130">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="f8e27-131">Cambie los directorios (`cd`) a una carpeta que contenga el proyecto.</span><span class="sxs-lookup"><span data-stu-id="f8e27-131">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="f8e27-132">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="f8e27-132">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="f8e27-133">El comando `dotnet new` crea un nuevo servicio gRPC en la carpeta *GrpcGreeter*.</span><span class="sxs-lookup"><span data-stu-id="f8e27-133">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="f8e27-134">El comando `code` abre la carpeta *GrpcGreeter* en una nueva instancia de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="f8e27-134">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="f8e27-135">Se muestra un cuadro de diálogo con el texto **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="f8e27-135">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="f8e27-136">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="f8e27-136">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f8e27-137">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="f8e27-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f8e27-138">Desde un terminal, ejecute estos comandos:</span><span class="sxs-lookup"><span data-stu-id="f8e27-138">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="f8e27-139">Los comandos anteriores utilizan la [CLI de .NET Core](/dotnet/core/tools/dotnet) para crear un servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="f8e27-139">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="f8e27-140">Abrir el proyecto</span><span class="sxs-lookup"><span data-stu-id="f8e27-140">Open the project</span></span>

<span data-ttu-id="f8e27-141">En Visual Studio, seleccione **Archivo > Abrir** y elija el archivo *GrpcGreeter.sln*.</span><span class="sxs-lookup"><span data-stu-id="f8e27-141">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="f8e27-142">Ejecutar el servicio</span><span class="sxs-lookup"><span data-stu-id="f8e27-142">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f8e27-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8e27-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f8e27-144">Presione `Ctrl+F5` para ejecutar el servicio gRPC sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="f8e27-144">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="f8e27-145">Visual Studio ejecuta el servicio en un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="f8e27-145">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f8e27-146">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="f8e27-146">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="f8e27-147">Ejecute el proyecto *GrpcGreeter* de gRPC Greeter desde la línea de comandos mediante `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="f8e27-147">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="f8e27-148">Los registros muestran que el servicio está escuchando en `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="f8e27-148">The logs show the service listening on `http://localhost:50051`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

### <a name="examine-the-project-files"></a><span data-ttu-id="f8e27-149">Examen de los archivo del proyecto</span><span class="sxs-lookup"><span data-stu-id="f8e27-149">Examine the project files</span></span>

<span data-ttu-id="f8e27-150">Archivos de proyecto de *GrpcGreeter*:</span><span class="sxs-lookup"><span data-stu-id="f8e27-150">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="f8e27-151">*greet.proto*: El archivo *Protos/greet.proto* define el gRPC `Greeter` y se usa para generar los recursos de servidor gRPC.</span><span class="sxs-lookup"><span data-stu-id="f8e27-151">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="f8e27-152">Para obtener más información, vea [Introducción a gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="f8e27-152">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="f8e27-153">Carpeta *Servicios*: contiene la implementación del servicio `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="f8e27-153">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="f8e27-154">*appSettings.json*: contiene datos de configuración, como el protocolo usado por Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f8e27-154">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="f8e27-155">Para más información, consulte <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="f8e27-155">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="f8e27-156">*Program.cs*: contiene el punto de entrada para el servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="f8e27-156">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="f8e27-157">Para más información, consulte <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="f8e27-157">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="f8e27-158">*Startup.cs*: Contiene código que configura el comportamiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f8e27-158">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="f8e27-159">Para obtener más información, vea [Inicio de la aplicación](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="f8e27-159">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="f8e27-160">Creación del cliente gRPC en una aplicación de consola de .NET</span><span class="sxs-lookup"><span data-stu-id="f8e27-160">Create the gRPC client in a .NET console app</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f8e27-161">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8e27-161">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f8e27-162">Abra una segunda instancia de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f8e27-162">Open a second instance of Visual Studio.</span></span>
* <span data-ttu-id="f8e27-163">Seleccione **Archivo** > **Nuevo** > **Proyecto** de la barra de menús.</span><span class="sxs-lookup"><span data-stu-id="f8e27-163">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="f8e27-164">En el cuadro de diálogo **Crear un nuevo proyecto**, seleccione **Aplicación de consola (.NET Core)** .</span><span class="sxs-lookup"><span data-stu-id="f8e27-164">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="f8e27-165">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="f8e27-165">Select **Next**</span></span>
* <span data-ttu-id="f8e27-166">En el cuadro de texto **Nombre**, escriba "GrpcGreeterClient".</span><span class="sxs-lookup"><span data-stu-id="f8e27-166">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="f8e27-167">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="f8e27-167">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f8e27-168">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f8e27-168">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f8e27-169">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="f8e27-169">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="f8e27-170">Cambie los directorios (`cd`) a una carpeta que contenga el proyecto.</span><span class="sxs-lookup"><span data-stu-id="f8e27-170">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="f8e27-171">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="f8e27-171">Run the following commands:</span></span>

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f8e27-172">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="f8e27-172">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f8e27-173">Siga las instrucciones que se indican [aquí](/dotnet/core/tutorials/using-on-mac-vs-full-solution) para crear una aplicación de consola con el nombre *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="f8e27-173">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a><span data-ttu-id="f8e27-174">Adición de paquetes necesarios</span><span class="sxs-lookup"><span data-stu-id="f8e27-174">Add required packages</span></span>

<span data-ttu-id="f8e27-175">El proyecto de cliente gRPC requiere los siguientes paquetes:</span><span class="sxs-lookup"><span data-stu-id="f8e27-175">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="f8e27-176">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), que contiene el cliente de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f8e27-176">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="f8e27-177">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), que contiene API de mensajes protobuf para C#.</span><span class="sxs-lookup"><span data-stu-id="f8e27-177">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="f8e27-178">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), que contiene compatibilidad con herramientas de C# para archivos protobuf.</span><span class="sxs-lookup"><span data-stu-id="f8e27-178">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="f8e27-179">El paquete de herramientas no es necesario en el runtime, de modo que la dependencia se marca con `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="f8e27-179">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f8e27-180">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8e27-180">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f8e27-181">Instale los paquetes con la Consola del Administrador de paquetes (PMC) o mediante Administrar paquetes NuGet.</span><span class="sxs-lookup"><span data-stu-id="f8e27-181">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="f8e27-182">Opción de PMC para instalar paquetes</span><span class="sxs-lookup"><span data-stu-id="f8e27-182">PMC option to install packages</span></span>

* <span data-ttu-id="f8e27-183">En Visual Studio, seleccione **Herramientas** > **Administrador de paquetes de NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="f8e27-183">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="f8e27-184">En la ventana de la **Consola del Administrador de paquetes**, desplácese al directorio en el que se encuentra el archivo *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="f8e27-184">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="f8e27-185">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="f8e27-185">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="f8e27-186">Administración de la opción Paquetes NuGet para instalar paquetes</span><span class="sxs-lookup"><span data-stu-id="f8e27-186">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="f8e27-187">Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** > **Administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="f8e27-187">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="f8e27-188">Seleccione la pestaña **Examinar**.</span><span class="sxs-lookup"><span data-stu-id="f8e27-188">Select the **Browse** tab.</span></span>
* <span data-ttu-id="f8e27-189">Escriba **Grpc.Core** en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="f8e27-189">Enter **Grpc.Core** in the search box.</span></span>
* <span data-ttu-id="f8e27-190">Seleccione el paquete **Grpc.Core** en la pestaña **Examinar** y haga clic en **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="f8e27-190">Select the **Grpc.Core** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="f8e27-191">Repita el proceso para `Google.Protobuf` y `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="f8e27-191">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f8e27-192">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f8e27-192">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f8e27-193">Ejecute los siguientes comandos en el **terminal integrado**:</span><span class="sxs-lookup"><span data-stu-id="f8e27-193">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f8e27-194">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="f8e27-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f8e27-195">Haga clic con el botón derecho en la carpeta **Paquetes**, en **Panel de solución** > **Agregar paquetes**.</span><span class="sxs-lookup"><span data-stu-id="f8e27-195">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="f8e27-196">Escriba **Grpc.Net.Client** en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="f8e27-196">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="f8e27-197">Seleccione el paquete **Grpc.Net.Client** en el panel de resultados y seleccione **Agregar paquete**.</span><span class="sxs-lookup"><span data-stu-id="f8e27-197">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="f8e27-198">Repita el proceso para `Google.Protobuf` y `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="f8e27-198">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="f8e27-199">Adición de greet.proto</span><span class="sxs-lookup"><span data-stu-id="f8e27-199">Add greet.proto</span></span>

* <span data-ttu-id="f8e27-200">Cree una carpeta **Protos** en el proyecto de cliente gRPC.</span><span class="sxs-lookup"><span data-stu-id="f8e27-200">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="f8e27-201">Copie el archivo **Protos\greet.proto** del servicio gRPC Greeter en el proyecto de cliente gRPC.</span><span class="sxs-lookup"><span data-stu-id="f8e27-201">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="f8e27-202">Edite el archivo de proyecto *GrpcGreeterClient.csproj*:</span><span class="sxs-lookup"><span data-stu-id="f8e27-202">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f8e27-203">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8e27-203">Visual Studio</span></span>](#tab/visual-studio) 

  <span data-ttu-id="f8e27-204">Haga clic con el botón derecho en el proyecto y seleccione **Editar archivo del proyecto**.</span><span class="sxs-lookup"><span data-stu-id="f8e27-204">Right-click the project and select **Edit Project File**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f8e27-205">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f8e27-205">Visual Studio Code</span></span>](#tab/visual-studio-code) 

  <span data-ttu-id="f8e27-206">Seleccione el archivo *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="f8e27-206">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f8e27-207">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="f8e27-207">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="f8e27-208">Haga clic con el botón derecho en el proyecto y seleccione **Herramientas > Editar archivo**.</span><span class="sxs-lookup"><span data-stu-id="f8e27-208">Right-click the project and select **Tools > Edit File**.</span></span>

  ---

* <span data-ttu-id="f8e27-209">Agregue un grupo de elementos con un elemento `<Protobuf>` que hace referencia al archivo **greet.proto**:</span><span class="sxs-lookup"><span data-stu-id="f8e27-209">Add an item group with a `<Protobuf>` element that refers to the **greet.proto** file:</span></span>

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="f8e27-210">Creación del cliente de Greeter</span><span class="sxs-lookup"><span data-stu-id="f8e27-210">Create the Greeter client</span></span>

<span data-ttu-id="f8e27-211">Compile el proyecto para crear los tipos en el espacio de nombres `GrpcGreeter`.</span><span class="sxs-lookup"><span data-stu-id="f8e27-211">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="f8e27-212">El proceso de compilación genera automáticamente los tipos `GrpcGreeter`.</span><span class="sxs-lookup"><span data-stu-id="f8e27-212">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="f8e27-213">Actualice el archivo *Program.cs* del cliente gRPC con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="f8e27-213">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="f8e27-214">*Program.cs* contiene el punto de entrada y la lógica para el cliente gRPC.</span><span class="sxs-lookup"><span data-stu-id="f8e27-214">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="f8e27-215">El cliente de Greeter se crea mediante lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f8e27-215">The Greeter client is created by:</span></span>

* <span data-ttu-id="f8e27-216">Creación de una instancia de `HttpClient` que contiene la información para crear la conexión al servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="f8e27-216">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="f8e27-217">Uso de `HttpClient` para construir el cliente de Greeter:</span><span class="sxs-lookup"><span data-stu-id="f8e27-217">Using the `HttpClient` to construct the Greeter client:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-9)]

<span data-ttu-id="f8e27-218">El cliente de Greeter realiza una llamada al método `SayHello` asincrónico.</span><span class="sxs-lookup"><span data-stu-id="f8e27-218">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="f8e27-219">Se muestra el resultado de la llamada a `SayHello`:</span><span class="sxs-lookup"><span data-stu-id="f8e27-219">The result of the `SayHello` call is displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=10-12)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="f8e27-220">Prueba del cliente gRPC con el servicio gRPC Greeter</span><span class="sxs-lookup"><span data-stu-id="f8e27-220">Test the gRPC client with the gRPC Greeter service</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f8e27-221">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8e27-221">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f8e27-222">En el servicio Greeter, presione `Ctrl+F5` para iniciar el servidor sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="f8e27-222">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="f8e27-223">En el proyecto `GrpcGreeterClient`, presione `Ctrl+F5` para iniciar el servidor sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="f8e27-223">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the server without the debugger.</span></span>

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f8e27-224">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="f8e27-224">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="f8e27-225">Inicie el servicio Greeter.</span><span class="sxs-lookup"><span data-stu-id="f8e27-225">Start the Greeter service.</span></span>
* <span data-ttu-id="f8e27-226">Inicie el cliente.</span><span class="sxs-lookup"><span data-stu-id="f8e27-226">Start the client.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="f8e27-227">El cliente envía un saludo al servicio con un mensaje que contiene su nombre "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="f8e27-227">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="f8e27-228">El servicio envía el mensaje "Hello GreeterClient" como respuesta.</span><span class="sxs-lookup"><span data-stu-id="f8e27-228">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="f8e27-229">La respuesta "Hello GreeterClient" se muestra en el símbolo del sistema:</span><span class="sxs-lookup"><span data-stu-id="f8e27-229">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="f8e27-230">El servicio gRPC registra los detalles de la llamada correcta en los registros escritos en el símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="f8e27-230">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="next-steps"></a><span data-ttu-id="f8e27-231">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="f8e27-231">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
