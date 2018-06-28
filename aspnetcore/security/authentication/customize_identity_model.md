---
title: Personalización del modelo de identidad
author: ajcvickers
description: En este artículo se describe cómo personalizar el modelo de datos de Entity Framework Core subyacente de ASP.NET Core Identity.
ms.author: avickers
ms.date: 04/12/2018
ms.prod: asp.net-core
uid: security/authentication/customize_identity_model
ms.openlocfilehash: b44b4fd0f24d245b969588a7226ea6aacbe2a722
ms.sourcegitcommit: 7003d27b607e529642ded0400aa48ae692a0e666
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036902"
---
# <a name="identity-model-customization"></a><span data-ttu-id="b4cf2-103">Personalización del modelo de identidad</span><span class="sxs-lookup"><span data-stu-id="b4cf2-103">Identity model customization</span></span>

<span data-ttu-id="b4cf2-104">Por [Arthur Vickers](https://github.com/ajcvickers)</span><span class="sxs-lookup"><span data-stu-id="b4cf2-104">By [Arthur Vickers](https://github.com/ajcvickers)</span></span>

<span data-ttu-id="b4cf2-105">Identidad de ASP.NET Core proporciona un marco para administrar y almacenar las cuentas de usuario en aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-105">ASP.NET Core Identity provides a framework for managing and storing user accounts in ASP.NET Core applications.</span></span> <span data-ttu-id="b4cf2-106">Identidad se agrega al proyecto cuando se selecciona "Cuentas de usuario individuales" como el mecanismo de autenticación.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-106">Identity is added to your project when "Individual User Accounts" is selected as the authentication mechanism.</span></span> <span data-ttu-id="b4cf2-107">De forma predeterminada, identidad hace que use de un Entity Framework (EF) modelo de datos principal.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-107">By default, Identity makes use of an Entity Framework (EF) Core data model.</span></span> <span data-ttu-id="b4cf2-108">En este artículo se describe cómo personalizar el modelo de identidad.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-108">This article describes how to customize the Identity model.</span></span>

<a name="identity-migrations"></a>

## <a name="identity-and-ef-core-migrations"></a><span data-ttu-id="b4cf2-109">Identidad y migraciones de núcleo EF</span><span class="sxs-lookup"><span data-stu-id="b4cf2-109">Identity and EF Core Migrations</span></span>

<span data-ttu-id="b4cf2-110">Antes de examinar el modelo, es útil para comprender cómo funciona la identidad con [EF Core migraciones](/ef/core/managing-schemas/migrations/) para crear y actualizar una base de datos.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-110">Before examining the model, it's useful to understand how Identity works with [EF Core Migrations](/ef/core/managing-schemas/migrations/) to create and update a database.</span></span> <span data-ttu-id="b4cf2-111">En el nivel superior, el proceso es:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-111">At the top level, the process is:</span></span>

1. <span data-ttu-id="b4cf2-112">Definir o actualizar una [modelo de datos en el código](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="b4cf2-112">Define or update a [data model in code](/ef/core/modeling/).</span></span>
1. <span data-ttu-id="b4cf2-113">Agregue una migración para este modelo se traducen en cambios que se pueden aplicar a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-113">Add a migration to translate this model into changes that can be applied to the database.</span></span>
1. <span data-ttu-id="b4cf2-114">Compruebe que la migración representa correctamente sus intenciones.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-114">Check that the migration correctly represents your intentions.</span></span>
1. <span data-ttu-id="b4cf2-115">Aplicar la migración para actualizar la base de datos para que esté sincronizado con el modelo.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-115">Apply the migration to update the database to be in sync with the model.</span></span>
1. <span data-ttu-id="b4cf2-116">Repita los pasos 1 a 4 para perfeccionar el modelo y mantener sincronizada la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-116">Repeat steps 1-4 to further refine the model and keep the database in sync.</span></span>

<span data-ttu-id="b4cf2-117">Las migraciones se agregan y aplican mediante:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-117">Migrations are added and applied using:</span></span>

* <span data-ttu-id="b4cf2-118">El [Package Manager Console](/ef/core/miscellaneous/cli/powershell) (PMC) si está utilizando Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-118">The [Package Manager Console](/ef/core/miscellaneous/cli/powershell) (PMC) if you are using Visual Studio.</span></span>
* <span data-ttu-id="b4cf2-119">El [comandos dotnet](/ef/core/miscellaneous/cli/dotnet) en la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-119">The [dotnet commands](/ef/core/miscellaneous/cli/dotnet) on the command line.</span></span>
* <span data-ttu-id="b4cf2-120">Si selecciona el vínculo de migraciones en la página de error cuando se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-120">Selecting the migrations link on the error page when the application is run.</span></span>

<span data-ttu-id="b4cf2-121">ASP.NET Core tiene un controlador de la página de error de tiempo de desarrollo que puede utilizarse para las migraciones se aplican cuando se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-121">ASP.NET Core has a development-time error page handler that can be used to apply migrations when the application is run.</span></span> <span data-ttu-id="b4cf2-122">Para las aplicaciones de producción, suele ser más adecuado para generar secuencias de comandos SQL a partir de las migraciones e implementarlas en la base de datos como parte de una implementación de aplicación y base de datos controlada.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-122">For production applications, it is often more appropriate to generate SQL scripts from the migrations and deploy these to the database as part of a controlled application and database deployment.</span></span>

<span data-ttu-id="b4cf2-123">Cuando se crea una nueva aplicación de uso de la identidad, ya se han completado los pasos 1 y 2 anteriores.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-123">When a new application using Identity is created, steps 1 and 2 above have already been completed.</span></span> <span data-ttu-id="b4cf2-124">Es decir, el modelo de datos iniciales ya existe, y la migración inicial se ha agregado al proyecto.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-124">That is, the initial data model already exists, and the initial migration has been added to the project.</span></span> <span data-ttu-id="b4cf2-125">La migración inicial todavía debe aplicarse a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-125">The initial migration still needs to be applied to the database.</span></span> <span data-ttu-id="b4cf2-126">La migración inicial puede realizarse mediante la ejecución de la base de datos de actualización (PMC), el comando de actualización (.NET Core CLI) de base de datos de ef dotnet, o mediante la página de error cuando se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-126">The initial migration can be done by running the Update-Database (PMC), the dotnet ef database update (.NET Core CLI) command, or by using the error page when the application is run.</span></span>

<span data-ttu-id="b4cf2-127">Los pasos anteriores que deba repetir cuando se realizan cambios en el modelo.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-127">The preceding steps need to be repeated as changes are made to the model.</span></span>

<a name="identity-model"></a>

## <a name="the-identity-model"></a><span data-ttu-id="b4cf2-128">El modelo de identidad</span><span class="sxs-lookup"><span data-stu-id="b4cf2-128">The Identity model</span></span>

### <a name="entity-types"></a><span data-ttu-id="b4cf2-129">Tipos de entidad</span><span class="sxs-lookup"><span data-stu-id="b4cf2-129">Entity types</span></span>

<span data-ttu-id="b4cf2-130">El modelo de identidad consta de siete tipos de entidad:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-130">The Identity model consists of seven entity types:</span></span>

* <span data-ttu-id="b4cf2-131">`User` -representa al usuario</span><span class="sxs-lookup"><span data-stu-id="b4cf2-131">`User` - represents the user</span></span>
* <span data-ttu-id="b4cf2-132">`Role` -Representa un rol</span><span class="sxs-lookup"><span data-stu-id="b4cf2-132">`Role` - represents a role</span></span>
* <span data-ttu-id="b4cf2-133">`UserClaim` -Representa una notificación que el usuario posea</span><span class="sxs-lookup"><span data-stu-id="b4cf2-133">`UserClaim` - represents a claim that a user possess</span></span>
* <span data-ttu-id="b4cf2-134">`UserToken` -Representa un token de autenticación para un usuario</span><span class="sxs-lookup"><span data-stu-id="b4cf2-134">`UserToken` - represents an authentication token for a user</span></span>
* <span data-ttu-id="b4cf2-135">`UserLogin` -asocia un usuario a un inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="b4cf2-135">`UserLogin` - associates a user with a login</span></span>
* <span data-ttu-id="b4cf2-136">`RoleClaim` -Representa una notificación que se concede a todos los usuarios dentro de un rol</span><span class="sxs-lookup"><span data-stu-id="b4cf2-136">`RoleClaim` - represents a claim that is granted to all users within a role</span></span>
* <span data-ttu-id="b4cf2-137">`UserRole` -unirse a la entidad que se asocia a los usuarios y roles</span><span class="sxs-lookup"><span data-stu-id="b4cf2-137">`UserRole` - join entity that associates users and roles</span></span>

### <a name="entity-type-relationships"></a><span data-ttu-id="b4cf2-138">Relaciones de tipo de entidad</span><span class="sxs-lookup"><span data-stu-id="b4cf2-138">Entity type relationships</span></span>

<span data-ttu-id="b4cf2-139">Estos tipos de entidad están relacionados entre sí de las maneras siguientes:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-139">These entity types are related to each other in the following ways:</span></span>

* <span data-ttu-id="b4cf2-140">Cada `User` puede tener muchos `UserClaims`</span><span class="sxs-lookup"><span data-stu-id="b4cf2-140">Each `User` can have many `UserClaims`</span></span>
* <span data-ttu-id="b4cf2-141">Cada `User` puede tener muchos `UserLogins`</span><span class="sxs-lookup"><span data-stu-id="b4cf2-141">Each `User` can have many `UserLogins`</span></span>
* <span data-ttu-id="b4cf2-142">Cada `User` puede tener muchos `UserTokens`</span><span class="sxs-lookup"><span data-stu-id="b4cf2-142">Each `User` can have many `UserTokens`</span></span>
* <span data-ttu-id="b4cf2-143">Cada `Role` puede tener muchos asociados `RoleClaims`</span><span class="sxs-lookup"><span data-stu-id="b4cf2-143">Each `Role` can have many associated `RoleClaims`</span></span>
* <span data-ttu-id="b4cf2-144">Cada `User` puede tener muchos asociados `Roles`y cada `Role` pueden asociarse con muchos usuarios</span><span class="sxs-lookup"><span data-stu-id="b4cf2-144">Each `User` can have many associated `Roles`, and each `Role` can be associated with many Users</span></span>
  * <span data-ttu-id="b4cf2-145">Se trata de una relación de varios a varios, lo que requiere una tabla de combinación en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-145">This is a many-to-many relationship, which requires a join table in the database.</span></span> <span data-ttu-id="b4cf2-146">La tabla de combinación se representa mediante el `UserRole` entidad.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-146">The join table is represented by the `UserRole` entity.</span></span>

### <a name="default-model-configuration"></a><span data-ttu-id="b4cf2-147">Configuración del modelo predeterminado</span><span class="sxs-lookup"><span data-stu-id="b4cf2-147">Default model configuration</span></span>

<span data-ttu-id="b4cf2-148">Identidad define una variedad de "clases de contexto", que se heredan de [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) para configurar y utilizar el modelo.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-148">Identity defines a variety of "context classes" which inherit from [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) to configure and use the model.</span></span> <span data-ttu-id="b4cf2-149">Esta configuración se realiza mediante la [API fluida de la primera de código EF Core](/ef/core/modeling/) en el [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) método de la clase de contexto.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-149">This configuration is done using the [EF Core Code First Fluent API](/ef/core/modeling/) in the [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) method of the context class.</span></span> <span data-ttu-id="b4cf2-150">La configuración predeterminada es:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-150">The default configuration is:</span></span>

```CSharp
builder.Entity<TUser>(b =>
{
    // Primary key
    b.HasKey(u => u.Id);

    // Indexes for "normalized" username and email, to allow efficient lookups
    b.HasIndex(u => u.NormalizedUserName).HasName("UserNameIndex").IsUnique();
    b.HasIndex(u => u.NormalizedEmail).HasName("EmailIndex");

    // Maps to the AspNetUsers table
    b.ToTable("AspNetUsers");

    // A concurrency token for use with the optimistic concurrency checking
    b.Property(u => u.ConcurrencyStamp).IsConcurrencyToken();

    // Limit the size of columns to use efficient database types
    b.Property(u => u.UserName).HasMaxLength(256);
    b.Property(u => u.NormalizedUserName).HasMaxLength(256);
    b.Property(u => u.Email).HasMaxLength(256);
    b.Property(u => u.NormalizedEmail).HasMaxLength(256);

    // The relationships between User and other entity types
    // Note that these relationships are configured with no navigation properties

    // Each User can have many UserClaims
    b.HasMany<TUserClaim>().WithOne().HasForeignKey(uc => uc.UserId).IsRequired();

    // Each User can have many UserLogins
    b.HasMany<TUserLogin>().WithOne().HasForeignKey(ul => ul.UserId).IsRequired();

    // Each User can have many UserTokens
    b.HasMany<TUserToken>().WithOne().HasForeignKey(ut => ut.UserId).IsRequired();

    // Each User can have many entries in the UserRole join table
    b.HasMany<TUserRole>().WithOne().HasForeignKey(ur => ur.UserId).IsRequired();
});

builder.Entity<TUserClaim>(b =>
{
    // Primary key
    b.HasKey(uc => uc.Id);

    // Maps to the AspNetUserClaims table
    b.ToTable("AspNetUserClaims");
});

builder.Entity<TUserLogin>(b =>
{
    // Composite primary key consisting of the LoginProvider and the key to use
    // with that provider
    b.HasKey(l => new { l.LoginProvider, l.ProviderKey });

    // Limit the size of the composite key columns due to common database restrictions
    b.Property(l => l.LoginProvider).HasMaxLength(128);
    b.Property(l => l.ProviderKey).HasMaxLength(128);

    // Maps to the AspNetUserLogins table
    b.ToTable("AspNetUserLogins");
});

builder.Entity<TUserToken>(b =>
{
    // Composite primary key consisting of the UserId, LoginProvider and Name
    b.HasKey(t => new { t.UserId, t.LoginProvider, t.Name });

    // Limit the size of the composite key columns due to common database restrictions
    b.Property(t => t.LoginProvider).HasMaxLength(maxKeyLength);
    b.Property(t => t.Name).HasMaxLength(maxKeyLength);

    // Maps to the AspNetUserTokens table
    b.ToTable("AspNetUserTokens");
});

builder.Entity<TRole>(b =>
{
    // Primary key
    b.HasKey(r => r.Id);

    // Index for "normalized" role name to allow efficient lookups
    b.HasIndex(r => r.NormalizedName).HasName("RoleNameIndex").IsUnique();

    // Maps to the AspNetRoles table
    b.ToTable("AspNetRoles");

    // A concurrency token for use with the optimistic concurrency checking
    b.Property(r => r.ConcurrencyStamp).IsConcurrencyToken();

    // Limit the size of columns to use efficient database types
    b.Property(u => u.Name).HasMaxLength(256);
    b.Property(u => u.NormalizedName).HasMaxLength(256);

    // The relationships between Role and other entity types
    // Note that these relationships are configured with no navigation properties

    // Each Role can have many entries in the UserRole join table
    b.HasMany<TUserRole>().WithOne().HasForeignKey(ur => ur.RoleId).IsRequired();

    // Each Role can have many associated RoleClaims
    b.HasMany<TRoleClaim>().WithOne().HasForeignKey(rc => rc.RoleId).IsRequired();
});

builder.Entity<TRoleClaim>(b =>
{
    // Primary key
    b.HasKey(rc => rc.Id);

    // Maps to the AspNetRoleClaims table
    b.ToTable("AspNetRoleClaims");
});

builder.Entity<TUserRole>(b =>
{
    // Primary key
    b.HasKey(r => new { r.UserId, r.RoleId });

    // Maps to the AspNetUserRoles table
    b.ToTable("AspNetUserRoles");
});
```

### <a name="model-generic-types"></a><span data-ttu-id="b4cf2-151">Tipos genéricos de modelo</span><span class="sxs-lookup"><span data-stu-id="b4cf2-151">Model generic types</span></span>

<span data-ttu-id="b4cf2-152">Identidad define los tipos CLR predeterminada para cada uno de los tipos de entidad mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-152">Identity defines default CLR types for each of the entity types listed above.</span></span> <span data-ttu-id="b4cf2-153">Todos estos tipos tienen el prefijo con "Identidad": `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, y `IdentityUserRole`.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-153">These types are all prefixed with "Identity": `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, and `IdentityUserRole`.</span></span>

<span data-ttu-id="b4cf2-154">En lugar de usar directamente estos tipos, los tipos pueden usarse como clases base para los tipos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-154">Rather than using these types directly, the types can be used as base classes for the application's own types.</span></span> <span data-ttu-id="b4cf2-155">El `DbContext` clases definidas por identidad establecen genéricas que diferentes tipos CLR pueden utilizarse para uno o varios de los tipos de entidad en el modelo.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-155">The `DbContext` classes defined by Identity are generic such that different CLR types can be used for one or more of the entity types in the model.</span></span> <span data-ttu-id="b4cf2-156">Estos tipos genéricos también permiten para el tipo de la clave principal de usuario va a cambiar.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-156">These generic types also allow for the type of the User primary key to be changed.</span></span>

<span data-ttu-id="b4cf2-157">Cuando se usa la identidad con compatibilidad para los roles, una [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) debe utilizarse la clase:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-157">When using Identity with support for roles, an [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) class should be used:</span></span>

```CSharp
// Uses all the built-in Identity types
// Uses `string` as the key type
public class IdentityDbContext
    : IdentityDbContext<IdentityUser, IdentityRole, string>
{
}

// Uses the built-in Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityDbContext<TUser>
    : IdentityDbContext<TUser, IdentityRole, string>
        where TUser : IdentityUser
{
}

// Uses the built-in Identity types except with custom User and Role types
// The key type is defined by TKey
public class IdentityDbContext<TUser, TRole, TKey>
    : IdentityDbContext<TUser, TRole, TKey, IdentityUserClaim<TKey>, IdentityUserRole<TKey>, IdentityUserLogin<TKey>, IdentityRoleClaim<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TRole : IdentityRole<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments
// The key type is defined by TKey
public abstract class IdentityDbContext<TUser, TRole, TKey, TUserClaim, TUserRole, TUserLogin, TRoleClaim, TUserToken>
    : IdentityUserContext<TUser, TKey, TUserClaim, TUserLogin, TUserToken>
         where TUser : IdentityUser<TKey>
         where TRole : IdentityRole<TKey>
         where TKey : IEquatable<TKey>
         where TUserClaim : IdentityUserClaim<TKey>
         where TUserRole : IdentityUserRole<TKey>
         where TUserLogin : IdentityUserLogin<TKey>
         where TRoleClaim : IdentityRoleClaim<TKey>
         where TUserToken : IdentityUserToken<TKey>

```

<span data-ttu-id="b4cf2-158">También es posible usar identidad sin roles (solo las notificaciones), en cuyo caso una `IdentityUserContext` debe utilizarse la clase:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-158">It is also possible to use Identity without roles (only claims), in which case an `IdentityUserContext` class should be used:</span></span>


```CSharp
// Uses the built-in non-role Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityUserContext<TUser>
    : IdentityUserContext<TUser, string>
        where TUser : IdentityUser
{
}

// Uses the built-in non-role Identity types except with a custom User type
// The key type is defined by TKey
public class IdentityUserContext<TUser, TKey>
    : IdentityUserContext<TUser, TKey, IdentityUserClaim<TKey>, IdentityUserLogin<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments, with no roles
// The key type is defined by TKey
public abstract class IdentityUserContext<TUser, TKey, TUserClaim, TUserLogin, TUserToken>
    : DbContext
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
        where TUserClaim : IdentityUserClaim<TKey>
        where TUserLogin : IdentityUserLogin<TKey>
        where TUserToken : IdentityUserToken<TKey>
{
}
```

## <a name="customizing-the-model"></a><span data-ttu-id="b4cf2-159">Personalizar el modelo</span><span class="sxs-lookup"><span data-stu-id="b4cf2-159">Customizing the model</span></span>

<span data-ttu-id="b4cf2-160">El punto de partida para personalizar el modelo es derivar del tipo de contexto adecuado; Consulte la sección anterior.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-160">The starting point for customizing the model is to derive from the appropriate context type; see the preceding section.</span></span> <span data-ttu-id="b4cf2-161">Este tipo de contexto habitualmente se denomina `ApplicationDbContext` y se crea mediante las plantillas de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-161">This context type is customarily called `ApplicationDbContext` and is created by the ASP.NET Core templates.</span></span>

<span data-ttu-id="b4cf2-162">El contexto se utiliza para configurar el modelo de dos maneras:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-162">The context is used to configure the model in two ways:</span></span>
* <span data-ttu-id="b4cf2-163">Proporcionando los tipos de entidad y la clave para los parámetros de tipo genérico.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-163">By supplying entity and key types for the generic type parameters.</span></span>
* <span data-ttu-id="b4cf2-164">Mediante la invalidación `OnModelCreating` para modificar la asignación de estos tipos.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-164">By overriding `OnModelCreating` to modify the mapping of these types.</span></span>

<span data-ttu-id="b4cf2-165">Al reemplazar `OnModelCreating`, `base.OnModelCreating` debe llamarse en primer lugar, el reemplazo configuración debe llamarse a continuación.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-165">When overriding `OnModelCreating`, `base.OnModelCreating` should be called first, the overriding configuration should be called next.</span></span> <span data-ttu-id="b4cf2-166">EF Core generalmente tiene una directiva de última-uno-wins para la configuración.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-166">EF Core generally has a last-one-wins policy for configuration.</span></span> <span data-ttu-id="b4cf2-167">Por ejemplo, si la `ToTable` de una entidad es tipo llamado primero con el nombre de una tabla y, a continuación, vuelva a versiones posteriores con un nombre diferente, a continuación, el nombre de tabla en la segunda llamada es la que se utiliza.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-167">For example, if the `ToTable` for an entity type is called first with one table name and then again later with a different table name, then the table name in the second call is the one that is used.</span></span>

### <a name="using-a-custom-user-type"></a><span data-ttu-id="b4cf2-168">Uso de un tipo de usuario personalizado</span><span class="sxs-lookup"><span data-stu-id="b4cf2-168">Using a custom User type</span></span>

<span data-ttu-id="b4cf2-169">Para usar un tipo de usuario personalizado, crea el tipo y hacer que hereden de `IdentityUser`.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-169">To use a custom User type, create the type and have it inherit from `IdentityUser`.</span></span> <span data-ttu-id="b4cf2-170">Es habitual para este tipo de nombre `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-170">It is customary to name this type `ApplicationUser`.</span></span> <span data-ttu-id="b4cf2-171">Este tipo normalmente tendrán propiedades adicionales no en el tipo base, en caso contrario, no habría ningún valor para su creación.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-171">This type will typically have additional properties not in the base type, otherwise there would be no value in creating it.</span></span> <span data-ttu-id="b4cf2-172">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-172">For example:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

<span data-ttu-id="b4cf2-173">A continuación, use este tipo como argumento genérico para el contexto:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-173">Next use this type as a generic argument for the context:</span></span>

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

<span data-ttu-id="b4cf2-174">Actualización `ConfigureServices` para utilizar el nuevo `ApplicationUser` clase:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-174">Update `ConfigureServices` to use the new `ApplicationUser` class:</span></span>

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="b4cf2-175">No es necesario invalidar `OnModelCreating` aquí porque EF Core asignará el `CustomTag` propiedad por convención.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-175">There is no need to override `OnModelCreating` here because EF Core will map the `CustomTag` property by convention.</span></span> <span data-ttu-id="b4cf2-176">Sin embargo, tendrá la base de datos se actualicen para obtener una nueva `CustomTag` columna.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-176">However, the database will need to be updated to get a new `CustomTag` column.</span></span> <span data-ttu-id="b4cf2-177">Para ello, agregue una migración y, a continuación, actualice la base de datos como se describe en [identidad y migraciones de núcleo de EF](#identity-migrations).</span><span class="sxs-lookup"><span data-stu-id="b4cf2-177">To do this, add a migration, and then update the database as described in  [Identity and EF Core Migrations](#identity-migrations).</span></span>

### <a name="changing-the-key-type"></a><span data-ttu-id="b4cf2-178">Cambiar el tipo de clave</span><span class="sxs-lookup"><span data-stu-id="b4cf2-178">Changing the key type</span></span>

<span data-ttu-id="b4cf2-179">Cambiar el tipo de una columna de clave principal (PK) una vez creada la base de datos es problemático en muchos sistemas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-179">Changing the type of a primary key (PK) column after the database has been created is problematic on many database systems.</span></span> <span data-ttu-id="b4cf2-180">Cambiar la clave principal normalmente hay que quitar y volver a crear la tabla.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-180">Changing the PK typically involves dropping and re-creating the table.</span></span> <span data-ttu-id="b4cf2-181">Por lo tanto, se recomienda que los tipos de clave se especifiquen en la migración inicial tal que los tipos de clave de destino se crean cuando se crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-181">Therefore, it is recommended that key types be specified in the initial migration such that the target key types are created when the database is created.</span></span>

<span data-ttu-id="b4cf2-182">Si se ha creado la base de datos, a continuación, `Drop-Database` (PMC) o `dotnet ef database drop` (.NET Core CLI) puede usarse para eliminarlo.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-182">If the database has been created, then `Drop-Database` (PMC) or `dotnet ef database drop` (.NET Core CLI) can be used to delete it.</span></span>

<span data-ttu-id="b4cf2-183">Una vez que se confirma que la base de datos no existe, quite la migración inicial con `Remove-Migration` (PMC) o `dotnet ef migrations remove` (.NET Core CLI).</span><span class="sxs-lookup"><span data-stu-id="b4cf2-183">Once it is confirmed that the database doesn't exist,  remove the initial migration with `Remove-Migration` (PMC) or `dotnet ef migrations remove` (.NET Core CLI).</span></span>

<span data-ttu-id="b4cf2-184">Actualización de la `ApplicationDbContext` para utilizar una clase base diferente, especificando el nuevo tipo de clave para `TKey`.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-184">Update the `ApplicationDbContext` to use a different base class, specifying the new key type for `TKey`.</span></span> <span data-ttu-id="b4cf2-185">Por ejemplo, para utilizar un `Guid` clave:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-185">For example, to use a `Guid` key:</span></span>

```CSharp
public class ApplicationDbContext : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

<span data-ttu-id="b4cf2-186">Tenga en cuenta que las clases genéricas `IdentityUser<TKey>` y `IdentityRole<TKey>` también debe especificarse para usar el nuevo tipo de clave.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-186">Notice that the generic classes `IdentityUser<TKey>` and `IdentityRole<TKey>` must also be specified to use the new key type.</span></span> <span data-ttu-id="b4cf2-187">`ConfigureServices` se deben actualizar para usar el usuario genérico:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-187">`ConfigureServices` must be updated to use the generic user:</span></span>

```CSharp
services.AddDefaultIdentity<IdentityUser<Guid>>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="b4cf2-188">Si un personalizado `ApplicationUser` está utilizándose, actualizarlo para heredar de `IdentityUser<TKey>`.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-188">If a custom `ApplicationUser` is being used, update it to inherit from `IdentityUser<TKey>`.</span></span> <span data-ttu-id="b4cf2-189">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-189">For example:</span></span>

```CSharp
public class ApplicationUser : IdentityUser<Guid>
{
    public string CustomTag { get; set; }
}

public class ApplicationDbContext : IdentityDbContext<ApplicationUser, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

### <a name="adding-navigation-properties"></a><span data-ttu-id="b4cf2-190">Agregar propiedades de navegación</span><span class="sxs-lookup"><span data-stu-id="b4cf2-190">Adding navigation properties</span></span>

<span data-ttu-id="b4cf2-191">Cambiar la configuración del modelo para las relaciones puede ser un más difícil que realizar otros cambios.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-191">Changing the model configuration for relationships can be a more difficult than making other changes.</span></span> <span data-ttu-id="b4cf2-192">Se debe tener cuidado para reemplazar las relaciones existentes en lugar de crear una nueva relación adicional.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-192">Care must be taken to replace the existing relationships rather than create a new additional relationships.</span></span> <span data-ttu-id="b4cf2-193">En concreto, la relación modificada debe especificar la misma propiedad de clave externa como la relación existente.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-193">In particular, the changed relationship must specify the same foreign key property as the existing relationship.</span></span> <span data-ttu-id="b4cf2-194">Por ejemplo, la relación entre `Users` y `UserClaims` es por valor predeterminado especificado como:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-194">For example, the relationship between `Users` and `UserClaims` is by default specified as:</span></span>

```CSharp
builder.Entity<TUser>(b =>
{
    // Each User can have many UserClaims
    b.HasMany<TUserClaim>()
     .WithOne()
     .HasForeignKey(uc => uc.UserId)
     .IsRequired();
});
```

<span data-ttu-id="b4cf2-195">La clave externa de esta relación se especifica como el `UserClaim.UserId` propiedad.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-195">The foreign key for this relationship is specified as the `UserClaim.UserId` property.</span></span> <span data-ttu-id="b4cf2-196">`HasMany` y `WithOne` se llama sin argumentos para crear la relación sin propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-196">`HasMany` and `WithOne` are called without arguments to create the relationship without navigation properties.</span></span>

<span data-ttu-id="b4cf2-197">Agregar una propiedad de navegación para `ApplicationUser` que le permitirá asociados `UserClaims` para hacer referencia desde el usuario:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-197">Add a navigation property to `ApplicationUser` that will allow associated `UserClaims` to be referenced from the user:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

<span data-ttu-id="b4cf2-198">Tenga en cuenta que la `TKey` para `IdentityUserClaim<TKey>` es el tipo especificado para la clave principal de los usuarios, en este caso `string` puesto que estamos usando los valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-198">Note that the `TKey` for `IdentityUserClaim<TKey>` is type specified for the primary key of users--in this case `string` since we're using the defaults.</span></span> <span data-ttu-id="b4cf2-199">Es **no** el tipo de clave principal para el `UserClaim` tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-199">It is **not** the primary key type for the `UserClaim` entity type.</span></span>

<span data-ttu-id="b4cf2-200">Ahora que existe la propiedad de navegación deben configurarse en `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-200">Now that the navigation property exists it must be configured in `OnModelCreating`:</span></span>

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();
        });
    }
}
```

<span data-ttu-id="b4cf2-201">Tenga en cuenta que la relación está configurada exactamente igual que antes, solo con una propiedad de navegación especificada en la llamada a `HasMany`.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-201">Notice that relationship is configured exactly as it was before, only with a navigation property specified in the call to `HasMany`.</span></span>

<span data-ttu-id="b4cf2-202">Las propiedades de navegación solo existen en el modelo EF, no en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-202">The navigation properties only exist in the EF model, not the database.</span></span> <span data-ttu-id="b4cf2-203">Dado que no ha cambiado la clave externa de la relación, este tipo de cambio en el modelo no requiere la base de datos se actualicen.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-203">Because the foreign key for the relationship has not changed, this kind of model change does not require the database to be updated.</span></span> <span data-ttu-id="b4cf2-204">Esto se puede comprobar mediante la adición de una migración después de realizar el cambio: el `Up` y `Down` métodos están vacíos.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-204">This can be checked by adding a migration after making the change: the `Up` and `Down` methods are empty.</span></span>

### <a name="adding-all-user-navigation-properties"></a><span data-ttu-id="b4cf2-205">Agregar todas las propiedades de navegación de usuario</span><span class="sxs-lookup"><span data-stu-id="b4cf2-205">Adding all User navigation properties</span></span>

<span data-ttu-id="b4cf2-206">Con la sección anterior como guía, mostramos un ejemplo que configura las propiedades de navegación unidireccional para todas las relaciones de usuario:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-206">Using the section above as guidance, here is an example that configures unidirectional navigation properties for all relationships on User:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<IdentityUserRole<string>> UserRoles { get; set; }
}
```

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne()
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne()
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne()
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });
    }
}
```

### <a name="adding-user-and-role-navigation-properties"></a><span data-ttu-id="b4cf2-207">Agregar propiedades de navegación de usuario y el rol</span><span class="sxs-lookup"><span data-stu-id="b4cf2-207">Adding User and Role navigation properties</span></span>

<span data-ttu-id="b4cf2-208">Con la sección anterior como guía, mostramos un ejemplo que configura las propiedades de navegación para todas las relaciones de usuario y el rol:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-208">Using the section above as guidance, here is an example that configures navigation properties for all relationships on User and Role:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationRole : IdentityRole
{
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationUserRole : IdentityUserRole<string>
{
    public virtual ApplicationUser User { get; set; }
    public virtual ApplicationRole Role { get; set; }
}
```

