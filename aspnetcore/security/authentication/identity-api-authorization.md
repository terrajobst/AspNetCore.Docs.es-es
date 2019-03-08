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
# <a name="authentication-and-authorization-for-spas"></a><span data-ttu-id="3ac02-103">Autenticación y autorización para las spa</span><span class="sxs-lookup"><span data-stu-id="3ac02-103">Authentication and authorization for SPAs</span></span>

<span data-ttu-id="3ac02-104">ASP.NET Core 3.0 o posterior ofrece una autenticación en aplicaciones de una sola página (SPA) mediante la compatibilidad para la autorización de API.</span><span class="sxs-lookup"><span data-stu-id="3ac02-104">ASP.NET Core 3.0 or later offers authentication in Single Page Apps (SPAs) using the support for API authorization.</span></span> <span data-ttu-id="3ac02-105">ASP.NET Core Identity para autenticar y almacenar los usuarios se combina con [IdentityServer](https://identityserver.io/) para la implementación de OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="3ac02-105">ASP.NET Core Identity for authenticating and storing users is combined with [IdentityServer](https://identityserver.io/) for implementing Open ID Connect.</span></span>

<span data-ttu-id="3ac02-106">Se agregó un parámetro de autenticación a la **Angular** y **reaccionar** plantillas que es similar al parámetro de la autenticación de proyecto la **aplicación Web (Model-View-Controller)**  (MVC) y **aplicación Web** (las páginas de Razor) plantillas de proyecto.</span><span class="sxs-lookup"><span data-stu-id="3ac02-106">An authentication parameter was added to the **Angular** and **React** project templates that is similar to the authentication parameter in the **Web Application (Model-View-Controller)** (MVC) and **Web Application** (Razor Pages) project templates.</span></span> <span data-ttu-id="3ac02-107">Los valores de parámetros permitidos son **ninguno** y **individuales**.</span><span class="sxs-lookup"><span data-stu-id="3ac02-107">The allowed parameter values are **None** and **Individual**.</span></span> <span data-ttu-id="3ac02-108">El **React.js y Redux** plantilla de proyecto no es compatible con el parámetro de autenticación en este momento.</span><span class="sxs-lookup"><span data-stu-id="3ac02-108">The **React.js and Redux** project template doesn't support the authentication parameter at this time.</span></span>

## <a name="create-an-app-with-api-authorization-support"></a><span data-ttu-id="3ac02-109">Crear una aplicación con el soporte técnico de autorización de API</span><span class="sxs-lookup"><span data-stu-id="3ac02-109">Create an app with API authorization support</span></span>

<span data-ttu-id="3ac02-110">Autorización y autenticación de usuario pueden utilizarse con Angular y React spa.</span><span class="sxs-lookup"><span data-stu-id="3ac02-110">User authentication and authorization can be used with both Angular and React SPAs.</span></span> <span data-ttu-id="3ac02-111">Abra un shell de comandos y ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="3ac02-111">Open a command shell, and run the following command:</span></span>

<span data-ttu-id="3ac02-112">**Angular**:</span><span class="sxs-lookup"><span data-stu-id="3ac02-112">**Angular**:</span></span>

```console
dotnet new angular -o <output_directory_name> -au Individual
```

<span data-ttu-id="3ac02-113">**React**:</span><span class="sxs-lookup"><span data-stu-id="3ac02-113">**React**:</span></span>

```console
dotnet new react -o <output_directory_name> -au Individual
```

<span data-ttu-id="3ac02-114">El comando anterior crea una aplicación de ASP.NET Core con un *ClientApp* directorio que contiene la SPA.</span><span class="sxs-lookup"><span data-stu-id="3ac02-114">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the SPA.</span></span>

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a><span data-ttu-id="3ac02-115">Descripción general de los componentes de ASP.NET Core de la aplicación</span><span class="sxs-lookup"><span data-stu-id="3ac02-115">General description of the ASP.NET Core components of the app</span></span>

<span data-ttu-id="3ac02-116">Las secciones siguientes describen las adiciones al proyecto cuando se incluye compatibilidad con la autenticación:</span><span class="sxs-lookup"><span data-stu-id="3ac02-116">The following sections describe additions to the project when authentication support is included:</span></span>

### <a name="startup-class"></a><span data-ttu-id="3ac02-117">Clase de inicio</span><span class="sxs-lookup"><span data-stu-id="3ac02-117">Startup class</span></span>

<span data-ttu-id="3ac02-118">La `Startup` clase tiene las siguientes adiciones:</span><span class="sxs-lookup"><span data-stu-id="3ac02-118">The `Startup` class has the following additions:</span></span>

* <span data-ttu-id="3ac02-119">Dentro de la `Startup.ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="3ac02-119">Inside the `Startup.ConfigureServices` method:</span></span>
  * <span data-ttu-id="3ac02-120">Identidad con la interfaz de usuario predeterminada:</span><span class="sxs-lookup"><span data-stu-id="3ac02-120">Identity with the default UI:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * <span data-ttu-id="3ac02-121">IdentityServer con más `AddApiAuthorization` método auxiliar que configuraciones algunas convenciones de ASP.NET Core en la parte superior IdentityServer de predeterminado:</span><span class="sxs-lookup"><span data-stu-id="3ac02-121">IdentityServer with an additional `AddApiAuthorization` helper method that setups some default ASP.NET Core conventions on top of IdentityServer:</span></span>

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * <span data-ttu-id="3ac02-122">Autenticación con otros `AddIdentityServerJwt` método auxiliar que configura la aplicación para validar los tokens JWT generado por IdentityServer:</span><span class="sxs-lookup"><span data-stu-id="3ac02-122">Authentication with an additional `AddIdentityServerJwt` helper method that configures the app to validate JWT tokens produced by IdentityServer:</span></span>

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* <span data-ttu-id="3ac02-123">Dentro de la `Startup.Configure` método:</span><span class="sxs-lookup"><span data-stu-id="3ac02-123">Inside the `Startup.Configure` method:</span></span>
  * <span data-ttu-id="3ac02-124">El middleware de autenticación que se encarga de validar las credenciales de la solicitud y establecer el usuario en el contexto de solicitud:</span><span class="sxs-lookup"><span data-stu-id="3ac02-124">The authentication middleware that is responsible for validating the request credentials and setting the user on the request context:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

  * <span data-ttu-id="3ac02-125">El middleware IdentityServer que expone los puntos de conexión de Open ID Connect:</span><span class="sxs-lookup"><span data-stu-id="3ac02-125">The IdentityServer middleware that exposes the Open ID Connect endpoints:</span></span>

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="3ac02-126">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="3ac02-126">AddApiAuthorization</span></span>

<span data-ttu-id="3ac02-127">Este método auxiliar configura IdentityServer para usar la configuración admitida.</span><span class="sxs-lookup"><span data-stu-id="3ac02-127">This helper method configures IdentityServer to use our supported configuration.</span></span> <span data-ttu-id="3ac02-128">IdentityServer es un marco eficaz y extensible para controlar cuestiones de seguridad de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3ac02-128">IdentityServer is a powerful and extensible framework for handling app security concerns.</span></span> <span data-ttu-id="3ac02-129">Al mismo tiempo, que expone una complejidad innecesaria para los escenarios más comunes.</span><span class="sxs-lookup"><span data-stu-id="3ac02-129">At the same time, that exposes unnecessary complexity for the most common scenarios.</span></span> <span data-ttu-id="3ac02-130">Por lo tanto, se proporciona un conjunto de convenciones y las opciones de configuración que se consideran un buen punto de partida.</span><span class="sxs-lookup"><span data-stu-id="3ac02-130">Consequently, a set of conventions and configuration options is provided to you that are considered a good starting point.</span></span> <span data-ttu-id="3ac02-131">Una vez que la autenticación de sus necesidades evolucionan, toda la eficacia de IdentityServer sigue estando disponible para personalizar la autenticación para satisfacer sus necesidades.</span><span class="sxs-lookup"><span data-stu-id="3ac02-131">Once your authentication needs change, the full power of IdentityServer is still available to customize authentication to suit your needs.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="3ac02-132">AddIdentityServerJwt</span><span class="sxs-lookup"><span data-stu-id="3ac02-132">AddIdentityServerJwt</span></span>

<span data-ttu-id="3ac02-133">Este método auxiliar configura una combinación de directivas para la aplicación como el controlador de autenticación predeterminado.</span><span class="sxs-lookup"><span data-stu-id="3ac02-133">This helper method configures a policy scheme for the app as the default authentication handler.</span></span> <span data-ttu-id="3ac02-134">La directiva está configurada para permitir que la identidad de controlar todas las solicitudes que se enrutan a cualquier subtrazado en el espacio de direcciones URL de identidad "/ identidad".</span><span class="sxs-lookup"><span data-stu-id="3ac02-134">The policy is configured to let Identity handle all requests routed to any subpath in the Identity URL space "/Identity".</span></span> <span data-ttu-id="3ac02-135">El `JwtBearerHandler` controla todas las demás solicitudes.</span><span class="sxs-lookup"><span data-stu-id="3ac02-135">The `JwtBearerHandler` handles all other requests.</span></span> <span data-ttu-id="3ac02-136">Además, este método registra un `<<ApplicationName>>API` recurso de API con IdentityServer con un ámbito predeterminado de `<<ApplicationName>>API` y configura el middleware del token de portador de JWT para validar los tokens emitidos por IdentityServer para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3ac02-136">Additionally, this method registers an `<<ApplicationName>>API` API resource with IdentityServer with a default scope of `<<ApplicationName>>API` and configures the JWT Bearer token middleware to validate tokens issued by IdentityServer for the app.</span></span>

### <a name="sampledatacontroller"></a><span data-ttu-id="3ac02-137">SampleDataController</span><span class="sxs-lookup"><span data-stu-id="3ac02-137">SampleDataController</span></span>

<span data-ttu-id="3ac02-138">En el *Controllers\SampleDataController.cs* de archivo, tenga en cuenta el `[Authorize]` atributo aplicado a la clase que indica que el usuario debe tener autorización en función de la directiva predeterminada para tener acceso al recurso.</span><span class="sxs-lookup"><span data-stu-id="3ac02-138">In the *Controllers\SampleDataController.cs* file, notice the `[Authorize]` attribute applied to the class that indicates that the user needs to be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="3ac02-139">La directiva de autorización predeterminado ocurre deberá estar configurado para usar el esquema de autenticación predeterminado, que se configura de forma `AddIdentityServerJwt` en el esquema de la directiva que se ha mencionado anteriormente, que hace el `JwtBearerHandler` configurado por el método de este tipo auxiliar del controlador predeterminado para solicitudes a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3ac02-139">The default authorization policy happens to be configured to use the default authentication scheme, which is set up by `AddIdentityServerJwt` to the policy scheme that was mentioned above, making the `JwtBearerHandler` configured by such helper method the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="3ac02-140">ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="3ac02-140">ApplicationDbContext</span></span>

<span data-ttu-id="3ac02-141">En el *Data\ApplicationDbContext.cs* de archivos, haremos lo mismo `DbContext` identidad se usa con la excepción que extiende `ApiAuthorizationDbContext` (más derivado de clase de `IdentityDbContext`) para incluir el esquema para IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="3ac02-141">In the *Data\ApplicationDbContext.cs* file, notice the same `DbContext` is used in Identity with the exception that it extends `ApiAuthorizationDbContext` (a more derived class from `IdentityDbContext`) to include the schema for IdentityServer.</span></span>

<span data-ttu-id="3ac02-142">Para tener control total sobre el esquema de base de datos, hereda de una de las identidades disponibles `DbContext` clases y configurar el contexto para incluir el esquema de identidad mediante una llamada a `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` en el `OnModelCreating` método.</span><span class="sxs-lookup"><span data-stu-id="3ac02-142">To gain full control of the database schema, inherit from one of the available Identity `DbContext` classes and configure the context to include the Identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` on the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="3ac02-143">OidcConfigurationController</span><span class="sxs-lookup"><span data-stu-id="3ac02-143">OidcConfigurationController</span></span>

<span data-ttu-id="3ac02-144">En el *Controllers\OidcConfigurationController.cs* de archivo, tenga en cuenta el punto de conexión que se aprovisiona para dar servicio a los parámetros OIDC que el cliente debe usar.</span><span class="sxs-lookup"><span data-stu-id="3ac02-144">In the *Controllers\OidcConfigurationController.cs* file, notice the endpoint that's provisioned to serve the OIDC parameters that the client needs to use.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="3ac02-145">appsettings.json</span><span class="sxs-lookup"><span data-stu-id="3ac02-145">appsettings.json</span></span>

<span data-ttu-id="3ac02-146">En el *appsettings.json* archivo de la raíz del proyecto, hay un nuevo `IdentityServer` configurado de sección que describe la lista de clientes.</span><span class="sxs-lookup"><span data-stu-id="3ac02-146">In the *appsettings.json* file of the project root, there's a new `IdentityServer` section that describes the list of configured clients.</span></span> <span data-ttu-id="3ac02-147">En el ejemplo siguiente, hay un solo cliente.</span><span class="sxs-lookup"><span data-stu-id="3ac02-147">In the following example, there's a single client.</span></span> <span data-ttu-id="3ac02-148">El nombre del cliente corresponde con el nombre de la aplicación y se asigna por convención para el OAuth `ClientId` parámetro.</span><span class="sxs-lookup"><span data-stu-id="3ac02-148">The client name corresponds to the app name and is mapped by convention to the OAuth `ClientId` parameter.</span></span> <span data-ttu-id="3ac02-149">El perfil indica el tipo de aplicación que se está configurando.</span><span class="sxs-lookup"><span data-stu-id="3ac02-149">The profile indicates the app type being configured.</span></span> <span data-ttu-id="3ac02-150">Se usa internamente para las convenciones de la unidad que simplifican el proceso de configuración para el servidor.</span><span class="sxs-lookup"><span data-stu-id="3ac02-150">It's used internally to drive conventions that simplify the configuration process for the server.</span></span> <span data-ttu-id="3ac02-151">Hay varios perfiles disponibles, como se explica en el [perfiles de aplicación](#application-profiles) sección.</span><span class="sxs-lookup"><span data-stu-id="3ac02-151">There are several profiles available, as explained in the [Application profiles](#application-profiles) section.</span></span>

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a><span data-ttu-id="3ac02-152">appsettings.Development.json</span><span class="sxs-lookup"><span data-stu-id="3ac02-152">appsettings.Development.json</span></span>

<span data-ttu-id="3ac02-153">En el *appsettings. Development.JSON* archivo de la raíz del proyecto, hay un `IdentityServer` sección que describe la clave utilizada para firmar los tokens.</span><span class="sxs-lookup"><span data-stu-id="3ac02-153">In the *appsettings.Development.json* file of the project root, there's an `IdentityServer` section that describes the key used to sign tokens.</span></span> <span data-ttu-id="3ac02-154">Al implementar en producción, debe una clave se aprovisione e implemente junto con la aplicación, como se explica en el [implementar en producción](#deploy-to-production) sección.</span><span class="sxs-lookup"><span data-stu-id="3ac02-154">When deploying to production, a key needs to be provisioned and deployed alongside the app, as explained in the [Deploy to production](#deploy-to-production) section.</span></span>

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="general-description-of-the-angular-app"></a><span data-ttu-id="3ac02-155">Descripción general de la aplicación Angular</span><span class="sxs-lookup"><span data-stu-id="3ac02-155">General description of the Angular app</span></span>

<span data-ttu-id="3ac02-156">Admiten la autenticación y autorización de API en el Angular plantilla reside en su propio módulo Angular en el *ClientApp\src\api autorización* directory.</span><span class="sxs-lookup"><span data-stu-id="3ac02-156">The authentication and API authorization support in the Angular template resides in its own Angular module in the *ClientApp\src\api-authorization* directory.</span></span> <span data-ttu-id="3ac02-157">El módulo se compone de los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="3ac02-157">The module is composed of the following elements:</span></span>

* <span data-ttu-id="3ac02-158">3 componentes:</span><span class="sxs-lookup"><span data-stu-id="3ac02-158">3 components:</span></span>
  * <span data-ttu-id="3ac02-159">*login.component.ts*: Controla el flujo de inicio de sesión de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3ac02-159">*login.component.ts*: Handles the app's login flow.</span></span>
  * <span data-ttu-id="3ac02-160">*logout.component.ts*: Controla el flujo de cierre de sesión de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3ac02-160">*logout.component.ts*: Handles the app's logout flow.</span></span>
  * <span data-ttu-id="3ac02-161">*login-menu.component.ts*: Un widget que muestra uno de los siguientes conjuntos de vínculos:</span><span class="sxs-lookup"><span data-stu-id="3ac02-161">*login-menu.component.ts*: A widget that displays one of the following sets of links:</span></span>
    * <span data-ttu-id="3ac02-162">Administración de perfiles de usuario y vínculos cuando el usuario se autentica al cerrar la sesión.</span><span class="sxs-lookup"><span data-stu-id="3ac02-162">User profile management and log out links when the user is authenticated.</span></span>
    * <span data-ttu-id="3ac02-163">Registro y registro en los vínculos cuando el usuario no está autenticado.</span><span class="sxs-lookup"><span data-stu-id="3ac02-163">Registration and log in links when the user isn't authenticated.</span></span>
* <span data-ttu-id="3ac02-164">Una restricción de ruta `AuthorizeGuard` que pueden agregarse a las rutas y requiere que el usuario se autentique antes de visitar la ruta.</span><span class="sxs-lookup"><span data-stu-id="3ac02-164">A route guard `AuthorizeGuard` that can be added to routes and requires a user to be authenticated before visiting the route.</span></span>
* <span data-ttu-id="3ac02-165">Un interceptor HTTP `AuthorizeInterceptor` que asocia el token de acceso a las solicitudes HTTP salientes destinadas a la API cuando se autentica el usuario.</span><span class="sxs-lookup"><span data-stu-id="3ac02-165">An HTTP interceptor `AuthorizeInterceptor` that attaches the access token to outgoing HTTP requests targeting the API when the user is authenticated.</span></span>
* <span data-ttu-id="3ac02-166">Un servicio `AuthorizeService` que controla los detalles de bajo nivel del proceso de autenticación y expone información sobre el usuario autenticado para el resto de la aplicación para su uso.</span><span class="sxs-lookup"><span data-stu-id="3ac02-166">A service `AuthorizeService` that handles the lower-level details of the authentication process and exposes information about the authenticated user to the rest of the app for consumption.</span></span>
* <span data-ttu-id="3ac02-167">Un módulo Angular que define las rutas asociadas a las partes de la autenticación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3ac02-167">An Angular module that defines routes associated with the authentication parts of the app.</span></span> <span data-ttu-id="3ac02-168">Expone el componente de menú de inicio de sesión, el interceptor, la protección y el servicio para el consumo del resto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3ac02-168">It exposes the login menu component, the interceptor, the guard, and the service for consumption from the rest of the app.</span></span>

## <a name="general-description-of-the-react-app"></a><span data-ttu-id="3ac02-169">Descripción general de la aplicación React</span><span class="sxs-lookup"><span data-stu-id="3ac02-169">General description of the React app</span></span>

<span data-ttu-id="3ac02-170">La compatibilidad para la autenticación y autorización de API en la plantilla de React reside en el *ClientApp\src\components\api autorización* directory.</span><span class="sxs-lookup"><span data-stu-id="3ac02-170">The support for authentication and API authorization in the React template resides in the *ClientApp\src\components\api-authorization* directory.</span></span> <span data-ttu-id="3ac02-171">Se compone de los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="3ac02-171">It's composed of the following elements:</span></span>

* <span data-ttu-id="3ac02-172">4 componentes:</span><span class="sxs-lookup"><span data-stu-id="3ac02-172">4 components:</span></span>
  * <span data-ttu-id="3ac02-173">*Login.js*: Controla el flujo de inicio de sesión de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3ac02-173">*Login.js*: Handles the app's login flow.</span></span>
  * <span data-ttu-id="3ac02-174">*Logout.js*: Controla el flujo de cierre de sesión de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3ac02-174">*Logout.js*: Handles the app's logout flow.</span></span>
  * <span data-ttu-id="3ac02-175">*LoginMenu.js*: Un widget que muestra uno de los siguientes conjuntos de vínculos:</span><span class="sxs-lookup"><span data-stu-id="3ac02-175">*LoginMenu.js*: A widget that displays one of the following sets of links:</span></span>
    * <span data-ttu-id="3ac02-176">Administración de perfiles de usuario y vínculos cuando el usuario se autentica al cerrar la sesión.</span><span class="sxs-lookup"><span data-stu-id="3ac02-176">User profile management and log out links when the user is authenticated.</span></span>
    * <span data-ttu-id="3ac02-177">Registro y registro en los vínculos cuando el usuario no está autenticado.</span><span class="sxs-lookup"><span data-stu-id="3ac02-177">Registration and log in links when the user isn't authenticated.</span></span>
  * <span data-ttu-id="3ac02-178">*AuthorizeRoute.js*: Indica un componente de ruta que requiere que el usuario se autentique antes de representar el componente en el `Component` parámetro.</span><span class="sxs-lookup"><span data-stu-id="3ac02-178">*AuthorizeRoute.js*: A route component that requires a user to be authenticated before rendering the component indicated in the `Component` parameter.</span></span>
* <span data-ttu-id="3ac02-179">Un exportado `authService` instancia de clase `AuthorizeService` que controla los detalles de bajo nivel del proceso de autenticación y expone información sobre el usuario autenticado para el resto de la aplicación para su uso.</span><span class="sxs-lookup"><span data-stu-id="3ac02-179">An exported `authService` instance of class `AuthorizeService` that handles the lower-level details of the authentication process and exposes information about the authenticated user to the rest of the app for consumption.</span></span>

<span data-ttu-id="3ac02-180">Ahora que ha visto los componentes principales de la solución, se puede echar un vistazo más profundo en distintos escenarios para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3ac02-180">Now that you've seen the main components of the solution, you can take a deeper look at individual scenarios for the app.</span></span>

## <a name="require-authorization-on-a-new-api"></a><span data-ttu-id="3ac02-181">Requerir autorización sobre una nueva API</span><span class="sxs-lookup"><span data-stu-id="3ac02-181">Require authorization on a new API</span></span>

<span data-ttu-id="3ac02-182">De forma predeterminada, el sistema está configurado para requerir autorización fácilmente para las nuevas API.</span><span class="sxs-lookup"><span data-stu-id="3ac02-182">By default, the system is configured to easily require authorization for new APIs.</span></span> <span data-ttu-id="3ac02-183">Para ello, cree un nuevo controlador y agregue el `[Authorize]` atributo a la clase de controlador o a cualquier acción en el controlador.</span><span class="sxs-lookup"><span data-stu-id="3ac02-183">To do so, create a new controller and add the `[Authorize]` attribute to the controller class or to any action within the controller.</span></span>

## <a name="protect-a-client-side-route-angular"></a><span data-ttu-id="3ac02-184">Proteger una ruta del lado cliente (Angular)</span><span class="sxs-lookup"><span data-stu-id="3ac02-184">Protect a client-side route (Angular)</span></span>

<span data-ttu-id="3ac02-185">Protección de una ruta del lado cliente se realiza mediante la adición de la protección de autorizar a la lista de restricciones que se ejecutará cuando la configuración de una ruta.</span><span class="sxs-lookup"><span data-stu-id="3ac02-185">Protecting a client-side route is done by adding the authorize guard to the list of guards to run when configuring a route.</span></span> <span data-ttu-id="3ac02-186">Por ejemplo, puede ver cómo el `fetch-data` ruta está configurada en el módulo de aplicación principal Angular:</span><span class="sxs-lookup"><span data-stu-id="3ac02-186">As an example, you can see how the `fetch-data` route is configured within the main app Angular module:</span></span>

```typescript
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

<span data-ttu-id="3ac02-187">Es importante mencionar que la protección de una ruta no protege el punto de conexión real (que todavía se requiere un `[Authorize]` atributo aplicado a él), pero que solo se evita que el usuario de navegación a la ruta del lado cliente dada al que no está autenticado.</span><span class="sxs-lookup"><span data-stu-id="3ac02-187">It's important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client-side route when it isn't authenticated.</span></span>

## <a name="authenticate-api-requests-angular"></a><span data-ttu-id="3ac02-188">Autenticar solicitudes de API (Angular)</span><span class="sxs-lookup"><span data-stu-id="3ac02-188">Authenticate API requests (Angular)</span></span>

<span data-ttu-id="3ac02-189">Autenticar solicitudes a las API hospedadas por la aplicación se realiza automáticamente mediante el uso del interceptor de cliente HTTP definido por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3ac02-189">Authenticating requests to APIs hosted alongside the app is done automatically through the use of the HTTP client interceptor defined by the app.</span></span>

## <a name="protect-a-client-side-route-react"></a><span data-ttu-id="3ac02-190">Proteger una ruta del lado cliente (React)</span><span class="sxs-lookup"><span data-stu-id="3ac02-190">Protect a client-side route (React)</span></span>

<span data-ttu-id="3ac02-191">Proteger una ruta del lado cliente mediante el `AuthorizeRoute` componente en lugar de con el valor plain `Route` componente.</span><span class="sxs-lookup"><span data-stu-id="3ac02-191">Protect a client-side route by using the `AuthorizeRoute` component instead of the plain `Route` component.</span></span> <span data-ttu-id="3ac02-192">Por ejemplo, observe cómo la `fetch-data` ruta está configurada en el `App` componente:</span><span class="sxs-lookup"><span data-stu-id="3ac02-192">For example, notice how the `fetch-data` route is configured within the `App` component:</span></span>

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

<span data-ttu-id="3ac02-193">Protección de una ruta:</span><span class="sxs-lookup"><span data-stu-id="3ac02-193">Protecting a route:</span></span>

* <span data-ttu-id="3ac02-194">No protege el punto de conexión real (que todavía se requiere un `[Authorize]` atributo aplicado a él).</span><span class="sxs-lookup"><span data-stu-id="3ac02-194">Doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it).</span></span>
* <span data-ttu-id="3ac02-195">Solo impide al usuario navegar a la ruta del lado cliente dada al que no está autenticado.</span><span class="sxs-lookup"><span data-stu-id="3ac02-195">Only prevents the user from navigating to the given client-side route when it isn't authenticated.</span></span>

## <a name="authenticate-api-requests-react"></a><span data-ttu-id="3ac02-196">Autenticar solicitudes de API (React)</span><span class="sxs-lookup"><span data-stu-id="3ac02-196">Authenticate API requests (React)</span></span>

<span data-ttu-id="3ac02-197">Autenticar las solicitudes con React se realiza mediante la importación de la primera la `authService` instancia desde el `AuthorizeService`.</span><span class="sxs-lookup"><span data-stu-id="3ac02-197">Authenticating requests with React is done by first importing the `authService` instance from the `AuthorizeService`.</span></span> <span data-ttu-id="3ac02-198">El token de acceso se recupera de la `authService` y se adjunta a la solicitud, tal como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="3ac02-198">The access token is retrieved from the `authService` and is attached to the request as shown below.</span></span> <span data-ttu-id="3ac02-199">En los componentes de React, este trabajo se realiza normalmente en el `componentDidMount` método del ciclo de vida o que el resultado de una interacción del usuario.</span><span class="sxs-lookup"><span data-stu-id="3ac02-199">In React components, this work is typically done in the `componentDidMount` lifecycle method or as the result from some user interaction.</span></span>

### <a name="import-the-authservice-into-your-component"></a><span data-ttu-id="3ac02-200">Importar el authService su componente</span><span class="sxs-lookup"><span data-stu-id="3ac02-200">Import the authService into your component</span></span>

```javascript
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a><span data-ttu-id="3ac02-201">Recuperar y adjunte el token de acceso a la respuesta</span><span class="sxs-lookup"><span data-stu-id="3ac02-201">Retrieve and attach the access token to the response</span></span>

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

## <a name="deploy-to-production"></a><span data-ttu-id="3ac02-202">Implementar en producción</span><span class="sxs-lookup"><span data-stu-id="3ac02-202">Deploy to production</span></span>

<span data-ttu-id="3ac02-203">Para implementar la aplicación en producción, deben aprovisionar los recursos siguientes:</span><span class="sxs-lookup"><span data-stu-id="3ac02-203">To deploy the app to production, the following resources need to be provisioned:</span></span>

* <span data-ttu-id="3ac02-204">Una base de datos para almacenar las cuentas de usuario de identidad y las concesiones IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="3ac02-204">A database to store the Identity user accounts and the IdentityServer grants.</span></span>
* <span data-ttu-id="3ac02-205">Un certificado de producción que se usará para firmar los tokens.</span><span class="sxs-lookup"><span data-stu-id="3ac02-205">A production certificate to use for signing tokens.</span></span>
  * <span data-ttu-id="3ac02-206">No hay ningún requisito específico para este certificado; puede ser un certificado autofirmado o un certificado a través de una entidad CA.</span><span class="sxs-lookup"><span data-stu-id="3ac02-206">There are no specific requirements for this certificate; it can be a self-signed certificate or a certificate provisioned through a CA authority.</span></span>
  * <span data-ttu-id="3ac02-207">Se puede generar a través de herramientas estándares, como PowerShell o OpenSSL.</span><span class="sxs-lookup"><span data-stu-id="3ac02-207">It can be generated through standard tools like PowerShell or OpenSSL.</span></span>
  * <span data-ttu-id="3ac02-208">Puede ser instalado en el almacén de certificados en los equipos de destino o implementar como un *.pfx* archivo con una contraseña segura.</span><span class="sxs-lookup"><span data-stu-id="3ac02-208">It can be installed into the certificate store on the target machines or deployed as a *.pfx* file with a strong password.</span></span>

### <a name="example-deploy-to-azure-websites"></a><span data-ttu-id="3ac02-209">Ejemplo: Implemente en Azure Websites</span><span class="sxs-lookup"><span data-stu-id="3ac02-209">Example: Deploy to Azure Websites</span></span>

<span data-ttu-id="3ac02-210">Esta sección describe la implementación de la aplicación en Azure websites que usan un certificado almacenado en el almacén de certificados.</span><span class="sxs-lookup"><span data-stu-id="3ac02-210">This section describes deploying the app to Azure websites using a certificate stored in the certificate store.</span></span> <span data-ttu-id="3ac02-211">Para modificar la aplicación para cargar un certificado del almacén de certificados, el plan de App Service debe ser al menos el nivel estándar cuando se configura en un paso posterior.</span><span class="sxs-lookup"><span data-stu-id="3ac02-211">To modify the app to load a certificate from the certificate store, the App Service plan needs to be on at least the Standard tier when you configure in a later step.</span></span> <span data-ttu-id="3ac02-212">En la aplicación *appsettings.json* de archivos, modifique la `IdentityServer` sección deben incluir los detalles claves:</span><span class="sxs-lookup"><span data-stu-id="3ac02-212">In the app's *appsettings.json* file, modify the `IdentityServer` section to include the key details:</span></span>

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

* <span data-ttu-id="3ac02-213">La propiedad nombre de certificado se corresponde con el distintivo asunto del certificado.</span><span class="sxs-lookup"><span data-stu-id="3ac02-213">The name property on certificate corresponds with the distinguished subject for the certificate.</span></span>
* <span data-ttu-id="3ac02-214">La ubicación del almacén representa dónde se debe cargar el certificado desde (`CurrentUser` o `LocalMachine`).</span><span class="sxs-lookup"><span data-stu-id="3ac02-214">The store location represents where to load the certificate from (`CurrentUser` or `LocalMachine`).</span></span>
* <span data-ttu-id="3ac02-215">El nombre del almacén representa el nombre del almacén de certificados donde se almacena el certificado.</span><span class="sxs-lookup"><span data-stu-id="3ac02-215">The store name represents the name of the certificate store where the certificate is stored.</span></span> <span data-ttu-id="3ac02-216">En este caso, señala al almacén personal del usuario.</span><span class="sxs-lookup"><span data-stu-id="3ac02-216">In this case, it points to the personal user store.</span></span>

<span data-ttu-id="3ac02-217">Para implementar en Azure Websites, implemente la aplicación siguiendo los pasos de [implementar la aplicación en Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) para crear los recursos de Azure necesarios e implementar la aplicación en producción.</span><span class="sxs-lookup"><span data-stu-id="3ac02-217">To deploy to Azure Websites, deploy the app following the steps in [Deploy the app to Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) to create the necessary Azure resources and deploy the app to production.</span></span>

<span data-ttu-id="3ac02-218">Después de seguir las instrucciones anteriores, la aplicación se implementa en Azure pero todavía no es funcional.</span><span class="sxs-lookup"><span data-stu-id="3ac02-218">After following the preceding instructions, the app is deployed to Azure but isn't yet functional.</span></span> <span data-ttu-id="3ac02-219">El certificado utilizado por la aplicación todavía debe configurarse.</span><span class="sxs-lookup"><span data-stu-id="3ac02-219">The certificate used by the app still needs to be set up.</span></span> <span data-ttu-id="3ac02-220">Busque la huella digital del certificado que se usará y siga los pasos descritos en [cargar sus certificados](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).</span><span class="sxs-lookup"><span data-stu-id="3ac02-220">Locate the thumbprint for the certificate to be used, and follow the steps described in [Load your certificates](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).</span></span>

<span data-ttu-id="3ac02-221">Aunque estos pasos mencionan SSL, no hay un **certificados privados** sección en el portal donde puede cargar el certificado aprovisionado para usar con la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3ac02-221">While these steps mention SSL, there's a **Private certificates** section on the portal where you can upload the provisioned certificate to use with the app.</span></span>

<span data-ttu-id="3ac02-222">Después de este paso, reinicie la aplicación y debe ser funcional.</span><span class="sxs-lookup"><span data-stu-id="3ac02-222">After this step, restart the app and it should be functional.</span></span>

## <a name="other-configuration-options"></a><span data-ttu-id="3ac02-223">Otras opciones de configuración</span><span class="sxs-lookup"><span data-stu-id="3ac02-223">Other configuration options</span></span>

<span data-ttu-id="3ac02-224">La compatibilidad para la autorización de API se basa en IdentityServer con un conjunto de convenciones, valores predeterminados y mejoras para simplificar la experiencia para las spa.</span><span class="sxs-lookup"><span data-stu-id="3ac02-224">The support for API authorization builds on top of IdentityServer with a set of conventions, default values, and enhancements to simplify the experience for SPAs.</span></span> <span data-ttu-id="3ac02-225">Obviamente, toda la eficacia de IdentityServer está disponible en segundo plano si las aplicaciones integradas de ASP.NET Core no cubren su escenario.</span><span class="sxs-lookup"><span data-stu-id="3ac02-225">Needless to say, the full power of IdentityServer is available behind the scenes if the ASP.NET Core integrations don't cover your scenario.</span></span> <span data-ttu-id="3ac02-226">La compatibilidad de ASP.NET Core se centra en las aplicaciones "propias", donde todas las aplicaciones se crean e implementan en nuestra organización.</span><span class="sxs-lookup"><span data-stu-id="3ac02-226">The ASP.NET Core support is focused on "first-party" apps, where all the apps are created and deployed by our organization.</span></span> <span data-ttu-id="3ac02-227">Por lo tanto, no se ofrece soporte técnico para cosas como el consentimiento o la federación.</span><span class="sxs-lookup"><span data-stu-id="3ac02-227">As such, support isn't offered for things like consent or federation.</span></span> <span data-ttu-id="3ac02-228">Para esos escenarios, use IdentityServer y seguir su documentación.</span><span class="sxs-lookup"><span data-stu-id="3ac02-228">For those scenarios, use IdentityServer and follow their documentation.</span></span>

### <a name="application-profiles"></a><span data-ttu-id="3ac02-229">Perfiles de aplicación</span><span class="sxs-lookup"><span data-stu-id="3ac02-229">Application profiles</span></span>

<span data-ttu-id="3ac02-230">Perfiles de aplicación son configuraciones predefinidas para las aplicaciones que definan sus parámetros.</span><span class="sxs-lookup"><span data-stu-id="3ac02-230">Application profiles are predefined configurations for apps that further define their parameters.</span></span> <span data-ttu-id="3ac02-231">En este momento, se admiten los siguientes perfiles:</span><span class="sxs-lookup"><span data-stu-id="3ac02-231">At this time, the following profiles are supported:</span></span>

* <span data-ttu-id="3ac02-232">`IdentityServerSPA`: Representa una SPA hospedada por IdentityServer como una sola unidad.</span><span class="sxs-lookup"><span data-stu-id="3ac02-232">`IdentityServerSPA`: Represents a SPA hosted alongside IdentityServer as a single unit.</span></span>
  * <span data-ttu-id="3ac02-233">El `redirect_uri` el valor predeterminado es `/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="3ac02-233">The `redirect_uri` defaults to `/authentication/login-callback`.</span></span>
  * <span data-ttu-id="3ac02-234">El `post_logout_redirect_uri` el valor predeterminado es `/authentication/logout-callback`.</span><span class="sxs-lookup"><span data-stu-id="3ac02-234">The `post_logout_redirect_uri` defaults to `/authentication/logout-callback`.</span></span>
  * <span data-ttu-id="3ac02-235">El conjunto de ámbitos incluye el `openid`, `profile`y cada ámbito definido para las API en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3ac02-235">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the app.</span></span>
  * <span data-ttu-id="3ac02-236">El conjunto de tipos permitidos de respuesta OIDC es `id_token token` o cada uno de ellos individualmente (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="3ac02-236">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="3ac02-237">El modo de respuesta permitidos es `fragment`.</span><span class="sxs-lookup"><span data-stu-id="3ac02-237">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="3ac02-238">`SPA`: Representa una SPA no está hospedada con IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="3ac02-238">`SPA`: Represents a SPA that isn't hosted with IdentityServer.</span></span>
  * <span data-ttu-id="3ac02-239">El conjunto de ámbitos incluye el `openid`, `profile`y cada ámbito definido para las API en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3ac02-239">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the app.</span></span>
  * <span data-ttu-id="3ac02-240">El conjunto de tipos permitidos de respuesta OIDC es `id_token token` o cada uno de ellos individualmente (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="3ac02-240">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="3ac02-241">El modo de respuesta permitidos es `fragment`.</span><span class="sxs-lookup"><span data-stu-id="3ac02-241">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="3ac02-242">`IdentityServerJwt`: Representa una API que se hospeda junto con IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="3ac02-242">`IdentityServerJwt`: Represents an API that is hosted alongside with IdentityServer.</span></span>
  * <span data-ttu-id="3ac02-243">La aplicación está configurada para tener un ámbito que el valor predeterminado es el nombre de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3ac02-243">The app is configured to have a single scope that defaults to the app name.</span></span>
* <span data-ttu-id="3ac02-244">`API`: Representa una API que no está hospedada con IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="3ac02-244">`API`: Represents an API that isn't hosted with IdentityServer.</span></span>
  * <span data-ttu-id="3ac02-245">La aplicación está configurada para tener un ámbito que el valor predeterminado es el nombre de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3ac02-245">The app is configured to have a single scope that defaults to the app name.</span></span>

### <a name="configuration-through-appsettings"></a><span data-ttu-id="3ac02-246">Configuración a través de AppSettings</span><span class="sxs-lookup"><span data-stu-id="3ac02-246">Configuration through AppSettings</span></span>

<span data-ttu-id="3ac02-247">Configure las aplicaciones a través del sistema de configuración agregándolos a la lista de `Clients` o `Resources`.</span><span class="sxs-lookup"><span data-stu-id="3ac02-247">Configure the apps through the configuration system by adding them to the list of `Clients` or `Resources`.</span></span>

<span data-ttu-id="3ac02-248">Configuración de cada cliente `redirect_uri` y `post_logout_redirect_uri` propiedad, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="3ac02-248">Configure each client's `redirect_uri` and `post_logout_redirect_uri` property, as shown in the following example:</span></span>

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

<span data-ttu-id="3ac02-249">Al configurar los recursos, puede configurar los ámbitos para el recurso tal y como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="3ac02-249">When configuring resources, you can configure the scopes for the resource as shown below:</span></span>

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

### <a name="configuration-through-code"></a><span data-ttu-id="3ac02-250">Configuración mediante código</span><span class="sxs-lookup"><span data-stu-id="3ac02-250">Configuration through code</span></span>

<span data-ttu-id="3ac02-251">También puede configurar los clientes y los recursos a través de código mediante una sobrecarga de `AddApiAuthorization` que realiza una acción para configurar las opciones.</span><span class="sxs-lookup"><span data-stu-id="3ac02-251">You can also configure the clients and resources through code using an overload of `AddApiAuthorization` that takes an action to configure options.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="3ac02-252">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="3ac02-252">Additional resources</span></span>

* <xref:spa/angular>
* <xref:spa/react>
