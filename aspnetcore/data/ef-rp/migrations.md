---
title: "Páginas de Razor con EF Core - Migrations - 4 de 8"
author: rick-anderson
description: "En este tutorial, debe comenzar con la característica de migraciones de EF principal para administrar los cambios de modelo de datos en una aplicación de MVC de ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/migrations
ms.openlocfilehash: 7b0a3f73efd1d30b903b3258bea2082792eb6e8c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="migrations---ef-core-with-razor-pages-tutorial-4-of-8"></a>Migraciones - Core EF con el tutorial de las páginas de Razor (4 de 8)

Por [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), y [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

En este tutorial, se usa la característica de migraciones de EF principal para administrar los cambios del modelo de datos.

Si experimenta problemas no puede resolver, descargue el [aplicación final de esta fase](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

Cuando se desarrolla una aplicación nueva, el modelo de datos cambie con frecuencia. Cada vez que cambia el modelo, el modelo obtiene sincronizado con la base de datos. Este tutorial iniciado mediante la configuración de Entity Framework para crear la base de datos si no existe. Cada vez que los datos de cambios en el modelo:

* Se quita la base de datos.
* EF crea uno nuevo que coincide con el modelo.
* La aplicación propaga la base de datos con datos de prueba.

Este enfoque para mantener la base de datos sincronizada con el modelo de datos funciona bien hasta que implemente la aplicación para producción. Cuando se ejecuta la aplicación en producción, normalmente está almacenando datos que hay que mantenerlo. No se puede iniciar la aplicación con una prueba de base de datos cada vez que se realiza un cambio (por ejemplo, agregar una nueva columna). La característica de EF Core migraciones soluciona este problema habilitando el núcleo de EF para actualizar el esquema de base de datos en lugar de crear una nueva base de datos.

En lugar de quitar y volver a crear la base de datos cuando los datos de cambios en el modelo, las migraciones actualiza el esquema y conserva los datos existentes.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>Paquetes de Entity Framework Core NuGet para migraciones

Para trabajar con las migraciones, utilice la **Package Manager Console** (PMC) o la interfaz de línea de comandos (CLI). Estos tutoriales muestra cómo utilizar los comandos CLI. Obtener información sobre el PMC está en [al final de este tutorial](#pmc).

Las herramientas principales de EF de la interfaz de línea de comandos (CLI) se proporcionan en [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Para instalar este paquete, agréguelo a la `DotNetCliToolReference` colección en la *.csproj* de archivos, como se muestra. **Nota:** este paquete debe instalarse mediante la edición de la *.csproj* archivo. El`install-package` comando o la interfaz gráfica de usuario del Administrador de paquetes no se puede usar para instalar este paquete. Editar la *.csproj* archivo haciendo clic en el nombre del proyecto en **el Explorador de soluciones** y seleccionando **ContosoUniversity.csproj editar**.

El marcado siguiente muestra la actualización *.csproj* archivo con las herramientas de EF Core CLI resaltado:

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
Los números de versión en el ejemplo anterior eran actuales cuando se escribió el tutorial. Utilice la misma versión para las herramientas de EF CLI de núcleo disponible en los otros paquetes.

## <a name="change-the-connection-string"></a>Cambiar la cadena de conexión

En el *appSettings.JSON que se* de archivo, cambie el nombre de la base de datos en la cadena de conexión a ContosoUniversity2.

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

Cambiar el nombre de la base de datos en la cadena de conexión hace que la primera migración crear una nueva base de datos. Se crea una nueva base de datos porque no existe uno con ese nombre. Cambiar la cadena de conexión no es necesario para comenzar a usar migraciones.

Una alternativa a cambiar el nombre de la base de datos está eliminando la base de datos. Use **Explorador de objetos de SQL Server** (SSOX) o `database drop` comando de CLI:

 ```console
 dotnet ef database drop
 ```

En la siguiente sección se explica cómo ejecutar comandos de CLI.

## <a name="create-an-initial-migration"></a>Crear una migración inicial

Compile el proyecto.

Abra una ventana de comandos y navegue hasta la carpeta del proyecto. La carpeta del proyecto contiene el *Startup.cs* archivo.

En la ventana de comandos, escriba lo siguiente:

```console
dotnet ef migrations add InitialCreate
```

La ventana de comandos muestra información similar al siguiente:

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

Si se produce un error en la migración con el mensaje "*no se puede obtener acceso al archivo... ContosoUniversity.dll porque está siendo utilizado por otro proceso.* " se muestra:

* Detener IIS Express.

   * Salga y reinicie Visual Studio, o
   * El icono de IIS Express se encuentra en la bandeja del sistema de Windows.
   * Haga clic en el icono de IIS Express y, a continuación, haga clic en **ContosoUniversity > Detener sitio**.

Si el mensaje de error "" Error al crear. se muestra, vuelva a ejecutar el comando. Si obtiene este error, deje una nota en la parte inferior de este tutorial.

### <a name="examine-the-up-and-down-methods"></a>Examine el arriba y abajo métodos

El comando EF Core `migrations add` genera código para crear la base de datos de. Este código de migraciones se encuentra en la *migraciones\<timestamp > _InitialCreate.cs* archivo. El `Up` método de la `InitialCreate` clase crea las tablas de base de datos que corresponden a los conjuntos de entidades del modelo de datos. El `Down` método elimina, tal como se muestra en el ejemplo siguiente:

[!code-csharp[Main](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

Llamadas de migraciones el `Up` método para implementar los cambios de modelo de datos para una migración. Cuando se especifica un comando para revertir la actualización, llamadas de migraciones el `Down` método.

El código anterior es para la migración inicial. Ese código se creó cuando el `migrations add InitialCreate` se ejecutó el comando. El parámetro de nombre de migración ("InitialCreate" en el ejemplo) se utiliza para el nombre de archivo. El nombre de la migración puede ser cualquier nombre de archivo válido. Es mejor elegir una palabra o frase que resume lo que se hace en la migración. Por ejemplo, una migración que se ha agregado una tabla de departamento podría denominarse "AddDepartmentTable."

Si la migración inicial se crea y se cierra la base de datos:

* Se genera el código de creación de la base de datos.
* El código de creación de la base de datos no tiene que ejecutar porque la base de datos ya coincide con el modelo de datos. Si se ejecuta el código de creación de la base de datos, no realiza ningún cambio porque la base de datos ya coincide con el modelo de datos.

Cuando la aplicación se implementa en un nuevo entorno, se debe ejecutar el código de creación de la base de datos para crear la base de datos.

Anteriormente, la cadena de conexión se cambió para usar un nombre nuevo para la base de datos. La base de datos especificado no existe, por lo que las migraciones crea la base de datos.

### <a name="examine-the-data-model-snapshot"></a>Examine la instantánea del modelo de datos

Migraciones crea un *instantánea* del esquema de base de datos actual en *Migrations/SchoolContextModelSnapshot.cs*:

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

Como el esquema de base de datos actual se representa en el código, Core EF no tiene que interactuar con la base de datos para crear las migraciones. Cuando se agrega una migración, Core EF determina qué cambiado al comparar los modelos de datos para el archivo de instantánea. Núcleo EF interactúa con la base de datos sólo cuando tiene que actualizar la base de datos.

El archivo de instantánea debe estar sincronizado con las migraciones que lo creó. No se puede quitar una migración eliminando el archivo denominado  *\<timestamp > _\<migrationname > .cs*. Si se elimina dicho archivo, las restantes migraciones están sincronizadas con el archivo de instantánea de base de datos. Para eliminar la última migración agregada, use la [migraciones de ef dotnet quitar](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) comando.

## <a name="remove-ensurecreated"></a>Quitar EnsureCreated

Para el desarrollo inicial, el `EnsureCreated` se utilizó el comando. En este tutorial, se usa las migraciones. `EnsureCreated`tiene las siguientes limitaciones:

* Omite las migraciones y crea la base de datos y el esquema.
* No cree una tabla de las migraciones.
* Puede *no* utilizarse con las migraciones.
* Está diseñado para crear prototipos de prueba o rápido en la base de datos se quita y vuelve a crear con frecuencia.

Quite la siguiente línea de `DbInitializer`:

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a>La migración se aplican a la base de datos en desarrollo

En la ventana de comandos, escriba lo siguiente para crear la base de datos y tablas.

```console
dotnet ef database update
```

Nota: Si el `update` comando devuelve el error "Error al generar.":

* Vuelva a ejecutar el comando.
* Si se produce un error nuevo, salga de Visual Studio y, a continuación, ejecute el `update` comando.
* Dejar un mensaje en la parte inferior de la página.

El resultado del comando es similar a la `migrations add` salida de comandos. En el comando anterior, se muestran los registros para los comandos SQL que configurar la base de datos. La mayoría de los registros se omite en la siguiente salida de ejemplo:

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

Para reducir el nivel de detalle en los mensajes de registro, puede cambiar los niveles de registro en el *appsettings. Development.JSON* archivo. Para obtener más información, consulte [Introducción al registro](xref:fundamentals/logging/index).

Use **Explorador de objetos de SQL Server** para inspeccionar la base de datos. Tenga en cuenta la adición de un `__EFMigrationsHistory` tabla. El `__EFMigrationsHistory` tabla realiza un seguimiento de las migraciones se han aplicado a la base de datos. Ver los datos en el `__EFMigrationsHistory` tabla, muestra una fila para la primera migración. El último registro en el ejemplo de salida CLI anterior muestra la instrucción INSERT que crea esta fila.

Ejecute la aplicación y compruebe que todo funciona.

## <a name="appling-migrations-in-production"></a>Migraciones de aplicar en producción

Se recomienda que las aplicaciones de producción deberían **no** llamar a [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) al iniciar la aplicación. `Migrate`no debe llamarse desde una aplicación en la granja de servidores. Por ejemplo, si la aplicación se ha implementado con escalado horizontal (se ejecutan varias instancias de la aplicación) en la nube.

Migración de base de datos debe realizarse como parte de la implementación y de un modo controlado. Entre los métodos de migración de base de datos de producción se incluyen:

* Mediante las migraciones para crear secuencias de comandos SQL y las secuencias de comandos SQL en la implementación.
* Ejecutando `dotnet ef database update` desde un entorno controlado.

EF Core utiliza el `__MigrationsHistory` tabla para ver si necesitan ejecutar las migraciones. Si la base de datos está actualizado, no se ejecuta migración.

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>Diferencias entre la interfaz de línea de comandos (CLI) y Consola de administrador de paquetes (PMC)

El núcleo de EF conjunto de herramientas para administrar las migraciones están disponible desde:

* Comandos de CLI de .NET core.
* Los cmdlets de PowerShell en Visual Studio **Package Manager Console** ventana (PMC).

Este tutorial muestra cómo utilizar la CLI, algunos desarrolladores prefieren usar la PMC.

Los comandos de EF principales para el PMC están en el [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) paquete. Este paquete se incluye en el [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, por lo que no tiene que instalarlo.

**Importante:** no es el mismo paquete que la instalación de la CLI mediante la edición de la *.csproj* archivo. El nombre de éste termina en `Tools`, a diferencia del nombre de paquete CLI que finaliza en `Tools.DotNet`.

Para obtener más información acerca de los comandos CLI, consulte [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).

Para obtener más información acerca de los comandos PMC, consulte [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="troubleshooting"></a>Solución de problemas

Descargue el [aplicación final de esta fase](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

La aplicación genera la siguiente excepción:

```text
`SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

Solución: ejecute`dotnet ef database update`

Si el `update` comando devuelve el error "Error al generar.":

* Vuelva a ejecutar el comando.
* Dejar un mensaje en la parte inferior de la página.

>[!div class="step-by-step"]
[Anterior](xref:data/ef-rp/sort-filter-page)
[Siguiente](xref:data/ef-rp/complex-data-model)
