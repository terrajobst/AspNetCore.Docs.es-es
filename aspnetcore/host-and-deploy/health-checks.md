---
title: Comprobaciones de estado en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo configurar las comprobaciones de estado para la infraestructura de ASP.NET Core, como las aplicaciones y las bases de datos.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2019
uid: host-and-deploy/health-checks
ms.openlocfilehash: 9157c94c6e8f433869c8163ebf7772a7271b11ba
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265317"
---
# <a name="health-checks-in-aspnet-core"></a><span data-ttu-id="017d2-103">Comprobaciones de estado en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="017d2-103">Health checks in ASP.NET Core</span></span>

<span data-ttu-id="017d2-104">Por [Luke Latham](https://github.com/guardrex) y [Glenn Condron](https://github.com/glennc)</span><span class="sxs-lookup"><span data-stu-id="017d2-104">By [Luke Latham](https://github.com/guardrex) and [Glenn Condron](https://github.com/glennc)</span></span>

<span data-ttu-id="017d2-105">ASP.NET Core ofrece el Middleware de comprobaciones de estado y bibliotecas para informar sobre el estado de los componentes de la infraestructura de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="017d2-105">ASP.NET Core offers Health Check Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="017d2-106">Una aplicación se encarga de exponer las comprobaciones de estado como puntos de conexión HTTP.</span><span class="sxs-lookup"><span data-stu-id="017d2-106">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="017d2-107">Los puntos de conexión de las comprobaciones de estado pueden configurarse para diversos escenarios de supervisión en tiempo real:</span><span class="sxs-lookup"><span data-stu-id="017d2-107">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="017d2-108">Los orquestadores de contenedores y los equilibradores de carga pueden utilizar los sondeos de estado para comprobar el estado de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="017d2-108">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="017d2-109">Por ejemplo, para responder a una comprobación de estado con errores, es posible que un orquestador de contenedores detenga una implementación en curso o reinicie un contenedor.</span><span class="sxs-lookup"><span data-stu-id="017d2-109">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="017d2-110">Como respuesta a una aplicación con estado incorrecto, es posible que un equilibrador de carga enrute el tráfico al margen de la instancia con errores hacia una instancia con estado correcto.</span><span class="sxs-lookup"><span data-stu-id="017d2-110">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="017d2-111">El uso de la memoria, el disco y otros recursos del servidor físico puede supervisarse para determinar si el estado es correcto.</span><span class="sxs-lookup"><span data-stu-id="017d2-111">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="017d2-112">Las comprobaciones de estado pueden probar las dependencias de una aplicación, como las bases de datos y los puntos de conexión de servicio externo, para confirmar la disponibilidad y el funcionamiento normal.</span><span class="sxs-lookup"><span data-stu-id="017d2-112">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="017d2-113">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="017d2-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="017d2-114">La aplicación de muestra incluye ejemplos de los escenarios descritos en este tema.</span><span class="sxs-lookup"><span data-stu-id="017d2-114">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="017d2-115">Para ejecutar la aplicación de ejemplo para un escenario determinado, use el comando [dotnet run](/dotnet/core/tools/dotnet-run) desde la carpeta del proyecto en un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="017d2-115">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="017d2-116">Para obtener información sobre cómo utilizar la aplicación de ejemplo, consulte el archivo *README.md* de la aplicación de ejemplo y las descripciones de escenarios de este tema.</span><span class="sxs-lookup"><span data-stu-id="017d2-116">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="017d2-117">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="017d2-117">Prerequisites</span></span>

<span data-ttu-id="017d2-118">Normalmente, las comprobaciones de estado se usan con un servicio de supervisión externa o un orquestador de contenedores para comprobar el estado de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="017d2-118">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="017d2-119">Antes de agregar comprobaciones de estado a una aplicación, debe decidir en qué sistema de supervisión se va a usar.</span><span class="sxs-lookup"><span data-stu-id="017d2-119">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="017d2-120">El sistema de supervisión determina qué tipos de comprobaciones de estado se deben crear y cómo configurar sus puntos de conexión.</span><span class="sxs-lookup"><span data-stu-id="017d2-120">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="017d2-121">Haga referencia al [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) o agregue una referencia de paquete al paquete [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks).</span><span class="sxs-lookup"><span data-stu-id="017d2-121">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package.</span></span>

<span data-ttu-id="017d2-122">La aplicación de ejemplo proporciona código de inicio para mostrar las comprobaciones de estado para varios escenarios.</span><span class="sxs-lookup"><span data-stu-id="017d2-122">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="017d2-123">El escenario [sondeo de bases de datos](#database-probe) comprueba el estado de una conexión de base de datos mediante [BeatPulse](https://github.com/Xabaril/BeatPulse).</span><span class="sxs-lookup"><span data-stu-id="017d2-123">The [database probe](#database-probe) scenario checks the health of a database connection using [BeatPulse](https://github.com/Xabaril/BeatPulse).</span></span> <span data-ttu-id="017d2-124">El escenario [sondeo de DbContext](#entity-framework-core-dbcontext-probe) comprueba una base de datos mediante un elemento `DbContext` de EF Core.</span><span class="sxs-lookup"><span data-stu-id="017d2-124">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="017d2-125">Para explorar los escenarios de la base de datos, la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="017d2-125">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="017d2-126">Crea una base de datos y proporciona su cadena de conexión en el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="017d2-126">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="017d2-127">Tiene las siguientes referencias de paquete en su archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="017d2-127">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="017d2-128">AspNetCore.HealthChecks.SqlServer</span><span class="sxs-lookup"><span data-stu-id="017d2-128">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="017d2-129">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="017d2-129">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="017d2-130">Microsoft no se encarga del mantenimiento de [BeatPulse](https://github.com/Xabaril/BeatPulse) ni ofrece soporte técnico.</span><span class="sxs-lookup"><span data-stu-id="017d2-130">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="017d2-131">Otro escenario de comprobación de estado muestra cómo filtrar las comprobaciones de estado por un puerto de administración.</span><span class="sxs-lookup"><span data-stu-id="017d2-131">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="017d2-132">La aplicación de ejemplo requiere la creación de un archivo *Properties/launchSettings.json* que incluya la dirección URL de administración y el puerto de administración.</span><span class="sxs-lookup"><span data-stu-id="017d2-132">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="017d2-133">Para obtener más información, consulte la sección [Filtrado por puerto](#filter-by-port).</span><span class="sxs-lookup"><span data-stu-id="017d2-133">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="017d2-134">Sondeo de estado básico</span><span class="sxs-lookup"><span data-stu-id="017d2-134">Basic health probe</span></span>

<span data-ttu-id="017d2-135">Para muchas aplicaciones, una configuración de sondeo de estado básico que notifique la disponibilidad de la aplicación para procesar las solicitudes (*ejecución*) es suficiente para detectar el estado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="017d2-135">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="017d2-136">La configuración básica registra los servicios de comprobación de estado y llama al Middleware de comprobaciones de estado para responder a un punto de conexión de dirección URL con una respuesta de estado.</span><span class="sxs-lookup"><span data-stu-id="017d2-136">The basic configuration registers health check services and calls the Health Check Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="017d2-137">De forma predeterminada, no se registran comprobaciones de estado específicas para probar cualquier dependencia o subsistema concretos.</span><span class="sxs-lookup"><span data-stu-id="017d2-137">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="017d2-138">La aplicación se considera correcta si es capaz de responder en la dirección URL de punto de conexión de estado.</span><span class="sxs-lookup"><span data-stu-id="017d2-138">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="017d2-139">El escritor de respuesta predeterminado escribe el estado (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) como respuesta de texto no cifrado al cliente, que indica un estado [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) o [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus).</span><span class="sxs-lookup"><span data-stu-id="017d2-139">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="017d2-140">Registre los servicios de comprobación de estado con <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> de `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="017d2-140">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="017d2-141">Agregue el Middleware de comprobaciones de estado con <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> en la canalización de procesamiento de solicitudes de `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="017d2-141">Add Health Check Middleware with <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the request processing pipeline of `Startup.Configure`.</span></span>

<span data-ttu-id="017d2-142">En la aplicación de ejemplo, el punto de conexión de la comprobación de estado se crea en `/health` (*BasicStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="017d2-142">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/BasicStartup.cs?name=snippet1&highlight=5,10)]

<span data-ttu-id="017d2-143">Para ejecutar el escenario de configuración básica mediante la aplicación de ejemplo, ejecute el comando siguiente desde la carpeta del proyecto en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="017d2-143">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="017d2-144">Ejemplo de Docker</span><span class="sxs-lookup"><span data-stu-id="017d2-144">Docker example</span></span>

<span data-ttu-id="017d2-145">[Docker](xref:host-and-deploy/docker/index) ofrece una directiva de `HEALTHCHECK` integrada que puede utilizarse para comprobar el estado de una aplicación que use la configuración de comprobación de estado básica:</span><span class="sxs-lookup"><span data-stu-id="017d2-145">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="017d2-146">Creación de comprobaciones de estado</span><span class="sxs-lookup"><span data-stu-id="017d2-146">Create health checks</span></span>

<span data-ttu-id="017d2-147">Las comprobaciones de estado se crean mediante la implementación de la interfaz de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck>.</span><span class="sxs-lookup"><span data-stu-id="017d2-147">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="017d2-148">El método <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> devuelve un elemento `Task<` <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> `>` que indica el estado como `Healthy`, `Degraded` o `Unhealthy`.</span><span class="sxs-lookup"><span data-stu-id="017d2-148">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a `Task<` <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> `>` that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="017d2-149">El resultado se escribe como una respuesta de texto no cifrado con un código de estado configurable. (La configuración se describe en la sección [Opciones de comprobación de estado](#health-check-options)).</span><span class="sxs-lookup"><span data-stu-id="017d2-149">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="017d2-150"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> también puede devolver pares clave-valor opcionales.</span><span class="sxs-lookup"><span data-stu-id="017d2-150"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

### <a name="example-health-check"></a><span data-ttu-id="017d2-151">Comprobación de estado de ejemplo</span><span class="sxs-lookup"><span data-stu-id="017d2-151">Example health check</span></span>

<span data-ttu-id="017d2-152">La siguiente clase `ExampleHealthCheck` muestra el diseño de una comprobación de estado:</span><span class="sxs-lookup"><span data-stu-id="017d2-152">The following `ExampleHealthCheck` class demonstrates the layout of a health check:</span></span>

```csharp
public class ExampleHealthCheck : IHealthCheck
{
    public ExampleHealthCheck()
    {
        // Use dependency injection (DI) to supply any required services to the
        // health check.
    }

    public Task<HealthCheckResult> CheckHealthAsync(
        HealthCheckContext context,
        CancellationToken cancellationToken = default(CancellationToken))
    {
        // Execute health check logic here. This example sets a dummy
        // variable to true.
        var healthCheckResultHealthy = true;

        if (healthCheckResultHealthy)
        {
            return Task.FromResult(
                HealthCheckResult.Healthy("The check indicates a healthy result."));
        }

        return Task.FromResult(
            HealthCheckResult.Unhealthy("The check indicates an unhealthy result."));
    }
}
```

### <a name="register-health-check-services"></a><span data-ttu-id="017d2-153">Registro de los servicios de comprobación de estado</span><span class="sxs-lookup"><span data-stu-id="017d2-153">Register health check services</span></span>

<span data-ttu-id="017d2-154">El tipo `ExampleHealthCheck` se agrega a los servicios de comprobación de estado con <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>:</span><span class="sxs-lookup"><span data-stu-id="017d2-154">The `ExampleHealthCheck` type is added to health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck<ExampleHealthCheck>("example_health_check");
}
```

<span data-ttu-id="017d2-155">La sobrecarga <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> que se muestra en el ejemplo siguiente establece el estado de error (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) para notificar cuándo la comprobación de estado informa de un error.</span><span class="sxs-lookup"><span data-stu-id="017d2-155">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="017d2-156">Si el estado de error se establece en `null` (valor predeterminado), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) se notifica.</span><span class="sxs-lookup"><span data-stu-id="017d2-156">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="017d2-157">Esta sobrecarga es un escenario útil para los creadores de bibliotecas en los que la aplicación ejecuta el estado de error indicado por la biblioteca cuando se produce un error de comprobación de estado si la implementación de la comprobación de estado respeta la configuración.</span><span class="sxs-lookup"><span data-stu-id="017d2-157">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="017d2-158">Las *etiquetas* pueden usarse para filtrar las comprobaciones de estado, que se describen con más detalle en la sección [Filtrado de las comprobaciones de estado](#filter-health-checks).</span><span class="sxs-lookup"><span data-stu-id="017d2-158">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<span data-ttu-id="017d2-159"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> también puede ejecutar una función lambda.</span><span class="sxs-lookup"><span data-stu-id="017d2-159"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="017d2-160">En el ejemplo siguiente, el nombre de la comprobación de estado se especifica como `Example` y la comprobación siempre devuelve un estado correcto:</span><span class="sxs-lookup"><span data-stu-id="017d2-160">In the following example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck("Example", () =>
            HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" })
}
```

### <a name="use-health-checks-middleware"></a><span data-ttu-id="017d2-161">Uso del Middleware de comprobaciones de estado</span><span class="sxs-lookup"><span data-stu-id="017d2-161">Use Health Checks Middleware</span></span>

<span data-ttu-id="017d2-162">En `Startup.Configure`, llame a <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> en la canalización de procesamiento con la dirección URL del punto de conexión o la ruta de acceso relativa:</span><span class="sxs-lookup"><span data-stu-id="017d2-162">In `Startup.Configure`, call <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the processing pipeline with the endpoint URL or relative path:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health");
}
```

<span data-ttu-id="017d2-163">Si las comprobaciones de estado deben realizar la escucha en un puerto específico, use una sobrecarga de <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> para establecer el puerto, como se describe con más detalle en la sección [Filtrado por puerto](#filter-by-port):</span><span class="sxs-lookup"><span data-stu-id="017d2-163">If the health checks should listen on a specific port, use an overload of <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> to set the port (described further in the [Filter by port](#filter-by-port) section):</span></span>

```csharp
app.UseHealthChecks("/health", port: 8000);
```

<span data-ttu-id="017d2-164">El Middleware de comprobaciones de estado es un *middleware terminal* en la canalización de procesamiento de solicitudes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="017d2-164">Health Checks Middleware is a *terminal middleware* in the app's request processing pipeline.</span></span> <span data-ttu-id="017d2-165">El primer punto de conexión de comprobación de estado encontrado que sea una coincidencia exacta con la dirección URL de la solicitud ejecuta y cortocircuita el resto de la canalización de middleware.</span><span class="sxs-lookup"><span data-stu-id="017d2-165">The first health check endpoint encountered that's an exact match to the request URL executes and short-circuits the rest of the middleware pipeline.</span></span> <span data-ttu-id="017d2-166">Cuando se produce el cortocircuito, no se ejecuta ningún middleware que siga la comprobación de estado coincidente.</span><span class="sxs-lookup"><span data-stu-id="017d2-166">When short-circuiting occurs, no middleware following the matched health check executes.</span></span>

## <a name="health-check-options"></a><span data-ttu-id="017d2-167">Opciones de comprobación de estado</span><span class="sxs-lookup"><span data-stu-id="017d2-167">Health check options</span></span>

<span data-ttu-id="017d2-168">El elemento <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> ofrece una oportunidad para personalizar el comportamiento de las comprobaciones de estado:</span><span class="sxs-lookup"><span data-stu-id="017d2-168"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="017d2-169">Filtrado de las comprobaciones de estado</span><span class="sxs-lookup"><span data-stu-id="017d2-169">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="017d2-170">Personalización del código de estado HTTP</span><span class="sxs-lookup"><span data-stu-id="017d2-170">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="017d2-171">Supresión de los encabezados de caché</span><span class="sxs-lookup"><span data-stu-id="017d2-171">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="017d2-172">Personalización del resultado</span><span class="sxs-lookup"><span data-stu-id="017d2-172">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="017d2-173">Filtrado de las comprobaciones de estado</span><span class="sxs-lookup"><span data-stu-id="017d2-173">Filter health checks</span></span>

<span data-ttu-id="017d2-174">De forma predeterminada, el Middleware de comprobaciones de estado ejecuta todas las comprobaciones de estado registradas.</span><span class="sxs-lookup"><span data-stu-id="017d2-174">By default, Health Check Middleware runs all registered health checks.</span></span> <span data-ttu-id="017d2-175">Para ejecutar un subconjunto de comprobaciones de estado, proporcione una función que devuelva un valor booleano para la opción <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate>.</span><span class="sxs-lookup"><span data-stu-id="017d2-175">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="017d2-176">En el ejemplo siguiente, la etiqueta (`bar_tag`) de la comprobación de estado `Bar` la filtra, en la instrucción condicional de la función, donde `true` solo se devuelve si la propiedad <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> de la comprobación de estado coincide con `foo_tag` o `baz_tag`:</span><span class="sxs-lookup"><span data-stu-id="017d2-176">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

```csharp
using System.Threading.Tasks;
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck("Foo", () =>
            HealthCheckResult.Healthy("Foo is OK!"), tags: new[] { "foo_tag" })
        .AddCheck("Bar", () =>
            HealthCheckResult.Unhealthy("Bar is unhealthy!"), tags: new[] { "bar_tag" })
        .AddCheck("Baz", () =>
            HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // Filter out the 'Bar' health check. Only Foo and Baz execute.
        Predicate = (check) => check.Tags.Contains("foo_tag") ||
            check.Tags.Contains("baz_tag")
    });
}
```

### <a name="customize-the-http-status-code"></a><span data-ttu-id="017d2-177">Personalización del código de estado HTTP</span><span class="sxs-lookup"><span data-stu-id="017d2-177">Customize the HTTP status code</span></span>

<span data-ttu-id="017d2-178">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> para personalizar la asignación del estado de mantenimiento de los códigos de estado HTTP.</span><span class="sxs-lookup"><span data-stu-id="017d2-178">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="017d2-179">Las siguientes asignaciones de <xref:Microsoft.AspNetCore.Http.StatusCodes> son los valores predeterminados que el middleware utiliza.</span><span class="sxs-lookup"><span data-stu-id="017d2-179">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="017d2-180">Cambie los valores de código de estado para satisfacer sus necesidades.</span><span class="sxs-lookup"><span data-stu-id="017d2-180">Change the status code values to meet your requirements.</span></span>

```csharp
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // The following StatusCodes are the default assignments for
        // the HealthStatus properties.
        ResultStatusCodes =
        {
            [HealthStatus.Healthy] = StatusCodes.Status200OK,
            [HealthStatus.Degraded] = StatusCodes.Status200OK,
            [HealthStatus.Unhealthy] = StatusCodes.Status503ServiceUnavailable
        }
    });
}
```

### <a name="suppress-cache-headers"></a><span data-ttu-id="017d2-181">Supresión de los encabezados de caché</span><span class="sxs-lookup"><span data-stu-id="017d2-181">Suppress cache headers</span></span>

<span data-ttu-id="017d2-182"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controla si el Middleware de comprobaciones de estado agrega encabezados HTTP a una respuesta de sondeo para evitar el almacenamiento en caché de respuesta.</span><span class="sxs-lookup"><span data-stu-id="017d2-182"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Check Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="017d2-183">Si el valor es `false` (valor predeterminado), el middleware establece o invalida los encabezados `Cache-Control`, `Expires` y `Pragma` para evitar el almacenamiento en caché de respuesta.</span><span class="sxs-lookup"><span data-stu-id="017d2-183">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="017d2-184">Si el valor es `true`, el middleware no modifica los encabezados de caché de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="017d2-184">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

```csharp
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // The default value is false.
        AllowCachingResponses = false
    });
}
```

### <a name="customize-output"></a><span data-ttu-id="017d2-185">Personalización del resultado</span><span class="sxs-lookup"><span data-stu-id="017d2-185">Customize output</span></span>

<span data-ttu-id="017d2-186">La opción <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> obtiene o establece un delegado que se usa para escribir la respuesta.</span><span class="sxs-lookup"><span data-stu-id="017d2-186">The <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> option gets or sets a delegate used to write the response.</span></span> <span data-ttu-id="017d2-187">El delegado predeterminado escribe una respuesta de texto no cifrado mínima con el valor de cadena [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span><span class="sxs-lookup"><span data-stu-id="017d2-187">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span>

```csharp
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // WriteResponse is a delegate used to write the response.
        ResponseWriter = WriteResponse
    });
}

