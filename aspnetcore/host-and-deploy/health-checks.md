---
title: Comprobaciones de estado en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo configurar las comprobaciones de estado para la infraestructura de ASP.NET Core, como las aplicaciones y las bases de datos.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 12/15/2019
uid: host-and-deploy/health-checks
ms.openlocfilehash: 314e55c818cddf1dad2e3ec74d4d1e041ce7366f
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "78649691"
---
# <a name="health-checks-in-aspnet-core"></a><span data-ttu-id="214b1-103">Comprobaciones de estado en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="214b1-103">Health checks in ASP.NET Core</span></span>

<span data-ttu-id="214b1-104">Por [Glenn Condron](https://github.com/glennc)</span><span class="sxs-lookup"><span data-stu-id="214b1-104">By [Glenn Condron](https://github.com/glennc)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="214b1-105">ASP.NET Core ofrece el middleware de comprobaciones de estado y bibliotecas para informar sobre el estado de los componentes de la infraestructura de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="214b1-105">ASP.NET Core offers Health Checks Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="214b1-106">Una aplicación se encarga de exponer las comprobaciones de estado como puntos de conexión HTTP.</span><span class="sxs-lookup"><span data-stu-id="214b1-106">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="214b1-107">Los puntos de conexión de las comprobaciones de estado pueden configurarse para diversos escenarios de supervisión en tiempo real:</span><span class="sxs-lookup"><span data-stu-id="214b1-107">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="214b1-108">Los orquestadores de contenedores y los equilibradores de carga pueden utilizar los sondeos de estado para comprobar el estado de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="214b1-108">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="214b1-109">Por ejemplo, para responder a una comprobación de estado con errores, es posible que un orquestador de contenedores detenga una implementación en curso o reinicie un contenedor.</span><span class="sxs-lookup"><span data-stu-id="214b1-109">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="214b1-110">Como respuesta a una aplicación con estado incorrecto, es posible que un equilibrador de carga enrute el tráfico al margen de la instancia con errores hacia una instancia con estado correcto.</span><span class="sxs-lookup"><span data-stu-id="214b1-110">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="214b1-111">El uso de la memoria, el disco y otros recursos del servidor físico puede supervisarse para determinar si el estado es correcto.</span><span class="sxs-lookup"><span data-stu-id="214b1-111">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="214b1-112">Las comprobaciones de estado pueden probar las dependencias de una aplicación, como las bases de datos y los puntos de conexión de servicio externo, para confirmar la disponibilidad y el funcionamiento normal.</span><span class="sxs-lookup"><span data-stu-id="214b1-112">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="214b1-113">[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="214b1-113">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="214b1-114">La aplicación de muestra incluye ejemplos de los escenarios descritos en este tema.</span><span class="sxs-lookup"><span data-stu-id="214b1-114">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="214b1-115">Para ejecutar la aplicación de ejemplo para un escenario determinado, use el comando [dotnet run](/dotnet/core/tools/dotnet-run) desde la carpeta del proyecto en un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="214b1-115">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="214b1-116">Para obtener información sobre cómo utilizar la aplicación de ejemplo, consulte el archivo *README.md* de la aplicación de ejemplo y las descripciones de escenarios de este tema.</span><span class="sxs-lookup"><span data-stu-id="214b1-116">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="214b1-117">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="214b1-117">Prerequisites</span></span>

<span data-ttu-id="214b1-118">Normalmente, las comprobaciones de estado se usan con un servicio de supervisión externa o un orquestador de contenedores para comprobar el estado de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="214b1-118">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="214b1-119">Antes de agregar comprobaciones de estado a una aplicación, debe decidir en qué sistema de supervisión se va a usar.</span><span class="sxs-lookup"><span data-stu-id="214b1-119">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="214b1-120">El sistema de supervisión determina qué tipos de comprobaciones de estado se deben crear y cómo configurar sus puntos de conexión.</span><span class="sxs-lookup"><span data-stu-id="214b1-120">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="214b1-121">Se hace referencia implícitamente al paquete [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) para las aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="214b1-121">The [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package is referenced implicitly for ASP.NET Core apps.</span></span> <span data-ttu-id="214b1-122">Para realizar comprobaciones de estado mediante Entity Framework Core, agregue una referencia al paquete [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="214b1-122">To perform health checks using Entity Framework Core, add a package reference to the [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore) package.</span></span>

<span data-ttu-id="214b1-123">La aplicación de ejemplo proporciona código de inicio para mostrar las comprobaciones de estado para varios escenarios.</span><span class="sxs-lookup"><span data-stu-id="214b1-123">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="214b1-124">En el escenario de [sondeo de base de datos](#database-probe) se comprueba el estado de una conexión de base de datos mediante [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span><span class="sxs-lookup"><span data-stu-id="214b1-124">The [database probe](#database-probe) scenario checks the health of a database connection using [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span></span> <span data-ttu-id="214b1-125">El escenario [sondeo de DbContext](#entity-framework-core-dbcontext-probe) comprueba una base de datos mediante un elemento `DbContext` de EF Core.</span><span class="sxs-lookup"><span data-stu-id="214b1-125">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="214b1-126">Para explorar los escenarios de la base de datos, la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="214b1-126">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="214b1-127">Crea una base de datos y proporciona su cadena de conexión en el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="214b1-127">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="214b1-128">Tiene las siguientes referencias de paquete en su archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="214b1-128">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="214b1-129">AspNetCore.HealthChecks.SqlServer</span><span class="sxs-lookup"><span data-stu-id="214b1-129">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="214b1-130">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="214b1-130">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="214b1-131">Microsoft no ofrece soporte técnico para [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) ni lo mantiene.</span><span class="sxs-lookup"><span data-stu-id="214b1-131">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="214b1-132">Otro escenario de comprobación de estado muestra cómo filtrar las comprobaciones de estado por un puerto de administración.</span><span class="sxs-lookup"><span data-stu-id="214b1-132">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="214b1-133">La aplicación de ejemplo requiere la creación de un archivo *Properties/launchSettings.json* que incluya la dirección URL de administración y el puerto de administración.</span><span class="sxs-lookup"><span data-stu-id="214b1-133">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="214b1-134">Para obtener más información, consulte la sección [Filtrado por puerto](#filter-by-port).</span><span class="sxs-lookup"><span data-stu-id="214b1-134">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="214b1-135">Sondeo de estado básico</span><span class="sxs-lookup"><span data-stu-id="214b1-135">Basic health probe</span></span>

<span data-ttu-id="214b1-136">Para muchas aplicaciones, una configuración de sondeo de estado básico que notifique la disponibilidad de la aplicación para procesar las solicitudes (*ejecución*) es suficiente para detectar el estado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="214b1-136">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="214b1-137">La configuración básica registra los servicios de comprobación de estado y llama al middleware de comprobaciones de estado para responder a un punto de conexión de dirección URL con una respuesta de estado.</span><span class="sxs-lookup"><span data-stu-id="214b1-137">The basic configuration registers health check services and calls the Health Checks Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="214b1-138">De forma predeterminada, no se registran comprobaciones de estado específicas para probar cualquier dependencia o subsistema concretos.</span><span class="sxs-lookup"><span data-stu-id="214b1-138">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="214b1-139">La aplicación se considera correcta si es capaz de responder en la dirección URL de punto de conexión de estado.</span><span class="sxs-lookup"><span data-stu-id="214b1-139">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="214b1-140">El escritor de respuesta predeterminado escribe el estado (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) como respuesta de texto no cifrado al cliente, que indica un estado [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) o [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus).</span><span class="sxs-lookup"><span data-stu-id="214b1-140">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="214b1-141">Registre los servicios de comprobación de estado con <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> de `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="214b1-141">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="214b1-142">Cree un punto de conexión de comprobación de estado llamando a `MapHealthChecks` en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="214b1-142">Create a health check endpoint by calling `MapHealthChecks` in `Startup.Configure`.</span></span>

<span data-ttu-id="214b1-143">En la aplicación de ejemplo, el punto de conexión de la comprobación de estado se crea en `/health` (*BasicStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="214b1-143">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

```csharp
public class BasicStartup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddHealthChecks();
    }

    public void Configure(IApplicationBuilder app)
    {
        app.UseRouting();

        app.UseEndpoints(endpoints =>
        {
            endpoints.MapHealthChecks("/health");
        });
    }
}
```

<span data-ttu-id="214b1-144">Para ejecutar el escenario de configuración básica mediante la aplicación de ejemplo, ejecute el comando siguiente desde la carpeta del proyecto en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="214b1-144">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="214b1-145">Ejemplo de Docker</span><span class="sxs-lookup"><span data-stu-id="214b1-145">Docker example</span></span>

<span data-ttu-id="214b1-146">[Docker](xref:host-and-deploy/docker/index) ofrece una directiva de `HEALTHCHECK` integrada que puede utilizarse para comprobar el estado de una aplicación que use la configuración de comprobación de estado básica:</span><span class="sxs-lookup"><span data-stu-id="214b1-146">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="214b1-147">Creación de comprobaciones de estado</span><span class="sxs-lookup"><span data-stu-id="214b1-147">Create health checks</span></span>

<span data-ttu-id="214b1-148">Las comprobaciones de estado se crean mediante la implementación de la interfaz de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck>.</span><span class="sxs-lookup"><span data-stu-id="214b1-148">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="214b1-149">El método <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> devuelve un elemento <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> que indica el estado como `Healthy`, `Degraded` o `Unhealthy`.</span><span class="sxs-lookup"><span data-stu-id="214b1-149">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="214b1-150">El resultado se escribe como una respuesta de texto no cifrado con un código de estado configurable. (La configuración se describe en la sección [Opciones de comprobación de estado](#health-check-options)).</span><span class="sxs-lookup"><span data-stu-id="214b1-150">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="214b1-151"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> también puede devolver pares clave-valor opcionales.</span><span class="sxs-lookup"><span data-stu-id="214b1-151"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

<span data-ttu-id="214b1-152">La siguiente clase `ExampleHealthCheck` muestra el diseño de una comprobación de estado.</span><span class="sxs-lookup"><span data-stu-id="214b1-152">The following `ExampleHealthCheck` class demonstrates the layout of a health check.</span></span> <span data-ttu-id="214b1-153">La lógica de comprobaciones de estado se coloca en el método `CheckHealthAsync`.</span><span class="sxs-lookup"><span data-stu-id="214b1-153">The health checks logic is placed in the `CheckHealthAsync` method.</span></span> <span data-ttu-id="214b1-154">En el ejemplo siguiente se establece una variable ficticia, `healthCheckResultHealthy`, en `true`.</span><span class="sxs-lookup"><span data-stu-id="214b1-154">The following example sets a dummy variable, `healthCheckResultHealthy`, to `true`.</span></span> <span data-ttu-id="214b1-155">Si el valor de `healthCheckResultHealthy` se establece en `false`, se devuelve el estado [HealthCheckResult.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*).</span><span class="sxs-lookup"><span data-stu-id="214b1-155">If the value of `healthCheckResultHealthy` is set to `false`, the [HealthCheckResult.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) status is returned.</span></span>

```csharp
public class ExampleHealthCheck : IHealthCheck
{
    public Task<HealthCheckResult> CheckHealthAsync(
        HealthCheckContext context,
        CancellationToken cancellationToken = default(CancellationToken))
    {
        var healthCheckResultHealthy = true;

        if (healthCheckResultHealthy)
        {
            return Task.FromResult(
                HealthCheckResult.Healthy("A healthy result."));
        }

        return Task.FromResult(
            HealthCheckResult.Unhealthy("An unhealthy result."));
    }
}
```

## <a name="register-health-check-services"></a><span data-ttu-id="214b1-156">Registro de los servicios de comprobación de estado</span><span class="sxs-lookup"><span data-stu-id="214b1-156">Register health check services</span></span>

<span data-ttu-id="214b1-157">El tipo `ExampleHealthCheck` se agrega a los servicios de comprobación de estado con <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="214b1-157">The `ExampleHealthCheck` type is added to health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

<span data-ttu-id="214b1-158">La sobrecarga <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> que se muestra en el ejemplo siguiente establece el estado de error (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) para notificar cuándo la comprobación de estado informa de un error.</span><span class="sxs-lookup"><span data-stu-id="214b1-158">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="214b1-159">Si el estado de error se establece en `null` (valor predeterminado), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) se notifica.</span><span class="sxs-lookup"><span data-stu-id="214b1-159">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="214b1-160">Esta sobrecarga es un escenario útil para los creadores de bibliotecas en los que la aplicación ejecuta el estado de error indicado por la biblioteca cuando se produce un error de comprobación de estado si la implementación de la comprobación de estado respeta la configuración.</span><span class="sxs-lookup"><span data-stu-id="214b1-160">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="214b1-161">Las *etiquetas* pueden usarse para filtrar las comprobaciones de estado, que se describen con más detalle en la sección [Filtrado de las comprobaciones de estado](#filter-health-checks).</span><span class="sxs-lookup"><span data-stu-id="214b1-161">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<span data-ttu-id="214b1-162"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> también puede ejecutar una función lambda.</span><span class="sxs-lookup"><span data-stu-id="214b1-162"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="214b1-163">En el ejemplo siguiente, el nombre de la comprobación de estado se especifica como `Example` y la comprobación siempre devuelve un estado correcto:</span><span class="sxs-lookup"><span data-stu-id="214b1-163">In the following example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Example", () =>
        HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" });
```

<span data-ttu-id="214b1-164">Llame a <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddTypeActivatedCheck*> para pasar argumentos a una implementación de comprobación de estado.</span><span class="sxs-lookup"><span data-stu-id="214b1-164">Call <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddTypeActivatedCheck*> to pass arguments to a health check implementation.</span></span> <span data-ttu-id="214b1-165">En el ejemplo siguiente, `TestHealthCheckWithArgs` acepta un entero y una cadena que se van a usar cuando se llame a <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*>:</span><span class="sxs-lookup"><span data-stu-id="214b1-165">In the following example, `TestHealthCheckWithArgs` accepts an integer and a string for use when <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> is called:</span></span>

```csharp
private class TestHealthCheckWithArgs : IHealthCheck
{
    public TestHealthCheckWithArgs(int i, string s)
    {
        I = i;
        S = s;
    }

    public int I { get; set; }

    public string S { get; set; }

    public Task<HealthCheckResult> CheckHealthAsync(HealthCheckContext context, 
        CancellationToken cancellationToken = default)
    {
        ...
    }
}
```

<span data-ttu-id="214b1-166">`TestHealthCheckWithArgs` se registra mediante un llamado a `AddTypeActivatedCheck` con el entero y la cadena que se pasaron a la implementación:</span><span class="sxs-lookup"><span data-stu-id="214b1-166">`TestHealthCheckWithArgs` is registered by calling `AddTypeActivatedCheck` with the integer and string passed to the implementation:</span></span>

```csharp
services.AddHealthChecks()
    .AddTypeActivatedCheck<TestHealthCheckWithArgs>(
        "test", 
        failureStatus: HealthStatus.Degraded, 
        tags: new[] { "example" }, 
        args: new object[] { 5, "string" });
```

## <a name="use-health-checks-routing"></a><span data-ttu-id="214b1-167">Uso del enrutamiento de comprobaciones de estado</span><span class="sxs-lookup"><span data-stu-id="214b1-167">Use Health Checks Routing</span></span>

<span data-ttu-id="214b1-168">En `Startup.Configure`, llame a `MapHealthChecks` en el generador de puntos de conexiones con la dirección URL del punto de conexión o la ruta de acceso relativa:</span><span class="sxs-lookup"><span data-stu-id="214b1-168">In `Startup.Configure`, call `MapHealthChecks` on the endpoint builder with the endpoint URL or relative path:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
});
```

### <a name="require-host"></a><span data-ttu-id="214b1-169">Requerimiento de host</span><span class="sxs-lookup"><span data-stu-id="214b1-169">Require host</span></span>

<span data-ttu-id="214b1-170">Llame `RequireHost` a para especificar uno o más hosts permitidos para el punto de conexión de comprobación de estado.</span><span class="sxs-lookup"><span data-stu-id="214b1-170">Call `RequireHost` to specify one or more permitted hosts for the health check endpoint.</span></span> <span data-ttu-id="214b1-171">Los hosts deben ser Unicode en lugar de Punycode y pueden incluir un puerto.</span><span class="sxs-lookup"><span data-stu-id="214b1-171">Hosts should be Unicode rather than punycode and may include a port.</span></span> <span data-ttu-id="214b1-172">Si no se proporciona una colección, se acepta cualquier host.</span><span class="sxs-lookup"><span data-stu-id="214b1-172">If a collection isn't supplied, any host is accepted.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireHost("www.contoso.com:5001");
});
```

