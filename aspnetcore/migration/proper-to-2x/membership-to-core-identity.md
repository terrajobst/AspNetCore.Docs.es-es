---
title: Migrar de autenticación de pertenencia de ASP.NET a ASP.NET Core 2.0 Identity
author: isaac2004
description: Obtenga información sobre cómo migrar aplicaciones existentes de ASP.NET mediante la autenticación de pertenencia a ASP.NET Core 2.0 Identity.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2019
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 3b708da13ff9f2887eee87ea17844312a4fe1b8d
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65084876"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a>Migrar de autenticación de pertenencia de ASP.NET a ASP.NET Core 2.0 Identity

Por [Isaac Levin](https://isaaclevin.com)

En este artículo se muestra la migración del esquema de base de datos para las aplicaciones ASP.NET mediante la autenticación de pertenencia a ASP.NET Core 2.0 Identity.

> [!NOTE]
> Este documento proporciona los pasos necesarios para migrar el esquema de base de datos para las aplicaciones basadas en la pertenencia a ASP.NET para el esquema de base de datos utilizado para ASP.NET Core Identity. Para obtener más información sobre la migración de la autenticación basada en pertenencia de ASP.NET a ASP.NET Identity, vea [migrar una aplicación existente de la pertenencia de SQL a ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity). Para obtener más información acerca de ASP.NET Core Identity, vea [Introducción a la identidad en ASP.NET Core](xref:security/authentication/identity).

## <a name="review-of-membership-schema"></a>Revisión del esquema de pertenencia

Antes de ASP.NET 2.0, los desarrolladores se encargan que cree el proceso de autenticación y autorización completo para sus aplicaciones. Con ASP.NET 2.0, se introdujo pertenencia, proporcionar una solución de código reutilizable para controlar la seguridad dentro de las aplicaciones ASP.NET. Los desarrolladores ahora podían arrancar un esquema en una base de datos de SQL Server con el [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) comando. Después de ejecutar este comando, las siguientes tablas se crearon en la base de datos.

  ![Tablas de pertenencia](identity/_static/membership-tables.png)

Para migrar aplicaciones existentes a ASP.NET Core 2.0 Identity, los datos en estas tablas deben migrarse a las tablas utilizadas por el nuevo esquema de identidad.

## <a name="aspnet-core-identity-20-schema"></a>Esquema ASP.NET Core Identity 2.0

ASP.NET Core 2.0 sigue la [identidad](/aspnet/identity/index) principio introducida en ASP.NET 4.5. Aunque se comparte el principio, la implementación entre los marcos de trabajo es diferente, incluso entre las versiones de ASP.NET Core (consulte [migrar autenticación e identidad a ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).

Es la forma más rápida para ver el esquema para la identidad de ASP.NET Core 2.0 crear una nueva aplicación de ASP.NET Core 2.0. Siga estos pasos en Visual Studio 2017:

1. Seleccione **Archivo** > **Nuevo** > **Proyecto**.
1. Cree un nuevo **aplicación Web ASP.NET Core** proyecto denominado *CoreIdentitySample*.
1. Seleccione **ASP.NET Core 2.0** en la lista desplegable y, a continuación, seleccione **aplicación Web**. Esta plantilla genera un [las páginas de Razor](xref:razor-pages/index) app. Antes de hacer clic **Aceptar**, haga clic en **Cambiar autenticación**.
1. Elija **cuentas de usuario individuales** para las plantillas de identidad. Por último, haga clic en **Aceptar**, a continuación, **Aceptar**. Visual Studio crea un proyecto mediante la plantilla de ASP.NET Core Identity.
1. Seleccione **herramientas** > **Administrador de paquetes de NuGet** > **Package Manager Console** para abrir el **Package Manager Console** Ventana (PMC).
1. Navegue hasta la raíz del proyecto en la PMC y ejecute el [Entity Framework (EF) Core](/ef/core) `Update-Database` comando.

    Identidad de ASP.NET Core 2.0 usa EF Core para interactuar con la base de datos almacena los datos de autenticación. Para la aplicación recién creada para que funcione, debe ser una base de datos para almacenar los datos. Después de crear una nueva aplicación, la forma más rápida para inspeccionar el esquema en un entorno de base de datos es crear la base de datos mediante [migraciones de EF Core](/ef/core/managing-schemas/migrations/). Este proceso crea una base de datos, ya sea localmente o en otra parte, que imita ese esquema. Revise la documentación anterior para obtener más información.

    Comandos EF Core usan la cadena de conexión para la base de datos especificada en *appsettings.json*. La siguiente cadena de conexión tiene como destino una base de datos en *localhost* denominado *asp-net-core-identity*. En esta configuración, EF Core está configurado para usar el `DefaultConnection` cadena de conexión.

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
      }
    }
    ```

1. Seleccione **vista** > **Explorador de objetos SQL Server**. Expanda el nodo correspondiente al nombre de la base de datos especificado en el `ConnectionStrings:DefaultConnection` propiedad de *appsettings.json*.

    El `Update-Database` comando crea la base de datos especificada con el esquema y los datos necesarios para la inicialización de la aplicación. La siguiente imagen muestra la estructura de tabla que se crea con los pasos anteriores.

    ![Tablas de identidad](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a>Migrar el esquema

Existen diferencias sutiles en los campos para la suscripción y ASP.NET Core Identity y estructuras de tabla. El modelo ha cambiado significativamente para autenticación y autorización con aplicaciones ASP.NET y ASP.NET Core. Los objetos de clave que todavía se utilizan con identidad están *usuarios* y *Roles*. Estas son las tablas de asignación para *usuarios*, *Roles*, y *UserRoles*.

### <a name="users"></a>Usuarios

|*Identity<br>(dbo.AspNetUsers)*        ||*Pertenencia<br>(dbo.aspnet_Users / dbo.aspnet_Membership)*||
|----------------------------------------|-----------------------------------------------------------|
|**Nombre de campo**                 |**Tipo**|**Nombre de campo**                                    |**Tipo**|
|`Id`                           |string  |`aspnet_Users.UserId`                             |string  |
|`UserName`                     |string  |`aspnet_Users.UserName`                           |string  |
|`Email`                        |string  |`aspnet_Membership.Email`                         |string  |
|`NormalizedUserName`           |string  |`aspnet_Users.LoweredUserName`                    |string  |
|`NormalizedEmail`              |string  |`aspnet_Membership.LoweredEmail`                  |string  |
|`PhoneNumber`                  |string  |`aspnet_Users.MobileAlias`                        |string  |
|`LockoutEnabled`               |bits     |`aspnet_Membership.IsLockedOut`                   |bits     |

> [!NOTE]
> No todas las asignaciones de campos son similares a las relaciones uno a uno de la pertenencia a ASP.NET Core Identity. La tabla anterior toma el esquema de usuario de pertenencia predeterminado y lo asigna al esquema de ASP.NET Core Identity. Los campos personalizados que se usaron para la suscripción deben asignarse manualmente. En esta asignación, no hay ningún mapa de las contraseñas, como los criterios de la contraseña y Sales de la contraseña no se migran entre los dos. **Se recomienda dejar la contraseña como null y pedir a los usuarios restablecer sus contraseñas.** En ASP.NET Core Identity, `LockoutEnd` debe establecerse en una fecha en el futuro, si el usuario está bloqueado. Esto se muestra en el script de migración.

### <a name="roles"></a>Roles

|*Identity<br>(dbo.AspNetRoles)*        ||*Membership<br>(dbo.aspnet_Roles)*||
|----------------------------------------|-----------------------------------|
|**Nombre de campo**                 |**Tipo**|**Nombre de campo**   |**Tipo**         |
|`Id`                           |string  |`RoleId`         | string          |
|`Name`                         |string  |`RoleName`       | string          |
|`NormalizedName`               |string  |`LoweredRoleName`| string          |

### <a name="user-roles"></a>Roles de usuario

|*Identity<br>(dbo.AspNetUserRoles)*||*Membership<br>(dbo.aspnet_UsersInRoles)*||
|------------------------------------|------------------------------------------|
|**Nombre de campo**           |**Tipo**  |**Nombre de campo**|**Tipo**                   |
|`RoleId`                 |string    |`RoleId`      |string                     |
|`UserId`                 |string    |`UserId`      |string                     |

Hacer referencia a las tablas de asignación anterior al crear un script de migración para *usuarios* y *Roles*. En el siguiente ejemplo se da por supuesto que tiene dos bases de datos en un servidor de base de datos. Una base de datos contiene el esquema de pertenencia de ASP.NET existente y los datos. El otro *CoreIdentitySample* base de datos se creó mediante los pasos descritos anteriormente. Los comentarios están incluidas alineadas para obtener más detalles.

```sql
-- THIS SCRIPT NEEDS TO RUN FROM THE CONTEXT OF THE MEMBERSHIP DB
BEGIN TRANSACTION MigrateUsersAndRoles
USE aspnetdb

