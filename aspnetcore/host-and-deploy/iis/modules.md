---
title: Módulos IIS con ASP.NET Core
author: guardrex
description: Detectar activos e inactivos módulos IIS para aplicaciones ASP.NET Core y cómo administrar los módulos IIS.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/modules
ms.openlocfilehash: d9b3de915df333153255f91649f9169f76ba2fe0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
# <a name="iis-modules-with-aspnet-core"></a>Módulos IIS con ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

Aplicaciones de ASP.NET Core están hospedadas por IIS en una configuración de proxy inverso. Algunos de los módulos nativos de IIS y todos los módulos IIS administrados no están disponibles para procesar las solicitudes para las aplicaciones de ASP.NET Core. En muchos casos, ASP.NET Core ofrece una alternativa a las características de los módulos nativos y administrados de IIS.

## <a name="native-modules"></a>Módulos nativos

| Module | .NET core activo | Opción de ASP.NET Core |
| ------ | :--------------: | ------------------- |
| **Autenticación anónima**<br>`AnonymousAuthenticationModule` | Sí | |
| **Autenticación básica**<br>`BasicAuthenticationModule` | Sí | |
| **Autenticación de asignaciones de certificados de cliente**<br>`CertificateMappingAuthenticationModule` | Sí | |
| **CGI**<br>`CgiModule` | No | |
| **Validación de la configuración**<br>`ConfigurationValidationModule` | Sí | |
| **Errores HTTP**<br>`CustomErrorModule` | No | [Middleware de páginas de código de estado](xref:fundamentals/error-handling#configuring-status-code-pages) |
| **Registro personalizado**<br>`CustomLoggingModule` | Sí | |
| **Documento predeterminado**<br>`DefaultDocumentModule` | No | [Middleware de archivos predeterminado](xref:fundamentals/static-files#serve-a-default-document) |
| **Autenticación implícita**<br>`DigestAuthenticationModule` | Sí | |
| **Examen de directorios**<br>`DirectoryListingModule` | No | [Middleware de exploración de directorios](xref:fundamentals/static-files#enable-directory-browsing) |
| **Compresión dinámica**<br>`DynamicCompressionModule` | Sí | [Middleware de compresión de respuestas](xref:performance/response-compression) |
| **Traza**<br>`FailedRequestsTracingModule` | Sí | [Registro de ASP.NET Core](xref:fundamentals/logging/index#the-tracesource-provider) |
| **Almacenamiento en caché de archivo**<br>`FileCacheModule` | No | [Middleware de almacenamiento en caché de respuestas](xref:performance/caching/middleware) |
| **Almacenamiento en caché de HTTP**<br>`HttpCacheModule` | No | [Middleware de almacenamiento en caché de respuestas](xref:performance/caching/middleware) |
| **Registro HTTP**<br>`HttpLoggingModule` | Sí | [Registro de ASP.NET Core](xref:fundamentals/logging/index)<br>Implementaciones: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging), [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging), [NLog](https://github.com/NLog/NLog.Extensions.Logging), [Serilog](https://github.com/serilog/serilog-extensions-logging)
| **Redirección HTTP**<br>`HttpRedirectionModule` | Sí | [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting) |
| **Autenticación de asignaciones de certificado de cliente IIS**<br>`IISCertificateMappingAuthenticationModule` | Sí | |
| **Restricciones de IP y dominio**<br>`IpRestrictionModule` | Sí | |
| **Filtros ISAPI**<br>`IsapiFilterModule` | Sí | [Middleware](xref:fundamentals/middleware/index) |
| **ISAPI**<br>`IsapiModule` | Sí | [Middleware](xref:fundamentals/middleware/index) |
| **Compatibilidad con el protocolo**<br>`ProtocolSupportModule` | Sí | |
| **Filtrado de solicitudes**<br>`RequestFilteringModule` | Sí | [Middleware de reescritura de dirección URL `IRule`](xref:fundamentals/url-rewriting#irule-based-rule) |
| **Monitor de solicitudes**<br>`RequestMonitorModule` | Sí | |
| **Reescritura de direcciones URL**<br>`RewriteModule` | Sí&#8224; | [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting) |
| **Inclusiones del lado servidor**<br>`ServerSideIncludeModule` | No | |
| **Compresión estática**<br>`StaticCompressionModule` | No | [Middleware de compresión de respuestas](xref:performance/response-compression) |
| **Contenido estático**<br>`StaticFileModule` | No | [Middleware de archivos estáticos](xref:fundamentals/static-files) |
| **Símbolo (token) de almacenamiento en caché**<br>`TokenCacheModule` | Sí | |
| **Almacenamiento en caché de URI**<br>`UriCacheModule` | Sí | |
| **Autorización de URL**<br>`UrlAuthorizationModule` | Sí | [Identidad principal de ASP.NET](xref:security/authentication/identity) |
| **Autenticación de Windows**<br>`WindowsAuthenticationModule` | Sí | |

&#8224;Del módulo URL Rewrite `isFile` y `isDirectory` coincide con los tipos no funcionan con aplicaciones de ASP.NET Core debido a los cambios en [estructura de directorios](xref:host-and-deploy/directory-structure).

## <a name="managed-modules"></a>Módulos administrados

| Module                  | .NET core activo | Opción de ASP.NET Core |
| ----------------------- | :--------------: | ------------------- |
| AnonymousIdentification | No               | |
| DefaultAuthentication   | No               | |
| FileAuthorization       | No               | |
| FormsAuthentication     | No               | [Middleware de autenticación de cookies.](xref:security/authentication/cookie) |
| OutputCache             | No               | [Middleware de almacenamiento en caché de respuestas](xref:performance/caching/middleware) |
| Perfil                 | No               | |
| RoleManager             | No               | |
| ScriptModule-4.0        | No               | |
| Sesión                 | No               | [Middleware de sesión](xref:fundamentals/app-state) |
| UrlAuthorization        | No               | |
| UrlMappingsModule       | No               | [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting) |
| UrlRoutingModule-4.0    | No               | [Identidad principal de ASP.NET](xref:security/authentication/identity) |
| WindowsAuthentication   | No               | |

## <a name="iis-manager-application-changes"></a>Cambios en la aplicación Administrador de IIS

Al usar el Administrador de IIS para configurar opciones, el *web.config* se cambia el archivo de la aplicación. Si la implementación de una aplicación e incluida dicha cláusula *web.config*, se sobrescribirán los cambios realizados con el Administrador de IIS por implementado *web.config* archivo. Si se realizan cambios en el servidor *web.config* archivo, copie los archivos *web.config* archivo en el servidor para el proyecto local inmediatamente.

## <a name="disabling-iis-modules"></a>Deshabilitación de módulos IIS

Si un módulo IIS está configurado en el nivel de servidor que debe deshabilitarse para una aplicación, un complemento de la aplicación *web.config* archivo puede deshabilitar el módulo. Deje el módulo en su lugar y desactivarlo mediante un valor de configuración (si está disponible) o quitar el módulo de la aplicación.

### <a name="module-deactivation"></a>Desactivación de módulo

Muchos módulos ofrecen una opción de configuración que les permite deshabilitar sin quitar el módulo de la aplicación. Se trata de la forma más sencilla y rápida para desactivar un módulo. Por ejemplo, el módulo URL Rewrite de IIS puede deshabilitarse con la  **\<httpRedirect >** elemento *web.config*:

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

Para obtener más información acerca de cómo deshabilitar módulos con opciones de configuración, siga los vínculos en la *elementos secundarios* sección de [IIS \<system.webServer >](/iis/configuration/system.webServer/).

### <a name="module-removal"></a>Eliminación de módulo

Si la aceptación para quitar un módulo con una configuración en *web.config*, desbloquear el módulo y desbloquear el  **\<módulos >** sección de *web.config* primera:

1. Desbloquear el módulo en el nivel de servidor. Seleccione el servidor IIS en el Administrador de IIS **conexiones** sidebar. Abra la **módulos** en el **IIS** área. Seleccione el módulo en la lista. En el **acciones** sidebar a la derecha, seleccione **Unlock**. Desbloquear tantos módulos como va a quitar de *web.config* más tarde.

2. Implementar la aplicación sin un  **\<módulos >** sección *web.config*. Si una aplicación se implementa con un *web.config* que contiene el  **\<módulos >** sección sin necesidad de desbloquear la sección en primer lugar en el Administrador de IIS, el Administrador de configuración produce una excepción al intentar desbloquear la sección. Por lo tanto, implementar la aplicación sin un  **\<módulos >** sección.

3. Desbloquear la  **\<módulos >** sección de *web.config*. En el **conexiones** sidebar, seleccione el sitio Web en **sitios**. En el **administración** área, abra el **Editor de configuración**. Utilice los controles de navegación para seleccionar la `system.webServer/modules` sección. En el **acciones** sidebar a la derecha, seleccione esta opción para **Unlock** la sección.

4. En este punto, un  **\<módulos >** sección puede agregarse a la *web.config* de archivos con una  **\<quitar >** elemento que se va a quitar el módulo de la aplicación. Varios  **\<quitar >** pueden agregarse los elementos para quitar varios módulos. Si *web.config* se realizan cambios en el servidor, realizar inmediatamente los mismos cambios en el proyecto *web.config* archivo localmente. Si quita un módulo de esta manera no afectan al uso del módulo con otras aplicaciones en el servidor.

   ```xml
   <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
   </configuration>
   ```

Para una instalación de IIS con los módulos predeterminados instalados, utilice la siguiente  **\<módulo >** sección para desinstalar los módulos predeterminados.

```xml
<modules>
  <remove name="CustomErrorModule" />
  <remove name="DefaultDocumentModule" />
  <remove name="DirectoryListingModule" />
  <remove name="HttpCacheModule" />
  <remove name="HttpLoggingModule" />
  <remove name="ProtocolSupportModule" />
  <remove name="RequestFilteringModule" />
  <remove name="StaticCompressionModule" /> 
  <remove name="StaticFileModule" /> 
</modules>
```

También se puede quitar un módulo IIS con *Appcmd.exe*. Proporcionar la `MODULE_NAME` y `APPLICATION_NAME` en el comando:

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

Por ejemplo, quite el `DynamicCompressionModule` desde el sitio Web predeterminado:

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimum-module-configuration"></a>Configuración mínima del módulo

Los módulos solo necesarios para ejecutar una aplicación de ASP.NET Core son el módulo de autenticación anónima y el módulo principal de ASP.NET.

![Abra el Administrador de IIS a módulos con la configuración mínima del módulo que se muestra](modules/_static/modules.png)

## <a name="additional-resources"></a>Recursos adicionales

* [Hospedaje en Windows con IIS](xref:host-and-deploy/iis/index)
* [Introducción a las arquitecturas de IIS: módulos de IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#modules-in-iis)
* [Información general de los módulos IIS](/iis/get-started/introduction-to-iis/iis-modules-overview)
* [Personalización de IIS 7.0 Roles y módulos](https://technet.microsoft.com/library/cc627313.aspx)
* [IIS `<system.webServer>`](/iis/configuration/system.webServer/)
