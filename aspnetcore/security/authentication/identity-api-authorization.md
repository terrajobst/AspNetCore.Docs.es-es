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
# <a name="authentication-and-authorization-for-spas"></a><span data-ttu-id="9a4d9-103">Autenticación y autorización para las spa</span><span class="sxs-lookup"><span data-stu-id="9a4d9-103">Authentication and authorization for SPAs</span></span>

<span data-ttu-id="9a4d9-104">En ASP.NET 3.0 estamos introduciendo soporte para la autenticación en aplicaciones de página única con nuestra nueva compatibilidad para la autorización de API.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-104">In ASP.NET 3.0 we are introducing support for authentication in single page applications using our new support for API authorization.</span></span> <span data-ttu-id="9a4d9-105">Esta compatibilidad se basa en una combinación de ASP.NET Core Identity para autenticar y almacenar los usuarios y el servidor de identidad para la implementación de OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-105">This support is based on a combination of ASP.NET Core Identity for authenticating and storing users and Identity Server for implementing Open ID Connect.</span></span>

<span data-ttu-id="9a4d9-106">Hemos agregado un nuevo parámetro de autenticación para nuestro Angular y React plantillas que es similar al parámetro de autenticación en nuestras plantillas de páginas de razor y mvc con los valores permiten 'None' y 'Individual'.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-106">We have added a new authentication parameter to our Angular and React templates that is similar to the authentication parameter in our mvc and razor pages templates with allowed values 'None' and 'Individual'.</span></span>

## <a name="create-an-angular-app-with-api-authorization-support"></a><span data-ttu-id="9a4d9-107">Crear una aplicación Angular con compatibilidad con la autorización de API</span><span class="sxs-lookup"><span data-stu-id="9a4d9-107">Create an Angular app with API authorization support</span></span>

<span data-ttu-id="9a4d9-108">Para crear una nueva aplicación Angular con compatibilidad para la autenticación y autorización de usuarios, abra un shell de comandos y ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="9a4d9-108">To create a new Angular app with support for authentication and authorization of users, open a command shell and run the following command:</span></span>

```console
dotnet new angular -o <output_directory_name> -au Individual
```

<span data-ttu-id="9a4d9-109">El comando anterior crea una aplicación de ASP.NET Core con un *ClientApp* directorio que contiene la aplicación Angular.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-109">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the Angular app.</span></span>

## <a name="create-a-react-app-with-api-authorization-support"></a><span data-ttu-id="9a4d9-110">Crear una aplicación de React con compatibilidad con la autorización de API</span><span class="sxs-lookup"><span data-stu-id="9a4d9-110">Create a React app with API authorization support</span></span>

<span data-ttu-id="9a4d9-111">Para crear una nueva aplicación React con compatibilidad para la autenticación y autorización de usuarios, abra un shell de comandos y ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="9a4d9-111">To create a new React app with support for authentication and authorization of users, open a command shell and run the following command:</span></span>

```console
dotnet new react -o <output_directory_name> -au Individual
```

<span data-ttu-id="9a4d9-112">El comando anterior crea una aplicación de ASP.NET Core con un *ClientApp* directorio que contiene la aplicación React.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-112">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the React app.</span></span>

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a><span data-ttu-id="9a4d9-113">Descripción general de los componentes de ASP.NET Core de la aplicación</span><span class="sxs-lookup"><span data-stu-id="9a4d9-113">General description of the ASP.NET Core components of the app</span></span>

<span data-ttu-id="9a4d9-114">Hay varias adiciones al proyecto cuando se incluye compatibilidad con la autenticación:</span><span class="sxs-lookup"><span data-stu-id="9a4d9-114">There are several additions to the project when we include support for authentication:</span></span>

### <a name="startup-class"></a><span data-ttu-id="9a4d9-115">Clase de inicio</span><span class="sxs-lookup"><span data-stu-id="9a4d9-115">Startup class</span></span>

