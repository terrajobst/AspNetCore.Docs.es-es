---
title: Hospedaje de ASP.NET Core en Windows con IIS
author: guardrex
description: "Configuración de Windows Server Internet Information Services (IIS) e implementación de aplicaciones de ASP.NET Core."
keywords: "ASP.NET Core,internet information services,iis,windows server,lote de hospedaje,módulo asp.net core,implementación web"
ms.author: riande
manager: wpickett
ms.date: 03/13/2017
ms.topic: article
ms.assetid: a4449ad3-5bad-410c-afa7-dc32d832b552
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/iis
ms.openlocfilehash: e9e9019d5b879498e8800bb579c177dd3ad64061
ms.sourcegitcommit: 96af03c9f44f7c206e68ae3ef8596068e6b4e5fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2017
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>Hospedaje de ASP.NET Core en Windows con IIS

Por [Luke Latham](https://github.com/guardrex) y [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="supported-operating-systems"></a>Sistemas operativos admitidos

Los siguientes sistemas operativos son compatibles:

* Windows 7 y más recientes
* Windows Server 2008 R2 y más recientes&#8224;

&#8224;Conceptualmente, la configuración de IIS que se describe en este documento también se aplica al hospedaje de aplicaciones de ASP.NET Core en IIS de Nano Server, pero vea [ASP.NET Core con IIS en Nano Server](xref:tutorials/nano-server) para obtener instrucciones concretas.

El [servidor HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente denominado [WebListener](xref:fundamentals/servers/weblistener)) no funciona en una configuración de proxy inverso con IIS. Debe usar el [servidor Kestrel](xref:fundamentals/servers/kestrel).

## <a name="iis-configuration"></a>Configuración de IIS

Habilite el rol **Servidor web (IIS)** y establezca los servicios de rol.

### <a name="windows-desktop-operating-systems"></a>Sistemas operativos de escritorio Windows

Vaya a **Panel de control** > **Programas** > **Programas y características** > **Activar o desactivar las características de Windows** (lado izquierdo de la pantalla). Abra el grupo de **Internet Information Services** y **Herramientas de administración web**. Active la casilla de **Consola de administración de IIS**. Active la casilla de **Servicios World Wide Web**. Acepte las características predeterminadas de **Servicios World Wide Web** o personalice las características de IIS para satisfacer sus necesidades.

![Consola de administración de IIS y Servicios World Wide Web se activan en Características de Windows.](iis/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a>Sistemas operativos de servidor Windows

En los sistemas operativos de servidor, use el asistente **Agregar roles y características** a través del menú **Administrar** o el vínculo de **Administrador del servidor**. En el paso **Roles de servidor**, active la casilla de **Servidor web (IIS)**.

![El rol Servidor web (IIS) se activa en el paso Seleccionar roles de servidor.](iis/_static/server-roles-ws2016.png)

En el paso **Servicios de rol**, seleccione los servicios de rol IIS que quiera o acepte los servicios de rol predeterminados proporcionados.

![Los servicios de rol predeterminados se activan en el paso Seleccionar servicios de rol.](iis/_static/role-services-ws2016.png)

Continúe con el paso **Confirmación** para instalar el rol y los servicios de servidor web. No es necesario reiniciar el servidor ni IIS después de instalar el rol Servidor web (IIS).

## <a name="install-the-net-core-windows-server-hosting-bundle"></a>Instalación del lote de hospedaje .NET Core Windows Server

1. Instale el [lote de hospedaje .NET Core Windows Server](https://download.microsoft.com/download/5/C/1/5C190037-632B-443D-842D-39085F02E1E8/DotNetCore.2.0.3-WindowsHosting.exe) en el sistema host. El lote instala .NET Core Runtime, .NET Core Library y el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). El módulo crea el proxy inverso entre IIS y el servidor Kestrel. Si el sistema no tiene conexión a Internet, obtenga e instale [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) antes de instalar el lote de hospedaje .NET Core Windows Server.

2. Reinicie el sistema o ejecute **net stop was /y** seguido de **net start w3svc** desde un símbolo del sistema para obtener un cambio en la ruta de acceso del sistema.

> [!NOTE]
> Si usa una configuración compartida de IIS, vea [ASP.NET Core Module with IIS Shared Configuration](xref:hosting/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration) (Módulo ASP.NET Core con configuración compartida de IIS).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Instalación de Web Deploy al publicar con Visual Studio

Si piensa implementar las aplicaciones con [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) en [Visual Studio](https://www.visualstudio.com/vs/), instale la versión más reciente de Web Deploy en el sistema host. Para instalar Web Deploy, puede usar el [Instalador de plataforma web (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) u obtener un instalador directamente desde el [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=43717). El método preferido es usar WebPI. WebPI ofrece una instalación independiente y una configuración para los proveedores de hospedaje.

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

Tanto `UseKestrel` como `UseIISIntegration` son necesarios. La llamada al código *UseIISIntegration* no afecta a la portabilidad del código. Si la aplicación no se ejecuta detrás de IIS (por ejemplo, si se ejecuta directamente en Kestrel), `UseIISIntegration` no es posible realizar ninguna operación.

---

Para obtener más información sobre el hospedaje, consulte [Hosting in ASP.NET Core](xref:fundamentals/hosting) (Hospedaje en ASP.NET Core).

### <a name="iis-options"></a>Opciones de IIS

Para configurar las opciones del servicio *IISIntegration*, incluya una configuración del servicio para *IISOptions* en *ConfigureServices*:

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

El archivo *web.config* primero configura el módulo ASP.NET Core. Puede que, como alternativa, proporcione una configuración de IIS adicional. La creación, transformación y publicación de *web.config* se controlan con el SDK web de .NET Core (`Microsoft.NET.Sdk.Web`). El SDK se establece en la parte superior del archivo del proyecto (*.csproj*), `<Project Sdk="Microsoft.NET.Sdk.Web">`. Para evitar que el SDK transforme el archivo *web.config*, agregue la propiedad **\<IsTransformWebConfigDisabled>** al archivo del proyecto con un valor de `true`:

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Si no tiene un archivo *web.config* en el proyecto al publicar con *dotnet publish* o con Visual Studio, se crea el archivo en la [salida publicada](xref:hosting/directory-structure). Si tiene un archivo *web.config* en el proyecto, se transforma con los elementos *processPath* y *arguments* correctos para configurar el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) y se mueve a la salida publicada. La transformación no toca los valores de configuración de IIS que se han incluido en el archivo.

## <a name="create-the-iis-website"></a>Crear el sitio web de IIS

1. En el sistema IIS de destino, cree una carpeta que contenga las carpetas y los archivos publicados de la aplicación, que se describen en [Estructura de directorios](xref:hosting/directory-structure).

2. Dentro de la carpeta que ha creado, cree una carpeta de *registros* para contener los registros stdout (si tiene previsto habilitar los registros para solucionar problemas de inicio). Si piensa implementar la aplicación con una carpeta de *registros* en la carga, puede omitir este paso. Hay un [problema sin solucionar para crear la carpeta de forma automática](https://github.com/aspnet/AspNetCoreModule/issues/30). Si quiere que MSBuild cree la carpeta de *registros* automáticamente, agregue el siguiente elemento `Target` al archivo de proyecto:

   ```xml
   <Target Name="CreateLogsFolder" AfterTargets="AfterPublish">
     <MakeDir Directories="$(PublishDir)logs" Condition="!Exists('$(PublishDir)logs')" />
     <MakeDir Directories="$(PublishUrl)logs" Condition="!Exists('$(PublishUrl)logs')" />
   </Target>
   ```

3. En **Administrador de IIS**, cree un nuevo sitio web. Proporcione el **Nombre del sitio** y establezca la **Ruta de acceso física** a la carpeta de implementación de la aplicación que ha creado. Proporcione la configuración de **Enlace** y cree el sitio web.

4. Establezca el grupo de aplicaciones en **Sin código administrado**. ASP.NET Core se ejecuta en un proceso independiente y administra el runtime.

5. Abra la ventana **Agregar sitio web**.

   ![Haga clic en Agregar sitio web en el menú contextual de Sitios.](iis/_static/add-website-context-menu-ws2016.png)

6. Configure el sitio web.

   ![Proporcione el nombre del sitio, la ruta de acceso física y el nombre de host en el paso Agregar sitio web.](iis/_static/add-website-ws2016.png)

7. En el panel **Grupos de aplicaciones**, abra la ventana **Modificar grupo de aplicaciones**. Para ello, haga clic con el botón derecho en el grupo de aplicaciones del sitio web y seleccione **Configuración básica...** en el menú emergente.

   ![Seleccione Configuración básica en el menú contextual del grupo de aplicaciones.](iis/_static/apppools-basic-settings-ws2016.png)

8. Establezca la **Versión de .NET CLR** en **Sin código administrado**.

   ![Establezca Sin código administrado para Versión de .NET CLR.](iis/_static/edit-apppool-ws2016.png)
     
    Nota: El establecimiento de **Versión de .NET CLR** en **Sin código administrado** es opcional. ASP.NET Core no se basa en la carga de CLR de escritorio.

9. Confirme que la identidad del modelo de proceso tiene los permisos adecuados.

   Si cambia la identidad predeterminada del grupo de aplicaciones (**Modelo de proceso** > **Identidad**) de **ApplicationPoolIdentity** a otra identidad, compruebe que la nueva identidad tenga los permisos necesarios para acceder a la carpeta de la aplicación, la base de datos y otros recursos necesarios.
   
## <a name="deploy-the-application"></a>Implementar la aplicación
Implemente la aplicación en la carpeta que ha creado en el sistema IIS de destino. [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) es el mecanismo recomendado para la implementación. A continuación se ofrecen alternativas a Web Deploy.

Confirme que la aplicación publicada para la implementación no se está ejecutando. Los archivos de la carpeta *publish* quedan bloqueados mientras se ejecuta la aplicación. No se puede llevar a cabo la implementación porque no se pueden copiar los archivos bloqueados.

### <a name="web-deploy-with-visual-studio"></a>Web Deploy con Visual Studio
Vea el tema [Create publish profiles for Visual Studio and MSBuild, to deploy ASP.NET Core apps](xref:publishing/web-publishing-vs#publish-profiles) (Crear perfiles de publicación para Visual Studio y MSBuild para implementar aplicaciones de ASP.NET Core) para aprender a crear un perfil de publicación para su uso con Web Deploy. Si el proveedor de hospedaje proporciona un perfil de publicación o admite la creación de uno, descargue su perfil e impórtelo mediante el cuadro de diálogo **Publicar** de Visual Studio.

![Cuadro de diálogo Publicar](iis/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Web Deploy fuera de Visual Studio
También puede usar [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) fuera de Visual Studio desde la línea de comandos. Para más información, vea [Web Deployment Tool (Herramienta de implementación web)](/iis/publish/using-web-deploy/use-the-web-deployment-tool).

### <a name="alternatives-to-web-deploy"></a>Alternativas a Web Deploy
Si no quiere usar Web Deploy o no usa Visual Studio, puede emplear cualquiera de los diversos métodos para mover la aplicación al sistema host, como Xcopy, Robocopy o PowerShell. Los usuarios de Visual Studio pueden usar los [Ejemplos de publicación](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).

## <a name="browse-the-website"></a>Examinar el sitio web
![El explorador Microsoft Edge ha cargado la página de inicio de IIS.](iis/_static/browsewebsite.png)
   
> [!WARNING]
> Las aplicaciones de .NET Core se hospedan a través de un servidor proxy inverso entre IIS y el servidor Kestrel. Para crear el servidor proxy inverso, el archivo *web.config* debe estar presente en la ruta de acceso raíz del contenido (normalmente la ruta de acceso base de la aplicación) de la aplicación implementada, que es la ruta de acceso física del sitio web proporcionada a IIS. Los archivos confidenciales están en la ruta de acceso física de la aplicación, incluidas las subcarpetas, como *my_application.runtimeconfig.json*, *my_application.xml* (comentarios de documentación XML) y *my_application.deps.json*. El archivo *web.config* es necesario para crear el servidor proxy inverso para Kestrel, que evita que IIS entregue estos y otros archivos confidenciales. **Por lo tanto, es importante que el archivo *web.config* no se cambie de nombre ni se quite de la implementación accidentalmente.**

## <a name="data-protection"></a>Protección de datos

Una aplicación de ASP.NET Core almacena el conjunto de claves en memoria con las siguientes condiciones:

* Que haya un sitio web hospedado tras IIS.
* Que la pila de protección de datos no se haya configurado para almacenar el conjunto de claves en un almacén persistente.

Si el conjunto de claves se almacena en memoria cuando se reinicia la aplicación:

* Todos los tokens de autenticación de formularios se invalidan. 
* Los usuarios tienen que iniciar sesión de nuevo en la siguiente solicitud. 
* Los datos protegidos con el conjunto de claves ya no lo están.

> [!WARNING]
> Hay varios middlewares de ASP.NET que usan la protección de datos, incluidos los empleados en la autenticación. Aunque no llame a ninguna API de protección de datos desde su propio código, tiene que configurar la protección de datos con un script de implementación o en su código. Si no se configura la protección de datos, las claves se conservan en memoria y se descartan cuando se reinicia la aplicación de forma predeterminada. Al reiniciar se invalidan las cookies escritas por el middleware de autenticación con cookies y los usuarios tienen que iniciar sesión de nuevo.

Para configurar la protección de datos en IIS se debe usar uno de los enfoques siguientes:

* Ejecute un [script de PowerShell](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) para crear entradas del Registro adecuadas (por ejemplo, `.\Provision-AutoGenKeys.ps1 DefaultAppPool`). Esto almacena las claves en el Registro, protegidas mediante DPAPI con una clave de equipo.
* Configure el grupo de aplicaciones de IIS para cargar el perfil de usuario. Esta opción está en la sección **Modelo de proceso**, en la **Configuración avanzada** del grupo de aplicaciones. Establezca **Cargar perfil de usuario** en `True`. Esto almacena las claves en el directorio del perfil de usuario y las protege mediante DPAPI con una clave específica de la cuenta de usuario usada para el grupo de aplicaciones.
* Ajuste el código de la aplicación para [usar el sistema de archivos como un almacén de conjunto de claves](xref:security/data-protection/configuration/overview). Use un certificado X509 para proteger el conjunto de claves y asegúrese de que sea un certificado de confianza. Si es un certificado autofirmado, debe colocarlo en el almacén raíz de confianza.

Cuando se usa IIS en una granja de servidores web:

* Use un recurso compartido de archivos al que puedan acceder todos los equipos.
* Implemente un certificado X509 en cada equipo. Configure la [protección de datos en el código](https://docs.microsoft.com/aspnet/core/security/data-protection/configuration/overview).

### <a name="1-create-a-data-protection-registry-hive"></a>1. Crear un subárbol del Registro de protección de datos

Las claves de protección de datos usadas por las aplicaciones de ASP.NET se almacenan en subárboles del Registro externos a las aplicaciones. Para conservar las claves de una determinada aplicación, debe crear un subárbol del Registro para el grupo de aplicaciones de la aplicación.

En las instalaciones independientes de IIS, puede usar el [script de PowerShell Provision-AutoGenKeys.ps1 de protección de datos](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) para cada grupo de aplicaciones usado con una aplicación de ASP.NET Core. Este script crea una clave del Registro especial en el registro HKLM que solo se incluye en la ACL de la cuenta de proceso de trabajo. Las claves se cifran en reposo con DPAPI.

En escenarios de granja de servidores web, una aplicación puede configurarse para usar una ruta de acceso UNC para almacenar su conjunto de claves de protección de datos. De forma predeterminada, las claves de protección de datos no se cifran. Debe asegurarse de que los permisos de archivo de un recurso compartido de este tipo se limitan a la cuenta de Windows como la que se ejecuta la aplicación. Además puede optar por proteger las claves en reposo con un certificado X509. Es posible que quiera considerar un mecanismo que permita a los usuarios cargar certificados: coloque los certificados en el almacén de certificados de confianza del usuario y asegúrese de que están disponibles en todos los equipos en los que se ejecuta la aplicación del usuario. Vea [Configuración de la protección de datos](xref:security/data-protection/configuration/overview) para obtener detalles.

### <a name="2-configure-the-iis-application-pool-to-load-the-user-profile"></a>2. Configurar el grupo de aplicaciones de IIS para cargar el perfil de usuario

Esta opción está en la sección **Modelo de proceso**, en la **Configuración avanzada** del grupo de aplicaciones. Establezca Cargar perfil de usuario en `True`. Esto almacena las claves en el directorio del perfil de usuario y las protege mediante DPAPI con una clave específica de la cuenta de usuario usada para el grupo de aplicaciones.

### <a name="3-machine-wide-policy-for-data-protection"></a>3. Directiva a nivel de equipo para la protección de datos

El sistema de protección de datos tiene compatibilidad limitada con el establecimiento de una [directiva de equipo](xref:security/data-protection/configuration/machine-wide-policy) predeterminada para todas las aplicaciones que usan las API de protección de datos. Vea la documentación de [protección de datos](xref:security/data-protection/index) para obtener más detalles.

## <a name="configuration-of-sub-applications"></a>Configuración de aplicaciones secundarias

Las aplicaciones secundarias agregadas bajo la aplicación raíz no deben incluir el módulo ASP.NET Core como controlador. Si agrega el módulo como controlador al archivo *web.config* de una aplicación secundaria, aparece un error 500.19 (Error interno del servidor) que hace referencia al archivo de configuración erróneo al intentar examinar la aplicación secundaria. En el ejemplo siguiente se muestra el contenido de un archivo publicado *web.config* para una aplicación secundaria de ASP.NET Core:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Si piensa hospedar una aplicación secundaria que no es de ASP.NET Core bajo una aplicación de ASP.NET Core, debe quitar explícitamente el controlador heredado del archivo *web.config* de la aplicación secundaria:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Para más información sobre la configuración del módulo ASP.NET Core con el archivo *web.config*, vea el tema [Introducción al módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) y [Referencia de configuración del módulo ASP.NET Core](xref:hosting/aspnet-core-module).

## <a name="configuration-of-iis-with-webconfig"></a>Configuración de IIS con web.config

La configuración de IIS aún se ve afectada por la sección `<system.webServer>` de *web.config* en las características de IIS que se aplican a una configuración de proxy inverso. Por ejemplo, es posible que tenga IIS configurado en el nivel de sistema para usar compresión dinámica, aunque podría deshabilitar esa configuración para una aplicación con el elemento `<urlCompression>` del archivo *web.config* de la aplicación. Para más información, vea la [referencia de configuración para `<system.webServer>`](https://docs.microsoft.com/iis/configuration/system.webServer/), [Referencia de configuración del módulo ASP.NET Core](xref:hosting/aspnet-core-module) y [Uso de módulos de IIS con ASP.NET Core](xref:hosting/iis-modules). Si tiene que establecer variables de entorno para aplicaciones individuales que se ejecutan en grupos de aplicaciones aislados (se admite en IIS 10.0+), vea la sección *Comando AppCmd.exe* del tema [Variables de entorno \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) de la documentación de referencia de IIS.

## <a name="configuration-sections-of-webconfig"></a>Secciones de configuración de web.config

A diferencia de las aplicaciones de .NET Framework que se configuran con los elementos `<system.web>`, `<appSettings>`, `<connectionStrings>` y `<location>` de *web.config*, las aplicaciones de ASP.NET Core se configuran con otros proveedores de configuración. Para obtener más información, vea [Configuración](xref:fundamentals/configuration).

## <a name="application-pools"></a>Grupos de aplicaciones

Al hospedar varios sitios web en un único sistema, debe aislar las aplicaciones entre sí mediante la ejecución de cada una de ellas en su propio grupo de aplicaciones. El valor predeterminado del cuadro de diálogo **Agregar sitio web** es este comportamiento. Cuando se proporciona el **Nombre del sitio**, el texto se transfiere automáticamente al cuadro de texto **Grupo de aplicaciones**. Al agregar el sitio web se crea un nuevo grupo de aplicaciones con el nombre del sitio.

## <a name="application-pool-identity"></a>Identidad del grupo de aplicaciones

Una cuenta de identidad del grupo de aplicaciones permite ejecutar una aplicación en una cuenta única sin tener que crear ni administrar dominios o cuentas locales. En IIS 8.0+, el proceso de trabajo de administración de IIS (WAS) crea una cuenta virtual con el nombre del nuevo grupo de aplicaciones y ejecuta los procesos de trabajo del grupo de aplicaciones en esta cuenta de forma predeterminada. En la Consola de administración de IIS, en la opción **Configuración avanzada** del grupo de aplicaciones, asegúrese de que la **Identidad** está establecida para usar **ApplicationPoolIdentity**, como se muestra en la imagen siguiente.

![Cuadro de diálogo Configuración avanzada del grupo de aplicaciones](iis/_static/apppool-identity.png)

El proceso de administración de IIS crea un identificador seguro con el nombre del grupo de aplicaciones en el sistema de seguridad de Windows. Los recursos se pueden proteger mediante esta identidad, aunque no es una cuenta de usuario real ni se muestra en la consola de administración de usuario de Windows.

Si tiene que conceder al proceso de trabajo de IIS acceso elevado a la aplicación, debe modificar la lista de control de acceso (ACL) del directorio que contiene la aplicación.

1. Abra el Explorador de Windows y vaya al directorio.

2. Haga clic con el botón derecho en el directorio y luego haga clic en **Propiedades**.

3. En la pestaña **Seguridad**, haga clic en el botón **Editar** y luego en el botón **Agregar**.

4. Haga clic en el botón **Ubicaciones** y asegúrese de seleccionar el sistema.

5. Escriba **IIS AppPool\DefaultAppPool** en el cuadro de texto **Escribir los nombres de objeto para seleccionar**.

  ![Selección del cuadro de diálogo de usuarios o grupos para la carpeta de la aplicación](iis/_static/select-users-or-groups-1.png)

6. Haga clic en el botón **Comprobar nombres** y luego en **Aceptar**.

  ![Selección del cuadro de diálogo de usuarios o grupos para la carpeta de la aplicación](iis/_static/select-users-or-groups-2.png)

También puede hacerlo mediante un símbolo del sistema con la herramienta **ICACLS**:

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="troubleshooting-tips"></a>Sugerencias para la solución de problemas

Para diagnosticar problemas en las implementaciones de IIS:

* Estudie la salida del explorador.
* Examine el registro de **aplicaciones** del sistema mediante el **Visor de eventos**.
* Habilite el registro `stdout`. El registro del **módulo ASP.NET Core** se encuentra en la ruta de acceso proporcionada en el atributo *stdoutLogFile* del elemento `<aspNetCore>` de *web.config*. Las carpetas de la ruta de acceso proporcionada en el valor del atributo deben existir en la implementación. También debe establecer *stdoutLogEnabled="true"*. Las aplicaciones que usan el SDK de `Microsoft.NET.Sdk.Web` para crear el archivo *web.config* tienen como valor predeterminado *stdoutLogEnabled* establecido en *false*, por lo que debe proporcionar manualmente el archivo *web.config* o modificar el archivo con el fin de habilitar los registros `stdout`.

Algunos de los errores comunes no aparecen en el explorador, el registro de aplicaciones ni el registro del módulo ASP.NET Core hasta que se ha pasado el módulo *startupTimeLimit* (valor predeterminado: 120 segundos) y *startupRetryCount* (valor predeterminado: 2). Por lo tanto, espere seis minutos completos antes de deducir que el módulo no ha iniciado un proceso de la aplicación.

Una manera rápida de determinar si la aplicación funciona correctamente es ejecutarla directamente en Kestrel. Si la aplicación se ha publicado como una implementación dependiente del marco de trabajo (FDD), ejecute `dotnet my_application.dll` en la carpeta de implementación, que es la ruta de acceso física de IIS a la aplicación. Si la aplicación se ha publicado como una implementación independiente (SCD), ejecute el ejecutable de la aplicación directamente desde un símbolo del sistema, `my_application.exe`, en la carpeta de implementación. Si Kestrel está escuchando en el puerto predeterminado 5000, debe ser capaz de examinar la aplicación en `http://localhost:5000/`. Si la aplicación responde normalmente en la dirección del punto de conexión Kestrel, lo más probable es que el problema esté relacionado con la configuración de IIS-módulo ASP.NET Core-Kestrel y menos con la propia aplicación.

Una manera de determinar si el servidor proxy inverso de IIS para el servidor Kestrel funciona correctamente es realizar una solicitud de archivo estático simple de una hoja de estilos, un script o una imagen de los archivos estáticos de la aplicación en *wwwroot* con [middleware de archivo estático](xref:fundamentals/static-files). Si la aplicación puede entregar archivos estáticos pero se producen errores en las vistas de MVC y otros puntos de conexión, es menos probable que el problema esté relacionado con la configuración de IIS-módulo ASP.NET Core-Kestrel y más con la propia aplicación (por ejemplo, el enrutamiento MVC o un error interno del servidor 500).

Si Kestrel se inicia normalmente tras IIS pero la aplicación no se ejecuta en el sistema después de hacerlo correctamente en local, puede agregar provisionalmente una variable de entorno a *web.config* para establecer `ASPNETCORE_ENVIRONMENT` en `Development`. Siempre y cuando no se invalide el entorno al inicio de la aplicación, esto permite que la [página de excepción para el desarrollador](xref:fundamentals/error-handling) aparezca cuando la aplicación se ejecuta en el sistema. Solo se recomienda establecer la variable de entorno para `ASPNETCORE_ENVIRONMENT` de este modo para los sistemas de almacenamiento o pruebas que no están expuestos a Internet. Asegúrese de quitar la variable de entorno del archivo *web.config* cuando termine. Para más información sobre cómo establecer variables de entorno mediante el archivo *web.config* del proxy inverso, vea [environmentVariables child element of aspNetCore](xref:hosting/aspnet-core-module#setting-environment-variables) (Elemento secundario environmentVariables de aspNetCore).

En la mayoría de los casos, la habilitación del registro de la aplicación ayuda a solucionar problemas con el proxy inverso o la aplicación. Vea [Logging](xref:fundamentals/logging/index) (Registro) para más información.

La última sugerencia de solución de problemas se refiere a las aplicaciones que no se ejecutan después de actualizar el SDK de .NET Core en el equipo de desarrollo o las versiones de paquete en la aplicación. En algunos casos, los paquetes incoherentes pueden interrumpir una aplicación al realizar actualizaciones importantes. Puede corregir la mayoría de estos problemas mediante la eliminación de las carpetas `bin` y `obj` del proyecto, el borrado de las memorias caché de paquete en `%UserProfile%\.nuget\packages\` y `%LocalAppData%\Nuget\v3-cache`, la restauración del proyecto y la confirmación de que la implementación anterior en el sistema se ha eliminado por completo antes de volver a implementar la aplicación.

> [!TIP]
> Una manera cómoda de borrar las memorias caché de paquete es obtener la herramienta *NuGet.exe* en [NuGet.org](https://www.nuget.org/), agregarla a la ruta de acceso del sistema y ejecutar `nuget locals all -clear` desde un símbolo del sistema. También puede ejecutar el comando `dotnet nuget locals all --clear` desde un símbolo del sistema sin obtener *NuGet.exe*.

## <a name="common-errors"></a>Errores comunes

La siguiente no es una lista completa de errores. Si detecta un error que no aparece aquí, deje un mensaje de error detallado en la sección de comentarios siguiente.

### <a name="installer-unable-to-obtain-vc-redistributable"></a>El instalador no puede obtener VC++ Redistributable

* **Excepción del instalador:** 0x80072efd o 0x80072f76 - Error no especificado

* **Excepción del registro del instalador&#8224;:** Error 0x80072efd o 0x80072f76: Error al ejecutar el paquete EXE

  &#8224;El registro se encuentra en C:\Users\\{USUARIO}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.

Solución del problema:

* Si el sistema no tiene acceso a Internet al instalar el lote de hospedaje del servidor, se produce esta excepción cuando se evita que el instalador obtenga *Microsoft Visual C++ 2015 Redistributable*. Puede obtener un instalador en el [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=53840). Si se produce un error en el instalador, es posible que no reciba el runtime de .NET Core necesario para hospedar una implementación dependiente del marco de trabajo (FDD). Si va a hospedar una FDD, confirme que el runtime está instalado en Programas y características. Puede obtener un instalador de runtime en [Descargas de .NET](https://www.microsoft.com/net/download/core). Después de instalar el runtime, reinicie el sistema o IIS al ejecutar **net stop was /y** seguido de **net start w3svc** desde un símbolo del sistema.

### <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>La actualización del sistema operativo ha quitado el módulo ASP.NET Core de 32 bits

* **Registro de aplicación:** Error al cargar el archivo DLL del módulo **C:\WINDOWS\system32\inetsrv\aspnetcore.dll**. Los datos son el error.

Solución del problema:

* Los archivos que no son de SO del directorio **C:\Windows\SysWOW64\inetsrv** no se conservan durante una actualización del sistema operativo. Si ha instalado el módulo ASP.NET Core antes de una actualización del sistema operativo y luego intenta ejecutar cualquier grupo de aplicaciones en modo de 32 bits después de una actualización del sistema operativo, se produce este problema. Después de actualizar el sistema operativo, repare el módulo ASP.NET Core. Vea [Instalación del lote de hospedaje .NET Core Windows Server](#install-the-net-core-windows-server-hosting-bundle). Seleccione **Reparar** al ejecutar el instalador.

### <a name="platform-conflicts-with-rid"></a>Conflictos de plataforma con RID

* **Explorador:** Error HTTP 502.5 - Error en el proceso

* **Registro de aplicación:** La aplicación 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' con raíz física ' C:\{PATH}\' no pudo iniciar el proceso con la línea de comandos '"C:\\{PATH}\my_application.{exe|dll}" ', ErrorCode = '0x80004005 : ff.

* **Registro del módulo ASP.NET Core:** Excepción no controlada: System.BadImageFormatException: No se pudo cargar el archivo o ensamblado 'my_application.dll'. Se ha intentado cargar un programa con un formato incorrecto.

Solución del problema:

* Confirme que la aplicación se ejecuta localmente en Kestrel. Un error de proceso puede ser el resultado de un problema en la aplicación. Para más información, vea [Sugerencias para la solución de problemas](#troubleshooting-tips).

* Confirme que no ha establecido un elemento `<PlatformTarget>` en *.csproj* que entre en conflicto con el RID. Por ejemplo, no especifique un elemento `<PlatformTarget>` de `x86` ni publique con un RID de `win10-x64`, ya sea mediante *dotnet publish -c Release -r win10-x64* o al establecer el elemento `<RuntimeIdentifiers>` de *.csproj*  en `win10-x64`. El proyecto se publica sin advertencias ni errores, pero se produce un error con las excepciones registradas anteriores en el sistema.

* Si esta excepción se produce en una implementación de aplicaciones de Azure al actualizar una aplicación e implementar ensamblados más recientes, elimine manualmente todos los archivos de la implementación anterior. Los ensamblados persistentes no compatibles pueden producir una excepción `System.BadImageFormatException` al implementar una aplicación actualizada.

### <a name="uri-endpoint-wrong-or-stopped-website"></a>Punto de conexión de URI incorrecto o sitio web detenido

* **Explorador:** ERR_CONNECTION_REFUSED

* **Registro de aplicación:** Sin entrada

* **Registro del módulo ASP.NET Core:** Archivo de registro no creado

Solución del problema:

* Confirme que usa el punto de conexión de URI correcto para la aplicación. Compruebe los enlaces.

* Confirme que el sitio web de IIS no está en estado *Detenido*.

### <a name="corewebengine-or-w3svc-server-features-disabled"></a>Características de servidor CoreWebEngine o W3SVC deshabilitadas

* **Excepción de sistema operativo:** Se deben instalar las características de IIS 7.0 CoreWebEngine y W3SVC para usar el módulo ASP.NET Core.

Solución del problema:

* Confirme que ha habilitado el rol y las características correctos. Vea [Configuración de IIS](#iis-configuration).

### <a name="incorrect-website-physical-path-or-application-missing"></a>Ruta de acceso física de sitio web incorrecta o aplicación que falta

* **Explorador:** 403 Prohibido - Acceso denegado **--O BIEN--** 403.14 Prohibido - El servidor web está configurado para no mostrar una lista de los contenidos de este directorio.

* **Registro de aplicación:** Sin entrada

* **Registro del módulo ASP.NET Core:** Archivo de registro no creado

Solución del problema:

* Vea la **Configuración básica** del sitio web de IIS y la carpeta de la aplicación física. Confirme que la aplicación está en la carpeta en la **ruta de acceso física** del sitio web de IIS.

### <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a>Rol incorrecto, módulo no instalado o permisos incorrectos

* **Explorador:** 500.19 Error interno del servidor - No se puede obtener acceso a la página solicitada porque los datos de configuración relacionados de la página no son válidos.

* **Registro de aplicación:** Sin entrada

* **Registro del módulo ASP.NET Core:** Archivo de registro no creado

Solución del problema:

* Confirme que ha habilitado el rol correcto. Vea [Configuración de IIS](#iis-configuration).

* Compruebe **Programas &amp; Características** y confirme que el **módulo Microsoft ASP.NET Core** se ha instalado. Si el **módulo Microsoft ASP.NET Core** no aparece en la lista de programas instalados, instálelo. Vea [Instalación del lote de hospedaje .NET Core Windows Server](#install-the-net-core-windows-server-hosting-bundle).

* Asegúrese de que **Grupo de aplicaciones** > **Modelo de proceso** > **Identidad** esté establecido en **ApplicationPoolIdentity** o de que la identidad personalizada tenga los permisos correctos para acceder a la carpeta de implementación de la aplicación.

### <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>Elemento processPath incorrecto, variable PATH que falta, lote de hospedaje no instalado, sistema o IIS no reiniciado, VC++ Redistributable no instalado o infracción de acceso de dotnet.exe

* **Explorador:** Error HTTP 502.5 - Error en el proceso

* **Registro de aplicación:** La aplicación 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' con raíz física 'C:\\{PATH}\' no pudo iniciar el proceso con la línea de comandos '".\my_application.exe" ', ErrorCode = '0x80070002 : 0.

* **Registro del módulo ASP.NET Core:** Archivo de registro creado pero vacío

Solución del problema:

* Confirme que la aplicación se ejecuta localmente en Kestrel. Un error de proceso puede ser el resultado de un problema en la aplicación. Para más información, vea [Sugerencias para la solución de problemas](#troubleshooting-tips).

* Compruebe el atributo *processPath* del elemento `<aspNetCore>` de *web.config* para confirmar que es *dotnet* para una implementación dependiente del marco de trabajo (FDD) o *.\my_application.exe* para una implementación independiente (SCD).

* En el caso de una FDD, *dotnet.exe* podría no ser accesible a través del valor PATH. Confirme que *C:\Archivos de programa\dotnet\* existe en el valor PATH del sistema.

* En el caso de una FDD, *dotnet.exe* podría no ser accesible para la identidad del usuario del grupo de aplicaciones. Confirme que la identidad del usuario de AppPool tiene acceso al directorio *C:\Archivos de programa\dotnet*. Confirme que no haya ninguna regla de denegación configurada para la identidad del usuario de AppPool en los directorios *C:\Archivos de programa\dotnet* y de la aplicación.

* Es posible que haya realizado una FDD e instalado .NET Core sin reiniciar IIS. Ejecute **net stop was /y** seguido de **net start w3svc** desde un símbolo del sistema para reiniciar el servidor o IIS.

* Quizás haya realizado una FDD sin instalar el runtime de .NET Core en el sistema host. Si está intentando realizar una FDD y no ha instalado el runtime de .NET Core, ejecute el **instalador del lote de hospedaje .NET Core Windows Server** en el sistema. Vea [Instalación del lote de hospedaje .NET Core Windows Server](#install-the-net-core-windows-server-hosting-bundle). Si está intentando instalar el runtime de .NET Core en un sistema sin conexión a Internet, obtenga el runtime en [Descargas de .NET](https://www.microsoft.com/net/download/core) y ejecute el instalador del lote de hospedaje para instalar el módulo ASP.NET Core. Para completar la instalación, reinicie el sistema o IIS mediante la ejecución de **net stop was /y** seguido de **net start w3svc** desde un símbolo del sistema.

* Es posible que haya realizado una FDD e instalado .NET Core sin reiniciar el sistema o IIS. Ejecute **net stop was /y** seguido de **net start w3svc** desde un símbolo del sistema para reiniciar el sistema o IIS.

* Quizás haya realizado una FDD y *Microsoft Visual C++ 2015 Redistributable (x64)* no esté instalado en el sistema. Puede obtener un instalador en el [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=53840).

### <a name="incorrect-arguments-of-aspnetcore-element"></a>Argumentos incorrectos del elemento \<aspNetCore\>

* **Explorador:** Error HTTP 502.5 - Error en el proceso

* **Registro de aplicación:** La aplicación 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' con raíz física 'C:\\{PATH}\' no pudo iniciar el proceso con la línea de comandos '"dotnet" .\my_application.dll', ErrorCode = '0x80004005 : 80008081.

* **Registro del módulo ASP.NET Core:** La aplicación que se va a ejecutar no existe: 'PATH\my_application.dll'

Solución del problema:

* Confirme que la aplicación se ejecuta localmente en Kestrel. Un error de proceso puede ser el resultado de un problema en la aplicación. Para más información, vea [Sugerencias para la solución de problemas](#troubleshooting-tips).

* Examine el atributo *arguments* del elemento `<aspNetCore>` de *web.config* para confirmar que (a) es *.\my_application.dll* para una implementación dependiente del marco de trabajo (FDD); o (b) no está presente, es una cadena vacía (*arguments=""*) o una lista de argumentos de la aplicación (*arguments="arg1, arg2, ..."*) para una implementación independiente (SCD).

### <a name="missing-net-framework-version"></a>Falta la versión de .NET Framework

* **Explorador:** 502.3 Puerta de enlace incorrecta - Error de conexión al intentar enrutar la solicitud.

* **Registro de aplicación:** ErrorCode = La aplicación 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' con raíz física 'C:\\{PATH}\' no pudo iniciar el proceso con la línea de comandos '"dotnet" .\my_application.dll', ErrorCode = '0x80004005 : 80008081.

* **Registro del módulo ASP.NET Core:** Excepción de falta de método, archivo o ensamblado. El método, el archivo o el ensamblado especificado en la excepción es un método, archivo o ensamblado de .NET Framework.

Solución del problema:

* Instale la versión de .NET Framework que falta en el sistema.

* En el caso de una implementación dependiente del marco de trabajo (FDD), confirme que tiene instalado el runtime correcto en el sistema. Si actualiza un proyecto de 1.1 a 2.0, implementa en el sistema host y recibe esta excepción, asegúrese de instalar la versión 2.0 del marco de trabajo en el sistema host.

### <a name="stopped-application-pool"></a>Grupo de aplicaciones detenido

* **Explorador:** 503 Servicio no disponible

* **Registro de aplicación:** Sin entrada

* **Registro del módulo ASP.NET Core:** Archivo de registro no creado

Solución de problemas

* Confirme que el grupo de aplicaciones no está en estado *Detenido*.

### <a name="iis-integration-middleware-not-implemented"></a>Middleware de IIS Integration no implementado

* **Explorador:** Error HTTP 502.5 - Error en el proceso

* **Registro de aplicación:** La aplicación 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' con raíz física 'C:\\{PATH}\' ha creado el proceso con la línea de comandos '"C:\\{PATH}\my_application.{exe|dll}" ', pero se ha bloqueado, no ha respondido o no ha escuchado en el puerto especificado '{PUERTO}', ErrorCode = '0x800705b4'

* **Registro del módulo ASP.NET Core:** Archivo de registro creado que muestra un funcionamiento normal.

Solución de problemas

* Confirme que la aplicación se ejecuta localmente en Kestrel. Un error de proceso puede ser el resultado de un problema en la aplicación. Para más información, vea [Sugerencias para la solución de problemas](#troubleshooting-tips).

* Confirme que ha hecho referencia correctamente al middleware de IIS Integration mediante una llamada al método *.UseIISIntegration()* en el elemento *WebHostBuilder()* de la aplicación (ASP.NET Core 1.x) o ha usado el método `CreateDefaultBuilder` (ASP.NET Core 2.x). Vea [Hospedar en ASP.NET Core](xref:fundamentals/hosting) para obtener detalles.

### <a name="sub-application-includes-a-handlers-section"></a>La aplicación secundaria incluye una sección de \<controladores\>

* **Explorador:** Error HTTP 500.19 - Error interno del servidor

* **Registro de aplicación:** Sin entrada

* **Registro del módulo ASP.NET Core:** Archivo de registro creado que muestra un funcionamiento normal de la aplicación raíz. Archivo de registro no creado para la aplicación secundaria.

Solución de problemas

* Confirme que el archivo *web.config* de la aplicación secundaria no incluye una sección `<handlers>`.

### <a name="application-configuration-general-issue"></a>Problema general de configuración de aplicación

* **Explorador:** Error HTTP 502.5 - Error en el proceso

* **Registro de aplicación:** La aplicación 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' con raíz física 'C:\\{PATH}\' ha creado el proceso con la línea de comandos '"C:\\{PATH}\my_application.{exe|dll}" ', pero se ha bloqueado, no ha respondido o no ha escuchado en el puerto especificado '{PUERTO}', ErrorCode = '0x800705b4'

* **Registro del módulo ASP.NET Core:** Archivo de registro creado pero vacío

Solución de problemas

* Esta excepción general indica que el proceso no se ha iniciado, probablemente debido a un problema de configuración de la aplicación. Vea [Estructura de directorios](xref:hosting/directory-structure) para confirmar que los archivos y las carpetas implementados de la aplicación son adecuados y que los archivos de configuración de la aplicación están presentes y contienen la configuración correcta para la aplicación y el entorno. Para más información, vea [Sugerencias para la solución de problemas](#troubleshooting-tips).

## <a name="resources"></a>Recursos

* [Introducción al módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Referencia de configuración del módulo ASP.NET Core](xref:hosting/aspnet-core-module)
* [Uso de módulos de IIS con ASP.NET Core](xref:hosting/iis-modules)
* [Introducción a ASP.NET Core](../index.md)
* [Sitio oficial de Microsoft IIS](https://www.iis.net/)
* [Biblioteca de TechNet de Microsoft: Windows Server](https://docs.microsoft.com/windows-server/windows-server-versions)
