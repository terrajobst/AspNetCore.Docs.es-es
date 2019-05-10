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
# <a name="tutorial-create-a-net-core-grpc-client"></a><span data-ttu-id="c93db-104">Tutorial: Creación de un cliente gRPC de .NET Core</span><span class="sxs-lookup"><span data-stu-id="c93db-104">Tutorial: Create a .NET Core gRPC client</span></span>

<span data-ttu-id="c93db-105">Por [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="c93db-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="c93db-106">En este tutorial se muestra cómo crear un cliente [gRPC](https://grpc.io/docs/guides/) de .NET Core que se puede comunicar con servicios gRPC.</span><span class="sxs-lookup"><span data-stu-id="c93db-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client that can communicate with gRPC services.</span></span>

<span data-ttu-id="c93db-107">Al final tendrá un cliente gRPC que se comunica con el servicio Greeter de gRPC.</span><span class="sxs-lookup"><span data-stu-id="c93db-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

[!INCLUDE[View or download sample code](~/includes/grpc/downloadClient.md)]

<span data-ttu-id="c93db-108">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="c93db-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c93db-109">Crear un cliente gRPC.</span><span class="sxs-lookup"><span data-stu-id="c93db-109">Create a gRPC client.</span></span>
> * <span data-ttu-id="c93db-110">Ejecutar el servicio con el servicio gRPC Greeter creado en el tutorial anterior.</span><span class="sxs-lookup"><span data-stu-id="c93db-110">Run the service against the gRPC Greeter service created in the previous tutorial.</span></span>
> * <span data-ttu-id="c93db-111">Examinar los archivos de proyecto.</span><span class="sxs-lookup"><span data-stu-id="c93db-111">Examine the project files.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-net-console-application"></a><span data-ttu-id="c93db-112">Creación de una aplicación de consola .NET</span><span class="sxs-lookup"><span data-stu-id="c93db-112">Create a .NET console application</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c93db-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c93db-113">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c93db-114">Siga las instrucciones que se indican [aquí](/dotnet/core/tutorials/with-visual-studio) para crear una aplicación de consola con el nombre *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="c93db-114">Follow the instructions [here](/dotnet/core/tutorials/with-visual-studio) to create a console app with the name *GrpcGreeterClient*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c93db-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c93db-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="c93db-116">Siga las instrucciones que se indican [aquí](/dotnet/core/tutorials/with-visual-studio-code) para crear una aplicación de consola con el nombre *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="c93db-116">Follow the instructions [here](/dotnet/core/tutorials/with-visual-studio-code) to create a console app with the name *GrpcGreeterClient*.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c93db-117">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="c93db-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="c93db-118">Siga las instrucciones que se indican [aquí](/dotnet/core/tutorials/using-on-mac-vs-full-solution) para crear una aplicación de consola con el nombre *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="c93db-118">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

## <a name="add-required-packages"></a><span data-ttu-id="c93db-119">Adición de paquetes necesarios</span><span class="sxs-lookup"><span data-stu-id="c93db-119">Add required packages</span></span>

<span data-ttu-id="c93db-120">Agregue los siguientes paquetes al proyecto de cliente gRPC:</span><span class="sxs-lookup"><span data-stu-id="c93db-120">Add the following packages to the gRPC client project:</span></span>

* <span data-ttu-id="c93db-121">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core), que contiene la API de C# para el cliente de C-core.</span><span class="sxs-lookup"><span data-stu-id="c93db-121">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core), which contains the C# API for the C-core client.</span></span>
* <span data-ttu-id="c93db-122">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), que contiene API de mensajes protobuf para C#.</span><span class="sxs-lookup"><span data-stu-id="c93db-122">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="c93db-123">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), que contiene compatibilidad con herramientas de C# para archivos protobuf.</span><span class="sxs-lookup"><span data-stu-id="c93db-123">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="c93db-124">El paquete de herramientas no es necesario en el runtime, de modo que la dependencia se marca con `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="c93db-124">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