<span data-ttu-id="9a4d9-116">Si observamos el código de la clase Startup a continuación podemos agradecemos las inclusiones siguientes:</span><span class="sxs-lookup"><span data-stu-id="9a4d9-116">If we look at the code in the Startup class below we can appreciate the following inclusions:</span></span>
* <span data-ttu-id="9a4d9-117">Dentro de `public void ConfigureServices(IServiceCollection services)`:</span><span class="sxs-lookup"><span data-stu-id="9a4d9-117">Inside `public void ConfigureServices(IServiceCollection services)`:</span></span>
  * <span data-ttu-id="9a4d9-118">Identidad con la interfaz de usuario predeterminada.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-118">Identity with the default UI.</span></span>
    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
      options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
      .AddDefaultUI(UIFramework.Bootstrap4)
      .AddEntityFrameworkStores<ApplicationDbContext>();
    ```
  * <span data-ttu-id="9a4d9-119">Servidor de identidad con un método de aplicación auxiliar AddApiAuthorization adicional que las instalaciones algunos predeterminada ASP.NET convenciones sobre el servidor de identidades.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-119">Identity Server with an additional AddApiAuthorization helper method that setups some default ASP.NET Conventions on top of Identity Server.</span></span>
    ```csharp
    services.AddIdentityServer()
      .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```
  * <span data-ttu-id="9a4d9-120">Autenticación con un método auxiliar AddIdentityServerJwt adicional que configure la aplicación para validar los tokens de Jwt generados por Identity Server.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-120">Authentication with an additional AddIdentityServerJwt helper method that configures the application to validate Jwt tokens produced by Identity Server.</span></span> 
    ```csharp
    services.AddAuthentication()
      .AddIdentityServerJwt();
    ```
* <span data-ttu-id="9a4d9-121">Dentro de `public void Configure(IApplicationBuilder app)`:</span><span class="sxs-lookup"><span data-stu-id="9a4d9-121">Inside `public void Configure(IApplicationBuilder app)`:</span></span>
  * <span data-ttu-id="9a4d9-122">El middleware de autenticación que se encarga de validar las credenciales en la solicitud entrante y establecer el usuario en el contexto de solicitud.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-122">The authentication middleware that is responsible for validating the credentials in the incoming request and setting the user on the request context.</span></span>
    ```csharp
    app.UseAuthentication();
    ```
  * <span data-ttu-id="9a4d9-123">El middleware de servidor de identidades que expone los puntos de conexión de OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-123">The identity server middleware that exposes the Open ID Connect endpoints.</span></span>
    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="9a4d9-124">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="9a4d9-124">AddApiAuthorization</span></span> 
<span data-ttu-id="9a4d9-125">Este método auxiliar configura el servidor de identidades para utilizar nuestra configuración admitida.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-125">This helper method configures Identity Server to use our supported configuration.</span></span> <span data-ttu-id="9a4d9-126">Servidor de identidades es un marco muy eficaz y extensible para controlar cuestiones de seguridad de la aplicación pero al mismo tiempo que expone mucha complejidad que no tenemos que saber acerca de los escenarios más comunes, así que elegimos un conjunto de convenciones y Opciones de configuración que consideramos que son un buen punto de partida.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-126">Identity Server is a very powerful and extensible framework for handling application security concerns but at the same time that exposes a lot of complexity that we don't need to know about for the most common scenarios, so we choose a set of conventions and configuration options for you that we consider are a good starting point.</span></span> <span data-ttu-id="9a4d9-127">Una vez que cambian las necesidades de su autenticación toda la funcionalidad de Identity Server sigue estando disponible para usted para que pueda personalizar para satisfacer sus necesidades.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-127">Once your authentication needs change the full power of Identity Server is still available to you so you can customize it to suit your needs.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="9a4d9-128">AddIdentityServerJwt</span><span class="sxs-lookup"><span data-stu-id="9a4d9-128">AddIdentityServerJwt</span></span>
<span data-ttu-id="9a4d9-129">Este método auxiliar configura una combinación de directivas para la aplicación como el controlador de autenticación predeterminado.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-129">This helper method configures a policy scheme for the application as the default authentication handler.</span></span> <span data-ttu-id="9a4d9-130">La directiva está configurada para permitir que la identidad de controlar todas las solicitudes que se envían a cualquier subtrazado en el espacio de direcciones url de identidad "/ Identity" y para permitirle controlar todas las demás solicitudes a la JwtBearerHandler.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-130">The policy is configured to let identity handle all the requests that go to any subpath in the Identity url space "/Identity" and to let the JwtBearerHandler handle all other requests.</span></span>
<span data-ttu-id="9a4d9-131">Addionally este método registra un `<<ApplicationName>>API` recurso de la Api con el servidor de identidad con un ámbito predeterminado de `<<ApplicationName>>API` y configura el middleware del token de portador de JWT para validar los tokens emitidos por Identity Server para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-131">Addionally this method registers an `<<ApplicationName>>API` Api resource with identity server with a default scope of `<<ApplicationName>>API` and configures the JWT Bearer token middleware to validate tokens issued by Identity Server for the application.</span></span>

### <a name="sampledatacontroller"></a><span data-ttu-id="9a4d9-132">SampleDataController</span><span class="sxs-lookup"><span data-stu-id="9a4d9-132">SampleDataController</span></span>
<span data-ttu-id="9a4d9-133">Si observamos el archivo Controllers\SampleDataController.cs nos podemos observar la `[Authorize]` atributo aplicado a la clase que indica que el usuario debe tener autorización en función de la directiva predeterminada para tener acceso al recurso.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-133">If we look at the file Controllers\SampleDataController.cs we can observe the `[Authorize]` attribute applied to the class that indicates that the user needs to be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="9a4d9-134">La directiva de autorización predeterminado ocurre deberá estar configurado para usar el esquema de autenticación predeterminado que se configura de forma `AddIdentityServerJwt` a la combinación de directivas que se mencionó anteriormente, que hace el controlador JwtBearer configurado por el método de este tipo auxiliar del controlador predeterminado para solicitudes a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-134">The default authorization policy happens to be configured to use the default authentication scheme which is set up by `AddIdentityServerJwt` to the policy scheme that we mentioned above, making the JwtBearer handler configured by such helper method the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="9a4d9-135">ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="9a4d9-135">ApplicationDbContext</span></span>
<span data-ttu-id="9a4d9-136">Si observamos el archivo en Data\ApplicationDbContext.cs podemos ver el mismo DbContext que usamos en identidad con la excepción que extiende ApiAuthorizationDbContext (una clase más derivada de IdentityDbContext) para incluir el esquema para el servidor de identidades.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-136">If we look at the file in Data\ApplicationDbContext.cs we can see the same DbContext we use in identity with the exception that it extends ApiAuthorizationDbContext (a more derived class from IdentityDbContext) to include the schema for Identity Server.</span></span>
<span data-ttu-id="9a4d9-137">Si queremos control total sobre el esquema de base de datos simplemente podemos heredar de una de las clases DbContext de identidad disponibles y configurar el contexto para incluir el esquema de identidad mediante una llamada a `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` en el `OnModelCreating` método.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-137">If we want full control of the database schema we can simply inherit from one of the available Identity DbContext classes and configure the context to include the identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` on the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="9a4d9-138">OidcConfigurationController</span><span class="sxs-lookup"><span data-stu-id="9a4d9-138">OidcConfigurationController</span></span>
<span data-ttu-id="9a4d9-139">Si nos centramos en el archivo Controllers\OidcConfigurationController.cs podemos ver el punto de conexión que nos breves para atender los parámetros OIDC que el cliente debe usar.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-139">If we look at the file Controllers\OidcConfigurationController.cs we can see the endpoint that we stand-up to serve the OIDC parameters that the client needs to use.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="9a4d9-140">appsettings.json</span><span class="sxs-lookup"><span data-stu-id="9a4d9-140">appsettings.json</span></span>
<span data-ttu-id="9a4d9-141">Si observamos el archivo appsettings.json en la raíz del proyecto, podemos ver un nuevo `IdentityServer` configurado de sección que describe la lista de clientes y podemos ver que hay un solo cliente.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-141">If we look at the appsettings.json file on the root of the project, we can see a new `IdentityServer` section that describes the list of configured clients and we can see that there is a single client.</span></span> <span data-ttu-id="9a4d9-142">El nombre del cliente corresponde al nombre de la aplicación y se puede asignar por convención para el parámetro de ClientId de oAuth.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-142">The name of the client corresponds to the name of the application and is mapped by convention to the oAuth ClientId parameter.</span></span> <span data-ttu-id="9a4d9-143">El perfil indica qué tipo de aplicación, vamos a configurar y se usa internamente para las convenciones de la unidad que simplifican el proceso de configuración para el servidor.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-143">The profile indicates what type of application we are configuring, and we use it internally to drive conventions that simplify the configuration process for the server.</span></span> <span data-ttu-id="9a4d9-144">Hay disponibles varios perfiles, se explica en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-144">There are several profiles available explained in the section below.</span></span>

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a><span data-ttu-id="9a4d9-145">appsettings.Development.json</span><span class="sxs-lookup"><span data-stu-id="9a4d9-145">appsettings.Development.json</span></span>
<span data-ttu-id="9a4d9-146">Si miramos a appsettings. Archivo Development.JSON en la raíz del proyecto, podemos ver un nuevo `IdentityServer` sección que describe la clave se usa para firmar los tokens.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-146">If we look at the appsettings.Development.json file on the root of the project, we can see a new `IdentityServer` section that describes the key we are using to sign tokens.</span></span> <span data-ttu-id="9a4d9-147">Cuando se implementa en producción debe una clave se aprovisione e implemente junto con la aplicación tal como se explica más adelante.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-147">When deploying to production a key needs to be provisioned and deployed alongside the application as explained below.</span></span>

