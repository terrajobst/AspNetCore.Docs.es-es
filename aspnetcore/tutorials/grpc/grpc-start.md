---
title: Creación de un servidor y un cliente gRPC en ASP.NET Core
author: juntaoluo
description: En este tutorial se le mostrará cómo crear un servicio gRPC y un cliente gRPC en ASP.NET Core. Aprenda a crear un proyecto de servicio gRPC, edite un archivo proto y agregue una llamada de streaming dúplex.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 5/30/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 2b4325d2413e335a3061a7695def88a1b23ee52b
ms.sourcegitcommit: 4d05e30567279072f1b070618afe58ae1bcefd5a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376371"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="a4df3-104">Tutorial: Crear un servidor y un cliente gRPC en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a4df3-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="a4df3-105">Por [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="a4df3-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="a4df3-106">En este tutorial se muestra cómo crear un cliente [gRPC](https://grpc.io/docs/guides/) de .NET Core y un servidor gRPC de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a4df3-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="a4df3-107">Al final tendrá un cliente gRPC que se comunica con el servicio Greeter de gRPC.</span><span class="sxs-lookup"><span data-stu-id="a4df3-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="a4df3-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="a4df3-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="a4df3-109">En este tutorial hará lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a4df3-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a4df3-110">Crear un servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="a4df3-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="a4df3-111">Crear un cliente gRPC.</span><span class="sxs-lookup"><span data-stu-id="a4df3-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="a4df3-112">Probar el servicio cliente gRPC con el servicio gRPC Greeter.</span><span class="sxs-lookup"><span data-stu-id="a4df3-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="a4df3-113">Crear un servicio gRPC</span><span class="sxs-lookup"><span data-stu-id="a4df3-113">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a4df3-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4df3-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a4df3-115">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="a4df3-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="a4df3-116">En el cuadro de diálogo **Crear un proyecto nuevo**, seleccione **Aplicación web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a4df3-116">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="a4df3-117">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="a4df3-117">Select **Next**</span></span>
* <span data-ttu-id="a4df3-118">Llame al proyecto **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="a4df3-118">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="a4df3-119">Es importante asignarle el nombre *GrpcGreeter* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="a4df3-119">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="a4df3-120">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="a4df3-120">Select **Create**</span></span>
* <span data-ttu-id="a4df3-121">En el cuadro de diálogo **Crear una aplicación web ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="a4df3-121">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="a4df3-122">Seleccione **.NET Core** y **ASP.NET Core 3.0** en los menús desplegables.</span><span class="sxs-lookup"><span data-stu-id="a4df3-122">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="a4df3-123">Seleccione la plantilla **Servicio gRPC**.</span><span class="sxs-lookup"><span data-stu-id="a4df3-123">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="a4df3-124">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="a4df3-124">Select **Create**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a4df3-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a4df3-125">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a4df3-126">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="a4df3-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="a4df3-127">Cambie los directorios (`cd`) a una carpeta que contenga el proyecto.</span><span class="sxs-lookup"><span data-stu-id="a4df3-127">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="a4df3-128">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="a4df3-128">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="a4df3-129">El comando `dotnet new` crea un nuevo servicio gRPC en la carpeta *GrpcGreeter*.</span><span class="sxs-lookup"><span data-stu-id="a4df3-129">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="a4df3-130">El comando `code` abre la carpeta *GrpcGreeter* en una nueva instancia de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a4df3-130">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="a4df3-131">Se muestra un cuadro de diálogo con el texto **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="a4df3-131">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="a4df3-132">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="a4df3-132">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a4df3-133">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="a4df3-133">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a4df3-134">Desde un terminal, ejecute estos comandos:</span><span class="sxs-lookup"><span data-stu-id="a4df3-134">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="a4df3-135">Los comandos anteriores utilizan la [CLI de .NET Core](/dotnet/core/tools/dotnet) para crear un servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="a4df3-135">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="a4df3-136">Abrir el proyecto</span><span class="sxs-lookup"><span data-stu-id="a4df3-136">Open the project</span></span>

<span data-ttu-id="a4df3-137">En Visual Studio, seleccione **Archivo > Abrir** y elija el archivo *GrpcGreeter.sln*.</span><span class="sxs-lookup"><span data-stu-id="a4df3-137">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="a4df3-138">Ejecutar el servicio</span><span class="sxs-lookup"><span data-stu-id="a4df3-138">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a4df3-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4df3-139">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a4df3-140">Presione Ctrl+F5 para ejecutar el servicio gRPC sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="a4df3-140">Press Ctrl+F5 to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="a4df3-141">Visual Studio ejecuta el servicio en un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="a4df3-141">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a4df3-142">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="a4df3-142">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="a4df3-143">Ejecute el proyecto GrpcGreeter de gRPC Greeter desde la línea de comandos mediante `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="a4df3-143">Run the gRPC Greeter project GrpcGreeter from the command line using `dotnet run`.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="a4df3-144">Los registros muestran que el servicio está escuchando en `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="a4df3-144">The logs show the service listening on `http://localhost:50051`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

### <a name="examine-the-project-files"></a><span data-ttu-id="a4df3-145">Examen de los archivo del proyecto</span><span class="sxs-lookup"><span data-stu-id="a4df3-145">Examine the project files</span></span>

<span data-ttu-id="a4df3-146">Archivos de GrpcGreeter:</span><span class="sxs-lookup"><span data-stu-id="a4df3-146">GrpcGreeter files:</span></span>

* <span data-ttu-id="a4df3-147">*greet.proto*: El archivo *Protos/greet.proto* define el gRPC `Greeter` y se usa para generar los recursos de servidor gRPC.</span><span class="sxs-lookup"><span data-stu-id="a4df3-147">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="a4df3-148">Para obtener más información, vea [Introducción a gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="a4df3-148">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="a4df3-149">Carpeta *Servicios*: contiene la implementación del servicio `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="a4df3-149">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="a4df3-150">*appSettings.json*: contiene datos de configuración, como el protocolo usado por Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a4df3-150">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="a4df3-151">Para obtener más información, vea <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="a4df3-151">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="a4df3-152">*Program.cs*: contiene el punto de entrada para el servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="a4df3-152">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="a4df3-153">Para obtener más información, vea <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="a4df3-153">For more information, see <xref:fundamentals/host/web-host>.</span></span>
* <span data-ttu-id="a4df3-154">*Startup.cs*: Contiene código que configura el comportamiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a4df3-154">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="a4df3-155">Para obtener más información, vea [Inicio de la aplicación](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="a4df3-155">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="a4df3-156">Creación del cliente gRPC en una aplicación de consola de .NET</span><span class="sxs-lookup"><span data-stu-id="a4df3-156">Create the gRPC client in a .NET console app</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a4df3-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4df3-157">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a4df3-158">Seleccione **Archivo** > **Nuevo** > **Proyecto** de la barra de menús.</span><span class="sxs-lookup"><span data-stu-id="a4df3-158">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="a4df3-159">En el cuadro de diálogo **Crear un nuevo proyecto**, seleccione **Aplicación de consola (.NET Core)** .</span><span class="sxs-lookup"><span data-stu-id="a4df3-159">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="a4df3-160">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="a4df3-160">Select **Next**</span></span>
* <span data-ttu-id="a4df3-161">En el cuadro de texto **Nombre**, escriba "GrpcGreeterClient".</span><span class="sxs-lookup"><span data-stu-id="a4df3-161">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="a4df3-162">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="a4df3-162">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a4df3-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a4df3-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a4df3-164">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="a4df3-164">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="a4df3-165">Cambie los directorios (`cd`) a una carpeta que contenga el proyecto.</span><span class="sxs-lookup"><span data-stu-id="a4df3-165">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="a4df3-166">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="a4df3-166">Run the following commands:</span></span>

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a4df3-167">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="a4df3-167">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a4df3-168">Siga las instrucciones que se indican [aquí](/dotnet/core/tutorials/using-on-mac-vs-full-solution) para crear una aplicación de consola con el nombre *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="a4df3-168">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a><span data-ttu-id="a4df3-169">Adición de paquetes necesarios</span><span class="sxs-lookup"><span data-stu-id="a4df3-169">Add required packages</span></span>

<span data-ttu-id="a4df3-170">Agregue los siguientes paquetes al proyecto de cliente gRPC:</span><span class="sxs-lookup"><span data-stu-id="a4df3-170">Add the following packages to the gRPC client project:</span></span>

* <span data-ttu-id="a4df3-171">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core), que contiene la API de C# para el cliente de C-core.</span><span class="sxs-lookup"><span data-stu-id="a4df3-171">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core), which contains the C# API for the C-core client.</span></span>
* <span data-ttu-id="a4df3-172">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), que contiene API de mensajes protobuf para C#.</span><span class="sxs-lookup"><span data-stu-id="a4df3-172">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="a4df3-173">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), que contiene compatibilidad con herramientas de C# para archivos protobuf.</span><span class="sxs-lookup"><span data-stu-id="a4df3-173">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="a4df3-174">El paquete de herramientas no es necesario en el runtime, de modo que la dependencia se marca con `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="a4df3-174">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a4df3-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4df3-175">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a4df3-176">Instalación de los paquetes con la Consola del Administrador de paquetes (PMC) o mediante Administrar paquete de NuGet</span><span class="sxs-lookup"><span data-stu-id="a4df3-176">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Package</span></span>

####  <a name="pmc-option-to-install-packages"></a><span data-ttu-id="a4df3-177">Opción de PMC para instalar paquetes</span><span class="sxs-lookup"><span data-stu-id="a4df3-177">PMC option to install packages</span></span>

* <span data-ttu-id="a4df3-178">En Visual Studio, seleccione **Herramientas** > **Administrador de paquetes de NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="a4df3-178">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="a4df3-179">En la ventana de la **Consola del Administrador de paquetes**, desplácese al directorio en el que se encuentra el archivo *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="a4df3-179">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="a4df3-180">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="a4df3-180">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Core
Install-Package Grpc.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="a4df3-181">Administración de la opción Paquetes NuGet para instalar paquetes</span><span class="sxs-lookup"><span data-stu-id="a4df3-181">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="a4df3-182">Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** > **Administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="a4df3-182">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="a4df3-183">Seleccione la pestaña **Examinar**.</span><span class="sxs-lookup"><span data-stu-id="a4df3-183">Select the **Browse** tab.</span></span>
* <span data-ttu-id="a4df3-184">Escriba **Grpc.Core** en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="a4df3-184">Enter **Grpc.Core** in the search box.</span></span>
* <span data-ttu-id="a4df3-185">Seleccione el paquete **Grpc.Core** en la pestaña **Examinar** y haga clic en **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="a4df3-185">Select the **Grpc.Core** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="a4df3-186">Repita el proceso para `Google.Protobuf` y `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="a4df3-186">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a4df3-187">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a4df3-187">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a4df3-188">Ejecute los siguientes comandos en el **terminal integrado**:</span><span class="sxs-lookup"><span data-stu-id="a4df3-188">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Core
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a4df3-189">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="a4df3-189">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a4df3-190">Haga clic con el botón derecho en la carpeta **Paquetes**, en **Panel de solución** > **Agregar paquetes**.</span><span class="sxs-lookup"><span data-stu-id="a4df3-190">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="a4df3-191">Escriba **Grpc.Core** en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="a4df3-191">Enter **Grpc.Core** in the search box.</span></span>
* <span data-ttu-id="a4df3-192">Seleccione el paquete **Grpc.Core** en el panel de resultados y haga clic en **Agregar paquete**.</span><span class="sxs-lookup"><span data-stu-id="a4df3-192">Select the **Grpc.Core** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="a4df3-193">Repita el proceso para `Google.Protobuf` y `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="a4df3-193">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="a4df3-194">Adición de greet.proto</span><span class="sxs-lookup"><span data-stu-id="a4df3-194">Add greet.proto</span></span>

* <span data-ttu-id="a4df3-195">Cree una carpeta **Protos** en el proyecto de cliente gRPC.</span><span class="sxs-lookup"><span data-stu-id="a4df3-195">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="a4df3-196">Copie el archivo **Protos\greet.proto** del servicio gRPC Greeter en el proyecto de cliente gRPC.</span><span class="sxs-lookup"><span data-stu-id="a4df3-196">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="a4df3-197">Edite el archivo de proyecto *GrpcGreeterClient.csproj*:</span><span class="sxs-lookup"><span data-stu-id="a4df3-197">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a4df3-198">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4df3-198">Visual Studio</span></span>](#tab/visual-studio) 

  <span data-ttu-id="a4df3-199">Haga clic con el botón derecho en el proyecto y seleccione **Editar GrpcGreeterClient.csproj**.</span><span class="sxs-lookup"><span data-stu-id="a4df3-199">Right-click the project and select the **Edit GrpcGreeterClient.csproj**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a4df3-200">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a4df3-200">Visual Studio Code</span></span>](#tab/visual-studio-code) 

  <span data-ttu-id="a4df3-201">Seleccione el archivo *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="a4df3-201">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a4df3-202">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="a4df3-202">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="a4df3-203">Haga clic con el botón derecho en el proyecto y seleccione **Herramientas > Editar archivo**.</span><span class="sxs-lookup"><span data-stu-id="a4df3-203">Right click the project and select **Tools > Edit File**.</span></span>

  ------

* <span data-ttu-id="a4df3-204">Agregue el archivo **greet.proto** al grupo de elementos `<Protobuf>` del archivo del proyecto de GrpcGreeterClient:</span><span class="sxs-lookup"><span data-stu-id="a4df3-204">Add the **greet.proto** file to the `<Protobuf>` item group of the GrpcGreeterClient project file:</span></span>

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

<span data-ttu-id="a4df3-205">Compile el proyecto cliente para desencadenar la generación de los recursos del cliente de C#.</span><span class="sxs-lookup"><span data-stu-id="a4df3-205">Build the client project to trigger the generation of the C# client assets.</span></span>

### <a name="create-the-greater-client"></a><span data-ttu-id="a4df3-206">Creación del cliente Greeter</span><span class="sxs-lookup"><span data-stu-id="a4df3-206">Create the greater client</span></span>

<span data-ttu-id="a4df3-207">Compile el proyecto para crear los tipos en el espacio de nombres **Greeter**.</span><span class="sxs-lookup"><span data-stu-id="a4df3-207">Build the project to create the types in the **Greeter** namespace.</span></span> <span data-ttu-id="a4df3-208">El proceso de compilación genera automáticamente los tipos `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="a4df3-208">The `Greeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="a4df3-209">Actualice el archivo *Program.cs* del cliente gRPC con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="a4df3-209">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="a4df3-210">*Program.cs* contiene el punto de entrada y la lógica para el cliente gRPC.</span><span class="sxs-lookup"><span data-stu-id="a4df3-210">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="a4df3-211">Se crea el cliente de Greeter del siguiente modo:</span><span class="sxs-lookup"><span data-stu-id="a4df3-211">The greater client is created by:</span></span>

* <span data-ttu-id="a4df3-212">Creación de una instancia de `Channel` que contiene la información para crear la conexión al servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="a4df3-212">Instantiating a `Channel` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="a4df3-213">Uso de `Channel` para construir el cliente de Greeter:</span><span class="sxs-lookup"><span data-stu-id="a4df3-213">Using the `Channel` to construct the greater client:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-6)]

