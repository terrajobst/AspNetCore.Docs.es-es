---
title: Personalización del modelo de identidad en ASP.NET Core
author: ajcvickers
description: En este artículo se describe cómo personalizar el modelo de datos subyacente de Entity Framework Core para ASP.NET Core Identity.
ms.author: avickers
ms.date: 04/24/2019
uid: security/authentication/customize_identity_model
ms.openlocfilehash: 53ce77e20722f3ba3282ff4455a0b70d30e635b0
ms.sourcegitcommit: ffe3ed7921ec6c7c70abaac1d10703ec9a43374c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/11/2019
ms.locfileid: "65536018"
---
# <a name="identity-model-customization-in-aspnet-core"></a>Personalización del modelo de identidad en ASP.NET Core

Por [Arthur Vickers](https://github.com/ajcvickers)

Identidad de ASP.NET Core proporciona un marco para administrar y almacenar las cuentas de usuario en aplicaciones ASP.NET Core. Identidad se agrega al proyecto cuando **cuentas de usuario individuales** está seleccionado como el mecanismo de autenticación. De forma predeterminada, identidad hace uso de Entity Framework (EF) modelo de datos principal. En este artículo se describe cómo personalizar el modelo de identidad.

## <a name="identity-and-ef-core-migrations"></a>Identidad y migraciones de EF Core

Antes de examinar el modelo, es útil comprender cómo funciona la identidad con [migraciones de EF Core](/ef/core/managing-schemas/migrations/) para crear y actualizar una base de datos. En el nivel superior, el proceso es:

1. Definir o actualizar un [modelo de datos en el código](/ef/core/modeling/).
1. Agregue una migración para convertir este modelo en los cambios que se pueden aplicar a la base de datos.
1. Compruebe que la migración representa correctamente sus intenciones.
1. Aplique la migración para actualizar la base de datos estén sincronizadas con el modelo.
1. Repita los pasos del 1 al 4 para perfeccionar el modelo y mantener sincronizada la base de datos.

Para agregar y aplicar migraciones, use uno de los enfoques siguientes:

* El **Package Manager Console** ventana de (PMC) si usa Visual Studio. Para obtener más información, consulte [herramientas de EF Core PMC](/ef/core/miscellaneous/cli/powershell).
* La CLI de .NET Core si usa la línea de comandos. Para obtener más información, consulte [herramientas de línea de comandos de EF Core .NET](/ef/core/miscellaneous/cli/dotnet).
* Al hacer clic en el **aplicar migraciones** botón en la página de error cuando se ejecuta la aplicación.

ASP.NET Core tiene un controlador de la página de error de tiempo de desarrollo. El controlador puede aplicar las migraciones cuando se ejecuta la aplicación. Las aplicaciones de producción normalmente generan scripts SQL a partir de las migraciones e implementación cambios de base de datos como parte de una aplicación controlada y la implementación de la base de datos.

Cuando se crea una nueva aplicación mediante la identidad, ya se han completado los pasos 1 y 2 anteriores. Es decir, el modelo de datos iniciales ya existe y la migración inicial se ha agregado al proyecto. La migración inicial todavía debe aplicarse a la base de datos. Puede aplicar la migración inicial a través de uno de los métodos siguientes:

* Ejecute `Update-Database` en la PMC.
* Ejecute `dotnet ef database update` en un shell de comandos.
* Haga clic en el **aplicar migraciones** botón en la página de error cuando se ejecuta la aplicación.

Repita los pasos anteriores según se realizan cambios en el modelo.

## <a name="the-identity-model"></a>El modelo de identidad

### <a name="entity-types"></a>Tipos de entidad

El modelo de identidad consta de los siguientes tipos de entidad.

|Tipo de entidad|Descripción                                                  |
|-----------|-------------------------------------------------------------|
|`User`     |Representa al usuario.                                         |
|`Role`     |Representa un rol.                                           |
|`UserClaim`|Representa una notificación que posee un usuario.                    |
|`UserToken`|Representa un token de autenticación para un usuario.               |
|`UserLogin`|Asocia un usuario a un inicio de sesión.                              |
|`RoleClaim`|Representa una notificación que se concede a todos los usuarios dentro de un rol.|
|`UserRole` |Una entidad de combinación que se asocia a los usuarios y roles.               |

### <a name="entity-type-relationships"></a>Relaciones entre tipos de entidad

El [tipos de entidad](#entity-types) están relacionados entre sí de las maneras siguientes:

* Cada `User` puede tener muchos `UserClaims`.
* Cada `User` puede tener muchos `UserLogins`.
* Cada `User` puede tener muchos `UserTokens`.
* Cada `Role` puede tener muchos asociados `RoleClaims`.
* Cada `User` puede tener muchos asociados `Roles`y cada `Role` puede asociarse con muchos `Users`. Se trata de una relación de varios a varios que requiere una tabla combinada en la base de datos. La tabla de combinación se representa mediante el `UserRole` entidad.

### <a name="default-model-configuration"></a>Configuración predeterminada del modelo

Identidad define muchos *las clases de contexto* que heredan de <xref:Microsoft.EntityFrameworkCore.DbContext> para configurar y usar el modelo. Esta configuración se realiza mediante el [EF Core Fluent API de Code First](/ef/core/modeling/) en el <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating*> método de la clase de contexto. La configuración predeterminada es:

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

### <a name="model-generic-types"></a>Tipos genéricos de modelo

Identidad define el valor predeterminado [Common Language Runtime](/dotnet/standard/glossary#clr) tipos (CLR) para cada uno de los tipos de entidad mencionados anteriormente. Estos tipos tienen antepuesto el prefijo *identidad*:

* `IdentityUser`
* `IdentityRole`
* `IdentityUserClaim`
* `IdentityUserToken`
* `IdentityUserLogin`
* `IdentityRoleClaim`
* `IdentityUserRole`

En lugar de usar directamente estos tipos, se pueden usar los tipos como clases base para los tipos de la aplicación. El `DbContext` clases definidas por la identidad son genéricas, tal que se pueden usar diferentes tipos CLR para uno o varios de los tipos de entidad en el modelo. Estos tipos genéricos también permiten la `User` principal (PK) datos tipo de clave que se puede cambiar.

Cuando se usa la identidad con el soporte técnico para los roles, un <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext> se debe usar la clase. Por ejemplo:

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

También es posible usar identidad sin roles (solo notificaciones), en cuyo caso un <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUserContext%601> se debe usar la clase:

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

## <a name="customize-the-model"></a>Personalizar el modelo

El punto de partida para la personalización del modelo consiste en derivar del tipo de contexto apropiado. Consulte la [tipos genéricos de modelo](#model-generic-types) sección. Este tipo de contexto se denomina habitualmente `ApplicationDbContext` y se crea mediante las plantillas de ASP.NET Core.

El contexto se utiliza para configurar el modelo de dos maneras:

* Proporciona la entidad y tipos de clave para los parámetros de tipo genérico.
* Reemplazar `OnModelCreating` para modificar la asignación de estos tipos.

Cuando se reemplaza `OnModelCreating`, `base.OnModelCreating` se debe llamar primero; la configuración de invalidación debe llamarse a continuación. EF Core generalmente tiene una directiva de última uno-wins para la configuración. Por ejemplo, si el `ToTable` para un tipo de entidad es llamar primero al método con el nombre de una tabla y, a continuación, de nuevo más tarde con un nombre de tabla diferente, el nombre de tabla en la segunda llamada se utiliza.

### <a name="custom-user-data"></a>Datos de usuario personalizada

<!--
set projNam=WebApp1
dotnet new webapp -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design 
dotnet aspnet-codegenerator identity  -dc ApplicationDbContext --useDefaultUI 
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
 -->

[Datos de usuario personalizada](xref:security/authentication/add-user-data) se admite mediante la herencia de `IdentityUser`. Es habitual para este tipo de nombre `ApplicationUser`:

```csharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

Use el `ApplicationUser` tipo como un argumento genérico para el contexto:

```csharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder builder)
    {
        base.OnModelCreating(builder);
    }
}
```

No es necesario invalidar `OnModelCreating` en el `ApplicationDbContext` clase. EF Core se asigna el `CustomTag` propiedad por convención. Sin embargo, la base de datos debe actualizarse para crear un nuevo `CustomTag` columna. Para crear la columna, agregar una migración y, a continuación, actualice la base de datos, como se describe en [identidad y migraciones de EF Core](#identity-and-ef-core-migrations).

Actualización *Pages/Shared/_LoginPartial.cshtml* y reemplace `IdentityUser` con `ApplicationUser`:

```cshtml
@using Microsoft.AspNetCore.Identity
@using WebApp1.Areas.Identity.Data
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager
```

Actualización *Areas/Identity/IdentityHostingStartup.cs* o `Startup.ConfigureServices` y reemplace `IdentityUser` con `ApplicationUser`.

```csharp
services.AddDefaultIdentity<ApplicationUser>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultUI();
```

En ASP.NET Core 2.1 o posterior, la identidad se proporciona como una biblioteca de clases de Razor. Para obtener más información, consulta <xref:security/authentication/scaffold-identity>. Por lo tanto, el código anterior requiere una llamada a <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>. Si el proveedor de scaffolding de identidad se usó para agregar archivos de identidad para el proyecto, quite la llamada a `AddDefaultUI`. Para obtener más información, consulte:

* [Identidad de scaffolding](xref:security/authentication/scaffold-identity)
* [Agregar, descargar y eliminar datos de usuario personalizada para la identidad](xref:security/authentication/add-user-data)

### <a name="change-the-primary-key-type"></a>Cambiar el tipo de clave principal

Un cambio en el tipo de datos de la columna de clave principal una vez creada la base de datos es problemático en muchos sistemas de base de datos. Cambiar la clave principal normalmente implica quitar y volver a crear la tabla. Por lo tanto, los tipos de clave deben especificarse en la migración inicial cuando se crea la base de datos.

Siga estos pasos para cambiar el tipo de clave principal:

1. Si la base de datos se creó antes del cambio de clave principal, ejecute `Drop-Database` (PMC) o `dotnet ef database drop` (CLI de .NET Core) para eliminarlo.
2. Después de confirmar la eliminación de la base de datos, quite la migración inicial con `Remove-Migration` (PMC) o `dotnet ef migrations remove` (CLI de .NET Core).
3. Actualización de la `ApplicationDbContext` clase derive de <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext%603>. Especifique el nuevo tipo de clave para `TKey`. Por ejemplo, para usar un `Guid` tipo de clave:

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

    En el código anterior, las clases genéricas <xref:Microsoft.AspNetCore.Identity.IdentityUser%601> y <xref:Microsoft.AspNetCore.Identity.IdentityRole%601> debe especificarse para usar el nuevo tipo de clave.

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    En el código anterior, las clases genéricas <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUser%601> y <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityRole%601> debe especificarse para usar el nuevo tipo de clave.

    ::: moniker-end

    `Startup.ConfigureServices` debe actualizarse para utilizar el usuario genérico:

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

4. Si un personalizado `ApplicationUser` clase se utiliza, actualice la clase para heredar de `IdentityUser`. Por ejemplo:

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Models/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    ::: moniker range=">= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    Actualización `ApplicationDbContext` para hacer referencia a la personalizada `ApplicationUser` clase:

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

    Registrar la clase de contexto de base de datos personalizados al agregar el servicio de identidad en `Startup.ConfigureServices`:

    ::: moniker range=">= aspnetcore-2.1"

    ```csharp
    services.AddDefaultIdentity<ApplicationUser>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultUI()
            .AddDefaultTokenProviders();
    ```

    Tipo de datos de la clave principal se deduce mediante el análisis de la <xref:Microsoft.EntityFrameworkCore.DbContext> objeto.

    En ASP.NET Core 2.1 o posterior, la identidad se proporciona como una biblioteca de clases de Razor. Para obtener más información, consulta <xref:security/authentication/scaffold-identity>. Por lo tanto, el código anterior requiere una llamada a <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>. Si el proveedor de scaffolding de identidad se usó para agregar archivos de identidad para el proyecto, quite la llamada a `AddDefaultUI`.

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    Tipo de datos de la clave principal se deduce mediante el análisis de la <xref:Microsoft.EntityFrameworkCore.DbContext> objeto.

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
            .AddDefaultTokenProviders();
    ```

    El <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> método acepta un `TKey` tipo que indica el tipo de datos de la clave principal.

    ::: moniker-end

5. Si un personalizado `ApplicationRole` clase se utiliza, actualice la clase para heredar de `IdentityRole<TKey>`. Por ejemplo:

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationRole.cs?name=snippet_ApplicationRole&highlight=4)]

    Actualización `ApplicationDbContext` para hacer referencia a la personalizada `ApplicationRole` clase. Por ejemplo, la clase siguiente hace referencia a una personalizada `ApplicationUser` y personalizada `ApplicationRole`:

    ::: moniker range=">= aspnetcore-2.1"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Registrar la clase de contexto de base de datos personalizados al agregar el servicio de identidad en `Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=13-16)]

    Tipo de datos de la clave principal se deduce mediante el análisis de la <xref:Microsoft.EntityFrameworkCore.DbContext> objeto.

    En ASP.NET Core 2.1 o posterior, la identidad se proporciona como una biblioteca de clases de Razor. Para obtener más información, consulta <xref:security/authentication/scaffold-identity>. Por lo tanto, el código anterior requiere una llamada a <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>. Si el proveedor de scaffolding de identidad se usó para agregar archivos de identidad para el proyecto, quite la llamada a `AddDefaultUI`.

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Registrar la clase de contexto de base de datos personalizados al agregar el servicio de identidad en `Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    Tipo de datos de la clave principal se deduce mediante el análisis de la <xref:Microsoft.EntityFrameworkCore.DbContext> objeto.

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Registrar la clase de contexto de base de datos personalizados al agregar el servicio de identidad en `Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    El <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> método acepta un `TKey` tipo que indica el tipo de datos de la clave principal.

    ::: moniker-end

### <a name="add-navigation-properties"></a>Agregar las propiedades de navegación

Cambiar la configuración del modelo para las relaciones puede ser más difícil que realizar otros cambios. Debe tener cuidado para reemplazar las relaciones existentes en lugar de crear nuevas relaciones adicionales. En concreto, la relación modificada debe especificar la propiedad de clave externa misma (FK) como la relación existente. Por ejemplo, la relación entre `Users` y `UserClaims` es, de forma predeterminada, se especifica como sigue:

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

La clave externa de esta relación se especifica como el `UserClaim.UserId` propiedad. `HasMany` y `WithOne` se llama sin argumentos para crear la relación sin propiedades de navegación.

Agregar una propiedad de navegación `ApplicationUser` que permite asociados `UserClaims` hacer referencia a ella desde el usuario:

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

El `TKey` para `IdentityUserClaim<TKey>` es del tipo especificado para la clave principal de los usuarios. En este caso, `TKey` es `string` porque se usan los valores predeterminados. Tiene **no** el tipo de clave principal para el `UserClaim` tipo de entidad.

Ahora que existe la propiedad de navegación, deben configurarse en `OnModelCreating`:

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

Tenga en cuenta que se configura la relación exactamente igual que antes, solo con una propiedad de navegación especificada en la llamada a `HasMany`.

Las propiedades de navegación solo existen en el modelo EF, no en la base de datos. Dado que no ha cambiado la clave externa para la relación, este tipo de cambio de modelo no requiere la base de datos se puede actualizar. Esto se puede comprobar mediante la adición de una migración después de realizar el cambio. El `Up` y `Down` métodos están vacíos.

### <a name="add-all-user-navigation-properties"></a>Agregar todas las propiedades de navegación del usuario

Con la sección anterior como guía, en el ejemplo siguiente se configura las propiedades de navegación unidireccional para todas las relaciones de usuario:

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

### <a name="add-user-and-role-navigation-properties"></a>Agregar las propiedades de navegación del usuario y el rol

Uso de la sección anterior como guía, en el ejemplo siguiente se configura las propiedades de navegación para todas las relaciones de usuario y el rol:

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

Notas:

* En este ejemplo también incluye el `UserRole` unirse a la entidad, que es necesaria para navegar por la relación de muchos a muchos de los usuarios a Roles.
* No olvide cambiar los tipos de las propiedades de navegación para reflejar ese hecho `ApplicationXxx` tipos ahora se utilizan en lugar de `IdentityXxx` tipos.
* Recuerde que debe usar el `ApplicationXxx` en el modelo genérico `ApplicationContext` definición.

### <a name="add-all-navigation-properties"></a>Agregar todas las propiedades de navegación

Uso de la sección anterior como guía, en el ejemplo siguiente se configura las propiedades de navegación para todas las relaciones en todos los tipos de entidad:

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

### <a name="use-composite-keys"></a>Use las claves compuestas

Las secciones anteriores muestran el cambio del tipo de clave que se usa en el modelo de identidad. Cambiar el modelo de identidad clave para usar las claves compuestas no es compatible ni recomendable. Usar una clave compuesta con identidad implica la modificación de cómo interactúa el código del Administrador de identidades con el modelo. Esta personalización está fuera del ámbito de este documento.

### <a name="change-tablecolumn-names-and-facets"></a>Cambiar los nombres de tabla o columna y las facetas

Para cambiar los nombres de tablas y columnas, llame a `base.OnModelCreating`. A continuación, agregue la configuración para reemplazar cualquiera de los valores predeterminados. Por ejemplo, para cambiar el nombre de todas las tablas de identidad:

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

Estos ejemplos utilizan los tipos de identidad predeterminada. Si usa un tipo de aplicación, como `ApplicationUser`, configurar ese tipo en lugar del tipo predeterminado.

El ejemplo siguiente cambia algunos nombres de columna:

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

Algunos tipos de columnas de la base de datos pueden configurarse con determinados *facetas* (por ejemplo, el máximo `string` longitud permitida). El ejemplo siguiente establece las longitudes de columna máximo para varios `string` propiedades en el modelo:

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

### <a name="map-to-a-different-schema"></a>Asignar a un esquema diferente

Los esquemas pueden comportarse de manera diferente a través de proveedores de base de datos. Para SQL Server, el valor predeterminado es crear todas las tablas de la *dbo* esquema. Las tablas pueden crearse en un esquema diferente. Por ejemplo:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

::: moniker range=">= aspnetcore-2.1"

### <a name="lazy-loading"></a>Carga diferida

En esta sección, se agrega compatibilidad con servidores proxy de la carga diferida en el modelo de identidad. Carga diferida es útil porque permite que las propiedades de navegación que se utilizará sin garantizar primero que están cargado.

Tipos de entidad pueden realizarse adecuados para diferida carga de varias maneras, como se describe en el [documentación de EF Core](/ef/core/querying/related-data#lazy-loading). Por motivos de simplicidad, use servidores proxy de la carga diferida, lo que requiere:

* Instalación de la [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) paquete.
* Una llamada a <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies*> dentro de <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext*>.
* Tipos de entidades públicas con `public virtual` las propiedades de navegación.

El ejemplo siguiente se muestra cómo llamar `UseLazyLoadingProxies` en `Startup.ConfigureServices`:

```csharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
              .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Consulte los ejemplos anteriores para obtener instrucciones sobre cómo agregar propiedades de navegación a los tipos de entidad.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:security/authentication/scaffold-identity>

::: moniker-end
