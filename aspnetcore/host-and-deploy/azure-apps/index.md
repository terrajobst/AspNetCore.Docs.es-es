---
title: Implementar aplicaciones de ASP.NET Core en Azure App Service
author: guardrex
description: Este artículo contiene vínculos a recursos de implementación y hospedaje de Azure.
ms.author: riande
ms.custom: mvc
ms.date: 08/29/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: f2de81af4bd2992aec76a287484d0057021231d8
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2018
ms.locfileid: "47860971"
---
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a><span data-ttu-id="e231e-103">Implementar aplicaciones de ASP.NET Core en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e231e-103">Deploy ASP.NET Core apps to Azure App Service</span></span>

<span data-ttu-id="e231e-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) es un [servicio de plataforma de informática en la nube de Microsoft](https://azure.microsoft.com/) que sirve para hospedar aplicaciones web, como ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e231e-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="e231e-105">Recursos útiles</span><span class="sxs-lookup"><span data-stu-id="e231e-105">Useful resources</span></span>

<span data-ttu-id="e231e-106">La [documentación de Web Apps](/azure/app-service/) de Azure es un recurso que incluye documentación, tutoriales, ejemplos, guías de procedimientos y otros recursos de aplicaciones de Azure.</span><span class="sxs-lookup"><span data-stu-id="e231e-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="e231e-107">Dos tutoriales importantes que pertenecen al hospedaje de aplicaciones de ASP.NET Core son:</span><span class="sxs-lookup"><span data-stu-id="e231e-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="e231e-108">Inicio rápido: Crear una aplicación web de ASP.NET Core en Azure</span><span class="sxs-lookup"><span data-stu-id="e231e-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="e231e-109">Usar Visual Studio para crear e implementar una aplicación web de ASP.NET Core para Azure App Service en Windows.</span><span class="sxs-lookup"><span data-stu-id="e231e-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="e231e-110">Inicio rápido: Crear una aplicación web de .NET Core en App Service en Linux</span><span class="sxs-lookup"><span data-stu-id="e231e-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="e231e-111">Usar la línea de comandos para crear e implementar una aplicación web de ASP.NET Core para Azure App Service en Linux.</span><span class="sxs-lookup"><span data-stu-id="e231e-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="e231e-112">Los artículos siguientes están disponibles en la documentación de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="e231e-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="e231e-113">Publicación en Azure con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e231e-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="e231e-114">Obtenga información sobre cómo publicar una aplicación de ASP.NET Core en Azure App Service con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e231e-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="e231e-115">Implementación continua en Azure con Visual Studio y Git</span><span class="sxs-lookup"><span data-stu-id="e231e-115">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="e231e-116">Obtenga información sobre cómo crear una aplicación web de ASP.NET Core con Visual Studio e implementarla en Azure App Service con Git para una implementación continua.</span><span class="sxs-lookup"><span data-stu-id="e231e-116">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="e231e-117">Creación de la primera canalización con Azure Pipelines</span><span class="sxs-lookup"><span data-stu-id="e231e-117">Create your first pipeline with Azure Pipelines</span></span>](/azure/devops/pipelines/get-started-yaml)  
<span data-ttu-id="e231e-118">Configure una compilación de integración continua para una aplicación de ASP.NET Core y, después, cree una versión de implementación continua para Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="e231e-118">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="e231e-119">Espacio aislado de Azure Web App</span><span class="sxs-lookup"><span data-stu-id="e231e-119">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="e231e-120">Detecte limitaciones de ejecución en tiempo de ejecución de Azure App Service aplicadas por la plataforma de aplicaciones de Azure.</span><span class="sxs-lookup"><span data-stu-id="e231e-120">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a><span data-ttu-id="e231e-121">Configuración de aplicación</span><span class="sxs-lookup"><span data-stu-id="e231e-121">Application configuration</span></span>

