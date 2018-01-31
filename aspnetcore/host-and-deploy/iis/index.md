---
title: Hospedaje de ASP.NET Core en Windows con IIS
author: guardrex
description: "Obtenga información sobre cómo hospedar aplicaciones de ASP.NET Core en Windows Server Internet Information Services (IIS)."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/index
ms.openlocfilehash: 1df438af2394f41b686413cd1ce5ad73a9416ec5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>Hospedaje de ASP.NET Core en Windows con IIS

Por [Luke Latham](https://github.com/guardrex) y [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="supported-operating-systems"></a>Sistemas operativos admitidos

Los siguientes sistemas operativos son compatibles:

* Windows 7 y más recientes
* Windows Server 2008 R2 y más recientes&#8224;

&#8224;Conceptualmente, la configuración de IIS que se describe en este documento también se aplica al hospedaje de aplicaciones de ASP.NET Core en IIS de Nano Server, pero vea [ASP.NET Core con IIS en Nano Server](xref:tutorials/nano-server) para obtener instrucciones concretas.

El [servidor HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente denominado [WebListener](xref:fundamentals/servers/weblistener)) no funciona en una configuración de proxy inverso con IIS. Use el [servidor Kestrel](xref:fundamentals/servers/kestrel).

## <a name="iis-configuration"></a>Configuración de IIS

Habilite el rol **Servidor web (IIS)** y establezca los servicios de rol.

### <a name="windows-desktop-operating-systems"></a>Sistemas operativos de escritorio Windows

Vaya a **Panel de control** > **Programas** > **Programas y características** > **Activar o desactivar las características de Windows** (lado izquierdo de la pantalla). Abra el grupo de **Internet Information Services** y **Herramientas de administración web**. Active la casilla de **Consola de administración de IIS**. Active la casilla de **Servicios World Wide Web**. Acepte las características predeterminadas de **Servicios World Wide Web** o personalice las características de IIS.

![Consola de administración de IIS y Servicios World Wide Web se activan en Características de Windows.](index/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a>Sistemas operativos de servidor Windows

En los sistemas operativos de servidor, use el asistente **Agregar roles y características** a través del menú **Administrar** o el vínculo de **Administrador del servidor**. En el paso **Roles de servidor**, active la casilla de **Servidor web (IIS)**.

![El rol Servidor web (IIS) se activa en el paso Seleccionar roles de servidor.](index/_static/server-roles-ws2016.png)

En el paso **Servicios de rol**, seleccione los servicios de rol IIS que quiera o acepte los servicios de rol predeterminados proporcionados.

![Los servicios de rol predeterminados se activan en el paso Seleccionar servicios de rol.](index/_static/role-services-ws2016.png)

Continúe con el paso **Confirmación** para instalar el rol y los servicios de servidor web. No es necesario reiniciar el servidor ni IIS después de instalar el rol Servidor web (IIS).

## <a name="install-the-net-core-windows-server-hosting-bundle"></a>Instalación del lote de hospedaje .NET Core Windows Server

1. Instale el [lote de hospedaje .NET Core Windows Server](https://aka.ms/dotnetcore-2-windowshosting) en el sistema host. El lote instala .NET Core Runtime, .NET Core Library y el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). El módulo crea el proxy inverso entre IIS y el servidor Kestrel. Si el sistema no tiene conexión a Internet, obtenga e instale [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) antes de instalar el lote de hospedaje .NET Core Windows Server.

2. Reinicie el sistema o ejecute **net stop was /y** seguido de **net start w3svc** desde un símbolo del sistema para obtener un cambio en la ruta de acceso del sistema.

> [!NOTE]
> Para obtener información sobre la configuración compartida de IIS, vea [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration) (Módulo de ASP.NET Core con configuración compartida de IIS).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Instalación de Web Deploy al publicar con Visual Studio

Al implementar aplicaciones en servidores con [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), instale la versión más reciente de Web Deploy en el servidor. Para instalar Web Deploy, use el [Instalador de plataforma web (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) u obtenga un instalador directamente desde el [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=43717). El método preferido es usar WebPI. WebPI ofrece una instalación independiente y una configuración para los proveedores de hospedaje.

## <a name="application-configuration"></a>Configuración de aplicación

### <a name="enabling-the-iisintegration-components"></a>Habilitación de los componentes de IISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Los archivos *Program.cs* estándar llaman a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para empezar a configurar un host. `CreateDefaultBuilder` configura [Kestrel](xref:fundamentals/servers/kestrel) como el servidor web y habilita IIS Integration configurando la ruta de acceso base y el puerto para el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module):

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Incluya una dependencia en el paquete [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) de dependencias de la aplicación. Incorpore middleware de IIS Integration a la aplicación al agregar el método de extensión *UseIISIntegration* a *WebHostBuilder*:

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

Tanto `UseKestrel` como `UseIISIntegration` son necesarios. La llamada al código `UseIISIntegration` no afecta a la portabilidad del código. Si la aplicación no se ejecuta detrás de IIS (por ejemplo, si se ejecuta directamente en Kestrel), `UseIISIntegration` no es posible realizar ninguna operación.

---

Para obtener más información sobre el hospedaje, consulte [Hosting in ASP.NET Core](xref:fundamentals/hosting) (Hospedaje en ASP.NET Core).

### <a name="iis-options"></a>Opciones de IIS

Para configurar las opciones de IIS, incluya una configuración de servicio para `IISOptions` en `ConfigureServices`:

```csharp
services.Configure<IISOptions>(options => 
{
    ...
});
```

| Opción                         | Default | Parámetro |
| ------------------------------ | ------- | ------- |
| `AutomaticAuthentication`      | `true`  | Si `true`, el middleware de autenticación configura el `HttpContext.User` y responde a los desafíos genéricos. Si `false`, el middleware de autenticación solo proporciona una identidad (`HttpContext.User`) y responde a los desafíos cuando se le solicita explícitamente mediante el `AuthenticationScheme`. Autenticación de Windows debe estar habilitado en IIS para que `AutomaticAuthentication` funcione. |
| `AuthenticationDisplayName`    | `null`  | Establece el nombre para mostrar que se muestra a los usuarios en las páginas de inicio de sesión. |
| `ForwardClientCertificate`     | `true`  | Si `HttpContext.Connection.ClientCertificate` y el encabezado de solicitud `true` está presente, se rellena `MS-ASPNETCORE-CLIENTCERT`. |

### <a name="webconfig"></a>web.config

El objetivo principal del archivo *web.config* consiste en configurar el [módulo de ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). Puede que, como alternativa, proporcione una configuración de IIS adicional. La creación, transformación y publicación de *web.config* se controlan con el SDK web de .NET Core (`Microsoft.NET.Sdk.Web`). El SDK se establece en la parte superior del archivo del proyecto, `<Project Sdk="Microsoft.NET.Sdk.Web">`. Para evitar que el SDK transforme el archivo *web.config*, agregue la propiedad **\<IsTransformWebConfigDisabled>** al archivo del proyecto con un valor de `true`:

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Si el proyecto incluye un archivo *web.config*, se transforma con los elementos *processPath* y *arguments* correctos para configurar el [módulo de ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) y se mueve a la [salida publicada](xref:host-and-deploy/directory-structure). La transformación no modifica los valores de configuración de IIS del archivo.

### <a name="webconfig-location"></a>Ubicación de web.config

Las aplicaciones de .NET Core se hospedan a través de un servidor proxy inverso entre IIS y el servidor Kestrel. Para crear el servidor proxy inverso, el archivo *web.config* debe estar presente en la ruta de acceso raíz del contenido (normalmente la ruta de acceso base de la aplicación) de la aplicación implementada, que es la ruta de acceso física del sitio web proporcionada a IIS. El archivo *web.config* debe estar en la raíz de la aplicación para habilitar la publicación de varias aplicaciones mediante Web Deploy.

Los archivos confidenciales están en la ruta de acceso física de la aplicación, incluidas las subcarpetas, como *\<nombre_de_ensamblado>.runtimeconfig.json*, *\<nombre_de_ensamblado>.xml* (comentarios de documentación XML) y *\<nombre_de_ensamblado>.deps.json*. Cuando el archivo *web.config* está presente y configura el sitio, IIS impide que se proporcionen estos archivos confidenciales. **Por lo tanto, es importante que el archivo *web.config* no se cambie de nombre ni se quite de la implementación accidentalmente.**

## <a name="create-the-iis-website"></a>Crear el sitio web de IIS

1. En el sistema IIS de destino, cree una carpeta que contenga las carpetas y los archivos publicados de la aplicación, que se describen en [Estructura de directorios](xref:host-and-deploy/directory-structure).

2. Dentro de la carpeta, cree una carpeta *logs* para los registros de stdout cuando esté habilitado el registro de stdout. Si la aplicación se ha implementado con una carpeta *logs* en la carga, omita este paso. Para obtener instrucciones sobre la manera en que MSBuild crea la carpeta *logs*, vea el tema [Directory structure](xref:host-and-deploy/directory-structure) (Estructura de directorios).

3. En **Administrador de IIS**, cree un nuevo sitio web. Proporcione el **Nombre del sitio** y establezca la **Ruta de acceso física** a la carpeta de implementación de la aplicación. Proporcione la configuración de **Enlace** y cree el sitio web.

4. Establezca el grupo de aplicaciones en **Sin código administrado**. ASP.NET Core se ejecuta en un proceso independiente y administra el runtime.

5. Abra la ventana **Agregar sitio web**.

   ![Haga clic en "Agregar sitio web" en el menú contextual de Sitios.](index/_static/add-website-context-menu-ws2016.png)

6. Configure el sitio web.

   ![Proporcione el nombre del sitio, la ruta de acceso física y el nombre de host en el paso Agregar sitio web.](index/_static/add-website-ws2016.png)

7. En el panel **Grupos de aplicaciones**, abra la ventana **Modificar grupo de aplicaciones**. Para ello, haga clic con el botón derecho en el grupo de aplicaciones del sitio web y seleccione **Configuración básica...** en el menú emergente.

   ![Seleccione Configuración básica en el menú contextual del grupo de aplicaciones.](index/_static/apppools-basic-settings-ws2016.png)

8. Establezca la **Versión de .NET CLR** en **Sin código administrado**.

   ![Establezca Sin código administrado para Versión de .NET CLR.](index/_static/edit-apppool-ws2016.png)
     
    Nota: El establecimiento de **Versión de .NET CLR** en **Sin código administrado** es opcional. ASP.NET Core no se basa en la carga de CLR de escritorio.

9. Confirme que la identidad del modelo de proceso tiene los permisos adecuados.

   Si cambia la identidad predeterminada del grupo de aplicaciones (**Modelo de proceso** > **Identidad**) de **ApplicationPoolIdentity** a otra identidad, compruebe que la nueva identidad tenga los permisos necesarios para obtener acceso a la carpeta de la aplicación, la base de datos y otros recursos necesarios.
   
## <a name="deploy-the-app"></a>Implementación de la aplicación

Implemente la aplicación en la carpeta que ha creado en el sistema IIS de destino. [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) es el mecanismo recomendado para la implementación.

Confirme que la aplicación publicada para la implementación no se está ejecutando. Los archivos de la carpeta *publish* quedan bloqueados mientras se ejecuta la aplicación. Los archivos bloqueados no se pueden sobrescribir. Para liberar archivos bloqueados de una implementación, detenga el grupo de aplicaciones:

* Manualmente en el Administrador de IIS en el servidor.
* Mediante Web Deploy con una referencia a `Microsoft.NET.Sdk.Web` en el archivo de proyecto. Se coloca un archivo *app_offline.htm* en la raíz del directorio de aplicación web. Cuando el archivo está presente, el módulo de ASP.NET Core cierra correctamente la aplicación y proporciona el archivo *app_offline.htm* durante la implementación. Para más información, vea [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm) (Referencia de configuración del módulo de ASP.NET Core).
* Use PowerShell para detener y reiniciar el grupo de aplicaciones (requiere PowerShell 5 o posterior):
  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

### <a name="web-deploy-with-visual-studio"></a>Web Deploy con Visual Studio

Vea el tema [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) (Perfiles de publicación de Visual Studio para la implementación de aplicaciones de ASP.NET Core) para obtener más información sobre cómo crear un perfil de publicación para su uso con Web Deploy. Si el proveedor de hospedaje proporciona un perfil de publicación o admite la creación de uno, descargue su perfil e impórtelo mediante el cuadro de diálogo **Publicar** de Visual Studio.

![Cuadro de diálogo Publicar](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Web Deploy fuera de Visual Studio

También puede usar [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) fuera de Visual Studio desde la línea de comandos. Para más información, vea [Web Deployment Tool (Herramienta de implementación web)](/iis/publish/using-web-deploy/use-the-web-deployment-tool).

### <a name="alternatives-to-web-deploy"></a>Alternativas a Web Deploy

Use cualquiera de los métodos disponibles para mover la aplicación al sistema de hospedaje, como Xcopy, Robocopy o PowerShell. Los usuarios de Visual Studio pueden usar los [Ejemplos de publicación](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).

## <a name="browse-the-website"></a>Examinar el sitio web

![El explorador Microsoft Edge ha cargado la página de inicio de IIS.](index/_static/browsewebsite.png)

## <a name="data-protection"></a>Protección de datos

Hay varios middlewares de ASP.NET que usan la protección de datos, incluidos los empleados en la autenticación. Incluso si no se llama a las API de protección de datos desde el código del usuario, se debe configurar la protección de datos con un script de implementación o en el código de usuario para crear un almacén de claves persistente. Si no se configura la protección de datos, las claves se conservan en memoria y se descartan cuando se reinicia la aplicación.

Si el conjunto de claves se almacena en memoria cuando se reinicia la aplicación:

* Todos los tokens de autenticación de formularios se invalidan. 
* Los usuarios tienen que iniciar sesión de nuevo en la siguiente solicitud. 
* Ya no se pueden descifrar los datos protegidos con el conjunto de claves.

Para configurar la protección de datos en IIS, use **uno** de los enfoques siguientes:

### <a name="create-a-data-protection-registry-hive"></a>Crear un subárbol del Registro de protección de datos

Las claves de protección de datos usadas por las aplicaciones de ASP.NET se almacenan en subárboles del Registro externos a las aplicaciones. Para conservar las claves de una determinada aplicación, cree un subárbol del Registro para el grupo de aplicaciones.

En las instalaciones independientes de IIS que no son de granja de servidores web, puede usar el [script de PowerShell Provision-AutoGenKeys.ps1 de protección de datos](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) para cada grupo de aplicaciones usado con una aplicación de ASP.NET Core. Este script crea una clave del Registro especial en el registro HKLM que solo se incluye en la ACL de la cuenta de proceso de trabajo. Las claves se cifran en reposo mediante DPAPI con una clave de equipo.

En escenarios de granja de servidores web, una aplicación puede configurarse para usar una ruta de acceso UNC para almacenar su conjunto de claves de protección de datos. De forma predeterminada, las claves de protección de datos no se cifran. Asegúrese de que los permisos de archivo de un recurso compartido de este tipo se limitan a la cuenta de Windows como la que se ejecuta la aplicación. Además, puede usar un certificado X509 para proteger las claves en reposo. Considere un mecanismo que permita a los usuarios cargar certificados: coloque los certificados en el almacén de certificados de confianza del usuario y asegúrese de que están disponibles en todos los equipos en los que se ejecuta la aplicación del usuario. Vea [Configuración de la protección de datos](xref:security/data-protection/configuration/overview) para obtener detalles.

### <a name="configure-the-iis-application-pool-to-load-the-user-profile"></a>Configurar el grupo de aplicaciones de IIS para cargar el perfil de usuario

Esta opción está en la sección **Modelo de proceso**, en la **Configuración avanzada** del grupo de aplicaciones. Establezca Cargar perfil de usuario en `True`. Esto almacena las claves en el directorio del perfil de usuario y las protege mediante DPAPI con una clave específica de la cuenta de usuario usada para el grupo de aplicaciones.

### <a name="use-the-file-system-as-a-key-ring-store"></a>Usar el sistema de archivos como un almacén de conjunto de claves

Ajuste el código de la aplicación para [usar el sistema de archivos como un almacén de conjunto de claves](xref:security/data-protection/configuration/overview). Use un certificado X509 para proteger el conjunto de claves y asegúrese de que sea un certificado de confianza. Si es un certificado autofirmado, colóquelo en el almacén raíz de confianza.

Cuando se usa IIS en una granja de servidores web:

* Use un recurso compartido de archivos al que puedan acceder todos los equipos.
* Implemente un certificado X509 en cada equipo. Configure la [protección de datos en el código](xref:security/data-protection/configuration/overview).

### <a name="set-a-machine-wide-policy-for-data-protection"></a>Establecimiento de una directiva a nivel de equipo para la protección de datos

El sistema de protección de datos tiene compatibilidad limitada con el establecimiento de una [directiva de equipo](xref:security/data-protection/configuration/machine-wide-policy) predeterminada para todas las aplicaciones que usan las API de protección de datos. Vea la documentación de [protección de datos](xref:security/data-protection/index) para obtener más detalles.

## <a name="configuration-of-sub-applications"></a>Configuración de aplicaciones secundarias

Las aplicaciones secundarias agregadas bajo la aplicación raíz no deben incluir el módulo de ASP.NET Core como controlador. Si se agrega el módulo como controlador al archivo *web.config* de una aplicación secundaria, aparece un error 500.19 (Error interno del servidor) que hace referencia al archivo de configuración erróneo al intentar examinar la aplicación secundaria. En el ejemplo siguiente se muestra el contenido de un archivo publicado *web.config* para una aplicación secundaria de ASP.NET Core:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Al hospedar una aplicación secundaria que no es de ASP.NET Core bajo una aplicación de ASP.NET Core, quite explícitamente el controlador heredado del archivo *web.config* de la aplicación secundaria:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Para más información sobre la configuración del módulo de ASP.NET Core, vea el tema [Introducción al módulo de ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) y [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module) (Referencia de configuración del módulo de ASP.NET Core).

## <a name="configuration-of-iis-with-webconfig"></a>Configuración de IIS con web.config

La configuración de IIS aún se ve afectada por la sección **\<system.webServer>** de *web.config* en las características de IIS que se aplican a una configuración de proxy inverso. Si IIS está configurado en el nivel de sistema para usar compresión dinámica, puede deshabilitar esa configuración para una aplicación con el elemento **\<urlCompression>** del archivo *web.config* de la aplicación. Para más información, vea la [referencia de configuración para \<system.webServer>](https://docs.microsoft.com/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module) (Referencia de configuración del módulo de ASP.NET Core) y [Using IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules) (Uso de módulos de IIS con ASP.NET Core). Si tiene que establecer variables de entorno para aplicaciones individuales que se ejecutan en grupos de aplicaciones aislados (se admite en IIS 10.0+), vea la sección *AppCmd.exe command* (Comando AppCmd.exe) del tema [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) (Variables de entorno <environmentVariables>) de la documentación de referencia de IIS.

## <a name="configuration-sections-of-webconfig"></a>Secciones de configuración de web.config

Las aplicaciones de ASP.NET Core no usan para la configuración las secciones de configuración de aplicaciones de ASP.NET Framework en *web.config*:

* **\<system.web>**
* **\<appSettings>**
* **\<connectionStrings>**
* **\<location>**

Las aplicaciones de ASP.NET Core se configuran mediante otros proveedores de configuración. Para obtener más información, vea [Configuración](xref:fundamentals/configuration/index).

## <a name="application-pools"></a>Grupos de aplicaciones

Al hospedar varios sitios web en un único sistema, aísle las aplicaciones entre sí mediante la ejecución de cada una de ellas en su propio grupo de aplicaciones. El valor predeterminado del cuadro de diálogo **Agregar sitio web** es esta configuración. Cuando se proporciona el **Nombre del sitio**, el texto se transfiere automáticamente al cuadro de texto **Grupo de aplicaciones**. Al agregar el sitio web se crea un grupo de aplicaciones con el nombre del sitio.

## <a name="application-pool-identity"></a>Identidad del grupo de aplicaciones

Una cuenta de identidad del grupo de aplicaciones permite ejecutar una aplicación en una cuenta única sin tener que crear ni administrar dominios o cuentas locales. En IIS 8.0 y versiones posteriores, el proceso de trabajo de administración de IIS (WAS) crea una cuenta virtual con el nombre del nuevo grupo de aplicaciones y ejecuta los procesos de trabajo del grupo de aplicaciones en esta cuenta de forma predeterminada. En la Consola de administración de IIS, en la opción **Configuración avanzada** del grupo de aplicaciones, asegúrese de que la **Identidad** está establecida para usar **ApplicationPoolIdentity**:

![Cuadro de diálogo Configuración avanzada del grupo de aplicaciones](index/_static/apppool-identity.png)

El proceso de administración de IIS crea un identificador seguro con el nombre del grupo de aplicaciones en el sistema de seguridad de Windows. Los recursos se pueden proteger mediante esta identidad, aunque no es una cuenta de usuario real ni se muestra en la consola de administración de usuario de Windows.

Si el proceso de trabajo de IIS requiere acceso con privilegios elevados a la aplicación, modifique la lista de control de acceso (ACL) del directorio que contiene la aplicación:

1. Abra el Explorador de Windows y vaya al directorio.

2. Haga clic con el botón derecho en el directorio y seleccione **Propiedades**.

3. En la pestaña **Seguridad**, haga clic en el botón **Editar** y en el botón **Agregar**.

4. Haga clic en el botón **Ubicaciones** y asegúrese de seleccionar el sistema.

5. Escriba **IIS AppPool\DefaultAppPool** en el cuadro de texto **Escribir los nombres de objeto para seleccionar**.

   ![Selección del cuadro de diálogo de usuarios o grupos para la carpeta de la aplicación](index/_static/select-users-or-groups-1.png)

6. Haga clic en el botón **Comprobar nombres**. Seleccione **Aceptar**.

   ![Selección del cuadro de diálogo de usuarios o grupos para la carpeta de la aplicación](index/_static/select-users-or-groups-2.png)

También puede hacerlo mediante un símbolo del sistema con la herramienta **ICACLS**:

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="additional-resources"></a>Recursos adicionales

* [Solución de problemas de ASP.NET Core en IIS](xref:host-and-deploy/iis/troubleshoot)
* [Referencia de errores comunes de Azure App Service e IIS con ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Introducción al módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Referencia de configuración del módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Uso de módulos de IIS con ASP.NET Core](xref:host-and-deploy/iis/modules)
* [Introducción a ASP.NET Core](../index.md)
* [Sitio oficial de Microsoft IIS](https://www.iis.net/)
* [Biblioteca de TechNet de Microsoft: Windows Server](https://docs.microsoft.com/windows-server/windows-server-versions)