```json
  "IdentityServer": {
    "Key": {
      "Type": "Development"
    }
  }
}
```

## <a name="general-description-of-the-angular-application"></a><span data-ttu-id="9a4d9-148">Descripción general de la aplicación Angular</span><span class="sxs-lookup"><span data-stu-id="9a4d9-148">General description of the Angular application</span></span>
<span data-ttu-id="9a4d9-149">La compatibilidad para la autenticación y autorización de API en la plantilla de Angular reside en su propio módulo de Angular.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-149">The support for authentication and API authorization in the Angular template lives in its own Angular module.</span></span> <span data-ttu-id="9a4d9-150">Bajo ClientApp\src\api autorización y se compone de los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="9a4d9-150">Under ClientApp\src\api-authorization and it is composed of the following elements:</span></span>
* <span data-ttu-id="9a4d9-151">3 componentes:</span><span class="sxs-lookup"><span data-stu-id="9a4d9-151">3 Components:</span></span>
  * <span data-ttu-id="9a4d9-152">Componente de inicio de sesión: Controla el flujo de inicio de sesión para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-152">Login component: Handles the login flow for the app.</span></span>
  * <span data-ttu-id="9a4d9-153">Componente de cierre de sesión: Controla el flujo de cierre de sesión para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-153">Logout component: Handles the logout flow for the app.</span></span>
  * <span data-ttu-id="9a4d9-154">Componente de menú de inicio de sesión: Un widget que muestra el usuario autenticado actual con vínculos para administrar el perfil de usuario y cierre de sesión o los vínculos para iniciar sesión o registrarse cuando el usuario no está autenticado.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-154">Login menu component: A widget that displays the current authenticated user with links to manage the user profile and log out or links to log in or register when the user is not authenticated.</span></span>