```CSharp
public class ApplicationDbContext
    : IdentityDbContext<
        ApplicationUser, ApplicationRole, string,
        IdentityUserClaim<string>, ApplicationUserRole, IdentityUserLogin<string>,
        IdentityRoleClaim<string>, IdentityUserToken<string>>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne()
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne()
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.User)
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });

        modelBuilder.Entity<ApplicationRole>(b =>
        {
            // Each Role can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.Role)
                .HasForeignKey(ur => ur.RoleId)
                .IsRequired();
        });

    }
}
```

<span data-ttu-id="b4cf2-209">Notas:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-209">Notes:</span></span>

* <span data-ttu-id="b4cf2-210">En este ejemplo también incluye el `UserRole` unirse a la entidad, que se necesita para navegar por la relación de varios a varios de los usuarios a Roles.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-210">This example also includes the `UserRole` join entity, which is needed to navigate the many-to-many relationship from Users to Roles.</span></span>
* <span data-ttu-id="b4cf2-211">No olvide cambiar los tipos de las propiedades de navegación para reflejar que `ApplicationXxx` tipos ahora se utilizan en lugar de `IdentityXxx` tipos.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-211">Remember to change the types of the navigation properties to reflect that `ApplicationXxx` types are now being used instead of `IdentityXxx` types.</span></span>
* <span data-ttu-id="b4cf2-212">Recuerde que debe usar el `ApplicationXxx` en la interfaz genérica `ApplicationContext` definición.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-212">Remember to use the `ApplicationXxx` in the generic `ApplicationContext` definition.</span></span>

