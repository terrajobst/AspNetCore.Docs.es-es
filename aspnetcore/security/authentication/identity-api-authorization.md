---
title: Introducción a la autenticación para aplicaciones de página única en ASP.NET Core
author: ''
description: Usar la identidad con una aplicación hospeda dentro de una aplicación ASP.NET Core.
ms.author: ''
ms.date: 03/05/2018
uid: security/authentication/identity/spa
ms.openlocfilehash: cf04ec1ff0ae9afea066fd1864ab0a7956ace32c
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346789"
---
# <a name="authentication-and-authorization-for-spas"></a>Autenticación y autorización para las spa

En ASP.NET 3.0 estamos introduciendo soporte para la autenticación en aplicaciones de página única con nuestra nueva compatibilidad para la autorización de API. Esta compatibilidad se basa en una combinación de ASP.NET Core Identity para autenticar y almacenar los usuarios y el servidor de identidad para la implementación de OpenID Connect.

Hemos agregado un nuevo parámetro de autenticación para nuestro Angular y React plantillas que es similar al parámetro de autenticación en nuestras plantillas de páginas de razor y mvc con los valores permiten 'None' y 'Individual'.

## <a name="create-an-angular-app-with-api-authorization-support"></a>Crear una aplicación Angular con compatibilidad con la autorización de API

Para crear una nueva aplicación Angular con compatibilidad para la autenticación y autorización de usuarios, abra un shell de comandos y ejecute el siguiente comando:

```console
dotnet new angular -o <output_directory_name> -au Individual
```

El comando anterior crea una aplicación de ASP.NET Core con un *ClientApp* directorio que contiene la aplicación Angular.

## <a name="create-a-react-app-with-api-authorization-support"></a>Crear una aplicación de React con compatibilidad con la autorización de API

Para crear una nueva aplicación React con compatibilidad para la autenticación y autorización de usuarios, abra un shell de comandos y ejecute el siguiente comando:

```console
dotnet new react -o <output_directory_name> -au Individual
```

El comando anterior crea una aplicación de ASP.NET Core con un *ClientApp* directorio que contiene la aplicación React.

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a>Descripción general de los componentes de ASP.NET Core de la aplicación

Hay varias adiciones al proyecto cuando se incluye compatibilidad con la autenticación:

### <a name="startup-class"></a>Clase de inicio

