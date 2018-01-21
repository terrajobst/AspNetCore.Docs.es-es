---
title: "Núcleo de ASP.NET MVC con EF Core - Migrations - 4 de 10"
author: tdykstra
description: "En este tutorial, debe comenzar con la característica de migraciones de EF principal para administrar los cambios del modelo de datos en una aplicación MVC de ASP.NET Core."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/migrations
ms.openlocfilehash: 9081ddd14e6ed9192c6bd8ce8b265d14dbca7e23
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="migrations---ef-core-with-aspnet-core-mvc-tutorial-4-of-10"></a>Migraciones - Core EF con el tutorial de MVC de ASP.NET Core (4 de 10)

Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)

La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones web de MVC de ASP.NET Core con Entity Framework Core y Visual Studio. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](intro.md).

En este tutorial, debe comenzar con la característica de migraciones de EF principal para administrar los cambios del modelo de datos. En los tutoriales posteriores, agregará más migraciones a medida que cambia el modelo de datos.

## <a name="introduction-to-migrations"></a>Introducción a las migraciones

Al desarrollar una aplicación nueva, el modelo de datos cambia con frecuencia y cada vez que los cambios del modelo, se le asigna sincronizada con la base de datos. Estos tutoriales se inició mediante la configuración de Entity Framework para crear la base de datos si no existe. A continuación, cada vez que cambie el modelo de datos, agregar, quitar, o cambiar las clases de entidad o cambiar la clase DbContext--puede eliminar la base de datos y EF crea uno nuevo que coincide con el modelo y propaga con datos de prueba.

Este método para mantener la base de datos sincronizada con el modelo de datos funciona bien hasta que implemente la aplicación en producción. Cuando se ejecuta la aplicación en producción normalmente almacena los datos que desea mantener, y no desea perder todo lo que cada vez que se realice un cambio como agregar una nueva columna. La característica de EF Core migraciones soluciona este problema habilitando EF actualizar el esquema de base de datos en lugar de crear una nueva base de datos.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>Paquetes de Entity Framework Core NuGet para migraciones

