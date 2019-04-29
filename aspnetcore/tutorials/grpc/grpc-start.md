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
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="405af-104">Tutorial: Introducción al servicio gRPC en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="405af-104">Tutorial: Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="405af-105">Por [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="405af-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="405af-106">En este tutorial se enseñan los conceptos básicos de la compilación de un servicio gRPC en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="405af-106">This tutorial teaches the basics of building a gRPC service on ASP.NET Core.</span></span>

<span data-ttu-id="405af-107">Al final, tendrá un servicio gRPC que repite saludos.</span><span class="sxs-lookup"><span data-stu-id="405af-107">At the end, you'll have a gRPC service that echoes greetings.</span></span>

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

<span data-ttu-id="405af-108">En este tutorial hará lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="405af-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="405af-109">Crear un servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="405af-109">Create a gRPC service.</span></span>
> * <span data-ttu-id="405af-110">Ejecutar el servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="405af-110">Run the gRPC service.</span></span>
> * <span data-ttu-id="405af-111">Examinar los archivos de proyecto.</span><span class="sxs-lookup"><span data-stu-id="405af-111">Examine the project files.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="405af-112">Crear un servicio gRPC</span><span class="sxs-lookup"><span data-stu-id="405af-112">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="405af-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="405af-113">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="405af-114">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="405af-114">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="405af-115">Cree una aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="405af-115">Create a new ASP.NET Core Web Application.</span></span>
  <span data-ttu-id="405af-116">![Nueva aplicación web de ASP.NET Core](grpc-start/_static/np_3_0.1.png)</span><span class="sxs-lookup"><span data-stu-id="405af-116">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.1.png)</span></span>