Si observamos el código de la clase Startup a continuación podemos agradecemos las inclusiones siguientes:
* Dentro de `public void ConfigureServices(IServiceCollection services)`:
  * Identidad con la interfaz de usuario predeterminada.
    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
      options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
      .AddDefaultUI(UIFramework.Bootstrap4)
      .AddEntityFrameworkStores<ApplicationDbContext>();
    ```
  * Servidor de identidad con un método de aplicación auxiliar AddApiAuthorization adicional que las instalaciones algunos predeterminada ASP.NET convenciones sobre el servidor de identidades.
    ```csharp
    services.AddIdentityServer()
      .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```
  * Autenticación con un método auxiliar AddIdentityServerJwt adicional que configure la aplicación para validar los tokens de Jwt generados por Identity Server. 
    ```csharp
    services.AddAuthentication()
      .AddIdentityServerJwt();
    ```
* Dentro de `public void Configure(IApplicationBuilder app)`:
  * El middleware de autenticación que se encarga de validar las credenciales en la solicitud entrante y establecer el usuario en el contexto de solicitud.
    ```csharp
    app.UseAuthentication();
    ```
  * El middleware de servidor de identidades que expone los puntos de conexión de OpenID Connect.
    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a>AddApiAuthorization 
Este método auxiliar configura el servidor de identidades para utilizar nuestra configuración admitida. Servidor de identidades es un marco muy eficaz y extensible para controlar cuestiones de seguridad de la aplicación pero al mismo tiempo que expone mucha complejidad que no tenemos que saber acerca de los escenarios más comunes, así que elegimos un conjunto de convenciones y Opciones de configuración que consideramos que son un buen punto de partida. Una vez que cambian las necesidades de su autenticación toda la funcionalidad de Identity Server sigue estando disponible para usted para que pueda personalizar para satisfacer sus necesidades.

### <a name="addidentityserverjwt"></a>AddIdentityServerJwt
Este método auxiliar configura una combinación de directivas para la aplicación como el controlador de autenticación predeterminado. La directiva está configurada para permitir que la identidad de controlar todas las solicitudes que se envían a cualquier subtrazado en el espacio de direcciones url de identidad "/ Identity" y para permitirle controlar todas las demás solicitudes a la JwtBearerHandler.
Addionally este método registra un `<<ApplicationName>>API` recurso de la Api con el servidor de identidad con un ámbito predeterminado de `<<ApplicationName>>API` y configura el middleware del token de portador de JWT para validar los tokens emitidos por Identity Server para la aplicación.

### <a name="sampledatacontroller"></a>SampleDataController
Si observamos el archivo Controllers\SampleDataController.cs nos podemos observar la `[Authorize]` atributo aplicado a la clase que indica que el usuario debe tener autorización en función de la directiva predeterminada para tener acceso al recurso. La directiva de autorización predeterminado ocurre deberá estar configurado para usar el esquema de autenticación predeterminado que se configura de forma `AddIdentityServerJwt` a la combinación de directivas que se mencionó anteriormente, que hace el controlador JwtBearer configurado por el método de este tipo auxiliar del controlador predeterminado para solicitudes a la aplicación.

### <a name="applicationdbcontext"></a>ApplicationDbContext
Si observamos el archivo en Data\ApplicationDbContext.cs podemos ver el mismo DbContext que usamos en identidad con la excepción que extiende ApiAuthorizationDbContext (una clase más derivada de IdentityDbContext) para incluir el esquema para el servidor de identidades.
Si queremos control total sobre el esquema de base de datos simplemente podemos heredar de una de las clases DbContext de identidad disponibles y configurar el contexto para incluir el esquema de identidad mediante una llamada a `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` en el `OnModelCreating` método.

### <a name="oidcconfigurationcontroller"></a>OidcConfigurationController
Si nos centramos en el archivo Controllers\OidcConfigurationController.cs podemos ver el punto de conexión que nos breves para atender los parámetros OIDC que el cliente debe usar.

### <a name="appsettingsjson"></a>appsettings.json
Si observamos el archivo appsettings.json en la raíz del proyecto, podemos ver un nuevo `IdentityServer` configurado de sección que describe la lista de clientes y podemos ver que hay un solo cliente. El nombre del cliente corresponde al nombre de la aplicación y se puede asignar por convención para el parámetro de ClientId de oAuth. El perfil indica qué tipo de aplicación, vamos a configurar y se usa internamente para las convenciones de la unidad que simplifican el proceso de configuración para el servidor. Hay disponibles varios perfiles, se explica en la sección siguiente.

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
Si miramos a appsettings. Archivo Development.JSON en la raíz del proyecto, podemos ver un nuevo `IdentityServer` sección que describe la clave se usa para firmar los tokens. Cuando se implementa en producción debe una clave se aprovisione e implemente junto con la aplicación tal como se explica más adelante.

```json
  "IdentityServer": {
    "Key": {
      "Type": "Development"
    }
  }
}
```

## <a name="general-description-of-the-angular-application"></a>Descripción general de la aplicación Angular
La compatibilidad para la autenticación y autorización de API en la plantilla de Angular reside en su propio módulo de Angular. Bajo ClientApp\src\api autorización y se compone de los siguientes elementos:
* 3 componentes:
  * Componente de inicio de sesión: Controla el flujo de inicio de sesión para la aplicación.
  * Componente de cierre de sesión: Controla el flujo de cierre de sesión para la aplicación.
  * Componente de menú de inicio de sesión: Un widget que muestra el usuario autenticado actual con vínculos para administrar el perfil de usuario y cierre de sesión o los vínculos para iniciar sesión o registrarse cuando el usuario no está autenticado.
* Una restricción de ruta `AuthorizeGuard` que pueden agregarse a las rutas y requiere que el usuario se autentique antes de visitar la ruta.
* Un interceptor http `AuthorizeInterceptor` que asocia el token de acceso a las solicitudes HTTP salientes destinadas a la API cuando se autentica el usuario.
* Un servicio `AuthorizeService` que controla los detalles de nivel inferior del proceso de autenticación y expone información sobre el usuario autenticado para el resto de la aplicación para su uso.
* Un módulo de angular que define las rutas asociadas con las partes de la autenticación de la aplicación y expone el componente de menú de inicio de sesión, el interceptor, la protección y el servicio para el consumo del resto de la aplicación.

## <a name="general-description-of-the-react-application"></a>Descripción general de la aplicación React
La compatibilidad para la autenticación y autorización de API en la vida de la plantilla de React en ClientApp\src\components\api authorization\ y se compone de los siguientes elementos:
* 4 componentes:
  * Componente de inicio de sesión: Controla el flujo de inicio de sesión para la aplicación.
  * Componente de cierre de sesión: Controla el flujo de cierre de sesión para la aplicación.
  * Componente de menú de inicio de sesión: Un widget que muestra el usuario autenticado actual con vínculos para administrar el perfil de usuario y cierre de sesión o los vínculos para iniciar sesión o registrarse cuando el usuario no está autenticado.
  * AuthorizeRoute: Un componente de ruta que requiere que el usuario se autentique antes de representar el componente indicado en el parámetro de componente.
  * Un exportado `authService` instancia de clase `AuthorizeService` que controla los detalles de nivel inferior del proceso de autenticación y expone información sobre el usuario autenticado para el resto de la aplicación para su uso.

Ahora que hemos visto los componentes principales de la solución, podemos echamos un vistazo específico en distintos escenarios para la aplicación:

## <a name="requiring-authorization-on-a-new-api"></a>Requerir autorización sobre una nueva API
El sistema está configurado de fábrica que resulte trivial para solicitar autorización para las nuevas API. Para ello, basta con crear un nuevo controlador y agregue el `[Authorize]` atributo a la clase de controlador o a cualquier acción en el controlador.

## <a name="protecting-a-client-side-route-angular"></a>Protección de una ruta del lado cliente (Angular)
Protección de una ruta de lado cliente se realiza mediante la adición de la protección de autorizar a la lista de restricciones que se ejecutará cuando la configuración de una ruta. Por ejemplo puede ver cómo se configura la ruta de la captura de datos dentro del módulo angular de la aplicación principal:

```ts
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

