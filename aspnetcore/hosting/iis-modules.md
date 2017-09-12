---
title: "Uso de módulos IIS con ASP.NET Core"
author: guardrex
description: "Documento de referencia que describe activos e inactivos módulos IIS para las aplicaciones de ASP.NET Core."
keywords: "Núcleo de ASP.NET, iis, el módulo, el servidor proxy inverso"
ms.author: riande
manager: wpickett
ms.date: 03/08/2017
ms.topic: article
ms.assetid: 492b3a7e-04c5-461b-b96a-38ecee5c64bc
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/iis-modules
ms.openlocfilehash: 353cd4c18cb2708f2dece5ba2b5271f452379d52
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="using-iis-modules-with-aspnet-core"></a>Uso de módulos IIS con ASP.NET Core

Por [Luke Latham](https://github.com/GuardRex)

Las aplicaciones de ASP.NET Core se hospedan en IIS en una configuración de proxy inverso. Algunos de los módulos nativos de IIS y todos los módulos IIS administrados no están disponibles para procesar las solicitudes para las aplicaciones de ASP.NET Core. En muchos casos, ASP.NET Core ofrece una alternativa a las características de los módulos nativos y administrados de IIS.

## <a name="native-modules"></a>Módulos nativos
Módulo | .NET core activo | Opción de ASP.NET Core
--- | :---: | ---
**Autenticación anónima**<br>`AnonymousAuthenticationModule` | Sí | 
**Autenticación básica**<br>`BasicAuthenticationModule` | Sí | 
**Autenticación de asignaciones de certificados de cliente**<br>`CertificateMappingAuthenticationModule` | Sí | 
**CGI**<br>`CgiModule` | No | 
**Validación de la configuración**<br>`ConfigurationValidationModule` | Sí | 
**Errores HTTP**<br>`CustomErrorModule` | No | [Middleware de páginas de código de estado](xref:fundamentals/error-handling#configuring-status-code-pages)
**Registro personalizado**<br>`CustomLoggingModule` | Sí | 
**Documento predeterminado**<br>`DefaultDocumentModule` | No | [Middleware de archivos predeterminado](xref:fundamentals/static-files#serving-a-default-document)
**Autenticación implícita**<br>`DigestAuthenticationModule` | Sí | 
**Examen de directorios**<br>`DirectoryListingModule` | No | [Middleware de exploración de directorios](xref:fundamentals/static-files#enabling-directory-browsing)
**Compresión dinámica**<br>`DynamicCompressionModule` | Sí | [Middleware de compresión de respuesta](xref:performance/response-compression)
**Seguimiento**<br>`FailedRequestsTracingModule` | Sí | [Registro de ASP.NET Core](xref:fundamentals/logging#the-tracesource-provider)
**Almacenamiento en caché de archivo**<br>`FileCacheModule` | No | [Middleware de almacenamiento en caché de respuesta](xref:performance/caching/middleware)
**Almacenamiento en caché de HTTP**<br>`HttpCacheModule` | No | [Middleware de almacenamiento en caché de respuesta](xref:performance/caching/middleware)
**Registro HTTP**<br>`HttpLoggingModule` | Sí | [Registro de ASP.NET Core](xref:fundamentals/logging)<br>Implementaciones: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging), [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging), [NLog](https://github.com/NLog/NLog.Extensions.Logging), [Serilog](https://github.com/serilog/serilog-extensions-logging)
**Redirección HTTP**<br>`HttpRedirectionModule` | Sí | [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting)
**Autenticación de asignaciones de certificado de cliente IIS**<br>`IISCertificateMappingAuthenticationModule` | Sí | 
**Restricciones de IP y dominio**<br>`IpRestrictionModule` | Sí | 
**Filtros ISAPI**<br>`IsapiFilterModule` | Sí | [Software intermedio](xref:fundamentals/middleware)
**ISAPI**<br>`IsapiModule` | Sí | [Software intermedio](xref:fundamentals/middleware)
**Compatibilidad con el protocolo**<br>`ProtocolSupportModule` | Sí | 
**Filtrado de solicitudes**<br>`RequestFilteringModule` | Sí | [Middleware de reescritura de dirección URL`IRule`](xref:fundamentals/url-rewriting#irule-based-rule)
**Monitor de solicitudes**<br>`RequestMonitorModule` | Sí | 
**Reescritura de direcciones URL**<br>`RewriteModule` | Yes† | [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting)
**Inclusiones del lado servidor**<br>`ServerSideIncludeModule` | No | 
**Compresión estática**<br>`StaticCompressionModule` | No | [Middleware de compresión de respuesta](xref:performance/response-compression)
**Contenido estático**<br>`StaticFileModule` | No | [Middleware de archivos estáticos](xref:fundamentals/static-files)
**Símbolo (token) de almacenamiento en caché**<br>`TokenCacheModule` | Sí | 
**Almacenamiento en caché de URI**<br>`UriCacheModule` | Sí | 
**Autorización de URL**<br>`UrlAuthorizationModule` | Sí | [Identidad principal de ASP.NET](xref:security/authentication/identity)
**Autenticación de Windows**<br>`WindowsAuthenticationModule` | Sí | 

†The módulo URL Rewrite `isFile` y `isDirectory` no funcionan con aplicaciones de ASP.NET Core debido a los cambios en [estructura de directorios](xref:hosting/directory-structure).

## <a name="managed-modules"></a>Módulos administrados
Módulo | .NET core activo | Opción de ASP.NET Core
--- | :---: | ---
AnonymousIdentification | No | 
DefaultAuthentication | No | 
FileAuthorization | No | 
FormsAuthentication | No | [Middleware de autenticación de cookies.](xref:security/authentication/cookie)
OutputCache | No | [Middleware de almacenamiento en caché de respuesta](xref:performance/caching/middleware)
Perfil | No | 
RoleManager | No | 
ScriptModule 4.0 | No | 
Sesión | No | [Middleware de sesión](xref:fundamentals/app-state)
UrlAuthorization | No | 
UrlMappingsModule | No | [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting)
UrlRoutingModule 4.0 | No | [Identidad principal de ASP.NET](xref:security/authentication/identity)
WindowsAuthentication | No | 

## <a name="iis-manager-application-changes"></a>Cambios en la aplicación Administrador de IIS
Cuando utiliza el Administrador de IIS para configurar, se modifica directamente la *web.config* archivo de la aplicación. Si implementa la aplicación e incluyen *web.config*, se sobrescribirá los cambios realizados con el Administrador de IIS por implementado *archivo web.config*. Por lo tanto, si realiza cambios en el servidor *web.config* archivo, copie los archivos *web.config* archivo a su proyecto local inmediatamente.

## <a name="disabling-iis-modules"></a>Deshabilitación de módulos IIS
Si tiene un módulo IIS configurado en el nivel de servidor que desea deshabilitar para una aplicación, puede hacerlo con un complemento de la *web.config* archivo. Deje el módulo en su lugar y desactivarlo mediante un valor de configuración (si está disponible) o quitar el módulo de la aplicación.

### <a name="module-deactivation"></a>Desactivación de módulo
Muchos módulos ofrecen una opción de configuración que le permitirá hacer deshabilitarlas sin quitarlos de la aplicación. Se trata de la forma más sencilla y rápida para desactivar un módulo. Por ejemplo, si desea deshabilitar el módulo URL Rewrite de IIS, use el `<httpRedirect>` elemento tal y como se muestra a continuación. Para obtener más información acerca de cómo deshabilitar módulos con opciones de configuración, siga los vínculos en la *elementos secundarios* sección de [IIS `<system.webServer>` ](https://docs.microsoft.com/iis/configuration/system.webServer/).

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

### <a name="module-removal"></a>Eliminación de módulo
Si decide para quitar un módulo con una configuración en *web.config*, debe desbloquear el módulo y desbloquear el `<modules>` sección de *web.config* primero. Los pasos se describen a continuación:

1. Desbloquear el módulo en el nivel de servidor. Haga clic en el servidor IIS en el Administrador de IIS **conexiones** sidebar. Abra la **módulos** en el **IIS** área. Haga clic en el módulo en la lista. En el **acciones** sidebar a la derecha, haga clic en **Unlock**. Desbloquear tantos módulos como piensa quitar con *web.config* más tarde.

2. Implementar la aplicación sin un `<modules>` sección *web.config*. Si implementa una aplicación con un *web.config* que contiene el `<modules>` sección sin necesidad de desbloquear la sección en primer lugar en el Administrador de IIS, el Administrador de configuración iniciará una excepción al intentar desbloquear la sección. Por lo tanto, implementar la aplicación sin un `<modules>` sección.

3. Desbloquear la `<modules>` sección de *web.config*. En el **conexiones** sidebar, haga clic en el sitio Web en **sitios**. En el **administración** área, abra el **Editor de configuración**. Utilice los controles de navegación para seleccionar la `system.webServer/modules` sección. En el **acciones** sidebar a la derecha, haga clic en a **Unlock** la sección.

4. En este momento, podrá agregar un `<modules>` sección a su *web.config* de archivos con un `<remove>` elemento que se va a quitar el módulo de la aplicación. Puede agregar varias `<remove>` elementos que se va a quitar varios módulos. No olvide que si realiza *web.config* cambios en el servidor para que sean inmediatamente en el proyecto de forma local. Si quita un módulo de esta manera no afectará a su uso del módulo con otras aplicaciones en el servidor.

  ```xml
  <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
  </configuration>
  ```

Para una instalación de IIS con los módulos predeterminados instalado, puede utilizar la siguiente `<module>` sección para desinstalar los módulos predeterminados.

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

También puede quitar un módulo IIS con *Appcmd.exe*. Proporcionar la `MODULE_NAME` y `APPLICATION_NAME` en el comando que se muestra a continuación:

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

Aquí se muestra cómo quitar el `DynamicCompressionModule` desde el sitio Web predeterminado:

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimal-module-configuration"></a>Configuración mínima del módulo
Los módulos solo necesarios para ejecutar una aplicación de ASP.NET Core son el módulo de autenticación anónima y el módulo principal de ASP.NET.

![Abra el Administrador de IIS a módulos con la configuración mínima del módulo que se muestra](iis-modules/_static/modules.png)

## <a name="resources"></a>Recursos
* [Publicar en IIS](xref:publishing/iis)
* [Información general de los módulos IIS](https://docs.microsoft.com/iis/get-started/introduction-to-iis/iis-modules-overview)
* [Personalización de IIS 7.0 Roles y módulos](https://technet.microsoft.com/library/cc627313.aspx)
* [IIS`<system.webServer>`](https://docs.microsoft.com/iis/configuration/system.webServer/)
