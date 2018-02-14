---
title: "Páginas de Razor con EF Core: Migraciones (4 de 8)"
author: rick-anderson
description: "En este tutorial, empezará a usar la característica de EF Core de migraciones para administrar los cambios de modelos de datos en una aplicación de ASP.NET Core MVC."
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/migrations
ms.openlocfilehash: e89d95702cb94556bc6e5dc73253c51acaa11578
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2018
---
# <a name="migrations---ef-core-with-razor-pages-tutorial-4-of-8"></a>Migraciones: tutorial de EF Core con páginas de Razor (4 de 8)

Por [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) y [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

En este tutorial, se usa la característica de migraciones de EF Core para administrar cambios en el modelo de datos.

Si experimenta problemas que no puede resolver, descargue la [aplicación completada para esta fase](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

Cuando se desarrolla una aplicación nueva, el modelo de datos cambia con frecuencia. Cada vez que el modelo cambia, este deja de estar sincronizado con la base de datos. Este tutorial se inició con la configuración de Entity Framework para crear la base de datos si no existía. Cada vez que los datos del modelo cambian:

* Se quita la base de datos.
* EF crea una que coincide con el modelo.
* La aplicación inicializa la base de datos con datos de prueba.

Este enfoque para mantener la base de datos sincronizada con el modelo de datos funciona bien hasta que la aplicación se implemente en producción. Cuando se ejecuta la aplicación en producción, normalmente está almacenando datos que hay que mantener. No se puede iniciar la aplicación con una prueba de base de datos cada vez que se hace un cambio (por ejemplo, agregar una nueva columna). La característica Migraciones de EF Core soluciona este problema habilitando EF Core para actualizar el esquema de la base de datos en lugar de crear una.

En lugar de quitar y volver a crear la base de datos cuando los datos del modelo cambian, las migraciones actualizan el esquema y conservan los datos existentes.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>Paquetes de Entity Framework Core NuGet para migraciones

Para trabajar con las migraciones, use la **Consola del Administrador de paquetes** (PMC) o la interfaz de la línea de comandos (CLI). En estos tutoriales se muestra cómo usar los comandos de la CLI. [Al final de este tutorial](#pmc) encontrará información sobre la PMC.

Las herramientas de EF Core para la interfaz de la línea de comandos (CLI) se proporcionan en [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Para instalar este paquete, agréguelo a la colección `DotNetCliToolReference` del archivo *.csproj*, como se muestra a continuación. **Nota:** Debe instalarse este paquete mediante la edición del archivo *.csproj*. No se puede usar el comando `install-package` o la interfaz gráfica de usuario del administrador de paquetes para instalar este paquete. Edite el archivo *.csproj*; para ello, haga clic con el botón derecho en el nombre del proyecto en el **Explorador de soluciones** y seleccione **Editar ContosoUniversity.csproj**.

El siguiente marcado muestra el archivo *.csproj* actualizado con las herramientas de la CLI de EF Core resaltadas:

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
Los números de versión en el ejemplo anterior eran los actuales cuando se escribió el tutorial. Use la misma versión para las herramientas de la CLI de EF Core disponibles en los otros paquetes.

## <a name="change-the-connection-string"></a>Cambiar la cadena de conexión

En el archivo *appsettings.json*, cambie el nombre de la base de datos en la cadena de conexión a ContosoUniversity2.

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

Cambiar el nombre de la base de datos en la cadena de conexión hace que la primera migración cree una base de datos. Se crea una base de datos porque no existe ninguna con ese nombre. No es necesario cambiar la cadena de conexión para comenzar a usar las migraciones.

Una alternativa a cambiar el nombre de la base de datos es eliminarla. Use el **Explorador de objetos de SQL Server** (SSOX) o el comando de la CLI `database drop`:

 ```console
 dotnet ef database drop
 ```

En la siguiente sección se explica cómo ejecutar comandos de la CLI.

## <a name="create-an-initial-migration"></a>Crear una migración inicial

Compile el proyecto.

Abra una ventana de comandos y desplácese hasta la carpeta del proyecto. La carpeta del proyecto contiene el archivo *Startup.cs*.

Escriba lo siguiente en la ventana de comandos:

```console
dotnet ef migrations add InitialCreate
```

La ventana de comandos muestra información similar a la siguiente:

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

Si se produce un error en la migración y se muestra el mensaje "*No se puede tener acceso al archivo... ContosoUniversity.dll ya que otro proceso lo está usando.*" :

* Detenga IIS Express.

   * Salga y reinicie Visual Studio o
   * Busque el icono de IIS Express en la bandeja del sistema de Windows.
   * Haga clic con el botón derecho en el icono de IIS Express y, después, haga clic en **ContosoUniversity > Detener sitio**.

Si se muestra el mensaje de error "Error de complicación.", vuelva a ejecutar el comando. Si obtiene este error, deje una nota en la parte inferior de este tutorial.

### <a name="examine-the-up-and-down-methods"></a>Examinar los métodos Up y Down

El comando de EF Core `migrations add` generó código desde donde crear la base de datos. Este código de migraciones se encuentra en el archivo *Migrations\<marca_de_tiempo>_InitialCreate.cs*. El método `Up` de la clase `InitialCreate` crea las tablas de base de datos que corresponden a los conjuntos de entidades del modelo de datos. El método `Down` las elimina, tal como se muestra en el ejemplo siguiente:

[!code-csharp[Main](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

Las migraciones llaman al método `Up` para implementar los cambios del modelo de datos para una migración. Cuando se escribe un comando para revertir la actualización, las migraciones llaman al método `Down`.

El código anterior es para la migración inicial. Ese código se creó cuando se ejecutó el comando `migrations add InitialCreate`. El parámetro de nombre de la migración ("InitialCreate" en el ejemplo) se usa para el nombre de archivo. El nombre de la migración puede ser cualquier nombre de archivo válido. Es más recomendable elegir una palabra o frase que resuma lo que se hace en la migración. Por ejemplo, una migración que ha agregado una tabla de departamento podría denominarse "AddDepartmentTable".

Si se crea la migración inicial y la base de datos existe:

* Se genera el código de creación de la base de datos.
* El código de creación de la base de datos no tiene que ejecutarse porque la base de datos ya coincide con el modelo de datos. Si el código de creación de la base de datos se está ejecutando, no hace ningún cambio porque la base de datos ya coincide con el modelo de datos.

Cuando la aplicación se implementa en un entorno nuevo, se debe ejecutar el código de creación de la base de datos para crear la base de datos.

Anteriormente, la cadena de conexión se cambió para usar un nombre nuevo para la base de datos. La base de datos especificada no existe, por lo que las migraciones crean la base de datos.

### <a name="examine-the-data-model-snapshot"></a>Examinar la instantánea del modelo de datos

Las migraciones crean una *instantánea* del esquema de la base de datos actual en *Migrations/SchoolContextModelSnapshot.cs*:

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

Como el esquema de la base de datos actual se representa en el código, EF Core no tiene que interactuar con la base de datos para crear las migraciones. Cuando se agrega una migración, EF Core determina qué ha cambiado mediante la comparación del modelo de datos con el archivo de instantánea. EF Core interactúa con la base de datos solo cuando tiene que actualizarla.

El archivo de instantánea debe estar sincronizado con las migraciones que lo crearon. No se puede quitar una migración eliminando el archivo denominado *\<marca_de_tiempo>_\<nombre_migración>.cs*. Si se elimina dicho archivo, las restantes migraciones no estarán sincronizadas con el archivo de instantánea de base de datos. Para eliminar la última migración que agregó, use el comando [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).

## <a name="remove-ensurecreated"></a>Quitar EnsureCreated

Para el desarrollo inicial se usó el comando `EnsureCreated`. En este tutorial, se usan las migraciones. `EnsureCreated` tiene las siguientes limitaciones:

* Omite las migraciones y crea la base de datos y el esquema.
* No crea una tabla de migraciones.
* *No* puede usarse con las migraciones.
* Está diseñado para crear prototipos rápidos o de prueba donde se quita y vuelve a crear la base de datos con frecuencia.

Quite las siguientes líneas de `DbInitializer`:

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a>Aplicar la migración a la base de datos en desarrollo

En la ventana de comandos, escriba lo siguiente para crear la base de datos y las tablas.

```console
dotnet ef database update
```

Nota: Si el comando `update` devuelve el error "Error de compilación.":

* Vuelva a ejecutar el comando.
* Si se vuelve a producir un error, salga de Visual Studio y luego ejecute el comando `update`.
* Deje un mensaje en la parte inferior de la página.

El resultado del comando es similar al resultado del comando `migrations add`. En el comando anterior se muestran los registros para los comandos de SQL que configuran la base de datos. La mayoría de los registros se omite en la siguiente salida de ejemplo:

```text
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (467ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20170816151242_InitialCreate', N'2.0.0-rtm-26452');
Done.
```

Para reducir el nivel de detalle en los mensajes de registro, puede cambiarlo en el archivo *appsettings.Development.json*. Para obtener más información, vea [Introducción al registro](xref:fundamentals/logging/index).

Use el **Explorador de objetos de SQL Server** para inspeccionar la base de datos. Observe la adición de una tabla `__EFMigrationsHistory`. La tabla `__EFMigrationsHistory` realiza un seguimiento de las migraciones que se han aplicado a la base de datos. Examine los datos de la tabla `__EFMigrationsHistory`, muestra una fila para la primera migración. En el último registro del ejemplo de salida de la CLI anterior se muestra la instrucción INSERT que crea esta fila.

Ejecute la aplicación y compruebe que todo funciona correctamente.

## <a name="appling-migrations-in-production"></a>Aplicar las migraciones en producción

Se recomienda que las aplicaciones de producción **no** llamen a [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) al iniciar la aplicación. No debe llamarse a `Migrate` desde una aplicación en la granja de servidores. Por ejemplo, si la aplicación se ha implementado en la nube con escalado horizontal (se ejecutan varias instancias de la aplicación).

La migración de bases de datos debe realizarse como parte de la implementación y de un modo controlado. Entre los métodos de migración de base de datos de producción se incluyen:

* Uso de las migraciones para crear scripts SQL y uso de scripts SQL en la implementación.
* Ejecución de `dotnet ef database update` desde un entorno controlado.

EF Core usa la tabla `__MigrationsHistory` para ver si es necesario ejecutar las migraciones. Si la base de datos está actualizada, no se ejecuta la migración.

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>Diferencias entre la interfaz de la línea de comandos (CLI) y la Consola del Administrador de paquetes (PMC)

El conjunto de herramientas de EF Core para administrar las migraciones está disponible desde:

* Comandos de la CLI de .NET Core.
* Los cmdlets de PowerShell en la ventana **Consola del Administrador de paquetes** (PMC) de Visual Studio.

Este tutorial muestra cómo usar la CLI, algunos desarrolladores prefieren usar la PMC.

Los comandos de EF Core para la PMC están en el paquete [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools). Este paquete se incluye en el metapaquete [Microsoft.AspNetCore.All](xref:fundamentals/metapackage), por lo que no es necesario instalarlo.

**Importante:** Este no es el mismo paquete que el que se instala para la CLI mediante la edición del archivo *.csproj*. El nombre de este paquete termina en `Tools`, a diferencia del nombre de paquete de la CLI que termina en `Tools.DotNet`.

Para obtener más información sobre los comandos de la CLI, vea [CLI de .NET Core](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).

Para obtener más información sobre los comandos de la PMC, vea [Consola del Administrador de paquetes (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="troubleshooting"></a>Solución de problemas

Descargue la [aplicación completa de esta fase](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

La aplicación genera la siguiente excepción:

```text
`SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

Solución: ejecute `dotnet ef database update`

Si el comando `update` devuelve el error "Error de compilación.":

* Vuelva a ejecutar el comando.
* Deje un mensaje en la parte inferior de la página.

>[!div class="step-by-step"]
[Anterior](xref:data/ef-rp/sort-filter-page)
[Siguiente](xref:data/ef-rp/complex-data-model)