### <a name="adding-all-navigation-properties"></a><span data-ttu-id="b4cf2-213">Agregar todas las propiedades de navegación</span><span class="sxs-lookup"><span data-stu-id="b4cf2-213">Adding all navigation properties</span></span>

<span data-ttu-id="b4cf2-214">Con la sección anterior como guía, mostramos un ejemplo que configura las propiedades de navegación para todas las relaciones en todos los tipos de entidad:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-214">Using the section above as guidance, here is an example that configures navigation properties for all relationships on all entity types:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<ApplicationUserClaim> Claims { get; set; }
    public virtual ICollection<ApplicationUserLogin> Logins { get; set; }
    public virtual ICollection<ApplicationUserToken> Tokens { get; set; }
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationRole : IdentityRole
{
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
    public virtual ICollection<ApplicationRoleClaim> RoleClaims { get; set; }
}

public class ApplicationUserRole : IdentityUserRole<string>
{
    public virtual ApplicationUser User { get; set; }
    public virtual ApplicationRole Role { get; set; }
}

public class ApplicationUserClaim : IdentityUserClaim<string>
{
    public virtual ApplicationUser User { get; set; }
}

public class ApplicationUserLogin : IdentityUserLogin<string>
{
    public virtual ApplicationUser User { get; set; }
}

