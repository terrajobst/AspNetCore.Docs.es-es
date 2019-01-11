---
title: Migrar de autenticación de pertenencia de ASP.NET a ASP.NET Core 2.0 Identity
author: isaac2004
description: Obtenga información sobre cómo migrar aplicaciones existentes de ASP.NET mediante la autenticación de pertenencia a ASP.NET Core 2.0 Identity.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2019
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 0b7001a311eeaaa78e3d52e2ec66d33ad057c381
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207413"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a><span data-ttu-id="3db36-103">Migrar de autenticación de pertenencia de ASP.NET a ASP.NET Core 2.0 Identity</span><span class="sxs-lookup"><span data-stu-id="3db36-103">Migrate from ASP.NET Membership authentication to ASP.NET Core 2.0 Identity</span></span>

<span data-ttu-id="3db36-104">Por [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="3db36-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="3db36-105">En este artículo se muestra la migración del esquema de base de datos para las aplicaciones ASP.NET mediante la autenticación de pertenencia a ASP.NET Core 2.0 Identity.</span><span class="sxs-lookup"><span data-stu-id="3db36-105">This article demonstrates migrating the database schema for ASP.NET apps using Membership authentication to ASP.NET Core 2.0 Identity.</span></span>

> [!NOTE]
> <span data-ttu-id="3db36-106">Este documento proporciona los pasos necesarios para migrar el esquema de base de datos para las aplicaciones basadas en la pertenencia a ASP.NET para el esquema de base de datos utilizado para ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="3db36-106">This document provides the steps needed to migrate the database schema for ASP.NET Membership-based apps to the database schema used for ASP.NET Core Identity.</span></span> <span data-ttu-id="3db36-107">Para obtener más información sobre la migración de la autenticación basada en pertenencia de ASP.NET a ASP.NET Identity, vea [migrar una aplicación existente de la pertenencia de SQL a ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span><span class="sxs-lookup"><span data-stu-id="3db36-107">For more information about migrating from ASP.NET Membership-based authentication to ASP.NET Identity, see [Migrate an existing app from SQL Membership to ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span></span> <span data-ttu-id="3db36-108">Para obtener más información acerca de ASP.NET Core Identity, vea [Introducción a la identidad en ASP.NET Core](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="3db36-108">For more information about ASP.NET Core Identity, see [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity).</span></span>

## <a name="review-of-membership-schema"></a><span data-ttu-id="3db36-109">Revisión del esquema de pertenencia</span><span class="sxs-lookup"><span data-stu-id="3db36-109">Review of Membership schema</span></span>

<span data-ttu-id="3db36-110">Antes de ASP.NET 2.0, los desarrolladores se encargan que cree el proceso de autenticación y autorización completo para sus aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="3db36-110">Prior to ASP.NET 2.0, developers were tasked with creating the entire authentication and authorization process for their apps.</span></span> <span data-ttu-id="3db36-111">Con ASP.NET 2.0, se introdujo pertenencia, proporcionar una solución de código reutilizable para controlar la seguridad dentro de las aplicaciones ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3db36-111">With ASP.NET 2.0, Membership was introduced, providing a boilerplate solution to handling security within ASP.NET apps.</span></span> <span data-ttu-id="3db36-112">Los desarrolladores ahora podían arrancar un esquema en una base de datos de SQL Server con el [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) comando.</span><span class="sxs-lookup"><span data-stu-id="3db36-112">Developers were now able to bootstrap a schema into a SQL Server database with the [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) command.</span></span> <span data-ttu-id="3db36-113">Después de ejecutar este comando, las siguientes tablas se crearon en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3db36-113">After running this command, the following tables were created in the database.</span></span>

  ![Tablas de pertenencia](identity/_static/membership-tables.png)

<span data-ttu-id="3db36-115">Para migrar aplicaciones existentes a ASP.NET Core 2.0 Identity, los datos en estas tablas deben migrarse a las tablas utilizadas por el nuevo esquema de identidad.</span><span class="sxs-lookup"><span data-stu-id="3db36-115">To migrate existing apps to ASP.NET Core 2.0 Identity, the data in these tables needs to be migrated to the tables used by the new Identity schema.</span></span>

## <a name="aspnet-core-identity-20-schema"></a><span data-ttu-id="3db36-116">Esquema ASP.NET Core Identity 2.0</span><span class="sxs-lookup"><span data-stu-id="3db36-116">ASP.NET Core Identity 2.0 schema</span></span>

<span data-ttu-id="3db36-117">ASP.NET Core 2.0 sigue la [identidad](/aspnet/identity/index) principio introducida en ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="3db36-117">ASP.NET Core 2.0 follows the [Identity](/aspnet/identity/index) principle introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="3db36-118">Aunque se comparte el principio, la implementación entre los marcos de trabajo es diferente, incluso entre las versiones de ASP.NET Core (consulte [migrar autenticación e identidad a ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span><span class="sxs-lookup"><span data-stu-id="3db36-118">Though the principle is shared, the implementation between the frameworks is different, even between versions of ASP.NET Core (see [Migrate authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span></span>

<span data-ttu-id="3db36-119">Es la forma más rápida para ver el esquema para la identidad de ASP.NET Core 2.0 crear una nueva aplicación de ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="3db36-119">The fastest way to view the schema for ASP.NET Core 2.0 Identity is to create a new ASP.NET Core 2.0 app.</span></span> <span data-ttu-id="3db36-120">Siga estos pasos en Visual Studio 2017:</span><span class="sxs-lookup"><span data-stu-id="3db36-120">Follow these steps in Visual Studio 2017:</span></span>

1. <span data-ttu-id="3db36-121">Seleccione **Archivo** > **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="3db36-121">Select **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="3db36-122">Cree un nuevo **aplicación Web ASP.NET Core** proyecto denominado *CoreIdentitySample*.</span><span class="sxs-lookup"><span data-stu-id="3db36-122">Create a new **ASP.NET Core Web Application** project named *CoreIdentitySample*.</span></span>
1. <span data-ttu-id="3db36-123">Seleccione **ASP.NET Core 2.0** en la lista desplegable y, a continuación, seleccione **aplicación Web**.</span><span class="sxs-lookup"><span data-stu-id="3db36-123">Select **ASP.NET Core 2.0** in the dropdown and then select **Web Application**.</span></span> <span data-ttu-id="3db36-124">Esta plantilla genera un [las páginas de Razor](xref:razor-pages/index) app.</span><span class="sxs-lookup"><span data-stu-id="3db36-124">This template produces a [Razor Pages](xref:razor-pages/index) app.</span></span> <span data-ttu-id="3db36-125">Antes de hacer clic **Aceptar**, haga clic en **Cambiar autenticación**.</span><span class="sxs-lookup"><span data-stu-id="3db36-125">Before clicking **OK**, click **Change Authentication**.</span></span>
1. <span data-ttu-id="3db36-126">Elija **cuentas de usuario individuales** para las plantillas de identidad.</span><span class="sxs-lookup"><span data-stu-id="3db36-126">Choose **Individual User Accounts** for the Identity templates.</span></span> <span data-ttu-id="3db36-127">Por último, haga clic en **Aceptar**, a continuación, **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="3db36-127">Finally, click **OK**, then **OK**.</span></span> <span data-ttu-id="3db36-128">Visual Studio crea un proyecto mediante la plantilla de ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="3db36-128">Visual Studio creates a project using the ASP.NET Core Identity template.</span></span>
1. <span data-ttu-id="3db36-129">Seleccione **herramientas** > **Administrador de paquetes de NuGet** > **Package Manager Console** para abrir el **Package Manager Console** Ventana (PMC).</span><span class="sxs-lookup"><span data-stu-id="3db36-129">Select **Tools** > **NuGet Package Manager** > **Package Manager Console** to open the **Package Manager Console** (PMC) window.</span></span>
1. <span data-ttu-id="3db36-130">Navegue hasta la raíz del proyecto en la PMC y ejecute el [Entity Framework (EF) Core](/ef/core) `Update-Database` comando.</span><span class="sxs-lookup"><span data-stu-id="3db36-130">Navigate to the project root in PMC, and run the [Entity Framework (EF) Core](/ef/core) `Update-Database` command.</span></span>

    <span data-ttu-id="3db36-131">Identidad de ASP.NET Core 2.0 usa EF Core para interactuar con la base de datos almacena los datos de autenticación.</span><span class="sxs-lookup"><span data-stu-id="3db36-131">ASP.NET Core 2.0 Identity uses EF Core to interact with the database storing the authentication data.</span></span> <span data-ttu-id="3db36-132">Para la aplicación recién creada para que funcione, debe ser una base de datos para almacenar los datos.</span><span class="sxs-lookup"><span data-stu-id="3db36-132">In order for the newly created app to work, there needs to be a database to store this data.</span></span> <span data-ttu-id="3db36-133">Después de crear una nueva aplicación, la forma más rápida para inspeccionar el esquema en un entorno de base de datos es crear la base de datos mediante [migraciones de EF Core](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="3db36-133">After creating a new app, the fastest way to inspect the schema in a database environment is to create the database using [EF Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="3db36-134">Este proceso crea una base de datos, ya sea localmente o en otra parte, que imita ese esquema.</span><span class="sxs-lookup"><span data-stu-id="3db36-134">This process creates a database, either locally or elsewhere, which mimics that schema.</span></span> <span data-ttu-id="3db36-135">Revise la documentación anterior para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="3db36-135">Review the preceding documentation for more information.</span></span>

    <span data-ttu-id="3db36-136">Comandos EF Core usan la cadena de conexión para la base de datos especificada en *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="3db36-136">EF Core commands use the connection string for the database specified in *appsettings.json*.</span></span> <span data-ttu-id="3db36-137">La siguiente cadena de conexión tiene como destino una base de datos en *localhost* denominado *asp-net-core-identity*.</span><span class="sxs-lookup"><span data-stu-id="3db36-137">The following connection string targets a database on *localhost* named *asp-net-core-identity*.</span></span> <span data-ttu-id="3db36-138">En esta configuración, EF Core está configurado para usar el `DefaultConnection` cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="3db36-138">In this setting, EF Core is configured to use the `DefaultConnection` connection string.</span></span>

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
      }
    }
    ```
1. <span data-ttu-id="3db36-139">Seleccione **vista** > **Explorador de objetos SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="3db36-139">Select **View** > **SQL Server Object Explorer**.</span></span> <span data-ttu-id="3db36-140">Expanda el nodo correspondiente al nombre de la base de datos especificado en el `ConnectionStrings:DefaultConnection` propiedad de *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="3db36-140">Expand the node corresponding to the database name specified in the `ConnectionStrings:DefaultConnection` property of *appsettings.json*.</span></span>

    <span data-ttu-id="3db36-141">El `Update-Database` comando crea la base de datos especificada con el esquema y los datos necesarios para la inicialización de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3db36-141">The `Update-Database` command created the database specified with the schema and any data needed for app initialization.</span></span> <span data-ttu-id="3db36-142">La siguiente imagen muestra la estructura de tabla que se crea con los pasos anteriores.</span><span class="sxs-lookup"><span data-stu-id="3db36-142">The following image depicts the table structure that's created with the preceding steps.</span></span>

    ![Tablas de identidad](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a><span data-ttu-id="3db36-144">Migrar el esquema</span><span class="sxs-lookup"><span data-stu-id="3db36-144">Migrate the schema</span></span>

<span data-ttu-id="3db36-145">Existen diferencias sutiles en los campos para la suscripción y ASP.NET Core Identity y estructuras de tabla.</span><span class="sxs-lookup"><span data-stu-id="3db36-145">There are subtle differences in the table structures and fields for both Membership and ASP.NET Core Identity.</span></span> <span data-ttu-id="3db36-146">El modelo ha cambiado significativamente para autenticación y autorización con aplicaciones ASP.NET y ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3db36-146">The pattern has changed substantially for authentication/authorization with ASP.NET and ASP.NET Core apps.</span></span> <span data-ttu-id="3db36-147">Los objetos de clave que todavía se utilizan con identidad están *usuarios* y *Roles*.</span><span class="sxs-lookup"><span data-stu-id="3db36-147">The key objects that are still used with Identity are *Users* and *Roles*.</span></span> <span data-ttu-id="3db36-148">Estas son las tablas de asignación para *usuarios*, *Roles*, y *UserRoles*.</span><span class="sxs-lookup"><span data-stu-id="3db36-148">Here are mapping tables for *Users*, *Roles*, and *UserRoles*.</span></span>

### <a name="users"></a><span data-ttu-id="3db36-149">Usuarios</span><span class="sxs-lookup"><span data-stu-id="3db36-149">Users</span></span>

|<span data-ttu-id="3db36-150">*Identidad<br>(dbo. AspNetUsers)*</span><span class="sxs-lookup"><span data-stu-id="3db36-150">*Identity<br>(dbo.AspNetUsers)*</span></span>        ||<span data-ttu-id="3db36-151">*Pertenencia<br>(dbo.aspnet_Users / dbo.aspnet_Membership)*</span><span class="sxs-lookup"><span data-stu-id="3db36-151">*Membership<br>(dbo.aspnet_Users / dbo.aspnet_Membership)*</span></span>||
|----------------------------------------|-----------------------------------------------------------|
|<span data-ttu-id="3db36-152">**Nombre de campo**</span><span class="sxs-lookup"><span data-stu-id="3db36-152">**Field Name**</span></span>                 |<span data-ttu-id="3db36-153">**Type**</span><span class="sxs-lookup"><span data-stu-id="3db36-153">**Type**</span></span>|<span data-ttu-id="3db36-154">**Nombre de campo**</span><span class="sxs-lookup"><span data-stu-id="3db36-154">**Field Name**</span></span>                                    |<span data-ttu-id="3db36-155">**Type**</span><span class="sxs-lookup"><span data-stu-id="3db36-155">**Type**</span></span>|
|`Id`                           |<span data-ttu-id="3db36-156">cadena</span><span class="sxs-lookup"><span data-stu-id="3db36-156">string</span></span>  |`aspnet_Users.UserId`                             |<span data-ttu-id="3db36-157">cadena</span><span class="sxs-lookup"><span data-stu-id="3db36-157">string</span></span>  |
|`UserName`                     |<span data-ttu-id="3db36-158">cadena</span><span class="sxs-lookup"><span data-stu-id="3db36-158">string</span></span>  |`aspnet_Users.UserName`                           |<span data-ttu-id="3db36-159">cadena</span><span class="sxs-lookup"><span data-stu-id="3db36-159">string</span></span>  |
|`Email`                        |<span data-ttu-id="3db36-160">cadena</span><span class="sxs-lookup"><span data-stu-id="3db36-160">string</span></span>  |`aspnet_Membership.Email`                         |<span data-ttu-id="3db36-161">cadena</span><span class="sxs-lookup"><span data-stu-id="3db36-161">string</span></span>  |
|`NormalizedUserName`           |<span data-ttu-id="3db36-162">cadena</span><span class="sxs-lookup"><span data-stu-id="3db36-162">string</span></span>  |`aspnet_Users.LoweredUserName`                    |<span data-ttu-id="3db36-163">cadena</span><span class="sxs-lookup"><span data-stu-id="3db36-163">string</span></span>  |
|`NormalizedEmail`              |<span data-ttu-id="3db36-164">cadena</span><span class="sxs-lookup"><span data-stu-id="3db36-164">string</span></span>  |`aspnet_Membership.LoweredEmail`                  |<span data-ttu-id="3db36-165">cadena</span><span class="sxs-lookup"><span data-stu-id="3db36-165">string</span></span>  |
|`PhoneNumber`                  |<span data-ttu-id="3db36-166">cadena</span><span class="sxs-lookup"><span data-stu-id="3db36-166">string</span></span>  |`aspnet_Users.MobileAlias`                        |<span data-ttu-id="3db36-167">cadena</span><span class="sxs-lookup"><span data-stu-id="3db36-167">string</span></span>  |
|`LockoutEnabled`               |<span data-ttu-id="3db36-168">bits</span><span class="sxs-lookup"><span data-stu-id="3db36-168">bit</span></span>     |`aspnet_Membership.IsLockedOut`                   |<span data-ttu-id="3db36-169">bits</span><span class="sxs-lookup"><span data-stu-id="3db36-169">bit</span></span>     |

> [!NOTE]
> <span data-ttu-id="3db36-170">No todas las asignaciones de campos son similares a las relaciones uno a uno de la pertenencia a ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="3db36-170">Not all the field mappings resemble one-to-one relationships from Membership to ASP.NET Core Identity.</span></span> <span data-ttu-id="3db36-171">La tabla anterior toma el esquema de usuario de pertenencia predeterminado y lo asigna al esquema de ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="3db36-171">The preceding table takes the default Membership User schema and maps it to the ASP.NET Core Identity schema.</span></span> <span data-ttu-id="3db36-172">Los campos personalizados que se usaron para la suscripción deben asignarse manualmente.</span><span class="sxs-lookup"><span data-stu-id="3db36-172">Any other custom fields that were used for Membership need to be mapped manually.</span></span> <span data-ttu-id="3db36-173">En esta asignación, no hay ningún mapa de las contraseñas, como los criterios de la contraseña y Sales de la contraseña no se migran entre los dos.</span><span class="sxs-lookup"><span data-stu-id="3db36-173">In this mapping, there's no map for passwords, as both password criteria and password salts don't migrate between the two.</span></span> <span data-ttu-id="3db36-174">**Se recomienda dejar la contraseña como null y pedir a los usuarios restablecer sus contraseñas.**</span><span class="sxs-lookup"><span data-stu-id="3db36-174">**It's recommended to leave the password as null and to ask users to reset their passwords.**</span></span> <span data-ttu-id="3db36-175">En ASP.NET Core Identity, `LockoutEnd` debe establecerse en una fecha en el futuro, si el usuario está bloqueado. Esto se muestra en el script de migración.</span><span class="sxs-lookup"><span data-stu-id="3db36-175">In ASP.NET Core Identity, `LockoutEnd` should be set to some date in the future if the user is locked out. This is shown in the migration script.</span></span>

### <a name="roles"></a><span data-ttu-id="3db36-176">Roles</span><span class="sxs-lookup"><span data-stu-id="3db36-176">Roles</span></span>

|<span data-ttu-id="3db36-177">*Identidad<br>(dbo. AspNetRoles)*</span><span class="sxs-lookup"><span data-stu-id="3db36-177">*Identity<br>(dbo.AspNetRoles)*</span></span>        ||<span data-ttu-id="3db36-178">*Pertenencia<br>(dbo.aspnet_Roles)*</span><span class="sxs-lookup"><span data-stu-id="3db36-178">*Membership<br>(dbo.aspnet_Roles)*</span></span>||
|----------------------------------------|-----------------------------------|
|<span data-ttu-id="3db36-179">**Nombre de campo**</span><span class="sxs-lookup"><span data-stu-id="3db36-179">**Field Name**</span></span>                 |<span data-ttu-id="3db36-180">**Type**</span><span class="sxs-lookup"><span data-stu-id="3db36-180">**Type**</span></span>|<span data-ttu-id="3db36-181">**Nombre de campo**</span><span class="sxs-lookup"><span data-stu-id="3db36-181">**Field Name**</span></span>   |<span data-ttu-id="3db36-182">**Type**</span><span class="sxs-lookup"><span data-stu-id="3db36-182">**Type**</span></span>         |
|`Id`                           |<span data-ttu-id="3db36-183">cadena</span><span class="sxs-lookup"><span data-stu-id="3db36-183">string</span></span>  |`RoleId`         | <span data-ttu-id="3db36-184">cadena</span><span class="sxs-lookup"><span data-stu-id="3db36-184">string</span></span>          |
|`Name`                         |<span data-ttu-id="3db36-185">cadena</span><span class="sxs-lookup"><span data-stu-id="3db36-185">string</span></span>  |`RoleName`       | <span data-ttu-id="3db36-186">cadena</span><span class="sxs-lookup"><span data-stu-id="3db36-186">string</span></span>          |
|`NormalizedName`               |<span data-ttu-id="3db36-187">cadena</span><span class="sxs-lookup"><span data-stu-id="3db36-187">string</span></span>  |`LoweredRoleName`| <span data-ttu-id="3db36-188">cadena</span><span class="sxs-lookup"><span data-stu-id="3db36-188">string</span></span>          |

### <a name="user-roles"></a><span data-ttu-id="3db36-189">Roles de usuario</span><span class="sxs-lookup"><span data-stu-id="3db36-189">User Roles</span></span>

|<span data-ttu-id="3db36-190">*Identidad<br>(dbo. AspNetUserRoles)*</span><span class="sxs-lookup"><span data-stu-id="3db36-190">*Identity<br>(dbo.AspNetUserRoles)*</span></span>||<span data-ttu-id="3db36-191">*Pertenencia<br>(dbo.aspnet_UsersInRoles)*</span><span class="sxs-lookup"><span data-stu-id="3db36-191">*Membership<br>(dbo.aspnet_UsersInRoles)*</span></span>||
|------------------------------------|------------------------------------------|
|<span data-ttu-id="3db36-192">**Nombre de campo**</span><span class="sxs-lookup"><span data-stu-id="3db36-192">**Field Name**</span></span>           |<span data-ttu-id="3db36-193">**Type**</span><span class="sxs-lookup"><span data-stu-id="3db36-193">**Type**</span></span>  |<span data-ttu-id="3db36-194">**Nombre de campo**</span><span class="sxs-lookup"><span data-stu-id="3db36-194">**Field Name**</span></span>|<span data-ttu-id="3db36-195">**Type**</span><span class="sxs-lookup"><span data-stu-id="3db36-195">**Type**</span></span>                   |
|`RoleId`                 |<span data-ttu-id="3db36-196">cadena</span><span class="sxs-lookup"><span data-stu-id="3db36-196">string</span></span>    |`RoleId`      |<span data-ttu-id="3db36-197">cadena</span><span class="sxs-lookup"><span data-stu-id="3db36-197">string</span></span>                     |
|`UserId`                 |<span data-ttu-id="3db36-198">cadena</span><span class="sxs-lookup"><span data-stu-id="3db36-198">string</span></span>    |`UserId`      |<span data-ttu-id="3db36-199">cadena</span><span class="sxs-lookup"><span data-stu-id="3db36-199">string</span></span>                     |

<span data-ttu-id="3db36-200">Hacer referencia a las tablas de asignación anterior al crear un script de migración para *usuarios* y *Roles*.</span><span class="sxs-lookup"><span data-stu-id="3db36-200">Reference the preceding mapping tables when creating a migration script for *Users* and *Roles*.</span></span> <span data-ttu-id="3db36-201">En el siguiente ejemplo se da por supuesto que tiene dos bases de datos en un servidor de base de datos.</span><span class="sxs-lookup"><span data-stu-id="3db36-201">The following example assumes you have two databases on a database server.</span></span> <span data-ttu-id="3db36-202">Una base de datos contiene el esquema de pertenencia de ASP.NET existente y los datos.</span><span class="sxs-lookup"><span data-stu-id="3db36-202">One database contains the existing ASP.NET Membership schema and data.</span></span> <span data-ttu-id="3db36-203">El otro *CoreIdentitySample* base de datos se creó mediante los pasos descritos anteriormente.</span><span class="sxs-lookup"><span data-stu-id="3db36-203">The other *CoreIdentitySample* database was created using steps described earlier.</span></span> <span data-ttu-id="3db36-204">Los comentarios están incluidas alineadas para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="3db36-204">Comments are included inline for more details.</span></span>

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

<span data-ttu-id="3db36-205">Cuando haya finalizado el script anterior, la aplicación de ASP.NET Core Identity que creó anteriormente se rellena con usuarios de pertenencia.</span><span class="sxs-lookup"><span data-stu-id="3db36-205">After completion of the preceding script, the ASP.NET Core Identity app created earlier is populated with Membership users.</span></span> <span data-ttu-id="3db36-206">Los usuarios necesitan cambiar sus contraseñas antes de iniciar sesión.</span><span class="sxs-lookup"><span data-stu-id="3db36-206">Users need to change their passwords before logging in.</span></span>

> [!NOTE]
> <span data-ttu-id="3db36-207">Si el sistema de pertenencia tenía usuarios con los nombres de usuario que no coincide con su dirección de correo electrónico, los cambios son necesarios para la aplicación que creó anteriormente para dar cabida a esto.</span><span class="sxs-lookup"><span data-stu-id="3db36-207">If the Membership system had users with user names that didn't match their email address, changes are required to the app created earlier to accommodate this.</span></span> <span data-ttu-id="3db36-208">La plantilla predeterminada espera `UserName` y `Email` sea el mismo.</span><span class="sxs-lookup"><span data-stu-id="3db36-208">The default template expects `UserName` and `Email` to be the same.</span></span> <span data-ttu-id="3db36-209">Para situaciones en las que son diferentes, debe modificarse para usar el proceso de inicio de sesión `UserName` en lugar de `Email`.</span><span class="sxs-lookup"><span data-stu-id="3db36-209">For situations in which they're different, the login process needs to be modified to use `UserName` instead of `Email`.</span></span>

<span data-ttu-id="3db36-210">En el `PageModel` de la página de inicio de sesión, ubicado en *Pages\Account\Login.cshtml.cs*, quite el `[EmailAddress]` de atributo de la *correo electrónico* propiedad.</span><span class="sxs-lookup"><span data-stu-id="3db36-210">In the `PageModel` of the Login Page, located at *Pages\Account\Login.cshtml.cs*, remove the `[EmailAddress]` attribute from the *Email* property.</span></span> <span data-ttu-id="3db36-211">Cambie su nombre a *UserName*.</span><span class="sxs-lookup"><span data-stu-id="3db36-211">Rename it to *UserName*.</span></span> <span data-ttu-id="3db36-212">Esto requiere un cambio siempre que sea `EmailAddress` se ha mencionado, en la *vista* y *PageModel*.</span><span class="sxs-lookup"><span data-stu-id="3db36-212">This requires a change wherever `EmailAddress` is mentioned, in the *View* and *PageModel*.</span></span> <span data-ttu-id="3db36-213">El resultado es similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="3db36-213">The result looks like the following:</span></span>

 ![Inicio de sesión fijo](identity/_static/fixed-login.png)

## <a name="next-steps"></a><span data-ttu-id="3db36-215">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="3db36-215">Next steps</span></span>

<span data-ttu-id="3db36-216">En este tutorial, ha aprendido cómo migrar los usuarios de pertenencia SQL a ASP.NET Core 2.0 Identity.</span><span class="sxs-lookup"><span data-stu-id="3db36-216">In this tutorial, you learned how to port users from SQL membership to ASP.NET Core 2.0 Identity.</span></span> <span data-ttu-id="3db36-217">Para obtener más información sobre ASP.NET Core Identity, vea [Introducción a la identidad](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="3db36-217">For more information regarding ASP.NET Core Identity, see [Introduction to Identity](xref:security/authentication/identity).</span></span>