<span data-ttu-id="214b1-173">Para obtener más información, consulte la sección [Filtrado por puerto](#filter-by-port).</span><span class="sxs-lookup"><span data-stu-id="214b1-173">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

### <a name="require-authorization"></a><span data-ttu-id="214b1-174">Requerimiento de autorización</span><span class="sxs-lookup"><span data-stu-id="214b1-174">Require authorization</span></span>

<span data-ttu-id="214b1-175">Llame a `RequireAuthorization` para ejecutar el middleware de autorización en el punto de conexión de solicitudes de comprobación de estado.</span><span class="sxs-lookup"><span data-stu-id="214b1-175">Call `RequireAuthorization` to run Authorization Middleware on the health check request endpoint.</span></span> <span data-ttu-id="214b1-176">Una sobrecarga `RequireAuthorization` acepta una o varias directivas de autorización.</span><span class="sxs-lookup"><span data-stu-id="214b1-176">A `RequireAuthorization` overload accepts one or more authorization policies.</span></span> <span data-ttu-id="214b1-177">Si no se proporciona una directiva, se usa la directiva de autorización predeterminada.</span><span class="sxs-lookup"><span data-stu-id="214b1-177">If a policy isn't provided, the default authorization policy is used.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireAuthorization();
});
```

### <a name="enable-cross-origin-requests-cors"></a><span data-ttu-id="214b1-178">Habilitar solicitudes entre orígenes (CORS)</span><span class="sxs-lookup"><span data-stu-id="214b1-178">Enable Cross-Origin Requests (CORS)</span></span>

<span data-ttu-id="214b1-179">Aunque no se suelen realizar comprobaciones de estado manualmente desde un explorador, el middleware de CORS se puede habilitar mediante una llamada a `RequireCors` en los puntos de conexión de las comprobaciones de estado.</span><span class="sxs-lookup"><span data-stu-id="214b1-179">Although performing health checks manually from a browser isn't a common use scenario, CORS Middleware can be enabled by calling `RequireCors` on health checks endpoints.</span></span> <span data-ttu-id="214b1-180">Una sobrecarga `RequireCors` acepta un delegado del generador de directivas CORS (`CorsPolicyBuilder`) o un nombre de directiva.</span><span class="sxs-lookup"><span data-stu-id="214b1-180">A `RequireCors` overload accepts a CORS policy builder delegate (`CorsPolicyBuilder`) or a policy name.</span></span> <span data-ttu-id="214b1-181">Si no se proporciona una directiva, se usa la directiva CORS predeterminada.</span><span class="sxs-lookup"><span data-stu-id="214b1-181">If a policy isn't provided, the default CORS policy is used.</span></span> <span data-ttu-id="214b1-182">Para obtener más información, vea <xref:security/cors>.</span><span class="sxs-lookup"><span data-stu-id="214b1-182">For more information, see <xref:security/cors>.</span></span>

## <a name="health-check-options"></a><span data-ttu-id="214b1-183">Opciones de comprobación de estado</span><span class="sxs-lookup"><span data-stu-id="214b1-183">Health check options</span></span>

<span data-ttu-id="214b1-184">El elemento <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> ofrece una oportunidad para personalizar el comportamiento de las comprobaciones de estado:</span><span class="sxs-lookup"><span data-stu-id="214b1-184"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="214b1-185">Filtrado de las comprobaciones de estado</span><span class="sxs-lookup"><span data-stu-id="214b1-185">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="214b1-186">Personalización del código de estado HTTP</span><span class="sxs-lookup"><span data-stu-id="214b1-186">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="214b1-187">Supresión de los encabezados de caché</span><span class="sxs-lookup"><span data-stu-id="214b1-187">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="214b1-188">Personalización del resultado</span><span class="sxs-lookup"><span data-stu-id="214b1-188">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="214b1-189">Filtrado de las comprobaciones de estado</span><span class="sxs-lookup"><span data-stu-id="214b1-189">Filter health checks</span></span>

<span data-ttu-id="214b1-190">De forma predeterminada, el middleware de comprobaciones de estado ejecuta todas las comprobaciones de estado registradas.</span><span class="sxs-lookup"><span data-stu-id="214b1-190">By default, Health Checks Middleware runs all registered health checks.</span></span> <span data-ttu-id="214b1-191">Para ejecutar un subconjunto de comprobaciones de estado, proporcione una función que devuelva un valor booleano para la opción <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate>.</span><span class="sxs-lookup"><span data-stu-id="214b1-191">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="214b1-192">En el ejemplo siguiente, la etiqueta (`bar_tag`) de la comprobación de estado `Bar` la filtra, en la instrucción condicional de la función, donde `true` solo se devuelve si la propiedad <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> de la comprobación de estado coincide con `foo_tag` o `baz_tag`:</span><span class="sxs-lookup"><span data-stu-id="214b1-192">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

<span data-ttu-id="214b1-193">En `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="214b1-193">In `Startup.ConfigureServices`:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Foo", () =>
        HealthCheckResult.Healthy("Foo is OK!"), tags: new[] { "foo_tag" })
    .AddCheck("Bar", () =>
        HealthCheckResult.Unhealthy("Bar is unhealthy!"), tags: new[] { "bar_tag" })
    .AddCheck("Baz", () =>
        HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
```

<span data-ttu-id="214b1-194">En `Startup.Configure`, el `Predicate` filtra la comprobación de estado "Bar".</span><span class="sxs-lookup"><span data-stu-id="214b1-194">In `Startup.Configure`, the `Predicate` filters out the 'Bar' health check.</span></span> <span data-ttu-id="214b1-195">Solo se ejecutan Foo y Baz:</span><span class="sxs-lookup"><span data-stu-id="214b1-195">Only Foo and Baz execute.:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        Predicate = (check) => check.Tags.Contains("foo_tag") ||
            check.Tags.Contains("baz_tag")
    });
});
```

### <a name="customize-the-http-status-code"></a><span data-ttu-id="214b1-196">Personalización del código de estado HTTP</span><span class="sxs-lookup"><span data-stu-id="214b1-196">Customize the HTTP status code</span></span>

<span data-ttu-id="214b1-197">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> para personalizar la asignación del estado de mantenimiento de los códigos de estado HTTP.</span><span class="sxs-lookup"><span data-stu-id="214b1-197">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="214b1-198">Las siguientes asignaciones de <xref:Microsoft.AspNetCore.Http.StatusCodes> son los valores predeterminados que el middleware utiliza.</span><span class="sxs-lookup"><span data-stu-id="214b1-198">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="214b1-199">Cambie los valores de código de estado para satisfacer sus necesidades.</span><span class="sxs-lookup"><span data-stu-id="214b1-199">Change the status code values to meet your requirements.</span></span>

<span data-ttu-id="214b1-200">En `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="214b1-200">In `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResultStatusCodes =
        {
            [HealthStatus.Healthy] = StatusCodes.Status200OK,
            [HealthStatus.Degraded] = StatusCodes.Status200OK,
            [HealthStatus.Unhealthy] = StatusCodes.Status503ServiceUnavailable
        }
    });
});
```

### <a name="suppress-cache-headers"></a><span data-ttu-id="214b1-201">Supresión de los encabezados de caché</span><span class="sxs-lookup"><span data-stu-id="214b1-201">Suppress cache headers</span></span>

<span data-ttu-id="214b1-202"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controla si el middleware de comprobaciones de estado agrega encabezados HTTP a una respuesta de sondeo para evitar el almacenamiento en caché de respuesta.</span><span class="sxs-lookup"><span data-stu-id="214b1-202"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Checks Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="214b1-203">Si el valor es `false` (valor predeterminado), el middleware establece o invalida los encabezados `Cache-Control`, `Expires` y `Pragma` para evitar el almacenamiento en caché de respuesta.</span><span class="sxs-lookup"><span data-stu-id="214b1-203">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="214b1-204">Si el valor es `true`, el middleware no modifica los encabezados de caché de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="214b1-204">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

<span data-ttu-id="214b1-205">En `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="214b1-205">In `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        AllowCachingResponses = false
    });
});
```

### <a name="customize-output"></a><span data-ttu-id="214b1-206">Personalización del resultado</span><span class="sxs-lookup"><span data-stu-id="214b1-206">Customize output</span></span>