public class ApplicationRoleClaim : IdentityRoleClaim<string>
{
    public virtual ApplicationRole Role { get; set; }
}

public class ApplicationUserToken : IdentityUserToken<string>
{
    public virtual ApplicationUser User { get; set; }
}
```

```CSharp
public class ApplicationDbContext
    : IdentityDbContext<
        ApplicationUser, ApplicationRole, string,
        ApplicationUserClaim, ApplicationUserRole, ApplicationUserLogin,
        ApplicationRoleClaim, ApplicationUserToken>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne(e => e.User)
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne(e => e.User)
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne(e => e.User)
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.User)
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });

        modelBuilder.Entity<ApplicationRole>(b =>
        {
            // Each Role can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.Role)
                .HasForeignKey(ur => ur.RoleId)
                .IsRequired();

            // Each Role can have many associated RoleClaims
            b.HasMany(e => e.RoleClaims)
                .WithOne(e => e.Role)
                .HasForeignKey(rc => rc.RoleId)
                .IsRequired();
        });

    }
}
```

### <a name="using-composite-keys"></a><span data-ttu-id="b4cf2-215">Usando claves compuestas</span><span class="sxs-lookup"><span data-stu-id="b4cf2-215">Using composite keys</span></span>

<span data-ttu-id="b4cf2-216">En las secciones anteriores se muestran el cambio del tipo de clave que se usa en el modelo de identidad.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-216">The preceding sections demonstrated changing the type of key used in the Identity model.</span></span> <span data-ttu-id="b4cf2-217">No es compatible ni recomendable cambiar el modelo de clave de identidad para utilizar las claves compuestas.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-217">Changing the Identity key model to use composite keys is not supported or recommended.</span></span> <span data-ttu-id="b4cf2-218">Usar una clave compuesta con identidad implicaría cambiar cómo el código del Administrador de identidades interactúa con el modelo, que es una personalización no compatible y fuera del ámbito de este documento.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-218">Using a composite key with Identity would involve changing how the Identity manager code interacts with the model, which is an unsupported customization and beyond the scope of this document.</span></span>

### <a name="changing-tablecolumn-names-and-facets"></a><span data-ttu-id="b4cf2-219">Cambiar nombres de tabla o columna y las facetas</span><span class="sxs-lookup"><span data-stu-id="b4cf2-219">Changing table/column names and facets</span></span>

<span data-ttu-id="b4cf2-220">Para cambiar los nombres de tablas y columnas, llame a `base.OnModelCreating`y, a continuación, agregue una configuración para reemplazar cualquiera de los valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-220">To change the names of tables and columns, call `base.OnModelCreating`, and then add configuration to override any of the defaults.</span></span> <span data-ttu-id="b4cf2-221">Por ejemplo, para cambiar el nombre de todas las tablas de identidad:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-221">For example, to change the name of all the identity tables:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.ToTable("MyUsers");
    });

    modelBuilder.Entity<IdentityUserClaim<string>>(b =>
    {
        b.ToTable("MyUserClaims");
    });

    modelBuilder.Entity<IdentityUserLogin<string>>(b =>
    {
        b.ToTable("MyUserLogins");
    });

    modelBuilder.Entity<IdentityUserToken<string>>(b =>
    {
        b.ToTable("MyUserTokens");
    });

    modelBuilder.Entity<IdentityRole>(b =>
    {
        b.ToTable("MyRoles");
    });

    modelBuilder.Entity<IdentityRoleClaim<string>>(b =>
    {
        b.ToTable("MyRoleClaims");
    });

    modelBuilder.Entity<IdentityUserRole<string>>(b =>
    {
        b.ToTable("MyUserRoles");
    });
}
```

