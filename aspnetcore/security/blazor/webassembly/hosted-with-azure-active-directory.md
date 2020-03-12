---
title: Proteja un ASP.NET Core Blazor aplicación hospedada en webassembly con Azure Active Directory
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-azure-active-directory
ms.openlocfilehash: 0803e436d66ef7df3c68739e674a8652fde11166
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083666"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-azure-active-directory"></a>Proteja un ASP.NET Core Blazor aplicación hospedada en webassembly con Azure Active Directory

Por [Javier Calvarro Nelson](https://github.com/javiercn) y [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]



En este artículo se describe cómo crear una [aplicación hospedada en WebassemblyBlazor](xref:blazor/hosting-models#blazor-webassembly) que usa [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) para la autenticación.

## <a name="register-apps-in-aad-b2c-and-create-solution"></a>Registrar aplicaciones en AAD B2C y crear una solución

### <a name="create-a-tenant"></a>Creación de un inquilino

Siga las instrucciones de [Inicio rápido: configuración de un inquilino](/azure/active-directory/develop/quickstart-create-new-tenant) para crear un inquilino en AAD.

### <a name="register-a-server-api-app"></a>Registrar una aplicación de API de servidor

Siga las instrucciones de [Inicio rápido: registro de una aplicación con la plataforma de Microsoft Identity y los](/azure/active-directory/develop/quickstart-register-app) temas de Azure AAD subsiguientes para registrar una aplicación de AAD para la *aplicación de API de servidor* en el área de **Azure Active Directory** > **registros de aplicaciones** de la Azure Portal:

1. Seleccione **Nuevo registro**.
1. Proporcione un **nombre** para la aplicación (por ejemplo, **Blazor servidor AAD**).
1. Elija un **tipo de cuenta compatible**. Solo puede seleccionar **cuentas en este directorio de la organización** (un solo inquilino) para esta experiencia.
1. La *aplicación de API de servidor* no requiere un **URI de redirección** en este escenario, por lo que deje la lista desplegable establecida en **Web** y no escriba un URI de redirección.
1. Deshabilite la casilla **permisos** > **conceder permisos de administrador a OpenID y offline_access** .
1. Seleccione **Registrar**.

En **permisos**de la API, quite el **Microsoft Graph** > **User. Read** , ya que la aplicación no requiere el inicio de sesión o el acceso a UER Profile.

En **exponer una API**:

1. Seleccione **Agregar un ámbito**.
1. Seleccione **Guardar y continuar**.
1. Proporcione un **nombre de ámbito** (por ejemplo, `API.Access`).
1. Proporcione un **nombre para mostrar del consentimiento del administrador** (por ejemplo, `Access API`).
1. Proporcione una **Descripción del consentimiento del administrador** (por ejemplo, `Allows the app to access server app API endpoints.`).
1. Confirme que el **Estado** está establecido en **habilitado**.
1. Seleccione la opción **Agregar un ámbito**.

Registre la siguiente información:

* *Aplicación de API de servidor* IDENTIFICADOR de aplicación (ID. de cliente) (por ejemplo, `11111111-1111-1111-1111-111111111111`)
* IDENTIFICADOR de directorio (ID. de inquilino) (por ejemplo, `222222222-2222-2222-2222-222222222222`)
* Dominio del inquilino de AAD (por ejemplo, `contoso.onmicrosoft.com`)
* Ámbito predeterminado (por ejemplo, `API.Access`)

### <a name="register-a-client-app"></a>Registrar una aplicación de cliente

Siga las instrucciones de [Inicio rápido: registro de una aplicación con la plataforma de Microsoft Identity y los](/azure/active-directory/develop/quickstart-register-app) temas de Azure AAD subsiguientes para registrar una aplicación de AAD para la *aplicación cliente* en el área de **Azure Active Directory** > **registros de aplicaciones** de la Azure Portal:

1. Seleccione **Nuevo registro**.
1. Proporcione un **nombre** para la aplicación (por ejemplo, **Blazor cliente AAD**).
1. Elija un **tipo de cuenta compatible**. Solo puede seleccionar **cuentas en este directorio de la organización** (un solo inquilino) para esta experiencia.
1. Deje la lista desplegable **URI de redirección** establecida en **Web**y proporcione un URI de redireccionamiento de `https://localhost:5001/authentication/login-callback`.
1. Deshabilite la casilla **permisos** > **conceder permisos de administrador a OpenID y offline_access** .
1. Seleccione **Registrar**.

En **autenticación** > **configuraciones de plataforma** > **Web**:

1. Confirme que el **URI de redirección** de `https://localhost:5001/authentication/login-callback` está presente.
1. En **concesión implícita**, active las casillas de verificación de **tokens de acceso** y **tokens de identificador**.
1. Los valores predeterminados restantes de la aplicación son aceptables para esta experiencia.
1. Seleccione el botón **Guardar**.

En **permisos de API**:

1. Confirme que la aplicación tiene **Microsoft Graph** > **usuario.** permiso de lectura.
1. Seleccione **Agregar un permiso** seguido de **mis API**.
1. Seleccione la *aplicación de API de servidor* en la columna **nombre** (por ejemplo, **Blazor servidor AAD**).
1. Abra la lista de **API** .
1. Habilite el acceso a la API (por ejemplo, `API.Access`).
1. Seleccione **Agregar permisos**.
1. Seleccione el botón **conceder contenido de administración para {nombre de inquilino}** . Seleccione **Sí** para confirmar la acción.

Registre el identificador de aplicación de la aplicación *cliente* (identificador de cliente) (por ejemplo, `33333333-3333-3333-3333-333333333333`).

### <a name="create-the-app"></a>Creación de la aplicación

Reemplace los marcadores de posición en el siguiente comando por la información registrada anteriormente y ejecute el comando en un shell de comandos:

```dotnetcli
dotnet new blazorwasm -au SingleOrg --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP CLIENT ID}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{DOMAIN}" -ho --tenant-id "{TENANT ID}"
```

Para especificar la ubicación de salida, que crea una carpeta de proyecto si no existe, incluya la opción output en el comando con una ruta de acceso (por ejemplo, `-o BlazorSample`). El nombre de la carpeta también se convierte en parte del nombre del proyecto.

> [!NOTE]
> Consulte la sección [compatibilidad con el servicio de autenticación](#Authentication service support) para obtener un cambio de configuración importante en el ámbito de token de acceso predeterminado. El valor proporcionado por la plantilla Blazor webassembly debe cambiarse manualmente después de crear la *aplicación cliente* a partir de la plantilla.

## <a name="server-app-configuration"></a>Configuración de la aplicación de servidor

*Esta sección pertenece a la aplicación de **servidor** de la solución.*

### <a name="authentication-package"></a>Paquete de autenticación

La compatibilidad para autenticar y autorizar llamadas a ASP.NET Core API Web la proporciona el `Microsoft.AspNetCore.Authentication.AzureAD.UI`:

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureAD.UI" 
    Version="3.1.0" />
```

### <a name="authentication-service-support"></a>Compatibilidad con el servicio de autenticación

El método `AddAuthentication` configura los servicios de autenticación dentro de la aplicación y configura el controlador de portador JWT como el método de autenticación predeterminado. El método `AddAzureADBearer` configura los parámetros específicos en el controlador de portador JWT necesario para validar los tokens emitidos por el Azure Active Directory:

```csharp
services.AddAuthentication(AzureADDefaults.BearerAuthenticationScheme)
    .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

`UseAuthentication` y `UseAuthorization` Asegúrese de que:

* La aplicación intenta analizar y validar los tokens en las solicitudes entrantes.
* Se produce un error en cualquier solicitud que intente obtener acceso a un recurso protegido sin credenciales adecuadas.

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="app-settings"></a>Configuración de la aplicación

El archivo *appSettings. JSON* contiene las opciones para configurar el controlador de portador JWT que se usa para validar los tokens de acceso.

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "{DOMAIN}",
    "TenantId": "{TENANT ID}",
    "ClientId": "{API CLIENT ID}",
  }
}
```

### <a name="weatherforecast-controller"></a>Controlador de WeatherForecast

El controlador WeatherForecast (*Controllers/WeatherForecastController. CS*) expone una API protegida con el `[Authorize]` atributo que se aplica al controlador. Es **importante** comprender esto:

* El `[Authorize]` atributo de este controlador de API es lo único que protege esta API frente al acceso no autorizado.
* El atributo `[Authorize]` usado en la aplicación webassembly de Blazor solo sirve como sugerencia a la aplicación de que el usuario debe estar autorizado para que la aplicación funcione correctamente.

```csharp
[Authorize]
[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
{
    [HttpGet]
    public IEnumerable<WeatherForecast> Get()
    {
        ...
    }
}
```

## <a name="client-app-configuration"></a>Configuración de la aplicación cliente

*Esta sección pertenece a la aplicación **cliente** de la solución.*

### <a name="authentication-package"></a>Paquete de autenticación

Cuando se crea una aplicación para usar cuentas profesionales o educativas (`SingleOrg`), la aplicación recibe automáticamente una referencia de paquete para la [biblioteca de autenticación de Microsoft](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`). El paquete proporciona un conjunto de primitivas que ayudan a la aplicación a autenticar a los usuarios y obtener tokens para llamar a las API protegidas.

Si agrega la autenticación a una aplicación, agregue manualmente el paquete al archivo de proyecto de la aplicación:

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

Reemplace `{VERSION}` en la referencia de paquete anterior por la versión del paquete de `Microsoft.AspNetCore.Blazor.Templates` que se muestra en el artículo <xref:blazor/get-started>.

El paquete de `Microsoft.Authentication.WebAssembly.Msal` agrega de manera transitiva el paquete de `Microsoft.AspNetCore.Components.WebAssembly.Authentication` a la aplicación.

### <a name="authentication-service-support"></a>Compatibilidad con el servicio de autenticación

La compatibilidad con la autenticación de usuarios se registra en el contenedor de servicios con el método de extensión `AddMsalAuthentication` proporcionado por el paquete de `Microsoft.Authentication.WebAssembly.Msal`. Este método configura todos los servicios necesarios para que la aplicación interactúe con el proveedor de identidades (IP).

*Program.cs*:

Cuando se genera la *aplicación cliente* , el ámbito del token de acceso predeterminado tiene el formato `api://{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}`. **Quite la parte `api://` del valor de ámbito.** Este problema se solucionará en una versión preliminar futura.

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}");
});
```

> [!NOTE]
> El ámbito del token de acceso predeterminado debe tener el formato `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (por ejemplo, `11111111-1111-1111-1111-111111111111/API.Access`). Si se proporciona un esquema o esquema y un host a la configuración de ámbito (como se muestra en Azure portal), la *aplicación cliente* genera una excepción no controlada cuando recibe una respuesta *401 no autorizada* de la *aplicación de API de servidor*.

El método `AddMsalAuthentication` acepta una devolución de llamada para configurar los parámetros necesarios para autenticar una aplicación. Los valores necesarios para configurar la aplicación pueden obtenerse a partir de la configuración de AAD de Azure portal cuando se registra la aplicación.

Los ámbitos de token de acceso predeterminados representan la lista de ámbitos de token de acceso que son:

* Se incluye de forma predeterminada en la solicitud de inicio de sesión.
* Se usa para aprovisionar un token de acceso inmediatamente después de la autenticación.

Todos los ámbitos deben pertenecer a la misma aplicación por reglas de Azure Active Directory. Se pueden agregar ámbitos adicionales para las aplicaciones de API adicionales según sea necesario:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{SCOPE}");
});
```

### <a name="index-page"></a>Página de índice

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

### <a name="app-component"></a>Componente de aplicación

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a>Componente RedirectToLogin

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a>Componente LoginDisplay

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a>Componente de autenticación

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a>Componente FetchData

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>Recursos adicionales

* <xref:security/authentication/azure-active-directory/index>
