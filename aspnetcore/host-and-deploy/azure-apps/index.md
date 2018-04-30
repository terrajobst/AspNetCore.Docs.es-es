---
title: Hospedaje de ASP.NET Core en Azure App Service
author: guardrex
description: Descubra cómo hospedar aplicaciones de ASP.NET Core en Azure App Service con vínculos a recursos útiles.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: f53f77d342cc59094a80e8667db6ef345a6e8305
ms.sourcegitcommit: 01db73f2f7ac22b11ea48a947131d6176b0fe9ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2018
---
# <a name="host-aspnet-core-on-azure-app-service"></a>Hospedaje de ASP.NET Core en Azure App Service

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

[Publicación en Azure con herramientas de CLI](xref:tutorials/publish-to-azure-webapp-using-cli)  
Obtenga información sobre cómo publicar una aplicación de ASP.NET Core en Azure App Service con el cliente de línea de comandos de Git.

[Implementación continua en Azure con Visual Studio y Git](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
Obtenga información sobre cómo crear una aplicación web de ASP.NET Core con Visual Studio e implementarla en Azure App Service con Git para una implementación continua.

[Implementación continua en Azure con VSTS](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
Configure una compilación de integración continua para una aplicación de ASP.NET Core y, después, cree una versión de implementación continua para Azure App Service.

[Espacio aislado de Azure Web App](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
Detecte limitaciones de ejecución en tiempo de ejecución de Azure App Service aplicadas por la plataforma de aplicaciones de Azure.

## <a name="application-configuration"></a>Configuración de aplicación

En ASP.NET Core 2.0 y versiones posteriores, tres paquetes del [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage) proporcionan características de registro automático para aplicaciones implementadas en Azure App Service:

* [Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) utiliza [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) para proporcionar a ASP.NET Core integración iluminada con Azure App Service. El paquete `Microsoft.AspNetCore.AzureAppServicesIntegration` proporciona las características de registro agregadas.
* [Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) ejecuta [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) para agregar a Azure App Service proveedores de registro de diagnósticos en el paquete `Microsoft.Extensions.Logging.AzureAppServices`.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) proporciona implementaciones de registrador para admitir registros de diagnósticos de Azure App Service y características de transmisión en secuencias de registro.

## <a name="proxy-server-and-load-balancer-scenarios"></a>Escenarios de servidor proxy y equilibrador de carga

El software intermedio de integración con IIS, que configura el software intermedio de encabezados reenviados, y el módulo de ASP.NET Core están configurados para reenviar el esquema (HTTP/HTTPS) y la dirección IP remota donde se originó la solicitud. Podría ser necesario realizar una configuración adicional para las aplicaciones hospedadas detrás de servidores proxy y equilibradores de carga adicionales. Para más información, vea [Configurar ASP.NET Core para trabajar con servidores proxy y equilibradores de carga](xref:host-and-deploy/proxy-load-balancer).

## <a name="monitoring-and-logging"></a>Supervisión y registro

Para obtener información sobre supervisión, registro y solución de problemas, consulte los artículos siguientes:

[Cómo: Supervisar aplicaciones en Azure App Service](/azure/app-service/web-sites-monitor)  
Obtenga información sobre cómo revisar las cuotas y las métricas para las aplicaciones y los planes de App Service.

[Habilitar el registro de diagnósticos para las aplicaciones web en Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log)  
Descubra cómo habilitar y acceder a registro de diagnóstico para los códigos de estado HTTP, solicitudes con error y actividad del servidor web.

[Introducción a control de errores en ASP.NET Core](xref:fundamentals/error-handling)  
Comprender los métodos comunes para controlar los errores en las aplicaciones de ASP.NET Core.

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
* [Implementación de la aplicación independiente](#deploy-the-app-self-contained)
* [Uso de Docker con Web Apps para contenedores](#use-docker-with-web-apps-for-containers)

Si tiene algún problema al usar la extensión de sitio de versión preliminar, abra una incidencia en [GitHub](https://github.com/aspnet/azureintegration/issues/new).

### <a name="install-the-preview-site-extension"></a>Instalación de la extensión de sitio de versión preliminar

* En Azure Portal, vaya a la hoja de App Service.
* Escriba "ex" en el cuadro de búsqueda.
* Seleccione **Extensiones**.
* Seleccione "Agregar".

![Hoja de Azure App con los pasos anteriores](index/_static/x1.png)

* Seleccione **ASP.NET Core 2.1 (x86) Runtime** o **ASP.NET Core 2.1 (x64) Runtime**.
* Seleccione **Aceptar**. Vuelva a seleccionar **Aceptar**.

Cuando se complete la operación de adición, se instalará la versión preliminar de .NET Core 2.1. Para comprobar la instalación, ejecute `dotnet --info` en la consola. En la hoja de **App Service**:

* Escriba "con" en el cuadro de búsqueda.
* Seleccione **Consola**.
* Escriba `dotnet --info` en la consola.

![Hoja de Azure App con los pasos anteriores](index/_static/cons.png)

La imagen anterior se capturó en el momento en que se escribía esto. Podría ver una versión diferente.

`dotnet --info` muestra la ruta de acceso a la extensión de sitio en la que se ha instalado la versión preliminar. Muestra que la aplicación se está ejecutando desde la extensión de sitio, en lugar de desde la ubicación predeterminada *ProgramFiles*. Si ve *ProgramFiles*, reinicie el sitio y ejecute `dotnet --info`.

**Uso de la extensión de sitio de versión preliminar con una plantilla de ARM**

Si usa una plantilla de ARM para crear e implementar aplicaciones, puede usar el tipo de recurso `siteextensions` para agregar la extensión de sitio a una aplicación web. Por ejemplo:

[!code-json[Main](index/sample/arm.json?highlight=2)]

### <a name="deploy-the-app-self-contained"></a>Implementación de la aplicación independiente

Puede implementar una [aplicación independiente](/dotnet/core/deploying/#self-contained-deployments-scd) que contenga la versión preliminar del entorno de ejecución. Al implementar una aplicación independiente:

* No es necesario que el sitio esté preparado.
* La aplicación se debe publicar de forma diferente en relación con una implementación que dependa de un marco, con un entorno de ejecución compartido y el servidor como host.

Las aplicaciones independientes son una opción válida para todas las aplicaciones ASP.NET Core.

### <a name="use-docker-with-web-apps-for-containers"></a>Uso de Docker con Web Apps para contenedores

[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contiene las imágenes de Docker de versión preliminar 2.1 más recientes. Las imágenes se pueden usar como base. Use la imagen y efectúe la implementación en Web App for Containers con normalidad.

## <a name="additional-resources"></a>Recursos adicionales

* [Introducción a Web Apps (vídeo de introducción de 5 minutos)](/azure/app-service/app-service-web-overview)
* [Azure App Service: el mejor lugar para hospedar las aplicaciones .NET (vídeo de introducción de 55 minutos)](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [Azure Friday: experiencia de diagnóstico y solución de problemas de Azure App Service (vídeo de 12 minutos)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [Introducción a diagnósticos de Azure App Service](/azure/app-service/app-service-diagnostics)

Azure App Service en Windows Server utiliza [Internet Information Services (IIS)](https://www.iis.net/). Los temas siguientes se aplican a la tecnología subyacente de IIS:

* [Hospedaje de ASP.NET Core en Windows con IIS](xref:host-and-deploy/iis/index)
* [Introducción al módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Referencia de configuración del módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Módulos de IIS con ASP.NET Core](xref:host-and-deploy/iis/modules)
* [Biblioteca de TechNet de Microsoft: Windows Server](/windows-server/windows-server-versions)