<span data-ttu-id="e231e-122">En ASP.NET Core 2.0 y versiones posteriores, los siguientes paquetes de NuGet proporcionan características de registro automáticas para aplicaciones implementadas en Azure App Service:</span><span class="sxs-lookup"><span data-stu-id="e231e-122">In ASP.NET Core 2.0 or later, the following NuGet packages provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="e231e-123">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) usa [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) para proporcionar a ASP.NET Core una integración ligera (Light-Up) con Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="e231e-123">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="e231e-124">El paquete `Microsoft.AspNetCore.AzureAppServicesIntegration` proporciona las características de registro agregadas.</span><span class="sxs-lookup"><span data-stu-id="e231e-124">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="e231e-125">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) ejecuta [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) para agregar a Azure App Service proveedores de registro de diagnósticos en el paquete `Microsoft.Extensions.Logging.AzureAppServices`.</span><span class="sxs-lookup"><span data-stu-id="e231e-125">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="e231e-126">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) proporciona implementaciones de registrador para admitir registros de diagnósticos de Azure App Service y características de transmisión en secuencias de registro.</span><span class="sxs-lookup"><span data-stu-id="e231e-126">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="e231e-127">Si tiene .NET Core como destino y hace referencia al [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage), los paquetes ya están incluidos.</span><span class="sxs-lookup"><span data-stu-id="e231e-127">If targeting .NET Core and referencing the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), the packages are already included.</span></span> <span data-ttu-id="e231e-128">Los paquetes no están en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), que es más reciente.</span><span class="sxs-lookup"><span data-stu-id="e231e-128">The packages are absent from the newer [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="e231e-129">Si tiene .NET Framework como destino o hace referencia al metapaquete `Microsoft.AspNetCore.App`, haga referencia a los paquetes de registro individuales.</span><span class="sxs-lookup"><span data-stu-id="e231e-129">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, reference the individual logging packages.</span></span>

::: moniker-end

## <a name="override-app-configuration-using-the-azure-portal"></a><span data-ttu-id="e231e-130">Invalidación de la configuración de la aplicación mediante Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e231e-130">Override app configuration using the Azure Portal</span></span>

<span data-ttu-id="e231e-131">El área **Configuración de la aplicación** de la hoja **Configuración de la aplicación** le permite establecer variables de entorno para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e231e-131">The **App Settings** area of the **Application settings** blade permits you to set environment variables for the app.</span></span> <span data-ttu-id="e231e-132">El [proveedor de configuración de variables de entorno](xref:fundamentals/configuration/index#environment-variables-configuration-provider) puede consumir las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="e231e-132">Environment variables can be consumed by the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="e231e-133">Cuando una aplicación usa el [host web](xref:fundamentals/host/web-host) y compila el host mediante [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), las variables de entorno que configuran el host usan el prefijo `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="e231e-133">When an app uses the [Web Host](xref:fundamentals/host/web-host) and builds the host using [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), environment variables that configure the host use the `ASPNETCORE_` prefix.</span></span> <span data-ttu-id="e231e-134">Para más información, consulte <xref:fundamentals/host/web-host> y el [proveedor de configuración de variables de entorno](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e231e-134">For more information, see <xref:fundamentals/host/web-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="e231e-135">Cuando una aplicación usa el [host genérico](xref:fundamentals/host/generic-host), no se cargan variables de entorno en la configuración de una aplicación de manera predeterminada y es el desarrollador el que debe agregar al proveedor de configuración.</span><span class="sxs-lookup"><span data-stu-id="e231e-135">When an app uses the [Generic Host](xref:fundamentals/host/generic-host), environment variables aren't loaded into an app's configuration by default and the configuration provider must be added by the developer.</span></span> <span data-ttu-id="e231e-136">El desarrollador determina el prefijo de variable de entorno cuando se agrega el proveedor de configuración.</span><span class="sxs-lookup"><span data-stu-id="e231e-136">The developer determines the environment variable prefix when the configuration provider is added.</span></span> <span data-ttu-id="e231e-137">Para más información, consulte <xref:fundamentals/host/generic-host> y el [proveedor de configuración de variables de entorno](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e231e-137">For more information, see <xref:fundamentals/host/generic-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="e231e-138">Escenarios de servidor proxy y equilibrador de carga</span><span class="sxs-lookup"><span data-stu-id="e231e-138">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="e231e-139">El software intermedio de integración con IIS, que configura el software intermedio de encabezados reenviados, y el módulo de ASP.NET Core están configurados para reenviar el esquema (HTTP/HTTPS) y la dirección IP remota donde se originó la solicitud.</span><span class="sxs-lookup"><span data-stu-id="e231e-139">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="e231e-140">Podría ser necesario realizar una configuración adicional para las aplicaciones hospedadas detrás de servidores proxy y equilibradores de carga adicionales.</span><span class="sxs-lookup"><span data-stu-id="e231e-140">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="e231e-141">Para más información, vea [Configurar ASP.NET Core para trabajar con servidores proxy y equilibradores de carga](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="e231e-141">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="e231e-142">Supervisión y registro</span><span class="sxs-lookup"><span data-stu-id="e231e-142">Monitoring and logging</span></span>

<span data-ttu-id="e231e-143">Para obtener información sobre supervisión, registro y solución de problemas, consulte los artículos siguientes:</span><span class="sxs-lookup"><span data-stu-id="e231e-143">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="e231e-144">Cómo: Supervisar aplicaciones en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e231e-144">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="e231e-145">Obtenga información sobre cómo revisar las cuotas y las métricas para las aplicaciones y los planes de App Service.</span><span class="sxs-lookup"><span data-stu-id="e231e-145">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="e231e-146">Habilitar el registro de diagnósticos para las aplicaciones web en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e231e-146">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="e231e-147">Descubra cómo habilitar y acceder a registro de diagnóstico para los códigos de estado HTTP, solicitudes con error y actividad del servidor web.</span><span class="sxs-lookup"><span data-stu-id="e231e-147">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="e231e-148">Introducción a control de errores en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e231e-148">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="e231e-149">Conozca los métodos habituales para controlar los errores en las aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e231e-149">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="e231e-150">Solución de problemas de ASP.NET Core en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e231e-150">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="e231e-151">Obtenga información sobre cómo diagnosticar problemas con las implementaciones de Azure App Service con las aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e231e-151">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="e231e-152">Referencia de errores comunes de Azure App Service e IIS con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e231e-152">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="e231e-153">Consulte los errores comunes de configuración de implementación para las aplicaciones hospedadas por Azure App Service/IIS con consejos de solución de problemas.</span><span class="sxs-lookup"><span data-stu-id="e231e-153">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="e231e-154">Anillo de clave de protección de datos y ranuras de implementación</span><span class="sxs-lookup"><span data-stu-id="e231e-154">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="e231e-155">Las [claves de protección de datos](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) se conservan en la carpeta *%HOME%\ASP.NET\DataProtection-Keys*.</span><span class="sxs-lookup"><span data-stu-id="e231e-155">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="e231e-156">Esta carpeta está respaldada por el almacenamiento de red y se sincroniza en todas las máquinas que hospedan la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e231e-156">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="e231e-157">Las claves no están protegidas en reposo.</span><span class="sxs-lookup"><span data-stu-id="e231e-157">Keys aren't protected at rest.</span></span> <span data-ttu-id="e231e-158">Esta carpeta proporciona el anillo de clave a todas las instancias de una aplicación en una única ranura de implementación.</span><span class="sxs-lookup"><span data-stu-id="e231e-158">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="e231e-159">Las ranuras de implementación independientes, por ejemplo, almacenamiento provisional y producción, no comparten ningún anillo de clave.</span><span class="sxs-lookup"><span data-stu-id="e231e-159">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="e231e-160">Al realizar un intercambio entre ranuras de implementación, cualquier sistema que utilice la protección de datos no podrá descifrar los datos almacenados mediante el anillo de clave situado dentro de la ranura anterior.</span><span class="sxs-lookup"><span data-stu-id="e231e-160">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="e231e-161">El middleware de cookies de ASP.NET usa protección de datos para proteger sus cookies.</span><span class="sxs-lookup"><span data-stu-id="e231e-161">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="e231e-162">Esto hace que los usuarios cierren sesión en una aplicación que usa el middleware de cookies de ASP.NET estándar.</span><span class="sxs-lookup"><span data-stu-id="e231e-162">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="e231e-163">Para una solución de anillo de clave independiente de la ranura, utilice un proveedor de anillo de clave externo, como estos:</span><span class="sxs-lookup"><span data-stu-id="e231e-163">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="e231e-164">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="e231e-164">Azure Blob Storage</span></span>
* <span data-ttu-id="e231e-165">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e231e-165">Azure Key Vault</span></span>
* <span data-ttu-id="e231e-166">Almacén SQL</span><span class="sxs-lookup"><span data-stu-id="e231e-166">SQL store</span></span>
* <span data-ttu-id="e231e-167">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="e231e-167">Redis cache</span></span>

<span data-ttu-id="e231e-168">Para obtener más información, consulte [Proveedores de almacenamiento de claves](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="e231e-168">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="e231e-169">Implementar una versión preliminar de ASP.NET Core en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e231e-169">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="e231e-170">Las aplicaciones de versión preliminar de ASP.NET Core se pueden implementar en Azure App Service con los procedimientos siguientes:</span><span class="sxs-lookup"><span data-stu-id="e231e-170">ASP.NET Core preview apps can be deployed to Azure App Service with the following approaches:</span></span>

* <span data-ttu-id="e231e-171">[Instalación de la extensión de sitio de versión preliminar](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span><span class="sxs-lookup"><span data-stu-id="e231e-171">[Install the preview site extension](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span></span>
* [<span data-ttu-id="e231e-172">Usar Docker con Web Apps para contenedores</span><span class="sxs-lookup"><span data-stu-id="e231e-172">Use Docker with Web Apps for containers</span></span>](#use-docker-with-web-apps-for-containers)

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="e231e-173">Instalación de la extensión de sitio de versión preliminar</span><span class="sxs-lookup"><span data-stu-id="e231e-173">Install the preview site extension</span></span>

<span data-ttu-id="e231e-174">Si tiene algún problema al usar la extensión de sitio de versión preliminar, abra una incidencia en [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span><span class="sxs-lookup"><span data-stu-id="e231e-174">If a problem occurs using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

1. <span data-ttu-id="e231e-175">En Azure Portal, vaya a la hoja de App Service.</span><span class="sxs-lookup"><span data-stu-id="e231e-175">From the Azure portal, navigate to the App Service blade.</span></span>
1. <span data-ttu-id="e231e-176">Seleccione la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="e231e-176">Select the web app.</span></span>
1. <span data-ttu-id="e231e-177">Escriba "ex" en el cuadro de búsqueda o desplácese hacia abajo en la lista de secciones de administración hasta llegar a **HERRAMIENTAS DE DESARROLLO**.</span><span class="sxs-lookup"><span data-stu-id="e231e-177">Type "ex" in the search box or scroll down the list of management sections to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="e231e-178">Seleccione **HERRAMIENTAS DE DESARROLLO** > **Extensiones**.</span><span class="sxs-lookup"><span data-stu-id="e231e-178">Select **DEVELOPMENT TOOLS** > **Extensions**.</span></span>
1. <span data-ttu-id="e231e-179">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="e231e-179">Select **Add**.</span></span>
1. <span data-ttu-id="e231e-180">Seleccione la extensión **Runtime de ASP.NET Core &lt;x.y&gt; (x86)** en la lista, donde `<x.y>` es la versión preliminar de ASP.NET Core (por ejemplo, **Runtime de ASP.NET Core 2.2 (x86)**).</span><span class="sxs-lookup"><span data-stu-id="e231e-180">Select the **ASP.NET Core &lt;x.y&gt; (x86) Runtime** extension from the list, where `<x.y>` is the ASP.NET Core preview version (for example, **ASP.NET Core 2.2 (x86) Runtime**).</span></span> <span data-ttu-id="e231e-181">Runtime de x86 es adecuado para [implementaciones dependientes del marco](/dotnet/core/deploying/#framework-dependent-deployments-fdd), que se basan en el hospedaje fuera de proceso proporcionado por el módulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e231e-181">The x86 runtime is appropriate for [framework-dependent deployments](/dotnet/core/deploying/#framework-dependent-deployments-fdd) that rely on out-of-process hosting by the ASP.NET Core Module.</span></span>
1. <span data-ttu-id="e231e-182">Seleccione **Aceptar** para aceptar los términos legales.</span><span class="sxs-lookup"><span data-stu-id="e231e-182">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="e231e-183">Seleccione **Aceptar** para instalar la extensión.</span><span class="sxs-lookup"><span data-stu-id="e231e-183">Select **OK** to install the extension.</span></span>

<span data-ttu-id="e231e-184">Cuando se complete la operación, se instalará la versión preliminar de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e231e-184">When the operation completes, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="e231e-185">Compruebe la instalación:</span><span class="sxs-lookup"><span data-stu-id="e231e-185">Verify the installation:</span></span>

1. <span data-ttu-id="e231e-186">Seleccione **Herramientas avanzadas** en **HERRAMIENTAS DE DESARROLLO**.</span><span class="sxs-lookup"><span data-stu-id="e231e-186">Select **Advanced Tools** under **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="e231e-187">Seleccione **Ir** en la hoja **Herramientas avanzadas**.</span><span class="sxs-lookup"><span data-stu-id="e231e-187">Select **Go** on the **Advanced Tools** blade.</span></span>
1. <span data-ttu-id="e231e-188">Seleccione el elemento de menú **Consola de depuración** > **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="e231e-188">Select the **Debug console** > **PowerShell** menu item.</span></span>
1. <span data-ttu-id="e231e-189">Ejecute el siguiente comando en el símbolo del sistema de PowerShell:</span><span class="sxs-lookup"><span data-stu-id="e231e-189">At the PowerShell prompt, execute the following command.</span></span> <span data-ttu-id="e231e-190">Sustituya la versión de runtime de ASP.NET Core por `<x.y>` en el comando:</span><span class="sxs-lookup"><span data-stu-id="e231e-190">Substitute the ASP.NET Core runtime version for `<x.y>` in the command:</span></span>

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x86\
   ```
   <span data-ttu-id="e231e-191">Si el runtime de la versión preliminar instalada es para ASP.NET Core 2.2, el comando será:</span><span class="sxs-lookup"><span data-stu-id="e231e-191">If the installed preview runtime is for ASP.NET Core 2.2, the command is:</span></span>
   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x86\
   ```
   <span data-ttu-id="e231e-192">El comando devuelve `True` cuando está instalado el runtime de la versión preliminar de x64.</span><span class="sxs-lookup"><span data-stu-id="e231e-192">The command returns `True` when the x64 preview runtime is installed.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="e231e-193">La arquitectura de plataforma (x86/x64) de una aplicación de App Services se configura en la hoja **Configuración de la aplicación**, en **Configuración general**, para las aplicaciones hospedadas en un cálculo de serie A o en un mejor nivel de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="e231e-193">The platform architecture (x86/x64) of an App Services app is set in the **Application Settings** blade under **General Settings** for apps that are hosted on an A-series compute or better hosting tier.</span></span> <span data-ttu-id="e231e-194">Si la aplicación se ejecuta en modo de en proceso y la arquitectura de plataforma está configurada para 64 bits (x64), el módulo ASP.NET Core usa el runtime de la versión preliminar de 64 bits, si está presente.</span><span class="sxs-lookup"><span data-stu-id="e231e-194">If the app is run in in-process mode and the platform architecture is configured for 64-bit (x64), the ASP.NET Core Module uses the 64-bit preview runtime, if present.</span></span> <span data-ttu-id="e231e-195">Instale la extensión **Runtime de ASP.NET Core &lt;x.y&gt; (x64)** (por ejemplo, **Runtime de ASP.NET Core 2.2 (x64)**).</span><span class="sxs-lookup"><span data-stu-id="e231e-195">Install the **ASP.NET Core &lt;x.y&gt; (x64) Runtime** extension (for example, **ASP.NET Core 2.2 (x64) Runtime**).</span></span>
>
> <span data-ttu-id="e231e-196">Después de instalar el runtime de la versión preliminar de x64, ejecute el siguiente comando en la ventana de comandos de Kudu PowerShell para comprobar la instalación.</span><span class="sxs-lookup"><span data-stu-id="e231e-196">After installing the x64 preview runtime, run the following command in the Kudu PowerShell command window to verify the installation.</span></span> <span data-ttu-id="e231e-197">Sustituya la versión de runtime de ASP.NET Core por `<x.y>` en el comando:</span><span class="sxs-lookup"><span data-stu-id="e231e-197">Substitute the ASP.NET Core runtime version for `<x.y>` in the command:</span></span>
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x64\
> ```
> <span data-ttu-id="e231e-198">Si el runtime de la versión preliminar instalada es para ASP.NET Core 2.2, el comando será:</span><span class="sxs-lookup"><span data-stu-id="e231e-198">If the installed preview runtime is for ASP.NET Core 2.2, the command is:</span></span>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x64\
> ```
> <span data-ttu-id="e231e-199">El comando devuelve `True` cuando está instalado el runtime de la versión preliminar de x64.</span><span class="sxs-lookup"><span data-stu-id="e231e-199">The command returns `True` when the x64 preview runtime is installed.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="e231e-200">Con **Extensiones de ASP.NET Core** se obtienen funciones adicionales para ASP.NET Core en Azure App Services, como habilitar el registro de Azure.</span><span class="sxs-lookup"><span data-stu-id="e231e-200">**ASP.NET Core Extensions** enables additional functionality for ASP.NET Core on Azure App Services, such as enabling Azure logging.</span></span> <span data-ttu-id="e231e-201">La extensión se instala automáticamente cuando se implementa desde Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e231e-201">The extension is installed automatically when deploying from Visual Studio.</span></span> <span data-ttu-id="e231e-202">Si no se instala la extensión, instálela para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e231e-202">If the extension isn't installed, install it for the app.</span></span>

<span data-ttu-id="e231e-203">**Uso de la extensión de sitio de versión preliminar con una plantilla de ARM**</span><span class="sxs-lookup"><span data-stu-id="e231e-203">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="e231e-204">Si usa una plantilla de ARM para crear e implementar aplicaciones, puede usar el tipo de recurso `siteextensions` para agregar la extensión de sitio a una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="e231e-204">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="e231e-205">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="e231e-205">For example:</span></span>

[!code-json[Main](index/sample/arm.json?highlight=2)]

<!--
### Deploy the app self-contained

A [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) can be deployed that carries the preview runtime in the deployment. When deploying a self-contained app:

* The site doesn't need to be prepared.
* The app must be published differently than when publishing for a framework-dependent deployment with the shared runtime and host on the server.

Self-contained apps are an option for all ASP.NET Core apps.
-->

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="e231e-206">Usar Docker con Web Apps para contenedores</span><span class="sxs-lookup"><span data-stu-id="e231e-206">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="e231e-207">[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contiene las imágenes de Docker de versión preliminar más recientes.</span><span class="sxs-lookup"><span data-stu-id="e231e-207">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="e231e-208">Las imágenes se pueden usar como base.</span><span class="sxs-lookup"><span data-stu-id="e231e-208">The images can be used as a base image.</span></span> <span data-ttu-id="e231e-209">Use la imagen y efectúe la implementación en Web App for Containers con normalidad.</span><span class="sxs-lookup"><span data-stu-id="e231e-209">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e231e-210">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="e231e-210">Additional resources</span></span>

* [<span data-ttu-id="e231e-211">Introducción a Web Apps (vídeo de introducción de 5 minutos)</span><span class="sxs-lookup"><span data-stu-id="e231e-211">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="e231e-212">Azure App Service: el mejor lugar para hospedar las aplicaciones .NET (vídeo de introducción de 55 minutos)</span><span class="sxs-lookup"><span data-stu-id="e231e-212">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="e231e-213">Azure Friday: experiencia de diagnóstico y solución de problemas de Azure App Service (vídeo de 12 minutos)</span><span class="sxs-lookup"><span data-stu-id="e231e-213">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="e231e-214">Introducción a diagnósticos de Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e231e-214">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="e231e-215">Azure App Service en Windows Server utiliza [Internet Information Services (IIS)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="e231e-215">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="e231e-216">Los temas siguientes se aplican a la tecnología subyacente de IIS:</span><span class="sxs-lookup"><span data-stu-id="e231e-216">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="e231e-217">Biblioteca de TechNet de Microsoft: Windows Server</span><span class="sxs-lookup"><span data-stu-id="e231e-217">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
