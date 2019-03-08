---
title: Introducción a la autenticación para aplicaciones de página única en ASP.NET Core
author: javiercn
description: Usar la identidad con una única aplicación de página que se hospeda dentro de una aplicación ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 03/07/2019
uid: security/authentication/identity/spa
ms.openlocfilehash: 4afc9ac0a3c54b452c6a1b23e4de31d7e2fc5284
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665342"
---
# <a name="authentication-and-authorization-for-spas"></a>Autenticación y autorización para las spa

ASP.NET Core 3.0 o posterior ofrece una autenticación en aplicaciones de una sola página (SPA) mediante la compatibilidad para la autorización de API. ASP.NET Core Identity para autenticar y almacenar los usuarios se combina con [IdentityServer](https://identityserver.io/) para la implementación de OpenID Connect.

Se agregó un parámetro de autenticación a la **Angular** y **reaccionar** plantillas que es similar al parámetro de la autenticación de proyecto la **aplicación Web (Model-View-Controller)**  (MVC) y **aplicación Web** (las páginas de Razor) plantillas de proyecto. Los valores de parámetros permitidos son **ninguno** y **individuales**. El **React.js y Redux** plantilla de proyecto no es compatible con el parámetro de autenticación en este momento.

## <a name="create-an-app-with-api-authorization-support"></a>Crear una aplicación con el soporte técnico de autorización de API

Autorización y autenticación de usuario pueden utilizarse con Angular y React spa. Abra un shell de comandos y ejecute el siguiente comando:

**Angular**:

```console
dotnet new angular -o <output_directory_name> -au Individual
```

**React**:

```console
dotnet new react -o <output_directory_name> -au Individual
```

El comando anterior crea una aplicación de ASP.NET Core con un *ClientApp* directorio que contiene la SPA.

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a>Descripción general de los componentes de ASP.NET Core de la aplicación

Las secciones siguientes describen las adiciones al proyecto cuando se incluye compatibilidad con la autenticación:

### <a name="startup-class"></a>Clase de inicio

La `Startup` clase tiene las siguientes adiciones:

* Dentro de la `Startup.ConfigureServices` método:
  * Identidad con la interfaz de usuario predeterminada:

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * IdentityServer con más `AddApiAuthorization` método auxiliar que configuraciones algunas convenciones de ASP.NET Core en la parte superior IdentityServer de predeterminado:

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * Autenticación con otros `AddIdentityServerJwt` método auxiliar que configura la aplicación para validar los tokens JWT generado por IdentityServer:

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* Dentro de la `Startup.Configure` método:
  * El middleware de autenticación que se encarga de validar las credenciales de la solicitud y establecer el usuario en el contexto de solicitud:

    ```csharp
    app.UseAuthentication();
    ```

  * El middleware IdentityServer que expone los puntos de conexión de Open ID Connect:

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a>AddApiAuthorization

Este método auxiliar configura IdentityServer para usar la configuración admitida. IdentityServer es un marco eficaz y extensible para controlar cuestiones de seguridad de la aplicación. Al mismo tiempo, que expone una complejidad innecesaria para los escenarios más comunes. Por lo tanto, se proporciona un conjunto de convenciones y las opciones de configuración que se consideran un buen punto de partida. Una vez que la autenticación de sus necesidades evolucionan, toda la eficacia de IdentityServer sigue estando disponible para personalizar la autenticación para satisfacer sus necesidades.

### <a name="addidentityserverjwt"></a>AddIdentityServerJwt

Este método auxiliar configura una combinación de directivas para la aplicación como el controlador de autenticación predeterminado. La directiva está configurada para permitir que la identidad de controlar todas las solicitudes que se enrutan a cualquier subtrazado en el espacio de direcciones URL de identidad "/ identidad". El `JwtBearerHandler` controla todas las demás solicitudes. Además, este método registra un `<<ApplicationName>>API` recurso de API con IdentityServer con un ámbito predeterminado de `<<ApplicationName>>API` y configura el middleware del token de portador de JWT para validar los tokens emitidos por IdentityServer para la aplicación.

### <a name="sampledatacontroller"></a>SampleDataController

En el *Controllers\SampleDataController.cs* de archivo, tenga en cuenta el `[Authorize]` atributo aplicado a la clase que indica que el usuario debe tener autorización en función de la directiva predeterminada para tener acceso al recurso. La directiva de autorización predeterminado ocurre deberá estar configurado para usar el esquema de autenticación predeterminado, que se configura de forma `AddIdentityServerJwt` en el esquema de la directiva que se ha mencionado anteriormente, que hace el `JwtBearerHandler` configurado por el método de este tipo auxiliar del controlador predeterminado para solicitudes a la aplicación.

### <a name="applicationdbcontext"></a>ApplicationDbContext

En el *Data\ApplicationDbContext.cs* de archivos, haremos lo mismo `DbContext` identidad se usa con la excepción que extiende `ApiAuthorizationDbContext` (más derivado de clase de `IdentityDbContext`) para incluir el esquema para IdentityServer.

Para tener control total sobre el esquema de base de datos, hereda de una de las identidades disponibles `DbContext` clases y configurar el contexto para incluir el esquema de identidad mediante una llamada a `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` en el `OnModelCreating` método.

### <a name="oidcconfigurationcontroller"></a>OidcConfigurationController

En el *Controllers\OidcConfigurationController.cs* de archivo, tenga en cuenta el punto de conexión que se aprovisiona para dar servicio a los parámetros OIDC que el cliente debe usar.

### <a name="appsettingsjson"></a>appsettings.json

En el *appsettings.json* archivo de la raíz del proyecto, hay un nuevo `IdentityServer` configurado de sección que describe la lista de clientes. En el ejemplo siguiente, hay un solo cliente. El nombre del cliente corresponde con el nombre de la aplicación y se asigna por convención para el OAuth `ClientId` parámetro. El perfil indica el tipo de aplicación que se está configurando. Se usa internamente para las convenciones de la unidad que simplifican el proceso de configuración para el servidor. Hay varios perfiles disponibles, como se explica en el [perfiles de aplicación](#application-profiles) sección.

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a>appsettings.Development.json

En el *appsettings. Development.JSON* archivo de la raíz del proyecto, hay un `IdentityServer` sección que describe la clave utilizada para firmar los tokens. Al implementar en producción, debe una clave se aprovisione e implemente junto con la aplicación, como se explica en el [implementar en producción](#deploy-to-production) sección.

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="general-description-of-the-angular-app"></a>Descripción general de la aplicación Angular

Admiten la autenticación y autorización de API en el Angular plantilla reside en su propio módulo Angular en el *ClientApp\src\api autorización* directory. El módulo se compone de los siguientes elementos:

* 3 componentes:
  * *login.component.ts*: Controla el flujo de inicio de sesión de la aplicación.
  * *logout.component.ts*: Controla el flujo de cierre de sesión de la aplicación.
  * *login-menu.component.ts*: Un widget que muestra uno de los siguientes conjuntos de vínculos:
    * Administración de perfiles de usuario y vínculos cuando el usuario se autentica al cerrar la sesión.
    * Registro y registro en los vínculos cuando el usuario no está autenticado.
* Una restricción de ruta `AuthorizeGuard` que pueden agregarse a las rutas y requiere que el usuario se autentique antes de visitar la ruta.
* Un interceptor HTTP `AuthorizeInterceptor` que asocia el token de acceso a las solicitudes HTTP salientes destinadas a la API cuando se autentica el usuario.
* Un servicio `AuthorizeService` que controla los detalles de bajo nivel del proceso de autenticación y expone información sobre el usuario autenticado para el resto de la aplicación para su uso.
* Un módulo Angular que define las rutas asociadas a las partes de la autenticación de la aplicación. Expone el componente de menú de inicio de sesión, el interceptor, la protección y el servicio para el consumo del resto de la aplicación.

## <a name="general-description-of-the-react-app"></a>Descripción general de la aplicación React

La compatibilidad para la autenticación y autorización de API en la plantilla de React reside en el *ClientApp\src\components\api autorización* directory. Se compone de los siguientes elementos:

* 4 componentes:
  * *Login.js*: Controla el flujo de inicio de sesión de la aplicación.
  * *Logout.js*: Controla el flujo de cierre de sesión de la aplicación.
  * *LoginMenu.js*: Un widget que muestra uno de los siguientes conjuntos de vínculos:
    * Administración de perfiles de usuario y vínculos cuando el usuario se autentica al cerrar la sesión.
    * Registro y registro en los vínculos cuando el usuario no está autenticado.
  * *AuthorizeRoute.js*: Indica un componente de ruta que requiere que el usuario se autentique antes de representar el componente en el `Component` parámetro.
* Un exportado `authService` instancia de clase `AuthorizeService` que controla los detalles de bajo nivel del proceso de autenticación y expone información sobre el usuario autenticado para el resto de la aplicación para su uso.

Ahora que ha visto los componentes principales de la solución, se puede echar un vistazo más profundo en distintos escenarios para la aplicación.

## <a name="require-authorization-on-a-new-api"></a>Requerir autorización sobre una nueva API

De forma predeterminada, el sistema está configurado para requerir autorización fácilmente para las nuevas API. Para ello, cree un nuevo controlador y agregue el `[Authorize]` atributo a la clase de controlador o a cualquier acción en el controlador.

## <a name="protect-a-client-side-route-angular"></a>Proteger una ruta del lado cliente (Angular)

Protección de una ruta del lado cliente se realiza mediante la adición de la protección de autorizar a la lista de restricciones que se ejecutará cuando la configuración de una ruta. Por ejemplo, puede ver cómo el `fetch-data` ruta está configurada en el módulo de aplicación principal Angular:

```typescript
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

Es importante mencionar que la protección de una ruta no protege el punto de conexión real (que todavía se requiere un `[Authorize]` atributo aplicado a él), pero que solo se evita que el usuario de navegación a la ruta del lado cliente dada al que no está autenticado.

## <a name="authenticate-api-requests-angular"></a>Autenticar solicitudes de API (Angular)

Autenticar solicitudes a las API hospedadas por la aplicación se realiza automáticamente mediante el uso del interceptor de cliente HTTP definido por la aplicación.

## <a name="protect-a-client-side-route-react"></a>Proteger una ruta del lado cliente (React)

Proteger una ruta del lado cliente mediante el `AuthorizeRoute` componente en lugar de con el valor plain `Route` componente. Por ejemplo, observe cómo la `fetch-data` ruta está configurada en el `App` componente:

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

Protección de una ruta:

* No protege el punto de conexión real (que todavía se requiere un `[Authorize]` atributo aplicado a él).
* Solo impide al usuario navegar a la ruta del lado cliente dada al que no está autenticado.

## <a name="authenticate-api-requests-react"></a>Autenticar solicitudes de API (React)

Autenticar las solicitudes con React se realiza mediante la importación de la primera la `authService` instancia desde el `AuthorizeService`. El token de acceso se recupera de la `authService` y se adjunta a la solicitud, tal como se muestra a continuación. En los componentes de React, este trabajo se realiza normalmente en el `componentDidMount` método del ciclo de vida o que el resultado de una interacción del usuario.

### <a name="import-the-authservice-into-your-component"></a>Importar el authService su componente

```javascript
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a>Recuperar y adjunte el token de acceso a la respuesta

```javascript
async populateWeatherData() {
  const token = await authService.getAccessToken();
  const response = await fetch('api/SampleData/WeatherForecasts', {
    headers: !token ? {} : { 'Authorization': `Bearer ${token}` }
  });
  const data = await response.json();
  this.setState({ forecasts: data, loading: false });
}
```

## <a name="deploy-to-production"></a>Implementar en producción

Para implementar la aplicación en producción, deben aprovisionar los recursos siguientes:

* Una base de datos para almacenar las cuentas de usuario de identidad y las concesiones IdentityServer.
* Un certificado de producción que se usará para firmar los tokens.
  * No hay ningún requisito específico para este certificado; puede ser un certificado autofirmado o un certificado a través de una entidad CA.
  * Se puede generar a través de herramientas estándares, como PowerShell o OpenSSL.
  * Puede ser instalado en el almacén de certificados en los equipos de destino o implementar como un *.pfx* archivo con una contraseña segura.

### <a name="example-deploy-to-azure-websites"></a>Ejemplo: Implemente en Azure Websites

Esta sección describe la implementación de la aplicación en Azure websites que usan un certificado almacenado en el almacén de certificados. Para modificar la aplicación para cargar un certificado del almacén de certificados, el plan de App Service debe ser al menos el nivel estándar cuando se configura en un paso posterior. En la aplicación *appsettings.json* de archivos, modifique la `IdentityServer` sección deben incluir los detalles claves:

```json
"IdentityServer": {
  "Key": {
    "Type": "Store",
    "StoreName": "My",
    "StoreLocation": "CurrentUser",
    "Name": "CN=MyApplication"
  }
}
```

* La propiedad nombre de certificado se corresponde con el distintivo asunto del certificado.
* La ubicación del almacén representa dónde se debe cargar el certificado desde (`CurrentUser` o `LocalMachine`).
* El nombre del almacén representa el nombre del almacén de certificados donde se almacena el certificado. En este caso, señala al almacén personal del usuario.

Para implementar en Azure Websites, implemente la aplicación siguiendo los pasos de [implementar la aplicación en Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) para crear los recursos de Azure necesarios e implementar la aplicación en producción.

Después de seguir las instrucciones anteriores, la aplicación se implementa en Azure pero todavía no es funcional. El certificado utilizado por la aplicación todavía debe configurarse. Busque la huella digital del certificado que se usará y siga los pasos descritos en [cargar sus certificados](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).

Aunque estos pasos mencionan SSL, no hay un **certificados privados** sección en el portal donde puede cargar el certificado aprovisionado para usar con la aplicación.

Después de este paso, reinicie la aplicación y debe ser funcional.

## <a name="other-configuration-options"></a>Otras opciones de configuración

La compatibilidad para la autorización de API se basa en IdentityServer con un conjunto de convenciones, valores predeterminados y mejoras para simplificar la experiencia para las spa. Obviamente, toda la eficacia de IdentityServer está disponible en segundo plano si las aplicaciones integradas de ASP.NET Core no cubren su escenario. La compatibilidad de ASP.NET Core se centra en las aplicaciones "propias", donde todas las aplicaciones se crean e implementan en nuestra organización. Por lo tanto, no se ofrece soporte técnico para cosas como el consentimiento o la federación. Para esos escenarios, use IdentityServer y seguir su documentación.

### <a name="application-profiles"></a>Perfiles de aplicación

Perfiles de aplicación son configuraciones predefinidas para las aplicaciones que definan sus parámetros. En este momento, se admiten los siguientes perfiles:

* `IdentityServerSPA`: Representa una SPA hospedada por IdentityServer como una sola unidad.
  * El `redirect_uri` el valor predeterminado es `/authentication/login-callback`.
  * El `post_logout_redirect_uri` el valor predeterminado es `/authentication/logout-callback`.
  * El conjunto de ámbitos incluye el `openid`, `profile`y cada ámbito definido para las API en la aplicación.
  * El conjunto de tipos permitidos de respuesta OIDC es `id_token token` o cada uno de ellos individualmente (`id_token`, `token`).
  * El modo de respuesta permitidos es `fragment`.
* `SPA`: Representa una SPA no está hospedada con IdentityServer.
  * El conjunto de ámbitos incluye el `openid`, `profile`y cada ámbito definido para las API en la aplicación.
  * El conjunto de tipos permitidos de respuesta OIDC es `id_token token` o cada uno de ellos individualmente (`id_token`, `token`).
  * El modo de respuesta permitidos es `fragment`.
* `IdentityServerJwt`: Representa una API que se hospeda junto con IdentityServer.
  * La aplicación está configurada para tener un ámbito que el valor predeterminado es el nombre de la aplicación.
* `API`: Representa una API que no está hospedada con IdentityServer.
  * La aplicación está configurada para tener un ámbito que el valor predeterminado es el nombre de la aplicación.

### <a name="configuration-through-appsettings"></a>Configuración a través de AppSettings

Configure las aplicaciones a través del sistema de configuración agregándolos a la lista de `Clients` o `Resources`.

Configuración de cada cliente `redirect_uri` y `post_logout_redirect_uri` propiedad, como se muestra en el ejemplo siguiente:

```json
"IdentityServer": {
  "Clients": {
    "MySPA": {
      "Profile": "SPA",
      "RedirectUri": "https://www.example.com/authentication/login-callback",
      "LogoutUri": "https://www.example.com/authentication/logout-callback"
    }
  }
}
```

Al configurar los recursos, puede configurar los ámbitos para el recurso tal y como se muestra a continuación:

```json
"IdentityServer": {
  "Resources": {
    "MyExternalApi": {
      "Profile": "API",
      "Scopes": "a b c"
    }
  }
}
```

### <a name="configuration-through-code"></a>Configuración mediante código

También puede configurar los clientes y los recursos a través de código mediante una sobrecarga de `AddApiAuthorization` que realiza una acción para configurar las opciones.

```csharp
AddApiAuthorization<ApplicationUser, ApplicationDbContext>(options =>
{
    options.Clients.AddSPA(
        "My SPA", spa =>
        spa.WithRedirectUri("http://www.example.com/authentication/login-callback")
           .WithLogoutRedirectUri(
               "http://www.example.com/authentication/logout-callback"));

    options.ApiResources.AddApiResource("MyExternalApi", resource =>
        resource.WithScopes("a", "b", "c"));
});
```

## <a name="additional-resources"></a>Recursos adicionales

* <xref:spa/angular>
* <xref:spa/react>