<span data-ttu-id="b4cf2-222">Tenga en cuenta que estos ejemplos utilizan los tipos de identidad predeterminada.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-222">Note that these examples use the default Identity types.</span></span> <span data-ttu-id="b4cf2-223">Si está utilizando un tipo de aplicación, como `ApplicationUser` , a continuación, configurar ese tipo en lugar del tipo de valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-223">If you are using an application type, such as `ApplicationUser` then configure that type instead of the default type.</span></span>

<span data-ttu-id="b4cf2-224">Este es un ejemplo que cambia algunos nombres de columna:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-224">Here is an example that changes some column names:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.Property(e => e.Email).HasColumnName("EMail");
    });

    modelBuilder.Entity<IdentityUserClaim<string>>(b =>
    {
        b.Property(e => e.ClaimType).HasColumnName("CType");
        b.Property(e => e.ClaimValue).HasColumnName("CValue");
    });
}
```

<span data-ttu-id="b4cf2-225">Algunos tipos de columnas de base de datos pueden configurarse con determinados _facetas_ como la longitud de cadena máxima permitida.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-225">Some types of database columns can be configured with certain _facets_ such as the maximum string length allowed.</span></span> <span data-ttu-id="b4cf2-226">Este es un ejemplo que establece max longitudes de columna para varias propiedades de cadena en el modelo:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-226">Here is an example that sets column max lengths for several string properties in the model:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.Property(u => u.UserName).HasMaxLength(128);
        b.Property(u => u.NormalizedUserName).HasMaxLength(128);
        b.Property(u => u.Email).HasMaxLength(128);
        b.Property(u => u.NormalizedEmail).HasMaxLength(128);
    });

    modelBuilder.Entity<IdentityUserToken<string>>(b =>
    {
        b.Property(t => t.LoginProvider).HasMaxLength(128);
        b.Property(t => t.Name).HasMaxLength(128);
    });
}
```