-- INSERT USERS
INSERT INTO CoreIdentitySample.dbo.AspNetUsers
            (Id,
             UserName,
             NormalizedUserName,
             PasswordHash,
             SecurityStamp,
             EmailConfirmed,
             PhoneNumber,
             PhoneNumberConfirmed,
             TwoFactorEnabled,
             LockoutEnd,
             LockoutEnabled,
             AccessFailedCount,
             Email,
             NormalizedEmail)
SELECT aspnet_Users.UserId,
       aspnet_Users.UserName,
       -- The NormalizedUserName value is upper case in ASP.NET Core Identity
       UPPER(aspnet_Users.UserName),
       -- Creates an empty password since passwords don't map between the 2 schemas
       '',
       /*
        The SecurityStamp token is used to verify the state of an account and
        is subject to change at any time. It should be initialized as a new ID.
       */
       NewID(),
       /*
        EmailConfirmed is set when a new user is created and confirmed via email.
        Users must have this set during migration to reset passwords.
       */
       1,
       aspnet_Users.MobileAlias,
       CASE
         WHEN aspnet_Users.MobileAlias IS NULL THEN 0
         ELSE 1
       END,
       -- 2FA likely wasn't setup in Membership for users, so setting as false.
       0,
       CASE
         -- Setting lockout date to time in the future (1,000 years)
         WHEN aspnet_Membership.IsLockedOut = 1 THEN Dateadd(year, 1000,
                                                     Sysutcdatetime())
         ELSE NULL
       END,
       aspnet_Membership.IsLockedOut,
       /*
        AccessFailedAccount is used to track failed logins. This is stored in
        Membership in multiple columns. Setting to 0 arbitrarily.
       */
       0,
       aspnet_Membership.Email,
       -- The NormalizedEmail value is upper case in ASP.NET Core Identity
       UPPER(aspnet_Membership.Email)