* <span data-ttu-id="9a4d9-155">Una restricción de ruta `AuthorizeGuard` que pueden agregarse a las rutas y requiere que el usuario se autentique antes de visitar la ruta.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-155">A route guard `AuthorizeGuard` that can be added to routes and requires a user to be authenticated before visiting the route.</span></span>
* <span data-ttu-id="9a4d9-156">Un interceptor http `AuthorizeInterceptor` que asocia el token de acceso a las solicitudes HTTP salientes destinadas a la API cuando se autentica el usuario.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-156">An http interceptor `AuthorizeInterceptor` that attaches the access token to outgoing HTTP requests targeting the API when the user is authenticated.</span></span>
* <span data-ttu-id="9a4d9-157">Un servicio `AuthorizeService` que controla los detalles de nivel inferior del proceso de autenticación y expone información sobre el usuario autenticado para el resto de la aplicación para su uso.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-157">A service `AuthorizeService` that handles the lower level details of the authentication process and exposes information about the authenticated user to the rest of the application for consumption.</span></span>
* <span data-ttu-id="9a4d9-158">Un módulo de angular que define las rutas asociadas con las partes de la autenticación de la aplicación y expone el componente de menú de inicio de sesión, el interceptor, la protección y el servicio para el consumo del resto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-158">An angular module that defines routes associated with the authentication parts of the application and exposes the login menu component, the interceptor, the guard and the service for consumption from the rest of the application.</span></span>

## <a name="general-description-of-the-react-application"></a><span data-ttu-id="9a4d9-159">Descripción general de la aplicación React</span><span class="sxs-lookup"><span data-stu-id="9a4d9-159">General description of the React application</span></span>
<span data-ttu-id="9a4d9-160">La compatibilidad para la autenticación y autorización de API en la vida de la plantilla de React en ClientApp\src\components\api authorization\ y se compone de los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="9a4d9-160">The support for authentication and API authorization in the React template lives under ClientApp\src\components\api-authorization\ and it is composed of the following elements:</span></span>
* <span data-ttu-id="9a4d9-161">4 componentes:</span><span class="sxs-lookup"><span data-stu-id="9a4d9-161">4 Components:</span></span>
  * <span data-ttu-id="9a4d9-162">Componente de inicio de sesión: Controla el flujo de inicio de sesión para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-162">Login component: Handles the login flow for the app.</span></span>
  * <span data-ttu-id="9a4d9-163">Componente de cierre de sesión: Controla el flujo de cierre de sesión para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-163">Logout component: Handles the logout flow for the app.</span></span>
  * <span data-ttu-id="9a4d9-164">Componente de menú de inicio de sesión: Un widget que muestra el usuario autenticado actual con vínculos para administrar el perfil de usuario y cierre de sesión o los vínculos para iniciar sesión o registrarse cuando el usuario no está autenticado.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-164">Login menu component: A widget that displays the current authenticated user with links to manage the user profile and log out or links to log in or register when the user is not authenticated.</span></span>
  * <span data-ttu-id="9a4d9-165">AuthorizeRoute: Un componente de ruta que requiere que el usuario se autentique antes de representar el componente indicado en el parámetro de componente.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-165">AuthorizeRoute: A route component that requires a user to be authenticated before rendering the component indicated in the Component parameter.</span></span>
  * <span data-ttu-id="9a4d9-166">Un exportado `authService` instancia de clase `AuthorizeService` que controla los detalles de nivel inferior del proceso de autenticación y expone información sobre el usuario autenticado para el resto de la aplicación para su uso.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-166">An exported `authService` instance of class `AuthorizeService` that handles the lower level details of the authentication process and exposes information about the authenticated user to the rest of the application for consumption.</span></span>

