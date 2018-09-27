---
title: Personalización del modelo de identidad en ASP.NET Core
author: ajcvickers
description: En este artículo se describe cómo personalizar el modelo de datos subyacente de Entity Framework Core para ASP.NET Core Identity.
ms.author: avickers
ms.date: 09/24/2018
uid: security/authentication/customize_identity_model
ms.openlocfilehash: 90c867eeac0e64bfe77cc7a829d61e831a2fb8e1
ms.sourcegitcommit: 9bdba90b2c97a4016188434657194b2d7027d6e3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47402260"
---
# <a name="identity-model-customization-in-aspnet-core"></a><span data-ttu-id="1ede2-103">Personalización del modelo de identidad en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ede2-103">Identity model customization in ASP.NET Core</span></span>

<span data-ttu-id="1ede2-104">Por [Arthur Vickers](https://github.com/ajcvickers)</span><span class="sxs-lookup"><span data-stu-id="1ede2-104">By [Arthur Vickers](https://github.com/ajcvickers)</span></span>

<span data-ttu-id="1ede2-105">Identidad de ASP.NET Core proporciona un marco para administrar y almacenar las cuentas de usuario en aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1ede2-105">ASP.NET Core Identity provides a framework for managing and storing user accounts in ASP.NET Core apps.</span></span> <span data-ttu-id="1ede2-106">Identidad se agrega al proyecto cuando **cuentas de usuario individuales** está seleccionado como el mecanismo de autenticación.</span><span class="sxs-lookup"><span data-stu-id="1ede2-106">Identity is added to your project when **Individual User Accounts** is selected as the authentication mechanism.</span></span> <span data-ttu-id="1ede2-107">De forma predeterminada, identidad hace uso de Entity Framework (EF) modelo de datos principal.</span><span class="sxs-lookup"><span data-stu-id="1ede2-107">By default, Identity makes use of an Entity Framework (EF) Core data model.</span></span> <span data-ttu-id="1ede2-108">En este artículo se describe cómo personalizar el modelo de identidad.</span><span class="sxs-lookup"><span data-stu-id="1ede2-108">This article describes how to customize the Identity model.</span></span>

## <a name="identity-and-ef-core-migrations"></a><span data-ttu-id="1ede2-109">Identidad y migraciones de EF Core</span><span class="sxs-lookup"><span data-stu-id="1ede2-109">Identity and EF Core Migrations</span></span>

<span data-ttu-id="1ede2-110">Antes de examinar el modelo, es útil comprender cómo funciona la identidad con [migraciones de EF Core](/ef/core/managing-schemas/migrations/) para crear y actualizar una base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ede2-110">Before examining the model, it's useful to understand how Identity works with [EF Core Migrations](/ef/core/managing-schemas/migrations/) to create and update a database.</span></span> <span data-ttu-id="1ede2-111">En el nivel superior, el proceso es:</span><span class="sxs-lookup"><span data-stu-id="1ede2-111">At the top level, the process is:</span></span>

1. <span data-ttu-id="1ede2-112">Definir o actualizar un [modelo de datos en el código](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="1ede2-112">Define or update a [data model in code](/ef/core/modeling/).</span></span>
1. <span data-ttu-id="1ede2-113">Agregue una migración para convertir este modelo en los cambios que se pueden aplicar a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ede2-113">Add a Migration to translate this model into changes that can be applied to the database.</span></span>
1. <span data-ttu-id="1ede2-114">Compruebe que la migración representa correctamente sus intenciones.</span><span class="sxs-lookup"><span data-stu-id="1ede2-114">Check that the Migration correctly represents your intentions.</span></span>
1. <span data-ttu-id="1ede2-115">Aplique la migración para actualizar la base de datos estén sincronizadas con el modelo.</span><span class="sxs-lookup"><span data-stu-id="1ede2-115">Apply the Migration to update the database to be in sync with the model.</span></span>
1. <span data-ttu-id="1ede2-116">Repita los pasos del 1 al 4 para perfeccionar el modelo y mantener sincronizada la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ede2-116">Repeat steps 1 through 4 to further refine the model and keep the database in sync.</span></span>

<span data-ttu-id="1ede2-117">Para agregar y aplicar migraciones, use uno de los enfoques siguientes:</span><span class="sxs-lookup"><span data-stu-id="1ede2-117">Use one of the following approaches to add and apply Migrations:</span></span>

* <span data-ttu-id="1ede2-118">El **Package Manager Console** ventana de (PMC) si usa Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ede2-118">The **Package Manager Console** (PMC) window if using Visual Studio.</span></span> <span data-ttu-id="1ede2-119">Para obtener más información, consulte [herramientas de EF Core PMC](/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="1ede2-119">For more information, see [EF Core PMC tools](/ef/core/miscellaneous/cli/powershell).</span></span>
* <span data-ttu-id="1ede2-120">La CLI de .NET Core si usa la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="1ede2-120">The .NET Core CLI if using the command line.</span></span> <span data-ttu-id="1ede2-121">Para obtener más información, consulte [herramientas de línea de comandos de EF Core .NET](/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="1ede2-121">For more information, see [EF Core .NET command line tools](/ef/core/miscellaneous/cli/dotnet).</span></span>
* <span data-ttu-id="1ede2-122">Al hacer clic en el **aplicar migraciones** botón en la página de error cuando se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1ede2-122">Clicking the **Apply Migrations** button on the error page when the app is run.</span></span>

<span data-ttu-id="1ede2-123">ASP.NET Core tiene un controlador de la página de error de tiempo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="1ede2-123">ASP.NET Core has a development-time error page handler.</span></span> <span data-ttu-id="1ede2-124">El controlador puede aplicar las migraciones cuando se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1ede2-124">The handler can apply migrations when the app is run.</span></span> <span data-ttu-id="1ede2-125">Para las aplicaciones de producción, a menudo es más adecuado para generar scripts SQL a partir de las migraciones e implementar los cambios de la base de datos como parte de una implementación controlada de aplicación y base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ede2-125">For production apps, it's often more appropriate to generate SQL scripts from the migrations and deploy database changes as part of a controlled app and database deployment.</span></span>

<span data-ttu-id="1ede2-126">Cuando se crea una nueva aplicación mediante la identidad, ya se han completado los pasos 1 y 2 anteriores.</span><span class="sxs-lookup"><span data-stu-id="1ede2-126">When a new app using Identity is created, steps 1 and 2 above have already been completed.</span></span> <span data-ttu-id="1ede2-127">Es decir, el modelo de datos iniciales ya existe y la migración inicial se ha agregado al proyecto.</span><span class="sxs-lookup"><span data-stu-id="1ede2-127">That is, the initial data model already exists, and the initial migration has been added to the project.</span></span> <span data-ttu-id="1ede2-128">La migración inicial todavía debe aplicarse a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ede2-128">The initial migration still needs to be applied to the database.</span></span> <span data-ttu-id="1ede2-129">Puede aplicar la migración inicial a través de uno de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="1ede2-129">The initial migration can be applied via one of the following approaches:</span></span>

* <span data-ttu-id="1ede2-130">Ejecute `Update-Database` en la PMC.</span><span class="sxs-lookup"><span data-stu-id="1ede2-130">Run `Update-Database` in PMC.</span></span>
* <span data-ttu-id="1ede2-131">Ejecute `dotnet ef database update` en un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="1ede2-131">Run `dotnet ef database update` in a command shell.</span></span>
* <span data-ttu-id="1ede2-132">Haga clic en el **aplicar migraciones** botón en la página de error cuando se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1ede2-132">Click the **Apply Migrations** button on the error page when the app is run.</span></span>

<span data-ttu-id="1ede2-133">Repita los pasos anteriores según se realizan cambios en el modelo.</span><span class="sxs-lookup"><span data-stu-id="1ede2-133">Repeat the preceding steps as changes are made to the model.</span></span>

## <a name="the-identity-model"></a><span data-ttu-id="1ede2-134">El modelo de identidad</span><span class="sxs-lookup"><span data-stu-id="1ede2-134">The Identity model</span></span>

### <a name="entity-types"></a><span data-ttu-id="1ede2-135">Tipos de entidad</span><span class="sxs-lookup"><span data-stu-id="1ede2-135">Entity types</span></span>

<span data-ttu-id="1ede2-136">El modelo de identidad consta de los siguientes tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="1ede2-136">The Identity model consists of the following entity types.</span></span>

|<span data-ttu-id="1ede2-137">Tipo de entidad</span><span class="sxs-lookup"><span data-stu-id="1ede2-137">Entity type</span></span>|<span data-ttu-id="1ede2-138">Descripción</span><span class="sxs-lookup"><span data-stu-id="1ede2-138">Description</span></span>                                                  |
|-----------|-------------------------------------------------------------|
|`User`     |<span data-ttu-id="1ede2-139">Representa al usuario.</span><span class="sxs-lookup"><span data-stu-id="1ede2-139">Represents the user.</span></span>                                         |
|`Role`     |<span data-ttu-id="1ede2-140">Representa un rol.</span><span class="sxs-lookup"><span data-stu-id="1ede2-140">Represents a role.</span></span>                                           |
|`UserClaim`|<span data-ttu-id="1ede2-141">Representa una notificación que posee un usuario.</span><span class="sxs-lookup"><span data-stu-id="1ede2-141">Represents a claim that a user possesses.</span></span>                    |
|`UserToken`|<span data-ttu-id="1ede2-142">Representa un token de autenticación para un usuario.</span><span class="sxs-lookup"><span data-stu-id="1ede2-142">Represents an authentication token for a user.</span></span>               |
|`UserLogin`|<span data-ttu-id="1ede2-143">Asocia un usuario a un inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="1ede2-143">Associates a user with a login.</span></span>                              |
|`RoleClaim`|<span data-ttu-id="1ede2-144">Representa una notificación que se concede a todos los usuarios dentro de un rol.</span><span class="sxs-lookup"><span data-stu-id="1ede2-144">Represents a claim that's granted to all users within a role.</span></span>|
|`UserRole` |<span data-ttu-id="1ede2-145">Una entidad de combinación que se asocia a los usuarios y roles.</span><span class="sxs-lookup"><span data-stu-id="1ede2-145">A join entity that associates users and roles.</span></span>               |

### <a name="entity-type-relationships"></a><span data-ttu-id="1ede2-146">Relaciones entre tipos de entidad</span><span class="sxs-lookup"><span data-stu-id="1ede2-146">Entity type relationships</span></span>

<span data-ttu-id="1ede2-147">El [tipos de entidad](#entity-types) están relacionados entre sí de las maneras siguientes:</span><span class="sxs-lookup"><span data-stu-id="1ede2-147">The [entity types](#entity-types) are related to each other in the following ways:</span></span>

* <span data-ttu-id="1ede2-148">Cada `User` puede tener muchos `UserClaims`.</span><span class="sxs-lookup"><span data-stu-id="1ede2-148">Each `User` can have many `UserClaims`.</span></span>
* <span data-ttu-id="1ede2-149">Cada `User` puede tener muchos `UserLogins`.</span><span class="sxs-lookup"><span data-stu-id="1ede2-149">Each `User` can have many `UserLogins`.</span></span>
* <span data-ttu-id="1ede2-150">Cada `User` puede tener muchos `UserTokens`.</span><span class="sxs-lookup"><span data-stu-id="1ede2-150">Each `User` can have many `UserTokens`.</span></span>
* <span data-ttu-id="1ede2-151">Cada `Role` puede tener muchos asociados `RoleClaims`.</span><span class="sxs-lookup"><span data-stu-id="1ede2-151">Each `Role` can have many associated `RoleClaims`.</span></span>
* <span data-ttu-id="1ede2-152">Cada `User` puede tener muchos asociados `Roles`y cada `Role` puede asociarse con muchos `Users`.</span><span class="sxs-lookup"><span data-stu-id="1ede2-152">Each `User` can have many associated `Roles`, and each `Role` can be associated with many `Users`.</span></span> <span data-ttu-id="1ede2-153">Se trata de una relación de varios a varios que requiere una tabla combinada en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ede2-153">This is a many-to-many relationship that requires a join table in the database.</span></span> <span data-ttu-id="1ede2-154">La tabla de combinación se representa mediante el `UserRole` entidad.</span><span class="sxs-lookup"><span data-stu-id="1ede2-154">The join table is represented by the `UserRole` entity.</span></span>

### <a name="default-model-configuration"></a><span data-ttu-id="1ede2-155">Configuración predeterminada del modelo</span><span class="sxs-lookup"><span data-stu-id="1ede2-155">Default model configuration</span></span>

<span data-ttu-id="1ede2-156">Identidad define muchos *las clases de contexto* que heredan de <xref:Microsoft.EntityFrameworkCore.DbContext> para configurar y usar el modelo.</span><span class="sxs-lookup"><span data-stu-id="1ede2-156">Identity defines many *context classes* that inherit from <xref:Microsoft.EntityFrameworkCore.DbContext> to configure and use the model.</span></span> <span data-ttu-id="1ede2-157">Esta configuración se realiza mediante el [EF Core Fluent API de Code First](/ef/core/modeling/) en el <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating*> método de la clase de contexto.</span><span class="sxs-lookup"><span data-stu-id="1ede2-157">This configuration is done using the [EF Core Code First Fluent API](/ef/core/modeling/) in the <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating*> method of the context class.</span></span> <span data-ttu-id="1ede2-158">La configuración predeterminada es:</span><span class="sxs-lookup"><span data-stu-id="1ede2-158">The default configuration is:</span></span>

```csharp
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

    // Limit the size of the composite key columns due to common DB restrictions
    b.Property(l => l.LoginProvider).HasMaxLength(128);
    b.Property(l => l.ProviderKey).HasMaxLength(128);

    // Maps to the AspNetUserLogins table
    b.ToTable("AspNetUserLogins");
});

builder.Entity<TUserToken>(b =>
{
    // Composite primary key consisting of the UserId, LoginProvider and Name
    b.HasKey(t => new { t.UserId, t.LoginProvider, t.Name });

    // Limit the size of the composite key columns due to common DB restrictions
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

### <a name="model-generic-types"></a><span data-ttu-id="1ede2-159">Tipos genéricos de modelo</span><span class="sxs-lookup"><span data-stu-id="1ede2-159">Model generic types</span></span>

<span data-ttu-id="1ede2-160">Identidad define el valor predeterminado [Common Language Runtime](/dotnet/standard/glossary#clr) tipos (CLR) para cada uno de los tipos de entidad mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="1ede2-160">Identity defines default [Common Language Runtime](/dotnet/standard/glossary#clr) (CLR) types for each of the entity types listed above.</span></span> <span data-ttu-id="1ede2-161">Estos tipos tienen antepuesto el prefijo *identidad*:</span><span class="sxs-lookup"><span data-stu-id="1ede2-161">These types are all prefixed with *Identity*:</span></span>

* `IdentityUser`
* `IdentityRole`
* `IdentityUserClaim`
* `IdentityUserToken`
* `IdentityUserLogin`
* `IdentityRoleClaim`
* `IdentityUserRole`

<span data-ttu-id="1ede2-162">En lugar de usar directamente estos tipos, se pueden usar los tipos como clases base para los tipos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1ede2-162">Rather than using these types directly, the types can be used as base classes for the app's own types.</span></span> <span data-ttu-id="1ede2-163">El `DbContext` clases definidas por la identidad son genéricas, tal que se pueden usar diferentes tipos CLR para uno o varios de los tipos de entidad en el modelo.</span><span class="sxs-lookup"><span data-stu-id="1ede2-163">The `DbContext` classes defined by Identity are generic, such that different CLR types can be used for one or more of the entity types in the model.</span></span> <span data-ttu-id="1ede2-164">Estos tipos genéricos también permiten la `User` principal (PK) datos tipo de clave que se puede cambiar.</span><span class="sxs-lookup"><span data-stu-id="1ede2-164">These generic types also allow the `User` primary key (PK) data type to be changed.</span></span>

<span data-ttu-id="1ede2-165">Cuando se usa la identidad con el soporte técnico para los roles, un <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext> se debe usar la clase.</span><span class="sxs-lookup"><span data-stu-id="1ede2-165">When using Identity with support for roles, an <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext> class should be used.</span></span> <span data-ttu-id="1ede2-166">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1ede2-166">For example:</span></span>

```csharp
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
public class IdentityDbContext<TUser, TRole, TKey> : IdentityDbContext<
    TUser, TRole, TKey, IdentityUserClaim<TKey>, IdentityUserRole<TKey>,
    IdentityUserLogin<TKey>, IdentityRoleClaim<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TRole : IdentityRole<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments
// The key type is defined by TKey
public abstract class IdentityDbContext<
    TUser, TRole, TKey, TUserClaim, TUserRole, TUserLogin, TRoleClaim, TUserToken>
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

<span data-ttu-id="1ede2-167">También es posible usar identidad sin roles (solo notificaciones), en cuyo caso un <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUserContext`1> se debe usar la clase:</span><span class="sxs-lookup"><span data-stu-id="1ede2-167">It's also possible to use Identity without roles (only claims), in which case an <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUserContext`1> class should be used:</span></span>

```csharp
// Uses the built-in non-role Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityUserContext<TUser>
    : IdentityUserContext<TUser, string>
        where TUser : IdentityUser
{
}

// Uses the built-in non-role Identity types except with a custom User type
// The key type is defined by TKey
public class IdentityUserContext<TUser, TKey> : IdentityUserContext<
    TUser, TKey, IdentityUserClaim<TKey>, IdentityUserLogin<TKey>,
    IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments, with no roles
// The key type is defined by TKey
public abstract class IdentityUserContext<
    TUser, TKey, TUserClaim, TUserLogin, TUserToken> : DbContext
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
        where TUserClaim : IdentityUserClaim<TKey>
        where TUserLogin : IdentityUserLogin<TKey>
        where TUserToken : IdentityUserToken<TKey>
{
}
```

## <a name="customize-the-model"></a><span data-ttu-id="1ede2-168">Personalizar el modelo</span><span class="sxs-lookup"><span data-stu-id="1ede2-168">Customize the model</span></span>

<span data-ttu-id="1ede2-169">El punto de partida para la personalización del modelo consiste en derivar del tipo de contexto apropiado.</span><span class="sxs-lookup"><span data-stu-id="1ede2-169">The starting point for model customization is to derive from the appropriate context type.</span></span> <span data-ttu-id="1ede2-170">Consulte la [tipos genéricos de modelo](#model-generic-types) sección.</span><span class="sxs-lookup"><span data-stu-id="1ede2-170">See the [Model generic types](#model-generic-types) section.</span></span> <span data-ttu-id="1ede2-171">Este tipo de contexto se denomina habitualmente `ApplicationDbContext` y se crea mediante las plantillas de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1ede2-171">This context type is customarily called `ApplicationDbContext` and is created by the ASP.NET Core templates.</span></span>

<span data-ttu-id="1ede2-172">El contexto se utiliza para configurar el modelo de dos maneras:</span><span class="sxs-lookup"><span data-stu-id="1ede2-172">The context is used to configure the model in two ways:</span></span>

* <span data-ttu-id="1ede2-173">Proporciona la entidad y tipos de clave para los parámetros de tipo genérico.</span><span class="sxs-lookup"><span data-stu-id="1ede2-173">Supplying entity and key types for the generic type parameters.</span></span>
* <span data-ttu-id="1ede2-174">Reemplazar `OnModelCreating` para modificar la asignación de estos tipos.</span><span class="sxs-lookup"><span data-stu-id="1ede2-174">Overriding `OnModelCreating` to modify the mapping of these types.</span></span>

<span data-ttu-id="1ede2-175">Cuando se reemplaza `OnModelCreating`, `base.OnModelCreating` se debe llamar primero; la configuración de invalidación debe llamarse a continuación.</span><span class="sxs-lookup"><span data-stu-id="1ede2-175">When overriding `OnModelCreating`, `base.OnModelCreating` should be called first; the overriding configuration should be called next.</span></span> <span data-ttu-id="1ede2-176">EF Core generalmente tiene una directiva de última uno-wins para la configuración.</span><span class="sxs-lookup"><span data-stu-id="1ede2-176">EF Core generally has a last-one-wins policy for configuration.</span></span> <span data-ttu-id="1ede2-177">Por ejemplo, si el `ToTable` para un tipo de entidad es llamar primero al método con el nombre de una tabla y, a continuación, de nuevo más tarde con un nombre de tabla diferente, el nombre de tabla en la segunda llamada se utiliza.</span><span class="sxs-lookup"><span data-stu-id="1ede2-177">For example, if the `ToTable` method for an entity type is called first with one table name and then again later with a different table name, the table name in the second call is used.</span></span>

### <a name="custom-user-data"></a><span data-ttu-id="1ede2-178">Datos de usuario personalizada</span><span class="sxs-lookup"><span data-stu-id="1ede2-178">Custom user data</span></span>

<span data-ttu-id="1ede2-179">[Datos de usuario personalizada](xref:security/authentication/add-user-data) se admite mediante la herencia de `IdentityUser`.</span><span class="sxs-lookup"><span data-stu-id="1ede2-179">[Custom user data](xref:security/authentication/add-user-data) is supported by inheriting from `IdentityUser`.</span></span> <span data-ttu-id="1ede2-180">Es habitual para este tipo de nombre `ApplicationUser`:</span><span class="sxs-lookup"><span data-stu-id="1ede2-180">It's customary to name this type `ApplicationUser`:</span></span>


```csharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

<span data-ttu-id="1ede2-181">Use el `ApplicationUser` tipo como un argumento genérico para el contexto:</span><span class="sxs-lookup"><span data-stu-id="1ede2-181">Use the `ApplicationUser` type as a generic argument for the context:</span></span>

```csharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

<span data-ttu-id="1ede2-182">No es necesario invalidar `OnModelCreating` en el `ApplicationDbContext` clase.</span><span class="sxs-lookup"><span data-stu-id="1ede2-182">There's no need to override `OnModelCreating` in the `ApplicationDbContext` class.</span></span> <span data-ttu-id="1ede2-183">EF Core se asigna el `CustomTag` propiedad por convención.</span><span class="sxs-lookup"><span data-stu-id="1ede2-183">EF Core maps the `CustomTag` property by convention.</span></span> <span data-ttu-id="1ede2-184">Sin embargo, la base de datos debe actualizarse para crear un nuevo `CustomTag` columna.</span><span class="sxs-lookup"><span data-stu-id="1ede2-184">However, the database needs to be updated to create a new `CustomTag` column.</span></span> <span data-ttu-id="1ede2-185">Para crear la columna, agregar una migración y, a continuación, actualice la base de datos, como se describe en [identidad y migraciones de EF Core](#identity-and-ef-core-migrations).</span><span class="sxs-lookup"><span data-stu-id="1ede2-185">To create the column, add a migration, and then update the database as described in [Identity and EF Core Migrations](#identity-and-ef-core-migrations).</span></span>

<span data-ttu-id="1ede2-186">Actualización `Startup.ConfigureServices` para utilizar el nuevo `ApplicationUser` clase:</span><span class="sxs-lookup"><span data-stu-id="1ede2-186">Update `Startup.ConfigureServices` to use the new `ApplicationUser` class:</span></span>

::: moniker range=">= aspnetcore-2.1"

```csharp
services.AddDefaultIdentity<ApplicationUser>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultUI();
```

<span data-ttu-id="1ede2-187">En ASP.NET Core 2.1 o posterior, la identidad se proporciona como una biblioteca de clases de Razor.</span><span class="sxs-lookup"><span data-stu-id="1ede2-187">In ASP.NET Core 2.1 or later, Identity is provided as a Razor Class Library.</span></span> <span data-ttu-id="1ede2-188">Para obtener más información, consulta <xref:security/authentication/scaffold-identity>.</span><span class="sxs-lookup"><span data-stu-id="1ede2-188">For more information, see <xref:security/authentication/scaffold-identity>.</span></span> <span data-ttu-id="1ede2-189">Por lo tanto, el código anterior requiere una llamada a <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span><span class="sxs-lookup"><span data-stu-id="1ede2-189">Consequently, the preceding code requires a call to <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span></span> <span data-ttu-id="1ede2-190">Si el proveedor de scaffolding de identidad se usó para agregar archivos de identidad para el proyecto, quite la llamada a `AddDefaultUI`.</span><span class="sxs-lookup"><span data-stu-id="1ede2-190">If the Identity scaffolder was used to add Identity files to the project, remove the call to `AddDefaultUI`.</span></span> <span data-ttu-id="1ede2-191">Para obtener más información, consulte:</span><span class="sxs-lookup"><span data-stu-id="1ede2-191">For more information, see:</span></span>

* [<span data-ttu-id="1ede2-192">Identidad de scaffolding</span><span class="sxs-lookup"><span data-stu-id="1ede2-192">Scaffold Identity</span></span>](xref:security/authentication/scaffold-identity)
* [<span data-ttu-id="1ede2-193">Agregar, descargar y eliminar datos de usuario personalizada para la identidad</span><span class="sxs-lookup"><span data-stu-id="1ede2-193">Add, download, and delete custom user data to Identity</span></span>](xref:security/authentication/add-user-data)

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();
```

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
        .AddDefaultTokenProviders();
```

::: moniker-end

### <a name="change-the-primary-key-type"></a><span data-ttu-id="1ede2-194">Cambiar el tipo de clave principal</span><span class="sxs-lookup"><span data-stu-id="1ede2-194">Change the primary key type</span></span>

<span data-ttu-id="1ede2-195">Un cambio en el tipo de datos de la columna de clave principal una vez creada la base de datos es problemático en muchos sistemas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ede2-195">A change to the PK column's data type after the database has been created is problematic on many database systems.</span></span> <span data-ttu-id="1ede2-196">Cambiar la clave principal normalmente implica quitar y volver a crear la tabla.</span><span class="sxs-lookup"><span data-stu-id="1ede2-196">Changing the PK typically involves dropping and re-creating the table.</span></span> <span data-ttu-id="1ede2-197">Por lo tanto, los tipos de clave deben especificarse en la migración inicial cuando se crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ede2-197">Therefore, key types should be specified in the initial migration when the database is created.</span></span>

<span data-ttu-id="1ede2-198">Siga estos pasos para cambiar el tipo de clave principal:</span><span class="sxs-lookup"><span data-stu-id="1ede2-198">Follow these steps to change the PK type:</span></span>

1. <span data-ttu-id="1ede2-199">Si la base de datos se creó antes del cambio de clave principal, ejecute `Drop-Database` (PMC) o `dotnet ef database drop` (CLI de .NET Core) para eliminarlo.</span><span class="sxs-lookup"><span data-stu-id="1ede2-199">If the database was created before the PK change, run `Drop-Database` (PMC) or `dotnet ef database drop` (.NET Core CLI) to delete it.</span></span>
2. <span data-ttu-id="1ede2-200">Después de confirmar la eliminación de la base de datos, quite la migración inicial con `Remove-Migration` (PMC) o `dotnet ef migrations remove` (CLI de .NET Core).</span><span class="sxs-lookup"><span data-stu-id="1ede2-200">After confirming deletion of the database, remove the initial migration with `Remove-Migration` (PMC) or `dotnet ef migrations remove` (.NET Core CLI).</span></span>
3. <span data-ttu-id="1ede2-201">Actualización de la `ApplicationDbContext` clase derive de <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext`3>.</span><span class="sxs-lookup"><span data-stu-id="1ede2-201">Update the `ApplicationDbContext` class to derive from <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext`3>.</span></span> <span data-ttu-id="1ede2-202">Especifique el nuevo tipo de clave para `TKey`.</span><span class="sxs-lookup"><span data-stu-id="1ede2-202">Specify the new key type for `TKey`.</span></span> <span data-ttu-id="1ede2-203">Por ejemplo, para usar un `Guid` tipo de clave:</span><span class="sxs-lookup"><span data-stu-id="1ede2-203">For example, to use a `Guid` key type:</span></span>

    ```csharp
    public class ApplicationDbContext
        : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
    }
    ```

    ::: moniker range=">= aspnetcore-2.0"

    <span data-ttu-id="1ede2-204">En el código anterior, las clases genéricas <xref:Microsoft.AspNetCore.Identity.IdentityUser`1> y <xref:Microsoft.AspNetCore.Identity.IdentityRole`1> debe especificarse para usar el nuevo tipo de clave.</span><span class="sxs-lookup"><span data-stu-id="1ede2-204">In the preceding code, the generic classes <xref:Microsoft.AspNetCore.Identity.IdentityUser`1> and <xref:Microsoft.AspNetCore.Identity.IdentityRole`1> must be specified to use the new key type.</span></span>

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    <span data-ttu-id="1ede2-205">En el código anterior, las clases genéricas <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUser`1> y <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityRole`1> debe especificarse para usar el nuevo tipo de clave.</span><span class="sxs-lookup"><span data-stu-id="1ede2-205">In the preceding code, the generic classes <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUser`1> and <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityRole`1> must be specified to use the new key type.</span></span>

    ::: moniker-end

    <span data-ttu-id="1ede2-206">`Startup.ConfigureServices` debe actualizarse para utilizar el usuario genérico:</span><span class="sxs-lookup"><span data-stu-id="1ede2-206">`Startup.ConfigureServices` must be updated to use the generic user:</span></span>

    ::: moniker range=">= aspnetcore-2.1"

    ```csharp
    services.AddDefaultIdentity<IdentityUser<Guid>>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    ```csharp
    services.AddIdentity<IdentityUser<Guid>, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    ```csharp
    services.AddIdentity<IdentityUser<Guid>, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
            .AddDefaultTokenProviders();
    ```

    ::: moniker-end

4. <span data-ttu-id="1ede2-207">Si un personalizado `ApplicationUser` clase se utiliza, actualice la clase para heredar de `IdentityUser`.</span><span class="sxs-lookup"><span data-stu-id="1ede2-207">If a custom `ApplicationUser` class is being used, update the class to inherit from `IdentityUser`.</span></span> <span data-ttu-id="1ede2-208">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1ede2-208">For example:</span></span>

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Models/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    ::: moniker range=">= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    <span data-ttu-id="1ede2-209">Actualización `ApplicationDbContext` para hacer referencia a la personalizada `ApplicationUser` clase:</span><span class="sxs-lookup"><span data-stu-id="1ede2-209">Update `ApplicationDbContext` to reference the custom `ApplicationUser` class:</span></span>

    ```csharp
    public class ApplicationDbContext
        : IdentityDbContext<ApplicationUser, IdentityRole<Guid>, Guid>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
    }
    ```

    <span data-ttu-id="1ede2-210">Registrar la clase de contexto de base de datos personalizados al agregar el servicio de identidad en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1ede2-210">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    ::: moniker range=">= aspnetcore-2.1"

    ```csharp
    services.AddDefaultIdentity<ApplicationUser>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultUI()
            .AddDefaultTokenProviders();
    ```

    <span data-ttu-id="1ede2-211">Tipo de datos de la clave principal se deduce mediante el análisis de la <xref:Microsoft.EntityFrameworkCore.DbContext> objeto.</span><span class="sxs-lookup"><span data-stu-id="1ede2-211">The primary key's data type is inferred by analyzing the <xref:Microsoft.EntityFrameworkCore.DbContext> object.</span></span>

    <span data-ttu-id="1ede2-212">En ASP.NET Core 2.1 o posterior, la identidad se proporciona como una biblioteca de clases de Razor.</span><span class="sxs-lookup"><span data-stu-id="1ede2-212">In ASP.NET Core 2.1 or later, Identity is provided as a Razor Class Library.</span></span> <span data-ttu-id="1ede2-213">Para obtener más información, consulta <xref:security/authentication/scaffold-identity>.</span><span class="sxs-lookup"><span data-stu-id="1ede2-213">For more information, see <xref:security/authentication/scaffold-identity>.</span></span> <span data-ttu-id="1ede2-214">Por lo tanto, el código anterior requiere una llamada a <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span><span class="sxs-lookup"><span data-stu-id="1ede2-214">Consequently, the preceding code requires a call to <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span></span> <span data-ttu-id="1ede2-215">Si el proveedor de scaffolding de identidad se usó para agregar archivos de identidad para el proyecto, quite la llamada a `AddDefaultUI`.</span><span class="sxs-lookup"><span data-stu-id="1ede2-215">If the Identity scaffolder was used to add Identity files to the project, remove the call to `AddDefaultUI`.</span></span>

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    <span data-ttu-id="1ede2-216">Tipo de datos de la clave principal se deduce mediante el análisis de la <xref:Microsoft.EntityFrameworkCore.DbContext> objeto.</span><span class="sxs-lookup"><span data-stu-id="1ede2-216">The primary key's data type is inferred by analyzing the <xref:Microsoft.EntityFrameworkCore.DbContext> object.</span></span>

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
            .AddDefaultTokenProviders();
    ```

    <span data-ttu-id="1ede2-217">El <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> método acepta un `TKey` tipo que indica el tipo de datos de la clave principal.</span><span class="sxs-lookup"><span data-stu-id="1ede2-217">The <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> method accepts a `TKey` type indicating the primary key's data type.</span></span>

    ::: moniker-end

5. <span data-ttu-id="1ede2-218">Si un personalizado `ApplicationRole` clase se utiliza, actualice la clase para heredar de `IdentityRole<TKey>`.</span><span class="sxs-lookup"><span data-stu-id="1ede2-218">If a custom `ApplicationRole` class is being used, update the class to inherit from `IdentityRole<TKey>`.</span></span> <span data-ttu-id="1ede2-219">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1ede2-219">For example:</span></span>

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationRole.cs?name=snippet_ApplicationRole&highlight=4)]

    <span data-ttu-id="1ede2-220">Actualización `ApplicationDbContext` para hacer referencia a la personalizada `ApplicationRole` clase.</span><span class="sxs-lookup"><span data-stu-id="1ede2-220">Update `ApplicationDbContext` to reference the custom `ApplicationRole` class.</span></span> <span data-ttu-id="1ede2-221">Por ejemplo, la clase siguiente hace referencia a una personalizada `ApplicationUser` y personalizada `ApplicationRole`:</span><span class="sxs-lookup"><span data-stu-id="1ede2-221">For example, the following class references a custom `ApplicationUser` and a custom `ApplicationRole`:</span></span>

    ::: moniker range=">= aspnetcore-2.1"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    <span data-ttu-id="1ede2-222">Registrar la clase de contexto de base de datos personalizados al agregar el servicio de identidad en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1ede2-222">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=13-16)]

    <span data-ttu-id="1ede2-223">Tipo de datos de la clave principal se deduce mediante el análisis de la <xref:Microsoft.EntityFrameworkCore.DbContext> objeto.</span><span class="sxs-lookup"><span data-stu-id="1ede2-223">The primary key's data type is inferred by analyzing the <xref:Microsoft.EntityFrameworkCore.DbContext> object.</span></span>

    <span data-ttu-id="1ede2-224">En ASP.NET Core 2.1 o posterior, la identidad se proporciona como una biblioteca de clases de Razor.</span><span class="sxs-lookup"><span data-stu-id="1ede2-224">In ASP.NET Core 2.1 or later, Identity is provided as a Razor Class Library.</span></span> <span data-ttu-id="1ede2-225">Para obtener más información, consulta <xref:security/authentication/scaffold-identity>.</span><span class="sxs-lookup"><span data-stu-id="1ede2-225">For more information, see <xref:security/authentication/scaffold-identity>.</span></span> <span data-ttu-id="1ede2-226">Por lo tanto, el código anterior requiere una llamada a <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span><span class="sxs-lookup"><span data-stu-id="1ede2-226">Consequently, the preceding code requires a call to <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span></span> <span data-ttu-id="1ede2-227">Si el proveedor de scaffolding de identidad se usó para agregar archivos de identidad para el proyecto, quite la llamada a `AddDefaultUI`.</span><span class="sxs-lookup"><span data-stu-id="1ede2-227">If the Identity scaffolder was used to add Identity files to the project, remove the call to `AddDefaultUI`.</span></span>

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    <span data-ttu-id="1ede2-228">Registrar la clase de contexto de base de datos personalizados al agregar el servicio de identidad en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1ede2-228">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    <span data-ttu-id="1ede2-229">Tipo de datos de la clave principal se deduce mediante el análisis de la <xref:Microsoft.EntityFrameworkCore.DbContext> objeto.</span><span class="sxs-lookup"><span data-stu-id="1ede2-229">The primary key's data type is inferred by analyzing the <xref:Microsoft.EntityFrameworkCore.DbContext> object.</span></span>

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    <span data-ttu-id="1ede2-230">Registrar la clase de contexto de base de datos personalizados al agregar el servicio de identidad en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1ede2-230">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    <span data-ttu-id="1ede2-231">El <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> método acepta un `TKey` tipo que indica el tipo de datos de la clave principal.</span><span class="sxs-lookup"><span data-stu-id="1ede2-231">The <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> method accepts a `TKey` type indicating the primary key's data type.</span></span>

    ::: moniker-end

### <a name="add-navigation-properties"></a><span data-ttu-id="1ede2-232">Agregar las propiedades de navegación</span><span class="sxs-lookup"><span data-stu-id="1ede2-232">Add navigation properties</span></span>

<span data-ttu-id="1ede2-233">Cambiar la configuración del modelo para las relaciones puede ser más difícil que realizar otros cambios.</span><span class="sxs-lookup"><span data-stu-id="1ede2-233">Changing the model configuration for relationships can be more difficult than making other changes.</span></span> <span data-ttu-id="1ede2-234">Debe tener cuidado para reemplazar las relaciones existentes en lugar de crear nuevas relaciones adicionales.</span><span class="sxs-lookup"><span data-stu-id="1ede2-234">Care must be taken to replace the existing relationships rather than create new, additional relationships.</span></span> <span data-ttu-id="1ede2-235">En concreto, la relación modificada debe especificar la propiedad de clave externa misma (FK) como la relación existente.</span><span class="sxs-lookup"><span data-stu-id="1ede2-235">In particular, the changed relationship must specify the same foreign key (FK) property as the existing relationship.</span></span> <span data-ttu-id="1ede2-236">Por ejemplo, la relación entre `Users` y `UserClaims` es, de forma predeterminada, se especifica como sigue:</span><span class="sxs-lookup"><span data-stu-id="1ede2-236">For example, the relationship between `Users` and `UserClaims` is, by default, specified as follows:</span></span>

```csharp
builder.Entity<TUser>(b =>
{
    // Each User can have many UserClaims
    b.HasMany<TUserClaim>()
     .WithOne()
     .HasForeignKey(uc => uc.UserId)
     .IsRequired();
});
```

<span data-ttu-id="1ede2-237">La clave externa de esta relación se especifica como el `UserClaim.UserId` propiedad.</span><span class="sxs-lookup"><span data-stu-id="1ede2-237">The FK for this relationship is specified as the `UserClaim.UserId` property.</span></span> <span data-ttu-id="1ede2-238">`HasMany` y `WithOne` se llama sin argumentos para crear la relación sin propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="1ede2-238">`HasMany` and `WithOne` are called without arguments to create the relationship without navigation properties.</span></span>

<span data-ttu-id="1ede2-239">Agregar una propiedad de navegación `ApplicationUser` que permite asociados `UserClaims` hacer referencia a ella desde el usuario:</span><span class="sxs-lookup"><span data-stu-id="1ede2-239">Add a navigation property to `ApplicationUser` that allows associated `UserClaims` to be referenced from the user:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

<span data-ttu-id="1ede2-240">El `TKey` para `IdentityUserClaim<TKey>` es del tipo especificado para la clave principal de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="1ede2-240">The `TKey` for `IdentityUserClaim<TKey>` is the type specified for the PK of users.</span></span> <span data-ttu-id="1ede2-241">En este caso, `TKey` es `string` porque se usan los valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="1ede2-241">In this case, `TKey` is `string` because the defaults are being used.</span></span> <span data-ttu-id="1ede2-242">Tiene **no** el tipo de clave principal para el `UserClaim` tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="1ede2-242">It's **not** the PK type for the `UserClaim` entity type.</span></span>

<span data-ttu-id="1ede2-243">Ahora que existe la propiedad de navegación, deben configurarse en `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="1ede2-243">Now that the navigation property exists, it must be configured in `OnModelCreating`:</span></span>

```csharp
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

<span data-ttu-id="1ede2-244">Tenga en cuenta que se configura la relación exactamente igual que antes, solo con una propiedad de navegación especificada en la llamada a `HasMany`.</span><span class="sxs-lookup"><span data-stu-id="1ede2-244">Notice that relationship is configured exactly as it was before, only with a navigation property specified in the call to `HasMany`.</span></span>

<span data-ttu-id="1ede2-245">Las propiedades de navegación solo existen en el modelo EF, no en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ede2-245">The navigation properties only exist in the EF model, not the database.</span></span> <span data-ttu-id="1ede2-246">Dado que no ha cambiado la clave externa para la relación, este tipo de cambio de modelo no requiere la base de datos se puede actualizar.</span><span class="sxs-lookup"><span data-stu-id="1ede2-246">Because the FK for the relationship hasn't changed, this kind of model change doesn't require the database to be updated.</span></span> <span data-ttu-id="1ede2-247">Esto se puede comprobar mediante la adición de una migración después de realizar el cambio.</span><span class="sxs-lookup"><span data-stu-id="1ede2-247">This can be checked by adding a migration after making the change.</span></span> <span data-ttu-id="1ede2-248">El `Up` y `Down` métodos están vacíos.</span><span class="sxs-lookup"><span data-stu-id="1ede2-248">The `Up` and `Down` methods are empty.</span></span>

### <a name="add-all-user-navigation-properties"></a><span data-ttu-id="1ede2-249">Agregar todas las propiedades de navegación del usuario</span><span class="sxs-lookup"><span data-stu-id="1ede2-249">Add all User navigation properties</span></span>

<span data-ttu-id="1ede2-250">Con la sección anterior como guía, en el ejemplo siguiente se configura las propiedades de navegación unidireccional para todas las relaciones de usuario:</span><span class="sxs-lookup"><span data-stu-id="1ede2-250">Using the section above as guidance, the following example configures unidirectional navigation properties for all relationships on User:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<IdentityUserRole<string>> UserRoles { get; set; }
}
```

```csharp
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

### <a name="add-user-and-role-navigation-properties"></a><span data-ttu-id="1ede2-251">Agregar las propiedades de navegación del usuario y el rol</span><span class="sxs-lookup"><span data-stu-id="1ede2-251">Add User and Role navigation properties</span></span>

<span data-ttu-id="1ede2-252">Uso de la sección anterior como guía, en el ejemplo siguiente se configura las propiedades de navegación para todas las relaciones de usuario y el rol:</span><span class="sxs-lookup"><span data-stu-id="1ede2-252">Using the section above as guidance, the following example configures navigation properties for all relationships on User and Role:</span></span>

```csharp
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

```csharp
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

<span data-ttu-id="1ede2-253">Notas:</span><span class="sxs-lookup"><span data-stu-id="1ede2-253">Notes:</span></span>

* <span data-ttu-id="1ede2-254">En este ejemplo también incluye el `UserRole` unirse a la entidad, que es necesaria para navegar por la relación de muchos a muchos de los usuarios a Roles.</span><span class="sxs-lookup"><span data-stu-id="1ede2-254">This example also includes the `UserRole` join entity, which is needed to navigate the many-to-many relationship from Users to Roles.</span></span>
* <span data-ttu-id="1ede2-255">No olvide cambiar los tipos de las propiedades de navegación para reflejar ese hecho `ApplicationXxx` tipos ahora se utilizan en lugar de `IdentityXxx` tipos.</span><span class="sxs-lookup"><span data-stu-id="1ede2-255">Remember to change the types of the navigation properties to reflect that `ApplicationXxx` types are now being used instead of `IdentityXxx` types.</span></span>
* <span data-ttu-id="1ede2-256">Recuerde que debe usar el `ApplicationXxx` en el modelo genérico `ApplicationContext` definición.</span><span class="sxs-lookup"><span data-stu-id="1ede2-256">Remember to use the `ApplicationXxx` in the generic `ApplicationContext` definition.</span></span>

### <a name="add-all-navigation-properties"></a><span data-ttu-id="1ede2-257">Agregar todas las propiedades de navegación</span><span class="sxs-lookup"><span data-stu-id="1ede2-257">Add all navigation properties</span></span>

<span data-ttu-id="1ede2-258">Uso de la sección anterior como guía, en el ejemplo siguiente se configura las propiedades de navegación para todas las relaciones en todos los tipos de entidad:</span><span class="sxs-lookup"><span data-stu-id="1ede2-258">Using the section above as guidance, the following example configures navigation properties for all relationships on all entity types:</span></span>

```csharp
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

```csharp
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

### <a name="use-composite-keys"></a><span data-ttu-id="1ede2-259">Use las claves compuestas</span><span class="sxs-lookup"><span data-stu-id="1ede2-259">Use composite keys</span></span>

<span data-ttu-id="1ede2-260">Las secciones anteriores muestran el cambio del tipo de clave que se usa en el modelo de identidad.</span><span class="sxs-lookup"><span data-stu-id="1ede2-260">The preceding sections demonstrated changing the type of key used in the Identity model.</span></span> <span data-ttu-id="1ede2-261">Cambiar el modelo de identidad clave para usar las claves compuestas no es compatible ni recomendable.</span><span class="sxs-lookup"><span data-stu-id="1ede2-261">Changing the Identity key model to use composite keys isn't supported or recommended.</span></span> <span data-ttu-id="1ede2-262">Usar una clave compuesta con identidad implica la modificación de cómo interactúa el código del Administrador de identidades con el modelo.</span><span class="sxs-lookup"><span data-stu-id="1ede2-262">Using a composite key with Identity involves changing how the Identity manager code interacts with the model.</span></span> <span data-ttu-id="1ede2-263">Esta personalización está fuera del ámbito de este documento.</span><span class="sxs-lookup"><span data-stu-id="1ede2-263">This customization is beyond the scope of this document.</span></span>

### <a name="change-tablecolumn-names-and-facets"></a><span data-ttu-id="1ede2-264">Cambiar los nombres de tabla o columna y las facetas</span><span class="sxs-lookup"><span data-stu-id="1ede2-264">Change table/column names and facets</span></span>

<span data-ttu-id="1ede2-265">Para cambiar los nombres de tablas y columnas, llame a `base.OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="1ede2-265">To change the names of tables and columns, call `base.OnModelCreating`.</span></span> <span data-ttu-id="1ede2-266">A continuación, agregue la configuración para reemplazar cualquiera de los valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="1ede2-266">Then, add configuration to override any of the defaults.</span></span> <span data-ttu-id="1ede2-267">Por ejemplo, para cambiar el nombre de todas las tablas de identidad:</span><span class="sxs-lookup"><span data-stu-id="1ede2-267">For example, to change the name of all the Identity tables:</span></span>

```csharp
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

<span data-ttu-id="1ede2-268">Estos ejemplos utilizan los tipos de identidad predeterminada.</span><span class="sxs-lookup"><span data-stu-id="1ede2-268">These examples use the default Identity types.</span></span> <span data-ttu-id="1ede2-269">Si usa un tipo de aplicación, como `ApplicationUser`, configurar ese tipo en lugar del tipo predeterminado.</span><span class="sxs-lookup"><span data-stu-id="1ede2-269">If using an app type such as `ApplicationUser`, configure that type instead of the default type.</span></span>

<span data-ttu-id="1ede2-270">El ejemplo siguiente cambia algunos nombres de columna:</span><span class="sxs-lookup"><span data-stu-id="1ede2-270">The following example changes some column names:</span></span>

```csharp
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

<span data-ttu-id="1ede2-271">Algunos tipos de columnas de la base de datos pueden configurarse con determinados *facetas* (por ejemplo, el máximo `string` longitud permitida).</span><span class="sxs-lookup"><span data-stu-id="1ede2-271">Some types of database columns can be configured with certain *facets* (for example, the maximum `string` length allowed).</span></span> <span data-ttu-id="1ede2-272">El ejemplo siguiente establece las longitudes de columna máximo para varios `string` propiedades en el modelo:</span><span class="sxs-lookup"><span data-stu-id="1ede2-272">The following example sets column maximum lengths for several `string` properties in the model:</span></span>

```csharp
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

### <a name="map-to-a-different-schema"></a><span data-ttu-id="1ede2-273">Asignar a un esquema diferente</span><span class="sxs-lookup"><span data-stu-id="1ede2-273">Map to a different schema</span></span>

<span data-ttu-id="1ede2-274">Los esquemas pueden comportarse de manera diferente a través de proveedores de base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ede2-274">Schemas can behave differently across database providers.</span></span> <span data-ttu-id="1ede2-275">Para SQL Server, el valor predeterminado es crear todas las tablas de la *dbo* esquema.</span><span class="sxs-lookup"><span data-stu-id="1ede2-275">For SQL Server, the default is to create all tables in the *dbo* schema.</span></span> <span data-ttu-id="1ede2-276">Las tablas pueden crearse en un esquema diferente.</span><span class="sxs-lookup"><span data-stu-id="1ede2-276">The tables can be created in a different schema.</span></span> <span data-ttu-id="1ede2-277">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1ede2-277">For example:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

::: moniker range=">= aspnetcore-2.1"

### <a name="lazy-loading"></a><span data-ttu-id="1ede2-278">Carga diferida</span><span class="sxs-lookup"><span data-stu-id="1ede2-278">Lazy loading</span></span>

<span data-ttu-id="1ede2-279">En esta sección, se agrega compatibilidad con servidores proxy de la carga diferida en el modelo de identidad.</span><span class="sxs-lookup"><span data-stu-id="1ede2-279">In this section, support for lazy-loading proxies in the Identity model is added.</span></span> <span data-ttu-id="1ede2-280">Carga diferida es útil porque permite que las propiedades de navegación que se utilizará sin garantizar primero que están cargado.</span><span class="sxs-lookup"><span data-stu-id="1ede2-280">Lazy-loading is useful since it allows navigation properties to be used without first ensuring they're loaded.</span></span>

<span data-ttu-id="1ede2-281">Tipos de entidad pueden realizarse adecuados para diferida carga de varias maneras, como se describe en el [documentación de EF Core](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="1ede2-281">Entity types can be made suitable for lazy-loading in several ways, as described in the [EF Core documentation](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="1ede2-282">Por motivos de simplicidad, use servidores proxy de la carga diferida, lo que requiere:</span><span class="sxs-lookup"><span data-stu-id="1ede2-282">For simplicity, use lazy-loading proxies, which requires:</span></span>

* <span data-ttu-id="1ede2-283">Instalación de la [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) paquete.</span><span class="sxs-lookup"><span data-stu-id="1ede2-283">Installation of the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package.</span></span>
* <span data-ttu-id="1ede2-284">Una llamada a <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies*> dentro de <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext*>.</span><span class="sxs-lookup"><span data-stu-id="1ede2-284">A call to <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies*> inside <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext*>.</span></span>
* <span data-ttu-id="1ede2-285">Tipos de entidades públicas con `public virtual` las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="1ede2-285">Public entity types with `public virtual` navigation properties.</span></span>

<span data-ttu-id="1ede2-286">El ejemplo siguiente se muestra cómo llamar `UseLazyLoadingProxies` en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1ede2-286">The following example demonstrates calling `UseLazyLoadingProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
              .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="1ede2-287">Consulte los ejemplos anteriores para obtener instrucciones sobre cómo agregar propiedades de navegación a los tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="1ede2-287">Refer to the preceding examples for guidance on adding navigation properties to the entity types.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1ede2-288">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="1ede2-288">Additional resources</span></span>

* <xref:security/authentication/scaffold-identity>

::: moniker-end