### <a name="mapping-to-a-different-schema"></a><span data-ttu-id="b4cf2-227">Asignación a un esquema diferente</span><span class="sxs-lookup"><span data-stu-id="b4cf2-227">Mapping to a different schema</span></span>

<span data-ttu-id="b4cf2-228">Los esquemas pueden comportarse de manera diferente en los proveedores de base de datos diferente, pero para SQL Server, el valor predeterminado es crear todas las tablas en el esquema "dbo".</span><span class="sxs-lookup"><span data-stu-id="b4cf2-228">Schemas can behave differently in different database providers, but for SQL Server, the default is to create all tables in the "dbo" schema.</span></span> <span data-ttu-id="b4cf2-229">Esto se puede cambiar para crear en su lugar las tablas en un esquema diferente.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-229">This can be changed to instead create the tables in a different schema.</span></span> <span data-ttu-id="b4cf2-230">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-230">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

### <a name="lazy-loading"></a><span data-ttu-id="b4cf2-231">Carga diferida</span><span class="sxs-lookup"><span data-stu-id="b4cf2-231">Lazy loading</span></span>

<span data-ttu-id="b4cf2-232">En esta sección se agrega compatibilidad con servidores proxy de carga diferida en el modelo de identidad.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-232">In this section support for lazy-loading proxies in the Identity model is added.</span></span> <span data-ttu-id="b4cf2-233">Lazy carga es útil porque permite que las propiedades de navegación que se utilizarán sin garantizar la primera que se cargan.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-233">Lazy-loading is useful since it allows navigation properties to be used without first ensuring they are loaded.</span></span>

