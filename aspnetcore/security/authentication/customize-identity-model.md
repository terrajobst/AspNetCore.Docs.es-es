---
title: Personalización del modelo de identidad en ASP.NET Core
author: ajcvickers
description: En este artículo se describe cómo personalizar el modelo de datos Entity Framework Core subyacente para la identidad de ASP.NET Core.
ms.author: avickers
ms.date: 07/01/2019
uid: security/authentication/customize_identity_model
ms.openlocfilehash: f549fdff4a416b5fadcb2b1078b051bbab8e402e
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78651461"
---
# <a name="identity-model-customization-in-aspnet-core"></a>Personalización del modelo de identidad en ASP.NET Core

Por [Arthur Vickers](https://github.com/ajcvickers)

ASP.NET Core identidad proporciona un marco para administrar y almacenar cuentas de usuario en ASP.NET Core aplicaciones. La identidad se agrega al proyecto cuando se selecciona **cuentas de usuario individuales** como mecanismo de autenticación. De forma predeterminada, la identidad hace uso de un modelo de datos básico de Entity Framework (EF). En este artículo se describe cómo personalizar el modelo de identidad.

## <a name="identity-and-ef-core-migrations"></a>Migraciones de identidad y EF Core

Antes de examinar el modelo, resulta útil entender cómo funciona la identidad con [EF Core migraciones](/ef/core/managing-schemas/migrations/) para crear y actualizar una base de datos. En el nivel superior, el proceso es:

1. Definir o actualizar un [modelo de datos en el código](/ef/core/modeling/).
1. Agregue una migración para traducir este modelo a los cambios que se pueden aplicar a la base de datos.
1. Compruebe que la migración representa correctamente sus intenciones.
1. Aplique la migración para actualizar la base de datos y sincronizarla con el modelo.
1. Repita los pasos del 1 al 4 para refinar aún más el modelo y mantener la base de datos sincronizada.

Use uno de los métodos siguientes para agregar y aplicar migraciones:

* La ventana de la **consola del administrador de paquetes** (PMC) si usa Visual Studio. Para obtener más información, vea [EF Core herramientas PMC](/ef/core/miscellaneous/cli/powershell).
* CLI de .NET Core si se usa la línea de comandos. Para obtener más información, vea [EF Core herramientas de línea de comandos de .net](/ef/core/miscellaneous/cli/dotnet).
* Al hacer clic en el botón **aplicar migraciones** en la página de error cuando se ejecuta la aplicación.

ASP.NET Core tiene un controlador de página de errores en tiempo de desarrollo. El controlador puede aplicar migraciones cuando se ejecuta la aplicación. Normalmente, las aplicaciones de producción generan scripts SQL desde las migraciones e implementan cambios de base de datos como parte de una implementación controlada de una aplicación y una base de datos.

Cuando se crea una nueva aplicación con la identidad, los pasos 1 y 2 anteriores ya se han completado. Es decir, el modelo de datos inicial ya existe y la migración inicial se ha agregado al proyecto. Todavía es necesario aplicar la migración inicial a la base de datos. La migración inicial puede aplicarse mediante uno de los métodos siguientes:

* Ejecute `Update-Database` en PMC.
* Ejecute `dotnet ef database update` en un shell de comandos.
* Haga clic en el botón **aplicar migraciones** en la página de error cuando se ejecute la aplicación.

Repita los pasos anteriores a medida que se realicen cambios en el modelo.

## <a name="the-identity-model"></a>El modelo de identidad

### <a name="entity-types"></a>Tipos de entidades

El modelo de identidad se compone de los siguientes tipos de entidad.

|Tipo de entidad|Descripción                                                  |
|-----------|-------------------------------------------------------------|
|`User`     |Representa al usuario.                                         |
|`Role`     |Representa un rol.                                           |
|`UserClaim`|Representa una demanda que un usuario posee.                    |
|`UserToken`|Representa un token de autenticación para un usuario.               |
|`UserLogin`|Asocia un usuario a un inicio de sesión.                              |
|`RoleClaim`|Representa una demanda que se concede a todos los usuarios de un rol.|
|`UserRole` |Entidad de combinación que asocia usuarios y roles.               |

### <a name="entity-type-relationships"></a>Relaciones de tipo de entidad

Los [tipos de entidad](#entity-types) se relacionan entre sí de las siguientes maneras:

* Cada `User` puede tener muchos `UserClaims`.
* Cada `User` puede tener muchos `UserLogins`.
* Cada `User` puede tener muchos `UserTokens`.
* Cada `Role` puede tener muchos `RoleClaims`asociados.
* Cada `User` puede tener muchos `Roles`asociados y cada `Role` se puede asociar a muchos `Users`. Se trata de una relación de varios a varios que requiere una tabla de combinación en la base de datos. La tabla de combinación está representada por la entidad `UserRole`.

### <a name="default-model-configuration"></a>Configuración de modelo predeterminada

Identity define muchas *clases de contexto* que heredan de [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) para configurar y usar el modelo. Esta configuración se realiza mediante el [EF Core Code First API fluida](/ef/core/modeling/) en el método [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating) de la clase de contexto. La configuración predeterminada es la siguiente:

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

Identity define los tipos de [Common Language Runtime](/dotnet/standard/glossary#clr) (CLR) predeterminados para cada uno de los tipos de entidad enumerados anteriormente. Todos estos tipos tienen el prefijo *identidad*:

* `IdentityUser`
* `IdentityRole`
* `IdentityUserClaim`
* `IdentityUserToken`
* `IdentityUserLogin`
* `IdentityRoleClaim`
* `IdentityUserRole`

En lugar de usar estos tipos directamente, los tipos se pueden usar como clases base para los propios tipos de la aplicación. Las clases de `DbContext` definidas por la identidad son genéricas, de modo que se pueden usar diferentes tipos CLR para uno o varios de los tipos de entidad del modelo. Estos tipos genéricos también permiten cambiar el tipo de datos de la clave principal (PK) de `User`.

Al usar la identidad compatible con los roles, se debe usar una clase <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>. Por ejemplo:

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

También es posible usar la identidad sin roles (solo notificaciones), en cuyo caso se debe usar una clase de <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUserContext%601>:

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

El punto de partida para la personalización del modelo es derivar del tipo de contexto adecuado. Vea la sección [tipos genéricos de modelo](#model-generic-types) . Este tipo de contexto se denomina normalmente `ApplicationDbContext` y lo crean las plantillas de ASP.NET Core.

El contexto se utiliza para configurar el modelo de dos maneras:

* Proporcionar tipos de entidad y clave para los parámetros de tipo genérico.
* Reemplazar `OnModelCreating` para modificar la asignación de estos tipos.

Al reemplazar `OnModelCreating`, se debe llamar primero a `base.OnModelCreating`. la configuración de invalidación debe llamarse a continuación. Por lo general, EF Core tiene una directiva de el último de uno a WINS para la configuración. Por ejemplo, si se llama primero al método `ToTable` para un tipo de entidad con un nombre de tabla y después de nuevo con un nombre de tabla diferente, se utiliza el nombre de tabla de la segunda llamada.

### <a name="custom-user-data"></a>Datos de usuario personalizados

<!--
set projNam=WebApp1
dotnet new webapp -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design 
dotnet aspnet-codegenerator identity  -dc ApplicationDbContext --useDefaultUI 
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
 -->

Los [datos de usuario personalizados](xref:security/authentication/add-user-data) se admiten heredando de `IdentityUser`. Es un nombre personalizado para denominar este tipo `ApplicationUser`:

```csharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

Use el tipo de `ApplicationUser` como argumento genérico para el contexto:

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

No es necesario invalidar `OnModelCreating` en la clase `ApplicationDbContext`. EF Core asigna la propiedad `CustomTag` por Convención. Sin embargo, es necesario actualizar la base de datos para crear una nueva `CustomTag` columna. Para crear la columna, agregue una migración y, a continuación, actualice la base de datos como se describe en el artículo sobre las [migraciones de identidad y EF Core](#identity-and-ef-core-migrations).

Actualice *pages/Shared/_LoginPartial. cshtml* y reemplace `IdentityUser` por `ApplicationUser`:

```cshtml
@using Microsoft.AspNetCore.Identity
@using WebApp1.Areas.Identity.Data
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager
```

Actualice *areas/Identity/IdentityHostingStartup. CS* o `Startup.ConfigureServices` y reemplace `IdentityUser` por `ApplicationUser`.

```csharp
services.AddDefaultIdentity<ApplicationUser>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultUI();
```

En ASP.NET Core 2,1 o posterior, la identidad se proporciona como una biblioteca de clases de Razor. Para más información, consulte <xref:security/authentication/scaffold-identity>. Por consiguiente, el código anterior requiere una llamada a <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>. Si el scaffolding de identidad se utilizó para agregar archivos de identidad al proyecto, quite la llamada a `AddDefaultUI`. Para más información, consulte:

* [Identidad de scaffolding](xref:security/authentication/scaffold-identity)
* [Agregar, descargar y eliminar datos de usuario personalizados a la identidad](xref:security/authentication/add-user-data)

### <a name="change-the-primary-key-type"></a>Cambiar el tipo de clave principal

Un cambio en el tipo de datos de la columna PK después de crear la base de datos es problemático en muchos sistemas de base de datos. Cambiar la PK normalmente implica quitar y volver a crear la tabla. Por lo tanto, los tipos de clave se deben especificar en la migración inicial cuando se crea la base de datos.

Siga estos pasos para cambiar el tipo de PK:

1. Si la base de datos se creó antes del cambio de la PK, ejecute `Drop-Database` (PMC) o `dotnet ef database drop` (CLI de .NET Core) para eliminarla.
2. Después de confirmar la eliminación de la base de datos, quite la migración inicial con `Remove-Migration` (PMC) o `dotnet ef migrations remove` (CLI de .NET Core).
3. Actualice la clase `ApplicationDbContext` para que se derive de <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext%603>. Especifique el nuevo tipo de clave para `TKey`. Por ejemplo, para usar un tipo de clave `Guid`:

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

    En el código anterior, se deben especificar las clases genéricas <xref:Microsoft.AspNetCore.Identity.IdentityUser%601> y <xref:Microsoft.AspNetCore.Identity.IdentityRole%601> para usar el nuevo tipo de clave.

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    En el código anterior, se deben especificar las clases genéricas <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUser%601> y <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityRole%601> para usar el nuevo tipo de clave.

    ::: moniker-end

    `Startup.ConfigureServices` debe actualizarse para usar el usuario genérico:

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

4. Si se utiliza una clase de `ApplicationUser` personalizada, actualice la clase para que herede de `IdentityUser`. Por ejemplo:

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Models/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    ::: moniker range=">= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    Actualice `ApplicationDbContext` para hacer referencia a la clase de `ApplicationUser` personalizada:

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

    Registre la clase de contexto de base de datos personalizada al agregar el servicio de identidad en `Startup.ConfigureServices`:

    ::: moniker range=">= aspnetcore-2.1"

    ```csharp
    services.AddDefaultIdentity<ApplicationUser>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultUI()
            .AddDefaultTokenProviders();
    ```

    El tipo de datos de la clave principal se deduce mediante el análisis del objeto [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) .

    En ASP.NET Core 2,1 o posterior, la identidad se proporciona como una biblioteca de clases de Razor. Para más información, consulte <xref:security/authentication/scaffold-identity>. Por consiguiente, el código anterior requiere una llamada a <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>. Si el scaffolding de identidad se utilizó para agregar archivos de identidad al proyecto, quite la llamada a `AddDefaultUI`.

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    El tipo de datos de la clave principal se deduce mediante el análisis del objeto [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) .

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
            .AddDefaultTokenProviders();
    ```

    El método <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> acepta un tipo `TKey` que indica el tipo de datos de la clave principal.

    ::: moniker-end

5. Si se utiliza una clase de `ApplicationRole` personalizada, actualice la clase para que herede de `IdentityRole<TKey>`. Por ejemplo:

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationRole.cs?name=snippet_ApplicationRole&highlight=4)]

    Actualice `ApplicationDbContext` para hacer referencia a la clase de `ApplicationRole` personalizada. Por ejemplo, la clase siguiente hace referencia a un `ApplicationUser` personalizado y a un `ApplicationRole`personalizado:

    ::: moniker range=">= aspnetcore-2.1"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Registre la clase de contexto de base de datos personalizada al agregar el servicio de identidad en `Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=13-16)]

    El tipo de datos de la clave principal se deduce mediante el análisis del objeto [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) .

    En ASP.NET Core 2,1 o posterior, la identidad se proporciona como una biblioteca de clases de Razor. Para más información, consulte <xref:security/authentication/scaffold-identity>. Por consiguiente, el código anterior requiere una llamada a <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>. Si el scaffolding de identidad se utilizó para agregar archivos de identidad al proyecto, quite la llamada a `AddDefaultUI`.

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Registre la clase de contexto de base de datos personalizada al agregar el servicio de identidad en `Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    El tipo de datos de la clave principal se deduce mediante el análisis del objeto [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) .

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Registre la clase de contexto de base de datos personalizada al agregar el servicio de identidad en `Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    El método <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> acepta un tipo `TKey` que indica el tipo de datos de la clave principal.

    ::: moniker-end

### <a name="add-navigation-properties"></a>Agregar propiedades de navegación

Cambiar la configuración del modelo para las relaciones puede ser más difícil que realizar otros cambios. Se debe tener cuidado para reemplazar las relaciones existentes en lugar de crear nuevas relaciones adicionales. En concreto, la relación modificada debe especificar la misma propiedad de clave externa (FK) que la relación existente. Por ejemplo, la relación entre `Users` y `UserClaims` es, de forma predeterminada, especificada de la siguiente manera:

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

El parámetro FK para esta relación se especifica como la propiedad `UserClaim.UserId`. se llama a `HasMany` y `WithOne` sin argumentos para crear la relación sin las propiedades de navegación.

Agregue una propiedad de navegación a `ApplicationUser` que permita que el usuario pueda hacer referencia a `UserClaims` asociados:

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

La `TKey` para `IdentityUserClaim<TKey>` es el tipo especificado para la clave principal de los usuarios. En este caso, `TKey` se `string` porque se usan los valores predeterminados. **No** es el tipo PK del tipo de entidad `UserClaim`.

Ahora que la propiedad de navegación existe, debe configurarse en `OnModelCreating`:

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

Observe que la relación se configura exactamente como estaba antes, solo con una propiedad de navegación especificada en la llamada a `HasMany`.

Las propiedades de navegación solo existen en el modelo EF, no en la base de datos. Dado que el FK de la relación no ha cambiado, este tipo de cambio de modelo no requiere que se actualice la base de datos. Para comprobarlo, agregue una migración después de realizar el cambio. Los métodos `Up` y `Down` están vacíos.

### <a name="add-all-user-navigation-properties"></a>Agregar todas las propiedades de navegación de usuario

Con la sección anterior como guía, en el ejemplo siguiente se configuran las propiedades de navegación unidireccional para todas las relaciones del usuario:

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

### <a name="add-user-and-role-navigation-properties"></a>Agregar propiedades de navegación de usuario y rol

Con la sección anterior como guía, en el ejemplo siguiente se configuran las propiedades de navegación para todas las relaciones en el usuario y el rol:

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

* En este ejemplo también se incluye la entidad `UserRole` join, que es necesaria para navegar por la relación de varios a varios entre usuarios y roles.
* Recuerde cambiar los tipos de las propiedades de navegación para reflejar que ahora se usan tipos de `ApplicationXxx` en lugar de tipos de `IdentityXxx`.
* Recuerde usar el `ApplicationXxx` en la definición de `ApplicationContext` genérico.

### <a name="add-all-navigation-properties"></a>Agregar todas las propiedades de navegación

Con la sección anterior como guía, en el ejemplo siguiente se configuran las propiedades de navegación para todas las relaciones en todos los tipos de entidad:

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

### <a name="use-composite-keys"></a>Usar claves compuestas

En las secciones anteriores se mostró cómo cambiar el tipo de clave que se usa en el modelo de identidad. No se admite ni se recomienda cambiar el modelo de clave de identidad para utilizar claves compuestas. El uso de una clave compuesta con identidad implica cambiar el modo en que el código de Identity Manager interactúa con el modelo. Esta personalización está fuera del ámbito de este documento.

### <a name="change-tablecolumn-names-and-facets"></a>Cambiar los nombres de tabla/columna y las caras

Para cambiar los nombres de tablas y columnas, llame a `base.OnModelCreating`. A continuación, agregue la configuración para invalidar cualquiera de los valores predeterminados. Por ejemplo, para cambiar el nombre de todas las tablas de identidad:

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

En estos ejemplos se usan los tipos de identidad predeterminados. Si usa un tipo de aplicación como `ApplicationUser`, configure ese tipo en lugar del tipo predeterminado.

En el ejemplo siguiente se cambian algunos nombres de columna:

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

Algunos tipos de columnas de base de datos se pueden configurar con ciertas *caras* (por ejemplo, la longitud máxima de `string` permitida). En el ejemplo siguiente se establecen las longitudes máximas de columna para varias propiedades de `string` del modelo:

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

Los esquemas pueden comportarse de manera diferente en los proveedores de bases de datos. Por SQL Server, el valor predeterminado es crear todas las tablas en el esquema *DBO* . Las tablas se pueden crear en un esquema diferente. Por ejemplo:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

::: moniker range=">= aspnetcore-2.1"

### <a name="lazy-loading"></a>Carga diferida

En esta sección, se agrega compatibilidad con los proxies de carga diferida en el modelo de identidad. La carga diferida resulta útil, ya que permite usar las propiedades de navegación sin asegurarse primero de que se cargan.

Los tipos de entidad se pueden hacer adecuados para la carga diferida de varias maneras, como se describe en la [documentación EF Core](/ef/core/querying/related-data#lazy-loading). Para simplificar, use servidores proxy de carga diferida, que requiere:

* Instalación del paquete [Microsoft. EntityFrameworkCore. Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) .
* Una llamada a <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies*> dentro de [AddDbContext\<TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).
* Tipos de entidad pública con propiedades de navegación de `public virtual`.

En el ejemplo siguiente se muestra cómo llamar a `UseLazyLoadingProxies` en `Startup.ConfigureServices`:

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
