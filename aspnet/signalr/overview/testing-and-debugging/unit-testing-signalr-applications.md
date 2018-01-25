---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Pruebas unitarias de aplicaciones SignalR | Documentos de Microsoft
author: pfletcher
description: "En este artículo se describe cómo usar las características de pruebas unitarias de SignalR 2.0."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: d767e1a9d27670387133e5a48a8f92f5bdd39d9e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="unit-testing-signalr-applications"></a><span data-ttu-id="6ff50-103">Aplicaciones de SignalR de pruebas unitarias</span><span class="sxs-lookup"><span data-stu-id="6ff50-103">Unit Testing SignalR Applications</span></span>
====================
<span data-ttu-id="6ff50-104">por [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="6ff50-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="6ff50-105">Este artículo describe cómo utilizar las características de pruebas unitarias de SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="6ff50-105">This article describes using the Unit Testing features of SignalR 2.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="6ff50-106">Versiones de software que se usa en este tema</span><span class="sxs-lookup"><span data-stu-id="6ff50-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="6ff50-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="6ff50-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="6ff50-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="6ff50-108">.NET 4.5</span></span>
> - <span data-ttu-id="6ff50-109">SignalR versión 2</span><span class="sxs-lookup"><span data-stu-id="6ff50-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="6ff50-110">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="6ff50-110">Questions and comments</span></span>
> 
> <span data-ttu-id="6ff50-111">Vota sobre cómo le gustó este tutorial y lo que podemos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="6ff50-111">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="6ff50-112">Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="6ff50-112">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a><span data-ttu-id="6ff50-113">Pruebas unitarias de aplicaciones SignalR</span><span class="sxs-lookup"><span data-stu-id="6ff50-113">Unit testing SignalR applications</span></span>

<span data-ttu-id="6ff50-114">Puede usar las características de pruebas unitarias en SignalR 2 para crear pruebas unitarias para la aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="6ff50-114">You can use the unit test features in SignalR 2 to create unit tests for your SignalR application.</span></span> <span data-ttu-id="6ff50-115">SignalR 2 incluye la [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interfaz, que se puede usar para crear un objeto ficticio para simular los métodos de concentrador de pruebas.</span><span class="sxs-lookup"><span data-stu-id="6ff50-115">SignalR 2 includes the [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interface, which can be used to create a mock object to simulate your hub methods for testing.</span></span>

<span data-ttu-id="6ff50-116">En esta sección, agregará las pruebas unitarias para la aplicación creada en el [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md) con [XUnit.net](https://github.com/xunit/xunit) y [Moq](https://github.com/Moq/moq4).</span><span class="sxs-lookup"><span data-stu-id="6ff50-116">In this section, you'll add unit tests for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using [XUnit.net](https://github.com/xunit/xunit) and [Moq](https://github.com/Moq/moq4).</span></span>

<span data-ttu-id="6ff50-117">XUnit.net se usará para controlar la prueba; Moq se usará para crear un [simular](http://en.wikipedia.org/wiki/Mock_object) objeto para realizar pruebas.</span><span class="sxs-lookup"><span data-stu-id="6ff50-117">XUnit.net will be used to control the test; Moq will be used to create a [mock](http://en.wikipedia.org/wiki/Mock_object) object for testing.</span></span> <span data-ttu-id="6ff50-118">Se pueden usar otros marcos de simulación si lo desea; [NSubstitute](http://nsubstitute.github.io/) también es una buena elección.</span><span class="sxs-lookup"><span data-stu-id="6ff50-118">Other mocking frameworks can be used if desired; [NSubstitute](http://nsubstitute.github.io/) is also a good choice.</span></span> <span data-ttu-id="6ff50-119">Este tutorial muestra cómo configurar el objeto ficticio de dos maneras: en primer lugar, utilizando un `dynamic` objeto (introducida en .NET Framework 4) y, después, mediante una interfaz.</span><span class="sxs-lookup"><span data-stu-id="6ff50-119">This tutorial demonstrates how to set up the mock object in two ways: First, using a `dynamic` object (introduced in .NET Framework 4), and second, using an interface.</span></span>

### <a name="contents"></a><span data-ttu-id="6ff50-120">Contenido</span><span class="sxs-lookup"><span data-stu-id="6ff50-120">Contents</span></span>

<span data-ttu-id="6ff50-121">Este tutorial contiene las siguientes secciones.</span><span class="sxs-lookup"><span data-stu-id="6ff50-121">This tutorial contains the following sections.</span></span>

- [<span data-ttu-id="6ff50-122">Pruebas unitarias con dinámicos</span><span class="sxs-lookup"><span data-stu-id="6ff50-122">Unit testing with Dynamic</span></span>](#dynamic)
- [<span data-ttu-id="6ff50-123">Pruebas por tipo de unidad</span><span class="sxs-lookup"><span data-stu-id="6ff50-123">Unit testing by type</span></span>](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a><span data-ttu-id="6ff50-124">Pruebas unitarias con dinámicos</span><span class="sxs-lookup"><span data-stu-id="6ff50-124">Unit testing with Dynamic</span></span>

<span data-ttu-id="6ff50-125">En esta sección, agregará una prueba unitaria para la aplicación creada en el [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md) mediante un objeto dinámico.</span><span class="sxs-lookup"><span data-stu-id="6ff50-125">In this section, you'll add a unit test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using a dynamic object.</span></span>

1. <span data-ttu-id="6ff50-126">Instalar el [extensión XUnit ejecutor](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) para Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="6ff50-126">Install the [XUnit Runner extension](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) for Visual Studio 2013.</span></span>
2. <span data-ttu-id="6ff50-127">Completar la [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md), o descargar la aplicación completa de [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="6ff50-127">Either complete the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the completed application from [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
3. <span data-ttu-id="6ff50-128">Si está utilizando la versión de descarga de la aplicación de introducción, abra **Package Manager Console** y haga clic en **restaurar** para agregar el paquete de SignalR para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="6ff50-128">If you are using the download version of the Getting Started application, open **Package Manager Console** and click **Restore** to add the SignalR package to the project.</span></span>

    ![Restaurar los paquetes](unit-testing-signalr-applications/_static/image1.png)
4. <span data-ttu-id="6ff50-130">Agregar un proyecto a la solución para la prueba unitaria.</span><span class="sxs-lookup"><span data-stu-id="6ff50-130">Add a project to the solution for the unit test.</span></span> <span data-ttu-id="6ff50-131">Haga clic en la solución en **el Explorador de soluciones** y seleccione **agregar**, **nuevo proyecto...** . En el **C#** nodo, seleccione el **Windows** nodo.</span><span class="sxs-lookup"><span data-stu-id="6ff50-131">Right-click your solution in **Solution Explorer** and select **Add**, **New Project...**. Under the **C#** node, select the **Windows** node.</span></span> <span data-ttu-id="6ff50-132">Seleccione **biblioteca de clases**.</span><span class="sxs-lookup"><span data-stu-id="6ff50-132">Select **Class Library**.</span></span> <span data-ttu-id="6ff50-133">Asigne al nuevo proyecto **TestLibrary** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="6ff50-133">Name the new project **TestLibrary** and click **OK**.</span></span>

    ![Crear biblioteca de prueba](unit-testing-signalr-applications/_static/image2.png)
5. <span data-ttu-id="6ff50-135">Agregue una referencia en el proyecto de biblioteca de prueba al proyecto SignalRChat.</span><span class="sxs-lookup"><span data-stu-id="6ff50-135">Add a reference in the test library project to the SignalRChat project.</span></span> <span data-ttu-id="6ff50-136">Haga clic en el **TestLibrary** de proyecto y seleccione **agregar**, **referencia...** . Seleccione el **proyectos** nodo bajo el **solución** nodo y verificación **SignalRChat**.</span><span class="sxs-lookup"><span data-stu-id="6ff50-136">Right-click the **TestLibrary** project and select **Add**, **Reference...**. Select the **Projects** node under the **Solution** node, and check **SignalRChat**.</span></span> <span data-ttu-id="6ff50-137">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="6ff50-137">Click **OK**.</span></span>

    ![Agregar referencia de proyecto](unit-testing-signalr-applications/_static/image3.png)
6. <span data-ttu-id="6ff50-139">Agregar los paquetes SignalR, Moq y XUnit el **TestLibrary** proyecto.</span><span class="sxs-lookup"><span data-stu-id="6ff50-139">Add the SignalR, Moq, and XUnit packages to the **TestLibrary** project.</span></span> <span data-ttu-id="6ff50-140">En el **Package Manager Console**, establezca el **proyecto predeterminado** control dropdown **TestLibrary**.</span><span class="sxs-lookup"><span data-stu-id="6ff50-140">In the **Package Manager Console**, set the **Default Project** dropdown to **TestLibrary**.</span></span> <span data-ttu-id="6ff50-141">Ejecute los siguientes comandos en la ventana de consola:</span><span class="sxs-lookup"><span data-stu-id="6ff50-141">Run the following commands in the console window:</span></span>

    - `Install-Package Microsoft.AspNet.SignalR`
    - `Install-Package Moq`
    - `Install-Package XUnit`

    ![Instalar paquetes](unit-testing-signalr-applications/_static/image4.png)
7. <span data-ttu-id="6ff50-143">Crear el archivo de prueba.</span><span class="sxs-lookup"><span data-stu-id="6ff50-143">Create the test file.</span></span> <span data-ttu-id="6ff50-144">Haga clic en el **TestLibrary** del proyecto y haga clic en **agregar...** , **Clase**.</span><span class="sxs-lookup"><span data-stu-id="6ff50-144">Right-click the **TestLibrary** project and click **Add...**, **Class**.</span></span> <span data-ttu-id="6ff50-145">La nueva clase el nombre **Tests.cs**.</span><span class="sxs-lookup"><span data-stu-id="6ff50-145">Name the new class **Tests.cs**.</span></span>
8. <span data-ttu-id="6ff50-146">Reemplace el contenido de Tests.cs con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="6ff50-146">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    <span data-ttu-id="6ff50-147">En el código anterior, se crea un cliente de prueba mediante el `Mock` objeto desde el [Moq](https://github.com/Moq/moq4) biblioteca, de tipo [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (en SignalR 2.1, asigne `dynamic` para el tipo parámetro). La `IHubCallerConnectionContext` interfaz es el objeto de proxy con el que invocar métodos en el cliente.</span><span class="sxs-lookup"><span data-stu-id="6ff50-147">In the code above, a test client is created using the `Mock` object from the [Moq](https://github.com/Moq/moq4) library, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span> <span data-ttu-id="6ff50-148">El `broadcastMessage` función, a continuación, se define para el cliente ficticio para que se puede llamar a través del `ChatHub` clase.</span><span class="sxs-lookup"><span data-stu-id="6ff50-148">The `broadcastMessage` function is then defined for the mock client so that it can be called by the `ChatHub` class.</span></span> <span data-ttu-id="6ff50-149">El motor de pruebas, a continuación, llama a la `Send` método de la `ChatHub` (clase), que a su vez llama a la simuladas `broadcastMessage` función.</span><span class="sxs-lookup"><span data-stu-id="6ff50-149">The test engine then calls the `Send` method of the `ChatHub` class, which in turn calls the mocked `broadcastMessage` function.</span></span>
9. <span data-ttu-id="6ff50-150">Compile la solución presionando **F6**.</span><span class="sxs-lookup"><span data-stu-id="6ff50-150">Build the solution by pressing **F6**.</span></span>
10. <span data-ttu-id="6ff50-151">Ejecute la prueba unitaria.</span><span class="sxs-lookup"><span data-stu-id="6ff50-151">Run the unit test.</span></span> <span data-ttu-id="6ff50-152">En Visual Studio, seleccione **prueba**, **Windows**, **Explorador de pruebas**.</span><span class="sxs-lookup"><span data-stu-id="6ff50-152">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="6ff50-153">En la ventana Explorador de pruebas, haga clic en **HubsAreMockableViaDynamic** y seleccione **ejecutar pruebas seleccionadas**.</span><span class="sxs-lookup"><span data-stu-id="6ff50-153">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Explorador de pruebas](unit-testing-signalr-applications/_static/image5.png)
11. <span data-ttu-id="6ff50-155">Compruebe que se superó la prueba activando el panel inferior de la ventana del explorador de pruebas.</span><span class="sxs-lookup"><span data-stu-id="6ff50-155">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="6ff50-156">La ventana mostrará que se superó la prueba.</span><span class="sxs-lookup"><span data-stu-id="6ff50-156">The window will show that the test passed.</span></span>

    ![Prueba superada](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a><span data-ttu-id="6ff50-158">Pruebas por tipo de unidad</span><span class="sxs-lookup"><span data-stu-id="6ff50-158">Unit testing by type</span></span>

<span data-ttu-id="6ff50-159">En esta sección, agregará una prueba para la aplicación creada en el [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md) usando una interfaz que contiene el método que se va a probar.</span><span class="sxs-lookup"><span data-stu-id="6ff50-159">In this section, you'll add a test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using an interface that contains the method to be tested.</span></span>

1. <span data-ttu-id="6ff50-160">Complete los pasos del 1 al 7 en el [pruebas unitarias con dinámicos](#dynamic) tutorial anterior.</span><span class="sxs-lookup"><span data-stu-id="6ff50-160">Complete steps 1-7 in the [Unit testing with Dynamic](#dynamic) tutorial above.</span></span>
2. <span data-ttu-id="6ff50-161">Reemplace el contenido de Tests.cs con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="6ff50-161">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    <span data-ttu-id="6ff50-162">En el código anterior, se crea una interfaz definiendo la firma de la `broadcastMessage` método para el que el motor de pruebas creará un cliente ficticio.</span><span class="sxs-lookup"><span data-stu-id="6ff50-162">In the code above, an interface is created defining the signature of the `broadcastMessage` method for which the test engine will create a mock client.</span></span> <span data-ttu-id="6ff50-163">A continuación, se crea un cliente ficticio mediante la `Mock` objeto del tipo [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (en SignalR 2.1, asigne `dynamic` para el parámetro de tipo.) La `IHubCallerConnectionContext` interfaz es el objeto de proxy con el que invocar métodos en el cliente.</span><span class="sxs-lookup"><span data-stu-id="6ff50-163">A mock client is then created using the `Mock` object, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span>

    <span data-ttu-id="6ff50-164">La prueba, a continuación, crea una instancia de `ChatHub`y, a continuación, crea una versión ficticia de la `broadcastMessage` método, que a su vez, se invoca mediante una llamada a la `Send` método en el concentrador.</span><span class="sxs-lookup"><span data-stu-id="6ff50-164">The test then creates an instance of `ChatHub`, and then creates a mock version of the `broadcastMessage` method, which in turn is invoked by calling the `Send` method on the hub.</span></span>
3. <span data-ttu-id="6ff50-165">Compile la solución presionando **F6**.</span><span class="sxs-lookup"><span data-stu-id="6ff50-165">Build the solution by pressing **F6**.</span></span>
4. <span data-ttu-id="6ff50-166">Ejecute la prueba unitaria.</span><span class="sxs-lookup"><span data-stu-id="6ff50-166">Run the unit test.</span></span> <span data-ttu-id="6ff50-167">En Visual Studio, seleccione **prueba**, **Windows**, **Explorador de pruebas**.</span><span class="sxs-lookup"><span data-stu-id="6ff50-167">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="6ff50-168">En la ventana Explorador de pruebas, haga clic en **HubsAreMockableViaDynamic** y seleccione **ejecutar pruebas seleccionadas**.</span><span class="sxs-lookup"><span data-stu-id="6ff50-168">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Explorador de pruebas](unit-testing-signalr-applications/_static/image7.png)
5. <span data-ttu-id="6ff50-170">Compruebe que se superó la prueba activando el panel inferior de la ventana del explorador de pruebas.</span><span class="sxs-lookup"><span data-stu-id="6ff50-170">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="6ff50-171">La ventana mostrará que se superó la prueba.</span><span class="sxs-lookup"><span data-stu-id="6ff50-171">The window will show that the test passed.</span></span>

    ![Prueba superada](unit-testing-signalr-applications/_static/image8.png)