private static Task WriteResponse(HttpContext httpContext,
    HealthReport result)
{
    httpContext.Response.ContentType = "application/json";

    var json = new JObject(
        new JProperty("status", result.Status.ToString()),
        new JProperty("results", new JObject(result.Entries.Select(pair =>
            new JProperty(pair.Key, new JObject(
                new JProperty("status", pair.Value.Status.ToString()),
                new JProperty("description", pair.Value.Description),
                new JProperty("data", new JObject(pair.Value.Data.Select(
                    p => new JProperty(p.Key, p.Value))))))))));
    return httpContext.Response.WriteAsync(
        json.ToString(Formatting.Indented));
}
```

## <a name="database-probe"></a><span data-ttu-id="017d2-188">Sondeo de bases de datos</span><span class="sxs-lookup"><span data-stu-id="017d2-188">Database probe</span></span>

<span data-ttu-id="017d2-189">Una comprobación de estado puede especificar que una consulta de base de datos se ejecute como una prueba booleana para indicar si esta responde con normalidad.</span><span class="sxs-lookup"><span data-stu-id="017d2-189">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="017d2-190">La aplicación de ejemplo usa [BeatPulse](https://github.com/Xabaril/BeatPulse), una biblioteca de comprobaciones de estado para las aplicaciones de ASP.NET Core, para realizar una comprobación de estado en una base de datos de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="017d2-190">The sample app uses [BeatPulse](https://github.com/Xabaril/BeatPulse), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="017d2-191">BeatPulse ejecuta una consulta `SELECT 1` en la base de datos para confirmar que la conexión a la base de datos es correcta.</span><span class="sxs-lookup"><span data-stu-id="017d2-191">BeatPulse executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="017d2-192">Al comprobar la conexión de una base de datos a una consulta, elija una consulta que se devuelva rápidamente.</span><span class="sxs-lookup"><span data-stu-id="017d2-192">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="017d2-193">El enfoque de la consulta plantea el riesgo de sobrecargar la base de datos y degradar el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="017d2-193">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="017d2-194">En la mayoría de los casos, no es necesario ejecutar una consulta de prueba.</span><span class="sxs-lookup"><span data-stu-id="017d2-194">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="017d2-195">Simplemente, realizar una conexión correcta a la base de datos es suficiente.</span><span class="sxs-lookup"><span data-stu-id="017d2-195">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="017d2-196">Si resulta necesario ejecutar una consulta, elija una consulta SELECT sencilla, como `SELECT 1`.</span><span class="sxs-lookup"><span data-stu-id="017d2-196">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="017d2-197">Para usar la biblioteca BeatPulse, incluya una referencia de paquete a [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="017d2-197">In order to use the BeatPulse library, include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="017d2-198">Proporcione una cadena de conexión a base de datos válida en el archivo *appsettings.json* de la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="017d2-198">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="017d2-199">La aplicación usa una base de datos de SQL Server denominada `HealthCheckSample`:</span><span class="sxs-lookup"><span data-stu-id="017d2-199">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="017d2-200">Registre los servicios de comprobación de estado con <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> de `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="017d2-200">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="017d2-201">La aplicación de ejemplo llama al método `AddSqlServer` de BeatPulse con la cadena de conexión de la base de datos (*DbHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="017d2-201">The sample app calls BeatPulse's `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="017d2-202">Llame al Middleware de comprobaciones de estado en la canalización de procesamiento de la aplicación en `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="017d2-202">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_Configure)]

<span data-ttu-id="017d2-203">Para ejecutar el escenario de sondeo de base de datos mediante la aplicación de ejemplo, ejecute el comando siguiente desde la carpeta del proyecto en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="017d2-203">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="017d2-204">Microsoft no se encarga del mantenimiento de [BeatPulse](https://github.com/Xabaril/BeatPulse) ni ofrece soporte técnico.</span><span class="sxs-lookup"><span data-stu-id="017d2-204">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="017d2-205">Sondeo de DbContext de Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="017d2-205">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="017d2-206">La comprobación `DbContext` confirma que la aplicación puede comunicarse con la base de datos configurada para un elemento `DbContext` de EF Core.</span><span class="sxs-lookup"><span data-stu-id="017d2-206">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="017d2-207">La comprobación `DbContext` se admite en las aplicaciones que:</span><span class="sxs-lookup"><span data-stu-id="017d2-207">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="017d2-208">Usan [Entity Framework (EF) Core](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="017d2-208">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="017d2-209">Incluyen una referencia de paquete a [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span><span class="sxs-lookup"><span data-stu-id="017d2-209">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="017d2-210">`AddDbContextCheck<TContext>` registra una comprobación de estado para un elemento `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="017d2-210">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="017d2-211">El elemento `DbContext` se proporciona como `TContext` en el método.</span><span class="sxs-lookup"><span data-stu-id="017d2-211">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="017d2-212">Hay disponible una sobrecarga para configurar el estado de error, las etiquetas y una consulta de prueba personalizada.</span><span class="sxs-lookup"><span data-stu-id="017d2-212">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="017d2-213">De manera predeterminada:</span><span class="sxs-lookup"><span data-stu-id="017d2-213">By default:</span></span>

* <span data-ttu-id="017d2-214">`DbContextHealthCheck` llama al método `CanConnectAsync` de EF Core.</span><span class="sxs-lookup"><span data-stu-id="017d2-214">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="017d2-215">Se puede personalizar qué operación se ejecuta al comprobar el estado con sobrecargas del método `AddDbContextCheck`.</span><span class="sxs-lookup"><span data-stu-id="017d2-215">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="017d2-216">El nombre de la comprobación de estado es el nombre del tipo `TContext`.</span><span class="sxs-lookup"><span data-stu-id="017d2-216">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="017d2-217">En la aplicación de ejemplo, `AppDbContext` se proporciona para `AddDbContextCheck` y se registra como un servicio en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="017d2-217">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="017d2-218">*DbContextHealthStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="017d2-218">*DbContextHealthStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="017d2-219">En la aplicación de ejemplo, `UseHealthChecks` agrega el Middleware de comprobaciones de estado en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="017d2-219">In the sample app, `UseHealthChecks` adds the Health Check Middleware in `Startup.Configure`.</span></span>

<span data-ttu-id="017d2-220">*DbContextHealthStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="017d2-220">*DbContextHealthStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_Configure)]