Es importante mencionar que la protección de una ruta no protege el punto de conexión real (que todavía se requiere un `[Authorize]` atributo aplicado a él), pero que solo se evita que el usuario de navegación a la ruta del lado cliente determinado cuando no está autenticada.

## <a name="authenticate-api-requests-angular"></a>Autenticar solicitudes de API (Angular)

Autenticar solicitudes a las API hospedadas a lo largo del lado de que la aplicación se realiza automáticamente mediante el uso del interceptor de cliente HTTP definido por la aplicación.

## <a name="protect-a-client-side-route-react"></a>Proteger una ruta del lado cliente (React)

Protección de una ruta de lado cliente se realiza mediante el componente AuthorizeRoute en lugar del componente de ruta sin formato. Por ejemplo puede ver cómo se configura la ruta de la captura de datos dentro del componente de aplicación:

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

Es importante mencionar que la protección de una ruta no protege el punto de conexión real (que todavía se requiere un `[Authorize]` atributo aplicado a él), pero que solo se evita que el usuario de navegación a la ruta del lado cliente determinado cuando no está autenticada.

## <a name="authenticate-api-requests-react"></a>Autenticar solicitudes de API (React)

Autenticar las solicitudes con react se realiza mediante la importación de la primera la `authService` instancia desde el `AuthorizeService` y, a continuación, recuperar el token de acceso de la authService y conectarlo a la solicitud, tal como se muestra a continuación. En los componentes de react normalmente esto se hace en el método de ciclo de vida de componentDidMount o como resultado de una interacción del usuario.

### <a name="import-the-authservice-into-your-component"></a>Importar el authService su componente

