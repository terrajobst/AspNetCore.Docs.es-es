---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Tutorial: Difusión de servidores con SignalR 2 | Microsoft Docs'
author: tdykstra
description: Este tutorial muestra cómo crear una aplicación web que usa ASP.NET SignalR 2 para proporcionar funcionalidad de difusión de servidor.
ms.author: riande
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: a6014e604613492db91b2dc6f846c3c73d938d99
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099304"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a><span data-ttu-id="3c3e5-103">Tutorial: Servidor de difusión con SignalR 2</span><span class="sxs-lookup"><span data-stu-id="3c3e5-103">Tutorial: Server broadcast with SignalR 2</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="3c3e5-104">Este tutorial muestra cómo crear una aplicación web que usa ASP.NET SignalR 2 para proporcionar funcionalidad de difusión de servidor.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span> <span data-ttu-id="3c3e5-105">Difusión de servidor significa que el servidor inicia a las comunicaciones enviadas a los clientes.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-105">Server broadcast means that the server starts the communications sent to clients.</span></span>

<span data-ttu-id="3c3e5-106">La aplicación que creará en este tutorial simula un tablero de cotizaciones, un escenario típico para la funcionalidad de difusión de servidor.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-106">The application that you'll create in this tutorial simulates a stock ticker, a typical scenario for server broadcast functionality.</span></span> <span data-ttu-id="3c3e5-107">Periódicamente, el servidor actualiza cotizaciones bursátiles aleatoriamente y difunde las actualizaciones a todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-107">Periodically, the server randomly updates stock prices and broadcast the updates to all connected clients.</span></span> <span data-ttu-id="3c3e5-108">En el explorador, los números y símbolos en el **cambiar** y **%** columnas cambian dinámicamente en respuesta a las notificaciones desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-108">In the browser, the numbers and symbols in the **Change** and **%** columns dynamically change in response to notifications from the server.</span></span> <span data-ttu-id="3c3e5-109">Si abre los exploradores adicionales a la misma dirección URL, todos ellos muestran los mismos datos y los mismos cambios a los datos simultáneamente.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-109">If you open additional browsers to the same URL, they all show the same data and the same changes to the data simultaneously.</span></span>

![Creación de web](tutorial-server-broadcast-with-signalr/_static/image1.png)

<span data-ttu-id="3c3e5-111">En este tutorial, puede:</span><span class="sxs-lookup"><span data-stu-id="3c3e5-111">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3c3e5-112">Crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="3c3e5-112">Create the project</span></span>
> * <span data-ttu-id="3c3e5-113">Configurar el código de servidor</span><span class="sxs-lookup"><span data-stu-id="3c3e5-113">Set up the server code</span></span>
> * <span data-ttu-id="3c3e5-114">Examine el código de servidor</span><span class="sxs-lookup"><span data-stu-id="3c3e5-114">Examine the server code</span></span>
> * <span data-ttu-id="3c3e5-115">Configurar el código de cliente</span><span class="sxs-lookup"><span data-stu-id="3c3e5-115">Set up the client code</span></span>
> * <span data-ttu-id="3c3e5-116">Examine el código de cliente</span><span class="sxs-lookup"><span data-stu-id="3c3e5-116">Examine the client code</span></span>
> * <span data-ttu-id="3c3e5-117">Probar la aplicación</span><span class="sxs-lookup"><span data-stu-id="3c3e5-117">Test the application</span></span>
> * <span data-ttu-id="3c3e5-118">Habilite el registro</span><span class="sxs-lookup"><span data-stu-id="3c3e5-118">Enable logging</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3c3e5-119">Si no desea trabajar con los pasos de la creación de la aplicación, puede instalar el paquete SignalR.Sample en un nuevo proyecto de aplicación Web ASP.NET vacía.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-119">If you don't want to work through the steps of building the application, you can install the SignalR.Sample package in a new Empty ASP.NET Web Application project.</span></span> <span data-ttu-id="3c3e5-120">Si instala el paquete de NuGet sin necesidad de realizar los pasos de este tutorial, debe seguir las instrucciones de la *readme.txt* archivo.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-120">If you install the NuGet package without performing the steps in this tutorial, you must follow the instructions in the *readme.txt* file.</span></span> <span data-ttu-id="3c3e5-121">Para ejecutar el paquete que necesita agregar un inicio OWIN que llama a la clase el `ConfigureSignalR` método en el paquete instalado.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-121">To run the package you need to add an OWIN startup class which calls the `ConfigureSignalR` method in the installed package.</span></span> <span data-ttu-id="3c3e5-122">Recibirá un error si no se agrega la clase de inicio OWIN.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-122">You will receive an error if you do not add the OWIN startup class.</span></span> <span data-ttu-id="3c3e5-123">Consulte la [instalar el ejemplo StockTicker](#install-the-stockticker-sample) sección de este artículo.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-123">See the [Install the StockTicker sample](#install-the-stockticker-sample) section of this article.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="3c3e5-124">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="3c3e5-124">Prerequisites</span></span>

 * <span data-ttu-id="3c3e5-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con el **ASP.NET y desarrollo web** carga de trabajo.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="3c3e5-126">Crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="3c3e5-126">Create the project</span></span>

<span data-ttu-id="3c3e5-127">En esta sección se muestra cómo usar Visual Studio 2017 para crear una aplicación Web de ASP.NET vacía.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-127">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application.</span></span>

1. <span data-ttu-id="3c3e5-128">En Visual Studio, cree una aplicación Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-128">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Creación de web](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. <span data-ttu-id="3c3e5-130">En el **nueva aplicación Web de ASP.NET - SignalR.StockTicker** ventana, deje **vacía** seleccionado y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-130">In the **New ASP.NET Web Application - SignalR.StockTicker** window, leave **Empty** selected and select **OK**.</span></span>

## <a name="set-up-the-server-code"></a><span data-ttu-id="3c3e5-131">Configurar el código de servidor</span><span class="sxs-lookup"><span data-stu-id="3c3e5-131">Set up the server code</span></span>

<span data-ttu-id="3c3e5-132">En esta sección, se establece el código que se ejecuta en el servidor.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-132">In this section, you set up the code that runs on the server.</span></span>

### <a name="create-the-stock-class"></a><span data-ttu-id="3c3e5-133">Crear la clase de Stock</span><span class="sxs-lookup"><span data-stu-id="3c3e5-133">Create the Stock class</span></span>

<span data-ttu-id="3c3e5-134">Empiece por crear la *Stock* clase que va a usar para almacenar y transmitir información sobre una acción de modelo.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-134">You begin by creating the *Stock* model class that you'll use to store and transmit information about a stock.</span></span>

1. <span data-ttu-id="3c3e5-135">En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **clase**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-135">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="3c3e5-136">Nombre de la clase *Stock* y agréguelo al proyecto.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-136">Name the class *Stock* and add it to the project.</span></span>

1. <span data-ttu-id="3c3e5-137">Reemplace el código en el *Stock.cs* archivo con este código:</span><span class="sxs-lookup"><span data-stu-id="3c3e5-137">Replace the code in the *Stock.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    <span data-ttu-id="3c3e5-138">Las dos propiedades que se va a configurar al crear las poblaciones son `Symbol` (por ejemplo, MSFT para Microsoft) y `Price`.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-138">The two properties that you'll set when you create stocks are `Symbol` (for example, MSFT for Microsoft) and `Price`.</span></span> <span data-ttu-id="3c3e5-139">Las demás propiedades dependen de cómo y cuándo establecer `Price`.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-139">The other properties depend on how and when you set `Price`.</span></span> <span data-ttu-id="3c3e5-140">La primera vez que establece `Price`, el valor se propague a `DayOpen`.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-140">The first time you set `Price`, the value gets propagated to `DayOpen`.</span></span> <span data-ttu-id="3c3e5-141">Después de eso, al establecer `Price`, la aplicación calcula el `Change` y `PercentChange` los valores de propiedad según la diferencia entre `Price` y `DayOpen`.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-141">After that, when you set `Price`, the app calculates the `Change` and `PercentChange` property values based on the difference between `Price` and `DayOpen`.</span></span>

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a><span data-ttu-id="3c3e5-142">Cree las clases StockTickerHub y StockTicker</span><span class="sxs-lookup"><span data-stu-id="3c3e5-142">Create the StockTickerHub and StockTicker classes</span></span>