<span data-ttu-id="9a4d9-167">Ahora que hemos visto los componentes principales de la solución, podemos echamos un vistazo específico en distintos escenarios para la aplicación:</span><span class="sxs-lookup"><span data-stu-id="9a4d9-167">Now that we've seen the main components of the solution, we can take a specific look at individual scenarios for the application:</span></span>

## <a name="requiring-authorization-on-a-new-api"></a><span data-ttu-id="9a4d9-168">Requerir autorización sobre una nueva API</span><span class="sxs-lookup"><span data-stu-id="9a4d9-168">Requiring authorization on a new API</span></span>
<span data-ttu-id="9a4d9-169">El sistema está configurado de fábrica que resulte trivial para solicitar autorización para las nuevas API.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-169">The system is configured out of the box to make it trivial to require authorization for new APIs.</span></span> <span data-ttu-id="9a4d9-170">Para ello, basta con crear un nuevo controlador y agregue el `[Authorize]` atributo a la clase de controlador o a cualquier acción en el controlador.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-170">In order to do so, simply create a new controller and add the `[Authorize]` attribute to the controller class or to any action within the controller.</span></span>

## <a name="protecting-a-client-side-route-angular"></a><span data-ttu-id="9a4d9-171">Protección de una ruta del lado cliente (Angular)</span><span class="sxs-lookup"><span data-stu-id="9a4d9-171">Protecting a client-side route (Angular)</span></span>
<span data-ttu-id="9a4d9-172">Protección de una ruta de lado cliente se realiza mediante la adición de la protección de autorizar a la lista de restricciones que se ejecutará cuando la configuración de una ruta.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-172">Protecting a client side route is done by adding the authorize guard to the list of guards to run when configuring a route.</span></span> <span data-ttu-id="9a4d9-173">Por ejemplo puede ver cómo se configura la ruta de la captura de datos dentro del módulo angular de la aplicación principal:</span><span class="sxs-lookup"><span data-stu-id="9a4d9-173">As an example you can see how the fetch-data route is configured within the main app angular module:</span></span>