<span data-ttu-id="a4df3-214">El cliente Greeter realiza una llamada al método `SayHello` asincrónico.</span><span class="sxs-lookup"><span data-stu-id="a4df3-214">The greater client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="a4df3-215">Se muestra el resultado de la llamada a `SayHello`:</span><span class="sxs-lookup"><span data-stu-id="a4df3-215">The result of the `SayHello` call is displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-9)]

<span data-ttu-id="a4df3-216">Apague el parámetro `Channel` que usa el cliente cuando las operaciones hayan finalizado la liberación de todos los recursos.</span><span class="sxs-lookup"><span data-stu-id="a4df3-216">Shut down the `Channel` used by the client when operations have finished to release all resources.</span></span>

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="a4df3-217">Prueba del cliente gRPC con el servicio gRPC Greeter</span><span class="sxs-lookup"><span data-stu-id="a4df3-217">Test the gRPC client with the gRPC Greeter service</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a4df3-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4df3-218">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a4df3-219">En el servicio Greeter, pulse Ctrl+F5 para iniciar el servidor sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="a4df3-219">In the Greeter service, press Ctrl+F5 to start the server without the debugger.</span></span>
* <span data-ttu-id="a4df3-220">En el proyecto GrpcGreeterClient, pulse Ctrl+F5 para iniciar el servidor sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="a4df3-220">In the GrpcGreeterClient project, press Ctrl+F5 to start the server without the debugger.</span></span>

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a4df3-221">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="a4df3-221">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="a4df3-222">Inicie el servicio Greeter.</span><span class="sxs-lookup"><span data-stu-id="a4df3-222">Start the Greeter service.</span></span>
* <span data-ttu-id="a4df3-223">Inicie el cliente.</span><span class="sxs-lookup"><span data-stu-id="a4df3-223">Start the client.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="a4df3-224">El cliente envía un saludo al servicio con un mensaje que contiene su nombre "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="a4df3-224">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="a4df3-225">El servicio envía el mensaje "Hello GreeterClient" como respuesta.</span><span class="sxs-lookup"><span data-stu-id="a4df3-225">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="a4df3-226">La respuesta "Hello GreeterClient" se muestra en el símbolo del sistema:</span><span class="sxs-lookup"><span data-stu-id="a4df3-226">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="a4df3-227">El servicio gRPC registra los detalles de la llamada correcta en los registros escritos en el símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="a4df3-227">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="next-steps"></a><span data-ttu-id="a4df3-228">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="a4df3-228">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