<span data-ttu-id="3c3e5-143">Usará la API de concentrador SignalR para controlar la interacción con el cliente y el servidor.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-143">You'll use the SignalR Hub API to handle server-to-client interaction.</span></span> <span data-ttu-id="3c3e5-144">Un `StockTickerHub` clase que deriva la `SignalRHub` clase controlará recibir conexiones y las llamadas a métodos de los clientes.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-144">A `StockTickerHub` class that derives from the `SignalRHub` class will handle receiving connections and method calls from clients.</span></span> <span data-ttu-id="3c3e5-145">También deberá mantener datos de acciones y ejecutar un `Timer` objeto.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-145">You also need to maintain stock data and run a `Timer` object.</span></span> <span data-ttu-id="3c3e5-146">La `Timer` objeto desencadenará periódicamente las actualizaciones de precios independientes de las conexiones de cliente.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-146">The `Timer` object will periodically trigger price updates independent of client connections.</span></span> <span data-ttu-id="3c3e5-147">No se puede colocar estas funciones un `Hub` clase, porque los concentradores son transitorios.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-147">You can't put these functions in a `Hub` class, because Hubs are transient.</span></span> <span data-ttu-id="3c3e5-148">La aplicación crea un `Hub` instancia de la clase para cada tarea en el centro, como las conexiones y las llamadas desde el cliente al servidor.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-148">The app creates a `Hub` class instance for each task on the hub, like connections and calls from the client to the server.</span></span> <span data-ttu-id="3c3e5-149">Por lo que el mecanismo que mantiene los datos de cotizaciones, actualiza los precios y difunde las actualizaciones de precios debe ejecutarse en una clase independiente.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-149">So the mechanism that keeps stock data, updates prices, and broadcasts the price updates has to run in a separate class.</span></span> <span data-ttu-id="3c3e5-150">Le asigne el nombre de la clase `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-150">You'll name the class `StockTicker`.</span></span>

![Difusión de StockTicker](tutorial-server-broadcast-with-signalr/_static/image3.png)

<span data-ttu-id="3c3e5-152">Solo quiere una instancia de la `StockTicker` clase para ejecutar en el servidor, por lo que necesitará configurar una referencia de cada `StockTickerHub` instancia para el singleton `StockTicker` instancia.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-152">You only want one instance of the `StockTicker` class to run on the server, so you'll need to set up a reference from each `StockTickerHub` instance to the singleton `StockTicker` instance.</span></span> <span data-ttu-id="3c3e5-153">El `StockTicker` clase debe difundir a los clientes porque tiene los datos de acciones y desencadenadores de las actualizaciones, pero `StockTicker` no es un `Hub` clase.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-153">The `StockTicker` class has to broadcast to clients because it has the stock data and triggers updates, but `StockTicker` isn't a `Hub` class.</span></span> <span data-ttu-id="3c3e5-154">La `StockTicker` clase tiene que obtener una referencia al objeto de contexto de conexión de concentrador SignalR.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-154">The `StockTicker` class has to get a reference to the SignalR Hub connection context object.</span></span> <span data-ttu-id="3c3e5-155">También puede usar el objeto de contexto de conexión de SignalR para difundir a los clientes.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-155">It can then use the SignalR connection context object to broadcast to clients.</span></span>

#### <a name="create-stocktickerhubcs"></a><span data-ttu-id="3c3e5-156">Crear StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="3c3e5-156">Create StockTickerHub.cs</span></span>

1. <span data-ttu-id="3c3e5-157">En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-157">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="3c3e5-158">En **Agregar nuevo elemento - SignalR.StockTicker**, seleccione **instalado** > **Visual C#**   >  **Web**  >  **SignalR** y, a continuación, seleccione **clase de concentrador de SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-158">In **Add New Item - SignalR.StockTicker**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="3c3e5-159">Nombre de la clase *StockTickerHub* y agréguelo al proyecto.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-159">Name the class *StockTickerHub* and add it to the project.</span></span>

    <span data-ttu-id="3c3e5-160">Este paso se crea el *StockTickerHub.cs* archivo de clase.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-160">This step creates the *StockTickerHub.cs* class file.</span></span> <span data-ttu-id="3c3e5-161">Al mismo tiempo, agrega un conjunto de archivos de script y las referencias de ensamblado que es compatible con SignalR al proyecto.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-161">Simultaneously, it adds a set of script files and assembly references that supports SignalR to the project.</span></span>

1. <span data-ttu-id="3c3e5-162">Reemplace el código en el *StockTickerHub.cs* archivo con este código:</span><span class="sxs-lookup"><span data-stu-id="3c3e5-162">Replace the code in the *StockTickerHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. <span data-ttu-id="3c3e5-163">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-163">Save the file.</span></span>

<span data-ttu-id="3c3e5-164">La aplicación usa el [concentrador](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) clase para definir los métodos que se pueden llamar los clientes en el servidor.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-164">The app uses the [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) class to define methods the clients can call on the server.</span></span> <span data-ttu-id="3c3e5-165">Definir un método: `GetAllStocks()`.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-165">You're defining one method: `GetAllStocks()`.</span></span> <span data-ttu-id="3c3e5-166">Cuando un cliente se conecta inicialmente con el servidor, llamará a este método para obtener una lista de todas las acciones con el precio actual.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-166">When a client initially connects to the server, it will call this method to get a list of all of the stocks with their current prices.</span></span> <span data-ttu-id="3c3e5-167">Puede ejecutar de forma sincrónica y devolver el método `IEnumerable<Stock>` porque devuelve datos de la memoria.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-167">The method can run synchronously and return `IEnumerable<Stock>` because it's returning data from memory.</span></span>

<span data-ttu-id="3c3e5-168">Si el método tuvo que obtener los datos haciendo algo que implicaría espera, como una búsqueda de la base de datos o una llamada de servicio web, especificaría `Task<IEnumerable<Stock>>` como el valor devuelto para habilitar el procesamiento asincrónico.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-168">If the method had to get the data by doing something that would involve waiting, like a database lookup or a web service call, you would specify `Task<IEnumerable<Stock>>` as the return value to enable asynchronous processing.</span></span> <span data-ttu-id="3c3e5-169">Para obtener más información, consulte [Guía de la API de ASP.NET SignalR Hubs - Server - cuándo se debe ejecutar de forma asincrónica](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span><span class="sxs-lookup"><span data-stu-id="3c3e5-169">For more information, see [ASP.NET SignalR Hubs API Guide - Server - When to execute asynchronously](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span></span>

<span data-ttu-id="3c3e5-170">El `HubName` atributo especifica cómo la aplicación hará referencia el concentrador en el código de JavaScript en el cliente.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-170">The `HubName` attribute specifies how the app will reference the Hub in JavaScript code on the client.</span></span> <span data-ttu-id="3c3e5-171">El nombre predeterminado en el cliente si no usa este atributo, es una versión camelCase del nombre de clase, que en este caso sería `stockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-171">The default name on the client if you don't use this attribute, is a camelCase version of the class name, which in this case would be `stockTickerHub`.</span></span>

<span data-ttu-id="3c3e5-172">Como verá más adelante cuando se crea el `StockTicker` (clase), la aplicación crea una instancia singleton de esa clase en su estático `Instance` propiedad.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-172">As you'll see later when you create the `StockTicker` class, the app creates a singleton instance of that class in its static `Instance` property.</span></span> <span data-ttu-id="3c3e5-173">Esa instancia singleton de `StockTicker` está en memoria independientemente de cuántos clientes conexión o desconexión.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-173">That singleton instance of `StockTicker` is in memory no matter how many clients connect or disconnect.</span></span> <span data-ttu-id="3c3e5-174">Esa instancia es lo que el `GetAllStocks()` método se usa para devolver información bursátil actual.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-174">That instance is what the `GetAllStocks()` method uses to return current stock information.</span></span>

#### <a name="create-stocktickercs"></a><span data-ttu-id="3c3e5-175">Crear StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="3c3e5-175">Create StockTicker.cs</span></span>

1. <span data-ttu-id="3c3e5-176">En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **clase**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-176">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="3c3e5-177">Nombre de la clase *StockTicker* y agréguelo al proyecto.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-177">Name the class *StockTicker* and add it to the project.</span></span>

1. <span data-ttu-id="3c3e5-178">Reemplace el código en el *StockTicker.cs* archivo con este código:</span><span class="sxs-lookup"><span data-stu-id="3c3e5-178">Replace the code in the *StockTicker.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

<span data-ttu-id="3c3e5-179">Puesto que todos los subprocesos se ejecutará la misma instancia de código StockTicker, la clase StockTicker debe ser seguro para subprocesos.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-179">Since all threads will be running the same instance of StockTicker code, the StockTicker class has to be thread-safe.</span></span>

### <a name="examine-the-server-code"></a><span data-ttu-id="3c3e5-180">Examine el código de servidor</span><span class="sxs-lookup"><span data-stu-id="3c3e5-180">Examine the server code</span></span>

<span data-ttu-id="3c3e5-181">Si examina el código del servidor, le ayudará comprender el funcionamiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-181">If you examine the server code, it will help you understand how the app works.</span></span>