* <span data-ttu-id="405af-117">Llame al proyecto **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="405af-117">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="405af-118">Es importante asignarle el nombre *GrpcGreeter* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="405af-118">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="405af-119">![Nueva aplicación web de ASP.NET Core](grpc-start/_static/np_3_0.2.png)</span><span class="sxs-lookup"><span data-stu-id="405af-119">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.2.png)</span></span>
* <span data-ttu-id="405af-120">Seleccione **.NET Core** y **ASP.NET Core 3.0** en el menú desplegable.</span><span class="sxs-lookup"><span data-stu-id="405af-120">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown.</span></span> <span data-ttu-id="405af-121">Elija la plantilla **Servicio gRPC**.</span><span class="sxs-lookup"><span data-stu-id="405af-121">Choose the **gRPC Service** template.</span></span>

  <span data-ttu-id="405af-122">Se crea el proyecto de inicio siguiente:</span><span class="sxs-lookup"><span data-stu-id="405af-122">The following starter project is created:</span></span>

  ![Explorador de soluciones](grpc-start/_static/se3.0.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="405af-124">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="405af-124">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="405af-125">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="405af-125">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="405af-126">Cambie los directorios (`cd`) a una carpeta que contenga el proyecto.</span><span class="sxs-lookup"><span data-stu-id="405af-126">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="405af-127">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="405af-127">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="405af-128">El comando `dotnet new` crea un nuevo servicio gRPC en la carpeta *GrpcGreeter*.</span><span class="sxs-lookup"><span data-stu-id="405af-128">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="405af-129">El comando `code` abre la carpeta *GrpcGreeter* en una nueva instancia de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="405af-129">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="405af-130">Se muestra un cuadro de diálogo con el texto **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="405af-130">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="405af-131">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="405af-131">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="405af-132">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="405af-132">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="405af-133">Desde un terminal, ejecute estos comandos:</span><span class="sxs-lookup"><span data-stu-id="405af-133">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="405af-134">Los comandos anteriores utilizan la [CLI de .NET Core](/dotnet/core/tools/dotnet) para crear un servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="405af-134">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="405af-135">Abrir el proyecto</span><span class="sxs-lookup"><span data-stu-id="405af-135">Open the project</span></span>

<span data-ttu-id="405af-136">En Visual Studio, seleccione **Archivo > Abrir** y elija el archivo *GrpcGreeter.sln*.</span><span class="sxs-lookup"><span data-stu-id="405af-136">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="405af-137">Ejecutar el servicio</span><span class="sxs-lookup"><span data-stu-id="405af-137">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="405af-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="405af-138">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="405af-139">Presione Ctrl+F5 para ejecutar el servicio gRPC sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="405af-139">Press Ctrl+F5 to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="405af-140">Visual Studio ejecuta el servicio en un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="405af-140">Visual Studio runs the service in a command prompt.</span></span> <span data-ttu-id="405af-141">Los registros muestran que el servicio inició la escucha en `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="405af-141">The logs show that the service started listening on `http://localhost:50051`.</span></span>

  ![Nueva aplicación web de ASP.NET Core](grpc-start/_static/server_start.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="405af-143">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="405af-143">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="405af-144">Ejecute el proyecto GrpcGreeter de gRPC Greeter desde la línea de comandos mediante `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="405af-144">Run the gRPC Greeter project GrpcGreeter from the command line using `dotnet run`.</span></span> <span data-ttu-id="405af-145">Los registros muestran que el servicio inició la escucha en `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="405af-145">The logs show that the service started listening on `http://localhost:50051`.</span></span>

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

<span data-ttu-id="405af-146">En el siguiente tutorial se le enseñará cómo crear un cliente gRPC, que es necesario para probar el servicio Greeter.</span><span class="sxs-lookup"><span data-stu-id="405af-146">The next tutorial will demonstrate how to build a gRPC client, which is required to test the Greeter service.</span></span>

### <a name="examine-the-project-files-of-the-grpc-project"></a><span data-ttu-id="405af-147">Examinar los archivos de proyecto del proyecto gRPC</span><span class="sxs-lookup"><span data-stu-id="405af-147">Examine the project files of the gRPC project</span></span>

<span data-ttu-id="405af-148">Archivos de GrpcGreeter:</span><span class="sxs-lookup"><span data-stu-id="405af-148">GrpcGreeter files:</span></span>

* <span data-ttu-id="405af-149">greet.proto: El archivo *Protos/greet.proto* define el gRPC `Greeter` y se usa para generar los recursos de servidor gRPC.</span><span class="sxs-lookup"><span data-stu-id="405af-149">greet.proto: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="405af-150">Para obtener más información, vea <xref:grpc/index>.</span><span class="sxs-lookup"><span data-stu-id="405af-150">For more information, see <xref:grpc/index>.</span></span>
* <span data-ttu-id="405af-151">Carpeta *Servicios*: contiene la implementación del servicio `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="405af-151">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="405af-152">*appSettings.json*: contiene datos de configuración, como el protocolo usado por Kestrel.</span><span class="sxs-lookup"><span data-stu-id="405af-152">*appSettings.json*:Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="405af-153">Para obtener más información, vea <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="405af-153">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="405af-154">*Program.cs*: contiene el punto de entrada para el servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="405af-154">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="405af-155">Para obtener más información, vea <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="405af-155">For more information, see <xref:fundamentals/host/web-host>.</span></span>
* <span data-ttu-id="405af-156">Startup.cs: Contiene código que configura el comportamiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="405af-156">Startup.cs: Contains code that configures app behavior.</span></span> <span data-ttu-id="405af-157">Para obtener más información, vea <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="405af-157">For more information, see <xref:fundamentals/startup>.</span></span>

### <a name="test-the-service"></a><span data-ttu-id="405af-158">Probar el servicio</span><span class="sxs-lookup"><span data-stu-id="405af-158">Test the service</span></span>

## <a name="additional-resources"></a><span data-ttu-id="405af-159">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="405af-159">Additional resources</span></span>

<span data-ttu-id="405af-160">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="405af-160">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="405af-161">Creado un servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="405af-161">Created a gRPC service.</span></span>
> * <span data-ttu-id="405af-162">Ejecutado el servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="405af-162">Ran the gRPC service.</span></span>
> * <span data-ttu-id="405af-163">Examinado los archivo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="405af-163">Examined the project files.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="405af-164">Siguiente: Creación de un cliente gRPC de .NET Core</span><span class="sxs-lookup"><span data-stu-id="405af-164">Next: Create a .NET Core gRPC client</span></span>](xref:tutorials/grpc/grpc-client)