<span data-ttu-id="b4cf2-234">Tipos de entidad pueden realizarse adecuados para diferida de carga de varias maneras, como se describe en el [documentación principal de EF](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="b4cf2-234">Entity types can be made suitable for lazy-loading in several ways, as described in the [EF Core documentation](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="b4cf2-235">Por simplicidad, utilizaremos a servidores proxy de la carga diferida, lo que requiere:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-235">For simplicity, we will use lazy-loading proxies, which requires:</span></span>

* <span data-ttu-id="b4cf2-236">Instalación de la [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) paquete.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-236">Installation of the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package.</span></span>
* <span data-ttu-id="b4cf2-237">Una llamada a `.UseLazyLoadingProxies()` en `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-237">A call to `.UseLazyLoadingProxies()` inside `AddDbContext`.</span></span>
* <span data-ttu-id="b4cf2-238">Tipos de entidades públicas con propiedades de navegación virtual pública.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-238">Public entity types with public virtual navigation properties.</span></span>

<span data-ttu-id="b4cf2-239">Este es un ejemplo de llamar al método `.UseLazyLoadingProxies()`:</span><span class="sxs-lookup"><span data-stu-id="b4cf2-239">Here is an example of calling `.UseLazyLoadingProxies()`:</span></span>

```CSharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
            .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="b4cf2-240">Consulte los ejemplos anteriores para agregar propiedades de navegación a los tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="b4cf2-240">Refer back to the examples above for adding navigation properties to the entity types.</span></span>