#### <a name="storing-the-singleton-instance-in-a-static-field"></a><span data-ttu-id="3c3e5-182">Almacenamiento de la instancia de singleton en un campo estático</span><span class="sxs-lookup"><span data-stu-id="3c3e5-182">Storing the singleton instance in a static field</span></span>

<span data-ttu-id="3c3e5-183">El código inicializa estático `_instance` campo que respalda la `Instance` propiedad con una instancia de la clase.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-183">The code initializes the static `_instance` field that backs the `Instance` property with an instance of the class.</span></span> <span data-ttu-id="3c3e5-184">Dado que el constructor es privado, es la única instancia de la clase que se puede crear la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-184">Because the constructor is private, it's the only instance of the class that the app can create.</span></span> <span data-ttu-id="3c3e5-185">La aplicación usa [la inicialización diferida](/dotnet/framework/performance/lazy-initialization) para el `_instance` campo.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-185">The app uses [Lazy initialization](/dotnet/framework/performance/lazy-initialization) for the `_instance` field.</span></span> <span data-ttu-id="3c3e5-186">No es por motivos de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-186">It's not for performance reasons.</span></span> <span data-ttu-id="3c3e5-187">Es para asegurarse de que la creación de instancias es segura para subprocesos.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-187">It's to make sure the instance creation is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

<span data-ttu-id="3c3e5-188">Cada vez que un cliente se conecta al servidor, una nueva instancia de la clase StockTickerHub que se ejecuta en un subproceso independiente Obtiene la instancia de singleton StockTicker desde el `StockTicker.Instance` propiedad estática, como vimos anteriormente en el `StockTickerHub` clase.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-188">Each time a client connects to the server, a new instance of the StockTickerHub class running in a separate thread gets the StockTicker singleton instance from the `StockTicker.Instance` static property, as you saw earlier in the `StockTickerHub` class.</span></span>

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a><span data-ttu-id="3c3e5-189">Almacenar datos de acciones en un elemento ConcurrentDictionary</span><span class="sxs-lookup"><span data-stu-id="3c3e5-189">Storing stock data in a ConcurrentDictionary</span></span>

<span data-ttu-id="3c3e5-190">El constructor inicializa el `_stocks` colección con datos de ejemplo stock, y `GetAllStocks` devuelve las existencias.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-190">The constructor initializes the `_stocks` collection with some sample stock data, and `GetAllStocks` returns the stocks.</span></span> <span data-ttu-id="3c3e5-191">Como vimos anteriormente, se devuelve esta colección de acciones por `StockTickerHub.GetAllStocks`, que es un método de servidor en el `Hub` clase que se pueden llamar los clientes.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-191">As you saw earlier, this collection of stocks is returned by `StockTickerHub.GetAllStocks`, which is a server method in the `Hub` class that clients can call.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

<span data-ttu-id="3c3e5-192">La colección de acciones se define como un [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) tipo de seguridad para subprocesos.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-192">The stocks collection is defined as a [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) type for thread safety.</span></span> <span data-ttu-id="3c3e5-193">Como alternativa, puede usar un [diccionario](https://msdn.microsoft.com/library/xfhwa508.aspx) de objetos y bloquear explícitamente el diccionario al realizar cambios en ella.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-193">As an alternative, you could use a [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) object and explicitly lock the dictionary when you make changes to it.</span></span>

<span data-ttu-id="3c3e5-194">Para esta aplicación de ejemplo, Aceptar es para almacenar datos de la aplicación en la memoria y perder los datos cuando la aplicación se desecha el `StockTicker` instancia.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-194">For this sample application, it's OK to store application data in memory and to lose the data when the app disposes of the  `StockTicker` instance.</span></span> <span data-ttu-id="3c3e5-195">En una aplicación real, podría funcionar con un almacén de datos back-end como una base de datos.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-195">In a real application, you would work with a back-end data store like a database.</span></span>

#### <a name="periodically-updating-stock-prices"></a><span data-ttu-id="3c3e5-196">Actualizaciones periódicas de cotizaciones</span><span class="sxs-lookup"><span data-stu-id="3c3e5-196">Periodically updating stock prices</span></span>

<span data-ttu-id="3c3e5-197">El constructor se inicia una `Timer` objeto que llama periódicamente a los métodos que actualizan los precios de las acciones de forma aleatoria.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-197">The constructor starts up a `Timer` object that periodically calls methods that update stock prices on a random basis.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 <span data-ttu-id="3c3e5-198">`Timer` las llamadas `UpdateStockPrices`, qué pasa null en el parámetro state.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-198">`Timer` calls `UpdateStockPrices`, which passes in null in the state parameter.</span></span> <span data-ttu-id="3c3e5-199">Antes de actualizar los precios, la aplicación toma un bloqueo en el `_updateStockPricesLock` objeto.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-199">Before updating prices, the app takes a lock on the `_updateStockPricesLock` object.</span></span> <span data-ttu-id="3c3e5-200">El código comprueba si otro subproceso ya se está actualizando los precios y, a continuación, llama a `TryUpdateStockPrice` en cada acción en la lista.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-200">The code checks if another thread is already updating prices, and then it calls `TryUpdateStockPrice` on each stock in the list.</span></span> <span data-ttu-id="3c3e5-201">El `TryUpdateStockPrice` método decide si debe cambiar el precio y cuánto para cambiarlo.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-201">The `TryUpdateStockPrice` method decides whether to change the stock price, and how much to change it.</span></span> <span data-ttu-id="3c3e5-202">Si cambia el precio, la aplicación llama a `BroadcastStockPrice` difundir el cambio de precio de las acciones a todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-202">If the stock price changes, the app calls `BroadcastStockPrice` to broadcast the stock price change to all connected clients.</span></span>

