---
title: Administración de memoria y patrones en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo se administra la memoria en ASP.NET Core y cómo funciona el recolector de elementos no utilizados (GC).
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: performance/memory
ms.openlocfilehash: 85e34c9faa31a1020a4200eb99003455ca435ec3
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880948"
---
# <a name="memory-management-and-garbage-collection-gc-in-aspnet-core"></a><span data-ttu-id="08d0c-103">Administración de memoria y recolección de elementos no utilizados (GC) en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="08d0c-103">Memory management and garbage collection (GC) in ASP.NET Core</span></span>

<span data-ttu-id="08d0c-104">Por [Sébastien Ros](https://github.com/sebastienros) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="08d0c-104">By [Sébastien Ros](https://github.com/sebastienros) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="08d0c-105">La administración de memoria es compleja, incluso en un marco administrado como .NET.</span><span class="sxs-lookup"><span data-stu-id="08d0c-105">Memory management is complex, even in a managed framework like .NET.</span></span> <span data-ttu-id="08d0c-106">Analizar y comprender los problemas de memoria puede ser desafiante.</span><span class="sxs-lookup"><span data-stu-id="08d0c-106">Analyzing and understanding memory issues can be challenging.</span></span> <span data-ttu-id="08d0c-107">En este artículo:</span><span class="sxs-lookup"><span data-stu-id="08d0c-107">This article:</span></span>

* <span data-ttu-id="08d0c-108">Estaba motivada por muchos problemas de *pérdida de memoria* y el *GC no funciona* .</span><span class="sxs-lookup"><span data-stu-id="08d0c-108">Was motivated by many *memory leak* and *GC not working* issues.</span></span> <span data-ttu-id="08d0c-109">La mayoría de estos problemas se debieron a no comprender cómo funciona el consumo de memoria en .NET Core o no comprender cómo se mide.</span><span class="sxs-lookup"><span data-stu-id="08d0c-109">Most of these issues were caused by not understanding how memory consumption works in .NET Core, or not understanding how it's measured.</span></span>
* <span data-ttu-id="08d0c-110">Muestra el uso de memoria problemático y sugiere enfoques alternativos.</span><span class="sxs-lookup"><span data-stu-id="08d0c-110">Demonstrates problematic memory use, and suggests alternative approaches.</span></span>

## <a name="how-garbage-collection-gc-works-in-net-core"></a><span data-ttu-id="08d0c-111">Cómo funciona la recolección de elementos no utilizados (GC) en .NET Core</span><span class="sxs-lookup"><span data-stu-id="08d0c-111">How garbage collection (GC) works in .NET Core</span></span>

<span data-ttu-id="08d0c-112">El GC asigna segmentos del montón donde cada segmento es un intervalo de memoria contiguo.</span><span class="sxs-lookup"><span data-stu-id="08d0c-112">The GC allocates heap segments where each segment is a contiguous range of memory.</span></span> <span data-ttu-id="08d0c-113">Los objetos colocados en el montón se clasifican en una de estas tres generaciones: 0, 1 o 2.</span><span class="sxs-lookup"><span data-stu-id="08d0c-113">Objects placed in the heap are categorized into one of 3 generations: 0, 1, or 2.</span></span> <span data-ttu-id="08d0c-114">La generación determina la frecuencia con la que el GC intenta liberar memoria en los objetos administrados a los que la aplicación ya no hace referencia.</span><span class="sxs-lookup"><span data-stu-id="08d0c-114">The generation determines the frequency the GC attempts to release memory on managed objects that are no longer referenced by the app.</span></span> <span data-ttu-id="08d0c-115">Las generaciones con números inferiores son GC con más frecuencia.</span><span class="sxs-lookup"><span data-stu-id="08d0c-115">Lower numbered generations are GC'd more frequently.</span></span>

<span data-ttu-id="08d0c-116">Los objetos se mueven de una generación a otra en función de su duración.</span><span class="sxs-lookup"><span data-stu-id="08d0c-116">Objects are moved from one generation to another based on their lifetime.</span></span> <span data-ttu-id="08d0c-117">A medida que los objetos viven más tiempo, se mueven a una generación superior.</span><span class="sxs-lookup"><span data-stu-id="08d0c-117">As objects live longer, they are moved into a higher generation.</span></span> <span data-ttu-id="08d0c-118">Como se mencionó anteriormente, las generaciones mayores son menos a menudo.</span><span class="sxs-lookup"><span data-stu-id="08d0c-118">As mentioned previously, higher generations are GC'd less often.</span></span> <span data-ttu-id="08d0c-119">Los objetos de corta duración siempre permanecen en la generación 0.</span><span class="sxs-lookup"><span data-stu-id="08d0c-119">Short term lived objects always remain in generation 0.</span></span> <span data-ttu-id="08d0c-120">Por ejemplo, los objetos a los que se hace referencia durante la vida de una solicitud Web son de corta duración.</span><span class="sxs-lookup"><span data-stu-id="08d0c-120">For example, objects that are referenced during the life of a web request are short lived.</span></span> <span data-ttu-id="08d0c-121">Los [Singleton](xref:fundamentals/dependency-injection#service-lifetimes) de nivel de aplicación generalmente migran a la generación 2.</span><span class="sxs-lookup"><span data-stu-id="08d0c-121">Application level [singletons](xref:fundamentals/dependency-injection#service-lifetimes) generally migrate to generation 2.</span></span>

<span data-ttu-id="08d0c-122">Cuando se inicia una aplicación ASP.NET Core, el GC:</span><span class="sxs-lookup"><span data-stu-id="08d0c-122">When an ASP.NET Core app starts, the GC:</span></span>

* <span data-ttu-id="08d0c-123">Reserva memoria para los segmentos de montón iniciales.</span><span class="sxs-lookup"><span data-stu-id="08d0c-123">Reserves some memory for the initial heap segments.</span></span>
* <span data-ttu-id="08d0c-124">Confirma una pequeña parte de la memoria cuando se carga el tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="08d0c-124">Commits a small portion of memory when the runtime is loaded.</span></span>

<span data-ttu-id="08d0c-125">Las asignaciones de memoria anteriores se realizan por motivos de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="08d0c-125">The preceding memory allocations are done for performance reasons.</span></span> <span data-ttu-id="08d0c-126">La ventaja de rendimiento procede de los segmentos del montón en la memoria contigua.</span><span class="sxs-lookup"><span data-stu-id="08d0c-126">The performance benefit comes from heap segments in contiguous memory.</span></span>

### <a name="call-gccollect"></a><span data-ttu-id="08d0c-127">Llame a GC. Impuesto</span><span class="sxs-lookup"><span data-stu-id="08d0c-127">Call GC.Collect</span></span>

<span data-ttu-id="08d0c-128">Llamando a [GC. Recopilar](xref:System.GC.Collect*) explícitamente:</span><span class="sxs-lookup"><span data-stu-id="08d0c-128">Calling [GC.Collect](xref:System.GC.Collect*) explicitly:</span></span>

* <span data-ttu-id="08d0c-129">**No** debe realizarse en las aplicaciones de ASP.net Core de producción.</span><span class="sxs-lookup"><span data-stu-id="08d0c-129">Should **not** be done by production ASP.NET Core apps.</span></span>
* <span data-ttu-id="08d0c-130">Resulta útil para investigar pérdidas de memoria.</span><span class="sxs-lookup"><span data-stu-id="08d0c-130">Is useful when investigating memory leaks.</span></span>
* <span data-ttu-id="08d0c-131">Al investigar, comprueba que el GC ha quitado todos los objetos pendientes de la memoria, por lo que se puede medir la memoria.</span><span class="sxs-lookup"><span data-stu-id="08d0c-131">When investigating, verifies the GC has removed all dangling objects from memory so memory can be measured.</span></span>

## <a name="analyzing-the-memory-usage-of-an-app"></a><span data-ttu-id="08d0c-132">Analizar el uso de memoria de una aplicación</span><span class="sxs-lookup"><span data-stu-id="08d0c-132">Analyzing the memory usage of an app</span></span>

<span data-ttu-id="08d0c-133">Las herramientas dedicadas pueden ayudar a analizar el uso de memoria:</span><span class="sxs-lookup"><span data-stu-id="08d0c-133">Dedicated tools can help analyzing memory usage:</span></span>

- <span data-ttu-id="08d0c-134">Recuento de referencias de objeto</span><span class="sxs-lookup"><span data-stu-id="08d0c-134">Counting object references</span></span>
- <span data-ttu-id="08d0c-135">Medir la cantidad de impacto que el GC tiene en el uso de CPU</span><span class="sxs-lookup"><span data-stu-id="08d0c-135">Measuring how much impact the GC has on CPU usage</span></span>
- <span data-ttu-id="08d0c-136">Medir el espacio de memoria usado para cada generación</span><span class="sxs-lookup"><span data-stu-id="08d0c-136">Measuring memory space used for each generation</span></span>

<span data-ttu-id="08d0c-137">Use las siguientes herramientas para analizar el uso de memoria:</span><span class="sxs-lookup"><span data-stu-id="08d0c-137">Use the following tools to analyze memory usage:</span></span>

* <span data-ttu-id="08d0c-138">[dotnet-Trace](/dotnet/core/diagnostics/dotnet-trace): puede usarse en equipos de producción.</span><span class="sxs-lookup"><span data-stu-id="08d0c-138">[dotnet-trace](/dotnet/core/diagnostics/dotnet-trace): Can be  used on production machines.</span></span>
* [<span data-ttu-id="08d0c-139">Analizar el uso de memoria sin el depurador de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08d0c-139">Analyze memory usage without the Visual Studio debugger</span></span>](/visualstudio/profiling/memory-usage-without-debugging2)
* [<span data-ttu-id="08d0c-140">Análisis del uso de memoria en Visual Studio</span><span class="sxs-lookup"><span data-stu-id="08d0c-140">Profile memory usage in Visual Studio</span></span>](/visualstudio/profiling/memory-usage)

### <a name="detecting-memory-issues"></a><span data-ttu-id="08d0c-141">Detección de problemas de memoria</span><span class="sxs-lookup"><span data-stu-id="08d0c-141">Detecting memory issues</span></span>

<span data-ttu-id="08d0c-142">Se puede usar el administrador de tareas para hacerse una idea de la cantidad de memoria que está usando una aplicación de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="08d0c-142">Task Manager can be used to get an idea of how much memory an ASP.NET app is using.</span></span> <span data-ttu-id="08d0c-143">El valor de memoria del administrador de tareas:</span><span class="sxs-lookup"><span data-stu-id="08d0c-143">The Task Manager memory value:</span></span>

* <span data-ttu-id="08d0c-144">Representa la cantidad de memoria que utiliza el proceso ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="08d0c-144">Represents the amount of memory that is used by the ASP.NET process.</span></span>
* <span data-ttu-id="08d0c-145">Incluye los objetos vivos de la aplicación y otros consumidores de memoria, como el uso de memoria nativa.</span><span class="sxs-lookup"><span data-stu-id="08d0c-145">Includes the app's living objects and other memory consumers such as native memory usage.</span></span>

<span data-ttu-id="08d0c-146">Si el valor de memoria del administrador de tareas aumenta indefinidamente y nunca se reduce, la aplicación tiene una pérdida de memoria.</span><span class="sxs-lookup"><span data-stu-id="08d0c-146">If the Task Manager memory value increases indefinitely and never flattens out, the app has a memory leak.</span></span> <span data-ttu-id="08d0c-147">En las secciones siguientes se muestran y explican varios patrones de uso de memoria.</span><span class="sxs-lookup"><span data-stu-id="08d0c-147">The following sections demonstrate and explain several memory usage patterns.</span></span>

## <a name="sample-display-memory-usage-app"></a><span data-ttu-id="08d0c-148">Ejemplo de aplicación de uso de memoria de muestra</span><span class="sxs-lookup"><span data-stu-id="08d0c-148">Sample display memory usage app</span></span>

<span data-ttu-id="08d0c-149">La [aplicación de ejemplo MemoryLeak](https://github.com/sebastienros/memoryleak) está disponible en github.</span><span class="sxs-lookup"><span data-stu-id="08d0c-149">The [MemoryLeak sample app](https://github.com/sebastienros/memoryleak) is available on GitHub.</span></span> <span data-ttu-id="08d0c-150">La aplicación MemoryLeak:</span><span class="sxs-lookup"><span data-stu-id="08d0c-150">The MemoryLeak app:</span></span>

* <span data-ttu-id="08d0c-151">Incluye un controlador de diagnóstico que recopila datos de GC y memoria en tiempo real de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="08d0c-151">Includes a diagnostic controller that gathers real-time memory and GC data for the app.</span></span>
* <span data-ttu-id="08d0c-152">Tiene una página de índice que muestra los datos de memoria y GC.</span><span class="sxs-lookup"><span data-stu-id="08d0c-152">Has an Index page that displays the memory and GC data.</span></span> <span data-ttu-id="08d0c-153">La página de índice se actualiza cada segundo.</span><span class="sxs-lookup"><span data-stu-id="08d0c-153">The Index page is refreshed every second.</span></span>
* <span data-ttu-id="08d0c-154">Contiene un controlador de API que proporciona varios patrones de carga de memoria.</span><span class="sxs-lookup"><span data-stu-id="08d0c-154">Contains an API controller that provides various memory load patterns.</span></span>
* <span data-ttu-id="08d0c-155">No es una herramienta compatible; sin embargo, se puede usar para mostrar patrones de uso de memoria de ASP.NET Core aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="08d0c-155">Is not a supported tool, however, it can be used to display memory usage patterns of ASP.NET Core apps.</span></span>

<span data-ttu-id="08d0c-156">Ejecute MemoryLeak.</span><span class="sxs-lookup"><span data-stu-id="08d0c-156">Run MemoryLeak.</span></span> <span data-ttu-id="08d0c-157">La memoria asignada aumenta lentamente hasta que se produce un GC.</span><span class="sxs-lookup"><span data-stu-id="08d0c-157">Allocated memory slowly increases until a GC occurs.</span></span> <span data-ttu-id="08d0c-158">La memoria aumenta porque la herramienta asigna el objeto personalizado para capturar los datos.</span><span class="sxs-lookup"><span data-stu-id="08d0c-158">Memory increases because the tool allocates custom object to capture data.</span></span> <span data-ttu-id="08d0c-159">La siguiente imagen muestra la página de índice de MemoryLeak cuando se produce un GC de gen. 0.</span><span class="sxs-lookup"><span data-stu-id="08d0c-159">The following image shows the MemoryLeak Index page when a Gen 0 GC occurs.</span></span> <span data-ttu-id="08d0c-160">El gráfico muestra 0 RPS (solicitudes por segundo) porque no se ha llamado a ningún punto de conexión de API del controlador de API.</span><span class="sxs-lookup"><span data-stu-id="08d0c-160">The chart shows 0 RPS (Requests per second) because no API endpoints from the API controller have been called.</span></span>

![gráfico anterior](memory/_static/0RPS.png)

<span data-ttu-id="08d0c-162">El gráfico muestra dos valores para el uso de memoria:</span><span class="sxs-lookup"><span data-stu-id="08d0c-162">The chart displays two values for the memory usage:</span></span>

- <span data-ttu-id="08d0c-163">Asignado: la cantidad de memoria ocupada por los objetos administrados</span><span class="sxs-lookup"><span data-stu-id="08d0c-163">Allocated: the amount of memory occupied by managed objects</span></span>
- <span data-ttu-id="08d0c-164">Espacio de [trabajo](/windows/win32/memory/working-set): el conjunto de páginas del espacio de direcciones virtuales del proceso residente actualmente en la memoria física.</span><span class="sxs-lookup"><span data-stu-id="08d0c-164">[Working set](/windows/win32/memory/working-set): The set of pages in the virtual address space of the process that are currently resident in physical memory.</span></span> <span data-ttu-id="08d0c-165">El espacio de trabajo mostrado es el mismo valor que se muestra en el administrador de tareas.</span><span class="sxs-lookup"><span data-stu-id="08d0c-165">The working set shown is the same value Task Manager displays.</span></span>

### <a name="transient-objects"></a><span data-ttu-id="08d0c-166">Objetos transitorios</span><span class="sxs-lookup"><span data-stu-id="08d0c-166">Transient objects</span></span>

<span data-ttu-id="08d0c-167">La API siguiente crea una instancia de cadena de 10 KB y la devuelve al cliente.</span><span class="sxs-lookup"><span data-stu-id="08d0c-167">The following API creates a 10-KB String instance and returns it to the client.</span></span> <span data-ttu-id="08d0c-168">En cada solicitud, se asigna un nuevo objeto en la memoria y se escribe en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="08d0c-168">On each request, a new object is allocated in memory and written to the response.</span></span> <span data-ttu-id="08d0c-169">Las cadenas se almacenan como caracteres UTF-16 en .NET, por lo que cada carácter ocupa 2 bytes en la memoria.</span><span class="sxs-lookup"><span data-stu-id="08d0c-169">Strings are stored as UTF-16 characters in .NET so each character takes 2 bytes in memory.</span></span>

```csharp
[HttpGet("bigstring")]
public ActionResult<string> GetBigString()
{
    return new String('x', 10 * 1024);
}
```

<span data-ttu-id="08d0c-170">El siguiente gráfico se genera con una carga relativamente pequeña en para mostrar cómo las asignaciones de memoria se ven afectadas por el GC.</span><span class="sxs-lookup"><span data-stu-id="08d0c-170">The following graph is generated with a relatively small load in to show how memory allocations are impacted by the GC.</span></span>

![gráfico anterior](memory/_static/bigstring.png)

<span data-ttu-id="08d0c-172">En el gráfico anterior se muestra:</span><span class="sxs-lookup"><span data-stu-id="08d0c-172">The preceding chart shows:</span></span>

* <span data-ttu-id="08d0c-173">4K RPS (solicitudes por segundo).</span><span class="sxs-lookup"><span data-stu-id="08d0c-173">4K RPS (Requests per second).</span></span>
* <span data-ttu-id="08d0c-174">Las colecciones de GC de generación 0 se producen aproximadamente cada dos segundos.</span><span class="sxs-lookup"><span data-stu-id="08d0c-174">Generation 0 GC collections occur about every two seconds.</span></span>
* <span data-ttu-id="08d0c-175">El espacio de trabajo es constante de aproximadamente 500 MB.</span><span class="sxs-lookup"><span data-stu-id="08d0c-175">The working set is constant at approximately 500 MB.</span></span>
* <span data-ttu-id="08d0c-176">La CPU es del 12%.</span><span class="sxs-lookup"><span data-stu-id="08d0c-176">CPU is 12%.</span></span>
* <span data-ttu-id="08d0c-177">El consumo de memoria y la versión (a través de GC) son estables.</span><span class="sxs-lookup"><span data-stu-id="08d0c-177">The memory consumption and release (through GC) is stable.</span></span>

<span data-ttu-id="08d0c-178">El siguiente gráfico se toma en el rendimiento máximo que puede controlar el equipo.</span><span class="sxs-lookup"><span data-stu-id="08d0c-178">The following chart is taken at the max throughput that can be handled by the machine.</span></span>

![gráfico anterior](memory/_static/bigstring2.png)

<span data-ttu-id="08d0c-180">En el gráfico anterior se muestra:</span><span class="sxs-lookup"><span data-stu-id="08d0c-180">The preceding chart shows:</span></span>

* <span data-ttu-id="08d0c-181">22K RPS</span><span class="sxs-lookup"><span data-stu-id="08d0c-181">22K RPS</span></span>
* <span data-ttu-id="08d0c-182">Las colecciones de GC de generación 0 se producen varias veces por segundo.</span><span class="sxs-lookup"><span data-stu-id="08d0c-182">Generation 0 GC collections occur several times per second.</span></span>
* <span data-ttu-id="08d0c-183">Las colecciones de la generación 1 se desencadenan porque la aplicación ha asignado significativamente más memoria por segundo.</span><span class="sxs-lookup"><span data-stu-id="08d0c-183">Generation 1 collections are triggered because the app allocated significantly more memory per second.</span></span>
* <span data-ttu-id="08d0c-184">El espacio de trabajo es constante de aproximadamente 500 MB.</span><span class="sxs-lookup"><span data-stu-id="08d0c-184">The working set is constant at approximately 500 MB.</span></span>
* <span data-ttu-id="08d0c-185">La CPU es del 33%.</span><span class="sxs-lookup"><span data-stu-id="08d0c-185">CPU is 33%.</span></span>
* <span data-ttu-id="08d0c-186">El consumo de memoria y la versión (a través de GC) son estables.</span><span class="sxs-lookup"><span data-stu-id="08d0c-186">The memory consumption and release (through GC) is stable.</span></span>
* <span data-ttu-id="08d0c-187">CPU (33%) no se ha utilizado en exceso, por lo que la recolección de elementos no utilizados puede mantener un gran número de asignaciones.</span><span class="sxs-lookup"><span data-stu-id="08d0c-187">The CPU (33%) is not over-utilized, therefore the garbage collection can keep up with a high number of allocations.</span></span>

### <a name="workstation-gc-vs-server-gc"></a><span data-ttu-id="08d0c-188">GC de estación de trabajo frente a GC de servidor</span><span class="sxs-lookup"><span data-stu-id="08d0c-188">Workstation GC vs. Server GC</span></span>

<span data-ttu-id="08d0c-189">El recolector de elementos no utilizados de .NET tiene dos modos diferentes:</span><span class="sxs-lookup"><span data-stu-id="08d0c-189">The .NET Garbage Collector has two different modes:</span></span>

* <span data-ttu-id="08d0c-190">**GC de estación de trabajo**: optimizado para el escritorio.</span><span class="sxs-lookup"><span data-stu-id="08d0c-190">**Workstation GC**: Optimized for the desktop.</span></span>
* <span data-ttu-id="08d0c-191">**GC del servidor**.</span><span class="sxs-lookup"><span data-stu-id="08d0c-191">**Server GC**.</span></span> <span data-ttu-id="08d0c-192">El GC predeterminado para ASP.NET Core aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="08d0c-192">The default GC for ASP.NET Core apps.</span></span> <span data-ttu-id="08d0c-193">Optimizado para el servidor.</span><span class="sxs-lookup"><span data-stu-id="08d0c-193">Optimized for the server.</span></span>

<span data-ttu-id="08d0c-194">El modo GC se puede establecer explícitamente en el archivo de proyecto o en el archivo *runtimeConfig. JSON* de la aplicación publicada.</span><span class="sxs-lookup"><span data-stu-id="08d0c-194">The GC mode can be set explicitly in the project file or in the *runtimeconfig.json* file of the published app.</span></span> <span data-ttu-id="08d0c-195">El marcado siguiente muestra el establecimiento de `ServerGarbageCollection` en el archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="08d0c-195">The following markup shows setting `ServerGarbageCollection` in the project file:</span></span>

```xml
<PropertyGroup>
  <ServerGarbageCollection>true</ServerGarbageCollection>
</PropertyGroup>
```

<span data-ttu-id="08d0c-196">Cambiar `ServerGarbageCollection` en el archivo de proyecto requiere que se vuelva a generar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="08d0c-196">Changing `ServerGarbageCollection` in the project file requires the app to be rebuilt.</span></span>

<span data-ttu-id="08d0c-197">**Nota:** La recolección de elementos **no** utilizados de servidor no está disponible en equipos con un solo núcleo.</span><span class="sxs-lookup"><span data-stu-id="08d0c-197">**Note:** Server garbage collection is **not** available on machines with a single core.</span></span> <span data-ttu-id="08d0c-198">Para obtener más información, vea <xref:System.Runtime.GCSettings.IsServerGC>.</span><span class="sxs-lookup"><span data-stu-id="08d0c-198">For more information, see <xref:System.Runtime.GCSettings.IsServerGC>.</span></span>

<span data-ttu-id="08d0c-199">En la imagen siguiente se muestra el perfil de memoria en un 5.000 RPS mediante el GC de la estación de trabajo.</span><span class="sxs-lookup"><span data-stu-id="08d0c-199">The following image shows the memory profile under a 5K RPS using the Workstation GC.</span></span>

![gráfico anterior](memory/_static/workstation.png)

<span data-ttu-id="08d0c-201">Las diferencias entre este gráfico y la versión del servidor son significativas:</span><span class="sxs-lookup"><span data-stu-id="08d0c-201">The differences between this chart and the server version are significant:</span></span>

- <span data-ttu-id="08d0c-202">El espacio de trabajo desciende de 500 MB a 70 MB.</span><span class="sxs-lookup"><span data-stu-id="08d0c-202">The working set drops from 500 MB to 70 MB.</span></span>
- <span data-ttu-id="08d0c-203">El GC realiza colecciones de la generación 0 varias veces por segundo en lugar de cada dos segundos.</span><span class="sxs-lookup"><span data-stu-id="08d0c-203">The GC does generation 0 collections multiple times per second instead of every two seconds.</span></span>
- <span data-ttu-id="08d0c-204">GC desciende de 300 MB a 10 MB.</span><span class="sxs-lookup"><span data-stu-id="08d0c-204">GC drops from 300 MB to 10 MB.</span></span>

<span data-ttu-id="08d0c-205">En un entorno de servidor Web típico, el uso de CPU es más importante que la memoria, por lo que el GC del servidor es mejor.</span><span class="sxs-lookup"><span data-stu-id="08d0c-205">On a typical web server environment, CPU usage is more important than memory, therefore the Server GC is better.</span></span> <span data-ttu-id="08d0c-206">Si la utilización de la memoria es alta y el uso de la CPU es relativamente bajo, el GC de la estación de trabajo puede ser más eficaz.</span><span class="sxs-lookup"><span data-stu-id="08d0c-206">If memory utilization is high and CPU usage is relatively low, the Workstation GC might be more performant.</span></span> <span data-ttu-id="08d0c-207">Por ejemplo, alta densidad que hospeda varias aplicaciones web en las que la memoria es escasa.</span><span class="sxs-lookup"><span data-stu-id="08d0c-207">For example, high density hosting several web apps where memory is scarce.</span></span>

### <a name="persistent-object-references"></a><span data-ttu-id="08d0c-208">Referencias de objeto persistentes</span><span class="sxs-lookup"><span data-stu-id="08d0c-208">Persistent object references</span></span>

<span data-ttu-id="08d0c-209">El GC no puede liberar objetos a los que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="08d0c-209">The GC cannot free objects that are referenced.</span></span> <span data-ttu-id="08d0c-210">Los objetos a los que se hace referencia pero que ya no se necesitan producen una pérdida de memoria.</span><span class="sxs-lookup"><span data-stu-id="08d0c-210">Objects that are referenced but no longer needed result in a memory leak.</span></span> <span data-ttu-id="08d0c-211">Si la aplicación asigna objetos con frecuencia y no puede liberarlos después de que ya no se necesiten, el uso de memoria aumentará con el tiempo.</span><span class="sxs-lookup"><span data-stu-id="08d0c-211">If the app frequently allocates objects and fails to free them after they are no longer needed, memory usage will increase over time.</span></span>

<span data-ttu-id="08d0c-212">La API siguiente crea una instancia de cadena de 10 KB y la devuelve al cliente.</span><span class="sxs-lookup"><span data-stu-id="08d0c-212">The following API creates a 10-KB String instance and returns it to the client.</span></span> <span data-ttu-id="08d0c-213">La diferencia con el ejemplo anterior es que un miembro estático hace referencia a esta instancia, lo que significa que nunca está disponible para la recolección.</span><span class="sxs-lookup"><span data-stu-id="08d0c-213">The difference with the previous example is that this instance is referenced by a static member, which means it's never available for collection.</span></span>

```csharp
private static ConcurrentBag<string> _staticStrings = new ConcurrentBag<string>();

[HttpGet("staticstring")]
public ActionResult<string> GetStaticString()
{
    var bigString = new String('x', 10 * 1024);
    _staticStrings.Add(bigString);
    return bigString;
}
```

<span data-ttu-id="08d0c-214">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="08d0c-214">The preceding code:</span></span>

* <span data-ttu-id="08d0c-215">Es un ejemplo de una pérdida de memoria típica.</span><span class="sxs-lookup"><span data-stu-id="08d0c-215">Is an example of a typical memory leak.</span></span>
* <span data-ttu-id="08d0c-216">Con llamadas frecuentes, hace que la memoria de la aplicación aumente hasta que el proceso se bloquee con una excepción `OutOfMemory`.</span><span class="sxs-lookup"><span data-stu-id="08d0c-216">With frequent calls, causes app memory to increases until the process crashes with an `OutOfMemory` exception.</span></span>

![gráfico anterior](memory/_static/eternal.png)

<span data-ttu-id="08d0c-218">En la imagen anterior:</span><span class="sxs-lookup"><span data-stu-id="08d0c-218">In the preceding image:</span></span>

* <span data-ttu-id="08d0c-219">La prueba de carga del punto de conexión de `/api/staticstring` produce un aumento lineal en la memoria.</span><span class="sxs-lookup"><span data-stu-id="08d0c-219">Load testing the `/api/staticstring` endpoint causes a linear increase in memory.</span></span>
* <span data-ttu-id="08d0c-220">El GC intenta liberar memoria a medida que aumenta la presión de memoria, llamando a una colección de generación 2.</span><span class="sxs-lookup"><span data-stu-id="08d0c-220">The GC tries to free memory as the memory pressure grows, by calling a generation 2 collection.</span></span>
* <span data-ttu-id="08d0c-221">El GC no puede liberar la memoria perdida.</span><span class="sxs-lookup"><span data-stu-id="08d0c-221">The GC cannot free the leaked memory.</span></span> <span data-ttu-id="08d0c-222">La asignación y el espacio de trabajo aumentan con el tiempo.</span><span class="sxs-lookup"><span data-stu-id="08d0c-222">Allocated and working set increase with time.</span></span>

<span data-ttu-id="08d0c-223">Algunos escenarios, como el almacenamiento en caché, requieren que las referencias de objeto se mantengan hasta que la presión de memoria obliga a que se liberen.</span><span class="sxs-lookup"><span data-stu-id="08d0c-223">Some scenarios, such as caching, require object references to be held until memory pressure forces them to be released.</span></span> <span data-ttu-id="08d0c-224">La clase <xref:System.WeakReference> se puede usar para este tipo de código de almacenamiento en caché.</span><span class="sxs-lookup"><span data-stu-id="08d0c-224">The <xref:System.WeakReference> class can be used for this type of caching code.</span></span> <span data-ttu-id="08d0c-225">Un objeto de `WeakReference` se recopila bajo presión de memoria.</span><span class="sxs-lookup"><span data-stu-id="08d0c-225">A `WeakReference` object is collected under memory pressures.</span></span> <span data-ttu-id="08d0c-226">La implementación predeterminada de <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache> usa `WeakReference`.</span><span class="sxs-lookup"><span data-stu-id="08d0c-226">The default implementation of <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache> uses `WeakReference`.</span></span>

### <a name="native-memory"></a><span data-ttu-id="08d0c-227">Memoria nativa</span><span class="sxs-lookup"><span data-stu-id="08d0c-227">Native memory</span></span>

<span data-ttu-id="08d0c-228">Algunos objetos .NET Core se basan en la memoria nativa.</span><span class="sxs-lookup"><span data-stu-id="08d0c-228">Some .NET Core objects rely on native memory.</span></span> <span data-ttu-id="08d0c-229">El GC **no** puede recopilar memoria nativa.</span><span class="sxs-lookup"><span data-stu-id="08d0c-229">Native memory can **not** be collected by the GC.</span></span> <span data-ttu-id="08d0c-230">El objeto .NET que usa memoria nativa debe liberarlo mediante código nativo.</span><span class="sxs-lookup"><span data-stu-id="08d0c-230">The .NET object using native memory must free it using native code.</span></span>

<span data-ttu-id="08d0c-231">.NET proporciona la interfaz de <xref:System.IDisposable> para permitir que los desarrolladores liberen memoria nativa.</span><span class="sxs-lookup"><span data-stu-id="08d0c-231">.NET provides the <xref:System.IDisposable> interface to let developers release native memory.</span></span> <span data-ttu-id="08d0c-232">Incluso si no se llama a <xref:System.IDisposable.Dispose*>, las clases implementadas correctamente llaman a `Dispose` cuando se ejecuta el [finalizador](/dotnet/csharp/programming-guide/classes-and-structs/destructors) .</span><span class="sxs-lookup"><span data-stu-id="08d0c-232">Even if <xref:System.IDisposable.Dispose*> is not called, correctly implemented classes call `Dispose` when the [finalizer](/dotnet/csharp/programming-guide/classes-and-structs/destructors) runs.</span></span>

<span data-ttu-id="08d0c-233">Observe el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="08d0c-233">Consider the following code:</span></span>

```csharp
[HttpGet("fileprovider")]
public void GetFileProvider()
{
    var fp = new PhysicalFileProvider(TempPath);
    fp.Watch("*.*");
}
```

<span data-ttu-id="08d0c-234">[PhysicaFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider?view=dotnet-plat-ext-3.0) es una clase administrada, por lo que cualquier instancia se recopilará al final de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="08d0c-234">[PhysicaFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider?view=dotnet-plat-ext-3.0) is a managed class, so any instance will be collected at the end of the request.</span></span>

<span data-ttu-id="08d0c-235">La siguiente imagen muestra el perfil de memoria al invocar la API de `fileprovider` continuamente.</span><span class="sxs-lookup"><span data-stu-id="08d0c-235">The following image shows the memory profile while invoking the `fileprovider` API continuously.</span></span>

![gráfico anterior](memory/_static/fileprovider.png)

<span data-ttu-id="08d0c-237">En el gráfico anterior se muestra un problema obvio con la implementación de esta clase, ya que sigue aumentando el uso de memoria.</span><span class="sxs-lookup"><span data-stu-id="08d0c-237">The preceding chart shows an obvious issue with the implementation of this class, as it keeps increasing memory usage.</span></span> <span data-ttu-id="08d0c-238">Se trata de un problema conocido del que se está realizando [el seguimiento en este problema](https://github.com/aspnet/Home/issues/3110).</span><span class="sxs-lookup"><span data-stu-id="08d0c-238">This is a known problem that is being tracked in [this issue](https://github.com/aspnet/Home/issues/3110).</span></span>

<span data-ttu-id="08d0c-239">La misma fuga podría producirse en el código de usuario, mediante una de las siguientes acciones:</span><span class="sxs-lookup"><span data-stu-id="08d0c-239">The same leak could be happen in user code, by one of the following:</span></span>

* <span data-ttu-id="08d0c-240">No se libera correctamente la clase.</span><span class="sxs-lookup"><span data-stu-id="08d0c-240">Not releasing the class correctly.</span></span>
* <span data-ttu-id="08d0c-241">Olvidar invocar el método `Dispose`de los objetos dependientes que se deben desechar.</span><span class="sxs-lookup"><span data-stu-id="08d0c-241">Forgetting to invoke the `Dispose`method of the dependent objects that should be disposed.</span></span>

### <a name="large-objects-heap"></a><span data-ttu-id="08d0c-242">Montón de objetos grandes</span><span class="sxs-lookup"><span data-stu-id="08d0c-242">Large objects heap</span></span>

<span data-ttu-id="08d0c-243">Los ciclos gratuitos y de asignación de memoria frecuentes pueden fragmentar la memoria, especialmente cuando se asignan grandes fragmentos de memoria.</span><span class="sxs-lookup"><span data-stu-id="08d0c-243">Frequent memory allocation/free cycles can fragment memory, especially when allocating large chunks of memory.</span></span> <span data-ttu-id="08d0c-244">Los objetos se asignan en bloques de memoria contiguos.</span><span class="sxs-lookup"><span data-stu-id="08d0c-244">Objects are allocated in contiguous blocks of memory.</span></span> <span data-ttu-id="08d0c-245">Para mitigar la fragmentación, cuando el GC libera memoria, trys para desfragmentarla.</span><span class="sxs-lookup"><span data-stu-id="08d0c-245">To mitigate fragmentation, when the GC frees memory, it trys to defragment it.</span></span> <span data-ttu-id="08d0c-246">Este proceso se denomina **compactación**.</span><span class="sxs-lookup"><span data-stu-id="08d0c-246">This process is called **compaction**.</span></span> <span data-ttu-id="08d0c-247">La compactación implica mover objetos.</span><span class="sxs-lookup"><span data-stu-id="08d0c-247">Compaction involves moving objects.</span></span> <span data-ttu-id="08d0c-248">Mover objetos grandes impone una reducción del rendimiento.</span><span class="sxs-lookup"><span data-stu-id="08d0c-248">Moving large objects imposes a performance penalty.</span></span> <span data-ttu-id="08d0c-249">Por esta razón, el GC crea una zona de memoria especial para objetos _grandes_ , denominada [montón de objetos grandes](/dotnet/standard/garbage-collection/large-object-heap) (montón).</span><span class="sxs-lookup"><span data-stu-id="08d0c-249">For this reason the GC creates a special memory zone for _large_ objects, called the [large object heap](/dotnet/standard/garbage-collection/large-object-heap) (LOH).</span></span> <span data-ttu-id="08d0c-250">Los objetos que tienen más de 85.000 bytes (aproximadamente 83 KB) son:</span><span class="sxs-lookup"><span data-stu-id="08d0c-250">Objects that are greater than 85,000 bytes (approximately 83 KB) are:</span></span>

* <span data-ttu-id="08d0c-251">Se coloca en el montón.</span><span class="sxs-lookup"><span data-stu-id="08d0c-251">Placed on the LOH.</span></span>
* <span data-ttu-id="08d0c-252">No compactado.</span><span class="sxs-lookup"><span data-stu-id="08d0c-252">Not compacted.</span></span>
* <span data-ttu-id="08d0c-253">Recopilados durante la generación 2 GC.</span><span class="sxs-lookup"><span data-stu-id="08d0c-253">Collected during generation 2 GCs.</span></span>

<span data-ttu-id="08d0c-254">Cuando el montón está lleno, el GC desencadenará una recolección de la generación 2.</span><span class="sxs-lookup"><span data-stu-id="08d0c-254">When the LOH is full, the GC will trigger a generation 2 collection.</span></span> <span data-ttu-id="08d0c-255">Recopilaciones de generación 2:</span><span class="sxs-lookup"><span data-stu-id="08d0c-255">Generation 2 collections:</span></span>

* <span data-ttu-id="08d0c-256">Son intrínsecamente lentas.</span><span class="sxs-lookup"><span data-stu-id="08d0c-256">Are inherently slow.</span></span>
* <span data-ttu-id="08d0c-257">Además, incurre en el costo de desencadenar una colección en todas las demás generaciones.</span><span class="sxs-lookup"><span data-stu-id="08d0c-257">Additionally incur the cost of triggering a collection on all other generations.</span></span>

<span data-ttu-id="08d0c-258">El siguiente código compacta el montón inmediatamente:</span><span class="sxs-lookup"><span data-stu-id="08d0c-258">The following code compacts the LOH immediately:</span></span>

```csharp
GCSettings.LargeObjectHeapCompactionMode = GCLargeObjectHeapCompactionMode.CompactOnce;
GC.Collect();
```

<span data-ttu-id="08d0c-259">Consulte <xref:System.Runtime.GCSettings.LargeObjectHeapCompactionMode> para obtener información sobre cómo compactar el montón de datos.</span><span class="sxs-lookup"><span data-stu-id="08d0c-259">See <xref:System.Runtime.GCSettings.LargeObjectHeapCompactionMode> for information on compacting the LOH.</span></span>

<span data-ttu-id="08d0c-260">En los contenedores con .NET Core 3,0 y versiones posteriores, el montón de base de los se compacta automáticamente.</span><span class="sxs-lookup"><span data-stu-id="08d0c-260">In containers using .NET Core 3.0 and later, the LOH is automatically compacted.</span></span>

<span data-ttu-id="08d0c-261">La siguiente API que muestra este comportamiento:</span><span class="sxs-lookup"><span data-stu-id="08d0c-261">The following API that illustrates this behavior:</span></span>

```csharp
[HttpGet("loh/{size=85000}")]
public int GetLOH1(int size)
{
   return new byte[size].Length;
}
```

<span data-ttu-id="08d0c-262">En el gráfico siguiente se muestra el perfil de memoria de la llamada al punto de conexión `/api/loh/84975`, en la carga máxima:</span><span class="sxs-lookup"><span data-stu-id="08d0c-262">The following chart shows the memory profile of calling the `/api/loh/84975` endpoint, under maximum load:</span></span>

![gráfico anterior](memory/_static/loh1.png)

<span data-ttu-id="08d0c-264">En el gráfico siguiente se muestra el perfil de memoria de llamar al extremo `/api/loh/84976`, asignando *solo un byte más*:</span><span class="sxs-lookup"><span data-stu-id="08d0c-264">The following chart shows the memory profile of calling the `/api/loh/84976` endpoint, allocating *just one more byte*:</span></span>

![gráfico anterior](memory/_static/loh2.png)

<span data-ttu-id="08d0c-266">Nota: la estructura de `byte[]` tiene bytes de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="08d0c-266">Note: The `byte[]` structure has overhead bytes.</span></span> <span data-ttu-id="08d0c-267">Por eso 84.976 bytes desencadena el límite de 85.000.</span><span class="sxs-lookup"><span data-stu-id="08d0c-267">That's why 84,976 bytes triggers the 85,000 limit.</span></span>

<span data-ttu-id="08d0c-268">Comparar los dos gráficos anteriores:</span><span class="sxs-lookup"><span data-stu-id="08d0c-268">Comparing the two preceding charts:</span></span>

* <span data-ttu-id="08d0c-269">El espacio de trabajo es similar en ambos escenarios, aproximadamente 450 MB.</span><span class="sxs-lookup"><span data-stu-id="08d0c-269">The working set is similar for both scenarios, about 450 MB.</span></span>
* <span data-ttu-id="08d0c-270">El bajo solicitudes de montón (84.975 bytes) muestra principalmente colecciones de la generación 0.</span><span class="sxs-lookup"><span data-stu-id="08d0c-270">The under LOH requests (84,975 bytes) shows mostly generation 0 collections.</span></span>
* <span data-ttu-id="08d0c-271">Las solicitudes de montón superior generan recolecciones de la generación 2.</span><span class="sxs-lookup"><span data-stu-id="08d0c-271">The over LOH requests generate constant generation 2 collections.</span></span> <span data-ttu-id="08d0c-272">Las colecciones de generación 2 son costosas.</span><span class="sxs-lookup"><span data-stu-id="08d0c-272">Generation 2 collections are expensive.</span></span> <span data-ttu-id="08d0c-273">Se necesita más CPU y el rendimiento se reduce casi el 50%.</span><span class="sxs-lookup"><span data-stu-id="08d0c-273">More CPU is required and throughput drops almost 50%.</span></span>

<span data-ttu-id="08d0c-274">Los objetos temporales grandes son especialmente problemáticos, ya que provocan GC de la segunda generación.</span><span class="sxs-lookup"><span data-stu-id="08d0c-274">Temporary large objects are particularly problematic because they cause gen2 GCs.</span></span>

<span data-ttu-id="08d0c-275">Para obtener el máximo rendimiento, se debe minimizar el uso de objetos grandes.</span><span class="sxs-lookup"><span data-stu-id="08d0c-275">For maximum performance, large object use should be minimized.</span></span> <span data-ttu-id="08d0c-276">Si es posible, divida los objetos grandes.</span><span class="sxs-lookup"><span data-stu-id="08d0c-276">If possible, split up large objects.</span></span> <span data-ttu-id="08d0c-277">Por ejemplo, el middleware de [almacenamiento en caché de respuesta](xref:performance/caching/response) de ASP.net Core divide las entradas de la memoria caché en bloques inferiores a 85.000 bytes.</span><span class="sxs-lookup"><span data-stu-id="08d0c-277">For example, [Response Caching](xref:performance/caching/response) middleware in ASP.NET Core split the cache entries into blocks less than 85,000 bytes.</span></span>

<span data-ttu-id="08d0c-278">En los siguientes vínculos se muestra el enfoque ASP.NET Core para mantener objetos bajo el límite de montón:</span><span class="sxs-lookup"><span data-stu-id="08d0c-278">The following links show the ASP.NET Core approach to keeping objects under the LOH limit:</span></span>
- [<span data-ttu-id="08d0c-279">ResponseCaching/streams/StreamUtilities. CS</span><span class="sxs-lookup"><span data-stu-id="08d0c-279">ResponseCaching/Streams/StreamUtilities.cs</span></span>](https://github.com/aspnet/AspNetCore/blob/v3.0.0/src/Middleware/ResponseCaching/src/Streams/StreamUtilities.cs#L16)
- [<span data-ttu-id="08d0c-280">ResponseCaching/MemoryResponseCache. CS</span><span class="sxs-lookup"><span data-stu-id="08d0c-280">ResponseCaching/MemoryResponseCache.cs</span></span>](https://github.com/aspnet/ResponseCaching/blob/c1cb7576a0b86e32aec990c22df29c780af29ca5/src/Microsoft.AspNetCore.ResponseCaching/Internal/MemoryResponseCache.cs#L55)

<span data-ttu-id="08d0c-281">Para obtener más información, vea:</span><span class="sxs-lookup"><span data-stu-id="08d0c-281">For more information, see:</span></span>

* [<span data-ttu-id="08d0c-282">Montón de objetos grandes descubierto</span><span class="sxs-lookup"><span data-stu-id="08d0c-282">Large Object Heap Uncovered</span></span>](https://devblogs.microsoft.com/dotnet/large-object-heap-uncovered-from-an-old-msdn-article/)
* [<span data-ttu-id="08d0c-283">Montón de objetos grandes</span><span class="sxs-lookup"><span data-stu-id="08d0c-283">Large object heap</span></span>](/dotnet/standard/garbage-collection/large-object-heap)

### <a name="httpclient"></a><span data-ttu-id="08d0c-284">HttpClient</span><span class="sxs-lookup"><span data-stu-id="08d0c-284">HttpClient</span></span>

<span data-ttu-id="08d0c-285">El uso incorrecto de <xref:System.Net.Http.HttpClient> puede producir una pérdida de recursos.</span><span class="sxs-lookup"><span data-stu-id="08d0c-285">Incorrectly using <xref:System.Net.Http.HttpClient> can result in a resource leak.</span></span> <span data-ttu-id="08d0c-286">Recursos del sistema, como conexiones de bases de datos, sockets, identificadores de archivo, etc.:</span><span class="sxs-lookup"><span data-stu-id="08d0c-286">System resources, such as database connections, sockets, file handles, etc.:</span></span>

* <span data-ttu-id="08d0c-287">Son más escasos que la memoria.</span><span class="sxs-lookup"><span data-stu-id="08d0c-287">Are more scarce than memory.</span></span>
* <span data-ttu-id="08d0c-288">Son más problemáticas cuando se pierden la memoria.</span><span class="sxs-lookup"><span data-stu-id="08d0c-288">Are more problematic when leaked than memory.</span></span>

<span data-ttu-id="08d0c-289">Los desarrolladores de .NET experimentados saben llamar a <xref:System.IDisposable.Dispose*> en objetos que implementan <xref:System.IDisposable>.</span><span class="sxs-lookup"><span data-stu-id="08d0c-289">Experienced .NET developers know to call <xref:System.IDisposable.Dispose*> on objects that implement <xref:System.IDisposable>.</span></span> <span data-ttu-id="08d0c-290">No desechar objetos que implementan `IDisposable` suele producir pérdida de memoria o de recursos del sistema perdidos.</span><span class="sxs-lookup"><span data-stu-id="08d0c-290">Not disposing objects that implement `IDisposable` typically results in leaked memory or leaked system resources.</span></span>

<span data-ttu-id="08d0c-291">`HttpClient` implementa `IDisposable`, pero **no** debe desecharse en cada invocación.</span><span class="sxs-lookup"><span data-stu-id="08d0c-291">`HttpClient` implements `IDisposable`, but should **not** be disposed on every invocation.</span></span> <span data-ttu-id="08d0c-292">En su lugar, se debe reutilizar `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="08d0c-292">Rather, `HttpClient` should be reused.</span></span>

<span data-ttu-id="08d0c-293">El siguiente punto de conexión crea y desecha una nueva instancia de `HttpClient` en cada solicitud:</span><span class="sxs-lookup"><span data-stu-id="08d0c-293">The following endpoint creates and disposes a new  `HttpClient` instance on every request:</span></span>

```csharp
[HttpGet("httpclient1")]
public async Task<int> GetHttpClient1(string url)
{
    using (var httpClient = new HttpClient())
    {
        var result = await httpClient.GetAsync(url);
        return (int)result.StatusCode;
    }
}
```

<span data-ttu-id="08d0c-294">Bajo carga, se registran los siguientes mensajes de error:</span><span class="sxs-lookup"><span data-stu-id="08d0c-294">Under load, the following error messages are logged:</span></span>

```
fail: Microsoft.AspNetCore.Server.Kestrel[13]
      Connection id "0HLG70PBE1CR1", Request id "0HLG70PBE1CR1:00000031":
      An unhandled exception was thrown by the application.
System.Net.Http.HttpRequestException: Only one usage of each socket address
    (protocol/network address/port) is normally permitted --->
    System.Net.Sockets.SocketException: Only one usage of each socket address
    (protocol/network address/port) is normally permitted
   at System.Net.Http.ConnectHelper.ConnectAsync(String host, Int32 port,
    CancellationToken cancellationToken)
```

<span data-ttu-id="08d0c-295">Aunque se eliminen las instancias de `HttpClient`, la conexión de red real tarda algún tiempo en publicarse en el sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="08d0c-295">Even though the `HttpClient` instances are disposed, the actual network connection takes some time to be released by the operating system.</span></span> <span data-ttu-id="08d0c-296">Al crear continuamente nuevas conexiones, se produce el agotamiento de los _puertos_ .</span><span class="sxs-lookup"><span data-stu-id="08d0c-296">By continuously creating new connections, _ports exhaustion_ occurs.</span></span> <span data-ttu-id="08d0c-297">Cada conexión de cliente requiere su propio puerto de cliente.</span><span class="sxs-lookup"><span data-stu-id="08d0c-297">Each client connection requires its own client port.</span></span>

<span data-ttu-id="08d0c-298">Una manera de evitar el agotamiento del puerto es reutilizar la misma instancia de `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="08d0c-298">One way to prevent port exhaustion is to reuse the same `HttpClient` instance:</span></span>

```csharp
private static readonly HttpClient _httpClient = new HttpClient();

[HttpGet("httpclient2")]
public async Task<int> GetHttpClient2(string url)
{
    var result = await _httpClient.GetAsync(url);
    return (int)result.StatusCode;
}
```

<span data-ttu-id="08d0c-299">La instancia de `HttpClient` se libera cuando se detiene la aplicación.</span><span class="sxs-lookup"><span data-stu-id="08d0c-299">The `HttpClient` instance is released when the app stops.</span></span> <span data-ttu-id="08d0c-300">Este ejemplo muestra que no se deben eliminar todos los recursos descartados después de cada uso.</span><span class="sxs-lookup"><span data-stu-id="08d0c-300">This example shows that not every disposable resource should be disposed after each use.</span></span>

<span data-ttu-id="08d0c-301">Vea lo siguiente para obtener una mejor manera de controlar la duración de una instancia de `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="08d0c-301">See the following for a better way to handle the lifetime of an `HttpClient` instance:</span></span>

* [<span data-ttu-id="08d0c-302">HttpClient y administración de la duración</span><span class="sxs-lookup"><span data-stu-id="08d0c-302">HttpClient and lifetime management</span></span>](/aspnet/core/fundamentals/http-requests#httpclient-and-lifetime-management)
* [<span data-ttu-id="08d0c-303">Blog de factoría de HTTPClient</span><span class="sxs-lookup"><span data-stu-id="08d0c-303">HTTPClient factory blog</span></span>](https://devblogs.microsoft.com/aspnet/asp-net-core-2-1-preview1-introducing-httpclient-factory/)
 
### <a name="object-pooling"></a><span data-ttu-id="08d0c-304">Agrupación de objetos</span><span class="sxs-lookup"><span data-stu-id="08d0c-304">Object pooling</span></span>

<span data-ttu-id="08d0c-305">En el ejemplo anterior se mostró cómo se puede hacer que la instancia de `HttpClient` sea estática y reutilizada por todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="08d0c-305">The previous example showed how the `HttpClient` instance can be made static and reused by all requests.</span></span> <span data-ttu-id="08d0c-306">La reutilización evita quedarse sin recursos.</span><span class="sxs-lookup"><span data-stu-id="08d0c-306">Reuse prevents running out of resources.</span></span>

<span data-ttu-id="08d0c-307">Agrupación de objetos:</span><span class="sxs-lookup"><span data-stu-id="08d0c-307">Object pooling:</span></span>

* <span data-ttu-id="08d0c-308">Usa el patrón de reutilización.</span><span class="sxs-lookup"><span data-stu-id="08d0c-308">Uses the reuse pattern.</span></span>
* <span data-ttu-id="08d0c-309">Está diseñado para objetos que son caros de crear.</span><span class="sxs-lookup"><span data-stu-id="08d0c-309">Is designed for objects that are expensive to create.</span></span>

<span data-ttu-id="08d0c-310">Un grupo es una colección de objetos previamente inicializados que se pueden reservar y liberar entre subprocesos.</span><span class="sxs-lookup"><span data-stu-id="08d0c-310">A pool is a collection of pre-initialized objects that can be reserved and released across threads.</span></span> <span data-ttu-id="08d0c-311">Los grupos pueden definir reglas de asignación como límites, tamaños predefinidos o tasa de crecimiento.</span><span class="sxs-lookup"><span data-stu-id="08d0c-311">Pools can define allocation rules such as limits, predefined sizes, or growth rate.</span></span>

<span data-ttu-id="08d0c-312">El paquete NuGet [Microsoft. Extensions. ObjectPool](https://www.nuget.org/packages/Microsoft.Extensions.ObjectPool/) contiene clases que ayudan a administrar estos grupos.</span><span class="sxs-lookup"><span data-stu-id="08d0c-312">The NuGet package [Microsoft.Extensions.ObjectPool](https://www.nuget.org/packages/Microsoft.Extensions.ObjectPool/) contains classes that help to manage such pools.</span></span>

<span data-ttu-id="08d0c-313">El siguiente punto de conexión de API crea una instancia de un búfer `byte` que se rellena con números aleatorios en cada solicitud:</span><span class="sxs-lookup"><span data-stu-id="08d0c-313">The following API endpoint instantiates a `byte` buffer that is filled with random numbers on each request:</span></span>

```csharp
        [HttpGet("array/{size}")]
        public byte[] GetArray(int size)
        {
            var random = new Random();
            var array = new byte[size];
            random.NextBytes(array);

            return array;
        }
```

<span data-ttu-id="08d0c-314">En el siguiente gráfico se muestra la llamada a la API anterior con carga moderada:</span><span class="sxs-lookup"><span data-stu-id="08d0c-314">The following chart display calling the preceding API with moderate load:</span></span>

![gráfico anterior](memory/_static/array.png)

<span data-ttu-id="08d0c-316">En el gráfico anterior, las recopilaciones de generación 0 se producen aproximadamente una vez por segundo.</span><span class="sxs-lookup"><span data-stu-id="08d0c-316">In the preceding chart, generation 0 collections happen approximately once per second.</span></span>

<span data-ttu-id="08d0c-317">El código anterior se puede optimizar agrupando el búfer de `byte` mediante [ArrayPool\<t >](xref:System.Buffers.ArrayPool`1).</span><span class="sxs-lookup"><span data-stu-id="08d0c-317">The preceding code can be optimized by pooling the `byte` buffer by using [ArrayPool\<T>](xref:System.Buffers.ArrayPool`1).</span></span> <span data-ttu-id="08d0c-318">Una instancia estática se reutiliza en todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="08d0c-318">A static instance is reused across requests.</span></span>

<span data-ttu-id="08d0c-319">Lo que es diferente con este enfoque es que la API devuelve un objeto agrupado.</span><span class="sxs-lookup"><span data-stu-id="08d0c-319">What's different with this approach is that a pooled object is returned from the API.</span></span> <span data-ttu-id="08d0c-320">Esto significa:</span><span class="sxs-lookup"><span data-stu-id="08d0c-320">That means:</span></span>

* <span data-ttu-id="08d0c-321">El objeto está fuera del control tan pronto como se devuelve desde el método.</span><span class="sxs-lookup"><span data-stu-id="08d0c-321">The object is out of your control as soon as you return from the method.</span></span>
* <span data-ttu-id="08d0c-322">No se puede liberar el objeto.</span><span class="sxs-lookup"><span data-stu-id="08d0c-322">You can't release the object.</span></span>

<span data-ttu-id="08d0c-323">Para configurar la eliminación del objeto:</span><span class="sxs-lookup"><span data-stu-id="08d0c-323">To set up disposal of the object:</span></span>

* <span data-ttu-id="08d0c-324">Encapsula la matriz agrupada en un objeto descartable.</span><span class="sxs-lookup"><span data-stu-id="08d0c-324">Encapsulate the pooled array in a disposable object.</span></span>
* <span data-ttu-id="08d0c-325">Registre el objeto agrupado con [HttpContext. Response. RegisterForDispose](xref:Microsoft.AspNetCore.Http.HttpResponse.RegisterForDispose*).</span><span class="sxs-lookup"><span data-stu-id="08d0c-325">Register the pooled object with [HttpContext.Response.RegisterForDispose](xref:Microsoft.AspNetCore.Http.HttpResponse.RegisterForDispose*).</span></span>

<span data-ttu-id="08d0c-326">`RegisterForDispose` se encargará de llamar a `Dispose`en el objeto de destino para que solo se libere cuando se complete la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="08d0c-326">`RegisterForDispose` will take care of calling `Dispose`on the target object so that it's only released when the HTTP request is complete.</span></span>

```csharp
private static ArrayPool<byte> _arrayPool = ArrayPool<byte>.Create();

private class PooledArray : IDisposable
{
    public byte[] Array { get; private set; }

    public PooledArray(int size)
    {
        Array = _arrayPool.Rent(size);
    }

    public void Dispose()
    {
        _arrayPool.Return(Array);
    }
}

[HttpGet("pooledarray/{size}")]
public byte[] GetPooledArray(int size)
{
    var pooledArray = new PooledArray(size);

    var random = new Random();
    random.NextBytes(pooledArray.Array);

    HttpContext.Response.RegisterForDispose(pooledArray);

    return pooledArray.Array;
}
```

<span data-ttu-id="08d0c-327">Al aplicar la misma carga que la versión no agrupada, se obtiene el siguiente gráfico:</span><span class="sxs-lookup"><span data-stu-id="08d0c-327">Applying the same load as the non-pooled version results in the following chart:</span></span>

![gráfico anterior](memory/_static/pooledarray.png)

<span data-ttu-id="08d0c-329">La diferencia principal son los bytes asignados y, como consecuencia, muchas menos de la generación 0.</span><span class="sxs-lookup"><span data-stu-id="08d0c-329">The main difference is allocated bytes, and as a consequence much fewer generation 0 collections.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="08d0c-330">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="08d0c-330">Additional resources</span></span>

* [<span data-ttu-id="08d0c-331">Recolección de elementos no utilizados</span><span class="sxs-lookup"><span data-stu-id="08d0c-331">Garbage Collection</span></span>](/dotnet/standard/garbage-collection/)
* [<span data-ttu-id="08d0c-332">Descripción de los distintos modos de GC con el visualizador de simultaneidad</span><span class="sxs-lookup"><span data-stu-id="08d0c-332">Understanding different GC modes with Concurrency Visualizer</span></span>](https://blogs.msdn.microsoft.com/seteplia/2017/01/05/understanding-different-gc-modes-with-concurrency-visualizer/)
* [<span data-ttu-id="08d0c-333">Montón de objetos grandes descubierto</span><span class="sxs-lookup"><span data-stu-id="08d0c-333">Large Object Heap Uncovered</span></span>](https://devblogs.microsoft.com/dotnet/large-object-heap-uncovered-from-an-old-msdn-article/)
* [<span data-ttu-id="08d0c-334">Montón de objetos grandes</span><span class="sxs-lookup"><span data-stu-id="08d0c-334">Large object heap</span></span>](/dotnet/standard/garbage-collection/large-object-heap)