```ts
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

<span data-ttu-id="9a4d9-174">Es importante mencionar que la protección de una ruta no protege el punto de conexión real (que todavía se requiere un `[Authorize]` atributo aplicado a él), pero que solo se evita que el usuario de navegación a la ruta del lado cliente determinado cuando no está autenticada.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-174">It is important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client side route when it is not authenticated.</span></span>

## <a name="authenticate-api-requests-angular"></a><span data-ttu-id="9a4d9-175">Autenticar solicitudes de API (Angular)</span><span class="sxs-lookup"><span data-stu-id="9a4d9-175">Authenticate API requests (Angular)</span></span>

<span data-ttu-id="9a4d9-176">Autenticar solicitudes a las API hospedadas a lo largo del lado de que la aplicación se realiza automáticamente mediante el uso del interceptor de cliente HTTP definido por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-176">Authenticating requests to APIs hosted along side the application is done automatically through the use of the HTTP client interceptor defined by the application.</span></span>

## <a name="protect-a-client-side-route-react"></a><span data-ttu-id="9a4d9-177">Proteger una ruta del lado cliente (React)</span><span class="sxs-lookup"><span data-stu-id="9a4d9-177">Protect a client-side route (React)</span></span>

<span data-ttu-id="9a4d9-178">Protección de una ruta de lado cliente se realiza mediante el componente AuthorizeRoute en lugar del componente de ruta sin formato.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-178">Protecting a client side route is done by using the AuthorizeRoute component instead of the plain Route component.</span></span> <span data-ttu-id="9a4d9-179">Por ejemplo puede ver cómo se configura la ruta de la captura de datos dentro del componente de aplicación:</span><span class="sxs-lookup"><span data-stu-id="9a4d9-179">As an example you can see how the fetch-data route is configured within the App component:</span></span>

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

<span data-ttu-id="9a4d9-180">Es importante mencionar que la protección de una ruta no protege el punto de conexión real (que todavía se requiere un `[Authorize]` atributo aplicado a él), pero que solo se evita que el usuario de navegación a la ruta del lado cliente determinado cuando no está autenticada.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-180">It is important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client side route when it is not authenticated.</span></span>

## <a name="authenticate-api-requests-react"></a><span data-ttu-id="9a4d9-181">Autenticar solicitudes de API (React)</span><span class="sxs-lookup"><span data-stu-id="9a4d9-181">Authenticate API requests (React)</span></span>

<span data-ttu-id="9a4d9-182">Autenticar las solicitudes con react se realiza mediante la importación de la primera la `authService` instancia desde el `AuthorizeService` y, a continuación, recuperar el token de acceso de la authService y conectarlo a la solicitud, tal como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-182">Authenticating requests with react is done by first importing the `authService` instance from the `AuthorizeService` and then retrieving the access token from the authService and attaching it to the request as shown below.</span></span> <span data-ttu-id="9a4d9-183">En los componentes de react normalmente esto se hace en el método de ciclo de vida de componentDidMount o como resultado de una interacción del usuario.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-183">In react components this is typically done in the componentDidMount lifecycle method or as the result from some user interaction.</span></span>

### <a name="import-the-authservice-into-your-component"></a><span data-ttu-id="9a4d9-184">Importar el authService su componente</span><span class="sxs-lookup"><span data-stu-id="9a4d9-184">Import the authService into your component</span></span>

```js
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a><span data-ttu-id="9a4d9-185">Recuperar y adjunte el token de acceso a la respuesta</span><span class="sxs-lookup"><span data-stu-id="9a4d9-185">Retrieve and attach the access token to the response</span></span>

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

## <a name="deploy-into-production"></a><span data-ttu-id="9a4d9-186">Implementar en producción</span><span class="sxs-lookup"><span data-stu-id="9a4d9-186">Deploy into production</span></span>

<span data-ttu-id="9a4d9-187">Para implementar la aplicación en producción, es necesario aprovisionar varios recursos:</span><span class="sxs-lookup"><span data-stu-id="9a4d9-187">In order to deploy the application into production we need to provision several resources:</span></span>
* <span data-ttu-id="9a4d9-188">Concede a una base de datos para almacenar las cuentas de usuario de identidad y el servidor de identidad.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-188">A database to store the Identity user accounts and the identity server grants.</span></span>
* <span data-ttu-id="9a4d9-189">Un certificado de producción que se usará para firmar los tokens.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-189">A production certificate to use for signing tokens.</span></span>
  * <span data-ttu-id="9a4d9-190">No hay ningún requisito específico para este certificado; puede ser un certificado autofirmado o un certificado a través de una entidad CA.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-190">There are no specific requirements for this certificate; it can be a self-signed certificate or a certificate provisioned through a CA authority.</span></span>
  * <span data-ttu-id="9a4d9-191">Se puede generar a través de herramientas estándares, como powershell o openssl.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-191">It can be generated through standard tools like powershell or openssl.</span></span>
  * <span data-ttu-id="9a4d9-192">Puede ser instalado en el almacén de certificados en los equipos de destino o implementar como un archivo pfx con una contraseña segura.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-192">It can be installed into the certificate store on the target machines or deployed as a pfx file with a strong password.</span></span>

### <a name="example-deploy-into-azure-websites"></a><span data-ttu-id="9a4d9-193">Ejemplo: Implementar en sitios Web de Azure</span><span class="sxs-lookup"><span data-stu-id="9a4d9-193">Example: Deploy into Azure Websites</span></span>