Para trabajar con las migraciones, puede usar el **Package Manager Console** (PMC) o la interfaz de línea de comandos (CLI).  Estos tutoriales muestra cómo utilizar los comandos CLI. Obtener información sobre el PMC está en [al final de este tutorial](#pmc).

Las herramientas de EF para la interfaz de nivel de la línea de comandos se proporcionan en [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Para instalar este paquete, agréguelo a la `DotNetCliToolReference` colección en la *.csproj* de archivos, como se muestra. **Nota**: Tendrá que instalar este paquete mediante la edición del archivo *.csproj*; no se puede usar el comando `install-package` ni la interfaz gráfica de usuario del administrador de paquetes. Puede editar la *.csproj* archivo haciendo clic en el nombre del proyecto en **el Explorador de soluciones** y seleccionando **ContosoUniversity.csproj editar**.

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
(Los números de versión en este ejemplo fueron actuales cuando se escribió el tutorial.) 

## <a name="change-the-connection-string"></a>Cambiar la cadena de conexión

En el *appSettings.JSON que se* de archivo, cambie el nombre de la base de datos en la cadena de conexión para ContosoUniversity2 o algún otro nombre que no has utilizado en el equipo que está utilizando.

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

Este cambio se configura el proyecto para que la primera migración creará una nueva base de datos. Esto no es necesario para comenzar a usar migraciones, pero se verá más adelante por eso es una buena idea.

> [!NOTE]
> Como alternativa a cambiar el nombre de la base de datos, puede eliminar la base de datos. Use **Explorador de objetos de SQL Server** (SSOX) o `database drop` comando de CLI:
> ```console
> dotnet ef database drop
> ```
> En la siguiente sección se explica cómo ejecutar comandos de CLI.

## <a name="create-an-initial-migration"></a>Crear una migración inicial

Guarde los cambios y compile el proyecto. A continuación, abra una ventana de comandos y desplácese hasta la carpeta del proyecto. Aquí es una forma rápida de hacerlo:

* En **el Explorador de soluciones**, haga clic en el proyecto y elija **abierto en el Explorador de archivos** en el menú contextual.

  ![Abrir en el elemento de menú del explorador de archivos](migrations/_static/open-in-file-explorer.png)

* Escriba "cmd" en la barra de direcciones y presione ENTRAR.

  ![Abrir ventana de comandos](migrations/_static/open-command-window.png)

Escriba el siguiente comando en la ventana de comandos:

```console
dotnet ef migrations add InitialCreate
```

Ve un resultado similar al siguiente en la ventana de comandos:

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> Si ve un mensaje de error *ningún archivo ejecutable encuentra coincidencia comando "dotnet-ef"*, consulte [esta entrada de blog](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) para solucionar problemas de la Ayuda.

Si ve un mensaje de error "*no se puede obtener acceso al archivo... ContosoUniversity.dll porque está siendo utilizado por otro proceso.* ", el icono IIS Express se encuentra en la bandeja del sistema Windows y haga clic en él, haga clic en **ContosoUniversity > Detener sitio**.

## <a name="examine-the-up-and-down-methods"></a>Examine el arriba y abajo métodos

Cuando ejecuta el `migrations add` comando EF generó el código que va a crear la base de datos desde el principio. Este código está en el *migraciones* carpeta, en el archivo llamado  *\<timestamp > _InitialCreate.cs*. El `Up` método de la `InitialCreate` clase crea las tablas de base de datos que corresponden a los conjuntos de entidades del modelo de datos, y el `Down` método elimina, tal como se muestra en el ejemplo siguiente.

[!code-csharp[Main](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

Llamadas de migraciones el `Up` método para implementar los cambios de modelo de datos para una migración. Cuando se especifica un comando para revertir la actualización, llamadas de migraciones el `Down` método.

Este código es para la migración inicial que se creó cuando escribe el `migrations add InitialCreate` comando. El parámetro de nombre de la migración ("InitialCreate" en el ejemplo) se utiliza para el nombre de archivo y puede ser todo lo que desees. Es mejor elegir una palabra o frase que resume lo que se hace en la migración. Por ejemplo, podría denominar una migración más adelante "AddDepartmentTable".

Si ha creado la migración inicial cuando la base de datos ya existe, se genera el código de creación de la base de datos pero no es necesario ejecutar porque la base de datos ya coincide con el modelo de datos. Al implementar la aplicación en otro entorno donde la base de datos no existe aún, se ejecutará este código para crear la base de datos, por lo que es una buena idea para probarla en primer lugar. Por eso ha cambiado el nombre de la base de datos en versiones anteriores, la cadena de conexión para que las migraciones pueden crear uno nuevo desde cero.

## <a name="examine-the-data-model-snapshot"></a>Examine la instantánea del modelo de datos

Las migraciones también crea un *instantánea* del esquema de base de datos actual en *Migrations/SchoolContextModelSnapshot.cs*. Este es el aspecto de ese código:

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

Como el esquema de base de datos actual se representa en el código, Core EF no tiene que interactuar con la base de datos para crear las migraciones. Cuando se agrega una migración, EF determina qué cambiado al comparar los modelos de datos para el archivo de instantánea. EF interactúa con la base de datos sólo cuando tiene que actualizar la base de datos. 

El archivo de instantánea tiene que estar sincronizada con las migraciones que crea, por lo que no se puede eliminar una migración eliminando el archivo denominado  *\<timestamp > _\<migrationname > .cs*. Si elimina ese archivo, las restantes migraciones será sincronizadas con el archivo de instantánea de base de datos. Para eliminar la última migración que ha agregado, use la [migraciones de ef dotnet quitar](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) comando.

## <a name="apply-the-migration-to-the-database"></a>La migración se aplican a la base de datos

En la ventana de comandos, escriba el siguiente comando para crear la base de datos y tablas en él.

```console
dotnet ef database update
```

El resultado del comando es similar a la `migrations add` de comandos, salvo que verá registros para los comandos SQL que configura la base de datos. La mayoría de los registros se omite en la siguiente salida de ejemplo. Si no desea ver este nivel de detalle en los mensajes de registro, puede cambiar el nivel de registro en el *appsettings. Development.JSON* archivo. Para obtener más información, consulte [Introducción al registro](xref:fundamentals/logging/index).

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

Use **Explorador de objetos de SQL Server** para inspeccionar la base de datos como lo hizo en el primer tutorial.  Observará que la adición de una tabla de __EFMigrationsHistory que realiza un seguimiento de las migraciones se han aplicado a la base de datos. Ver los datos de esa tabla y verá una fila para la primera migración. (El último registro en el ejemplo de salida CLI anterior muestra la instrucción INSERT que crea esta fila).

Ejecute la aplicación para comprobar que todo funciona sigue el mismo que antes.

![Página de índice de estudiantes](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>Diferencias entre la interfaz de línea de comandos (CLI) y Consola de administrador de paquetes (PMC)

Las herramientas de EF para administrar las migraciones está disponible desde los comandos de CLI de núcleo de .NET o de los cmdlets de PowerShell en Visual Studio **Package Manager Console** ventana (PMC). Este tutorial muestra cómo utilizar la CLI, pero puede usar el PMC si lo prefiere.

Los comandos EF de los comandos PMC están en el [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) paquete. Este paquete ya está incluido en el [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, por lo que no tiene que instalarlo.

**Importante:** no es el mismo paquete que la instalación de la CLI mediante la edición de la *.csproj* archivo. El nombre de éste termina en `Tools`, a diferencia del nombre de paquete CLI que finaliza en `Tools.DotNet`.

Para obtener más información acerca de los comandos CLI, consulte [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet). 

Para obtener más información acerca de los comandos PMC, consulte [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="summary"></a>Resumen

En este tutorial, ha visto cómo crear y aplicar la primera migración. En el siguiente tutorial, comenzará examinando temas más avanzados expandiendo el modelo de datos. A lo largo de la forma podrá crear y aplicar migraciones adicionales.

>[!div class="step-by-step"]
[Anterior](sort-filter-page.md)
[Siguiente](complex-data-model.md)  