FROM   aspnet_Users
       LEFT OUTER JOIN aspnet_Membership
                    ON aspnet_Membership.ApplicationId =
                       aspnet_Users.ApplicationId
                       AND aspnet_Users.UserId = aspnet_Membership.UserId
       LEFT OUTER JOIN CoreIdentitySample.dbo.AspNetUsers
                    ON aspnet_Membership.UserId = AspNetUsers.Id
WHERE  AspNetUsers.Id IS NULL

-- INSERT ROLES
INSERT INTO CoreIdentitySample.dbo.AspNetRoles(Id, Name)
SELECT RoleId, RoleName
FROM aspnet_Roles;

-- INSERT USER ROLES
INSERT INTO CoreIdentitySample.dbo.AspNetUserRoles(UserId, RoleId)
SELECT UserId, RoleId
FROM aspnet_UsersInRoles;

IF @@ERROR <> 0
  BEGIN
    ROLLBACK TRANSACTION MigrateUsersAndRoles
    RETURN
  END

COMMIT TRANSACTION MigrateUsersAndRoles
```

Cuando haya finalizado el script anterior, la aplicación de ASP.NET Core Identity que creó anteriormente se rellena con usuarios de pertenencia. Los usuarios necesitan cambiar sus contraseñas antes de iniciar sesión.

> [!NOTE]
> Si el sistema de pertenencia tenía usuarios con los nombres de usuario que no coincide con su dirección de correo electrónico, los cambios son necesarios para la aplicación que creó anteriormente para dar cabida a esto. La plantilla predeterminada espera `UserName` y `Email` sea el mismo. Para situaciones en las que son diferentes, debe modificarse para usar el proceso de inicio de sesión `UserName` en lugar de `Email`.

En el `PageModel` de la página de inicio de sesión, ubicado en *Pages\Account\Login.cshtml.cs*, quite el `[EmailAddress]` de atributo de la *correo electrónico* propiedad. Cambie su nombre a *UserName*. Esto requiere un cambio siempre que sea `EmailAddress` se ha mencionado, en la *vista* y *PageModel*. El resultado es similar al siguiente:

 ![Inicio de sesión fijo](identity/_static/fixed-login.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido cómo migrar los usuarios de pertenencia SQL a ASP.NET Core 2.0 Identity. Para obtener más información sobre ASP.NET Core Identity, vea [Introducción a la identidad](xref:security/authentication/identity).