<span data-ttu-id="9a4d9-194">En esta sección vamos a implementar la aplicación en Azure websites que usan un certificado almacenado en el almacén de certificados.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-194">In this section we are going to deploy the application to Azure websites using a certificate stored in the certificate store.</span></span> <span data-ttu-id="9a4d9-195">Es necesario modificar la aplicación para cargar un certicate del almacén de certificados.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-195">We need to modify the application to load a certicate from the certificate store.</span></span> <span data-ttu-id="9a4d9-196">Para ello, nuestro plan de app service debe ser al menos en el nivel estándar cuando se configura en un paso posterior.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-196">To do so, our app service plan needs to be at least on the standard tier when we configure in a later step.</span></span> <span data-ttu-id="9a4d9-197">En nuestra aplicación, basta con modificar la sección IdentityServer en appsettings.json para incluir los detalles de la clave:</span><span class="sxs-lookup"><span data-stu-id="9a4d9-197">In our application we simply need to modify the IdentityServer section on appsettings.json to include the key details:</span></span>
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
* <span data-ttu-id="9a4d9-198">La propiedad nombre de certificado se corresponde con el distintivo asunto del certificado.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-198">The name property on certificate corresponds with the distinguished subject for the certificate.</span></span>
* <span data-ttu-id="9a4d9-199">La ubicación del almacén representa dónde se debe cargar el certificado desde (CurrentUrser o LocalMachine).</span><span class="sxs-lookup"><span data-stu-id="9a4d9-199">The store location represents where to load the certificate from (CurrentUrser or LocalMachine).</span></span>
* <span data-ttu-id="9a4d9-200">El nombre del almacén representa el nombre del almacén de certificados donde se almacena el certificado, en este caso apunta al almacén personal del usuario.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-200">The store name represents the name of the certificate store where the certificate is stored, in this case it points to the personal user store.</span></span>

<span data-ttu-id="9a4d9-201">Para implementar en Azure Websites, implemente la aplicación siguiendo los pasos de [implementar la aplicación en Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) para crear los recursos de Azure necesarios e implementar la aplicación en producción.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-201">To deploy to Azure Websites, deploy the app following the steps in [Deploy the app to Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) to create the necessary Azure resources and deploy the app to production.</span></span>

<span data-ttu-id="9a4d9-202">Una vez hecho esto, la aplicación se implementa en Azure, pero aún no está completamente funcional todavía es necesario configurar el certificado que va a usar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-202">After doing this, the application is deployed into Azure but is not yet completely functional as we still need to setup the certificate to be used by the application.</span></span> <span data-ttu-id="9a4d9-203">Para ello, necesitamos tener la huella digital del certificado que vamos a usar y siga los pasos descritos en [cargar sus certificados](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).</span><span class="sxs-lookup"><span data-stu-id="9a4d9-203">To do so, we need to have the thumbprint for the certificate we are going to use and follow the steps described in [Load your certificates](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).</span></span>

<span data-ttu-id="9a4d9-204">Aunque estos pasos mencionan SSL, hay una sección "Certificados privados" en el portal donde podemos cargar nuestro certificado aprovisionado para usar con nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-204">While these steps mention SSL, there is a "Private certificates" section on the portal where we can upload our provisioned certificate to use with our app.</span></span>

<span data-ttu-id="9a4d9-205">Después de este paso, debemos poder reiniciar la aplicación y debe ser completamente funcional.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-205">After this step, we should be able to restart our application and it should be completely functional.</span></span>

## <a name="other-configuration-options"></a><span data-ttu-id="9a4d9-206">Otras opciones de configuración</span><span class="sxs-lookup"><span data-stu-id="9a4d9-206">Other configuration options</span></span>
<span data-ttu-id="9a4d9-207">La compatibilidad para la autorización de API se basa en Identity Server con un conjunto de convenciones, valores predeterminados y mejoras para simplificar la experiencia para aplicaciones de página única.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-207">Our support for API authorization builds on top of Identity Server with a set of conventions, default values and enhancements to simplify the experience for Single Page Applications.</span></span> <span data-ttu-id="9a4d9-208">Obviamente, toda la funcionalidad de Identity Server está disponible en segundo plano si las integraciones que ofrecemos no cubren su escenario.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-208">Needless to say, the full power of Identity Server is available behind the scenes if the integrations that we offer don't cover your scenario.</span></span> <span data-ttu-id="9a4d9-209">Nuestro soporte técnico se centra en lo que llamamos aplicaciones "propias", donde todas las aplicaciones se crean e implementan en nuestra organización.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-209">Our support is focused on what we call "first-party" applications, where all the applications are created and deployed by our organization.</span></span> <span data-ttu-id="9a4d9-210">Por lo tanto no ofrecemos soporte para cosas como el consentimiento o la federación.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-210">As such we don't offer support for things like consent or federation.</span></span> <span data-ttu-id="9a4d9-211">Para estos escenarios, nuestra recomendación es utilizar servidor de identidades y seguir su documentación.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-211">For those scenarios our recommendation is to use Identity Server and follow their documentation.</span></span>

