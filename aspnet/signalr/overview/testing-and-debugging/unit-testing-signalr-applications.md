---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Pruebas unitarias de aplicaciones SignalR | Microsoft Docs
author: pfletcher
description: En este artículo se describe cómo usar las características de las pruebas unitarias de SignalR 2.0.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: b058e8a05e50c2841b6272743f00dcd5b73b1460
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366192"
---
<a name="unit-testing-signalr-applications"></a><span data-ttu-id="a5764-103">Las pruebas unitarias de aplicaciones de SignalR</span><span class="sxs-lookup"><span data-stu-id="a5764-103">Unit Testing SignalR Applications</span></span>
====================
<span data-ttu-id="a5764-104">por [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="a5764-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="a5764-105">En este artículo se describe el uso de las características de las pruebas unitarias de SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="a5764-105">This article describes using the Unit Testing features of SignalR 2.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="a5764-106">Versiones de software que se usa en este tema</span><span class="sxs-lookup"><span data-stu-id="a5764-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="a5764-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="a5764-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="a5764-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="a5764-108">.NET 4.5</span></span>
> - <span data-ttu-id="a5764-109">Versión 2 de SignalR</span><span class="sxs-lookup"><span data-stu-id="a5764-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="a5764-110">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="a5764-110">Questions and comments</span></span>
> 
> <span data-ttu-id="a5764-111">Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="a5764-111">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="a5764-112">Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="a5764-112">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a><span data-ttu-id="a5764-113">Pruebas unitarias de aplicaciones SignalR</span><span class="sxs-lookup"><span data-stu-id="a5764-113">Unit testing SignalR applications</span></span>

<span data-ttu-id="a5764-114">Puede usar las características de pruebas unitarias en SignalR 2 para crear pruebas unitarias para la aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="a5764-114">You can use the unit test features in SignalR 2 to create unit tests for your SignalR application.</span></span> <span data-ttu-id="a5764-115">SignalR 2 incluye la [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interfaz, que puede usarse para crear un objeto ficticio para simular los métodos de concentrador de pruebas.</span><span class="sxs-lookup"><span data-stu-id="a5764-115">SignalR 2 includes the [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interface, which can be used to create a mock object to simulate your hub methods for testing.</span></span>

<span data-ttu-id="a5764-116">En esta sección, agregará las pruebas unitarias para la aplicación creada en el [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md) mediante [XUnit.net](https://github.com/xunit/xunit) y [Moq](https://github.com/Moq/moq4).</span><span class="sxs-lookup"><span data-stu-id="a5764-116">In this section, you'll add unit tests for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using [XUnit.net](https://github.com/xunit/xunit) and [Moq](https://github.com/Moq/moq4).</span></span>

<span data-ttu-id="a5764-117">Se usará XUnit.net para controlar la prueba. Moq se usará para crear un [simular](http://en.wikipedia.org/wiki/Mock_object) objeto para las pruebas.</span><span class="sxs-lookup"><span data-stu-id="a5764-117">XUnit.net will be used to control the test; Moq will be used to create a [mock](http://en.wikipedia.org/wiki/Mock_object) object for testing.</span></span> <span data-ttu-id="a5764-118">Se pueden usar otros marcos de simulación si lo desea; [NSubstitute](http://nsubstitute.github.io/) también es una buena elección.</span><span class="sxs-lookup"><span data-stu-id="a5764-118">Other mocking frameworks can be used if desired; [NSubstitute](http://nsubstitute.github.io/) is also a good choice.</span></span> <span data-ttu-id="a5764-119">Este tutorial muestra cómo configurar el objeto ficticio de dos maneras: en primer lugar, utilizando un `dynamic` objeto (introducida en .NET Framework 4) y, después, mediante una interfaz.</span><span class="sxs-lookup"><span data-stu-id="a5764-119">This tutorial demonstrates how to set up the mock object in two ways: First, using a `dynamic` object (introduced in .NET Framework 4), and second, using an interface.</span></span>

### <a name="contents"></a><span data-ttu-id="a5764-120">Contenido</span><span class="sxs-lookup"><span data-stu-id="a5764-120">Contents</span></span>

<span data-ttu-id="a5764-121">Este tutorial contiene las siguientes secciones.</span><span class="sxs-lookup"><span data-stu-id="a5764-121">This tutorial contains the following sections.</span></span>

- [<span data-ttu-id="a5764-122">Pruebas unitarias con dinámicos</span><span class="sxs-lookup"><span data-stu-id="a5764-122">Unit testing with Dynamic</span></span>](#dynamic)
- [<span data-ttu-id="a5764-123">Pruebas unitarias al tipo</span><span class="sxs-lookup"><span data-stu-id="a5764-123">Unit testing by type</span></span>](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a><span data-ttu-id="a5764-124">Pruebas unitarias con dinámicos</span><span class="sxs-lookup"><span data-stu-id="a5764-124">Unit testing with Dynamic</span></span>

<span data-ttu-id="a5764-125">En esta sección, agregará una prueba unitaria para la aplicación creada en el [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md) mediante un objeto dinámico.</span><span class="sxs-lookup"><span data-stu-id="a5764-125">In this section, you'll add a unit test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using a dynamic object.</span></span>

1. <span data-ttu-id="a5764-126">Instalar el [extensión ejecutor de XUnit](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) para Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="a5764-126">Install the [XUnit Runner extension](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) for Visual Studio 2013.</span></span>
2. <span data-ttu-id="a5764-127">Completar la [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md), o descargar la aplicación completa de [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="a5764-127">Either complete the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the completed application from [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
3. <span data-ttu-id="a5764-128">Si se usa la versión de descarga de la aplicación de introducción, abra **Package Manager Console** y haga clic en **restaurar** para agregar el paquete de SignalR al proyecto.</span><span class="sxs-lookup"><span data-stu-id="a5764-128">If you are using the download version of the Getting Started application, open **Package Manager Console** and click **Restore** to add the SignalR package to the project.</span></span>

    ![Restaurar paquetes](unit-testing-signalr-applications/_static/image1.png)
4. <span data-ttu-id="a5764-130">Agregar un proyecto a la solución para la prueba unitaria.</span><span class="sxs-lookup"><span data-stu-id="a5764-130">Add a project to the solution for the unit test.</span></span> <span data-ttu-id="a5764-131">Haga clic en la solución en **el Explorador de soluciones** y seleccione **agregar**, **nuevo proyecto...** . En el **C#** nodo, seleccione el **Windows** nodo.</span><span class="sxs-lookup"><span data-stu-id="a5764-131">Right-click your solution in **Solution Explorer** and select **Add**, **New Project...**. Under the **C#** node, select the **Windows** node.</span></span> <span data-ttu-id="a5764-132">Seleccione **biblioteca de clases**.</span><span class="sxs-lookup"><span data-stu-id="a5764-132">Select **Class Library**.</span></span> <span data-ttu-id="a5764-133">Asigne al nuevo proyecto **TestLibrary** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="a5764-133">Name the new project **TestLibrary** and click **OK**.</span></span>

    ![Crear la biblioteca de pruebas](unit-testing-signalr-applications/_static/image2.png)
5. <span data-ttu-id="a5764-135">Agregue una referencia en el proyecto de biblioteca de prueba al proyecto SignalRChat.</span><span class="sxs-lookup"><span data-stu-id="a5764-135">Add a reference in the test library project to the SignalRChat project.</span></span> <span data-ttu-id="a5764-136">Haga clic en el **TestLibrary** del proyecto y seleccione **agregar**, **referencia...** . Seleccione el **proyectos** nodo bajo el **solución** nodo y verificación **SignalRChat**.</span><span class="sxs-lookup"><span data-stu-id="a5764-136">Right-click the **TestLibrary** project and select **Add**, **Reference...**. Select the **Projects** node under the **Solution** node, and check **SignalRChat**.</span></span> <span data-ttu-id="a5764-137">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="a5764-137">Click **OK**.</span></span>

    ![Agregar referencia de proyecto](unit-testing-signalr-applications/_static/image3.png)
6. <span data-ttu-id="a5764-139">Agregue los paquetes de SignalR, Moq y XUnit para el **TestLibrary** proyecto.</span><span class="sxs-lookup"><span data-stu-id="a5764-139">Add the SignalR, Moq, and XUnit packages to the **TestLibrary** project.</span></span> <span data-ttu-id="a5764-140">En el **Package Manager Console**, establezca el **proyecto predeterminado** lista desplegable para **TestLibrary**.</span><span class="sxs-lookup"><span data-stu-id="a5764-140">In the **Package Manager Console**, set the **Default Project** dropdown to **TestLibrary**.</span></span> <span data-ttu-id="a5764-141">En la ventana de consola, ejecute los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="a5764-141">Run the following commands in the console window:</span></span>

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![Instalar paquetes](unit-testing-signalr-applications/_static/image4.png)
7. <span data-ttu-id="a5764-143">Cree el archivo de prueba.</span><span class="sxs-lookup"><span data-stu-id="a5764-143">Create the test file.</span></span> <span data-ttu-id="a5764-144">Haga clic en el **TestLibrary** del proyecto y haga clic en **agregar...** , **Clase**.</span><span class="sxs-lookup"><span data-stu-id="a5764-144">Right-click the **TestLibrary** project and click **Add...**, **Class**.</span></span> <span data-ttu-id="a5764-145">Nombre de la nueva clase **Tests.cs**.</span><span class="sxs-lookup"><span data-stu-id="a5764-145">Name the new class **Tests.cs**.</span></span>
8. <span data-ttu-id="a5764-146">Reemplace el contenido de Tests.cs con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="a5764-146">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    <span data-ttu-id="a5764-147">En el código anterior, se crea un cliente de prueba mediante el `Mock` objeto desde el [Moq](https://github.com/Moq/moq4) biblioteca de tipo [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (en SignalR 2.1, asigne `dynamic` para el tipo parámetro.) El `IHubCallerConnectionContext` interfaz es el objeto de proxy con la que se invocan métodos en el cliente.</span><span class="sxs-lookup"><span data-stu-id="a5764-147">In the code above, a test client is created using the `Mock` object from the [Moq](https://github.com/Moq/moq4) library, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span> <span data-ttu-id="a5764-148">El `broadcastMessage` función, a continuación, se define para el cliente ficticio para que se puede llamar el `ChatHub` clase.</span><span class="sxs-lookup"><span data-stu-id="a5764-148">The `broadcastMessage` function is then defined for the mock client so that it can be called by the `ChatHub` class.</span></span> <span data-ttu-id="a5764-149">El motor de pruebas, a continuación, llama a la `Send` método de la `ChatHub` (clase), que a su vez llama a la ficticios `broadcastMessage` función.</span><span class="sxs-lookup"><span data-stu-id="a5764-149">The test engine then calls the `Send` method of the `ChatHub` class, which in turn calls the mocked `broadcastMessage` function.</span></span>
9. <span data-ttu-id="a5764-150">Compile la solución presionando **F6**.</span><span class="sxs-lookup"><span data-stu-id="a5764-150">Build the solution by pressing **F6**.</span></span>
10. <span data-ttu-id="a5764-151">Ejecute la prueba unitaria.</span><span class="sxs-lookup"><span data-stu-id="a5764-151">Run the unit test.</span></span> <span data-ttu-id="a5764-152">En Visual Studio, seleccione **prueba**, **Windows**, **Explorador de pruebas**.</span><span class="sxs-lookup"><span data-stu-id="a5764-152">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="a5764-153">En la ventana Explorador de pruebas, haga clic en **HubsAreMockableViaDynamic** y seleccione **ejecutar pruebas seleccionadas**.</span><span class="sxs-lookup"><span data-stu-id="a5764-153">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Explorador de pruebas](unit-testing-signalr-applications/_static/image5.png)
11. <span data-ttu-id="a5764-155">Compruebe que se superó la prueba mediante la comprobación de la parte inferior de la ventana del explorador de pruebas.</span><span class="sxs-lookup"><span data-stu-id="a5764-155">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="a5764-156">Se mostrará la ventana que se superó la prueba.</span><span class="sxs-lookup"><span data-stu-id="a5764-156">The window will show that the test passed.</span></span>

    ![Prueba superada](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a><span data-ttu-id="a5764-158">Pruebas unitarias al tipo</span><span class="sxs-lookup"><span data-stu-id="a5764-158">Unit testing by type</span></span>

<span data-ttu-id="a5764-159">En esta sección, agregará una prueba para la aplicación creada en el [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md) mediante una interfaz que contiene el método que va a probarse.</span><span class="sxs-lookup"><span data-stu-id="a5764-159">In this section, you'll add a test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using an interface that contains the method to be tested.</span></span>

1. <span data-ttu-id="a5764-160">Complete los pasos 1-7 en el [pruebas unitarias con Dynamic](#dynamic) tutorial anterior.</span><span class="sxs-lookup"><span data-stu-id="a5764-160">Complete steps 1-7 in the [Unit testing with Dynamic](#dynamic) tutorial above.</span></span>
2. <span data-ttu-id="a5764-161">Reemplace el contenido de Tests.cs con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="a5764-161">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    <span data-ttu-id="a5764-162">En el código anterior, se crea una interfaz definiendo la firma de la `broadcastMessage` método para que el motor de pruebas creará un cliente ficticio.</span><span class="sxs-lookup"><span data-stu-id="a5764-162">In the code above, an interface is created defining the signature of the `broadcastMessage` method for which the test engine will create a mock client.</span></span> <span data-ttu-id="a5764-163">Un cliente ficticio, a continuación, se crea mediante la `Mock` objeto del tipo [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (en SignalR 2.1, asigne `dynamic` para el parámetro de tipo.) El `IHubCallerConnectionContext` interfaz es el objeto de proxy con la que se invocan métodos en el cliente.</span><span class="sxs-lookup"><span data-stu-id="a5764-163">A mock client is then created using the `Mock` object, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span>

    <span data-ttu-id="a5764-164">La prueba, a continuación, crea una instancia de `ChatHub`y, a continuación, crea una versión ficticia del `broadcastMessage` método, que a su vez, se invoca mediante una llamada a la `Send` método del concentrador.</span><span class="sxs-lookup"><span data-stu-id="a5764-164">The test then creates an instance of `ChatHub`, and then creates a mock version of the `broadcastMessage` method, which in turn is invoked by calling the `Send` method on the hub.</span></span>
3. <span data-ttu-id="a5764-165">Compile la solución presionando **F6**.</span><span class="sxs-lookup"><span data-stu-id="a5764-165">Build the solution by pressing **F6**.</span></span>
4. <span data-ttu-id="a5764-166">Ejecute la prueba unitaria.</span><span class="sxs-lookup"><span data-stu-id="a5764-166">Run the unit test.</span></span> <span data-ttu-id="a5764-167">En Visual Studio, seleccione **prueba**, **Windows**, **Explorador de pruebas**.</span><span class="sxs-lookup"><span data-stu-id="a5764-167">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="a5764-168">En la ventana Explorador de pruebas, haga clic en **HubsAreMockableViaDynamic** y seleccione **ejecutar pruebas seleccionadas**.</span><span class="sxs-lookup"><span data-stu-id="a5764-168">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Explorador de pruebas](unit-testing-signalr-applications/_static/image7.png)
5. <span data-ttu-id="a5764-170">Compruebe que se superó la prueba mediante la comprobación de la parte inferior de la ventana del explorador de pruebas.</span><span class="sxs-lookup"><span data-stu-id="a5764-170">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="a5764-171">Se mostrará la ventana que se superó la prueba.</span><span class="sxs-lookup"><span data-stu-id="a5764-171">The window will show that the test passed.</span></span>

    ![Prueba superada](unit-testing-signalr-applications/_static/image8.png)
