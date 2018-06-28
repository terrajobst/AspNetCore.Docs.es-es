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
# <a name="identity-model-customization"></a>Personalización del modelo de identidad

Por [Arthur Vickers](https://github.com/ajcvickers)

Identidad de ASP.NET Core proporciona un marco para administrar y almacenar las cuentas de usuario en aplicaciones de ASP.NET Core. Identidad se agrega al proyecto cuando se selecciona "Cuentas de usuario individuales" como el mecanismo de autenticación. De forma predeterminada, identidad hace que use de un Entity Framework (EF) modelo de datos principal. En este artículo se describe cómo personalizar el modelo de identidad.

<a name="identity-migrations"></a>

## <a name="identity-and-ef-core-migrations"></a>Identidad y migraciones de núcleo EF

Antes de examinar el modelo, es útil para comprender cómo funciona la identidad con [EF Core migraciones](/ef/core/managing-schemas/migrations/) para crear y actualizar una base de datos. En el nivel superior, el proceso es:

1. Definir o actualizar una [modelo de datos en el código](/ef/core/modeling/).
1. Agregue una migración para este modelo se traducen en cambios que se pueden aplicar a la base de datos.
1. Compruebe que la migración representa correctamente sus intenciones.
1. Aplicar la migración para actualizar la base de datos para que esté sincronizado con el modelo.
1. Repita los pasos 1 a 4 para perfeccionar el modelo y mantener sincronizada la base de datos.

Las migraciones se agregan y aplican mediante:

* El [Package Manager Console](/ef/core/miscellaneous/cli/powershell) (PMC) si está utilizando Visual Studio.
* El [comandos dotnet](/ef/core/miscellaneous/cli/dotnet) en la línea de comandos.
* Si selecciona el vínculo de migraciones en la página de error cuando se ejecuta la aplicación.

ASP.NET Core tiene un controlador de la página de error de tiempo de desarrollo que puede utilizarse para las migraciones se aplican cuando se ejecuta la aplicación. Para las aplicaciones de producción, suele ser más adecuado para generar secuencias de comandos SQL a partir de las migraciones e implementarlas en la base de datos como parte de una implementación de aplicación y base de datos controlada.

Cuando se crea una nueva aplicación de uso de la identidad, ya se han completado los pasos 1 y 2 anteriores. Es decir, el modelo de datos iniciales ya existe, y la migración inicial se ha agregado al proyecto. La migración inicial todavía debe aplicarse a la base de datos. La migración inicial puede realizarse mediante la ejecución de la base de datos de actualización (PMC), el comando de actualización (.NET Core CLI) de base de datos de ef dotnet, o mediante la página de error cuando se ejecuta la aplicación.

Los pasos anteriores que deba repetir cuando se realizan cambios en el modelo.

<a name="identity-model"></a>

## <a name="the-identity-model"></a>El modelo de identidad

### <a name="entity-types"></a>Tipos de entidad

El modelo de identidad consta de siete tipos de entidad:

* `User` -representa al usuario
* `Role` -Representa un rol
* `UserClaim` -Representa una notificación que el usuario posea
* `UserToken` -Representa un token de autenticación para un usuario
* `UserLogin` -asocia un usuario a un inicio de sesión
* `RoleClaim` -Representa una notificación que se concede a todos los usuarios dentro de un rol
* `UserRole` -unirse a la entidad que se asocia a los usuarios y roles

### <a name="entity-type-relationships"></a>Relaciones de tipo de entidad

Estos tipos de entidad están relacionados entre sí de las maneras siguientes:

* Cada `User` puede tener muchos `UserClaims`
* Cada `User` puede tener muchos `UserLogins`
* Cada `User` puede tener muchos `UserTokens`
* Cada `Role` puede tener muchos asociados `RoleClaims`
* Cada `User` puede tener muchos asociados `Roles`y cada `Role` pueden asociarse con muchos usuarios
  * Se trata de una relación de varios a varios, lo que requiere una tabla de combinación en la base de datos. La tabla de combinación se representa mediante el `UserRole` entidad.

### <a name="default-model-configuration"></a>Configuración del modelo predeterminado

Identidad define una variedad de "clases de contexto", que se heredan de [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) para configurar y utilizar el modelo. Esta configuración se realiza mediante la [API fluida de la primera de código EF Core](/ef/core/modeling/) en el [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) método de la clase de contexto. La configuración predeterminada es:

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

### <a name="model-generic-types"></a>Tipos genéricos de modelo

Identidad define los tipos CLR predeterminada para cada uno de los tipos de entidad mencionados anteriormente. Todos estos tipos tienen el prefijo con "Identidad": `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, y `IdentityUserRole`.

En lugar de usar directamente estos tipos, los tipos pueden usarse como clases base para los tipos de la aplicación. El `DbContext` clases definidas por identidad establecen genéricas que diferentes tipos CLR pueden utilizarse para uno o varios de los tipos de entidad en el modelo. Estos tipos genéricos también permiten para el tipo de la clave principal de usuario va a cambiar.

Cuando se usa la identidad con compatibilidad para los roles, una [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) debe utilizarse la clase:

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

También es posible usar identidad sin roles (solo las notificaciones), en cuyo caso una `IdentityUserContext` debe utilizarse la clase:


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

## <a name="customizing-the-model"></a>Personalizar el modelo

El punto de partida para personalizar el modelo es derivar del tipo de contexto adecuado; Consulte la sección anterior. Este tipo de contexto habitualmente se denomina `ApplicationDbContext` y se crea mediante las plantillas de ASP.NET Core.

El contexto se utiliza para configurar el modelo de dos maneras:
* Proporcionando los tipos de entidad y la clave para los parámetros de tipo genérico.
* Mediante la invalidación `OnModelCreating` para modificar la asignación de estos tipos.

Al reemplazar `OnModelCreating`, `base.OnModelCreating` debe llamarse en primer lugar, el reemplazo configuración debe llamarse a continuación. EF Core generalmente tiene una directiva de última-uno-wins para la configuración. Por ejemplo, si la `ToTable` de una entidad es tipo llamado primero con el nombre de una tabla y, a continuación, vuelva a versiones posteriores con un nombre diferente, a continuación, el nombre de tabla en la segunda llamada es la que se utiliza.

### <a name="using-a-custom-user-type"></a>Uso de un tipo de usuario personalizado

Para usar un tipo de usuario personalizado, crea el tipo y hacer que hereden de `IdentityUser`. Es habitual para este tipo de nombre `ApplicationUser`. Este tipo normalmente tendrán propiedades adicionales no en el tipo base, en caso contrario, no habría ningún valor para su creación. Por ejemplo:

```CSharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

A continuación, use este tipo como argumento genérico para el contexto:

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

Actualización `ConfigureServices` para utilizar el nuevo `ApplicationUser` clase:

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

No es necesario invalidar `OnModelCreating` aquí porque EF Core asignará el `CustomTag` propiedad por convención. Sin embargo, tendrá la base de datos se actualicen para obtener una nueva `CustomTag` columna. Para ello, agregue una migración y, a continuación, actualice la base de datos como se describe en [identidad y migraciones de núcleo de EF](#identity-migrations).

### <a name="changing-the-key-type"></a>Cambiar el tipo de clave

Cambiar el tipo de una columna de clave principal (PK) una vez creada la base de datos es problemático en muchos sistemas de base de datos. Cambiar la clave principal normalmente hay que quitar y volver a crear la tabla. Por lo tanto, se recomienda que los tipos de clave se especifiquen en la migración inicial tal que los tipos de clave de destino se crean cuando se crea la base de datos.

Si se ha creado la base de datos, a continuación, `Drop-Database` (PMC) o `dotnet ef database drop` (.NET Core CLI) puede usarse para eliminarlo.

Una vez que se confirma que la base de datos no existe, quite la migración inicial con `Remove-Migration` (PMC) o `dotnet ef migrations remove` (.NET Core CLI).

Actualización de la `ApplicationDbContext` para utilizar una clase base diferente, especificando el nuevo tipo de clave para `TKey`. Por ejemplo, para utilizar un `Guid` clave:

```CSharp
public class ApplicationDbContext : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

Tenga en cuenta que las clases genéricas `IdentityUser<TKey>` y `IdentityRole<TKey>` también debe especificarse para usar el nuevo tipo de clave. `ConfigureServices` se deben actualizar para usar el usuario genérico:

```CSharp
services.AddDefaultIdentity<IdentityUser<Guid>>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Si un personalizado `ApplicationUser` está utilizándose, actualizarlo para heredar de `IdentityUser<TKey>`. Por ejemplo:

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

### <a name="adding-navigation-properties"></a>Agregar propiedades de navegación

Cambiar la configuración del modelo para las relaciones puede ser un más difícil que realizar otros cambios. Se debe tener cuidado para reemplazar las relaciones existentes en lugar de crear una nueva relación adicional. En concreto, la relación modificada debe especificar la misma propiedad de clave externa como la relación existente. Por ejemplo, la relación entre `Users` y `UserClaims` es por valor predeterminado especificado como:

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

La clave externa de esta relación se especifica como el `UserClaim.UserId` propiedad. `HasMany` y `WithOne` se llama sin argumentos para crear la relación sin propiedades de navegación.

Agregar una propiedad de navegación para `ApplicationUser` que le permitirá asociados `UserClaims` para hacer referencia desde el usuario:

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

Tenga en cuenta que la `TKey` para `IdentityUserClaim<TKey>` es el tipo especificado para la clave principal de los usuarios, en este caso `string` puesto que estamos usando los valores predeterminados. Es **no** el tipo de clave principal para el `UserClaim` tipo de entidad.

Ahora que existe la propiedad de navegación deben configurarse en `OnModelCreating`:

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

Tenga en cuenta que la relación está configurada exactamente igual que antes, solo con una propiedad de navegación especificada en la llamada a `HasMany`.

Las propiedades de navegación solo existen en el modelo EF, no en la base de datos. Dado que no ha cambiado la clave externa de la relación, este tipo de cambio en el modelo no requiere la base de datos se actualicen. Esto se puede comprobar mediante la adición de una migración después de realizar el cambio: el `Up` y `Down` métodos están vacíos.

### <a name="adding-all-user-navigation-properties"></a>Agregar todas las propiedades de navegación de usuario

Con la sección anterior como guía, mostramos un ejemplo que configura las propiedades de navegación unidireccional para todas las relaciones de usuario:

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

### <a name="adding-user-and-role-navigation-properties"></a>Agregar propiedades de navegación de usuario y el rol

Con la sección anterior como guía, mostramos un ejemplo que configura las propiedades de navegación para todas las relaciones de usuario y el rol:

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

Notas:

* En este ejemplo también incluye el `UserRole` unirse a la entidad, que se necesita para navegar por la relación de varios a varios de los usuarios a Roles.
* No olvide cambiar los tipos de las propiedades de navegación para reflejar que `ApplicationXxx` tipos ahora se utilizan en lugar de `IdentityXxx` tipos.
* Recuerde que debe usar el `ApplicationXxx` en la interfaz genérica `ApplicationContext` definición.

### <a name="adding-all-navigation-properties"></a>Agregar todas las propiedades de navegación

Con la sección anterior como guía, mostramos un ejemplo que configura las propiedades de navegación para todas las relaciones en todos los tipos de entidad:

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

### <a name="using-composite-keys"></a>Usando claves compuestas

En las secciones anteriores se muestran el cambio del tipo de clave que se usa en el modelo de identidad. No es compatible ni recomendable cambiar el modelo de clave de identidad para utilizar las claves compuestas. Usar una clave compuesta con identidad implicaría cambiar cómo el código del Administrador de identidades interactúa con el modelo, que es una personalización no compatible y fuera del ámbito de este documento.

### <a name="changing-tablecolumn-names-and-facets"></a>Cambiar nombres de tabla o columna y las facetas

Para cambiar los nombres de tablas y columnas, llame a `base.OnModelCreating`y, a continuación, agregue una configuración para reemplazar cualquiera de los valores predeterminados. Por ejemplo, para cambiar el nombre de todas las tablas de identidad:

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

Tenga en cuenta que estos ejemplos utilizan los tipos de identidad predeterminada. Si está utilizando un tipo de aplicación, como `ApplicationUser` , a continuación, configurar ese tipo en lugar del tipo de valor predeterminado.

Este es un ejemplo que cambia algunos nombres de columna:

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

Algunos tipos de columnas de base de datos pueden configurarse con determinados _facetas_ como la longitud de cadena máxima permitida. Este es un ejemplo que establece max longitudes de columna para varias propiedades de cadena en el modelo:

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

### <a name="mapping-to-a-different-schema"></a>Asignación a un esquema diferente

Los esquemas pueden comportarse de manera diferente en los proveedores de base de datos diferente, pero para SQL Server, el valor predeterminado es crear todas las tablas en el esquema "dbo". Esto se puede cambiar para crear en su lugar las tablas en un esquema diferente. Por ejemplo:

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

### <a name="lazy-loading"></a>Carga diferida

En esta sección se agrega compatibilidad con servidores proxy de carga diferida en el modelo de identidad. Lazy carga es útil porque permite que las propiedades de navegación que se utilizarán sin garantizar la primera que se cargan.

Tipos de entidad pueden realizarse adecuados para diferida de carga de varias maneras, como se describe en el [documentación principal de EF](/ef/core/querying/related-data#lazy-loading). Por simplicidad, utilizaremos a servidores proxy de la carga diferida, lo que requiere:

* Instalación de la [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) paquete.
* Una llamada a `.UseLazyLoadingProxies()` en `AddDbContext`.
* Tipos de entidades públicas con propiedades de navegación virtual pública.

Este es un ejemplo de llamar al método `.UseLazyLoadingProxies()`:

```CSharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
            .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Consulte los ejemplos anteriores para agregar propiedades de navegación a los tipos de entidad.
