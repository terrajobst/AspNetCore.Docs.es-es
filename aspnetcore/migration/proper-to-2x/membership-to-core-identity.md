---
title: Migrar de autenticación de pertenencia a ASP.NET a ASP.NET Core 2.0 Identity
author: isaac2004
description: Obtenga información acerca de cómo migrar las aplicaciones ASP.NET existentes mediante la autenticación de pertenencia a ASP.NET Core 2.0 Identity.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: f0d1099bfda01d036831350e0888ae3830ad3d58
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851547"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a><span data-ttu-id="a000a-103">Migrar de autenticación de pertenencia a ASP.NET a ASP.NET Core 2.0 Identity</span><span class="sxs-lookup"><span data-stu-id="a000a-103">Migrate from ASP.NET Membership authentication to ASP.NET Core 2.0 Identity</span></span>

<span data-ttu-id="a000a-104">Por [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="a000a-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="a000a-105">Este artículo muestra cómo migrar el esquema de base de datos para las aplicaciones ASP.NET mediante la autenticación de pertenencia a ASP.NET Core 2.0 Identity.</span><span class="sxs-lookup"><span data-stu-id="a000a-105">This article demonstrates migrating the database schema for ASP.NET apps using Membership authentication to ASP.NET Core 2.0 Identity.</span></span>

> [!NOTE]
> <span data-ttu-id="a000a-106">Este documento proporciona los pasos necesarios para migrar el esquema de base de datos para aplicaciones basadas en la pertenencia a ASP.NET para el esquema de base de datos usado para identidad de núcleo de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a000a-106">This document provides the steps needed to migrate the database schema for ASP.NET Membership-based apps to the database schema used for ASP.NET Core Identity.</span></span> <span data-ttu-id="a000a-107">Para obtener más información sobre cómo migrar de autenticación basada en pertenencia a ASP.NET a ASP.NET Identity, consulte [migrar una aplicación existente de SQL pertenencia a ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span><span class="sxs-lookup"><span data-stu-id="a000a-107">For more information about migrating from ASP.NET Membership-based authentication to ASP.NET Identity, see [Migrate an existing app from SQL Membership to ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span></span> <span data-ttu-id="a000a-108">Para obtener más información acerca de la identidad de núcleo de ASP.NET, vea [Introducción a la identidad en ASP.NET Core](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="a000a-108">For more information about ASP.NET Core Identity, see [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity).</span></span>

## <a name="review-of-membership-schema"></a><span data-ttu-id="a000a-109">Revisión del esquema de pertenencia</span><span class="sxs-lookup"><span data-stu-id="a000a-109">Review of Membership schema</span></span>

<span data-ttu-id="a000a-110">Antes de ASP.NET 2.0, los desarrolladores se encargan que cree el proceso de autenticación y autorización completo para sus aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="a000a-110">Prior to ASP.NET 2.0, developers were tasked with creating the entire authentication and authorization process for their apps.</span></span> <span data-ttu-id="a000a-111">Con ASP.NET 2.0, se introdujo pertenencia, proporcionar una solución reutilizable para administrar la seguridad en las aplicaciones ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a000a-111">With ASP.NET 2.0, Membership was introduced, providing a boilerplate solution to handling security within ASP.NET apps.</span></span> <span data-ttu-id="a000a-112">Los desarrolladores ahora podían arrancar un esquema en una base de datos de SQL Server con el [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) comando.</span><span class="sxs-lookup"><span data-stu-id="a000a-112">Developers were now able to bootstrap a schema into a SQL Server database with the [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) command.</span></span> <span data-ttu-id="a000a-113">Después de ejecutar este comando, en las siguientes tablas se crearon en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a000a-113">After running this command, the following tables were created in the database.</span></span>

  ![Tablas de pertenencia](identity/_static/membership-tables.png)

<span data-ttu-id="a000a-115">Para migrar aplicaciones existentes a ASP.NET Core 2.0 Identity, los datos en estas tablas tienen que migrarse a las tablas utilizadas por el nuevo esquema de identidad.</span><span class="sxs-lookup"><span data-stu-id="a000a-115">To migrate existing apps to ASP.NET Core 2.0 Identity, the data in these tables needs to be migrated to the tables used by the new Identity schema.</span></span>

## <a name="aspnet-core-identity-20-schema"></a><span data-ttu-id="a000a-116">Esquema de núcleo identidad 2.0 de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a000a-116">ASP.NET Core Identity 2.0 schema</span></span>

<span data-ttu-id="a000a-117">Núcleo de ASP.NET 2.0 sigue la [identidad](/aspnet/identity/index) principio introducida en ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="a000a-117">ASP.NET Core 2.0 follows the [Identity](/aspnet/identity/index) principle introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="a000a-118">Aunque se comparte el principio, la implementación entre los marcos es diferente, incluso entre las versiones de ASP.NET Core (vea [migrar autenticación e identidad a ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span><span class="sxs-lookup"><span data-stu-id="a000a-118">Though the principle is shared, the implementation between the frameworks is different, even between versions of ASP.NET Core (see [Migrate authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span></span>

<span data-ttu-id="a000a-119">La manera más rápida de ver el esquema de núcleo de ASP.NET 2.0 Identity consiste en crear una nueva aplicación de ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="a000a-119">The fastest way to view the schema for ASP.NET Core 2.0 Identity is to create a new ASP.NET Core 2.0 app.</span></span> <span data-ttu-id="a000a-120">Siga estos pasos en Visual Studio de 2017:</span><span class="sxs-lookup"><span data-stu-id="a000a-120">Follow these steps in Visual Studio 2017:</span></span>

* <span data-ttu-id="a000a-121">Seleccione **Archivo** > **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="a000a-121">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="a000a-122">Crear un nuevo **aplicación Web de ASP.NET Core**y el nombre del proyecto *CoreIdentitySample*.</span><span class="sxs-lookup"><span data-stu-id="a000a-122">Create a new **ASP.NET Core Web Application**, and name the project *CoreIdentitySample*.</span></span>
* <span data-ttu-id="a000a-123">Seleccione **ASP.NET Core 2.0** en la lista desplegable y, luego, seleccione **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="a000a-123">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span> <span data-ttu-id="a000a-124">Esta plantilla genera un [páginas de Razor](xref:mvc/razor-pages/index) aplicación.</span><span class="sxs-lookup"><span data-stu-id="a000a-124">This template produces a [Razor Pages](xref:mvc/razor-pages/index) app.</span></span> <span data-ttu-id="a000a-125">Antes de hacer clic **Aceptar**, haga clic en **Cambiar autenticación**.</span><span class="sxs-lookup"><span data-stu-id="a000a-125">Before clicking **OK**, click **Change Authentication**.</span></span>
* <span data-ttu-id="a000a-126">Elija **cuentas de usuario individuales** para las plantillas de identidad.</span><span class="sxs-lookup"><span data-stu-id="a000a-126">Choose **Individual User Accounts** for the Identity templates.</span></span> <span data-ttu-id="a000a-127">Por último, haga clic en **Aceptar**, a continuación, **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="a000a-127">Finally, click **OK**, then **OK**.</span></span> <span data-ttu-id="a000a-128">Visual Studio crea un proyecto mediante la plantilla de ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="a000a-128">Visual Studio creates a project using the ASP.NET Core Identity template.</span></span>

<span data-ttu-id="a000a-129">Identidad de núcleo de ASP.NET 2.0 utiliza [Entity Framework Core](/ef/core) para interactuar con la base de datos almacena los datos de autenticación.</span><span class="sxs-lookup"><span data-stu-id="a000a-129">ASP.NET Core 2.0 Identity uses [Entity Framework Core](/ef/core) to interact with the database storing the authentication data.</span></span> <span data-ttu-id="a000a-130">En orden de la aplicación recién creada para que funcione, debe ser una base de datos para almacenar estos datos.</span><span class="sxs-lookup"><span data-stu-id="a000a-130">In order for the newly created app to work, there needs to be a database to store this data.</span></span> <span data-ttu-id="a000a-131">Después de crear una nueva aplicación, la manera más rápida para inspeccionar el esquema en un entorno de base de datos es crear la base de datos usando migraciones de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a000a-131">After creating a new app, the fastest way to inspect the schema in a database environment is to create the database using Entity Framework migrations.</span></span> <span data-ttu-id="a000a-132">Este proceso crea una base de datos, ya sea localmente o en otro lugar, que imita ese esquema.</span><span class="sxs-lookup"><span data-stu-id="a000a-132">This process creates a database, either locally or elsewhere, which mimics that schema.</span></span> <span data-ttu-id="a000a-133">Revise la documentación anterior para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="a000a-133">Review the preceding documentation for more information.</span></span>

<span data-ttu-id="a000a-134">Para crear una base de datos con el esquema de la identidad de ASP.NET Core, ejecute el `Update-Database` en Visual Studio **Package Manager Console** ventana (PMC)&mdash;se encuentra en **herramientas**  >  **Administrador de paquetes de NuGet** > **consola de administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="a000a-134">To create a database with the ASP.NET Core Identity schema, run the `Update-Database` command in Visual Studio's **Package Manager Console** (PMC) window&mdash;it's located at **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="a000a-135">PMC admite la ejecución de los comandos de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a000a-135">PMC supports running Entity Framework commands.</span></span>

<span data-ttu-id="a000a-136">Comandos de Entity Framework usan la cadena de conexión para la base de datos especificada en *appSettings.JSON que se*.</span><span class="sxs-lookup"><span data-stu-id="a000a-136">Entity Framework commands use the connection string for the database specified in *appsettings.json*.</span></span> <span data-ttu-id="a000a-137">La siguiente cadena de conexión tiene como destino una base de datos en *localhost* denominado *asp-net-core-identity*.</span><span class="sxs-lookup"><span data-stu-id="a000a-137">The following connection string targets a database on *localhost* named *asp-net-core-identity*.</span></span> <span data-ttu-id="a000a-138">En esta configuración, Entity Framework está configurado para usar el `DefaultConnection` cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="a000a-138">In this setting, Entity Framework is configured to use the `DefaultConnection` connection string.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

<span data-ttu-id="a000a-139">Este comando crea la base de datos especificada con el esquema y los datos necesarios para la inicialización de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a000a-139">This command builds the database specified with the schema and any data needed for app initialization.</span></span> <span data-ttu-id="a000a-140">La siguiente imagen muestra la estructura de tabla que se crea con los pasos anteriores.</span><span class="sxs-lookup"><span data-stu-id="a000a-140">The following image depicts the table structure that's created with the preceding steps.</span></span>

   ![Tablas de identidad](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a><span data-ttu-id="a000a-142">migrar el esquema</span><span class="sxs-lookup"><span data-stu-id="a000a-142">Migrate the schema</span></span>

<span data-ttu-id="a000a-143">Existen diferencias sutiles en los campos para la suscripción y ASP.NET Core Identity y estructuras de tabla.</span><span class="sxs-lookup"><span data-stu-id="a000a-143">There are subtle differences in the table structures and fields for both Membership and ASP.NET Core Identity.</span></span> <span data-ttu-id="a000a-144">El modelo ha cambiado considerablemente para la autenticación/autorización con aplicaciones ASP.NET y ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a000a-144">The pattern has changed substantially for authentication/authorization with ASP.NET and ASP.NET Core apps.</span></span> <span data-ttu-id="a000a-145">Los objetos de clave que todavía se utilizan con identidad están *usuarios* y *Roles*.</span><span class="sxs-lookup"><span data-stu-id="a000a-145">The key objects that are still used with Identity are *Users* and *Roles*.</span></span> <span data-ttu-id="a000a-146">Estas son las tablas de asignación de *usuarios*, *Roles*, y *UserRoles*.</span><span class="sxs-lookup"><span data-stu-id="a000a-146">Here are mapping tables for *Users*, *Roles*, and *UserRoles*.</span></span>

### <a name="users"></a><span data-ttu-id="a000a-147">Usuarios</span><span class="sxs-lookup"><span data-stu-id="a000a-147">Users</span></span>

| <span data-ttu-id="a000a-148">*Identity(AspNetUsers)*</span><span class="sxs-lookup"><span data-stu-id="a000a-148">*Identity(AspNetUsers)*</span></span> |   | <span data-ttu-id="a000a-149">*Membership(aspnet_Users/aspnet_Membership)*</span><span class="sxs-lookup"><span data-stu-id="a000a-149">*Membership(aspnet_Users/aspnet_Membership)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="a000a-150">**Nombre de campo**</span><span class="sxs-lookup"><span data-stu-id="a000a-150">**Field Name**</span></span> | <span data-ttu-id="a000a-151">**Type**</span><span class="sxs-lookup"><span data-stu-id="a000a-151">**Type**</span></span>  |   <span data-ttu-id="a000a-152">**Nombre de campo**</span><span class="sxs-lookup"><span data-stu-id="a000a-152">**Field Name**</span></span> | <span data-ttu-id="a000a-153">**Type**</span><span class="sxs-lookup"><span data-stu-id="a000a-153">**Type**</span></span>  |
|`Id` | <span data-ttu-id="a000a-154">cadena</span><span class="sxs-lookup"><span data-stu-id="a000a-154">string</span></span> | `aspnet_Users.UserId` | <span data-ttu-id="a000a-155">cadena</span><span class="sxs-lookup"><span data-stu-id="a000a-155">string</span></span>
|`UserName` | <span data-ttu-id="a000a-156">cadena</span><span class="sxs-lookup"><span data-stu-id="a000a-156">string</span></span> | `aspnet_Users.UserName` | <span data-ttu-id="a000a-157">cadena</span><span class="sxs-lookup"><span data-stu-id="a000a-157">string</span></span>
|`Email` | <span data-ttu-id="a000a-158">cadena</span><span class="sxs-lookup"><span data-stu-id="a000a-158">string</span></span> | `aspnet_Membership.Email` | <span data-ttu-id="a000a-159">cadena</span><span class="sxs-lookup"><span data-stu-id="a000a-159">string</span></span>
|`NormalizedUserName` | <span data-ttu-id="a000a-160">cadena</span><span class="sxs-lookup"><span data-stu-id="a000a-160">string</span></span> | `aspnet_Users.LoweredUserName` | <span data-ttu-id="a000a-161">cadena</span><span class="sxs-lookup"><span data-stu-id="a000a-161">string</span></span>
|`NormalizedEmail` | <span data-ttu-id="a000a-162">cadena</span><span class="sxs-lookup"><span data-stu-id="a000a-162">string</span></span> | `aspnet_Membership.LoweredEmail` | <span data-ttu-id="a000a-163">cadena</span><span class="sxs-lookup"><span data-stu-id="a000a-163">string</span></span>
|`PhoneNumber` | <span data-ttu-id="a000a-164">cadena</span><span class="sxs-lookup"><span data-stu-id="a000a-164">string</span></span> | `aspnet_Users.MobileAlias` | <span data-ttu-id="a000a-165">cadena</span><span class="sxs-lookup"><span data-stu-id="a000a-165">string</span></span>
|`LockoutEnabled` | <span data-ttu-id="a000a-166">bits</span><span class="sxs-lookup"><span data-stu-id="a000a-166">bit</span></span> | `aspnet_Membership.IsLockedOut` | <span data-ttu-id="a000a-167">bits</span><span class="sxs-lookup"><span data-stu-id="a000a-167">bit</span></span>

> [!NOTE]
> <span data-ttu-id="a000a-168">No todas las asignaciones de campo son similares a las relaciones uno a uno de pertenencia a ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="a000a-168">Not all the field mappings resemble one-to-one relationships from Membership to ASP.NET Core Identity.</span></span> <span data-ttu-id="a000a-169">La tabla anterior toma el esquema de usuario de pertenencia predeterminado y lo asigna al esquema de núcleo de ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="a000a-169">The preceding table takes the default Membership User schema and maps it to the ASP.NET Core Identity schema.</span></span> <span data-ttu-id="a000a-170">Los campos personalizados que se utilizaron para la suscripción es necesario asignar manualmente.</span><span class="sxs-lookup"><span data-stu-id="a000a-170">Any other custom fields that were used for Membership need to be mapped manually.</span></span> <span data-ttu-id="a000a-171">En esta asignación, no hay ninguna asignación para las contraseñas, como criterios de contraseña y Sales de la contraseña no se migran entre los dos.</span><span class="sxs-lookup"><span data-stu-id="a000a-171">In this mapping, there's no map for passwords, as both password criteria and password salts don't migrate between the two.</span></span> <span data-ttu-id="a000a-172">**Se recomienda dejar la contraseña como null y pedir a los usuarios restablecer sus contraseñas.**</span><span class="sxs-lookup"><span data-stu-id="a000a-172">**It's recommended to leave the password as null and to ask users to reset their passwords.**</span></span> <span data-ttu-id="a000a-173">En ASP.NET Core Identity, `LockoutEnd` debe establecerse en una fecha en el futuro si el usuario está bloqueado. Esto se muestra en el script de migración.</span><span class="sxs-lookup"><span data-stu-id="a000a-173">In ASP.NET Core Identity, `LockoutEnd` should be set to some date in the future if the user is locked out. This is shown in the migration script.</span></span>

### <a name="roles"></a><span data-ttu-id="a000a-174">Roles</span><span class="sxs-lookup"><span data-stu-id="a000a-174">Roles</span></span>

| <span data-ttu-id="a000a-175">*Identity(AspNetRoles)*</span><span class="sxs-lookup"><span data-stu-id="a000a-175">*Identity(AspNetRoles)*</span></span> |   | <span data-ttu-id="a000a-176">*Membership(aspnet_Roles)*</span><span class="sxs-lookup"><span data-stu-id="a000a-176">*Membership(aspnet_Roles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="a000a-177">**Nombre de campo**</span><span class="sxs-lookup"><span data-stu-id="a000a-177">**Field Name**</span></span> | <span data-ttu-id="a000a-178">**Type**</span><span class="sxs-lookup"><span data-stu-id="a000a-178">**Type**</span></span>  |   <span data-ttu-id="a000a-179">**Nombre de campo**</span><span class="sxs-lookup"><span data-stu-id="a000a-179">**Field Name**</span></span> | <span data-ttu-id="a000a-180">**Type**</span><span class="sxs-lookup"><span data-stu-id="a000a-180">**Type**</span></span>  |
|`Id` | <span data-ttu-id="a000a-181">cadena</span><span class="sxs-lookup"><span data-stu-id="a000a-181">string</span></span> | `RoleId` | <span data-ttu-id="a000a-182">cadena</span><span class="sxs-lookup"><span data-stu-id="a000a-182">string</span></span>
|`Name` | <span data-ttu-id="a000a-183">cadena</span><span class="sxs-lookup"><span data-stu-id="a000a-183">string</span></span> | `RoleName` | <span data-ttu-id="a000a-184">cadena</span><span class="sxs-lookup"><span data-stu-id="a000a-184">string</span></span>
|`NormalizedName` | <span data-ttu-id="a000a-185">cadena</span><span class="sxs-lookup"><span data-stu-id="a000a-185">string</span></span> | `LoweredRoleName` | <span data-ttu-id="a000a-186">cadena</span><span class="sxs-lookup"><span data-stu-id="a000a-186">string</span></span>

### <a name="user-roles"></a><span data-ttu-id="a000a-187">Roles de usuario</span><span class="sxs-lookup"><span data-stu-id="a000a-187">User Roles</span></span>

| <span data-ttu-id="a000a-188">*Identity(AspNetUserRoles)*</span><span class="sxs-lookup"><span data-stu-id="a000a-188">*Identity(AspNetUserRoles)*</span></span> |   | <span data-ttu-id="a000a-189">*Membership(aspnet_UsersInRoles)*</span><span class="sxs-lookup"><span data-stu-id="a000a-189">*Membership(aspnet_UsersInRoles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="a000a-190">**Nombre de campo**</span><span class="sxs-lookup"><span data-stu-id="a000a-190">**Field Name**</span></span> | <span data-ttu-id="a000a-191">**Type**</span><span class="sxs-lookup"><span data-stu-id="a000a-191">**Type**</span></span>  |   <span data-ttu-id="a000a-192">**Nombre de campo**</span><span class="sxs-lookup"><span data-stu-id="a000a-192">**Field Name**</span></span> | <span data-ttu-id="a000a-193">**Type**</span><span class="sxs-lookup"><span data-stu-id="a000a-193">**Type**</span></span>  |
|`RoleId` | <span data-ttu-id="a000a-194">cadena</span><span class="sxs-lookup"><span data-stu-id="a000a-194">string</span></span> | `RoleId` | <span data-ttu-id="a000a-195">cadena</span><span class="sxs-lookup"><span data-stu-id="a000a-195">string</span></span>
|`UserId` | <span data-ttu-id="a000a-196">cadena</span><span class="sxs-lookup"><span data-stu-id="a000a-196">string</span></span> | `UserId` | <span data-ttu-id="a000a-197">cadena</span><span class="sxs-lookup"><span data-stu-id="a000a-197">string</span></span>

<span data-ttu-id="a000a-198">Hacer referencia a las tablas de asignación anterior al crear un script de migración para *usuarios* y *Roles*.</span><span class="sxs-lookup"><span data-stu-id="a000a-198">Reference the preceding mapping tables when creating a migration script for *Users* and *Roles*.</span></span> <span data-ttu-id="a000a-199">En el siguiente ejemplo se da por supuesto que tiene dos bases de datos en un servidor de base de datos.</span><span class="sxs-lookup"><span data-stu-id="a000a-199">The following example assumes you have two databases on a database server.</span></span> <span data-ttu-id="a000a-200">Una base de datos contiene el esquema de pertenencia a ASP.NET existente y los datos.</span><span class="sxs-lookup"><span data-stu-id="a000a-200">One database contains the existing ASP.NET Membership schema and data.</span></span> <span data-ttu-id="a000a-201">La otra base de datos se creó mediante los pasos descritos anteriormente.</span><span class="sxs-lookup"><span data-stu-id="a000a-201">The other database was created using steps described earlier.</span></span> <span data-ttu-id="a000a-202">Los comentarios son incluido en línea para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="a000a-202">Comments are included inline for more details.</span></span>

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

<span data-ttu-id="a000a-203">Tras finalizar este script, la aplicación de ASP.NET Core Identity que se creó anteriormente se rellena con los usuarios de pertenencia.</span><span class="sxs-lookup"><span data-stu-id="a000a-203">After completion of this script, the ASP.NET Core Identity app created earlier is populated with Membership users.</span></span> <span data-ttu-id="a000a-204">Los usuarios deben cambiar sus contraseñas antes de iniciar sesión.</span><span class="sxs-lookup"><span data-stu-id="a000a-204">Users need to change their passwords before logging in.</span></span>

> [!NOTE]
> <span data-ttu-id="a000a-205">Si el sistema de pertenencia tenía usuarios con nombres de usuario que no coincidía con su dirección de correo electrónico, se requiere para la aplicación que creó anteriormente para dar cabida a este cambio.</span><span class="sxs-lookup"><span data-stu-id="a000a-205">If the Membership system had users with user names that didn't match their email address, changes are required to the app created earlier to accommodate this.</span></span> <span data-ttu-id="a000a-206">La plantilla predeterminada espera `UserName` y `Email` para que coincidan.</span><span class="sxs-lookup"><span data-stu-id="a000a-206">The default template expects `UserName` and `Email` to be the same.</span></span> <span data-ttu-id="a000a-207">En situaciones en las que son diferentes, es necesario modificar para usar el proceso de inicio de sesión `UserName` en lugar de `Email`.</span><span class="sxs-lookup"><span data-stu-id="a000a-207">For situations in which they're different, the login process needs to be modified to use `UserName` instead of `Email`.</span></span>

<span data-ttu-id="a000a-208">En el `PageModel` de la página de inicio de sesión, ubicado en *Pages\Account\Login.cshtml.cs*, quite el `[EmailAddress]` de atributo de la *correo electrónico* propiedad.</span><span class="sxs-lookup"><span data-stu-id="a000a-208">In the `PageModel` of the Login Page, located at *Pages\Account\Login.cshtml.cs*, remove the `[EmailAddress]` attribute from the *Email* property.</span></span> <span data-ttu-id="a000a-209">Cambie su nombre a *nombre de usuario*.</span><span class="sxs-lookup"><span data-stu-id="a000a-209">Rename it to *UserName*.</span></span> <span data-ttu-id="a000a-210">Esto requiere un cambio siempre que sea `EmailAddress` se ha mencionado, en la *vista* y *PageModel*.</span><span class="sxs-lookup"><span data-stu-id="a000a-210">This requires a change wherever `EmailAddress` is mentioned, in the *View* and *PageModel*.</span></span> <span data-ttu-id="a000a-211">El resultado es similar a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a000a-211">The result looks like the following:</span></span>

 ![Inicio de sesión fijo](identity/_static/fixed-login.png)

## <a name="next-steps"></a><span data-ttu-id="a000a-213">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="a000a-213">Next steps</span></span>

<span data-ttu-id="a000a-214">En este tutorial, aprendió cómo trasladar a los usuarios de pertenencia SQL ASP.NET Core 2.0 Identity.</span><span class="sxs-lookup"><span data-stu-id="a000a-214">In this tutorial, you learned how to port users from SQL membership to ASP.NET Core 2.0 Identity.</span></span> <span data-ttu-id="a000a-215">Para obtener más información sobre ASP.NET Core Identity, consulte [Introducción a la identidad](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="a000a-215">For more information regarding ASP.NET Core Identity, see [Introduction to Identity](xref:security/authentication/identity).</span></span>
