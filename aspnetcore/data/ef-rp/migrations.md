---
title: 'Páginas de Razor con EF Core en ASP.NET Core: Migraciones (4 de 8)'
author: rick-anderson
description: En este tutorial, empezará a usar la característica de EF Core de migraciones para administrar los cambios de modelos de datos en una aplicación de ASP.NET Core MVC.
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/migrations
ms.openlocfilehash: 406b94d74a99dcec3b2ce01a5eb3313c48819148
ms.sourcegitcommit: 3376f224b47a89acf329b2d2f9260046a372f924
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/10/2019
ms.locfileid: "65517012"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a>Páginas de Razor con EF Core en ASP.NET Core: Migraciones (4 de 8)

Por [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) y [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

En este tutorial, se usa la característica de migraciones de EF Core para administrar cambios en el modelo de datos.

Si experimenta problemas que no puede resolver, descargue la [aplicación completada](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).

Cuando se desarrolla una aplicación nueva, el modelo de datos cambia con frecuencia. Cada vez que el modelo cambia, este deja de estar sincronizado con la base de datos. Este tutorial se inició con la configuración de Entity Framework para crear la base de datos si no existía. Cada vez que los datos del modelo cambian:

* Se quita la base de datos.
* EF crea una que coincide con el modelo.
* La aplicación inicializa la base de datos con datos de prueba.

Este enfoque para mantener la base de datos sincronizada con el modelo de datos funciona bien hasta que la aplicación se implemente en producción. Cuando se ejecuta la aplicación en producción, normalmente está almacenando datos que hay que mantener. No se puede iniciar la aplicación con una prueba de base de datos cada vez que se hace un cambio (por ejemplo, agregar una nueva columna). La característica Migraciones de EF Core soluciona este problema habilitando EF Core para actualizar el esquema de la base de datos en lugar de crear una.

En lugar de quitar y volver a crear la base de datos cuando los datos del modelo cambian, las migraciones actualizan el esquema y conservan los datos existentes.

## <a name="drop-the-database"></a>Eliminación de la base de datos

Use el **Explorador de objetos de SQL Server** (SSOX) o el comando `database drop`:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

En la **Consola del Administrador de paquetes** (PMC), ejecute el comando siguiente:

```PMC
Drop-Database
```

Ejecute `Get-Help about_EntityFrameworkCore` desde PMC para obtener información de ayuda.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Abra una ventana de comandos y desplácese hasta la carpeta del proyecto. La carpeta del proyecto contiene el archivo *Startup.cs*.

Escriba lo siguiente en la ventana de comandos:

 ```console
 dotnet ef database drop
 ```

---

## <a name="create-an-initial-migration-and-update-the-db"></a>Creación de una migración inicial y actualización de la base de datos

Compile el proyecto y cree la primera migración.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

---

### <a name="examine-the-up-and-down-methods"></a>Examinar los métodos Up y Down

El comando `migrations add` de EF Core ha generado código para crear la base de datos. Este código de migraciones se encuentra en el archivo *Migrations\<marca_de_tiempo>_InitialCreate.cs*. El método `Up` de la clase `InitialCreate` crea las tablas de base de datos que corresponden a los conjuntos de entidades del modelo de datos. El método `Down` las elimina, tal como se muestra en el ejemplo siguiente:

[!code-csharp[](intro/samples/cu21/Migrations/20180626224812_InitialCreate.cs?range=7-24,77-88)]

Las migraciones llaman al método `Up` para implementar los cambios del modelo de datos para una migración. Cuando se escribe un comando para revertir la actualización, las migraciones llaman al método `Down`.

El código anterior es para la migración inicial. Ese código se creó cuando se ejecutó el comando `migrations add InitialCreate`. El parámetro de nombre de la migración ("InitialCreate" en el ejemplo) se usa para el nombre de archivo. El nombre de la migración puede ser cualquier nombre de archivo válido. Es más recomendable elegir una palabra o frase que resuma lo que se hace en la migración. Por ejemplo, una migración que ha agregado una tabla de departamento podría denominarse "AddDepartmentTable".

Si la migración inicial está creada y la base de datos existe:

* Se genera el código de creación de la base de datos.
* El código de creación de la base de datos no tiene que ejecutarse porque la base de datos ya coincide con el modelo de datos. Si el código de creación de la base de datos se está ejecutando, no hace ningún cambio porque la base de datos ya coincide con el modelo de datos.

Cuando la aplicación se implementa en un entorno nuevo, se debe ejecutar el código de creación de la base de datos para crear la base de datos.

Anteriormente, la base de datos se eliminó, de modo que ya no existe y ahora se crea mediante las migraciones.

### <a name="the-data-model-snapshot"></a>La instantánea del modelo de datos

Las migraciones crean una *instantánea* del esquema de la base de datos actual en *Migrations/SchoolContextModelSnapshot.cs*. Cuando se agrega una migración, EF determina qué ha cambiado mediante la comparación del modelo de datos con el archivo de instantánea.

Para eliminar una migración, use el comando siguiente:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Remove-Migration

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

```console
dotnet ef migrations remove
```

Para obtener más información, vea [dotnet ef migrations remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).

---

El comando remove migrations elimina la migración y garantiza que la instantánea se restablece correctamente.

### <a name="remove-ensurecreated-and-test-the-app"></a>Eliminación de EnsureCreated y prueba de la aplicación

Para el desarrollo inicial se ha utilizado `EnsureCreated`. En este tutorial, se usan las migraciones. `EnsureCreated` tiene las siguientes limitaciones:

* Omite las migraciones y crea la base de datos y el esquema.
* No crea una tabla de migraciones.
* *No* puede usarse con las migraciones.
* Está diseñado para crear prototipos rápidos o de prueba donde se quita y vuelve a crear la base de datos con frecuencia.

Quite `EnsureCreated`:

```csharp
context.Database.EnsureCreated();
```

Ejecute la aplicación y compruebe que la base de datos se haya inicializado.

### <a name="inspect-the-database"></a>Inspección de la base de datos

Use el **Explorador de objetos de SQL Server** para inspeccionar la base de datos. Observe la adición de una tabla `__EFMigrationsHistory`. La tabla `__EFMigrationsHistory` realiza un seguimiento de las migraciones que se han aplicado a la base de datos. Examine los datos de la tabla `__EFMigrationsHistory`, muestra una fila para la primera migración. En el último registro del ejemplo de salida de la CLI anterior se muestra la instrucción INSERT que crea esta fila.

Ejecute la aplicación y compruebe que todo funciona correctamente.

## <a name="applying-migrations-in-production"></a>Aplicar las migraciones en producción

Se recomienda que las aplicaciones de producción **no** llamen a [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) al iniciar la aplicación. No debe llamarse a `Migrate` desde una aplicación en la granja de servidores. Por ejemplo, si la aplicación se ha implementado en la nube con escalado horizontal (se ejecutan varias instancias de la aplicación).

La migración de bases de datos debe realizarse como parte de la implementación y de un modo controlado. Entre los métodos de migración de base de datos de producción se incluyen:

* Uso de las migraciones para crear scripts SQL y uso de scripts SQL en la implementación.
* Ejecución de `dotnet ef database update` desde un entorno controlado.

EF Core usa la tabla `__MigrationsHistory` para ver si es necesario ejecutar las migraciones. Si la base de datos está actualizada, no se ejecuta ninguna migración.

## <a name="troubleshooting"></a>Solución de problemas

Descargue la [aplicación completada](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

La aplicación genera la siguiente excepción:

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

Solución: Ejecute `dotnet ef database update`.

### <a name="additional-resources"></a>Recursos adicionales

* [Versión en YouTube de este tutorial](https://www.youtube.com/watch?v=OWSUuMLKTJo)
* [CLI de .NET Core](/ef/core/miscellaneous/cli/dotnet).
* [Consola del administrador de paquetes (Visual Studio)](/ef/core/miscellaneous/cli/powershell)



> [!div class="step-by-step"]
> [Anterior](xref:data/ef-rp/sort-filter-page)
> [Siguiente](xref:data/ef-rp/complex-data-model)
