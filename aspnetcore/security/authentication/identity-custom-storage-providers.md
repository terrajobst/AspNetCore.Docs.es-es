---
title: Proveedores de almacenamiento personalizados para ASP.NET Core Identity
author: ardalis
description: Obtenga información sobre cómo configurar los proveedores de almacenamiento personalizados para ASP.NET Core Identity.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/authentication/identity-custom-storage-providers
ms.openlocfilehash: b10731261ca0c748548fcba94a229ba055d46eb5
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090841"
---
# <a name="custom-storage-providers-for-aspnet-core-identity"></a>Proveedores de almacenamiento personalizados para ASP.NET Core Identity

Por [Steve Smith](https://ardalis.com/)

ASP.NET Core Identity es un sistema extensible que le permite crear un proveedor de almacenamiento personalizado y conectarlo a la aplicación. Este tema describe cómo crear un proveedor de almacenamiento personalizados para ASP.NET Core Identity. Se tratan los conceptos importantes para crear su propio proveedor de almacenamiento, pero no es un tutorial paso a paso.

[Vea o descargue el ejemplo de GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample).

## <a name="introduction"></a>Introducción

De forma predeterminada, el sistema de ASP.NET Core Identity almacena información de usuario en una base de datos de SQL Server mediante Entity Framework Core. Para muchas aplicaciones, este enfoque funciona bien. Sin embargo, es preferible utilizar un mecanismo de persistencia diferente o un esquema de datos. Por ejemplo:

* Usa [Azure Table Storage](/azure/storage/) u otro almacén de datos.
* Las tablas de base de datos tienen una estructura diferente. 
* Puede que desee utilizar un enfoque de acceso a datos diferentes, como [Dapper](https://github.com/StackExchange/Dapper). 

En cada uno de estos casos, puede escribir un proveedor personalizado para su mecanismo de almacenamiento y conecte dicho proveedor en la aplicación.

ASP.NET Core Identity se incluye en las plantillas de proyecto en Visual Studio con la opción "Cuentas de usuario individuales".

Si utiliza la CLI de .NET Core, agregue `-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
```

## <a name="the-aspnet-core-identity-architecture"></a>La arquitectura de ASP.NET Core Identity

ASP.NET Core Identity consta de las clases que se llama a los administradores y los almacenes. *Los administradores* son clases de alto nivel que un desarrollador de aplicaciones que se usa para realizar operaciones como la creación de un usuario de identidad. *Almacenes* son clases de nivel inferior que especifican cómo se conservan las entidades, como usuarios y roles. Los almacenes siguen el patrón de repositorio y se acoplan estrechamente con el mecanismo de persistencia. Los administradores se desacoplan de los almacenes, lo que significa que puede reemplazar el mecanismo de persistencia sin cambiar el código de aplicación (excepto para la configuración).

El diagrama siguiente muestra cómo una aplicación web se interactúa con los administradores, mientras que los almacenes de interactúan con la capa de acceso a datos.

![Aplicaciones de ASP.NET Core trabaje con los administradores (por ejemplo, 'UserManager', 'RoleManager'). Los administradores trabajan con los almacenes (por ejemplo, ' UserStore') que se comunican con un origen de datos mediante una biblioteca como Entity Framework Core.](identity-custom-storage-providers/_static/identity-architecture-diagram.png)

Para crear un proveedor de almacenamiento personalizado, cree el origen de datos, la capa de acceso a datos y las clases de almacén que interactúan con esta capa de acceso a datos (los cuadros verdes y gris en el diagrama anterior). No es necesario personalizar los administradores o el código de aplicación que interactúa con ellos (los cuadros azules anteriores).

Al crear una nueva instancia de `UserManager` o `RoleManager` proporcionar el tipo de la clase de usuario y pasar una instancia de la clase de almacenamiento como un argumento. Este enfoque le permite conectar sus clases personalizadas en ASP.NET Core. 

[Volver a configurar la aplicación para usar el nuevo proveedor de almacenamiento](#reconfigure-app-to-use-a-new-storage-provider) se muestra cómo crear una instancia de `UserManager` y `RoleManager` con un almacén personalizado.

## <a name="aspnet-core-identity-stores-data-types"></a>ASP.NET Core Identity almacena tipos de datos

[ASP.NET Core Identity](https://github.com/aspnet/identity) tipos de datos que se detallan en las secciones siguientes:

### <a name="users"></a>Usuarios

Usuarios registrados de su sitio web. El [IdentityUser](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuser) tipo puede ampliada o se utiliza como ejemplo para su propio tipo personalizado. No necesita heredar de un tipo determinado para implementar su propia solución de almacenamiento de información de identidad personalizada.

### <a name="user-claims"></a>Notificaciones de usuario

Un conjunto de instrucciones (o [notificaciones](/dotnet/api/system.security.claims.claim)) sobre el usuario que representan la identidad del usuario. Puede permitir que una expresión mayor de la identidad del usuario que se puede lograr a través de roles.

### <a name="user-logins"></a>Inicios de sesión de usuario

Información sobre el proveedor de autenticación externos (como Facebook o una cuenta de Microsoft) que se usará al iniciar sesión en un usuario. [Ejemplo](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuserlogin)

### <a name="roles"></a>Roles

Grupos de autorización para su sitio. Incluye el nombre de identificador y el rol del rol (por ejemplo, "Admin" o "Employee"). [Ejemplo](/dotnet/api/microsoft.aspnet.identity.corecompat.identityrole)

## <a name="the-data-access-layer"></a>La capa de acceso a datos

En este tema se da por supuesto que está familiarizado con el mecanismo de persistencia que se va a usar y cómo crear entidades para dicho mecanismo. En este tema no proporciona detalles sobre cómo crear los repositorios o las clases de acceso a datos; Cuando se trabaja con ASP.NET Core Identity proporciona algunas sugerencias acerca de las decisiones de diseño.

Tiene mucha libertad al diseñar la capa de acceso a datos para un proveedor de almacén personalizado. Solo necesita crear mecanismos de persistencia para las características que piensa usar en su aplicación. Por ejemplo, si no usa roles en su aplicación, no es necesario crear almacenamiento para roles o las asociaciones de rol de usuario. La tecnología y la infraestructura existente pueden requerir una estructura que es muy diferente de la implementación predeterminada de ASP.NET Core Identity. En la capa de acceso a datos, proporcionar la lógica para que funcione con la estructura de su implementación de almacenamiento.

La capa de acceso a datos proporciona la lógica para guardar los datos de ASP.NET Core Identity a un origen de datos. La capa de acceso a datos para su proveedor de almacenamiento personalizado puede incluir las siguientes clases para almacenar la información de usuario y el rol.

### <a name="context-class"></a>Context (clase)

Encapsula la información para conectarse a su mecanismo de persistencia y ejecutar consultas. Varias clases de datos requieren una instancia de esta clase, que normalmente se suministran mediante la inserción de dependencias. [Ejemplo](/dotnet/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1).

### <a name="user-storage"></a>Almacenamiento de información de usuario

Almacena y recupera información de usuario (por ejemplo, hash de nombre y la contraseña de usuario). [Ejemplo](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="role-storage"></a>Almacenamiento de rol

Almacena y recupera información de funciones (por ejemplo, el nombre del rol). [Ejemplo](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)

### <a name="userclaims-storage"></a>Almacenamiento de UserClaims

Almacena y recupera información de notificaciones de usuario (por ejemplo, el tipo de notificación y el valor). [Ejemplo](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userlogins-storage"></a>Almacenamiento de UserLogins

Almacena y recupera información de inicio de sesión de usuario (por ejemplo, un proveedor de autenticación externo). [Ejemplo](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userrole-storage"></a>Almacenamiento de UserRole

Almacena y recupera los roles asignados a los usuarios. [Ejemplo](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

**Sugerencia:** implementar sólo las clases que va a usar en la aplicación.

En las clases de acceso a datos, proporcionar código para realizar operaciones de datos para el mecanismo de persistencia. Por ejemplo, dentro de un proveedor personalizado, podría tener el código siguiente para crear un nuevo usuario en el *almacenar* clase:

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs?name=createuser&highlight=7)]

La lógica de implementación para crear el usuario está en el `_usersTable.CreateAsync` método, se muestra a continuación.

## <a name="customize-the-user-class"></a>Personalizar la clase de usuario

Al implementar un proveedor de almacenamiento, cree una clase de usuario que es equivalente a la [clase IdentityUser](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuser).

Como mínimo, la clase de usuario debe incluir un `Id` y un `UserName` propiedad.

El `IdentityUser` clase define las propiedades que el `UserManager` llamadas al realizar las operaciones solicitadas. El tipo predeterminado de la `Id` propiedad es una cadena, pero puede heredar de `IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>` y especifique un tipo diferente. El marco de trabajo espera que la implementación de almacenamiento para controlar las conversiones de tipos de datos.

## <a name="customize-the-user-store"></a>Personalizar el almacén de usuario

Crear un `UserStore` clase que proporciona los métodos para todas las operaciones de datos en el usuario. Esta clase es equivalente a la [UserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1) clase. En su `UserStore` clase, implemente `IUserStore<TUser>` y las interfaces opcionales requeridas. Seleccione qué interfaces opcionales para la implementación en función de la funcionalidad ofrecida en la aplicación.

### <a name="optional-interfaces"></a>Interfaces opcionales

* [IUserRoleStore](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1)
* [IUserClaimStore](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1)
* [IUserPasswordStore](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1)
* [IUserSecurityStampStore](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1)
* [IUserEmailStore](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1)
* [IPhoneNumberStore](/dotnet/api/microsoft.aspnetcore.identity.iphonenumberstore-1)
* [IQueryableUserStore](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1)
* [IUserLoginStore](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1)
* [IUserTwoFactorStore](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1)
* [IUserLockoutStore](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1)

Las interfaces opcionales que se heredan de `IUserStore<TUser>`. Puede ver un usuario de ejemplo implementada parcialmente almacenar en el [aplicación de ejemplo](https://github.com/aspnet/Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs).

Dentro de la `UserStore` (clase), utilice las clases de acceso de datos que ha creado para llevar a cabo las operaciones. Estos se pasan mediante la inserción de dependencia. Por ejemplo, en el servidor SQL Server con la implementación de Dapper, el `UserStore` clase tiene el `CreateAsync` método que utiliza una instancia de `DapperUsersTable` para insertar un nuevo registro:

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/DapperUsersTable.cs?name=createuser&highlight=7)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Interfaces para implementar al personalizar el almacén de usuario

* **IUserStore**  
 El [IUserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserstore-1) es la única interfaz debe implementar en el almacén del usuario. Define métodos para crear, actualizar, eliminar y recuperar los usuarios.
* **IUserClaimStore**  
 El [IUserClaimStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1) interfaz define los métodos que se implementan para habilitar las notificaciones de usuario. Contiene métodos para agregar, quitar y recuperar las notificaciones de usuario.
* **IUserLoginStore**  
 El [IUserLoginStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1) define los métodos que se implementan para habilitar los proveedores de autenticación externos. Contiene métodos para agregar, quitar y recuperar los inicios de sesión de usuario y un método para recuperar un usuario basado en la información de inicio de sesión.
* **IUserRoleStore**  
 El [IUserRoleStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1) interfaz define los métodos que implementa para asignar un usuario a un rol. Contiene métodos para agregar, quitar y recuperar roles de un usuario y un método para comprobar si un usuario está asignado a un rol.
* **IUserPasswordStore**  
 El [IUserPasswordStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1) interfaz define los métodos que implementa para conservar las contraseñas con hash. Contiene métodos para obtener y establecer la contraseña con algoritmo hash y un método que indica si el usuario ha establecido una contraseña.
* **IUserSecurityStampStore**  
 El [IUserSecurityStampStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1) interfaz define los métodos que se implementan para usar una marca de seguridad para que indica si ha cambiado la información de la cuenta del usuario. Esta marca se actualiza cuando un usuario cambia la contraseña, o agrega o quita los inicios de sesión. Contiene métodos para obtener y establecer la marca de seguridad.
* **IUserTwoFactorStore**  
 El [IUserTwoFactorStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1) interfaz define los métodos que implemente para admitir la autenticación en dos fases. Contiene métodos para obtener y establecer si está habilitada la autenticación en dos fases para un usuario.
* **IUserPhoneNumberStore**  
 El [IUserPhoneNumberStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1) interfaz define los métodos que implementa para almacenar los números de teléfono del usuario. Contiene métodos para obtener y establecer el número de teléfono y si se ha confirmado el número de teléfono.
* **IUserEmailStore**  
 El [IUserEmailStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1) interfaz define los métodos que implementa para almacenar direcciones de correo electrónico del usuario. Contiene métodos para obtener y establecer la dirección de correo electrónico y si se ha confirmado el correo electrónico.
* **IUserLockoutStore**  
 El [IUserLockoutStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1) interfaz define los métodos que implementa para almacenar información acerca del bloqueo de una cuenta. Contiene métodos para realizar el seguimiento de intentos de acceso incorrectos y bloqueos.
* **IQueryableUserStore**  
 El [IQueryableUserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1) interfaz define los miembros que implementa para proporcionar un almacén de usuarios consultable.

Implementar sólo las interfaces que son necesarios en su aplicación. Por ejemplo:

```csharp
public class UserStore : IUserStore<IdentityUser>,
                         IUserClaimStore<IdentityUser>,
                         IUserLoginStore<IdentityUser>,
                         IUserRoleStore<IdentityUser>,
                         IUserPasswordStore<IdentityUser>,
                         IUserSecurityStampStore<IdentityUser>
{
    // interface implementations not shown
}
```

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim IdentityUserLogin y IdentityUserRole

El `Microsoft.AspNet.Identity.EntityFramework` espacio de nombres contiene las implementaciones de la [IdentityUserClaim](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1), [IdentityUserLogin](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuserlogin), y [IdentityUserRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1) clases. Si está usando estas características, es posible que desee crear sus propias versiones de estas clases y definir las propiedades de la aplicación. Sin embargo, a veces es más eficaz para no cargar estas entidades en memoria al realizar operaciones básicas (como agregar o quitar notificaciones de un usuario). En su lugar, las clases de almacén de back-end pueden ejecutar estas operaciones directamente en el origen de datos. Por ejemplo, el `UserStore.GetClaimsAsync` puede llamar al método el `userClaimTable.FindByUserId(user.Id)` método para ejecutar una consulta en que directamente de la tabla y devuelve una lista de notificaciones.

## <a name="customize-the-role-class"></a>Personalizar la clase de función

Al implementar un proveedor de almacenamiento de información de roles, puede crear un tipo de rol personalizado. No tiene que implementar una interfaz determinada, pero debe tener un `Id` y normalmente tendrán un `Name` propiedad.

La siguiente es una clase de función de ejemplo:

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/ApplicationRole.cs)]

## <a name="customize-the-role-store"></a>Personalizar el almacén de roles

Puede crear un `RoleStore` clase que proporciona los métodos para todas las operaciones de datos en roles. Esta clase es equivalente a la [RoleStore&lt;; TRole&gt; ](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1) clase. En el `RoleStore` (clase), implementa la `IRoleStore<TRole>` y, opcionalmente, el `IQueryableRoleStore<TRole>` interfaz.

* **IRoleStore&lt;; TRole&gt;**  
 El [IRoleStore&lt;; TRole&gt; ](/dotnet/api/microsoft.aspnetcore.identity.irolestore-1) interfaz define los métodos para implementar en la clase de almacén de rol. Contiene métodos para crear, actualizar, eliminar y recuperar roles.
* **RoleStore&lt;; TRole&gt;**  
 Para personalizar `RoleStore`, cree una clase que implementa el `IRoleStore<TRole>` interfaz. 

## <a name="reconfigure-app-to-use-a-new-storage-provider"></a>Volver a configurar la aplicación para que use un nuevo proveedor de almacenamiento

Una vez que ha implementado un proveedor de almacenamiento, configure la aplicación para que lo utilicen. Si la aplicación utiliza el proveedor predeterminado, reemplácelo por el proveedor personalizado.

1. Quitar el `Microsoft.AspNetCore.EntityFramework.Identity` paquete NuGet.
1. Si el proveedor de almacenamiento se encuentra en un proyecto independiente o un paquete, agregue una referencia a él.
1. Reemplace todas las referencias a `Microsoft.AspNetCore.EntityFramework.Identity` con el uso de una instrucción para el espacio de nombres de su proveedor de almacenamiento.
1. En el `ConfigureServices` método, cambio el `AddIdentity` método para usar sus tipos personalizados. Puede crear sus propios métodos de extensión para este propósito. Consulte [IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs) para obtener un ejemplo.
1. Si está utilizando Roles, actualice el `RoleManager` para usar su `RoleStore` clase.
1. Actualice la cadena de conexión y las credenciales para la configuración de la aplicación.

Ejemplo:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add identity types
    services.AddIdentity<ApplicationUser, ApplicationRole>()
        .AddDefaultTokenProviders();

    // Identity Services
    services.AddTransient<IUserStore<ApplicationUser>, CustomUserStore>();
    services.AddTransient<IRoleStore<ApplicationRole>, CustomRoleStore>();
    string connectionString = Configuration.GetConnectionString("DefaultConnection");
    services.AddTransient<SqlConnection>(e => new SqlConnection(connectionString));
    services.AddTransient<DapperUsersTable>();

    // additional configuration
}
```

## <a name="references"></a>Referencias

* [Proveedores de almacenamiento personalizados para la identidad ASP.NET 4.x](/aspnet/identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity)
* [ASP.NET Core Identity](https://github.com/aspnet/identity) &ndash; este repositorio incluye vínculos a la Comunidad que mantiene los proveedores de almacenes.