<span data-ttu-id="c93db-125">Se pueden agregar paquetes con los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="c93db-125">Packages can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c93db-126">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c93db-126">Visual Studio</span></span>](#tab/visual-studio)

### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="c93db-127">Opción de PMC para instalar paquetes</span><span class="sxs-lookup"><span data-stu-id="c93db-127">PMC option to install packages</span></span>

* <span data-ttu-id="c93db-128">En Visual Studio, seleccione **Herramientas** > **Administrador de paquetes de NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="c93db-128">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="c93db-129">En la ventana de la **Consola del Administrador de paquetes**, desplácese al directorio en el que se encuentra el archivo *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="c93db-129">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="c93db-130">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="c93db-130">Run the following command:</span></span>

    ```powershell
    Install-Package Grpc.Core
    ```

* <span data-ttu-id="c93db-131">Repita `Install-Package` para Google.Protobuf y Grpc.Tools.</span><span class="sxs-lookup"><span data-stu-id="c93db-131">Repeat the `Install-Package` for Google.Protobuf and Grpc.Tools</span></span>

<!-- Tutorials shouldn't have multiple options. Select what you think is the best approach. Recommend you removed this approach. -->

### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="c93db-132">Administración de la opción Paquetes NuGet para instalar paquetes</span><span class="sxs-lookup"><span data-stu-id="c93db-132">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="c93db-133">Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** > **Administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c93db-133">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="c93db-134">Establezca el **origen del paquete** en "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="c93db-134">Set the **Package source** to "nuget.org"</span></span>
* <span data-ttu-id="c93db-135">Escriba "Grpc.Core" en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="c93db-135">Enter "Grpc.Core" in the search box</span></span>
* <span data-ttu-id="c93db-136">Seleccione el paquete "Grpc.Core" en la pestaña **Examinar** y haga clic en **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="c93db-136">Select the "Grpc.Core" package from the **Browse** tab and click **Install**</span></span>
* <span data-ttu-id="c93db-137">Repita este paso para Google.Protobuf y Grpc.Tools.</span><span class="sxs-lookup"><span data-stu-id="c93db-137">Repeat for Google.Protobuf and Grpc.Tools</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c93db-138">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c93db-138">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="c93db-139">Ejecute el siguiente comando en el **terminal integrado**:</span><span class="sxs-lookup"><span data-stu-id="c93db-139">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Core
```

<span data-ttu-id="c93db-140">Repita este paso para Google.Protobuf y Grpc.Tools.</span><span class="sxs-lookup"><span data-stu-id="c93db-140">Repeat for Google.Protobuf and Grpc.Tools</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c93db-141">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="c93db-141">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c93db-142">Haga clic con el botón derecho en la carpeta *Paquetes* en **Panel de solución** > **Agregar paquetes...**</span><span class="sxs-lookup"><span data-stu-id="c93db-142">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="c93db-143">Establezca el menú desplegable **Origen** de la ventana **Agregar paquetes** en "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="c93db-143">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="c93db-144">Escriba "Grpc.Core" en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="c93db-144">Enter "Grpc.Core" in the search box</span></span>
* <span data-ttu-id="c93db-145">Seleccione el paquete "Grpc.Core" en el panel de resultados y haga clic en **Agregar paquete**.</span><span class="sxs-lookup"><span data-stu-id="c93db-145">Select the "Grpc.Core" package from the results pane and click **Add Package**</span></span>
* <span data-ttu-id="c93db-146">Repita este paso para Google.Protobuf y Grpc.Tools.</span><span class="sxs-lookup"><span data-stu-id="c93db-146">Repeat for Google.Protobuf and Grpc.Tools</span></span>

---

## <a name="add-the-greetproto-file"></a><span data-ttu-id="c93db-147">Adición del archivo greet.proto</span><span class="sxs-lookup"><span data-stu-id="c93db-147">Add the greet.proto file</span></span>

<span data-ttu-id="c93db-148">Copie el archivo **Protos\greet.proto** del servicio gRPC Greeter en el proyecto de cliente gRPC.</span><span class="sxs-lookup"><span data-stu-id="c93db-148">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span> <span data-ttu-id="c93db-149">Agregue el archivo **greet.proto** al grupo de elementos `<Protobuf>` del archivo del proyecto de GrpcGreeterClient:</span><span class="sxs-lookup"><span data-stu-id="c93db-149">Add the **greet.proto** file to the `<Protobuf>` item group of the GrpcGreeterClient project file:</span></span>

```XML
<Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
```

> [!NOTE]
> <span data-ttu-id="c93db-150">Para abrir el archivo del proyecto de GrpcGreeterClient, haga clic con el botón derecho en el proyecto y seleccione la opción **Editar GrpcGreeterClient.csproj** en el menú desplegable.</span><span class="sxs-lookup"><span data-stu-id="c93db-150">You can open the project file of GrpcGreeterClient by right-clicking the project and selecting the **Edit GrpcGreeterClient.csproj** option from the dropdown menu.</span></span>
>
> ![Nueva aplicación web de ASP.NET Core](grpc-start/_static/edit_csproj.png)
>
> <span data-ttu-id="c93db-152">También puede navegar al directorio de GrpcGreeterClient y editar `GrpcGreeterClient.csproj` con su editor favorito.</span><span class="sxs-lookup"><span data-stu-id="c93db-152">Alternatively, you can navigate to the GrpcGreeterClient directory and edit the `GrpcGreeterClient.csproj` with your favorite editor.</span></span>

<span data-ttu-id="c93db-153">El atributo `GrpcServices="Client"` se agrega para que solo se generen recursos del cliente de C# para el archivo protobuf incluido.</span><span class="sxs-lookup"><span data-stu-id="c93db-153">The `GrpcServices="Client"` attribute is added so that only the C# client assets are generated for the included protobuf file.</span></span> <span data-ttu-id="c93db-154">Compile el proyecto cliente para desencadenar la generación de los recursos del cliente de C#.</span><span class="sxs-lookup"><span data-stu-id="c93db-154">Build the client project to trigger the generation of the C# client assets.</span></span>

## <a name="create-a-greeterclient-and-invoke-the-sayhello-unary-call"></a><span data-ttu-id="c93db-155">Creación de GreeterClient e invocación de la llamada unaria SayHello</span><span class="sxs-lookup"><span data-stu-id="c93db-155">Create a GreeterClient and invoke the SayHello unary call</span></span>

<span data-ttu-id="c93db-156">Agregue el código siguiente al método `Main` del archivo `Program.cs` del proyecto de cliente gRPC:</span><span class="sxs-lookup"><span data-stu-id="c93db-156">Add the following code to `Main` method of the `Program.cs` file of the gRPC client project:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet)]

<span data-ttu-id="c93db-157">Para acceder a los tipos requeridos, se necesitan las instrucciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="c93db-157">To access the required types the following using statements are required:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=using)]

<span data-ttu-id="c93db-158">Se crea GreeterClient mediante la creación de una instancia de `Channel` que contiene la información para crear la conexión al servicio gRPC y usándola para construir `GreeterClient`:</span><span class="sxs-lookup"><span data-stu-id="c93db-158">The GreeterClient is created by instantiating a `Channel` containing the information for creating the connection to the gRPC service and using it to construct the `GreeterClient`:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-5)]

<span data-ttu-id="c93db-159">GreeterClient contiene la llamada unaria `SayHello`, que se puede invocar de forma asincrónica:</span><span class="sxs-lookup"><span data-stu-id="c93db-159">The GreeterClient contains the unary call `SayHello` which can be invoked asynchronously:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-8)]

<span data-ttu-id="c93db-160">Los resultados de la llamada `SayHello` se almacenan en `reply`, desde donde se pueden mostrar:</span><span class="sxs-lookup"><span data-stu-id="c93db-160">The results of the `SayHello` call is stored in `reply` which can then be displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=9)]

<span data-ttu-id="c93db-161">El valor de `Channel` que usa el cliente se debe apagar cuando las operaciones hayan finalizado la liberación de todos los recursos:</span><span class="sxs-lookup"><span data-stu-id="c93db-161">The `Channel` used by the client should be shut down when operations have finished to release all resources:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=11)]

> [!NOTE]
> <span data-ttu-id="c93db-162">Deberá compilar el proyecto antes de que los tipos del espacio de nombres **Greeter** se puedan resolver.</span><span class="sxs-lookup"><span data-stu-id="c93db-162">You will need to build the project before the types in the **Greeter** namespace can be resolved.</span></span> <span data-ttu-id="c93db-163">Estos tipos se generan automáticamente durante la compilación y no están disponibles antes de ejecutar una compilación.</span><span class="sxs-lookup"><span data-stu-id="c93db-163">These types are generated automatically during build and are not be available before a build is run.</span></span>

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="c93db-164">Prueba del cliente gRPC con el servicio gRPC Greeter</span><span class="sxs-lookup"><span data-stu-id="c93db-164">Test the gRPC client with the gRPC Greeter service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c93db-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c93db-165">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c93db-166">Asegúrese de que el servicio Greeter creado en el tutorial anterior está en ejecución.</span><span class="sxs-lookup"><span data-stu-id="c93db-166">Ensure the Greeter service created in the previous tutorial is running.</span></span>

* <span data-ttu-id="c93db-167">Una vez que el servicio esté en ejecución, vuelva al proyecto **GrpcGreeterClient** y establézcalo como el proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="c93db-167">Once the service is running, return to the **GrpcGreeterClient** project set it as the Startup Project.</span></span> <span data-ttu-id="c93db-168">Presione Ctrl+F5 para ejecutar el cliente sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="c93db-168">Press Ctrl+F5 to run the client without the debugger.</span></span>

  <span data-ttu-id="c93db-169">El cliente envía un saludo al servicio con un mensaje que contiene su nombre "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="c93db-169">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="c93db-170">El servicio enviará un mensaje "Hola GreeterClient" como respuesta que se muestra en el símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="c93db-170">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

  ![Nueva aplicación web de ASP.NET Core](grpc-start/_static/client.png)

  <span data-ttu-id="c93db-172">El servicio registra los detalles de la llamada correcta en los registros escritos en el símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="c93db-172">The service records the details of the successful call in the logs written to the command prompt.</span></span>

  ![Nueva aplicación web de ASP.NET Core](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="c93db-174">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="c93db-174">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="c93db-175">Asegúrese de que el servicio Greeter creado en el tutorial anterior está en ejecución.</span><span class="sxs-lookup"><span data-stu-id="c93db-175">Ensure the Greeter service created in the previous tutorial is running.</span></span>

* <span data-ttu-id="c93db-176">Ejecute el proyecto de cliente GrpcGreeter.Server desde la línea de comandos independiente mediante `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="c93db-176">Run the Client project GrpcGreeter.Client from the separate command line using `dotnet run`.</span></span>

<span data-ttu-id="c93db-177">El cliente envía un saludo al servicio con un mensaje que contiene su nombre "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="c93db-177">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="c93db-178">El servicio enviará un mensaje "Hola GreeterClient" como respuesta que se muestra en el símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="c93db-178">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="c93db-179">El servicio registra los detalles de la llamada correcta en los registros escritos en el símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="c93db-179">The service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="examine-the-project-files-of-the-grpc-project"></a><span data-ttu-id="c93db-180">Examinar los archivos de proyecto del proyecto gRPC</span><span class="sxs-lookup"><span data-stu-id="c93db-180">Examine the project files of the gRPC project</span></span>

<span data-ttu-id="c93db-181">Archivo GrpcGreeterClient del cliente gRPC:</span><span class="sxs-lookup"><span data-stu-id="c93db-181">gRPC client GrpcGreeterClient file:</span></span>

<span data-ttu-id="c93db-182">*Program.cs* contiene el punto de entrada y la lógica para el cliente gRPC.</span><span class="sxs-lookup"><span data-stu-id="c93db-182">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c93db-183">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="c93db-183">Additional resources</span></span>

<span data-ttu-id="c93db-184">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="c93db-184">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c93db-185">Crear un cliente gRPC.</span><span class="sxs-lookup"><span data-stu-id="c93db-185">Create a gRPC client.</span></span>
> * <span data-ttu-id="c93db-186">Ejecutar el servicio con el servicio gRPC Greeter creado en el tutorial anterior.</span><span class="sxs-lookup"><span data-stu-id="c93db-186">Run the service against the gRPC Greeter service created in the previous tutorial.</span></span>
> * <span data-ttu-id="c93db-187">Examinar los archivos de proyecto.</span><span class="sxs-lookup"><span data-stu-id="c93db-187">Examine the project files.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c93db-188">Anterior: Creación de un servicio gRPC Greeter</span><span class="sxs-lookup"><span data-stu-id="c93db-188">Previous: Create a gRPC Greeter Service</span></span>](xref:tutorials/grpc/grpc-start)