<span data-ttu-id="017d2-221">Para ejecutar el escenario de sondeo de `DbContext` mediante la aplicación de ejemplo, confirme que la base de datos que la cadena de conexión especifica no exista en la instancia de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="017d2-221">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="017d2-222">Si la base de datos existe, elimínela.</span><span class="sxs-lookup"><span data-stu-id="017d2-222">If the database exists, delete it.</span></span>

<span data-ttu-id="017d2-223">Ejecute el comando siguiente desde la carpeta del proyecto en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="017d2-223">Execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario dbcontext
```

<span data-ttu-id="017d2-224">Cuando la aplicación ya se esté ejecutando, compruebe el estado de mantenimiento mediante una solicitud al punto de conexión `/health` en un explorador.</span><span class="sxs-lookup"><span data-stu-id="017d2-224">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="017d2-225">La base de datos y `AppDbContext` no existen, por lo que la aplicación proporciona la respuesta siguiente:</span><span class="sxs-lookup"><span data-stu-id="017d2-225">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="017d2-226">Active la aplicación de ejemplo para crear la base de datos.</span><span class="sxs-lookup"><span data-stu-id="017d2-226">Trigger the sample app to create the database.</span></span> <span data-ttu-id="017d2-227">Realice una solicitud a `/createdatabase`.</span><span class="sxs-lookup"><span data-stu-id="017d2-227">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="017d2-228">La aplicación responde:</span><span class="sxs-lookup"><span data-stu-id="017d2-228">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="017d2-229">Realice una solicitud al punto de conexión `/health`.</span><span class="sxs-lookup"><span data-stu-id="017d2-229">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="017d2-230">La base de datos y el contexto existen, por lo que la aplicación responde:</span><span class="sxs-lookup"><span data-stu-id="017d2-230">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="017d2-231">Active la aplicación de ejemplo para eliminar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="017d2-231">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="017d2-232">Realice una solicitud a `/deletedatabase`.</span><span class="sxs-lookup"><span data-stu-id="017d2-232">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="017d2-233">La aplicación responde:</span><span class="sxs-lookup"><span data-stu-id="017d2-233">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="017d2-234">Realice una solicitud al punto de conexión `/health`.</span><span class="sxs-lookup"><span data-stu-id="017d2-234">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="017d2-235">La aplicación proporciona una respuesta incorrecta:</span><span class="sxs-lookup"><span data-stu-id="017d2-235">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="017d2-236">Sondeos de preparación y ejecución independientes</span><span class="sxs-lookup"><span data-stu-id="017d2-236">Separate readiness and liveness probes</span></span>

<span data-ttu-id="017d2-237">En algunos escenarios de hospedaje, se usa un par de comprobaciones de estado que distinguen los dos estados de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="017d2-237">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="017d2-238">La aplicación está funcionando, pero aún no está preparada para recibir solicitudes.</span><span class="sxs-lookup"><span data-stu-id="017d2-238">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="017d2-239">Este estado es la *preparación* de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="017d2-239">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="017d2-240">La aplicación está funcionando y responde a las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="017d2-240">The app is functioning and responding to requests.</span></span> <span data-ttu-id="017d2-241">Este estado es la *ejecución* de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="017d2-241">This state is the app's *liveness*.</span></span>

<span data-ttu-id="017d2-242">Normalmente, la comprobación de preparación realiza un conjunto más amplio de comprobaciones, que requieren mucho tiempo, para determinar si están disponibles todos los subsistemas y los recursos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="017d2-242">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="017d2-243">Una comprobación de ejecución simplemente realiza una comprobación rápida para determinar si la aplicación está disponible para procesar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="017d2-243">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="017d2-244">Después de que la aplicación pase la comprobación de preparación, no es necesario sobrecargar aún más la aplicación con el conjunto amplio de comprobaciones de preparación; en las comprobaciones siguientes solo se requiere comprobar la ejecución.</span><span class="sxs-lookup"><span data-stu-id="017d2-244">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="017d2-245">La aplicación de ejemplo contiene una comprobación de estado para notificar la finalización de la tarea de inicio de ejecución prolongada en un [servicio hospedado](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="017d2-245">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="017d2-246">El elemento `StartupHostedServiceHealthCheck` expone una propiedad, `StartupTaskCompleted`, que el servicio hospedado puede establecer en `true` al terminar su tarea de ejecución prolongada (*StartupHostedServiceHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="017d2-246">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=5)]

<span data-ttu-id="017d2-247">Un [servicio hospedado](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*) se encarga de iniciar la tarea en segundo plano de larga ejecución.</span><span class="sxs-lookup"><span data-stu-id="017d2-247">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="017d2-248">Al finalizar la tarea, `StartupHostedServiceHealthCheck.StartupTaskCompleted` se establece en `true`:</span><span class="sxs-lookup"><span data-stu-id="017d2-248">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="017d2-249">La comprobación de estado se registra con <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> en `Startup.ConfigureServices` junto con el servicio hospedado.</span><span class="sxs-lookup"><span data-stu-id="017d2-249">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="017d2-250">Dado que el servicio hospedado debe establecer la propiedad en la comprobación de estado, esta también se registra en el contenedor de servicios (*LivenessProbeStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="017d2-250">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="017d2-251">Llame al Middleware de comprobaciones de estado en la canalización de procesamiento de la aplicación en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="017d2-251">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="017d2-252">En la aplicación de ejemplo, se crean puntos de conexión de la comprobación de estado en `/health/ready` para la comprobación de preparación y en `/health/live` para la comprobación de ejecución.</span><span class="sxs-lookup"><span data-stu-id="017d2-252">In the sample app, the health check endpoints are created at `/health/ready` for the readiness check and `/health/live` for the liveness check.</span></span> <span data-ttu-id="017d2-253">La comprobación de preparación filtra las comprobaciones de estado con la etiqueta `ready`.</span><span class="sxs-lookup"><span data-stu-id="017d2-253">The readiness check filters health checks to the health check with the `ready` tag.</span></span> <span data-ttu-id="017d2-254">La comprobación de ejecución filtra el elemento `StartupHostedServiceHealthCheck` y devuelve `false` en [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate). Para obtener más información, consulte [Filtrado de las comprobaciones de estado](#filter-health-checks):</span><span class="sxs-lookup"><span data-stu-id="017d2-254">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks)):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_Configure)]

<span data-ttu-id="017d2-255">Para ejecutar el escenario de configuración de la preparación/ejecución mediante la aplicación de ejemplo, ejecute el comando siguiente desde la carpeta del proyecto en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="017d2-255">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario liveness
```