### <a name="application-profiles"></a><span data-ttu-id="9a4d9-212">Perfiles de aplicación</span><span class="sxs-lookup"><span data-stu-id="9a4d9-212">Application profiles</span></span>
<span data-ttu-id="9a4d9-213">Perfiles de aplicación son configuraciones predefinidas para las aplicaciones que definan sus parámetros.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-213">Application profiles are predefined configurations for applications that further define their parameters.</span></span> <span data-ttu-id="9a4d9-214">En este momento se admiten dos perfiles:</span><span class="sxs-lookup"><span data-stu-id="9a4d9-214">At this time we support two profiles:</span></span>
* <span data-ttu-id="9a4d9-215">IdentityServerSPA: Representa una aplicación hospedada por Identity Server como una sola unidad.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-215">IdentityServerSPA: Represents a single page application hosted alongside Identity Server as a single unit.</span></span>
  * <span data-ttu-id="9a4d9-216">Los redirect_uri el valor predeterminado es `/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-216">The redirect_uri defaults to `/authentication/login-callback`.</span></span>
  * <span data-ttu-id="9a4d9-217">Valor predeterminado es el post_logout_redirect_uri `/authentication/logout-callback`.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-217">The post_logout_redirect_uri defaults to `/authentication/logout-callback`.</span></span>
  * <span data-ttu-id="9a4d9-218">El conjunto de ámbitos incluye el `openid`, `profile`y cada ámbito definido para las API en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-218">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the application.</span></span>
  * <span data-ttu-id="9a4d9-219">El conjunto de tipos permitidos de respuesta OIDC es `id_token token` o cada uno de ellos individualmente (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="9a4d9-219">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="9a4d9-220">El modo de respuesta permitidos es `fragment`.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-220">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="9a4d9-221">SPA: Representa una aplicación de página única que no esté hospedada con Identity Server.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-221">SPA: Represents a single page application that is not hosted with Identity Server.</span></span>
  * <span data-ttu-id="9a4d9-222">El conjunto de ámbitos incluye el `openid`, `profile`y cada ámbito definido para las API en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-222">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the application.</span></span>
  * <span data-ttu-id="9a4d9-223">El conjunto de tipos permitidos de respuesta OIDC es `id_token token` o cada uno de ellos individualmente (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="9a4d9-223">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="9a4d9-224">El modo de respuesta permitidos es `fragment`.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-224">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="9a4d9-225">IdentityServerJwt: Representa una API que se hospeda junto con Identity Server.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-225">IdentityServerJwt: Represents an API that is hosted alongside with Identity Server.</span></span>
  * <span data-ttu-id="9a4d9-226">La aplicación está configurada para tener un ámbito que el valor predeterminado es el nombre de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-226">The application is configured to have a single scope that defaults to the application name.</span></span>
* <span data-ttu-id="9a4d9-227">API: Representa una API que no esté hospedada con Identity Server.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-227">API: Represents an API that is not hosted with Identity Server.</span></span>
  * <span data-ttu-id="9a4d9-228">La aplicación está configurada para tener un ámbito que el valor predeterminado es el nombre de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-228">The application is configured to have a single scope that defaults to the application name.</span></span>

### <a name="configuration-through-appsettings"></a><span data-ttu-id="9a4d9-229">Configuración a través de AppSettings</span><span class="sxs-lookup"><span data-stu-id="9a4d9-229">Configuration through AppSettings</span></span>
<span data-ttu-id="9a4d9-230">Podemos configurar las aplicaciones a través de nuestro sistema de configuración mediante la adición a la lista de clientes o recursos, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-230">We can configure the applications through our configuration system by adding them to the list of Clients or Resources respectively.</span></span> 

<span data-ttu-id="9a4d9-231">Al configurar los clientes podemos configurar el `redirect_uri` y `post_logout_redirect_uri` tal como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="9a4d9-231">When configuring clients we can configure the `redirect_uri` and the `post_logout_redirect_uri` as shown below:</span></span>
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

<span data-ttu-id="9a4d9-232">Al configurar recursos podemos configurar los ámbitos para el recurso tal y como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="9a4d9-232">When configuring resources we can configure the scopes for the resource as shown below:</span></span>
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

### <a name="configuration-through-code"></a><span data-ttu-id="9a4d9-233">Configuración mediante código</span><span class="sxs-lookup"><span data-stu-id="9a4d9-233">Configuration through code</span></span>
<span data-ttu-id="9a4d9-234">También podemos configurar los clientes y los recursos a través de código mediante una sobrecarga de AddApiAuthorization que realiza una acción para configurar las opciones.</span><span class="sxs-lookup"><span data-stu-id="9a4d9-234">We can also configure the clients and resources through code using an overload of AddApiAuthorization that takes an action to configure options.</span></span>
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
