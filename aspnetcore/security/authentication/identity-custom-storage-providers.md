---
title: Proveedores de almacenamiento personalizados para ASP.NET Core Identity | Documentos de Microsoft
author: ardalis
description: "Cómo configurar los proveedores de almacenamiento personalizados de ASP.NET Core Identity."
ms.author: riande
manager: wpickett
ms.date: 05/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-custom-storage-providers
ms.openlocfilehash: f0953ad5d9f1bfa92ecc5169d9a211ce6b8cda8f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="custom-storage-providers-for-aspnet-core-identity"></a>Proveedores de almacenamiento personalizados para ASP.NET Core Identity

Por [Steve Smith](https://ardalis.com/)

Identidad de ASP.NET Core es un sistema extensible que permite crear un proveedor de almacenamiento personalizado y conectarlo a la aplicación. En este tema se describe cómo crear un proveedor de almacenamiento personalizado de ASP.NET Core Identity. Explica los conceptos importantes para crear su propio proveedor de almacenamiento, pero no es un tutorial paso a paso.

[Ver o descargar el ejemplo desde GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample).

## <a name="introduction"></a>Introducción

De forma predeterminada, el sistema de identidades de ASP.NET Core almacena información de usuario en una base de datos de SQL Server con Entity Framework Core. Para muchas aplicaciones, este enfoque funciona bien. Sin embargo, es preferible utilizar un mecanismo de persistencia diferentes o el esquema de datos. Por ejemplo:

* Usa [almacenamiento de tablas Azure](https://docs.microsoft.com/azure/storage/) o en otro almacén de datos.
* Las tablas de base de datos tienen una estructura distinta. 
* Puede que desee utilizar un enfoque de acceso a datos diferentes, como [Dapper](https://github.com/StackExchange/Dapper). 

En cada uno de estos casos, puede escribir un proveedor personalizado para su mecanismo de almacenamiento y conecte dicho proveedor de la aplicación.

Identidad de ASP.NET Core se incluye en las plantillas de proyecto en Visual Studio con la opción de "Cuentas de usuario individuales".

Cuando se usa la CLI de núcleo. NET, agregue `-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
```

## <a name="the-aspnet-core-identity-architecture"></a>La arquitectura de ASP.NET Core Identity

ASP.NET Core Identity consta de las clases denominadas administradores y almacenes. *Administradores de* son clases de alto nivel que un desarrollador de aplicaciones que se utiliza para realizar operaciones, como la creación de un usuario de identidad. *Almacenes de* son clases de nivel inferior que especifican cómo se conservan las entidades, como los roles y los usuarios. Almacenes de seguir la [modelo de repositorio](http://deviq.com/repository-pattern/) y se acoplan estrechamente con el mecanismo de persistencia. Los administradores se desacoplan de almacenes, lo que significa que puede reemplazar el mecanismo de persistencia sin cambiar el código de aplicación (excepto la configuración).

El diagrama siguiente muestra cómo una aplicación web interactúa con los administradores, mientras que los almacenes interactúan con la capa de acceso a datos.

![Aplicaciones de ASP.NET Core funcionan con los administradores (por ejemplo, 'UserManager', 'RoleManager'). Los administradores trabajan con almacenes (por ejemplo, ' UserStore') que se comunican con un origen de datos mediante una biblioteca como Entity Framework Core.](identity-custom-storage-providers/_static/identity-architecture-diagram.png)

Para crear un proveedor de almacenamiento personalizado, cree el origen de datos, la capa de acceso a datos y las clases de almacén que interactúan con este nivel de acceso de datos (los cuadros de color verde y gris en el diagrama anterior). No es necesario personalizar los administradores o su código de aplicación que interactúa con ellos (los cuadros azules anteriores).

Cuando se crea una nueva instancia de `UserManager` o `RoleManager` proporciona el tipo de la clase de usuario y pasar una instancia de la clase de almacén como un argumento. Este enfoque permite conectar las clases personalizadas en ASP.NET Core. 

[Volver a configurar la aplicación para que use el nuevo proveedor de almacenamiento](#reconfigure-app-to-use-new-storage-provider) muestra cómo crear una instancia `UserManager` y `RoleManager` con un almacén personalizado.

## <a name="aspnet-core-identity-stores-data-types"></a>Tipos de datos de almacenes de identidades de ASP.NET Core

[ASP.NET Core Identity](https://github.com/aspnet/identity) tipos de datos se detallan en las secciones siguientes:

### <a name="users"></a>Usuarios

Usuarios registrados de su sitio web. El [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser) tipo puede ampliada o se utiliza como ejemplo para su propio tipo personalizado. No es necesario heredar de un tipo determinado para implementar su propia solución de almacenamiento de información de identidad personalizada.

### <a name="user-claims"></a>Notificaciones de usuario

Un conjunto de instrucciones (o [notificaciones](https://docs.microsoft.com//dotnet/api/system.security.claims.claim)) acerca del usuario que representan la identidad del usuario. Puede habilitar la expresión mayor de la identidad del usuario que se puede lograr a través de roles.

### <a name="user-logins"></a>Inicios de sesión de usuario

Información sobre el proveedor de autenticación externo (por ejemplo, Facebook o una cuenta de Microsoft) que se usará cuando un usuario de inicio de sesión. [Ejemplo](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin)

### <a name="roles"></a>Roles

Grupos de autorización para el sitio. Incluye el nombre de identificador y el rol del rol (por ejemplo, "Admin" o "Employee"). [Ejemplo](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityrole)

## <a name="the-data-access-layer"></a>La capa de acceso a datos

En este tema se da por supuesto que está familiarizado con el mecanismo de persistencia que se va a usar y cómo crear entidades para dicho mecanismo. En este tema no proporciona detalles acerca de cómo crear los repositorios o clases de acceso a datos; proporciona algunas sugerencias acerca de las decisiones de diseño cuando se trabaja con la identidad de núcleo de ASP.NET.

Tiene mucha libertad al diseñar la capa de acceso a datos para un proveedor de almacén personalizado. Basta con crear mecanismos de persistencia para las características que desee usar en la aplicación. Por ejemplo, si no se usan funciones en la aplicación, no es necesario crear un almacenamiento para roles o las asociaciones de rol de usuario. La tecnología y la infraestructura existente pueden requerir una estructura que es muy diferente de la implementación predeterminada de ASP.NET Core Identity. En la capa de acceso a datos, proporcionar la lógica para trabajar con la estructura de su implementación de almacenamiento.

La capa de acceso a datos proporciona la lógica para guardar los datos de identidad de núcleo de ASP.NET en un origen de datos. La capa de acceso a datos para su proveedor de almacenamiento personalizado podría incluir las siguientes clases para almacenar la información de usuario y el rol.

### <a name="context-class"></a>Context (clase)

Encapsula la información para conectarse a su mecanismo de persistencia y ejecutar consultas. Varias clases de datos requieren una instancia de esta clase, por lo general se proporcionan a través de inserción de dependencias. [En el ejemplo se](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1).

### <a name="user-storage"></a>Almacenamiento de información de usuario

Almacena y recupera información de usuario (por ejemplo, el hash de nombre y la contraseña de usuario). [Ejemplo](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="role-storage"></a>Almacenamiento de rol

Almacena y recupera información de funciones (por ejemplo, el nombre de rol). [Ejemplo](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)

### <a name="userclaims-storage"></a>Almacenamiento de UserClaims

Almacena y recupera información de notificaciones de usuario (por ejemplo, el tipo de notificación y el valor). [Ejemplo](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userlogins-storage"></a>Almacenamiento de UserLogins

Almacena y recupera información de inicio de sesión de usuario (por ejemplo, un proveedor de autenticación externo). [Ejemplo](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userrole-storage"></a>Almacenamiento de UserRole

Almacena y recupera los roles asignados a los usuarios. [Ejemplo](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

**Sugerencia:** implementar solo las clases que va a usar en la aplicación.

En las clases de acceso a datos, proporcionar código para realizar operaciones de datos para el mecanismo de persistencia. Por ejemplo, dentro de un proveedor personalizado, podría tener el código siguiente para crear un nuevo usuario en el *almacenar* clase:

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs?name=createuser&highlight=7)]

La lógica de implementación para crear el usuario está en el ``_usersTable.CreateAsync`` método, se muestra a continuación.

## <a name="customize-the-user-class"></a>Personalizar la clase de usuario

Al implementar un proveedor de almacenamiento, cree una clase de usuario que es equivalente a la [ `IdentityUser` clase](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser).

Como mínimo, debe incluir la clase de usuario una `Id` y un `UserName` propiedad.

El `IdentityUser` clase define las propiedades que el ``UserManager`` llamadas al realizar operaciones solicitadas. El tipo predeterminado de la `Id` propiedad es una cadena, pero se puede heredar de `IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>` y especifique un tipo diferente. El marco de trabajo espera que la implementación de almacenamiento para controlar las conversiones de tipo de datos.

## <a name="customize-the-user-store"></a>Personalizar el almacén del usuario

Crear un `UserStore` clase que proporciona los métodos para todas las operaciones de datos en el usuario. Esta clase es equivalente a la [UserStore<TUser> ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1) clase. En su `UserStore` clase, implemente `IUserStore<TUser>` y las interfaces opcionales requeridas. Seleccione qué interfaces opcionales que se implementarán en función de la funcionalidad de la aplicación.

### <a name="optional-interfaces"></a>Interfaces opcionales

- IUserRoleStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserrolestore-1
- IUserClaimStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserclaimstore-1
- IUserPasswordStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserpasswordstore-1
- IUserSecurityStampStore<!-- make these all links and remove / -->
- IUserEmailStore
- IPhoneNumberStore
- IQueryableUserStore
- IUserLoginStore
- IUserTwoFactorStore
- IUserLockoutStore

Las interfaces opcionales heredan de `IUserStore`. Puede ver un usuario de ejemplo parcialmente implementado almacenar [aquí](https://github.com/aspnet/Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs).

Dentro de la `UserStore` (clase), se utilizan las clases de acceso de datos que ha creado para realizar operaciones. Estos se pasan mediante la inserción de dependencia. Por ejemplo, en el servidor SQL Server con la implementación Dapper, la `UserStore` clase tiene la `CreateAsync` método que usa una instancia de `DapperUsersTable` para insertar un nuevo registro:

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/DapperUsersTable.cs?name=createuser&highlight=7)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Interfaces para implementar al personalizar el almacén de usuario

- **IUserStore**  
 El [IUserStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserstore-1) interfaz es la única interfaz que debe implementar en el almacén del usuario. Define métodos para crear, actualizar, eliminar y recuperar los usuarios.
- **IUserClaimStore**  
 El [IUserClaimStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserclaimstore-1) interfaz define los métodos que se implementan para habilitar notificaciones de usuario. Contiene métodos para agregar, quitar y recuperar notificaciones de usuario.
- **IUserLoginStore**  
 El [IUserLoginStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserloginstore-1) define los métodos que se implementan para permitir a los proveedores de autenticación externo. Contiene métodos para agregar, quitar y recuperar los inicios de sesión de usuario y un método para recuperar un usuario basándose en la información de inicio de sesión.
- **IUserRoleStore**  
 El [IUserRoleStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserrolestore-1) interfaz define los métodos que se implementan para asignar un usuario a un rol. Contiene métodos para agregar, quitar y recuperar los roles de usuario y un método para comprobar si un usuario está asignado a un rol.
- **IUserPasswordStore**  
 El [IUserPasswordStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserpasswordstore-1) interfaz define los métodos que se implementan para almacenar contraseñas con algoritmo hash. Contiene métodos para obtener y establecer la contraseña con hash y un método que indica si el usuario ha establecido una contraseña.
- **IUserSecurityStampStore**  
 El [IUserSecurityStampStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1) interfaz define los métodos que se implementan para usar una marca de seguridad para que indica si ha cambiado la información de la cuenta del usuario. Esta marca se actualiza cuando un usuario cambia la contraseña, o agregue o quite inicios de sesión. Contiene métodos para obtener y establecer la marca de seguridad.
- **IUserTwoFactorStore**  
 El [IUserTwoFactorStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iusertwofactorstore-1) interfaz define los métodos que se implementan para admitir la autenticación en dos fases. Contiene métodos para obtener y establecer si está habilitada la autenticación en dos fases para un usuario.
- **IUserPhoneNumberStore**  
 El [IUserPhoneNumberStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1) interfaz define los métodos que implementa para almacenar números de teléfono del usuario. Contiene métodos para obtener y establecer el número de teléfono y si se ha confirmado el número de teléfono.
- **IUserEmailStore**  
 El [IUserEmailStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuseremailstore-1) interfaz define los métodos que implementa para almacenar direcciones de correo electrónico del usuario. Contiene métodos para obtener y establecer la dirección de correo electrónico y si se ha confirmado el correo electrónico.
- **IUserLockoutStore**  
 El [IUserLockoutStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserlockoutstore-1) interfaz define los métodos que se implementan para almacenar información acerca de los bloqueos de una cuenta. Contiene métodos para realizar el seguimiento de intentos de acceso erróneos y bloqueos.
- **IQueryableUserStore**  
 El [IQueryableUserStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iqueryableuserstore-1) interfaz define el implementan miembros para proporcionar un almacén de usuarios consultable.

Implementar sólo las interfaces que son necesarios en la aplicación. Por ejemplo:

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

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim, IdentityUserLogin y IdentityUserRole

El ``Microsoft.AspNet.Identity.EntityFramework`` espacio de nombres contiene las implementaciones de la [IdentityUserClaim](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1), [IdentityUserLogin](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin), y [IdentityUserRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1) clases. Si está usando estas características, puede crear sus propias versiones de estas clases y definir las propiedades de la aplicación. Sin embargo, a veces resulta más eficaz para no cargar estas entidades en memoria al realizar operaciones básicas (como agregar o quitar la notificación del usuario). En su lugar, las clases de almacenamiento back-end pueden ejecutar estas operaciones directamente en el origen de datos. Por ejemplo, el ``UserStore.GetClaimsAsync`` método puede llamar a la ``userClaimTable.FindByUserId(user.Id)`` método para ejecutar una consulta en que la tabla directamente y devolver una lista de notificaciones.

## <a name="customize-the-role-class"></a>Personalizar el role (clase)

Al implementar un proveedor de almacenamiento de rol, puede crear un tipo de función personalizado. No necesita implementar una interfaz concreta, pero debe tener un `Id` y normalmente tendrá un `Name` propiedad.

La siguiente es una clase de rol de ejemplo:

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/ApplicationRole.cs)]

## <a name="customize-the-role-store"></a>Personalizar el almacén de roles

Puede crear un ``RoleStore`` clase que proporciona los métodos para todas las operaciones de datos en los roles. Esta clase es equivalente a la [RoleStore<TRole> ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1) clase. En el `RoleStore` (clase), implementar la ``IRoleStore<TRole>`` y, opcionalmente, la ``IQueryableRoleStore<TRole>`` interfaz.

- **IRoleStore&lt;TRole&gt;**  
 El [IRoleStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.irolestore-1) interfaz define los métodos que se implementan en la clase de almacén de rol. Contiene métodos para crear, actualizar, eliminar y recuperar roles.
- **RoleStore&lt;TRole&gt;**  
 Personalizar `RoleStore`, cree una clase que implementa el `IRoleStore` interfaz. 

## <a name="reconfigure-app-to-use-new-storage-provider"></a>Volver a configurar la aplicación para que use el nuevo proveedor de almacenamiento

Una vez que ha implementado un proveedor de almacenamiento, configure la aplicación para que lo utilice. Si la aplicación utiliza el proveedor predeterminado, reemplácelo por el proveedor personalizado.

1. Quitar el `Microsoft.AspNetCore.EntityFramework.Identity` paquete NuGet.
1. Si el proveedor de almacenamiento reside en un proyecto independiente o un paquete, agregue una referencia a él.
1. Reemplace todas las referencias a `Microsoft.AspNetCore.EntityFramework.Identity` con el uso de una instrucción para el espacio de nombres del proveedor de almacenamiento.
1. En el ``ConfigureServices`` (método), cambiar la `AddIdentity` método usar sus tipos personalizados. Puede crear sus propios métodos de extensión para este propósito. Vea [IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs) para obtener un ejemplo.
1. Si está utilizando Roles, actualice el `RoleManager` usar su `RoleStore` clase.
1. Actualizar la cadena de conexión y las credenciales para la configuración de la aplicación.

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

- [Proveedores de almacenamiento personalizado para identidades de ASP.NET](https://docs.microsoft.com/aspnet/identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity)
- [ASP.NET Core Identity](https://github.com/aspnet/identity) -este repositorio incluye vínculos a la Comunidad mantenida los proveedores de almacenes.