<span data-ttu-id="214b1-207">En `Startup.Configure`, establezca la opción [HealthCheckOptions.ResponseWriter](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter) en un delegado para escribir la respuesta:</span><span class="sxs-lookup"><span data-stu-id="214b1-207">In `Startup.Configure`, set the [HealthCheckOptions.ResponseWriter](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter) option to a delegate for writing the response:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
});
```

<span data-ttu-id="214b1-208">El delegado predeterminado escribe una respuesta de texto no cifrado mínima con el valor de cadena [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span><span class="sxs-lookup"><span data-stu-id="214b1-208">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span> <span data-ttu-id="214b1-209">Los siguientes delegados personalizados generan una respuesta JSON personalizada.</span><span class="sxs-lookup"><span data-stu-id="214b1-209">The following custom delegates output a custom JSON response.</span></span>

<span data-ttu-id="214b1-210">En el primer ejemplo de la aplicación de ejemplo se muestra cómo usar <xref:System.Text.Json?displayProperty=fullName>:</span><span class="sxs-lookup"><span data-stu-id="214b1-210">The first example from the sample app demonstrates how to use <xref:System.Text.Json?displayProperty=fullName>:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse_SystemTextJson)]

<span data-ttu-id="214b1-211">En el segundo ejemplo se muestra cómo usar [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/):</span><span class="sxs-lookup"><span data-stu-id="214b1-211">The second example demonstrates how to use [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse_NewtonSoftJson)]

<span data-ttu-id="214b1-212">En la aplicación de ejemplo, convierta en comentario la [directiva de preprocesador](xref:index#preprocessor-directives-in-sample-code) `SYSTEM_TEXT_JSON` en *CustomWriterStartup.cs* para habilitar la versión `Newtonsoft.Json` de `WriteResponse`.</span><span class="sxs-lookup"><span data-stu-id="214b1-212">In the sample app, comment out the `SYSTEM_TEXT_JSON` [preprocessor directive](xref:index#preprocessor-directives-in-sample-code) in *CustomWriterStartup.cs* to enable the `Newtonsoft.Json` version of `WriteResponse`.</span></span>

<span data-ttu-id="214b1-213">La API de comprobaciones de estado no ofrece compatibilidad integrada con formatos de retorno JSON complejos porque el formato es específico del sistema de supervisión elegido.</span><span class="sxs-lookup"><span data-stu-id="214b1-213">The health checks API doesn't provide built-in support for complex JSON return formats because the format is specific to your choice of monitoring system.</span></span> <span data-ttu-id="214b1-214">Personalice la respuesta en los ejemplos anteriores según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="214b1-214">Customize the response in the preceding examples as needed.</span></span> <span data-ttu-id="214b1-215">Para más información sobre la serialización de JSON con `System.Text.Json`, consulte [Procedimiento para serializar y deserializar JSON en .NET](/dotnet/standard/serialization/system-text-json-how-to).</span><span class="sxs-lookup"><span data-stu-id="214b1-215">For more information on JSON serialization with `System.Text.Json`, see [How to serialize and deserialize JSON in .NET](/dotnet/standard/serialization/system-text-json-how-to).</span></span>

## <a name="database-probe"></a><span data-ttu-id="214b1-216">Sondeo de bases de datos</span><span class="sxs-lookup"><span data-stu-id="214b1-216">Database probe</span></span>

<span data-ttu-id="214b1-217">Una comprobación de estado puede especificar que una consulta de base de datos se ejecute como una prueba booleana para indicar si esta responde con normalidad.</span><span class="sxs-lookup"><span data-stu-id="214b1-217">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="214b1-218">En la aplicación de ejemplo se usa [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), una biblioteca de comprobaciones de estado para las aplicaciones ASP.NET Core, que permite realizar una comprobación de estado en una base de datos de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="214b1-218">The sample app uses [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="214b1-219">`AspNetCore.Diagnostics.HealthChecks` ejecuta una consulta `SELECT 1` en la base de datos para confirmar que la conexión a la base de datos es correcta.</span><span class="sxs-lookup"><span data-stu-id="214b1-219">`AspNetCore.Diagnostics.HealthChecks` executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="214b1-220">Al comprobar la conexión de una base de datos a una consulta, elija una consulta que se devuelva rápidamente.</span><span class="sxs-lookup"><span data-stu-id="214b1-220">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="214b1-221">El enfoque de la consulta plantea el riesgo de sobrecargar la base de datos y degradar el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="214b1-221">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="214b1-222">En la mayoría de los casos, no es necesario ejecutar una consulta de prueba.</span><span class="sxs-lookup"><span data-stu-id="214b1-222">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="214b1-223">Simplemente, realizar una conexión correcta a la base de datos es suficiente.</span><span class="sxs-lookup"><span data-stu-id="214b1-223">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="214b1-224">Si resulta necesario ejecutar una consulta, elija una consulta SELECT sencilla, como `SELECT 1`.</span><span class="sxs-lookup"><span data-stu-id="214b1-224">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="214b1-225">Incluya una referencia de paquete a [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="214b1-225">Include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="214b1-226">Proporcione una cadena de conexión a base de datos válida en el archivo *appsettings.json* de la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="214b1-226">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="214b1-227">La aplicación usa una base de datos de SQL Server denominada `HealthCheckSample`:</span><span class="sxs-lookup"><span data-stu-id="214b1-227">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/3.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="214b1-228">Registre los servicios de comprobación de estado con <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> de `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="214b1-228">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="214b1-229">La aplicación de ejemplo llama al método `AddSqlServer` con la cadena de conexión de la base de datos (*DbHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="214b1-229">The sample app calls the `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="214b1-230">Se crea un punto de conexión de comprobación de estado llamando a `MapHealthChecks` en `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="214b1-230">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

<span data-ttu-id="214b1-231">Para ejecutar el escenario de sondeo de base de datos mediante la aplicación de ejemplo, ejecute el comando siguiente desde la carpeta del proyecto en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="214b1-231">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="214b1-232">Microsoft no ofrece soporte técnico para [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) ni lo mantiene.</span><span class="sxs-lookup"><span data-stu-id="214b1-232">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="214b1-233">Sondeo de DbContext de Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="214b1-233">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="214b1-234">La comprobación `DbContext` confirma que la aplicación puede comunicarse con la base de datos configurada para un elemento `DbContext` de EF Core.</span><span class="sxs-lookup"><span data-stu-id="214b1-234">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="214b1-235">La comprobación `DbContext` se admite en las aplicaciones que:</span><span class="sxs-lookup"><span data-stu-id="214b1-235">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="214b1-236">Usan [Entity Framework (EF) Core](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="214b1-236">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="214b1-237">Incluyen una referencia de paquete a [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span><span class="sxs-lookup"><span data-stu-id="214b1-237">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="214b1-238">`AddDbContextCheck<TContext>` registra una comprobación de estado para un elemento `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="214b1-238">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="214b1-239">El elemento `DbContext` se proporciona como `TContext` en el método.</span><span class="sxs-lookup"><span data-stu-id="214b1-239">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="214b1-240">Hay disponible una sobrecarga para configurar el estado de error, las etiquetas y una consulta de prueba personalizada.</span><span class="sxs-lookup"><span data-stu-id="214b1-240">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="214b1-241">De manera predeterminada:</span><span class="sxs-lookup"><span data-stu-id="214b1-241">By default:</span></span>

* <span data-ttu-id="214b1-242">`DbContextHealthCheck` llama al método `CanConnectAsync` de EF Core.</span><span class="sxs-lookup"><span data-stu-id="214b1-242">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="214b1-243">Se puede personalizar qué operación se ejecuta al comprobar el estado con sobrecargas del método `AddDbContextCheck`.</span><span class="sxs-lookup"><span data-stu-id="214b1-243">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="214b1-244">El nombre de la comprobación de estado es el nombre del tipo `TContext`.</span><span class="sxs-lookup"><span data-stu-id="214b1-244">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="214b1-245">En la aplicación de ejemplo, `AppDbContext` se proporciona para `AddDbContextCheck` y se registra como un servicio en `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="214b1-245">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="214b1-246">Se crea un punto de conexión de comprobación de estado llamando a `MapHealthChecks` en `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="214b1-246">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

<span data-ttu-id="214b1-247">Para ejecutar el escenario de sondeo de `DbContext` mediante la aplicación de ejemplo, confirme que la base de datos que la cadena de conexión especifica no exista en la instancia de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="214b1-247">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="214b1-248">Si la base de datos existe, elimínela.</span><span class="sxs-lookup"><span data-stu-id="214b1-248">If the database exists, delete it.</span></span>

<span data-ttu-id="214b1-249">Ejecute el comando siguiente desde la carpeta del proyecto en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="214b1-249">Execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario dbcontext
```

<span data-ttu-id="214b1-250">Cuando la aplicación ya se esté ejecutando, compruebe el estado de mantenimiento mediante una solicitud al punto de conexión `/health` en un explorador.</span><span class="sxs-lookup"><span data-stu-id="214b1-250">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="214b1-251">La base de datos y `AppDbContext` no existen, por lo que la aplicación proporciona la respuesta siguiente:</span><span class="sxs-lookup"><span data-stu-id="214b1-251">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="214b1-252">Active la aplicación de ejemplo para crear la base de datos.</span><span class="sxs-lookup"><span data-stu-id="214b1-252">Trigger the sample app to create the database.</span></span> <span data-ttu-id="214b1-253">Realice una solicitud a `/createdatabase`.</span><span class="sxs-lookup"><span data-stu-id="214b1-253">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="214b1-254">La aplicación responde:</span><span class="sxs-lookup"><span data-stu-id="214b1-254">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="214b1-255">Realice una solicitud al punto de conexión `/health`.</span><span class="sxs-lookup"><span data-stu-id="214b1-255">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="214b1-256">La base de datos y el contexto existen, por lo que la aplicación responde:</span><span class="sxs-lookup"><span data-stu-id="214b1-256">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="214b1-257">Active la aplicación de ejemplo para eliminar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="214b1-257">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="214b1-258">Realice una solicitud a `/deletedatabase`.</span><span class="sxs-lookup"><span data-stu-id="214b1-258">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="214b1-259">La aplicación responde:</span><span class="sxs-lookup"><span data-stu-id="214b1-259">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="214b1-260">Realice una solicitud al punto de conexión `/health`.</span><span class="sxs-lookup"><span data-stu-id="214b1-260">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="214b1-261">La aplicación proporciona una respuesta incorrecta:</span><span class="sxs-lookup"><span data-stu-id="214b1-261">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="214b1-262">Sondeos de preparación y ejecución independientes</span><span class="sxs-lookup"><span data-stu-id="214b1-262">Separate readiness and liveness probes</span></span>

<span data-ttu-id="214b1-263">En algunos escenarios de hospedaje, se usa un par de comprobaciones de estado que distinguen los dos estados de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="214b1-263">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="214b1-264">La aplicación está funcionando, pero aún no está preparada para recibir solicitudes.</span><span class="sxs-lookup"><span data-stu-id="214b1-264">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="214b1-265">Este estado es la *preparación* de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="214b1-265">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="214b1-266">La aplicación está funcionando y responde a las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="214b1-266">The app is functioning and responding to requests.</span></span> <span data-ttu-id="214b1-267">Este estado es la *ejecución* de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="214b1-267">This state is the app's *liveness*.</span></span>

<span data-ttu-id="214b1-268">Normalmente, la comprobación de preparación realiza un conjunto más amplio de comprobaciones, que requieren mucho tiempo, para determinar si están disponibles todos los subsistemas y los recursos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="214b1-268">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="214b1-269">Una comprobación de ejecución simplemente realiza una comprobación rápida para determinar si la aplicación está disponible para procesar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="214b1-269">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="214b1-270">Después de que la aplicación pase la comprobación de preparación, no es necesario sobrecargar aún más la aplicación con el conjunto amplio de comprobaciones de preparación; en las comprobaciones siguientes solo se requiere comprobar la ejecución.</span><span class="sxs-lookup"><span data-stu-id="214b1-270">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="214b1-271">La aplicación de ejemplo contiene una comprobación de estado para notificar la finalización de la tarea de inicio de ejecución prolongada en un [servicio hospedado](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="214b1-271">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="214b1-272">El elemento `StartupHostedServiceHealthCheck` expone una propiedad, `StartupTaskCompleted`, que el servicio hospedado puede establecer en `true` al terminar su tarea de ejecución prolongada (*StartupHostedServiceHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="214b1-272">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

<span data-ttu-id="214b1-273">Un [servicio hospedado](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*) se encarga de iniciar la tarea en segundo plano de larga ejecución.</span><span class="sxs-lookup"><span data-stu-id="214b1-273">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="214b1-274">Al finalizar la tarea, `StartupHostedServiceHealthCheck.StartupTaskCompleted` se establece en `true`:</span><span class="sxs-lookup"><span data-stu-id="214b1-274">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="214b1-275">La comprobación de estado se registra con <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> en `Startup.ConfigureServices` junto con el servicio hospedado.</span><span class="sxs-lookup"><span data-stu-id="214b1-275">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="214b1-276">Dado que el servicio hospedado debe establecer la propiedad en la comprobación de estado, esta también se registra en el contenedor de servicios (*LivenessProbeStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="214b1-276">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="214b1-277">Se crea un punto de conexión de comprobación de estado llamando a `MapHealthChecks` en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="214b1-277">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`.</span></span> <span data-ttu-id="214b1-278">En la aplicación de ejemplo, los puntos de conexión de la comprobación de estado se crean en:</span><span class="sxs-lookup"><span data-stu-id="214b1-278">In the sample app, the health check endpoints are created at:</span></span>

* <span data-ttu-id="214b1-279">`/health/ready` para la comprobación de la idoneidad.</span><span class="sxs-lookup"><span data-stu-id="214b1-279">`/health/ready` for the readiness check.</span></span> <span data-ttu-id="214b1-280">La comprobación de preparación filtra las comprobaciones de estado con la etiqueta `ready`.</span><span class="sxs-lookup"><span data-stu-id="214b1-280">The readiness check filters health checks to the health check with the `ready` tag.</span></span>
* <span data-ttu-id="214b1-281">`/health/live` para la comprobación de la ejecución.</span><span class="sxs-lookup"><span data-stu-id="214b1-281">`/health/live` for the liveness check.</span></span> <span data-ttu-id="214b1-282">La comprobación de ejecución filtra el elemento `StartupHostedServiceHealthCheck` y devuelve `false` en [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate). Para obtener más información, consulte [Filtrado de las comprobaciones de estado](#filter-health-checks).</span><span class="sxs-lookup"><span data-stu-id="214b1-282">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks))</span></span>

<span data-ttu-id="214b1-283">En el código de ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="214b1-283">In the following example code:</span></span>

* <span data-ttu-id="214b1-284">La comprobación de preparación usa todas las comprobaciones registradas con la etiqueta "ready".</span><span class="sxs-lookup"><span data-stu-id="214b1-284">The readiness check uses all registered checks with the 'ready' tag.</span></span>
* <span data-ttu-id="214b1-285">`Predicate` excluye todas las comprobaciones y devuelve una respuesta de que todo está correcto (200-OK).</span><span class="sxs-lookup"><span data-stu-id="214b1-285">The `Predicate` excludes all checks and return a 200-Ok.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health/ready", new HealthCheckOptions()
    {
        Predicate = (check) => check.Tags.Contains("ready"),
    });

    endpoints.MapHealthChecks("/health/live", new HealthCheckOptions()
    {
        Predicate = (_) => false
    });
}
```

<span data-ttu-id="214b1-286">Para ejecutar el escenario de configuración de la preparación/ejecución mediante la aplicación de ejemplo, ejecute el comando siguiente desde la carpeta del proyecto en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="214b1-286">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario liveness
```

<span data-ttu-id="214b1-287">En un explorador, visite `/health/ready` varias veces hasta que hayan pasado 15 segundos.</span><span class="sxs-lookup"><span data-stu-id="214b1-287">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="214b1-288">La comprobación de estado notifica un estado *Incorrecto* durante los primeros 15 segundos.</span><span class="sxs-lookup"><span data-stu-id="214b1-288">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="214b1-289">Pasados 15 segundos, el punto de conexión notifica un estado *Correcto*, lo que indica que el servicio hospedado ya ha finalizado la tarea de ejecución prolongada.</span><span class="sxs-lookup"><span data-stu-id="214b1-289">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="214b1-290">En este ejemplo también se crea un publicador de la comprobación de estado (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementación) que ejecuta la primera comprobación de preparación con un retraso de dos segundos.</span><span class="sxs-lookup"><span data-stu-id="214b1-290">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="214b1-291">Para obtener más información, consulte la sección [Publicador de la comprobación de estado](#health-check-publisher).</span><span class="sxs-lookup"><span data-stu-id="214b1-291">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="214b1-292">Ejemplo de Kubernetes</span><span class="sxs-lookup"><span data-stu-id="214b1-292">Kubernetes example</span></span>

<span data-ttu-id="214b1-293">Utilizar comprobaciones de preparación y ejecución independientes es útil en un entorno como [Kubernetes](https://kubernetes.io/).</span><span class="sxs-lookup"><span data-stu-id="214b1-293">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="214b1-294">En Kubernetes, es posible que una aplicación deba realizar un trabajo de inicio que requiera mucho tiempo antes de aceptar solicitudes, como una prueba de la disponibilidad de la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="214b1-294">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="214b1-295">El hecho de utilizar comprobaciones independientes permite que el orquestador distinga si la aplicación está funcionando, pero aún no esté preparada, o si la aplicación no se ha podido iniciar.</span><span class="sxs-lookup"><span data-stu-id="214b1-295">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="214b1-296">Para obtener más información sobre los sondeos de preparación y ejecución en Kubernetes, consulte [Configuración de sondeos de preparación y ejecución](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) en la documentación de Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="214b1-296">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="214b1-297">En el ejemplo siguiente, se muestra una configuración de sondeo de preparación de Kubernetes:</span><span class="sxs-lookup"><span data-stu-id="214b1-297">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="214b1-298">Sondeo basado en métrica con un escritor de respuesta personalizada</span><span class="sxs-lookup"><span data-stu-id="214b1-298">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="214b1-299">La aplicación de ejemplo muestra una comprobación de estado de memoria con un escritor de respuesta personalizada.</span><span class="sxs-lookup"><span data-stu-id="214b1-299">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="214b1-300">`MemoryHealthCheck` notifica un estado degradado si la aplicación usa más de un umbral de memoria determinado (1 GB en la aplicación de ejemplo).</span><span class="sxs-lookup"><span data-stu-id="214b1-300">`MemoryHealthCheck` reports a degraded status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="214b1-301">El elemento <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> incluye información del recolector de elementos no utilizados (GC) de la aplicación (*MemoryHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="214b1-301">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="214b1-302">Registre los servicios de comprobación de estado con <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> de `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="214b1-302">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="214b1-303">En lugar de pasar la comprobación de estado a <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> para habilitarla, `MemoryHealthCheck` se registra como servicio.</span><span class="sxs-lookup"><span data-stu-id="214b1-303">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="214b1-304">Todos los servicios registrados de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> están disponibles para los servicios de comprobación de estado y middleware.</span><span class="sxs-lookup"><span data-stu-id="214b1-304">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="214b1-305">Se recomienda registrar los servicios de comprobación de estado como los servicios de Singleton.</span><span class="sxs-lookup"><span data-stu-id="214b1-305">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="214b1-306">En *CustomWriterStartup.cs* de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="214b1-306">In *CustomWriterStartup.cs* of the sample app:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="214b1-307">Se crea un punto de conexión de comprobación de estado llamando a `MapHealthChecks` en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="214b1-307">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`.</span></span> <span data-ttu-id="214b1-308">Se proporciona un delegado `WriteResponse` a la propiedad <Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> para generar una respuesta JSON personalizada cuando se ejecuta la comprobación de estado:</span><span class="sxs-lookup"><span data-stu-id="214b1-308">A `WriteResponse` delegate is provided to the <Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> property to output a custom JSON response when the health check executes:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
}
```

<span data-ttu-id="214b1-309">El delegado `WriteResponse` da a `CompositeHealthCheckResult` el formato de objeto JSON y suspende el resultado de JSON para la respuesta de comprobación de estado.</span><span class="sxs-lookup"><span data-stu-id="214b1-309">The `WriteResponse` delegate formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response.</span></span> <span data-ttu-id="214b1-310">Para más información, consulte la sección [Personalización del resultado](#customize-output).</span><span class="sxs-lookup"><span data-stu-id="214b1-310">For more information, see the [Customize output](#customize-output) section.</span></span>

<span data-ttu-id="214b1-311">Para ejecutar el sondeo basado en métrica con el resultado de un escritor de respuesta personalizada mediante la aplicación de ejemplo, ejecute el comando siguiente desde la carpeta del proyecto en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="214b1-311">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="214b1-312">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) incluye escenarios de comprobación de estado basados en métrica, como comprobaciones de almacenamiento del disco y de ejecución del máximo valor.</span><span class="sxs-lookup"><span data-stu-id="214b1-312">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="214b1-313">Microsoft no ofrece soporte técnico para [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) ni lo mantiene.</span><span class="sxs-lookup"><span data-stu-id="214b1-313">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="214b1-314">Filtrado por puerto</span><span class="sxs-lookup"><span data-stu-id="214b1-314">Filter by port</span></span>

<span data-ttu-id="214b1-315">Llame a `RequireHost` en `MapHealthChecks` con un patrón de dirección URL que especifique un puerto para restringir las solicitudes de comprobación de estado al puerto especificado.</span><span class="sxs-lookup"><span data-stu-id="214b1-315">Call `RequireHost` on `MapHealthChecks` with a URL pattern that specifies a port to restrict health check requests to the port specified.</span></span> <span data-ttu-id="214b1-316">Esto se usa normalmente en un entorno de contenedor para exponer un puerto para los servicios de supervisión.</span><span class="sxs-lookup"><span data-stu-id="214b1-316">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="214b1-317">La aplicación de ejemplo configura el puerto con el [proveedor de configuración de variable de entorno](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="214b1-317">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="214b1-318">El puerto se establece en el archivo *launchSettings.json* y se pasa al proveedor de configuración a través de una variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="214b1-318">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="214b1-319">También debe configurar el servidor para que escuche las solicitudes en el puerto de administración.</span><span class="sxs-lookup"><span data-stu-id="214b1-319">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="214b1-320">Para utilizar la aplicación de ejemplo para que muestre la configuración del puerto de administración, cree el archivo *launchSettings.json* en una carpeta *Propiedades*.</span><span class="sxs-lookup"><span data-stu-id="214b1-320">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="214b1-321">El siguiente archivo *Properties/launchSettings.json* de la aplicación de ejemplo no se incluye en los archivos de proyecto de la aplicación de ejemplo y debe crearse manualmente:</span><span class="sxs-lookup"><span data-stu-id="214b1-321">The following *Properties/launchSettings.json* file in the sample app isn't included in the sample app's project files and must be created manually:</span></span>

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

<span data-ttu-id="214b1-322">Registre los servicios de comprobación de estado con <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> de `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="214b1-322">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="214b1-323">Cree un punto de conexión de comprobación de estado llamando a `MapHealthChecks` en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="214b1-323">Create a health check endpoint by calling `MapHealthChecks` in `Startup.Configure`.</span></span>

<span data-ttu-id="214b1-324">En la aplicación de ejemplo, una llamada a `RequireHost` en el punto de conexión `Startup.Configure` especifica el puerto de administración de la configuración:</span><span class="sxs-lookup"><span data-stu-id="214b1-324">In the sample app, a call to `RequireHost` on the endpoint in `Startup.Configure` specifies the management port from configuration:</span></span>

```csharp
endpoints.MapHealthChecks("/health")
    .RequireHost($"*:{Configuration["ManagementPort"]}");
```

<span data-ttu-id="214b1-325">Los puntos de conexión se crean en la aplicación de ejemplo en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="214b1-325">Endpoints are created in the sample app in `Startup.Configure`.</span></span> <span data-ttu-id="214b1-326">En el código de ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="214b1-326">In the following example code:</span></span>

* <span data-ttu-id="214b1-327">La comprobación de preparación usa todas las comprobaciones registradas con la etiqueta "ready".</span><span class="sxs-lookup"><span data-stu-id="214b1-327">The readiness check uses all registered checks with the 'ready' tag.</span></span>
* <span data-ttu-id="214b1-328">`Predicate` excluye todas las comprobaciones y devuelve una respuesta de que todo está correcto (200-OK).</span><span class="sxs-lookup"><span data-stu-id="214b1-328">The `Predicate` excludes all checks and return a 200-Ok.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health/ready", new HealthCheckOptions()
    {
        Predicate = (check) => check.Tags.Contains("ready"),
    });

    endpoints.MapHealthChecks("/health/live", new HealthCheckOptions()
    {
        Predicate = (_) => false
    });
}
```

> [!NOTE]
> <span data-ttu-id="214b1-329">Para evitar la creación del archivo *launchSettings.json* en la aplicación de ejemplo, configure el puerto de administración explícitamente en código.</span><span class="sxs-lookup"><span data-stu-id="214b1-329">You can avoid creating the *launchSettings.json* file in the sample app by setting the management port explicitly in code.</span></span> <span data-ttu-id="214b1-330">En *Program.cs*, donde se crea <xref:Microsoft.Extensions.Hosting.HostBuilder>, agregue una llamada a <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenAnyIP*> y proporcione el punto de conexión del puerto de administración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="214b1-330">In *Program.cs* where the <xref:Microsoft.Extensions.Hosting.HostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenAnyIP*> and provide the app's management port endpoint.</span></span> <span data-ttu-id="214b1-331">En `Configure` de *ManagementPortStartup.cs*, especifique el puerto de administración con `RequireHost`:</span><span class="sxs-lookup"><span data-stu-id="214b1-331">In `Configure` of *ManagementPortStartup.cs*, specify the management port with `RequireHost`:</span></span>
>
> <span data-ttu-id="214b1-332">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="214b1-332">*Program.cs*:</span></span>
>
> ```csharp
> return new HostBuilder()
>     .ConfigureWebHostDefaults(webBuilder =>
>     {
>         webBuilder.UseKestrel()
>             .ConfigureKestrel(serverOptions =>
>             {
>                 serverOptions.ListenAnyIP(5001);
>             })
>             .UseStartup(startupType);
>     })
>     .Build();
> ```
>
> <span data-ttu-id="214b1-333">*ManagementPortStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="214b1-333">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseEndpoints(endpoints =>
> {
>     endpoints.MapHealthChecks("/health").RequireHost("*:5001");
> });
> ```

<span data-ttu-id="214b1-334">Para ejecutar el escenario de configuración del puerto de administración mediante la aplicación de ejemplo, ejecute el comando siguiente desde la carpeta del proyecto en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="214b1-334">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="214b1-335">Distribución de una biblioteca de comprobación de estado</span><span class="sxs-lookup"><span data-stu-id="214b1-335">Distribute a health check library</span></span>

<span data-ttu-id="214b1-336">Para distribuir una comprobación de estado como una biblioteca, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="214b1-336">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="214b1-337">Escriba una comprobación de estado que implemente la interfaz de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> como una clase independiente.</span><span class="sxs-lookup"><span data-stu-id="214b1-337">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="214b1-338">La clase puede depender de la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection), de la activación del tipo y de las [opciones denominadas](xref:fundamentals/configuration/options) para acceder a los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="214b1-338">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   <span data-ttu-id="214b1-339">En la lógica de comprobaciones de estado de `CheckHealthAsync`:</span><span class="sxs-lookup"><span data-stu-id="214b1-339">In the health checks logic of `CheckHealthAsync`:</span></span>

   * <span data-ttu-id="214b1-340">`data1` y `data2` se usan en el método para ejecutar la lógica de comprobación de estado del sondeo.</span><span class="sxs-lookup"><span data-stu-id="214b1-340">`data1` and `data2` are used in the method to run the probe's health check logic.</span></span>
   * <span data-ttu-id="214b1-341">Se controla `AccessViolationException`.</span><span class="sxs-lookup"><span data-stu-id="214b1-341">`AccessViolationException` is handled.</span></span>

   <span data-ttu-id="214b1-342">Cuando se produce un <xref:System.AccessViolationException>, se devuelve <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> con <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> para permitir que los usuarios configuren el estado de error de las comprobaciones de estado.</span><span class="sxs-lookup"><span data-stu-id="214b1-342">When an <xref:System.AccessViolationException> occurs, the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> is returned with the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> to allow users to configure the health checks failure status.</span></span>

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

1. <span data-ttu-id="214b1-343">Escriba un método de extensión con los parámetros a los que la aplicación de uso llama en su método `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="214b1-343">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="214b1-344">En el ejemplo siguiente, suponga que existe la siguiente firma del método de comprobación de estado:</span><span class="sxs-lookup"><span data-stu-id="214b1-344">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="214b1-345">La firma anterior indica que la `ExampleHealthCheck` requiere datos adicionales para procesar la lógica de sondeo de la comprobación de estado.</span><span class="sxs-lookup"><span data-stu-id="214b1-345">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="214b1-346">Los datos se proporcionan al delegado que se usa para crear la instancia de la comprobación de estado cuando la comprobación de estado se registra con un método de extensión.</span><span class="sxs-lookup"><span data-stu-id="214b1-346">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="214b1-347">En el ejemplo siguiente, el autor de llamada especifica los siguientes elementos opcionales:</span><span class="sxs-lookup"><span data-stu-id="214b1-347">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="214b1-348">nombre de la comprobación de estado (`name`).</span><span class="sxs-lookup"><span data-stu-id="214b1-348">health check name (`name`).</span></span> <span data-ttu-id="214b1-349">En el caso de `null`, se utiliza `example_health_check`.</span><span class="sxs-lookup"><span data-stu-id="214b1-349">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="214b1-350">punto de datos de cadena para la comprobación de estado (`data1`).</span><span class="sxs-lookup"><span data-stu-id="214b1-350">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="214b1-351">punto de datos enteros para la comprobación de estado (`data2`).</span><span class="sxs-lookup"><span data-stu-id="214b1-351">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="214b1-352">En el caso de `null`, se utiliza `1`.</span><span class="sxs-lookup"><span data-stu-id="214b1-352">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="214b1-353">estado de error (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span><span class="sxs-lookup"><span data-stu-id="214b1-353">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="214b1-354">De manera predeterminada, es `null`.</span><span class="sxs-lookup"><span data-stu-id="214b1-354">The default is `null`.</span></span> <span data-ttu-id="214b1-355">Si `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) se notifica para un estado de error.</span><span class="sxs-lookup"><span data-stu-id="214b1-355">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="214b1-356">etiquetas (`IEnumerable<string>`).</span><span class="sxs-lookup"><span data-stu-id="214b1-356">tags (`IEnumerable<string>`).</span></span>

   ```csharp
   using System.Collections.Generic;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   public static class ExampleHealthCheckBuilderExtensions
   {
       const string DefaultName = "example_health_check";

       public static IHealthChecksBuilder AddExampleHealthCheck(
           this IHealthChecksBuilder builder,
           string name = default,
           string data1,
           int data2 = 1,
           HealthStatus? failureStatus = default,
           IEnumerable<string> tags = default)
       {
           return builder.Add(new HealthCheckRegistration(
               name ?? DefaultName,
               sp => new ExampleHealthCheck(data1, data2),
               failureStatus,
               tags));
       }
   }
   ```

## <a name="health-check-publisher"></a><span data-ttu-id="214b1-357">Publicador de la comprobación de estado</span><span class="sxs-lookup"><span data-stu-id="214b1-357">Health Check Publisher</span></span>

<span data-ttu-id="214b1-358">Cuando un elemento <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> se agrega al contenedor de servicios, el sistema de comprobación de estado ejecuta periódicamente las comprobaciones de estado y llama a `PublishAsync` con el resultado.</span><span class="sxs-lookup"><span data-stu-id="214b1-358">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="214b1-359">Esto es útil en un escenario de sistema de seguimiento de estado basado en inserción en el que se espera que cada proceso llame periódicamente al sistema de seguimiento con el fin de determinar el estado.</span><span class="sxs-lookup"><span data-stu-id="214b1-359">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="214b1-360">La interfaz de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> tiene un único método:</span><span class="sxs-lookup"><span data-stu-id="214b1-360">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="214b1-361"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> le permiten establecer:</span><span class="sxs-lookup"><span data-stu-id="214b1-361"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="214b1-362"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>&ndash; El retraso inicial aplicado tras iniciarse la aplicación antes de ejecutar instancias de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="214b1-362"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="214b1-363">El retraso se aplica una vez durante el inicio y no se aplica a las iteraciones posteriores.</span><span class="sxs-lookup"><span data-stu-id="214b1-363">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="214b1-364">El valor predeterminado es cinco segundos.</span><span class="sxs-lookup"><span data-stu-id="214b1-364">The default value is five seconds.</span></span>
* <span data-ttu-id="214b1-365"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period>&ndash; El período de ejecución de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="214b1-365"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="214b1-366">El valor predeterminado es 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="214b1-366">The default value is 30 seconds.</span></span>
* <span data-ttu-id="214b1-367"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; Si <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> es `null` (valor predeterminado), el servicio de publicador de la comprobación de estado ejecuta todas las comprobaciones de estado registradas.</span><span class="sxs-lookup"><span data-stu-id="214b1-367"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="214b1-368">Para ejecutar un subconjunto de comprobaciones de estado, proporcione una función que filtre el conjunto de comprobaciones.</span><span class="sxs-lookup"><span data-stu-id="214b1-368">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="214b1-369">El predicado se evalúa cada período.</span><span class="sxs-lookup"><span data-stu-id="214b1-369">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="214b1-370"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; El tiempo de expiración para ejecutar las comprobaciones de estado para todas las instancias de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="214b1-370"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="214b1-371">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> para ejecutar sin tiempo de expiración.</span><span class="sxs-lookup"><span data-stu-id="214b1-371">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="214b1-372">El valor predeterminado es 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="214b1-372">The default value is 30 seconds.</span></span>

<span data-ttu-id="214b1-373">En la aplicación de ejemplo, `ReadinessPublisher` es una implementación de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="214b1-373">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="214b1-374">El estado de comprobación de estado se registra para cada comprobación en un nivel de registro de:</span><span class="sxs-lookup"><span data-stu-id="214b1-374">The health check status is logged for each check at a log level of:</span></span>

* <span data-ttu-id="214b1-375">Información (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>) si el estado de las comprobaciones de estado es <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>.</span><span class="sxs-lookup"><span data-stu-id="214b1-375">Information (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>) if the health checks status is <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>.</span></span>
* <span data-ttu-id="214b1-376">Error (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>) si el estado es <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> o <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>.</span><span class="sxs-lookup"><span data-stu-id="214b1-376">Error (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>) if the status is either <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> or <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>.</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=18-27)]

<span data-ttu-id="214b1-377">En el ejemplo `LivenessProbeStartup` de la aplicación de ejemplo, la comprobación de preparación `StartupHostedService` tiene un retraso de inicio de dos segundos y ejecuta la comprobación cada 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="214b1-377">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="214b1-378">Para activar la implementación de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>, el ejemplo registra `ReadinessPublisher` como servicio singleton en el contenedor de [inserción de dependencias (DI)](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="214b1-378">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

> [!NOTE]
> <span data-ttu-id="214b1-379">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) incluye editores para varios sistemas, como [Application Insights](/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="214b1-379">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="214b1-380">Microsoft no ofrece soporte técnico para [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) ni lo mantiene.</span><span class="sxs-lookup"><span data-stu-id="214b1-380">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="restrict-health-checks-with-mapwhen"></a><span data-ttu-id="214b1-381">Restricción de las comprobaciones de estado con MapWhen</span><span class="sxs-lookup"><span data-stu-id="214b1-381">Restrict health checks with MapWhen</span></span>

<span data-ttu-id="214b1-382">Use <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> para crear de forma condicional una rama de la canalización de solicitudes para los puntos de conexión de comprobación del estado.</span><span class="sxs-lookup"><span data-stu-id="214b1-382">Use <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> to conditionally branch the request pipeline for health check endpoints.</span></span>

<span data-ttu-id="214b1-383">En el ejemplo siguiente, `MapWhen` crea una rama de la canalización de solicitudes para activar el middleware de comprobaciones de estado si se recibe una solicitud GET para el punto de conexión `api/HealthCheck`:</span><span class="sxs-lookup"><span data-stu-id="214b1-383">In the following example, `MapWhen` branches the request pipeline to activate Health Checks Middleware if a GET request is received for the `api/HealthCheck` endpoint:</span></span>

```csharp
app.MapWhen(
    context => context.Request.Method == HttpMethod.Get.Method && 
        context.Request.Path.StartsWith("/api/HealthCheck"),
    builder => builder.UseHealthChecks());

app.UseEndpoints(endpoints =>
{
    endpoints.MapRazorPages();
});
```

<span data-ttu-id="214b1-384">Para obtener más información, vea <xref:fundamentals/middleware/index#use-run-and-map>.</span><span class="sxs-lookup"><span data-stu-id="214b1-384">For more information, see <xref:fundamentals/middleware/index#use-run-and-map>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="214b1-385">ASP.NET Core ofrece el middleware de comprobaciones de estado y bibliotecas para informar sobre el estado de los componentes de la infraestructura de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="214b1-385">ASP.NET Core offers Health Checks Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="214b1-386">Una aplicación se encarga de exponer las comprobaciones de estado como puntos de conexión HTTP.</span><span class="sxs-lookup"><span data-stu-id="214b1-386">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="214b1-387">Los puntos de conexión de las comprobaciones de estado pueden configurarse para diversos escenarios de supervisión en tiempo real:</span><span class="sxs-lookup"><span data-stu-id="214b1-387">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="214b1-388">Los orquestadores de contenedores y los equilibradores de carga pueden utilizar los sondeos de estado para comprobar el estado de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="214b1-388">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="214b1-389">Por ejemplo, para responder a una comprobación de estado con errores, es posible que un orquestador de contenedores detenga una implementación en curso o reinicie un contenedor.</span><span class="sxs-lookup"><span data-stu-id="214b1-389">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="214b1-390">Como respuesta a una aplicación con estado incorrecto, es posible que un equilibrador de carga enrute el tráfico al margen de la instancia con errores hacia una instancia con estado correcto.</span><span class="sxs-lookup"><span data-stu-id="214b1-390">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="214b1-391">El uso de la memoria, el disco y otros recursos del servidor físico puede supervisarse para determinar si el estado es correcto.</span><span class="sxs-lookup"><span data-stu-id="214b1-391">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="214b1-392">Las comprobaciones de estado pueden probar las dependencias de una aplicación, como las bases de datos y los puntos de conexión de servicio externo, para confirmar la disponibilidad y el funcionamiento normal.</span><span class="sxs-lookup"><span data-stu-id="214b1-392">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="214b1-393">[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="214b1-393">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="214b1-394">La aplicación de muestra incluye ejemplos de los escenarios descritos en este tema.</span><span class="sxs-lookup"><span data-stu-id="214b1-394">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="214b1-395">Para ejecutar la aplicación de ejemplo para un escenario determinado, use el comando [dotnet run](/dotnet/core/tools/dotnet-run) desde la carpeta del proyecto en un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="214b1-395">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="214b1-396">Para obtener información sobre cómo utilizar la aplicación de ejemplo, consulte el archivo *README.md* de la aplicación de ejemplo y las descripciones de escenarios de este tema.</span><span class="sxs-lookup"><span data-stu-id="214b1-396">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="214b1-397">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="214b1-397">Prerequisites</span></span>

<span data-ttu-id="214b1-398">Normalmente, las comprobaciones de estado se usan con un servicio de supervisión externa o un orquestador de contenedores para comprobar el estado de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="214b1-398">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="214b1-399">Antes de agregar comprobaciones de estado a una aplicación, debe decidir en qué sistema de supervisión se va a usar.</span><span class="sxs-lookup"><span data-stu-id="214b1-399">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="214b1-400">El sistema de supervisión determina qué tipos de comprobaciones de estado se deben crear y cómo configurar sus puntos de conexión.</span><span class="sxs-lookup"><span data-stu-id="214b1-400">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="214b1-401">Haga referencia al [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) o agregue una referencia de paquete al paquete [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks).</span><span class="sxs-lookup"><span data-stu-id="214b1-401">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package.</span></span>

<span data-ttu-id="214b1-402">La aplicación de ejemplo proporciona código de inicio para mostrar las comprobaciones de estado para varios escenarios.</span><span class="sxs-lookup"><span data-stu-id="214b1-402">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="214b1-403">En el escenario de [sondeo de base de datos](#database-probe) se comprueba el estado de una conexión de base de datos mediante [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span><span class="sxs-lookup"><span data-stu-id="214b1-403">The [database probe](#database-probe) scenario checks the health of a database connection using [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span></span> <span data-ttu-id="214b1-404">El escenario [sondeo de DbContext](#entity-framework-core-dbcontext-probe) comprueba una base de datos mediante un elemento `DbContext` de EF Core.</span><span class="sxs-lookup"><span data-stu-id="214b1-404">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="214b1-405">Para explorar los escenarios de la base de datos, la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="214b1-405">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="214b1-406">Crea una base de datos y proporciona su cadena de conexión en el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="214b1-406">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="214b1-407">Tiene las siguientes referencias de paquete en su archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="214b1-407">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="214b1-408">AspNetCore.HealthChecks.SqlServer</span><span class="sxs-lookup"><span data-stu-id="214b1-408">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="214b1-409">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="214b1-409">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="214b1-410">Microsoft no ofrece soporte técnico para [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) ni lo mantiene.</span><span class="sxs-lookup"><span data-stu-id="214b1-410">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="214b1-411">Otro escenario de comprobación de estado muestra cómo filtrar las comprobaciones de estado por un puerto de administración.</span><span class="sxs-lookup"><span data-stu-id="214b1-411">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="214b1-412">La aplicación de ejemplo requiere la creación de un archivo *Properties/launchSettings.json* que incluya la dirección URL de administración y el puerto de administración.</span><span class="sxs-lookup"><span data-stu-id="214b1-412">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="214b1-413">Para obtener más información, consulte la sección [Filtrado por puerto](#filter-by-port).</span><span class="sxs-lookup"><span data-stu-id="214b1-413">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="214b1-414">Sondeo de estado básico</span><span class="sxs-lookup"><span data-stu-id="214b1-414">Basic health probe</span></span>

<span data-ttu-id="214b1-415">Para muchas aplicaciones, una configuración de sondeo de estado básico que notifique la disponibilidad de la aplicación para procesar las solicitudes (*ejecución*) es suficiente para detectar el estado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="214b1-415">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="214b1-416">La configuración básica registra los servicios de comprobación de estado y llama al middleware de comprobaciones de estado para responder a un punto de conexión de dirección URL con una respuesta de estado.</span><span class="sxs-lookup"><span data-stu-id="214b1-416">The basic configuration registers health check services and calls the Health Checks Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="214b1-417">De forma predeterminada, no se registran comprobaciones de estado específicas para probar cualquier dependencia o subsistema concretos.</span><span class="sxs-lookup"><span data-stu-id="214b1-417">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="214b1-418">La aplicación se considera correcta si es capaz de responder en la dirección URL de punto de conexión de estado.</span><span class="sxs-lookup"><span data-stu-id="214b1-418">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="214b1-419">El escritor de respuesta predeterminado escribe el estado (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) como respuesta de texto no cifrado al cliente, que indica un estado [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) o [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus).</span><span class="sxs-lookup"><span data-stu-id="214b1-419">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="214b1-420">Registre los servicios de comprobación de estado con <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> de `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="214b1-420">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="214b1-421">Agregue un punto de conexión para el middleware de comprobaciones de estado con <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> en la canalización de procesamiento de solicitudes de `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="214b1-421">Add an endpoint for Health Checks Middleware with <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the request processing pipeline of `Startup.Configure`.</span></span>

<span data-ttu-id="214b1-422">En la aplicación de ejemplo, el punto de conexión de la comprobación de estado se crea en `/health` (*BasicStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="214b1-422">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

```csharp
public class BasicStartup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddHealthChecks();
    }

    public void Configure(IApplicationBuilder app)
    {
        app.UseHealthChecks("/health");
    }
}
```

<span data-ttu-id="214b1-423">Para ejecutar el escenario de configuración básica mediante la aplicación de ejemplo, ejecute el comando siguiente desde la carpeta del proyecto en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="214b1-423">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="214b1-424">Ejemplo de Docker</span><span class="sxs-lookup"><span data-stu-id="214b1-424">Docker example</span></span>

<span data-ttu-id="214b1-425">[Docker](xref:host-and-deploy/docker/index) ofrece una directiva de `HEALTHCHECK` integrada que puede utilizarse para comprobar el estado de una aplicación que use la configuración de comprobación de estado básica:</span><span class="sxs-lookup"><span data-stu-id="214b1-425">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="214b1-426">Creación de comprobaciones de estado</span><span class="sxs-lookup"><span data-stu-id="214b1-426">Create health checks</span></span>

<span data-ttu-id="214b1-427">Las comprobaciones de estado se crean mediante la implementación de la interfaz de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck>.</span><span class="sxs-lookup"><span data-stu-id="214b1-427">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="214b1-428">El método <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> devuelve un elemento <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> que indica el estado como `Healthy`, `Degraded` o `Unhealthy`.</span><span class="sxs-lookup"><span data-stu-id="214b1-428">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="214b1-429">El resultado se escribe como una respuesta de texto no cifrado con un código de estado configurable. (La configuración se describe en la sección [Opciones de comprobación de estado](#health-check-options)).</span><span class="sxs-lookup"><span data-stu-id="214b1-429">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="214b1-430"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> también puede devolver pares clave-valor opcionales.</span><span class="sxs-lookup"><span data-stu-id="214b1-430"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

### <a name="example-health-check"></a><span data-ttu-id="214b1-431">Comprobación de estado de ejemplo</span><span class="sxs-lookup"><span data-stu-id="214b1-431">Example health check</span></span>

<span data-ttu-id="214b1-432">La siguiente clase `ExampleHealthCheck` muestra el diseño de una comprobación de estado.</span><span class="sxs-lookup"><span data-stu-id="214b1-432">The following `ExampleHealthCheck` class demonstrates the layout of a health check.</span></span> <span data-ttu-id="214b1-433">La lógica de comprobaciones de estado se coloca en el método `CheckHealthAsync`.</span><span class="sxs-lookup"><span data-stu-id="214b1-433">The health checks logic is placed in the `CheckHealthAsync` method.</span></span> <span data-ttu-id="214b1-434">En el ejemplo siguiente se establece una variable ficticia, `healthCheckResultHealthy`, en `true`.</span><span class="sxs-lookup"><span data-stu-id="214b1-434">The following example sets a dummy variable, `healthCheckResultHealthy`, to `true`.</span></span> <span data-ttu-id="214b1-435">Si el valor de `healthCheckResultHealthy` se establece en `false`, se devuelve el estado [HealthCheckResult.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*).</span><span class="sxs-lookup"><span data-stu-id="214b1-435">If the value of `healthCheckResultHealthy` is set to `false`, the [HealthCheckResult.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) status is returned.</span></span>

```csharp
public class ExampleHealthCheck : IHealthCheck
{
    public Task<HealthCheckResult> CheckHealthAsync(
        HealthCheckContext context,
        CancellationToken cancellationToken = default(CancellationToken))
    {
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

### <a name="register-health-check-services"></a><span data-ttu-id="214b1-436">Registro de los servicios de comprobación de estado</span><span class="sxs-lookup"><span data-stu-id="214b1-436">Register health check services</span></span>

<span data-ttu-id="214b1-437">El tipo `ExampleHealthCheck` se agrega a los servicios de comprobación de estado en `Startup.ConfigureServices` con <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>:</span><span class="sxs-lookup"><span data-stu-id="214b1-437">The `ExampleHealthCheck` type is added to health check services in `Startup.ConfigureServices` with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

<span data-ttu-id="214b1-438">La sobrecarga <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> que se muestra en el ejemplo siguiente establece el estado de error (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) para notificar cuándo la comprobación de estado informa de un error.</span><span class="sxs-lookup"><span data-stu-id="214b1-438">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="214b1-439">Si el estado de error se establece en `null` (valor predeterminado), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) se notifica.</span><span class="sxs-lookup"><span data-stu-id="214b1-439">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="214b1-440">Esta sobrecarga es un escenario útil para los creadores de bibliotecas en los que la aplicación ejecuta el estado de error indicado por la biblioteca cuando se produce un error de comprobación de estado si la implementación de la comprobación de estado respeta la configuración.</span><span class="sxs-lookup"><span data-stu-id="214b1-440">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="214b1-441">Las *etiquetas* pueden usarse para filtrar las comprobaciones de estado, que se describen con más detalle en la sección [Filtrado de las comprobaciones de estado](#filter-health-checks).</span><span class="sxs-lookup"><span data-stu-id="214b1-441">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<span data-ttu-id="214b1-442"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> también puede ejecutar una función lambda.</span><span class="sxs-lookup"><span data-stu-id="214b1-442"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="214b1-443">En el ejemplo siguiente `Startup.ConfigureServices`, el nombre de la comprobación de estado se especifica como `Example` y la comprobación siempre devuelve un estado correcto:</span><span class="sxs-lookup"><span data-stu-id="214b1-443">In the following `Startup.ConfigureServices` example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Example", () =>
        HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" });
```

### <a name="use-health-checks-middleware"></a><span data-ttu-id="214b1-444">Uso del Middleware de comprobaciones de estado</span><span class="sxs-lookup"><span data-stu-id="214b1-444">Use Health Checks Middleware</span></span>

<span data-ttu-id="214b1-445">En `Startup.Configure`, llame a <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> en la canalización de procesamiento con la dirección URL del punto de conexión o la ruta de acceso relativa:</span><span class="sxs-lookup"><span data-stu-id="214b1-445">In `Startup.Configure`, call <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the processing pipeline with the endpoint URL or relative path:</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="214b1-446">Si las comprobaciones de estado deben realizar la escucha en un puerto específico, use una sobrecarga de <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> para establecer el puerto, como se describe con más detalle en la sección [Filtrado por puerto](#filter-by-port):</span><span class="sxs-lookup"><span data-stu-id="214b1-446">If the health checks should listen on a specific port, use an overload of <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> to set the port (described further in the [Filter by port](#filter-by-port) section):</span></span>

```csharp
app.UseHealthChecks("/health", port: 8000);
```

## <a name="health-check-options"></a><span data-ttu-id="214b1-447">Opciones de comprobación de estado</span><span class="sxs-lookup"><span data-stu-id="214b1-447">Health check options</span></span>

<span data-ttu-id="214b1-448">El elemento <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> ofrece una oportunidad para personalizar el comportamiento de las comprobaciones de estado:</span><span class="sxs-lookup"><span data-stu-id="214b1-448"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="214b1-449">Filtrado de las comprobaciones de estado</span><span class="sxs-lookup"><span data-stu-id="214b1-449">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="214b1-450">Personalización del código de estado HTTP</span><span class="sxs-lookup"><span data-stu-id="214b1-450">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="214b1-451">Supresión de los encabezados de caché</span><span class="sxs-lookup"><span data-stu-id="214b1-451">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="214b1-452">Personalización del resultado</span><span class="sxs-lookup"><span data-stu-id="214b1-452">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="214b1-453">Filtrado de las comprobaciones de estado</span><span class="sxs-lookup"><span data-stu-id="214b1-453">Filter health checks</span></span>

<span data-ttu-id="214b1-454">De forma predeterminada, el middleware de comprobaciones de estado ejecuta todas las comprobaciones de estado registradas.</span><span class="sxs-lookup"><span data-stu-id="214b1-454">By default, Health Checks Middleware runs all registered health checks.</span></span> <span data-ttu-id="214b1-455">Para ejecutar un subconjunto de comprobaciones de estado, proporcione una función que devuelva un valor booleano para la opción <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate>.</span><span class="sxs-lookup"><span data-stu-id="214b1-455">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="214b1-456">En el ejemplo siguiente, la etiqueta (`bar_tag`) de la comprobación de estado `Bar` la filtra, en la instrucción condicional de la función, donde `true` solo se devuelve si la propiedad <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> de la comprobación de estado coincide con `foo_tag` o `baz_tag`:</span><span class="sxs-lookup"><span data-stu-id="214b1-456">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

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
            HealthCheckResult.Unhealthy("Bar is unhealthy!"), 
                tags: new[] { "bar_tag" })
        .AddCheck("Baz", () =>
            HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
}

public void Configure(IApplicationBuilder app)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        Predicate = (check) => check.Tags.Contains("foo_tag") ||
            check.Tags.Contains("baz_tag")
    });
}
```

### <a name="customize-the-http-status-code"></a><span data-ttu-id="214b1-457">Personalización del código de estado HTTP</span><span class="sxs-lookup"><span data-stu-id="214b1-457">Customize the HTTP status code</span></span>

<span data-ttu-id="214b1-458">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> para personalizar la asignación del estado de mantenimiento de los códigos de estado HTTP.</span><span class="sxs-lookup"><span data-stu-id="214b1-458">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="214b1-459">Las siguientes asignaciones de <xref:Microsoft.AspNetCore.Http.StatusCodes> son los valores predeterminados que el middleware utiliza.</span><span class="sxs-lookup"><span data-stu-id="214b1-459">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="214b1-460">Cambie los valores de código de estado para satisfacer sus necesidades.</span><span class="sxs-lookup"><span data-stu-id="214b1-460">Change the status code values to meet your requirements.</span></span>

<span data-ttu-id="214b1-461">En `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="214b1-461">In `Startup.Configure`:</span></span>

```csharp
//using Microsoft.AspNetCore.Diagnostics.HealthChecks;
//using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    ResultStatusCodes =
    {
        [HealthStatus.Healthy] = StatusCodes.Status200OK,
        [HealthStatus.Degraded] = StatusCodes.Status200OK,
        [HealthStatus.Unhealthy] = StatusCodes.Status503ServiceUnavailable
    }
});
```

### <a name="suppress-cache-headers"></a><span data-ttu-id="214b1-462">Supresión de los encabezados de caché</span><span class="sxs-lookup"><span data-stu-id="214b1-462">Suppress cache headers</span></span>

<span data-ttu-id="214b1-463"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controla si el middleware de comprobaciones de estado agrega encabezados HTTP a una respuesta de sondeo para evitar el almacenamiento en caché de respuesta.</span><span class="sxs-lookup"><span data-stu-id="214b1-463"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Checks Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="214b1-464">Si el valor es `false` (valor predeterminado), el middleware establece o invalida los encabezados `Cache-Control`, `Expires` y `Pragma` para evitar el almacenamiento en caché de respuesta.</span><span class="sxs-lookup"><span data-stu-id="214b1-464">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="214b1-465">Si el valor es `true`, el middleware no modifica los encabezados de caché de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="214b1-465">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

<span data-ttu-id="214b1-466">En `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="214b1-466">In `Startup.Configure`:</span></span>

```csharp
//using Microsoft.AspNetCore.Diagnostics.HealthChecks;
//using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    AllowCachingResponses = false
});
```

### <a name="customize-output"></a><span data-ttu-id="214b1-467">Personalización del resultado</span><span class="sxs-lookup"><span data-stu-id="214b1-467">Customize output</span></span>

<span data-ttu-id="214b1-468">La opción <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> obtiene o establece un delegado que se usa para escribir la respuesta.</span><span class="sxs-lookup"><span data-stu-id="214b1-468">The <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> option gets or sets a delegate used to write the response.</span></span> <span data-ttu-id="214b1-469">El delegado predeterminado escribe una respuesta de texto no cifrado mínima con el valor de cadena [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span><span class="sxs-lookup"><span data-stu-id="214b1-469">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span>

<span data-ttu-id="214b1-470">En `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="214b1-470">In `Startup.Configure`:</span></span>

```csharp
// using Microsoft.AspNetCore.Diagnostics.HealthChecks;
// using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    ResponseWriter = WriteResponse
});
```

<span data-ttu-id="214b1-471">El delegado predeterminado escribe una respuesta de texto no cifrado mínima con el valor de cadena [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span><span class="sxs-lookup"><span data-stu-id="214b1-471">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span> <span data-ttu-id="214b1-472">El siguiente delegado personalizado, `WriteResponse`, genera una respuesta JSON personalizada:</span><span class="sxs-lookup"><span data-stu-id="214b1-472">The following custom delegate, `WriteResponse`, outputs a custom JSON response:</span></span>

```csharp
private static Task WriteResponse(HttpContext httpContext, HealthReport result)
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

<span data-ttu-id="214b1-473">El sistema de comprobaciones de estado no ofrece compatibilidad integrada con formatos de retorno JSON complejos porque el formato es específico del sistema de supervisión elegido.</span><span class="sxs-lookup"><span data-stu-id="214b1-473">The health checks system doesn't provide built-in support for complex JSON return formats because the format is specific to your choice of monitoring system.</span></span> <span data-ttu-id="214b1-474">No dude en personalizar `JObject` en el ejemplo anterior según sea sus necesidades.</span><span class="sxs-lookup"><span data-stu-id="214b1-474">Feel free to customize the `JObject` in the preceding example as necessary to meet your needs.</span></span>

## <a name="database-probe"></a><span data-ttu-id="214b1-475">Sondeo de bases de datos</span><span class="sxs-lookup"><span data-stu-id="214b1-475">Database probe</span></span>

<span data-ttu-id="214b1-476">Una comprobación de estado puede especificar que una consulta de base de datos se ejecute como una prueba booleana para indicar si esta responde con normalidad.</span><span class="sxs-lookup"><span data-stu-id="214b1-476">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="214b1-477">En la aplicación de ejemplo se usa [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), una biblioteca de comprobaciones de estado para las aplicaciones ASP.NET Core, que permite realizar una comprobación de estado en una base de datos de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="214b1-477">The sample app uses [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="214b1-478">`AspNetCore.Diagnostics.HealthChecks` ejecuta una consulta `SELECT 1` en la base de datos para confirmar que la conexión a la base de datos es correcta.</span><span class="sxs-lookup"><span data-stu-id="214b1-478">`AspNetCore.Diagnostics.HealthChecks` executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="214b1-479">Al comprobar la conexión de una base de datos a una consulta, elija una consulta que se devuelva rápidamente.</span><span class="sxs-lookup"><span data-stu-id="214b1-479">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="214b1-480">El enfoque de la consulta plantea el riesgo de sobrecargar la base de datos y degradar el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="214b1-480">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="214b1-481">En la mayoría de los casos, no es necesario ejecutar una consulta de prueba.</span><span class="sxs-lookup"><span data-stu-id="214b1-481">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="214b1-482">Simplemente, realizar una conexión correcta a la base de datos es suficiente.</span><span class="sxs-lookup"><span data-stu-id="214b1-482">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="214b1-483">Si resulta necesario ejecutar una consulta, elija una consulta SELECT sencilla, como `SELECT 1`.</span><span class="sxs-lookup"><span data-stu-id="214b1-483">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="214b1-484">Incluya una referencia de paquete a [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="214b1-484">Include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="214b1-485">Proporcione una cadena de conexión a base de datos válida en el archivo *appsettings.json* de la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="214b1-485">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="214b1-486">La aplicación usa una base de datos de SQL Server denominada `HealthCheckSample`:</span><span class="sxs-lookup"><span data-stu-id="214b1-486">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="214b1-487">Registre los servicios de comprobación de estado con <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> de `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="214b1-487">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="214b1-488">La aplicación de ejemplo llama al método `AddSqlServer` con la cadena de conexión de la base de datos (*DbHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="214b1-488">The sample app calls the `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="214b1-489">Llame al middleware de comprobaciones de estado en la canalización de procesamiento de la aplicación en `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="214b1-489">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`:</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="214b1-490">Para ejecutar el escenario de sondeo de base de datos mediante la aplicación de ejemplo, ejecute el comando siguiente desde la carpeta del proyecto en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="214b1-490">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="214b1-491">Microsoft no ofrece soporte técnico para [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) ni lo mantiene.</span><span class="sxs-lookup"><span data-stu-id="214b1-491">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="214b1-492">Sondeo de DbContext de Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="214b1-492">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="214b1-493">La comprobación `DbContext` confirma que la aplicación puede comunicarse con la base de datos configurada para un elemento `DbContext` de EF Core.</span><span class="sxs-lookup"><span data-stu-id="214b1-493">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="214b1-494">La comprobación `DbContext` se admite en las aplicaciones que:</span><span class="sxs-lookup"><span data-stu-id="214b1-494">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="214b1-495">Usan [Entity Framework (EF) Core](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="214b1-495">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="214b1-496">Incluyen una referencia de paquete a [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span><span class="sxs-lookup"><span data-stu-id="214b1-496">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="214b1-497">`AddDbContextCheck<TContext>` registra una comprobación de estado para un elemento `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="214b1-497">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="214b1-498">El elemento `DbContext` se proporciona como `TContext` en el método.</span><span class="sxs-lookup"><span data-stu-id="214b1-498">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="214b1-499">Hay disponible una sobrecarga para configurar el estado de error, las etiquetas y una consulta de prueba personalizada.</span><span class="sxs-lookup"><span data-stu-id="214b1-499">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="214b1-500">De manera predeterminada:</span><span class="sxs-lookup"><span data-stu-id="214b1-500">By default:</span></span>

* <span data-ttu-id="214b1-501">`DbContextHealthCheck` llama al método `CanConnectAsync` de EF Core.</span><span class="sxs-lookup"><span data-stu-id="214b1-501">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="214b1-502">Se puede personalizar qué operación se ejecuta al comprobar el estado con sobrecargas del método `AddDbContextCheck`.</span><span class="sxs-lookup"><span data-stu-id="214b1-502">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="214b1-503">El nombre de la comprobación de estado es el nombre del tipo `TContext`.</span><span class="sxs-lookup"><span data-stu-id="214b1-503">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="214b1-504">En la aplicación de ejemplo, `AppDbContext` se proporciona para `AddDbContextCheck` y se registra como un servicio en `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="214b1-504">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="214b1-505">En la aplicación de ejemplo, `UseHealthChecks` agrega el middleware de comprobaciones de estado en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="214b1-505">In the sample app, `UseHealthChecks` adds the Health Checks Middleware in `Startup.Configure`.</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="214b1-506">Para ejecutar el escenario de sondeo de `DbContext` mediante la aplicación de ejemplo, confirme que la base de datos que la cadena de conexión especifica no exista en la instancia de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="214b1-506">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="214b1-507">Si la base de datos existe, elimínela.</span><span class="sxs-lookup"><span data-stu-id="214b1-507">If the database exists, delete it.</span></span>

<span data-ttu-id="214b1-508">Ejecute el comando siguiente desde la carpeta del proyecto en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="214b1-508">Execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario dbcontext
```

<span data-ttu-id="214b1-509">Cuando la aplicación ya se esté ejecutando, compruebe el estado de mantenimiento mediante una solicitud al punto de conexión `/health` en un explorador.</span><span class="sxs-lookup"><span data-stu-id="214b1-509">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="214b1-510">La base de datos y `AppDbContext` no existen, por lo que la aplicación proporciona la respuesta siguiente:</span><span class="sxs-lookup"><span data-stu-id="214b1-510">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="214b1-511">Active la aplicación de ejemplo para crear la base de datos.</span><span class="sxs-lookup"><span data-stu-id="214b1-511">Trigger the sample app to create the database.</span></span> <span data-ttu-id="214b1-512">Realice una solicitud a `/createdatabase`.</span><span class="sxs-lookup"><span data-stu-id="214b1-512">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="214b1-513">La aplicación responde:</span><span class="sxs-lookup"><span data-stu-id="214b1-513">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="214b1-514">Realice una solicitud al punto de conexión `/health`.</span><span class="sxs-lookup"><span data-stu-id="214b1-514">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="214b1-515">La base de datos y el contexto existen, por lo que la aplicación responde:</span><span class="sxs-lookup"><span data-stu-id="214b1-515">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="214b1-516">Active la aplicación de ejemplo para eliminar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="214b1-516">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="214b1-517">Realice una solicitud a `/deletedatabase`.</span><span class="sxs-lookup"><span data-stu-id="214b1-517">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="214b1-518">La aplicación responde:</span><span class="sxs-lookup"><span data-stu-id="214b1-518">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="214b1-519">Realice una solicitud al punto de conexión `/health`.</span><span class="sxs-lookup"><span data-stu-id="214b1-519">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="214b1-520">La aplicación proporciona una respuesta incorrecta:</span><span class="sxs-lookup"><span data-stu-id="214b1-520">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="214b1-521">Sondeos de preparación y ejecución independientes</span><span class="sxs-lookup"><span data-stu-id="214b1-521">Separate readiness and liveness probes</span></span>

<span data-ttu-id="214b1-522">En algunos escenarios de hospedaje, se usa un par de comprobaciones de estado que distinguen los dos estados de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="214b1-522">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="214b1-523">La aplicación está funcionando, pero aún no está preparada para recibir solicitudes.</span><span class="sxs-lookup"><span data-stu-id="214b1-523">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="214b1-524">Este estado es la *preparación* de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="214b1-524">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="214b1-525">La aplicación está funcionando y responde a las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="214b1-525">The app is functioning and responding to requests.</span></span> <span data-ttu-id="214b1-526">Este estado es la *ejecución* de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="214b1-526">This state is the app's *liveness*.</span></span>

<span data-ttu-id="214b1-527">Normalmente, la comprobación de preparación realiza un conjunto más amplio de comprobaciones, que requieren mucho tiempo, para determinar si están disponibles todos los subsistemas y los recursos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="214b1-527">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="214b1-528">Una comprobación de ejecución simplemente realiza una comprobación rápida para determinar si la aplicación está disponible para procesar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="214b1-528">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="214b1-529">Después de que la aplicación pase la comprobación de preparación, no es necesario sobrecargar aún más la aplicación con el conjunto amplio de comprobaciones de preparación; en las comprobaciones siguientes solo se requiere comprobar la ejecución.</span><span class="sxs-lookup"><span data-stu-id="214b1-529">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="214b1-530">La aplicación de ejemplo contiene una comprobación de estado para notificar la finalización de la tarea de inicio de ejecución prolongada en un [servicio hospedado](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="214b1-530">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="214b1-531">El elemento `StartupHostedServiceHealthCheck` expone una propiedad, `StartupTaskCompleted`, que el servicio hospedado puede establecer en `true` al terminar su tarea de ejecución prolongada (*StartupHostedServiceHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="214b1-531">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

<span data-ttu-id="214b1-532">Un [servicio hospedado](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*) se encarga de iniciar la tarea en segundo plano de larga ejecución.</span><span class="sxs-lookup"><span data-stu-id="214b1-532">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="214b1-533">Al finalizar la tarea, `StartupHostedServiceHealthCheck.StartupTaskCompleted` se establece en `true`:</span><span class="sxs-lookup"><span data-stu-id="214b1-533">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="214b1-534">La comprobación de estado se registra con <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> en `Startup.ConfigureServices` junto con el servicio hospedado.</span><span class="sxs-lookup"><span data-stu-id="214b1-534">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="214b1-535">Dado que el servicio hospedado debe establecer la propiedad en la comprobación de estado, esta también se registra en el contenedor de servicios (*LivenessProbeStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="214b1-535">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="214b1-536">Llame al middleware de comprobaciones de estado en la canalización de procesamiento de la aplicación en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="214b1-536">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="214b1-537">En la aplicación de ejemplo, se crean puntos de conexión de la comprobación de estado en `/health/ready` para la comprobación de preparación y en `/health/live` para la comprobación de ejecución.</span><span class="sxs-lookup"><span data-stu-id="214b1-537">In the sample app, the health check endpoints are created at `/health/ready` for the readiness check and `/health/live` for the liveness check.</span></span> <span data-ttu-id="214b1-538">La comprobación de preparación filtra las comprobaciones de estado con la etiqueta `ready`.</span><span class="sxs-lookup"><span data-stu-id="214b1-538">The readiness check filters health checks to the health check with the `ready` tag.</span></span> <span data-ttu-id="214b1-539">La comprobación de ejecución filtra el elemento `StartupHostedServiceHealthCheck` y devuelve `false` en [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate). Para obtener más información, consulte [Filtrado de las comprobaciones de estado](#filter-health-checks):</span><span class="sxs-lookup"><span data-stu-id="214b1-539">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks)):</span></span>

```csharp
app.UseHealthChecks("/health/ready", new HealthCheckOptions()
{
    Predicate = (check) => check.Tags.Contains("ready"), 
});

app.UseHealthChecks("/health/live", new HealthCheckOptions()
{
    Predicate = (_) => false
});
```

<span data-ttu-id="214b1-540">Para ejecutar el escenario de configuración de la preparación/ejecución mediante la aplicación de ejemplo, ejecute el comando siguiente desde la carpeta del proyecto en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="214b1-540">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario liveness
```

<span data-ttu-id="214b1-541">En un explorador, visite `/health/ready` varias veces hasta que hayan pasado 15 segundos.</span><span class="sxs-lookup"><span data-stu-id="214b1-541">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="214b1-542">La comprobación de estado notifica un estado *Incorrecto* durante los primeros 15 segundos.</span><span class="sxs-lookup"><span data-stu-id="214b1-542">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="214b1-543">Pasados 15 segundos, el punto de conexión notifica un estado *Correcto*, lo que indica que el servicio hospedado ya ha finalizado la tarea de ejecución prolongada.</span><span class="sxs-lookup"><span data-stu-id="214b1-543">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="214b1-544">En este ejemplo también se crea un publicador de la comprobación de estado (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementación) que ejecuta la primera comprobación de preparación con un retraso de dos segundos.</span><span class="sxs-lookup"><span data-stu-id="214b1-544">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="214b1-545">Para obtener más información, consulte la sección [Publicador de la comprobación de estado](#health-check-publisher).</span><span class="sxs-lookup"><span data-stu-id="214b1-545">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="214b1-546">Ejemplo de Kubernetes</span><span class="sxs-lookup"><span data-stu-id="214b1-546">Kubernetes example</span></span>

<span data-ttu-id="214b1-547">Utilizar comprobaciones de preparación y ejecución independientes es útil en un entorno como [Kubernetes](https://kubernetes.io/).</span><span class="sxs-lookup"><span data-stu-id="214b1-547">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="214b1-548">En Kubernetes, es posible que una aplicación deba realizar un trabajo de inicio que requiera mucho tiempo antes de aceptar solicitudes, como una prueba de la disponibilidad de la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="214b1-548">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="214b1-549">El hecho de utilizar comprobaciones independientes permite que el orquestador distinga si la aplicación está funcionando, pero aún no esté preparada, o si la aplicación no se ha podido iniciar.</span><span class="sxs-lookup"><span data-stu-id="214b1-549">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="214b1-550">Para obtener más información sobre los sondeos de preparación y ejecución en Kubernetes, consulte [Configuración de sondeos de preparación y ejecución](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) en la documentación de Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="214b1-550">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="214b1-551">En el ejemplo siguiente, se muestra una configuración de sondeo de preparación de Kubernetes:</span><span class="sxs-lookup"><span data-stu-id="214b1-551">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="214b1-552">Sondeo basado en métrica con un escritor de respuesta personalizada</span><span class="sxs-lookup"><span data-stu-id="214b1-552">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="214b1-553">La aplicación de ejemplo muestra una comprobación de estado de memoria con un escritor de respuesta personalizada.</span><span class="sxs-lookup"><span data-stu-id="214b1-553">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="214b1-554">`MemoryHealthCheck` notifica un estado incorrecto si la aplicación usa más de un umbral de memoria determinado (1 GB en la aplicación de ejemplo).</span><span class="sxs-lookup"><span data-stu-id="214b1-554">`MemoryHealthCheck` reports an unhealthy status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="214b1-555">El elemento <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> incluye información del recolector de elementos no utilizados (GC) de la aplicación (*MemoryHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="214b1-555">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="214b1-556">Registre los servicios de comprobación de estado con <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> de `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="214b1-556">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="214b1-557">En lugar de pasar la comprobación de estado a <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> para habilitarla, `MemoryHealthCheck` se registra como servicio.</span><span class="sxs-lookup"><span data-stu-id="214b1-557">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="214b1-558">Todos los servicios registrados de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> están disponibles para los servicios de comprobación de estado y middleware.</span><span class="sxs-lookup"><span data-stu-id="214b1-558">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="214b1-559">Se recomienda registrar los servicios de comprobación de estado como los servicios de Singleton.</span><span class="sxs-lookup"><span data-stu-id="214b1-559">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="214b1-560">En la aplicación de ejemplo (*CustomWriterStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="214b1-560">In the sample app (*CustomWriterStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="214b1-561">Llame al middleware de comprobaciones de estado en la canalización de procesamiento de la aplicación en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="214b1-561">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="214b1-562">Se proporciona un delegado de `WriteResponse` a la propiedad `ResponseWriter` para generar una respuesta JSON personalizada al ejecutarse la comprobación de estado:</span><span class="sxs-lookup"><span data-stu-id="214b1-562">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // This custom writer formats the detailed status as JSON.
        ResponseWriter = WriteResponse
    });
}
```

<span data-ttu-id="214b1-563">El método `WriteResponse` da a `CompositeHealthCheckResult` el formato de objeto JSON y suspende el resultado de JSON para la respuesta de comprobación de estado:</span><span class="sxs-lookup"><span data-stu-id="214b1-563">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="214b1-564">Para ejecutar el sondeo basado en métrica con el resultado de un escritor de respuesta personalizada mediante la aplicación de ejemplo, ejecute el comando siguiente desde la carpeta del proyecto en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="214b1-564">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="214b1-565">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) incluye escenarios de comprobación de estado basados en métrica, como comprobaciones de almacenamiento del disco y de ejecución del máximo valor.</span><span class="sxs-lookup"><span data-stu-id="214b1-565">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="214b1-566">Microsoft no ofrece soporte técnico para [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) ni lo mantiene.</span><span class="sxs-lookup"><span data-stu-id="214b1-566">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="214b1-567">Filtrado por puerto</span><span class="sxs-lookup"><span data-stu-id="214b1-567">Filter by port</span></span>

<span data-ttu-id="214b1-568">Una llamada a <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> con un puerto restringe las solicitudes de comprobación de estado al puerto especificado.</span><span class="sxs-lookup"><span data-stu-id="214b1-568">Calling <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> with a port restricts health check requests to the port specified.</span></span> <span data-ttu-id="214b1-569">Esto se usa normalmente en un entorno de contenedor para exponer un puerto para los servicios de supervisión.</span><span class="sxs-lookup"><span data-stu-id="214b1-569">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="214b1-570">La aplicación de ejemplo configura el puerto con el [proveedor de configuración de variable de entorno](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="214b1-570">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="214b1-571">El puerto se establece en el archivo *launchSettings.json* y se pasa al proveedor de configuración a través de una variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="214b1-571">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="214b1-572">También debe configurar el servidor para que escuche las solicitudes en el puerto de administración.</span><span class="sxs-lookup"><span data-stu-id="214b1-572">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="214b1-573">Para utilizar la aplicación de ejemplo para que muestre la configuración del puerto de administración, cree el archivo *launchSettings.json* en una carpeta *Propiedades*.</span><span class="sxs-lookup"><span data-stu-id="214b1-573">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="214b1-574">El siguiente archivo *Properties/launchSettings.json* de la aplicación de ejemplo no se incluye en los archivos de proyecto de la aplicación de ejemplo y debe crearse manualmente:</span><span class="sxs-lookup"><span data-stu-id="214b1-574">The following *Properties/launchSettings.json* file in the sample app isn't included in the sample app's project files and must be created manually:</span></span>

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

<span data-ttu-id="214b1-575">Registre los servicios de comprobación de estado con <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> de `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="214b1-575">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="214b1-576">La llamada a <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> especifica el puerto de administración (*ManagementPortStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="214b1-576">The call to <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> specifies the management port (*ManagementPortStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=17)]

> [!NOTE]
> <span data-ttu-id="214b1-577">Para evitar la creación del archivo *launchSettings.json* en la aplicación de ejemplo, configure las direcciones URL y el puerto de administración explícitamente en código.</span><span class="sxs-lookup"><span data-stu-id="214b1-577">You can avoid creating the *launchSettings.json* file in the sample app by setting the URLs and management port explicitly in code.</span></span> <span data-ttu-id="214b1-578">En *Program.cs*, donde se crea <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, agregue una llamada a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> y proporcione el punto de conexión de respuesta normal de la aplicación y el punto de conexión del puerto de administración.</span><span class="sxs-lookup"><span data-stu-id="214b1-578">In *Program.cs* where the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> and provide the app's normal response endpoint and the management port endpoint.</span></span> <span data-ttu-id="214b1-579">En *ManagementPortStartup.cs*, donde se llama a <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>, especifique explícitamente el puerto de administración.</span><span class="sxs-lookup"><span data-stu-id="214b1-579">In *ManagementPortStartup.cs* where <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> is called, specify the management port explicitly.</span></span>
>
> <span data-ttu-id="214b1-580">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="214b1-580">*Program.cs*:</span></span>
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
> <span data-ttu-id="214b1-581">*ManagementPortStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="214b1-581">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseHealthChecks("/health", port: 5001);
> ```

<span data-ttu-id="214b1-582">Para ejecutar el escenario de configuración del puerto de administración mediante la aplicación de ejemplo, ejecute el comando siguiente desde la carpeta del proyecto en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="214b1-582">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="214b1-583">Distribución de una biblioteca de comprobación de estado</span><span class="sxs-lookup"><span data-stu-id="214b1-583">Distribute a health check library</span></span>

<span data-ttu-id="214b1-584">Para distribuir una comprobación de estado como una biblioteca, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="214b1-584">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="214b1-585">Escriba una comprobación de estado que implemente la interfaz de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> como una clase independiente.</span><span class="sxs-lookup"><span data-stu-id="214b1-585">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="214b1-586">La clase puede depender de la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection), de la activación del tipo y de las [opciones denominadas](xref:fundamentals/configuration/options) para acceder a los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="214b1-586">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   <span data-ttu-id="214b1-587">En la lógica de comprobaciones de estado de `CheckHealthAsync`:</span><span class="sxs-lookup"><span data-stu-id="214b1-587">In the health checks logic of `CheckHealthAsync`:</span></span>

   * <span data-ttu-id="214b1-588">`data1` y `data2` se usan en el método para ejecutar la lógica de comprobación de estado del sondeo.</span><span class="sxs-lookup"><span data-stu-id="214b1-588">`data1` and `data2` are used in the method to run the probe's health check logic.</span></span>
   * <span data-ttu-id="214b1-589">Se controla `AccessViolationException`.</span><span class="sxs-lookup"><span data-stu-id="214b1-589">`AccessViolationException` is handled.</span></span>

   <span data-ttu-id="214b1-590">Cuando se produce un <xref:System.AccessViolationException>, se devuelve <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> con <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> para permitir que los usuarios configuren el estado de error de las comprobaciones de estado.</span><span class="sxs-lookup"><span data-stu-id="214b1-590">When an <xref:System.AccessViolationException> occurs, the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> is returned with the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> to allow users to configure the health checks failure status.</span></span>

   ```csharp
   using System;
   using System.Threading;
   using System.Threading.Tasks;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

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
   ```

1. <span data-ttu-id="214b1-591">Escriba un método de extensión con los parámetros a los que la aplicación de uso llama en su método `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="214b1-591">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="214b1-592">En el ejemplo siguiente, suponga que existe la siguiente firma del método de comprobación de estado:</span><span class="sxs-lookup"><span data-stu-id="214b1-592">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="214b1-593">La firma anterior indica que la `ExampleHealthCheck` requiere datos adicionales para procesar la lógica de sondeo de la comprobación de estado.</span><span class="sxs-lookup"><span data-stu-id="214b1-593">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="214b1-594">Los datos se proporcionan al delegado que se usa para crear la instancia de la comprobación de estado cuando la comprobación de estado se registra con un método de extensión.</span><span class="sxs-lookup"><span data-stu-id="214b1-594">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="214b1-595">En el ejemplo siguiente, el autor de llamada especifica los siguientes elementos opcionales:</span><span class="sxs-lookup"><span data-stu-id="214b1-595">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="214b1-596">nombre de la comprobación de estado (`name`).</span><span class="sxs-lookup"><span data-stu-id="214b1-596">health check name (`name`).</span></span> <span data-ttu-id="214b1-597">En el caso de `null`, se utiliza `example_health_check`.</span><span class="sxs-lookup"><span data-stu-id="214b1-597">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="214b1-598">punto de datos de cadena para la comprobación de estado (`data1`).</span><span class="sxs-lookup"><span data-stu-id="214b1-598">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="214b1-599">punto de datos enteros para la comprobación de estado (`data2`).</span><span class="sxs-lookup"><span data-stu-id="214b1-599">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="214b1-600">En el caso de `null`, se utiliza `1`.</span><span class="sxs-lookup"><span data-stu-id="214b1-600">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="214b1-601">estado de error (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span><span class="sxs-lookup"><span data-stu-id="214b1-601">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="214b1-602">De manera predeterminada, es `null`.</span><span class="sxs-lookup"><span data-stu-id="214b1-602">The default is `null`.</span></span> <span data-ttu-id="214b1-603">Si `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) se notifica para un estado de error.</span><span class="sxs-lookup"><span data-stu-id="214b1-603">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="214b1-604">etiquetas (`IEnumerable<string>`).</span><span class="sxs-lookup"><span data-stu-id="214b1-604">tags (`IEnumerable<string>`).</span></span>

   ```csharp
   using System.Collections.Generic;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   public static class ExampleHealthCheckBuilderExtensions
   {
       const string DefaultName = "example_health_check";

       public static IHealthChecksBuilder AddExampleHealthCheck(
           this IHealthChecksBuilder builder,
           string name = default,
           string data1,
           int data2 = 1,
           HealthStatus? failureStatus = default,
           IEnumerable<string> tags = default)
       {
           return builder.Add(new HealthCheckRegistration(
               name ?? DefaultName,
               sp => new ExampleHealthCheck(data1, data2),
               failureStatus,
               tags));
       }
   }
   ```

## <a name="health-check-publisher"></a><span data-ttu-id="214b1-605">Publicador de la comprobación de estado</span><span class="sxs-lookup"><span data-stu-id="214b1-605">Health Check Publisher</span></span>

<span data-ttu-id="214b1-606">Cuando un elemento <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> se agrega al contenedor de servicios, el sistema de comprobación de estado ejecuta periódicamente las comprobaciones de estado y llama a `PublishAsync` con el resultado.</span><span class="sxs-lookup"><span data-stu-id="214b1-606">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="214b1-607">Esto es útil en un escenario de sistema de seguimiento de estado basado en inserción en el que se espera que cada proceso llame periódicamente al sistema de seguimiento con el fin de determinar el estado.</span><span class="sxs-lookup"><span data-stu-id="214b1-607">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="214b1-608">La interfaz de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> tiene un único método:</span><span class="sxs-lookup"><span data-stu-id="214b1-608">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="214b1-609"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> le permiten establecer:</span><span class="sxs-lookup"><span data-stu-id="214b1-609"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="214b1-610"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>&ndash; El retraso inicial aplicado tras iniciarse la aplicación antes de ejecutar instancias de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="214b1-610"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="214b1-611">El retraso se aplica una vez durante el inicio y no se aplica a las iteraciones posteriores.</span><span class="sxs-lookup"><span data-stu-id="214b1-611">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="214b1-612">El valor predeterminado es cinco segundos.</span><span class="sxs-lookup"><span data-stu-id="214b1-612">The default value is five seconds.</span></span>
* <span data-ttu-id="214b1-613"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period>&ndash; El período de ejecución de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="214b1-613"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="214b1-614">El valor predeterminado es 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="214b1-614">The default value is 30 seconds.</span></span>
* <span data-ttu-id="214b1-615"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; Si <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> es `null` (valor predeterminado), el servicio de publicador de la comprobación de estado ejecuta todas las comprobaciones de estado registradas.</span><span class="sxs-lookup"><span data-stu-id="214b1-615"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="214b1-616">Para ejecutar un subconjunto de comprobaciones de estado, proporcione una función que filtre el conjunto de comprobaciones.</span><span class="sxs-lookup"><span data-stu-id="214b1-616">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="214b1-617">El predicado se evalúa cada período.</span><span class="sxs-lookup"><span data-stu-id="214b1-617">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="214b1-618"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; El tiempo de expiración para ejecutar las comprobaciones de estado para todas las instancias de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="214b1-618"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="214b1-619">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> para ejecutar sin tiempo de expiración.</span><span class="sxs-lookup"><span data-stu-id="214b1-619">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="214b1-620">El valor predeterminado es 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="214b1-620">The default value is 30 seconds.</span></span>

> [!WARNING]
> <span data-ttu-id="214b1-621">En la versión de ASP.NET Core 2.2, el establecimiento de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> no se asigna por medio de la implementación de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>; establece el valor de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span><span class="sxs-lookup"><span data-stu-id="214b1-621">In the ASP.NET Core 2.2 release, setting <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> isn't honored by the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation; it sets the value of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span></span> <span data-ttu-id="214b1-622">Este problema se ha corregido en ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="214b1-622">This issue has been addressed in ASP.NET Core 3.0.</span></span>

<span data-ttu-id="214b1-623">En la aplicación de ejemplo, `ReadinessPublisher` es una implementación de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="214b1-623">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="214b1-624">El estado de comprobación de estado se registra para cada comprobación como:</span><span class="sxs-lookup"><span data-stu-id="214b1-624">The health check status is logged for each check as either:</span></span>

* <span data-ttu-id="214b1-625">Información (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>) si el estado de las comprobaciones de estado es <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>.</span><span class="sxs-lookup"><span data-stu-id="214b1-625">Information (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>) if the health checks status is <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>.</span></span>
* <span data-ttu-id="214b1-626">Error (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>) si el estado es <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> o <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>.</span><span class="sxs-lookup"><span data-stu-id="214b1-626">Error (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>) if the status is either <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> or <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>.</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=18-27)]

<span data-ttu-id="214b1-627">En el ejemplo `LivenessProbeStartup` de la aplicación de ejemplo, la comprobación de preparación `StartupHostedService` tiene un retraso de inicio de dos segundos y ejecuta la comprobación cada 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="214b1-627">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="214b1-628">Para activar la implementación de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>, el ejemplo registra `ReadinessPublisher` como servicio singleton en el contenedor de [inserción de dependencias (DI)](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="214b1-628">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices&highlight=12-17,28)]

> [!NOTE]
> <span data-ttu-id="214b1-629">La siguiente solución alternativa permite la adición de una instancia de <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> al contenedor del servicio cuando uno o más servicios hospedados ya se han agregado a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="214b1-629">The following workaround permits adding an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instance to the service container when one or more other hosted services have already been added to the app.</span></span> <span data-ttu-id="214b1-630">Esta solución no es necesaria en ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="214b1-630">This workaround won't isn't required in ASP.NET Core 3.0.</span></span>
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

> [!NOTE]
> <span data-ttu-id="214b1-631">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) incluye editores para varios sistemas, como [Application Insights](/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="214b1-631">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="214b1-632">Microsoft no ofrece soporte técnico para [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) ni lo mantiene.</span><span class="sxs-lookup"><span data-stu-id="214b1-632">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="restrict-health-checks-with-mapwhen"></a><span data-ttu-id="214b1-633">Restricción de las comprobaciones de estado con MapWhen</span><span class="sxs-lookup"><span data-stu-id="214b1-633">Restrict health checks with MapWhen</span></span>

<span data-ttu-id="214b1-634">Use <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> para crear de forma condicional una rama de la canalización de solicitudes para los puntos de conexión de comprobación del estado.</span><span class="sxs-lookup"><span data-stu-id="214b1-634">Use <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> to conditionally branch the request pipeline for health check endpoints.</span></span>

<span data-ttu-id="214b1-635">En el ejemplo siguiente, `MapWhen` crea una rama de la canalización de solicitudes para activar el middleware de comprobaciones de estado si se recibe una solicitud GET para el punto de conexión `api/HealthCheck`:</span><span class="sxs-lookup"><span data-stu-id="214b1-635">In the following example, `MapWhen` branches the request pipeline to activate Health Checks Middleware if a GET request is received for the `api/HealthCheck` endpoint:</span></span>

```csharp
app.MapWhen(
    context => context.Request.Method == HttpMethod.Get.Method && 
        context.Request.Path.StartsWith("/api/HealthCheck"),
    builder => builder.UseHealthChecks());

app.UseMvc();
```

<span data-ttu-id="214b1-636">Para obtener más información, vea <xref:fundamentals/middleware/index#use-run-and-map>.</span><span class="sxs-lookup"><span data-stu-id="214b1-636">For more information, see <xref:fundamentals/middleware/index#use-run-and-map>.</span></span>

::: moniker-end