<span data-ttu-id="017d2-256">En un explorador, visite `/health/ready` varias veces hasta que hayan pasado 15 segundos.</span><span class="sxs-lookup"><span data-stu-id="017d2-256">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="017d2-257">La comprobación de estado notifica un estado *Incorrecto* durante los primeros 15 segundos.</span><span class="sxs-lookup"><span data-stu-id="017d2-257">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="017d2-258">Pasados 15 segundos, el punto de conexión notifica un estado *Correcto*, lo que indica que el servicio hospedado ya ha finalizado la tarea de ejecución prolongada.</span><span class="sxs-lookup"><span data-stu-id="017d2-258">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="017d2-259">En este ejemplo también se crea un publicador de la comprobación de estado (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementación) que ejecuta la primera comprobación de preparación con un retraso de dos segundos.</span><span class="sxs-lookup"><span data-stu-id="017d2-259">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="017d2-260">Para obtener más información, consulte la sección [Publicador de la comprobación de estado](#health-check-publisher).</span><span class="sxs-lookup"><span data-stu-id="017d2-260">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="017d2-261">Ejemplo de Kubernetes</span><span class="sxs-lookup"><span data-stu-id="017d2-261">Kubernetes example</span></span>

<span data-ttu-id="017d2-262">Utilizar comprobaciones de preparación y ejecución independientes es útil en un entorno como [Kubernetes](https://kubernetes.io/).</span><span class="sxs-lookup"><span data-stu-id="017d2-262">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="017d2-263">En Kubernetes, es posible que una aplicación deba realizar un trabajo de inicio que requiera mucho tiempo antes de aceptar solicitudes, como una prueba de la disponibilidad de la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="017d2-263">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="017d2-264">El hecho de utilizar comprobaciones independientes permite que el orquestador distinga si la aplicación está funcionando, pero aún no esté preparada, o si la aplicación no se ha podido iniciar.</span><span class="sxs-lookup"><span data-stu-id="017d2-264">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="017d2-265">Para obtener más información sobre los sondeos de preparación y ejecución en Kubernetes, consulte [Configuración de sondeos de preparación y ejecución](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) en la documentación de Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="017d2-265">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="017d2-266">En el ejemplo siguiente, se muestra una configuración de sondeo de preparación de Kubernetes:</span><span class="sxs-lookup"><span data-stu-id="017d2-266">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

```
spec:
  template:
  spec:
    readinessProbe:
      # an http probe
      httpGet:
        path: /health/ready
        port: 80
      # length of time to wait for a pod to initialize
      # after pod startup, before applying health checking
      initialDelaySeconds: 30
      timeoutSeconds: 1
    ports:
      - containerPort: 80
```

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="017d2-267">Sondeo basado en métrica con un escritor de respuesta personalizada</span><span class="sxs-lookup"><span data-stu-id="017d2-267">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="017d2-268">La aplicación de ejemplo muestra una comprobación de estado de memoria con un escritor de respuesta personalizada.</span><span class="sxs-lookup"><span data-stu-id="017d2-268">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="017d2-269">`MemoryHealthCheck` notifica un estado degradado si la aplicación usa más de un umbral de memoria determinado (1 GB en la aplicación de ejemplo).</span><span class="sxs-lookup"><span data-stu-id="017d2-269">`MemoryHealthCheck` reports a degraded status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="017d2-270">El elemento <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> incluye información del recolector de elementos no utilizados (GC) de la aplicación (*MemoryHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="017d2-270">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="017d2-271">Registre los servicios de comprobación de estado con <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> de `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="017d2-271">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="017d2-272">En lugar de pasar la comprobación de estado a <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> para habilitarla, `MemoryHealthCheck` se registra como servicio.</span><span class="sxs-lookup"><span data-stu-id="017d2-272">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="017d2-273">Todos los servicios registrados de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> están disponibles para los servicios de comprobación de estado y middleware.</span><span class="sxs-lookup"><span data-stu-id="017d2-273">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="017d2-274">Se recomienda registrar los servicios de comprobación de estado como los servicios de Singleton.</span><span class="sxs-lookup"><span data-stu-id="017d2-274">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="017d2-275">*CustomWriterStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="017d2-275">*CustomWriterStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="017d2-276">Llame al Middleware de comprobaciones de estado en la canalización de procesamiento de la aplicación en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="017d2-276">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="017d2-277">Se proporciona un delegado de `WriteResponse` a la propiedad `ResponseWriter` para generar una respuesta JSON personalizada al ejecutarse la comprobación de estado:</span><span class="sxs-lookup"><span data-stu-id="017d2-277">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_Configure&highlight=6)]

<span data-ttu-id="017d2-278">El método `WriteResponse` da a `CompositeHealthCheckResult` el formato de objeto JSON y suspende el resultado de JSON para la respuesta de comprobación de estado:</span><span class="sxs-lookup"><span data-stu-id="017d2-278">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="017d2-279">Para ejecutar el sondeo basado en métrica con el resultado de un escritor de respuesta personalizada mediante la aplicación de ejemplo, ejecute el comando siguiente desde la carpeta del proyecto en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="017d2-279">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="017d2-280">[BeatPulse](https://github.com/Xabaril/BeatPulse) incluye escenarios de comprobación de estado basados en métrica, como las comprobaciones del almacenamiento del disco y de ejecución de máximo valor.</span><span class="sxs-lookup"><span data-stu-id="017d2-280">[BeatPulse](https://github.com/Xabaril/BeatPulse) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="017d2-281">Microsoft no se encarga del mantenimiento de [BeatPulse](https://github.com/Xabaril/BeatPulse) ni ofrece soporte técnico.</span><span class="sxs-lookup"><span data-stu-id="017d2-281">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="017d2-282">Filtrado por puerto</span><span class="sxs-lookup"><span data-stu-id="017d2-282">Filter by port</span></span>

<span data-ttu-id="017d2-283">Una llamada a <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> con un puerto restringe las solicitudes de comprobación de estado al puerto especificado.</span><span class="sxs-lookup"><span data-stu-id="017d2-283">Calling <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> with a port restricts health check requests to the port specified.</span></span> <span data-ttu-id="017d2-284">Esto se usa normalmente en un entorno de contenedor para exponer un puerto para los servicios de supervisión.</span><span class="sxs-lookup"><span data-stu-id="017d2-284">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="017d2-285">La aplicación de ejemplo configura el puerto con el [proveedor de configuración de variable de entorno](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="017d2-285">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="017d2-286">El puerto se establece en el archivo *launchSettings.json* y se pasa al proveedor de configuración a través de una variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="017d2-286">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="017d2-287">También debe configurar el servidor para que escuche las solicitudes en el puerto de administración.</span><span class="sxs-lookup"><span data-stu-id="017d2-287">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="017d2-288">Para utilizar la aplicación de ejemplo para que muestre la configuración del puerto de administración, cree el archivo *launchSettings.json* en una carpeta *Propiedades*.</span><span class="sxs-lookup"><span data-stu-id="017d2-288">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="017d2-289">El siguiente archivo *launchSettings.json* no se incluye en los archivos de proyecto de la aplicación de ejemplo y debe crearse manualmente.</span><span class="sxs-lookup"><span data-stu-id="017d2-289">The following *launchSettings.json* file isn't included in the sample app's project files and must be created manually.</span></span>

<span data-ttu-id="017d2-290">*Properties/launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="017d2-290">*Properties/launchSettings.json*:</span></span>

```json
{
  "profiles": {
    "SampleApp": {
      "commandName": "Project",
      "commandLineArgs": "",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development",
        "ASPNETCORE_URLS": "http://localhost:5000/;http://localhost:5001/",
        "ASPNETCORE_MANAGEMENTPORT": "5001"
      },
      "applicationUrl": "http://localhost:5000/"
    }
  }
}
```

<span data-ttu-id="017d2-291">Registre los servicios de comprobación de estado con <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> de `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="017d2-291">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="017d2-292">La llamada a <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> especifica el puerto de administración (*ManagementPortStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="017d2-292">The call to <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> specifies the management port (*ManagementPortStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=12,18)]

> [!NOTE]
> <span data-ttu-id="017d2-293">Para evitar la creación del archivo *launchSettings.json* en la aplicación de ejemplo, configure las direcciones URL y el puerto de administración explícitamente en código.</span><span class="sxs-lookup"><span data-stu-id="017d2-293">You can avoid creating the *launchSettings.json* file in the sample app by setting the URLs and management port explicitly in code.</span></span> <span data-ttu-id="017d2-294">En *Program.cs*, donde se crea <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, agregue una llamada a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> y proporcione el punto de conexión de respuesta normal de la aplicación y el punto de conexión del puerto de administración.</span><span class="sxs-lookup"><span data-stu-id="017d2-294">In *Program.cs* where the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> and provide the app's normal response endpoint and the management port endpoint.</span></span> <span data-ttu-id="017d2-295">En *ManagementPortStartup.cs*, donde se llama a <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>, especifique explícitamente el puerto de administración.</span><span class="sxs-lookup"><span data-stu-id="017d2-295">In *ManagementPortStartup.cs* where <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> is called, specify the management port explicitly.</span></span>
>
> <span data-ttu-id="017d2-296">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="017d2-296">*Program.cs*:</span></span>
>
> ```csharp
> return new WebHostBuilder()
>     .UseConfiguration(config)
>     .UseUrls("http://localhost:5000/;http://localhost:5001/")
>     .ConfigureLogging(builder =>
>     {
>         builder.SetMinimumLevel(LogLevel.Trace);
>         builder.AddConfiguration(config);
>         builder.AddConsole();
>     })
>     .UseKestrel()
>     .UseStartup(startupType)
>     .Build();
> ```
>
> <span data-ttu-id="017d2-297">*ManagementPortStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="017d2-297">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseHealthChecks("/health", port: 5001);
> ```

<span data-ttu-id="017d2-298">Para ejecutar el escenario de configuración del puerto de administración mediante la aplicación de ejemplo, ejecute el comando siguiente desde la carpeta del proyecto en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="017d2-298">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="017d2-299">Distribución de una biblioteca de comprobación de estado</span><span class="sxs-lookup"><span data-stu-id="017d2-299">Distribute a health check library</span></span>

<span data-ttu-id="017d2-300">Para distribuir una comprobación de estado como una biblioteca, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="017d2-300">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="017d2-301">Escriba una comprobación de estado que implemente la interfaz de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> como una clase independiente.</span><span class="sxs-lookup"><span data-stu-id="017d2-301">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="017d2-302">La clase puede depender de la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection), de la activación del tipo y de las [opciones denominadas](xref:fundamentals/configuration/options) para acceder a los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="017d2-302">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   ```csharp
   using System;
   using System.Threading;
   using System.Threading.Tasks;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   namespace SampleApp
   {
       public class ExampleHealthCheck : IHealthCheck
       {
           private readonly string _data1;
           private readonly int? _data2;

           public ExampleHealthCheck(string data1, int? data2)
           {
               _data1 = data1 ?? throw new ArgumentNullException(nameof(data1));
               _data2 = data2 ?? throw new ArgumentNullException(nameof(data2));
           }

           public async Task<HealthCheckResult> CheckHealthAsync(
               HealthCheckContext context, CancellationToken cancellationToken)
           {
               try
               {
                   // Health check logic
                   //
                   // data1 and data2 are used in the method to
                   // run the probe's health check logic.

                   // Assume that it's possible for this health check
                   // to throw an AccessViolationException.

                   return HealthCheckResult.Healthy();
               }
               catch (AccessViolationException ex)
               {
                   return new HealthCheckResult(
                       context.Registration.FailureStatus,
                       description: "An access violation occurred during the check.",
                       exception: ex,
                       data: null);
               }
           }
       }
   }
   ```

1. <span data-ttu-id="017d2-303">Escriba un método de extensión con los parámetros a los que la aplicación de uso llama en su método `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="017d2-303">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="017d2-304">En el ejemplo siguiente, suponga que existe la siguiente firma del método de comprobación de estado:</span><span class="sxs-lookup"><span data-stu-id="017d2-304">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="017d2-305">La firma anterior indica que la `ExampleHealthCheck` requiere datos adicionales para procesar la lógica de sondeo de la comprobación de estado.</span><span class="sxs-lookup"><span data-stu-id="017d2-305">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="017d2-306">Los datos se proporcionan al delegado que se usa para crear la instancia de la comprobación de estado cuando la comprobación de estado se registra con un método de extensión.</span><span class="sxs-lookup"><span data-stu-id="017d2-306">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="017d2-307">En el ejemplo siguiente, el autor de llamada especifica los siguientes elementos opcionales:</span><span class="sxs-lookup"><span data-stu-id="017d2-307">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="017d2-308">nombre de la comprobación de estado (`name`).</span><span class="sxs-lookup"><span data-stu-id="017d2-308">health check name (`name`).</span></span> <span data-ttu-id="017d2-309">En el caso de `null`, se utiliza `example_health_check`.</span><span class="sxs-lookup"><span data-stu-id="017d2-309">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="017d2-310">punto de datos de cadena para la comprobación de estado (`data1`).</span><span class="sxs-lookup"><span data-stu-id="017d2-310">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="017d2-311">punto de datos enteros para la comprobación de estado (`data2`).</span><span class="sxs-lookup"><span data-stu-id="017d2-311">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="017d2-312">En el caso de `null`, se utiliza `1`.</span><span class="sxs-lookup"><span data-stu-id="017d2-312">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="017d2-313">estado de error (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span><span class="sxs-lookup"><span data-stu-id="017d2-313">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="017d2-314">De manera predeterminada, es `null`.</span><span class="sxs-lookup"><span data-stu-id="017d2-314">The default is `null`.</span></span> <span data-ttu-id="017d2-315">Si `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) se notifica para un estado de error.</span><span class="sxs-lookup"><span data-stu-id="017d2-315">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="017d2-316">etiquetas (`IEnumerable<string>`).</span><span class="sxs-lookup"><span data-stu-id="017d2-316">tags (`IEnumerable<string>`).</span></span>

   ```csharp
   using System.Collections.Generic;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   public static class ExampleHealthCheckBuilderExtensions
   {
       const string NAME = "example_health_check";

       public static IHealthChecksBuilder AddExampleHealthCheck(
           this IHealthChecksBuilder builder,
           string name = default,
           string data1,
           int data2 = 1,
           HealthStatus? failureStatus = default,
           IEnumerable<string> tags = default)
       {
           return builder.Add(new HealthCheckRegistration(
               name ?? NAME,
               sp => new ExampleHealthCheck(data1, data2),
               failureStatus,
               tags));
       }
   }
   ```

## <a name="health-check-publisher"></a><span data-ttu-id="017d2-317">Publicador de la comprobación de estado</span><span class="sxs-lookup"><span data-stu-id="017d2-317">Health Check Publisher</span></span>

<span data-ttu-id="017d2-318">Cuando un elemento <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> se agrega al contenedor de servicios, el sistema de comprobación de estado ejecuta periódicamente las comprobaciones de estado y llama a `PublishAsync` con el resultado.</span><span class="sxs-lookup"><span data-stu-id="017d2-318">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="017d2-319">Esto es útil en un escenario de sistema de seguimiento de estado basado en inserción en el que se espera que cada proceso llame periódicamente al sistema de seguimiento con el fin de determinar el estado.</span><span class="sxs-lookup"><span data-stu-id="017d2-319">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="017d2-320">La interfaz de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> tiene un único método:</span><span class="sxs-lookup"><span data-stu-id="017d2-320">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="017d2-321"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> le permiten establecer:</span><span class="sxs-lookup"><span data-stu-id="017d2-321"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="017d2-322"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; El retraso inicial aplicado tras iniciarse la aplicación antes de ejecutar instancias de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="017d2-322"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="017d2-323">El retraso se aplica una vez durante el inicio y no se aplica a las iteraciones posteriores.</span><span class="sxs-lookup"><span data-stu-id="017d2-323">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="017d2-324">El valor predeterminado es cinco segundos.</span><span class="sxs-lookup"><span data-stu-id="017d2-324">The default value is five seconds.</span></span>
* <span data-ttu-id="017d2-325"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; El período de ejecución de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="017d2-325"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="017d2-326">El valor predeterminado es 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="017d2-326">The default value is 30 seconds.</span></span>
* <span data-ttu-id="017d2-327"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; Si <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> es `null` (valor predeterminado), el servicio de publicador de la comprobación de estado ejecuta todas las comprobaciones de estado registradas.</span><span class="sxs-lookup"><span data-stu-id="017d2-327"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="017d2-328">Para ejecutar un subconjunto de comprobaciones de estado, proporcione una función que filtre el conjunto de comprobaciones.</span><span class="sxs-lookup"><span data-stu-id="017d2-328">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="017d2-329">El predicado se evalúa cada período.</span><span class="sxs-lookup"><span data-stu-id="017d2-329">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="017d2-330"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; El tiempo de expiración para ejecutar las comprobaciones de estado para todas las instancias de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="017d2-330"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="017d2-331">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> para ejecutar sin tiempo de expiración.</span><span class="sxs-lookup"><span data-stu-id="017d2-331">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="017d2-332">El valor predeterminado es 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="017d2-332">The default value is 30 seconds.</span></span>

::: moniker range="= aspnetcore-2.2"

> [!WARNING]
> <span data-ttu-id="017d2-333">En la versión de ASP.NET Core 2.2, el establecimiento de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> no se asigna por medio de la implementación de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>; establece el valor de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span><span class="sxs-lookup"><span data-stu-id="017d2-333">In the ASP.NET Core 2.2 release, setting <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> isn't honored by the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation; it sets the value of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span></span> <span data-ttu-id="017d2-334">Este problema se corregirá en ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="017d2-334">This issue will be fixed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="017d2-335">Para obtener más información, consulte [HealthCheckPublisherOptions.Period establece el valor de .Delay](https://github.com/aspnet/Extensions/issues/1041).</span><span class="sxs-lookup"><span data-stu-id="017d2-335">For more information, see [HealthCheckPublisherOptions.Period sets the value of .Delay](https://github.com/aspnet/Extensions/issues/1041).</span></span>

::: moniker-end

<span data-ttu-id="017d2-336">En la aplicación de ejemplo, `ReadinessPublisher` es una implementación de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="017d2-336">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="017d2-337">El estado de comprobación de estado se registra en `Entries` y para cada comprobación:</span><span class="sxs-lookup"><span data-stu-id="017d2-337">The health check status is recorded in `Entries` and logged for each check:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=20,22-23)]

<span data-ttu-id="017d2-338">En el ejemplo `LivenessProbeStartup` de la aplicación de ejemplo, la comprobación de preparación `StartupHostedService` tiene un retraso de inicio de dos segundos y ejecuta la comprobación cada 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="017d2-338">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="017d2-339">Para activar la implementación de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>, el ejemplo registra `ReadinessPublisher` como servicio singleton en el contenedor de [inserción de dependencias (DI)](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="017d2-339">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices&highlight=12-17,28)]

::: moniker range="= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="017d2-340">La siguiente solución alternativa permite la adición de una instancia de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> al contenedor del servicio cuando uno o más servicios hospedados ya se han agregado a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="017d2-340">The following workaround permits adding an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instance to the service container when one or more other hosted services have already been added to the app.</span></span> <span data-ttu-id="017d2-341">Esta solución alternativa no se requerirá con el lanzamiento de ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="017d2-341">This workaround won't be required with the release of ASP.NET Core 3.0.</span></span> <span data-ttu-id="017d2-342">Para obtener más información, consulte: https://github.com/aspnet/Extensions/issues/639.</span><span class="sxs-lookup"><span data-stu-id="017d2-342">For more information, see: https://github.com/aspnet/Extensions/issues/639.</span></span>
>
> ```csharp
> private const string HealthCheckServiceAssembly =
>     "Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherHostedService";
>
> services.TryAddEnumerable(
>     ServiceDescriptor.Singleton(typeof(IHostedService),
>         typeof(HealthCheckPublisherOptions).Assembly
>             .GetType(HealthCheckServiceAssembly)));
> ```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="017d2-343">[BeatPulse](https://github.com/Xabaril/BeatPulse) incluye editores para varios sistemas, incluido [Application Insights](/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="017d2-343">[BeatPulse](https://github.com/Xabaril/BeatPulse) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="017d2-344">Microsoft no se encarga del mantenimiento de [BeatPulse](https://github.com/Xabaril/BeatPulse) ni ofrece soporte técnico.</span><span class="sxs-lookup"><span data-stu-id="017d2-344">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>
