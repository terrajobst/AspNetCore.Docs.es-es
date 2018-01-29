---
title: "Uso de módulos IIS con ASP.NET Core"
author: guardrex
description: "Documento de referencia que describe activos e inactivos módulos IIS para las aplicaciones de ASP.NET Core."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/08/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: host-and-deploy/iis/modules
ms.openlocfilehash: 405297bdd1ceac390b995ed6e15ae8d95bb8501f
ms.sourcegitcommit: a3e88639a6bcf8fb4d634036dac93130c464a097
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2018
---
# <a name="using-iis-modules-with-aspnet-core"></a>Uso de módulos IIS con ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

Las aplicaciones de ASP.NET Core se hospedan en IIS en una configuración de proxy inverso. Algunos de los módulos nativos de IIS y todos los módulos IIS administrados no están disponibles para procesar las solicitudes para las aplicaciones de ASP.NET Core. En muchos casos, ASP.NET Core ofrece una alternativa a las características de los módulos nativos y administrados de IIS.

## <a name="native-modules"></a>Módulos nativos

Module | .NET core activo | Opción de ASP.NET Core
--- | :---: | ---
**Autenticación anónima**<br>`AnonymousAuthenticationModule` | Sí | 
**Autenticación básica**<br>`BasicAuthenticationModule` | Sí | 
**Autenticación de asignaciones de certificados de cliente**<br>`CertificateMappingAuthenticationModule` | Sí | 
**CGI**<br>`CgiModule` | No | 
**Validación de la configuración**<br>`ConfigurationValidationModule` | Sí | 
**Errores HTTP**<br>`CustomErrorModule` | No | [Middleware de páginas de código de estado](xref:fundamentals/error-handling#configuring-status-code-pages)
**Registro personalizado**<br>`CustomLoggingModule` | Sí | 
**Documento predeterminado**<br>`DefaultDocumentModule` | No | [Middleware de archivos predeterminado](xref:fundamentals/static-files#serve-a-default-document)
**Autenticación implícita**<br>`DigestAuthenticationModule` | Sí | 
**Examen de directorios**<br>`DirectoryListingModule` | No | [Middleware de exploración de directorios](xref:fundamentals/static-files#enable-directory-browsing)
**Compresión dinámica**<br>`DynamicCompressionModule` | Sí | [Middleware de compresión de respuestas](xref:performance/response-compression)
**Traza**<br>`FailedRequestsTracingModule` | Sí | [Registro de ASP.NET Core](xref:fundamentals/logging/index#the-tracesource-provider)
**Almacenamiento en caché de archivo**<br>`FileCacheModule` | No | [Middleware de almacenamiento en caché de respuestas](xref:performance/caching/middleware)
**Almacenamiento en caché de HTTP**<br>`HttpCacheModule` | No | [Middleware de almacenamiento en caché de respuestas](xref:performance/caching/middleware)
**Registro HTTP**<br>`HttpLoggingModule` | Sí | [Registro de ASP.NET Core](xref:fundamentals/logging/index)<br>Implementaciones: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging), [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging), [NLog](https://github.com/NLog/NLog.Extensions.Logging), [Serilog](https://github.com/serilog/serilog-extensions-logging)
**Redirección HTTP**<br>`HttpRedirectionModule` | Sí | [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting)
**Autenticación de asignaciones de certificado de cliente IIS**<br>`IISCertificateMappingAuthenticationModule` | Sí | 
**Restricciones de IP y dominio**<br>`IpRestrictionModule` | Sí | 
**Filtros ISAPI**<br>`IsapiFilterModule` | Sí | [Middleware](xref:fundamentals/middleware)
**ISAPI**<br>`IsapiModule` | Sí | [Middleware](xref:fundamentals/middleware)
**Compatibilidad con el protocolo**<br>`ProtocolSupportModule` | Sí | 
**Filtrado de solicitudes**<br>`RequestFilteringModule` | Sí | [Middleware de reescritura de dirección URL`IRule`](xref:fundamentals/url-rewriting#irule-based-rule)
**Monitor de solicitudes**<br>`RequestMonitorModule` | Sí | 
**Reescritura de direcciones URL**<br>`RewriteModule` | Yes† | [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting)
**Inclusiones del lado servidor**<br>`ServerSideIncludeModule` | No | 
**Compresión estática**<br>`StaticCompressionModule` | No | [Middleware de compresión de respuestas](xref:performance/response-compression)
**Contenido estático**<br>`StaticFileModule` | No | [Middleware de archivos estáticos](xref:fundamentals/static-files)
**Símbolo (token) de almacenamiento en caché**<br>`TokenCacheModule` | Sí | 
**Almacenamiento en caché de URI**<br>`UriCacheModule` | Sí | 
**Autorización de URL**<br>`UrlAuthorizationModule` | Sí | [Identidad principal de ASP.NET](xref:security/authentication/identity)
**Autenticación de Windows**<br>`WindowsAuthenticationModule` | Sí | 

†The módulo URL Rewrite `isFile` y `isDirectory` no funciona con aplicaciones de ASP.NET Core debido a los cambios en [estructura de directorios](xref:host-and-deploy/directory-structure).

## <a name="managed-modules"></a>Módulos administrados

Module | .NET core activo | Opción de ASP.NET Core
--- | :---: | ---
AnonymousIdentification | No | 
DefaultAuthentication | No | 
FileAuthorization | No | 
FormsAuthentication | No | [Middleware de autenticación de cookies.](xref:security/authentication/cookie)
OutputCache | No | [Middleware de almacenamiento en caché de respuestas](xref:performance/caching/middleware)
Perfil | No | 
RoleManager | No | 
ScriptModule-4.0 | No | 
Sesión | No | [Middleware de sesión](xref:fundamentals/app-state)
UrlAuthorization | No | 
UrlMappingsModule | No | [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting)
UrlRoutingModule-4.0 | No | [Identidad principal de ASP.NET](xref:security/authentication/identity)
WindowsAuthentication | No | 

## <a name="iis-manager-application-changes"></a>Cambios en la aplicación Administrador de IIS

Mediante el Administrador de IIS para configurar opciones, el *web.config* se cambia el archivo de la aplicación. Si la implementación de una aplicación e incluida dicha cláusula *web.config*, se sobrescribirán los cambios realizados con el Administrador de IIS por implementado *web.config* archivo. Si se realizan cambios en el servidor *web.config* archivo, copie los archivos *web.config* archivo al proyecto local inmediatamente.

## <a name="disabling-iis-modules"></a>Deshabilitación de módulos IIS

Si un módulo IIS está configurado en el nivel de servidor que debe deshabilitarse para una aplicación, un complemento de la aplicación *web.config* archivo puede deshabilitar el módulo. Deje el módulo en su lugar y desactivarlo mediante un valor de configuración (si está disponible) o quitar el módulo de la aplicación.

### <a name="module-deactivation"></a>Desactivación de módulo

Muchos módulos ofrecen una opción de configuración que permite que los deshabilita sin quitarlos de la aplicación. Se trata de la forma más sencilla y rápida para desactivar un módulo. Por ejemplo, si desea deshabilitar el módulo URL Rewrite de IIS, utilice la `<httpRedirect>` elemento tal y como se muestra a continuación. Para obtener más información acerca de cómo deshabilitar módulos con opciones de configuración, siga los vínculos en la *elementos secundarios* sección de [IIS `<system.webServer>` ](https://docs.microsoft.com/iis/configuration/system.webServer/).

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

### <a name="module-removal"></a>Eliminación de módulo

Si la aceptación para quitar un módulo con una configuración en *web.config*, desbloquear el módulo y desbloquear el `<modules>` sección de *web.config* primero. Los pasos se describen a continuación:

1. Desbloquear el módulo en el nivel de servidor. Haga clic en el servidor IIS en el Administrador de IIS **conexiones** sidebar. Abra la **módulos** en el **IIS** área. Haga clic en el módulo en la lista. En el **acciones** sidebar a la derecha, haga clic en **Unlock**. Desbloquear tantos módulos según lo previsto para quitar de *web.config* más tarde.

2. Implementar la aplicación sin un `<modules>` sección *web.config*. Si una aplicación se implementa con un *web.config* que contiene el `<modules>` sección sin necesidad de desbloquear la sección en primer lugar en el Administrador de IIS, el Administrador de configuración produce una excepción al intentar desbloquear la sección. Por lo tanto, implementar la aplicación sin un `<modules>` sección.

3. Desbloquear la `<modules>` sección de *web.config*. En el **conexiones** sidebar, haga clic en el sitio Web en **sitios**. En el **administración** área, abra el **Editor de configuración**. Utilice los controles de navegación para seleccionar la `system.webServer/modules` sección. En el **acciones** sidebar a la derecha, haga clic en a **Unlock** la sección.

4. En este punto, un `<modules>` sección puede agregarse a la *web.config* de archivos con un `<remove>` elemento que se va a quitar el módulo de la aplicación. Varios `<remove>` pueden agregarse los elementos para quitar varios módulos. No olvide que if *web.config* se realizan cambios en el servidor para que sean inmediatamente en el proyecto de forma local. Si quita un módulo de esta manera no afectan al uso del módulo con otras aplicaciones en el servidor.

  ```xml
  <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
  </configuration>
  ```

Para una instalación de IIS con los módulos predeterminados instalados, utilice la siguiente `<module>` sección para desinstalar los módulos predeterminados.

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

También se puede quitar un módulo IIS con *Appcmd.exe*. Proporcionar la `MODULE_NAME` y `APPLICATION_NAME` en el comando que se muestra a continuación:

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

Aquí se muestra cómo quitar el `DynamicCompressionModule` desde el sitio Web predeterminado:

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimum-module-configuration"></a>Configuración mínima del módulo

Los módulos solo necesarios para ejecutar una aplicación de ASP.NET Core son el módulo de autenticación anónima y el módulo principal de ASP.NET.

![Abra el Administrador de IIS a módulos con la configuración mínima del módulo que se muestra](modules/_static/modules.png)

## <a name="additional-resources"></a>Recursos adicionales

* [Hospedaje en Windows con IIS](xref:host-and-deploy/iis/index)
* [Información general de los módulos IIS](https://docs.microsoft.com/iis/get-started/introduction-to-iis/iis-modules-overview)
* [Personalización de IIS 7.0 Roles y módulos](https://technet.microsoft.com/library/cc627313.aspx)
* [IIS `<system.webServer>`](https://docs.microsoft.com/iis/configuration/system.webServer/)
