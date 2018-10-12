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
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a>Implementar aplicaciones de ASP.NET Core en Azure App Service

[Azure App Service](https://azure.microsoft.com/services/app-service/) es un [servicio de plataforma de informática en la nube de Microsoft](https://azure.microsoft.com/) que sirve para hospedar aplicaciones web, como ASP.NET Core.

## <a name="useful-resources"></a>Recursos útiles

La [documentación de Web Apps](/azure/app-service/) de Azure es un recurso que incluye documentación, tutoriales, ejemplos, guías de procedimientos y otros recursos de aplicaciones de Azure. Dos tutoriales importantes que pertenecen al hospedaje de aplicaciones de ASP.NET Core son:

[Inicio rápido: Crear una aplicación web de ASP.NET Core en Azure](/azure/app-service/app-service-web-get-started-dotnet)  
Usar Visual Studio para crear e implementar una aplicación web de ASP.NET Core para Azure App Service en Windows.

[Inicio rápido: Crear una aplicación web de .NET Core en App Service en Linux](/azure/app-service/containers/quickstart-dotnetcore)  
Usar la línea de comandos para crear e implementar una aplicación web de ASP.NET Core para Azure App Service en Linux.

Los artículos siguientes están disponibles en la documentación de ASP.NET Core:

[Publicación en Azure con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)  
Obtenga información sobre cómo publicar una aplicación de ASP.NET Core en Azure App Service con Visual Studio.

[Implementación continua en Azure con Visual Studio y Git](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
Obtenga información sobre cómo crear una aplicación web de ASP.NET Core con Visual Studio e implementarla en Azure App Service con Git para una implementación continua.

[Creación de la primera canalización con Azure Pipelines](/azure/devops/pipelines/get-started-yaml)  
Configure una compilación de integración continua para una aplicación de ASP.NET Core y, después, cree una versión de implementación continua para Azure App Service.

[Espacio aislado de Azure Web App](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
Detecte limitaciones de ejecución en tiempo de ejecución de Azure App Service aplicadas por la plataforma de aplicaciones de Azure.

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a>Configuración de aplicación

En ASP.NET Core 2.0 y versiones posteriores, los siguientes paquetes de NuGet proporcionan características de registro automáticas para aplicaciones implementadas en Azure App Service:

* [Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) usa [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) para proporcionar a ASP.NET Core una integración ligera (Light-Up) con Azure App Service. El paquete `Microsoft.AspNetCore.AzureAppServicesIntegration` proporciona las características de registro agregadas.
* [Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) ejecuta [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) para agregar a Azure App Service proveedores de registro de diagnósticos en el paquete `Microsoft.Extensions.Logging.AzureAppServices`.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) proporciona implementaciones de registrador para admitir registros de diagnósticos de Azure App Service y características de transmisión en secuencias de registro.

Si tiene .NET Core como destino y hace referencia al [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage), los paquetes ya están incluidos. Los paquetes no están en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), que es más reciente. Si tiene .NET Framework como destino o hace referencia al metapaquete `Microsoft.AspNetCore.App`, haga referencia a los paquetes de registro individuales.

::: moniker-end

## <a name="override-app-configuration-using-the-azure-portal"></a>Invalidación de la configuración de la aplicación mediante Azure Portal

El área **Configuración de la aplicación** de la hoja **Configuración de la aplicación** le permite establecer variables de entorno para la aplicación. El [proveedor de configuración de variables de entorno](xref:fundamentals/configuration/index#environment-variables-configuration-provider) puede consumir las variables de entorno.

Cuando una aplicación usa el [host web](xref:fundamentals/host/web-host) y compila el host mediante [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), las variables de entorno que configuran el host usan el prefijo `ASPNETCORE_`. Para más información, consulte <xref:fundamentals/host/web-host> y el [proveedor de configuración de variables de entorno](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

Cuando una aplicación usa el [host genérico](xref:fundamentals/host/generic-host), no se cargan variables de entorno en la configuración de una aplicación de manera predeterminada y es el desarrollador el que debe agregar al proveedor de configuración. El desarrollador determina el prefijo de variable de entorno cuando se agrega el proveedor de configuración. Para más información, consulte <xref:fundamentals/host/generic-host> y el [proveedor de configuración de variables de entorno](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

## <a name="proxy-server-and-load-balancer-scenarios"></a>Escenarios de servidor proxy y equilibrador de carga

El software intermedio de integración con IIS, que configura el software intermedio de encabezados reenviados, y el módulo de ASP.NET Core están configurados para reenviar el esquema (HTTP/HTTPS) y la dirección IP remota donde se originó la solicitud. Podría ser necesario realizar una configuración adicional para las aplicaciones hospedadas detrás de servidores proxy y equilibradores de carga adicionales. Para más información, vea [Configurar ASP.NET Core para trabajar con servidores proxy y equilibradores de carga](xref:host-and-deploy/proxy-load-balancer).

## <a name="monitoring-and-logging"></a>Supervisión y registro

Para obtener información sobre supervisión, registro y solución de problemas, consulte los artículos siguientes:

[Cómo: Supervisar aplicaciones en Azure App Service](/azure/app-service/web-sites-monitor)  
Obtenga información sobre cómo revisar las cuotas y las métricas para las aplicaciones y los planes de App Service.

[Habilitar el registro de diagnósticos para las aplicaciones web en Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log)  
Descubra cómo habilitar y acceder a registro de diagnóstico para los códigos de estado HTTP, solicitudes con error y actividad del servidor web.

[Introducción a control de errores en ASP.NET Core](xref:fundamentals/error-handling)  
Conozca los métodos habituales para controlar los errores en las aplicaciones de ASP.NET Core.

[Solución de problemas de ASP.NET Core en Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)  
Obtenga información sobre cómo diagnosticar problemas con las implementaciones de Azure App Service con las aplicaciones de ASP.NET Core.

[Referencia de errores comunes de Azure App Service e IIS con ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)  
Consulte los errores comunes de configuración de implementación para las aplicaciones hospedadas por Azure App Service/IIS con consejos de solución de problemas.

## <a name="data-protection-key-ring-and-deployment-slots"></a>Anillo de clave de protección de datos y ranuras de implementación

Las [claves de protección de datos](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) se conservan en la carpeta *%HOME%\ASP.NET\DataProtection-Keys*. Esta carpeta está respaldada por el almacenamiento de red y se sincroniza en todas las máquinas que hospedan la aplicación. Las claves no están protegidas en reposo. Esta carpeta proporciona el anillo de clave a todas las instancias de una aplicación en una única ranura de implementación. Las ranuras de implementación independientes, por ejemplo, almacenamiento provisional y producción, no comparten ningún anillo de clave.

Al realizar un intercambio entre ranuras de implementación, cualquier sistema que utilice la protección de datos no podrá descifrar los datos almacenados mediante el anillo de clave situado dentro de la ranura anterior. El middleware de cookies de ASP.NET usa protección de datos para proteger sus cookies. Esto hace que los usuarios cierren sesión en una aplicación que usa el middleware de cookies de ASP.NET estándar. Para una solución de anillo de clave independiente de la ranura, utilice un proveedor de anillo de clave externo, como estos:

* Azure Blob Storage
* Azure Key Vault
* Almacén SQL
* Redis Cache

Para obtener más información, consulte [Proveedores de almacenamiento de claves](xref:security/data-protection/implementation/key-storage-providers).

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a>Implementar una versión preliminar de ASP.NET Core en Azure App Service

Las aplicaciones de versión preliminar de ASP.NET Core se pueden implementar en Azure App Service con los procedimientos siguientes:

* [Instalación de la extensión de sitio de versión preliminar](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) -->
* [Usar Docker con Web Apps para contenedores](#use-docker-with-web-apps-for-containers)

### <a name="install-the-preview-site-extension"></a>Instalación de la extensión de sitio de versión preliminar

Si tiene algún problema al usar la extensión de sitio de versión preliminar, abra una incidencia en [GitHub](https://github.com/aspnet/azureintegration/issues/new).

1. En Azure Portal, vaya a la hoja de App Service.
1. Seleccione la aplicación web.
1. Escriba "ex" en el cuadro de búsqueda o desplácese hacia abajo en la lista de secciones de administración hasta llegar a **HERRAMIENTAS DE DESARROLLO**.
1. Seleccione **HERRAMIENTAS DE DESARROLLO** > **Extensiones**.
1. Seleccione **Agregar**.
1. Seleccione la extensión **Runtime de ASP.NET Core &lt;x.y&gt; (x86)** en la lista, donde `<x.y>` es la versión preliminar de ASP.NET Core (por ejemplo, **Runtime de ASP.NET Core 2.2 (x86)**). Runtime de x86 es adecuado para [implementaciones dependientes del marco](/dotnet/core/deploying/#framework-dependent-deployments-fdd), que se basan en el hospedaje fuera de proceso proporcionado por el módulo ASP.NET Core.
1. Seleccione **Aceptar** para aceptar los términos legales.
1. Seleccione **Aceptar** para instalar la extensión.

Cuando se complete la operación, se instalará la versión preliminar de .NET Core. Compruebe la instalación:

1. Seleccione **Herramientas avanzadas** en **HERRAMIENTAS DE DESARROLLO**.
1. Seleccione **Ir** en la hoja **Herramientas avanzadas**.
1. Seleccione el elemento de menú **Consola de depuración** > **PowerShell**.
1. Ejecute el siguiente comando en el símbolo del sistema de PowerShell: Sustituya la versión de runtime de ASP.NET Core por `<x.y>` en el comando:

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x86\
   ```
   Si el runtime de la versión preliminar instalada es para ASP.NET Core 2.2, el comando será:
   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x86\
   ```
   El comando devuelve `True` cuando está instalado el runtime de la versión preliminar de x64.

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> La arquitectura de plataforma (x86/x64) de una aplicación de App Services se configura en la hoja **Configuración de la aplicación**, en **Configuración general**, para las aplicaciones hospedadas en un cálculo de serie A o en un mejor nivel de hospedaje. Si la aplicación se ejecuta en modo de en proceso y la arquitectura de plataforma está configurada para 64 bits (x64), el módulo ASP.NET Core usa el runtime de la versión preliminar de 64 bits, si está presente. Instale la extensión **Runtime de ASP.NET Core &lt;x.y&gt; (x64)** (por ejemplo, **Runtime de ASP.NET Core 2.2 (x64)**).
>
> Después de instalar el runtime de la versión preliminar de x64, ejecute el siguiente comando en la ventana de comandos de Kudu PowerShell para comprobar la instalación. Sustituya la versión de runtime de ASP.NET Core por `<x.y>` en el comando:
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x64\
> ```
> Si el runtime de la versión preliminar instalada es para ASP.NET Core 2.2, el comando será:
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x64\
> ```
> El comando devuelve `True` cuando está instalado el runtime de la versión preliminar de x64.

::: moniker-end

> [!NOTE]
> Con **Extensiones de ASP.NET Core** se obtienen funciones adicionales para ASP.NET Core en Azure App Services, como habilitar el registro de Azure. La extensión se instala automáticamente cuando se implementa desde Visual Studio. Si no se instala la extensión, instálela para la aplicación.

**Uso de la extensión de sitio de versión preliminar con una plantilla de ARM**

Si usa una plantilla de ARM para crear e implementar aplicaciones, puede usar el tipo de recurso `siteextensions` para agregar la extensión de sitio a una aplicación web. Por ejemplo:

[!code-json[Main](index/sample/arm.json?highlight=2)]

<!--
### Deploy the app self-contained

A [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) can be deployed that carries the preview runtime in the deployment. When deploying a self-contained app:

* The site doesn't need to be prepared.
* The app must be published differently than when publishing for a framework-dependent deployment with the shared runtime and host on the server.

Self-contained apps are an option for all ASP.NET Core apps.
-->

### <a name="use-docker-with-web-apps-for-containers"></a>Usar Docker con Web Apps para contenedores

[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contiene las imágenes de Docker de versión preliminar más recientes. Las imágenes se pueden usar como base. Use la imagen y efectúe la implementación en Web App for Containers con normalidad.

## <a name="additional-resources"></a>Recursos adicionales

* [Introducción a Web Apps (vídeo de introducción de 5 minutos)](/azure/app-service/app-service-web-overview)
* [Azure App Service: el mejor lugar para hospedar las aplicaciones .NET (vídeo de introducción de 55 minutos)](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [Azure Friday: experiencia de diagnóstico y solución de problemas de Azure App Service (vídeo de 12 minutos)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [Introducción a diagnósticos de Azure App Service](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

Azure App Service en Windows Server utiliza [Internet Information Services (IIS)](https://www.iis.net/). Los temas siguientes se aplican a la tecnología subyacente de IIS:

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [Biblioteca de TechNet de Microsoft: Windows Server](/windows-server/windows-server-versions)