```js
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a>Recuperar y adjunte el token de acceso a la respuesta

```js
async populateWeatherData() {
  const token = await authService.getAccessToken();
  const response = await fetch('api/SampleData/WeatherForecasts', {
    headers: !token ? {} : { 'Authorization': `Bearer ${token}` }
  });
  const data = await response.json();
  this.setState({ forecasts: data, loading: false });
}
```

## <a name="deploy-into-production"></a>Implementar en producción

Para implementar la aplicación en producción, es necesario aprovisionar varios recursos:
* Concede a una base de datos para almacenar las cuentas de usuario de identidad y el servidor de identidad.
* Un certificado de producción que se usará para firmar los tokens.
  * No hay ningún requisito específico para este certificado; puede ser un certificado autofirmado o un certificado a través de una entidad CA.
  * Se puede generar a través de herramientas estándares, como powershell o openssl.
  * Puede ser instalado en el almacén de certificados en los equipos de destino o implementar como un archivo pfx con una contraseña segura.

### <a name="example-deploy-into-azure-websites"></a>Ejemplo: Implementar en sitios Web de Azure

En esta sección vamos a implementar la aplicación en Azure websites que usan un certificado almacenado en el almacén de certificados. Es necesario modificar la aplicación para cargar un certicate del almacén de certificados. Para ello, nuestro plan de app service debe ser al menos en el nivel estándar cuando se configura en un paso posterior. En nuestra aplicación, basta con modificar la sección IdentityServer en appsettings.json para incluir los detalles de la clave:
```json
  "IdentityServer": {
    "Key": {
      "Type": "Store",
      "StoreName": "My",
      "StoreLocation": "CurrentUser",
      "Name": "CN=MyApplication"
    }
  }
}
```
* La propiedad nombre de certificado se corresponde con el distintivo asunto del certificado.
* La ubicación del almacén representa dónde se debe cargar el certificado desde (CurrentUrser o LocalMachine).
* El nombre del almacén representa el nombre del almacén de certificados donde se almacena el certificado, en este caso apunta al almacén personal del usuario.

Para implementar en Azure Websites, implemente la aplicación siguiendo los pasos de [implementar la aplicación en Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) para crear los recursos de Azure necesarios e implementar la aplicación en producción.

Una vez hecho esto, la aplicación se implementa en Azure, pero aún no está completamente funcional todavía es necesario configurar el certificado que va a usar la aplicación. Para ello, necesitamos tener la huella digital del certificado que vamos a usar y siga los pasos descritos en [cargar sus certificados](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).

Aunque estos pasos mencionan SSL, hay una sección "Certificados privados" en el portal donde podemos cargar nuestro certificado aprovisionado para usar con nuestra aplicación.

Después de este paso, debemos poder reiniciar la aplicación y debe ser completamente funcional.

## <a name="other-configuration-options"></a>Otras opciones de configuración
La compatibilidad para la autorización de API se basa en Identity Server con un conjunto de convenciones, valores predeterminados y mejoras para simplificar la experiencia para aplicaciones de página única. Obviamente, toda la funcionalidad de Identity Server está disponible en segundo plano si las integraciones que ofrecemos no cubren su escenario. Nuestro soporte técnico se centra en lo que llamamos aplicaciones "propias", donde todas las aplicaciones se crean e implementan en nuestra organización. Por lo tanto no ofrecemos soporte para cosas como el consentimiento o la federación. Para estos escenarios, nuestra recomendación es utilizar servidor de identidades y seguir su documentación.

### <a name="application-profiles"></a>Perfiles de aplicación
Perfiles de aplicación son configuraciones predefinidas para las aplicaciones que definan sus parámetros. En este momento se admiten dos perfiles:
* IdentityServerSPA: Representa una aplicación hospedada por Identity Server como una sola unidad.
  * Los redirect_uri el valor predeterminado es `/authentication/login-callback`.
  * Valor predeterminado es el post_logout_redirect_uri `/authentication/logout-callback`.
  * El conjunto de ámbitos incluye el `openid`, `profile`y cada ámbito definido para las API en la aplicación.
  * El conjunto de tipos permitidos de respuesta OIDC es `id_token token` o cada uno de ellos individualmente (`id_token`, `token`).
  * El modo de respuesta permitidos es `fragment`.
* SPA: Representa una aplicación de página única que no esté hospedada con Identity Server.
  * El conjunto de ámbitos incluye el `openid`, `profile`y cada ámbito definido para las API en la aplicación.
  * El conjunto de tipos permitidos de respuesta OIDC es `id_token token` o cada uno de ellos individualmente (`id_token`, `token`).
  * El modo de respuesta permitidos es `fragment`.
* IdentityServerJwt: Representa una API que se hospeda junto con Identity Server.
  * La aplicación está configurada para tener un ámbito que el valor predeterminado es el nombre de la aplicación.
* API: Representa una API que no esté hospedada con Identity Server.
  * La aplicación está configurada para tener un ámbito que el valor predeterminado es el nombre de la aplicación.

### <a name="configuration-through-appsettings"></a>Configuración a través de AppSettings
Podemos configurar las aplicaciones a través de nuestro sistema de configuración mediante la adición a la lista de clientes o recursos, respectivamente. 

Al configurar los clientes podemos configurar el `redirect_uri` y `post_logout_redirect_uri` tal como se muestra a continuación:
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

Al configurar recursos podemos configurar los ámbitos para el recurso tal y como se muestra a continuación:
```json
"IdentityServer": {
  "Resources": {
    "MyExternalApi": {
      "Profile": "API",
      "Scopes": "a b c",
    }
  }
}
```

### <a name="configuration-through-code"></a>Configuración mediante código
También podemos configurar los clientes y los recursos a través de código mediante una sobrecarga de AddApiAuthorization que realiza una acción para configurar las opciones.
```csharp
AddApiAuthorization<ApplicationUser, ApplicationDbContext>(options =>
{
    options.Clients.AddSPA(
        "My SPA",
        spa => spa.WithRedirectUri("http://www.example.com/authentication/login-callback")
            .WithLogoutRedirectUri("http://www.example.com/authentication/logout-callback"));

    options.ApiResources.AddApiResource("MyExternalApi", resource => resource.WithScopes("a", "b", "c"));
});
```
