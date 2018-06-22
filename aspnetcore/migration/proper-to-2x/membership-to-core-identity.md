---
title: Migrar de autenticación de pertenencia a ASP.NET a ASP.NET Core 2.0 Identity
author: isaac2004
description: Obtenga información acerca de cómo migrar las aplicaciones ASP.NET existentes mediante la autenticación de pertenencia a ASP.NET Core 2.0 Identity.
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 3ec22713997a74b587ef5d18e71a28668a5481e2
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274110"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a>Migrar de autenticación de pertenencia a ASP.NET a ASP.NET Core 2.0 Identity

Por [Isaac Levin](https://isaaclevin.com)

Este artículo muestra cómo migrar el esquema de base de datos para las aplicaciones ASP.NET mediante la autenticación de pertenencia a ASP.NET Core 2.0 Identity.

> [!NOTE]
> Este documento proporciona los pasos necesarios para migrar el esquema de base de datos para aplicaciones basadas en la pertenencia a ASP.NET para el esquema de base de datos usado para identidad de núcleo de ASP.NET. Para obtener más información sobre cómo migrar de autenticación basada en pertenencia a ASP.NET a ASP.NET Identity, consulte [migrar una aplicación existente de SQL pertenencia a ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity). Para obtener más información acerca de la identidad de núcleo de ASP.NET, vea [Introducción a la identidad en ASP.NET Core](xref:security/authentication/identity).

## <a name="review-of-membership-schema"></a>Revisión del esquema de pertenencia

Antes de ASP.NET 2.0, los desarrolladores se encargan que cree el proceso de autenticación y autorización completo para sus aplicaciones. Con ASP.NET 2.0, se introdujo pertenencia, proporcionar una solución reutilizable para administrar la seguridad en las aplicaciones ASP.NET. Los desarrolladores ahora podían arrancar un esquema en una base de datos de SQL Server con el [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) comando. Después de ejecutar este comando, en las siguientes tablas se crearon en la base de datos.

  ![Tablas de pertenencia](identity/_static/membership-tables.png)

Para migrar aplicaciones existentes a ASP.NET Core 2.0 Identity, los datos en estas tablas tienen que migrarse a las tablas utilizadas por el nuevo esquema de identidad.

## <a name="aspnet-core-identity-20-schema"></a>Esquema de núcleo identidad 2.0 de ASP.NET

Núcleo de ASP.NET 2.0 sigue la [identidad](/aspnet/identity/index) principio introducida en ASP.NET 4.5. Aunque se comparte el principio, la implementación entre los marcos es diferente, incluso entre las versiones de ASP.NET Core (vea [migrar autenticación e identidad a ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).

La manera más rápida de ver el esquema de núcleo de ASP.NET 2.0 Identity consiste en crear una nueva aplicación de ASP.NET Core 2.0. Siga estos pasos en Visual Studio de 2017:

* Seleccione **Archivo** > **Nuevo** > **Proyecto**.
* Crear un nuevo **aplicación Web de ASP.NET Core**y el nombre del proyecto *CoreIdentitySample*.
* Seleccione **ASP.NET Core 2.0** en la lista desplegable y, luego, seleccione **Aplicación web**. Esta plantilla genera un [páginas de Razor](xref:razor-pages/index) aplicación. Antes de hacer clic **Aceptar**, haga clic en **Cambiar autenticación**.
* Elija **cuentas de usuario individuales** para las plantillas de identidad. Por último, haga clic en **Aceptar**, a continuación, **Aceptar**. Visual Studio crea un proyecto mediante la plantilla de ASP.NET Core Identity.

Identidad de núcleo de ASP.NET 2.0 utiliza [Entity Framework Core](/ef/core) para interactuar con la base de datos almacena los datos de autenticación. En orden de la aplicación recién creada para que funcione, debe ser una base de datos para almacenar estos datos. Después de crear una nueva aplicación, la manera más rápida para inspeccionar el esquema en un entorno de base de datos es crear la base de datos usando migraciones de Entity Framework. Este proceso crea una base de datos, ya sea localmente o en otro lugar, que imita ese esquema. Revise la documentación anterior para obtener más información.

Para crear una base de datos con el esquema de la identidad de ASP.NET Core, ejecute el `Update-Database` en Visual Studio **Package Manager Console** ventana (PMC)&mdash;se encuentra en **herramientas**  >  **Administrador de paquetes de NuGet** > **consola de administrador de paquetes**. PMC admite la ejecución de los comandos de Entity Framework.

Comandos de Entity Framework usan la cadena de conexión para la base de datos especificada en *appSettings.JSON que se*. La siguiente cadena de conexión tiene como destino una base de datos en *localhost* denominado *asp-net-core-identity*. En esta configuración, Entity Framework está configurado para usar el `DefaultConnection` cadena de conexión.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

Este comando crea la base de datos especificada con el esquema y los datos necesarios para la inicialización de la aplicación. La siguiente imagen muestra la estructura de tabla que se crea con los pasos anteriores.

   ![Tablas de identidad](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a>migrar el esquema

Existen diferencias sutiles en los campos para la suscripción y ASP.NET Core Identity y estructuras de tabla. El modelo ha cambiado considerablemente para la autenticación/autorización con aplicaciones ASP.NET y ASP.NET Core. Los objetos de clave que todavía se utilizan con identidad están *usuarios* y *Roles*. Estas son las tablas de asignación de *usuarios*, *Roles*, y *UserRoles*.

### <a name="users"></a>Usuarios

| *Identity(AspNetUsers)* |   | *Membership(aspnet_Users/aspnet_Membership)* ||
| --- | --- | --- | --- | --- | --- |
| **Nombre de campo** | **Type**  |   **Nombre de campo** | **Type**  |
|`Id` | cadena | `aspnet_Users.UserId` | cadena
|`UserName` | cadena | `aspnet_Users.UserName` | cadena
|`Email` | cadena | `aspnet_Membership.Email` | cadena
|`NormalizedUserName` | cadena | `aspnet_Users.LoweredUserName` | cadena
|`NormalizedEmail` | cadena | `aspnet_Membership.LoweredEmail` | cadena
|`PhoneNumber` | cadena | `aspnet_Users.MobileAlias` | cadena
|`LockoutEnabled` | bits | `aspnet_Membership.IsLockedOut` | bits

> [!NOTE]
> No todas las asignaciones de campo son similares a las relaciones uno a uno de pertenencia a ASP.NET Core Identity. La tabla anterior toma el esquema de usuario de pertenencia predeterminado y lo asigna al esquema de núcleo de ASP.NET Identity. Los campos personalizados que se utilizaron para la suscripción es necesario asignar manualmente. En esta asignación, no hay ninguna asignación para las contraseñas, como criterios de contraseña y Sales de la contraseña no se migran entre los dos. **Se recomienda dejar la contraseña como null y pedir a los usuarios restablecer sus contraseñas.** En ASP.NET Core Identity, `LockoutEnd` debe establecerse en una fecha en el futuro si el usuario está bloqueado. Esto se muestra en el script de migración.

### <a name="roles"></a>Roles

| *Identity(AspNetRoles)* |   | *Membership(aspnet_Roles)* ||
| --- | --- | --- | --- | --- | --- |
| **Nombre de campo** | **Type**  |   **Nombre de campo** | **Type**  |
|`Id` | cadena | `RoleId` | cadena
|`Name` | cadena | `RoleName` | cadena
|`NormalizedName` | cadena | `LoweredRoleName` | cadena

### <a name="user-roles"></a>Roles de usuario

| *Identity(AspNetUserRoles)* |   | *Membership(aspnet_UsersInRoles)* ||
| --- | --- | --- | --- | --- | --- |
| **Nombre de campo** | **Type**  |   **Nombre de campo** | **Type**  |
|`RoleId` | cadena | `RoleId` | cadena
|`UserId` | cadena | `UserId` | cadena

Hacer referencia a las tablas de asignación anterior al crear un script de migración para *usuarios* y *Roles*. En el siguiente ejemplo se da por supuesto que tiene dos bases de datos en un servidor de base de datos. Una base de datos contiene el esquema de pertenencia a ASP.NET existente y los datos. La otra base de datos se creó mediante los pasos descritos anteriormente. Los comentarios son incluido en línea para obtener más detalles.

```sql
-- THIS SCRIPT NEEDS TO RUN FROM THE CONTEXT OF THE MEMBERSHIP DB
BEGIN TRANSACTION MigrateUsersAndRoles
use aspnetdb

-- INSERT USERS
INSERT INTO coreidentity.dbo.aspnetusers
            (id,
             username,
             normalizedusername,
             passwordhash,
             securitystamp,
             emailconfirmed,
             phonenumber,
             phonenumberconfirmed,
             twofactorenabled,
             lockoutend,
             lockoutenabled,
             accessfailedcount,
             email,
             normalizedemail)
SELECT aspnet_users.userid,
       aspnet_users.username,
       aspnet_users.loweredusername,
       --Creates an empty password since passwords don't map between the two schemas
       '',
       --Security Stamp is a token used to verify the state of an account and is subject to change at any time. It should be intialized as a new ID.
       NewID(),
       --EmailConfirmed is set when a new user is created and confirmed via email. Users must have this set during migration to ensure they're able to reset passwords.
       1,
       aspnet_users.mobilealias,
       CASE
         WHEN aspnet_Users.MobileAlias is null THEN 0
         ELSE 1
       END,
       --2-factor Auth likely wasn't setup in Membership for users, so setting as false.
       0,
       CASE
         --Setting lockout date to time in the future (1000 years)
         WHEN aspnet_membership.islockedout = 1 THEN Dateadd(year, 1000,
                                                     Sysutcdatetime())
         ELSE NULL
       END,
       aspnet_membership.islockedout,
       --AccessFailedAccount is used to track failed logins. This is stored in membership in multiple columns. Setting to 0 arbitrarily.
       0,
       aspnet_membership.email,
       aspnet_membership.loweredemail
FROM   aspnet_users
       LEFT OUTER JOIN aspnet_membership
                    ON aspnet_membership.applicationid =
                       aspnet_users.applicationid
                       AND aspnet_users.userid = aspnet_membership.userid
       LEFT OUTER JOIN coreidentity.dbo.aspnetusers
                    ON aspnet_membership.userid = aspnetusers.id
WHERE  aspnetusers.id IS NULL

-- INSERT ROLES
INSERT INTO coreIdentity.dbo.aspnetroles(id,name)
SELECT roleId,rolename
FROM aspnet_roles;

-- INSERT USER ROLES
INSERT INTO coreidentity.dbo.aspnetuserroles(userid,roleid)
SELECT userid,roleid
FROM aspnet_usersinroles;

IF @@ERROR <> 0
  BEGIN
    ROLLBACK TRANSACTION MigrateUsersAndRoles
    RETURN
  END

COMMIT TRANSACTION MigrateUsersAndRoles
```

Tras finalizar este script, la aplicación de ASP.NET Core Identity que se creó anteriormente se rellena con los usuarios de pertenencia. Los usuarios deben cambiar sus contraseñas antes de iniciar sesión.

> [!NOTE]
> Si el sistema de pertenencia tenía usuarios con nombres de usuario que no coincidía con su dirección de correo electrónico, se requiere para la aplicación que creó anteriormente para dar cabida a este cambio. La plantilla predeterminada espera `UserName` y `Email` para que coincidan. En situaciones en las que son diferentes, es necesario modificar para usar el proceso de inicio de sesión `UserName` en lugar de `Email`.

En el `PageModel` de la página de inicio de sesión, ubicado en *Pages\Account\Login.cshtml.cs*, quite el `[EmailAddress]` de atributo de la *correo electrónico* propiedad. Cambie su nombre a *nombre de usuario*. Esto requiere un cambio siempre que sea `EmailAddress` se ha mencionado, en la *vista* y *PageModel*. El resultado es similar a lo siguiente:

 ![Inicio de sesión fijo](identity/_static/fixed-login.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprendió cómo trasladar a los usuarios de pertenencia SQL ASP.NET Core 2.0 Identity. Para obtener más información sobre ASP.NET Core Identity, consulte [Introducción a la identidad](xref:security/authentication/identity).