<span data-ttu-id="3c3e5-203">El `_updatingStockPrices` marca designado [volátil](https://msdn.microsoft.com/library/x13ttww7.aspx) para asegurarse de que es seguro para subprocesos.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-203">The `_updatingStockPrices` flag designated [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) to make sure it is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

<span data-ttu-id="3c3e5-204">En una aplicación real, el `TryUpdateStockPrice` método llamaría a un servicio web para buscar el precio.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-204">In a real application, the `TryUpdateStockPrice` method would call a web service to look up the price.</span></span> <span data-ttu-id="3c3e5-205">En este código, la aplicación utiliza un generador de números aleatorios para realizar cambios de forma aleatoria.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-205">In this code, the app uses a random number generator to make changes randomly.</span></span>

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a><span data-ttu-id="3c3e5-206">Obtener el contexto de SignalR para que la clase StockTicker puede difundir a los clientes</span><span class="sxs-lookup"><span data-stu-id="3c3e5-206">Getting the SignalR context so that the StockTicker class can broadcast to clients</span></span>

<span data-ttu-id="3c3e5-207">Dado que los cambios de precio se originan aquí en el `StockTicker` objeto, es el objeto que necesita llamar a un `updateStockPrice` método en todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-207">Because the price changes originate here in the `StockTicker` object, it's the object that needs to call an `updateStockPrice` method on all connected clients.</span></span> <span data-ttu-id="3c3e5-208">En un `Hub` (clase), que tiene una API para llamar a métodos de cliente, pero `StockTicker` no se deriva de la `Hub` clase y no tiene una referencia a ningún `Hub` objeto.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-208">In a `Hub` class, you have an API for calling client methods, but `StockTicker` doesn't derive from the `Hub` class and doesn't have a reference to any `Hub` object.</span></span> <span data-ttu-id="3c3e5-209">Para difundir a los clientes conectados, la `StockTicker` clase tiene que obtener la instancia del contexto de SignalR para el `StockTickerHub` clase y usarlo para llamar a métodos en los clientes.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-209">To broadcast to connected clients, the `StockTicker` class has to get the SignalR context instance for the `StockTickerHub` class and use that to call methods on clients.</span></span>

<span data-ttu-id="3c3e5-210">El código obtiene una referencia al contexto de SignalR cuando crea la instancia de la clase singleton, que hacen referencia a pasadas al constructor, y el constructor lo coloca en el `Clients` propiedad.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-210">The code gets a reference to the SignalR context when it creates the singleton class instance, passes that reference to the constructor, and the constructor puts it in the `Clients` property.</span></span>

<span data-ttu-id="3c3e5-211">Hay dos razones por qué desea obtener el contexto de una sola vez: obtener el contexto es una tarea costosa y hacer que una vez que garantiza que la aplicación conserva el orden de los mensajes enviados a los clientes que prefiera.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-211">There are two reasons why you want to get the context only once: getting the context is an expensive task, and getting it once makes sure the app preserves the intended order of messages sent to the clients.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

<span data-ttu-id="3c3e5-212">Obteniendo el `Clients` propiedad del contexto y ponerlo en la `StockTickerClient` propiedad le permite escribir código para llamar a los métodos de cliente que tiene el mismo aspecto como lo haría en un `Hub` clase.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-212">Getting the `Clients` property of the context and putting it in the `StockTickerClient` property lets you write code to call client methods that looks the same as it would in a `Hub` class.</span></span> <span data-ttu-id="3c3e5-213">Por ejemplo, puede escribir difundir a todos los clientes `Clients.All.updateStockPrice(stock)`.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-213">For instance, to broadcast to all clients you can write `Clients.All.updateStockPrice(stock)`.</span></span>

<span data-ttu-id="3c3e5-214">El `updateStockPrice` método al que está llamando en `BroadcastStockPrice` aún no existe.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-214">The `updateStockPrice` method that you're calling in `BroadcastStockPrice` doesn't exist yet.</span></span> <span data-ttu-id="3c3e5-215">Se agregará más adelante al escribir código que se ejecuta en el cliente.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-215">You'll add it later when you write code that runs on the client.</span></span> <span data-ttu-id="3c3e5-216">Puede hacer referencia a `updateStockPrice` aquí porque `Clients.All` es dinámico, lo que significa que la aplicación evaluará la expresión en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-216">You can refer to `updateStockPrice` here because `Clients.All` is dynamic, which means the app will evaluate the expression at runtime.</span></span> <span data-ttu-id="3c3e5-217">Cuando se ejecuta esta llamada al método, SignalR enviará el nombre del método y el valor del parámetro al cliente, y si el cliente tiene un método denominado `updateStockPrice`, llamará a ese método y pasar el valor del parámetro al la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-217">When this method call executes, SignalR will send the method name and the parameter value to the client, and if the client has a method named `updateStockPrice`, the app will call that method and pass the parameter value to it.</span></span>

<span data-ttu-id="3c3e5-218">`Clients.All` significa que se envíe a todos los clientes.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-218">`Clients.All` means send to all clients.</span></span> <span data-ttu-id="3c3e5-219">SignalR ofrece otras opciones para especificar qué clientes o grupos de clientes para enviar a.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-219">SignalR gives you other options to specify which clients or groups of clients to send to.</span></span> <span data-ttu-id="3c3e5-220">Para obtener más información, consulte [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="3c3e5-220">For more information, see [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span></span>

### <a name="register-the-signalr-route"></a><span data-ttu-id="3c3e5-221">Registre la ruta de SignalR</span><span class="sxs-lookup"><span data-stu-id="3c3e5-221">Register the SignalR route</span></span>

<span data-ttu-id="3c3e5-222">El servidor necesita saber qué dirección URL para interceptar y dirigir a SignalR.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-222">The server needs to know which URL to intercept and direct to SignalR.</span></span> <span data-ttu-id="3c3e5-223">Para ello, agregue una clase de inicio OWIN:</span><span class="sxs-lookup"><span data-stu-id="3c3e5-223">To do that, add an OWIN startup class:</span></span>

1. <span data-ttu-id="3c3e5-224">En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-224">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="3c3e5-225">En **Agregar nuevo elemento - SignalR.StockTicker** seleccione **instalado** > **Visual C#**   >  **Web** y a continuación, seleccione **clase de inicio OWIN**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-225">In **Add New Item - SignalR.StockTicker** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="3c3e5-226">Nombre de la clase *inicio* y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-226">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="3c3e5-227">Reemplace el código predeterminado en el *Startup.cs* archivo con este código:</span><span class="sxs-lookup"><span data-stu-id="3c3e5-227">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

<span data-ttu-id="3c3e5-228">Ahora ha terminado la configuración, el código de servidor.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-228">You have now finished setting up the server code.</span></span> <span data-ttu-id="3c3e5-229">En la siguiente sección, podrá configurar el cliente.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-229">In the next section, you'll set up the client.</span></span>

## <a name="set-up-the-client-code"></a><span data-ttu-id="3c3e5-230">Configurar el código de cliente</span><span class="sxs-lookup"><span data-stu-id="3c3e5-230">Set up the client code</span></span>

<span data-ttu-id="3c3e5-231">En esta sección, se establece el código que se ejecuta en el cliente.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-231">In this section, you set up the code that runs on the client.</span></span>

### <a name="create-the-html-page-and-javascript-file"></a><span data-ttu-id="3c3e5-232">Crear la página HTML y archivos de JavaScript</span><span class="sxs-lookup"><span data-stu-id="3c3e5-232">Create the HTML page and JavaScript file</span></span>

<span data-ttu-id="3c3e5-233">La página HTML mostrará los datos y el archivo JavaScript va a organizar los datos.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-233">The HTML page will display the data and the JavaScript file will organize the data.</span></span>

#### <a name="create-stocktickerhtml"></a><span data-ttu-id="3c3e5-234">Crear StockTicker.html</span><span class="sxs-lookup"><span data-stu-id="3c3e5-234">Create StockTicker.html</span></span>

<span data-ttu-id="3c3e5-235">En primer lugar, agregará al cliente HTML.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-235">First, you'll add the HTML client.</span></span>

1. <span data-ttu-id="3c3e5-236">En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **página HTML**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-236">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="3c3e5-237">Nombre del archivo *StockTicker* y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-237">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="3c3e5-238">Reemplace el código predeterminado en el *StockTicker.html* archivo con este código:</span><span class="sxs-lookup"><span data-stu-id="3c3e5-238">Replace the default code in the *StockTicker.html* file with this code:</span></span>

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    <span data-ttu-id="3c3e5-239">El código HTML crea una tabla con cinco columnas, una fila de encabezado y una fila de datos con una sola celda que abarca las cinco columnas.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-239">The HTML creates a table with five columns, a header row, and a data row with a single cell that spans all five columns.</span></span> <span data-ttu-id="3c3e5-240">La fila de datos muestra "Cargando..." momentáneamente cuando se inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-240">The data row shows "loading..." momentarily when the app starts.</span></span> <span data-ttu-id="3c3e5-241">Código de JavaScript quitará esa fila y agregar en su lugar de filas con datos recuperados del servidor de acciones.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-241">JavaScript code will remove that row and add in its place rows with stock data retrieved from the server.</span></span>

    <span data-ttu-id="3c3e5-242">Especifican las etiquetas de script:</span><span class="sxs-lookup"><span data-stu-id="3c3e5-242">The script tags specify:</span></span>

    * <span data-ttu-id="3c3e5-243">El archivo de script de jQuery.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-243">The jQuery script file.</span></span>

    * <span data-ttu-id="3c3e5-244">El archivo de script básico de SignalR.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-244">The SignalR core script file.</span></span>

    * <span data-ttu-id="3c3e5-245">El archivo de script de proxy de SignalR.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-245">The SignalR proxies script file.</span></span>

    * <span data-ttu-id="3c3e5-246">Un archivo de script StockTicker que creará más adelante.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-246">A StockTicker script file that you'll create later.</span></span>

    <span data-ttu-id="3c3e5-247">La aplicación genera dinámicamente el archivo de script de proxy de SignalR.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-247">The app dynamically generates the SignalR proxies script file.</span></span> <span data-ttu-id="3c3e5-248">Especifica la dirección URL "/ signalr/hubs" y define los métodos de proxy para los métodos de la clase de Hub, en este caso, para `StockTickerHub.GetAllStocks`.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-248">It specifies the "/signalr/hubs" URL and defines proxy methods for the methods on the Hub class, in this case, for `StockTickerHub.GetAllStocks`.</span></span> <span data-ttu-id="3c3e5-249">Si lo prefiere, puede generar manualmente este archivo de JavaScript mediante [SignalR utilidades](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span><span class="sxs-lookup"><span data-stu-id="3c3e5-249">If you prefer, you can generate this JavaScript file manually by using [SignalR Utilities](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span></span> <span data-ttu-id="3c3e5-250">No olvide deshabilitar la creación dinámica de archivos en el `MapHubs` llamada al método.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-250">Don't forget to disable dynamic file creation in the `MapHubs` method call.</span></span>

1. <span data-ttu-id="3c3e5-251">En **el Explorador de soluciones**, expanda **Scripts**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-251">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="3c3e5-252">Bibliotecas de scripts de jQuery y SignalR están visibles en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-252">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="3c3e5-253">El Administrador de paquetes instalará una versión posterior de los scripts de SignalR.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-253">The package manager will install a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="3c3e5-254">Actualizar las referencias de script en el bloque de código para que coincida con las versiones de los archivos de script en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-254">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

1. <span data-ttu-id="3c3e5-255">En **el Explorador de soluciones**, haga clic en *StockTicker.html*y, a continuación, seleccione **establecer como página de inicio**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-255">In **Solution Explorer**, right-click *StockTicker.html*, and then select **Set as Start Page**.</span></span>

#### <a name="create-stocktickerjs"></a><span data-ttu-id="3c3e5-256">Crear StockTicker.js</span><span class="sxs-lookup"><span data-stu-id="3c3e5-256">Create StockTicker.js</span></span>

<span data-ttu-id="3c3e5-257">Ahora cree el archivo de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-257">Now create the JavaScript file.</span></span>

1. <span data-ttu-id="3c3e5-258">En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **archivo JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-258">In **Solution Explorer**, right-click the project and select **Add** > **JavaScript File**.</span></span>

1. <span data-ttu-id="3c3e5-259">Nombre del archivo *StockTicker* y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-259">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="3c3e5-260">Agregue este código a la *StockTicker.js* archivo:</span><span class="sxs-lookup"><span data-stu-id="3c3e5-260">Add this code to the *StockTicker.js* file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a><span data-ttu-id="3c3e5-261">Examine el código de cliente</span><span class="sxs-lookup"><span data-stu-id="3c3e5-261">Examine the client code</span></span>

<span data-ttu-id="3c3e5-262">Si examina el código de cliente, le permitirá aprender cómo interactúa el código de cliente con el código del servidor para que la aplicación funcione.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-262">If you examine the client code, it will help you learn how the client code interacts with the server code to make the app work.</span></span>

#### <a name="starting-the-connection"></a><span data-ttu-id="3c3e5-263">A partir de la conexión</span><span class="sxs-lookup"><span data-stu-id="3c3e5-263">Starting the connection</span></span>

<span data-ttu-id="3c3e5-264">`$.connection` hace referencia a los servidores proxy de SignalR.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-264">`$.connection` refers to the SignalR proxies.</span></span> <span data-ttu-id="3c3e5-265">El código obtiene una referencia al proxy para el `StockTickerHub` clase y lo coloca en el `ticker` variable.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-265">The code gets a reference to the proxy for the `StockTickerHub` class and puts it in the `ticker` variable.</span></span> <span data-ttu-id="3c3e5-266">El nombre del proxy es el nombre que se ha establecido mediante la `HubName` atributo:</span><span class="sxs-lookup"><span data-stu-id="3c3e5-266">The proxy name is the name that was set by the `HubName` attribute:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

<span data-ttu-id="3c3e5-267">Después de definir todas las variables y funciones, la última línea de código en el archivo inicializa la conexión de SignalR mediante una llamada a SignalR `start` función.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-267">After you define all the variables and functions, the last line of code in the file initializes the SignalR connection by calling the SignalR `start` function.</span></span> <span data-ttu-id="3c3e5-268">El `start` función se ejecuta de forma asincrónica y devuelve un [jQuery aplazado objeto](http://api.jquery.com/category/deferred-object/).</span><span class="sxs-lookup"><span data-stu-id="3c3e5-268">The `start` function executes asynchronously and returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/).</span></span> <span data-ttu-id="3c3e5-269">Puede llamar a la función done para especificar la función que se va a llamar cuando la aplicación finalice la acción asincrónica.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-269">You can call the done function to specify the function to call when the app finishes the asynchronous action.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a><span data-ttu-id="3c3e5-270">Obtener todas las acciones</span><span class="sxs-lookup"><span data-stu-id="3c3e5-270">Getting all the stocks</span></span>

<span data-ttu-id="3c3e5-271">El `init` llamadas de función el `getAllStocks` funciones del servidor y usa la información que devuelve el servidor para actualizar la tabla stock.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-271">The `init` function calls the `getAllStocks` function on the server and uses the information that the server returns to update the stock table.</span></span> <span data-ttu-id="3c3e5-272">Tenga en cuenta que, de forma predeterminada, tiene que usar mayúsculas intercaladas en el cliente, incluso aunque el nombre del método con grafía pascal en el servidor.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-272">Notice that, by default, you have to use camelCasing on the client even though the method name is pascal-cased on the server.</span></span> <span data-ttu-id="3c3e5-273">La regla camelCasing solo se aplica a los métodos, no objetos.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-273">The camelCasing rule only applies to methods, not objects.</span></span> <span data-ttu-id="3c3e5-274">Por ejemplo, hacer referencia a `stock.Symbol` y `stock.Price`, no `stock.symbol` o `stock.price`.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-274">For example, you refer to `stock.Symbol` and `stock.Price`, not `stock.symbol` or `stock.price`.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

<span data-ttu-id="3c3e5-275">En el `init` método, la aplicación crea HTML para una fila de tabla para cada objeto de acción recibido del servidor mediante una llamada a `formatStock` a las propiedades de formato de la `stock` de objetos y, a continuación, al llamar a `supplant` para reemplazar los marcadores de posición en el `rowTemplate` variable con el `stock` valores de propiedad del objeto.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-275">In the `init` method, the app creates HTML for a table row for each stock object received from the server by calling `formatStock` to format properties of the `stock` object, and then by calling `supplant` to replace placeholders in the `rowTemplate` variable with the `stock` object property values.</span></span> <span data-ttu-id="3c3e5-276">El HTML resultante, a continuación, se anexa a la tabla stock.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-276">The resulting HTML is then appended to the stock table.</span></span>

> [!NOTE]
> <span data-ttu-id="3c3e5-277">Se llama a `init` pasándolo como un `callback` función que se ejecuta después de la asincrónica `start` función finaliza.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-277">You call `init` by passing it in as a `callback` function that executes after the asynchronous `start` function finishes.</span></span> <span data-ttu-id="3c3e5-278">Si llama a `init` como una instrucción de JavaScript independiente después de llamar a `start`, la función produciría un error porque se ejecutaría inmediatamente sin esperar a la función de inicio a fin de establecer la conexión.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-278">If you called `init` as a separate JavaScript statement after calling `start`, the function would fail because it would run immediately without waiting for the start function to finish establishing the connection.</span></span> <span data-ttu-id="3c3e5-279">En ese caso, el `init` función intentaría llamar a la `getAllStocks` funcionar antes de que la aplicación establece una conexión de servidor.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-279">In that case, the `init` function would try to call the `getAllStocks` function before the app establishes a server connection.</span></span>

#### <a name="getting-updated-stock-prices"></a><span data-ttu-id="3c3e5-280">Obtener cotizaciones bursátiles actualizadas</span><span class="sxs-lookup"><span data-stu-id="3c3e5-280">Getting updated stock prices</span></span>

<span data-ttu-id="3c3e5-281">Cuando el servidor cambia el precio de una acción, llama a la `updateStockPrice` en los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-281">When the server changes a stock's price, it calls the `updateStockPrice` on connected clients.</span></span> <span data-ttu-id="3c3e5-282">La aplicación agrega la función a la propiedad de cliente de la `stockTicker` proxy para que esté disponible para las llamadas desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-282">The app adds the function to the client property of the `stockTicker` proxy to make it available to calls from the server.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

<span data-ttu-id="3c3e5-283">El `updateStockPrice` formatos de función que un objeto estándar recibido del servidor en una tabla de filas del mismo modo que en el `init` función.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-283">The `updateStockPrice` function formats a stock object received from the server into a table row the same way as in the `init` function.</span></span> <span data-ttu-id="3c3e5-284">En lugar de anexar la fila a la tabla, se busca la fila actual de la acción en la tabla y reemplaza esa fila por uno nuevo.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-284">Instead of appending the row to the table, it finds the stock's current row in the table and replaces that row with the new one.</span></span>

## <a name="test-the-application"></a><span data-ttu-id="3c3e5-285">Probar la aplicación</span><span class="sxs-lookup"><span data-stu-id="3c3e5-285">Test the application</span></span>

<span data-ttu-id="3c3e5-286">Puede probar la aplicación para asegurarse de que se está trabajando.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-286">You can test the app to make sure it's working.</span></span> <span data-ttu-id="3c3e5-287">Verá todas las ventanas de explorador se muestran en la tabla de cotizaciones en directo con cotizaciones bursátiles fluctúa.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-287">You'll see all browser windows display the live stock table with stock prices fluctuating.</span></span>

1. <span data-ttu-id="3c3e5-288">En la barra de herramientas activar **depuración de scripts** y, a continuación, seleccione el botón Reproducir para ejecutar la aplicación en modo de depuración.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-288">In the toolbar, turn on **Script Debugging** and then select the play button to run the app in Debug mode.</span></span>

    ![Captura de pantalla del usuario activar el modo de depuración y seleccione Reproducir.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    <span data-ttu-id="3c3e5-290">Abrirá una ventana del explorador muestra la **Live tabla Stock**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-290">A browser window will open displaying the **Live Stock Table**.</span></span> <span data-ttu-id="3c3e5-291">La tabla stock inicialmente muestra la línea "Cargando...", a continuación, tras un tiempo breve, la aplicación muestra los datos de acciones iniciales y, a continuación, iniciar los precios de las acciones cambiar.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-291">The stock table initially shows the "loading..." line, then, after a short time, the app shows the initial stock data, and then the stock prices start to change.</span></span>

1. <span data-ttu-id="3c3e5-292">Copie la dirección URL desde el explorador, abra dos otros exploradores y pegue las direcciones URL en las barras de dirección.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-292">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

    <span data-ttu-id="3c3e5-293">La visualización del material inicial es el mismo que el primer explorador y los cambios se realizan simultáneamente.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-293">The initial stock display is the same as the first browser and changes happen simultaneously.</span></span>

1. <span data-ttu-id="3c3e5-294">Cierre todos los exploradores, abra un nuevo explorador y vaya a la misma dirección URL.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-294">Close all browsers, open a new browser, and go to the same URL.</span></span>

    <span data-ttu-id="3c3e5-295">El objeto singleton StockTicker siguiera ejecutándose en el servidor.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-295">The StockTicker singleton object continued to run in the server.</span></span> <span data-ttu-id="3c3e5-296">El **Live tabla Stock** muestra que siguieron las poblaciones a cambiar.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-296">The **Live Stock Table** shows that the stocks have continued to change.</span></span> <span data-ttu-id="3c3e5-297">Cambiar las cifras no ve la tabla con cero inicial.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-297">You don't see the initial table with zero change figures.</span></span>

1. <span data-ttu-id="3c3e5-298">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-298">Close the browser.</span></span>

## <a name="enable-logging"></a><span data-ttu-id="3c3e5-299">Habilite el registro</span><span class="sxs-lookup"><span data-stu-id="3c3e5-299">Enable logging</span></span>

<span data-ttu-id="3c3e5-300">SignalR tiene una función de registro integrados que se puede habilitar en el cliente para ayudar a solucionar problemas.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-300">SignalR has a built-in logging function that you can enable on the client to aid in troubleshooting.</span></span> <span data-ttu-id="3c3e5-301">En esta sección, habilite el registro y vea los ejemplos que muestran cómo los registros de indican qué de los siguientes métodos de transporte usa SignalR:</span><span class="sxs-lookup"><span data-stu-id="3c3e5-301">In this section, you enable logging and see examples that show how logs tell you which of the following transport methods SignalR is using:</span></span>

* <span data-ttu-id="3c3e5-302">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), admitido por IIS 8 y los exploradores actuales.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-302">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), supported by IIS 8 and current browsers.</span></span>

* <span data-ttu-id="3c3e5-303">[Eventos de servidor envió](http://en.wikipedia.org/wiki/Server-sent_events), admitido por los exploradores distintos de Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-303">[Server-sent events](http://en.wikipedia.org/wiki/Server-sent_events), supported by browsers other than Internet Explorer.</span></span>

* <span data-ttu-id="3c3e5-304">[Marco para siempre](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), admitido por Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-304">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supported by Internet Explorer.</span></span>

* <span data-ttu-id="3c3e5-305">[AJAX de sondeo prolongado](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), compatible con todos los exploradores.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-305">[Ajax long polling](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supported by all browsers.</span></span>

<span data-ttu-id="3c3e5-306">En una conexión determinada, SignalR elige el mejor método de transporte que admiten el servidor y el cliente.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-306">For any given connection, SignalR chooses the best transport method that both the server and the client support.</span></span>

1. <span data-ttu-id="3c3e5-307">Abra *StockTicker.js*.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-307">Open *StockTicker.js*.</span></span>

1. <span data-ttu-id="3c3e5-308">Agregue esta línea resaltada de código para habilitar el registro inmediatamente antes del código que inicializa la conexión al final del archivo:</span><span class="sxs-lookup"><span data-stu-id="3c3e5-308">Add this highlighted line of code to enable logging immediately before the code that initializes the connection at the end of the file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. <span data-ttu-id="3c3e5-309">Presione **F5** para ejecutar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-309">Press **F5** to run the project.</span></span>

1. <span data-ttu-id="3c3e5-310">Abra la ventana de herramientas de desarrollador de su explorador y seleccione la consola para ver los registros.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-310">Open your browser's developer tools window, and select the Console to see the logs.</span></span> <span data-ttu-id="3c3e5-311">Es posible que deba actualizar la página para ver los registros de SignalR negocie el método de transporte para una conexión nueva.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-311">You might have to refresh the page to see the logs of SignalR negotiating the transport method for a new connection.</span></span>

    * <span data-ttu-id="3c3e5-312">Si está ejecutando Internet Explorer 10 en Windows 8 (IIS 8), el método de transporte es **WebSockets**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-312">If you're running Internet Explorer 10 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

    * <span data-ttu-id="3c3e5-313">Si está ejecutando Internet Explorer 10 en Windows 7 (IIS 7.5), el método de transporte es **iframe**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-313">If you're running Internet Explorer 10 on Windows 7 (IIS 7.5), the transport method is **iframe**.</span></span>

    * <span data-ttu-id="3c3e5-314">Si está ejecutando el 19 de Firefox en Windows 8 (IIS 8), el método de transporte es **WebSockets**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-314">If you're running Firefox 19 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

        > [!TIP]
        > <span data-ttu-id="3c3e5-315">En Firefox, instale el complemento Firebug para obtener una ventana de consola.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-315">In Firefox, install the Firebug add-in to get a Console window.</span></span>

    * <span data-ttu-id="3c3e5-316">Si está ejecutando el 19 de Firefox en Windows 7 (IIS 7.5), el método de transporte es **servidor envió** eventos.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-316">If you're running Firefox 19 on Windows 7 (IIS 7.5), the transport method is **server-sent** events.</span></span>

## <a name="install-the-stockticker-sample"></a><span data-ttu-id="3c3e5-317">Instalar el ejemplo StockTicker</span><span class="sxs-lookup"><span data-stu-id="3c3e5-317">Install the StockTicker sample</span></span>

<span data-ttu-id="3c3e5-318">El [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) instala la aplicación StockTicker.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-318">The [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installs the StockTicker application.</span></span> <span data-ttu-id="3c3e5-319">El paquete NuGet incluye más características que la versión simplificada que ha creado desde cero.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-319">The NuGet package includes more features than the simplified version that you created from scratch.</span></span> <span data-ttu-id="3c3e5-320">En esta sección del tutorial, instale el paquete NuGet y revise las nuevas características y el código que implementa en ellos.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-320">In this section of the tutorial, you install the NuGet package and review the new features and the code that implements them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3c3e5-321">Si instala el paquete sin necesidad de realizar los pasos anteriores de este tutorial, debe agregar una clase de inicio OWIN al proyecto.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-321">If you install the package without performing the earlier steps of this tutorial, you must add an OWIN startup class to your project.</span></span> <span data-ttu-id="3c3e5-322">Este paso explica en este archivo readme.txt del paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-322">This readme.txt file for the NuGet package explains this step.</span></span>

### <a name="install-the-signalrsample-nuget-package"></a><span data-ttu-id="3c3e5-323">Instale el paquete SignalR.Sample NuGet</span><span class="sxs-lookup"><span data-stu-id="3c3e5-323">Install the SignalR.Sample NuGet package</span></span>

1. <span data-ttu-id="3c3e5-324">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-324">In **Solution Explorer**, right-click the project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="3c3e5-325">En **Administrador de paquetes de NuGet: SignalR.StockTicker**, seleccione **examinar**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-325">In **NuGet Package manager: SignalR.StockTicker**, select **Browse**.</span></span>

1. <span data-ttu-id="3c3e5-326">Desde **origen del paquete**, seleccione **nuget.org**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-326">From **Package source**, select **nuget.org**.</span></span>

1. <span data-ttu-id="3c3e5-327">Escriba *SignalR.Sample* en el cuadro de búsqueda y seleccione **Microsoft.AspNet.SignalR.Sample** > **instalar**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-327">Enter *SignalR.Sample* in the search box and select **Microsoft.AspNet.SignalR.Sample** > **Install**.</span></span>

1. <span data-ttu-id="3c3e5-328">En **el Explorador de soluciones**, expanda el *SignalR.Sample* carpeta.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-328">In **Solution Explorer**, expand the *SignalR.Sample* folder.</span></span>

    <span data-ttu-id="3c3e5-329">Instalar el paquete SignalR.Sample crea la carpeta y su contenido.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-329">Installing the SignalR.Sample package created the folder and its contents.</span></span>

1. <span data-ttu-id="3c3e5-330">En el *SignalR.Sample* carpeta, haga clic en *StockTicker.html*y, a continuación, seleccione **establecer como página principal**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-330">In the *SignalR.Sample* folder, right-click *StockTicker.html*, and then select **Set As Start Page**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3c3e5-331">Instalación de SignalR.Sample NuGet el paquete podría cambiar la versión de jQuery que tiene en su *Scripts* carpeta.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-331">Installing The SignalR.Sample NuGet package might change the version of jQuery that you have in your *Scripts* folder.</span></span> <span data-ttu-id="3c3e5-332">El nuevo *StockTicker.html* archivo que instala el paquete en el *SignalR.Sample* carpeta estará sincronizado con la versión de jQuery que instala el paquete, pero si desea ejecutar el original *StockTicker.html* archivo nuevo, es posible que deba actualizar la referencia de jQuery en la etiqueta de script en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-332">The new *StockTicker.html* file that the package installs in the *SignalR.Sample* folder will be in sync with the jQuery version that the package installs, but if you want to run your original *StockTicker.html* file again, you might have to update the jQuery reference in the script tag first.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="3c3e5-333">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="3c3e5-333">Run the application</span></span>

 <span data-ttu-id="3c3e5-334">La tabla que vio en la primera aplicación tenía características útiles.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-334">The table that you saw in the first app had useful features.</span></span> <span data-ttu-id="3c3e5-335">La aplicación de tablero de cotizaciones completo muestra las nuevas características: una ventana desplazable horizontal que muestra los datos de acciones y acciones que cambian de color tal y como se dividen y subiendo.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-335">The full stock ticker application shows new features: a horizontally scrolling window that shows the stock data and stocks that change color as they rise and fall.</span></span>

1. <span data-ttu-id="3c3e5-336">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-336">Press **F5** to run the app.</span></span>

     <span data-ttu-id="3c3e5-337">Al ejecutar la aplicación por primera vez, el mercado"" es "closed" y verá una tabla estática y una ventana del indicador que no es el desplazamiento.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-337">When you run the app for the first time, the "market" is "closed" and you see a static table and a ticker window that isn't scrolling.</span></span>

1. <span data-ttu-id="3c3e5-338">Seleccione **Open Market**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-338">Select **Open Market**.</span></span>

    ![Captura de pantalla de los tableros de cotizaciones en directo.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * <span data-ttu-id="3c3e5-340">El **Live bolsa** cuadro comienza a desplazarse horizontalmente y se inicia el servidor para difundir periódicamente los cambios de precio de las acciones de forma aleatoria.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-340">The **Live Stock Ticker** box starts to scroll horizontally, and the server starts to periodically broadcast stock price changes on a random basis.</span></span>

    * <span data-ttu-id="3c3e5-341">Cada vez que cambia de cotización, la aplicación actualiza la **Live tabla Stock** y el **tablero de cotizaciones en directo**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-341">Each time a stock price changes, the app updates both the **Live Stock Table** and the **Live Stock Ticker**.</span></span>

    * <span data-ttu-id="3c3e5-342">Cuando cambie el precio de una acción es positivo, la aplicación muestra las existencias con un fondo verde.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-342">When a stock's price change is positive, the app shows the stock with a green background.</span></span>

    * <span data-ttu-id="3c3e5-343">Cuando el cambio es negativo, la aplicación muestra las existencias con un fondo rojo.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-343">When the change is negative, the app shows the stock with a red background.</span></span>

1. <span data-ttu-id="3c3e5-344">Seleccione **cerrar mercado**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-344">Select **Close Market**.</span></span>

    * <span data-ttu-id="3c3e5-345">La tabla actualiza stop.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-345">The table updates stop.</span></span>

    * <span data-ttu-id="3c3e5-346">El teletipo detiene el desplazamiento.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-346">The ticker stops scrolling.</span></span>

1. <span data-ttu-id="3c3e5-347">Seleccione **restablecer**.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-347">Select **Reset**.</span></span>

    * <span data-ttu-id="3c3e5-348">Se restablece todos los datos de acciones.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-348">All stock data is reset.</span></span>

    * <span data-ttu-id="3c3e5-349">La aplicación restaura el estado inicial antes de los cambios de precio a trabajar.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-349">The app restores the initial state before price changes started.</span></span>

1. <span data-ttu-id="3c3e5-350">Copie la dirección URL desde el explorador, abra dos otros exploradores y pegue las direcciones URL en las barras de dirección.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-350">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="3c3e5-351">Verá los mismos datos que se actualiza dinámicamente al mismo tiempo en cada explorador.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-351">You see the same data dynamically updated at the same time in each browser.</span></span>

1. <span data-ttu-id="3c3e5-352">Cuando se selecciona cualquiera de los controles, todos los exploradores responden la misma manera a la vez.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-352">When you select any of the controls, all browsers respond the same way at the same time.</span></span>

### <a name="live-stock-ticker-display"></a><span data-ttu-id="3c3e5-353">Visualización de tablero de cotizaciones en directo</span><span class="sxs-lookup"><span data-stu-id="3c3e5-353">Live Stock Ticker display</span></span>

<span data-ttu-id="3c3e5-354">El **Live bolsa** presentación es una lista sin ordenar en un `<div>` elemento con formato en una sola línea, los estilos CSS.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-354">The **Live Stock Ticker** display is an unordered list in a `<div>` element formatted into a single line by CSS styles.</span></span> <span data-ttu-id="3c3e5-355">La aplicación se inicializa y actualiza el tablero del mismo modo que la tabla: reemplazando los marcadores de posición en una `<li>` cadena de plantilla y agregar dinámicamente la `<li>` elementos a la `<ul>` elemento.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-355">The app initializes and updates the ticker the same way as the table: by replacing placeholders in an `<li>` template string and dynamically adding the `<li>` elements to the `<ul>` element.</span></span> <span data-ttu-id="3c3e5-356">La aplicación incluye el desplazamiento con jQuery `animate` función para variar el margen izquierda de la lista sin ordenar dentro de la `<div>`.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-356">The app includes  scrolling by using the jQuery `animate` function to vary the margin-left of the unordered list within the `<div>`.</span></span>

#### <a name="signalrsample-stocktickerhtml"></a><span data-ttu-id="3c3e5-357">SignalR.Sample StockTicker.html</span><span class="sxs-lookup"><span data-stu-id="3c3e5-357">SignalR.Sample StockTicker.html</span></span>

<span data-ttu-id="3c3e5-358">El tablero de cotizaciones código HTML:</span><span class="sxs-lookup"><span data-stu-id="3c3e5-358">The stock ticker HTML code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a><span data-ttu-id="3c3e5-359">SignalR.Sample StockTicker.css</span><span class="sxs-lookup"><span data-stu-id="3c3e5-359">SignalR.Sample StockTicker.css</span></span>

<span data-ttu-id="3c3e5-360">El tablero de cotizaciones código CSS:</span><span class="sxs-lookup"><span data-stu-id="3c3e5-360">The stock ticker CSS code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a><span data-ttu-id="3c3e5-361">SignalR.Sample SignalR.StockTicker.js</span><span class="sxs-lookup"><span data-stu-id="3c3e5-361">SignalR.Sample SignalR.StockTicker.js</span></span>

<span data-ttu-id="3c3e5-362">Desplácese por el código jQuery que hace:</span><span class="sxs-lookup"><span data-stu-id="3c3e5-362">The jQuery code that makes it scroll:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a><span data-ttu-id="3c3e5-363">Métodos adicionales en el servidor que el cliente puede llamar a</span><span class="sxs-lookup"><span data-stu-id="3c3e5-363">Additional methods on the server that the client can call</span></span>

<span data-ttu-id="3c3e5-364">Para agregar flexibilidad a la aplicación, hay métodos adicionales que se puede llamar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-364">To add flexibility to the app, there are additional methods the app can call.</span></span>

#### <a name="signalrsample-stocktickerhubcs"></a><span data-ttu-id="3c3e5-365">SignalR.Sample StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="3c3e5-365">SignalR.Sample StockTickerHub.cs</span></span>

<span data-ttu-id="3c3e5-366">La `StockTickerHub` clase define cuatro métodos adicionales que puede llamar el cliente:</span><span class="sxs-lookup"><span data-stu-id="3c3e5-366">The `StockTickerHub` class defines four additional methods that the client can call:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

<span data-ttu-id="3c3e5-367">Las llamadas de aplicación `OpenMarket`, `CloseMarket`, y `Reset` en respuesta a los botones en la parte superior de la página.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-367">The app calls `OpenMarket`, `CloseMarket`, and `Reset` in response to the buttons at the top of the page.</span></span> <span data-ttu-id="3c3e5-368">Muestra el patrón de un cliente desencadenar un cambio de estado que se propagaban inmediatamente a todos los clientes.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-368">They demonstrate the pattern of one client triggering a change in state immediately propagated to all clients.</span></span> <span data-ttu-id="3c3e5-369">Cada uno de estos métodos llama a un método el `StockTicker` clase que hace que el cambio de estado de mercado y, a continuación, transmite el estado nueva.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-369">Each of these methods calls a method in the `StockTicker` class that causes the market state change and then broadcasts the new state.</span></span>

#### <a name="signalrsample-stocktickercs"></a><span data-ttu-id="3c3e5-370">SignalR.Sample StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="3c3e5-370">SignalR.Sample StockTicker.cs</span></span>

<span data-ttu-id="3c3e5-371">En el `StockTicker` (clase), la aplicación mantiene el estado del mercado con un `MarketState` propiedad que devuelve un `MarketState` valor enum:</span><span class="sxs-lookup"><span data-stu-id="3c3e5-371">In the `StockTicker` class, the app maintains the state of the market with a `MarketState` property that returns a `MarketState` enum value:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

<span data-ttu-id="3c3e5-372">Cada uno de los métodos que cambian el estado de mercado hacerlo dentro de un bloqueo porque el `StockTicker` clase debe ser seguro para subprocesos:</span><span class="sxs-lookup"><span data-stu-id="3c3e5-372">Each of the methods that change the market state do so inside a lock block because the `StockTicker` class has to be thread-safe:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

<span data-ttu-id="3c3e5-373">Para asegurarse de que este código es seguro para subprocesos, el `_marketState` campo que respalda la `MarketState` propiedad designada `volatile`:</span><span class="sxs-lookup"><span data-stu-id="3c3e5-373">To make sure this code is thread-safe, the `_marketState` field that backs the `MarketState` property designated `volatile`:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

<span data-ttu-id="3c3e5-374">El `BroadcastMarketStateChange` y `BroadcastMarketReset` métodos son similares al método BroadcastStockPrice que ya hemos visto, salvo que llaman a diferentes métodos definidos en el cliente:</span><span class="sxs-lookup"><span data-stu-id="3c3e5-374">The `BroadcastMarketStateChange` and `BroadcastMarketReset` methods are similar to the BroadcastStockPrice method that you already saw, except they call different methods defined at the client:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a><span data-ttu-id="3c3e5-375">Funciones adicionales en el cliente que el servidor puede llamar a</span><span class="sxs-lookup"><span data-stu-id="3c3e5-375">Additional functions on the client that the server can call</span></span>

<span data-ttu-id="3c3e5-376">El `updateStockPrice` función controla ahora en la tabla y la presentación de tableros de cotizaciones y usa `jQuery.Color` parpadea colores verde y rojo.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-376">The `updateStockPrice` function now handles both the table and the ticker display, and it uses `jQuery.Color` to flash red and green colors.</span></span>

<span data-ttu-id="3c3e5-377">Nuevas funciones en *SignalR.StockTicker.js* habilitar y deshabilitar los botones según el estado del mercado.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-377">New functions in *SignalR.StockTicker.js* enable and disable the buttons based on market state.</span></span> <span data-ttu-id="3c3e5-378">Se iniciar o detener también el **Live bolsa** desplazamiento horizontal.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-378">They also stop or start the **Live Stock Ticker** horizontal scrolling.</span></span> <span data-ttu-id="3c3e5-379">Puesto que muchas de las funciones se agregan a `ticker.client`, la aplicación usa el [jQuery ampliar la función](http://api.jquery.com/jQuery.extend/) para agregarlos.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-379">Since many functions are being added to `ticker.client`, the app uses the [jQuery extend function](http://api.jquery.com/jQuery.extend/) to add them.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a><span data-ttu-id="3c3e5-380">Programa de instalación de cliente adicionales después de establecer la conexión</span><span class="sxs-lookup"><span data-stu-id="3c3e5-380">Additional client setup after establishing the connection</span></span>

<span data-ttu-id="3c3e5-381">Una vez que el cliente establece la conexión, tiene cierto trabajo adicional para hacer:</span><span class="sxs-lookup"><span data-stu-id="3c3e5-381">After the client establishes the connection, it has some additional work to do:</span></span>

* <span data-ttu-id="3c3e5-382">Averiguar si el mercado está abierto o cerrado para llamar a la correspondiente `marketOpened` o `marketClosed` función.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-382">Find out if the market is open or closed to call the appropriate `marketOpened` or `marketClosed` function.</span></span>

* <span data-ttu-id="3c3e5-383">Adjunte las llamadas al método de servidor a los botones.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-383">Attach the server method calls to the buttons.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

<span data-ttu-id="3c3e5-384">Los métodos de servidor no conectado con los botones hasta que una vez que la aplicación establece la conexión.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-384">The server methods aren't wired up to the buttons until after the app establishes the connection.</span></span> <span data-ttu-id="3c3e5-385">Es por lo que el código no puede llamar a los métodos de servidor antes de estar disponibles.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-385">It's so the code can't call the server methods before they're available.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3c3e5-386">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="3c3e5-386">Additional resources</span></span>

<span data-ttu-id="3c3e5-387">En este tutorial ha aprendido cómo programar una aplicación de SignalR que transmite mensajes desde el servidor a todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-387">In this tutorial you've learned how to program a SignalR application that broadcasts messages from the server to all connected clients.</span></span> <span data-ttu-id="3c3e5-388">Ahora puede difundir mensajes de forma periódica y en respuesta a las notificaciones desde cualquier cliente.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-388">Now you can broadcast messages on a periodic basis and in response to notifications from any client.</span></span> <span data-ttu-id="3c3e5-389">Puede usar el concepto de la instancia de singleton multiproceso para mantener el estado del servidor en escenarios de juego en línea multijugador.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-389">You can use the concept of multi-threaded singleton instance to maintain server state in multi-player online game scenarios.</span></span> <span data-ttu-id="3c3e5-390">Para obtener un ejemplo, vea [ShootR juego basado en SignalR](https://github.com/NTaylorMullen/ShootR).</span><span class="sxs-lookup"><span data-stu-id="3c3e5-390">For an example, see [the ShootR game based on SignalR](https://github.com/NTaylorMullen/ShootR).</span></span>

<span data-ttu-id="3c3e5-391">Para ver tutoriales que muestran escenarios de comunicación punto a punto, consulte [Introducción a SignalR](introduction-to-signalr.md) y [actualizar en tiempo real con SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="3c3e5-391">For tutorials that show peer-to-peer communication scenarios, see [Getting Started with SignalR](introduction-to-signalr.md) and [Real-Time Updating with SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span></span>

<span data-ttu-id="3c3e5-392">Para obtener más información acerca de SignalR, consulte los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="3c3e5-392">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="3c3e5-393">SignalR de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3c3e5-393">ASP.NET SignalR</span></span>](../../index.md)
* [<span data-ttu-id="3c3e5-394">Proyecto de SignalR</span><span class="sxs-lookup"><span data-stu-id="3c3e5-394">SignalR Project</span></span>](http://signalr.net/)
* [<span data-ttu-id="3c3e5-395">SignalR GitHub y ejemplos</span><span class="sxs-lookup"><span data-stu-id="3c3e5-395">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)
* [<span data-ttu-id="3c3e5-396">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="3c3e5-396">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="3c3e5-397">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="3c3e5-397">Next steps</span></span>

<span data-ttu-id="3c3e5-398">En este tutorial, puede:</span><span class="sxs-lookup"><span data-stu-id="3c3e5-398">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3c3e5-399">Crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="3c3e5-399">Created the project</span></span>
> * <span data-ttu-id="3c3e5-400">Configurar el código de servidor</span><span class="sxs-lookup"><span data-stu-id="3c3e5-400">Set up the server code</span></span>
> * <span data-ttu-id="3c3e5-401">Examinar el código del servidor</span><span class="sxs-lookup"><span data-stu-id="3c3e5-401">Examined the server code</span></span>
> * <span data-ttu-id="3c3e5-402">Configurar el código de cliente</span><span class="sxs-lookup"><span data-stu-id="3c3e5-402">Set up the client code</span></span>
> * <span data-ttu-id="3c3e5-403">Examinar el código de cliente</span><span class="sxs-lookup"><span data-stu-id="3c3e5-403">Examined the client code</span></span>
> * <span data-ttu-id="3c3e5-404">Probar la aplicación</span><span class="sxs-lookup"><span data-stu-id="3c3e5-404">Tested the application</span></span>
> * <span data-ttu-id="3c3e5-405">Registro habilitado</span><span class="sxs-lookup"><span data-stu-id="3c3e5-405">Enabled logging</span></span>

<span data-ttu-id="3c3e5-406">Avance al siguiente artículo para obtener información sobre cómo crear una aplicación web en tiempo real que usa ASP.NET SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="3c3e5-406">Advance to the next article to learn how to create a real-time web application that uses ASP.NET SignalR 2.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="3c3e5-407">Crear aplicación web en tiempo real con SignalR</span><span class="sxs-lookup"><span data-stu-id="3c3e5-407">Create real-time web app with SignalR</span></span>](real-time-web-applications-with-signalr.md)