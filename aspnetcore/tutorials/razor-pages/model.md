---
title: "Adición de un modelo a una aplicación de páginas de Razor en ASP.NET Core"
author: rick-anderson
description: "Adición de un modelo a una aplicación de páginas de Razor en ASP.NET Core"
keywords: "ASP.NET Core, páginas de Razor, Razor, MVC"
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/model
ms.openlocfilehash: 195128f670223f15e1679c51fb6354c1aa527c01
ms.sourcegitcommit: 3ba32b2b6425ed94604cb0f681db0d5bb5f8ad58
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2017
---
# <a name="adding-a-model-to-a-razor-pages-app"></a>Adición de un modelo a una aplicación de páginas de Razor

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Agregar un modelo de datos

En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **RazorPagesMovie** > **Agregar** > **Nueva carpeta**. Asigne a la carpeta el nombre *Models*.

Haga clic con el botón derecho en la carpeta *Models*. Seleccione **Agregar** > **Clase**. Asigne a la clase el nombre **Movie** y agregue las siguientes propiedades:

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>Agregar una cadena de conexión de base de datos

Agregue una cadena de conexión al archivo *appsettings.json*.

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>Registrar el contexto de base de datos

Registre el contexto de base de datos con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en el archivo *Startup.cs*.

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-6)]

Compile el proyecto para comprobar que no contiene errores.

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>Agregar herramientas de scaffolding y realizar la migración inicial

En esta sección, usará la Consola del Administrador de paquetes (PMC) para:

* Agregar el paquete de generación de código de Visual Studio web. Este paquete es necesario para ejecutar el motor de scaffolding.
* Agregar una migración inicial.
* Actualizar la base de datos con la migración inicial.

En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.

  ![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

En PCM, escriba los siguientes comandos:

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.0
Add-Migration Initial
Update-Database
```

El comando `Install-Package` instala las herramientas necesarias para ejecutar el motor de scaffolding.

El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial. El esquema se basa en el modelo especificado en `DbContext` (en el archivo *Models/MovieContext.cs*). El argumento `Initial` se usa para asignar un nombre a las migraciones. Puede usar cualquier nombre, pero se suele elegir uno que describa la migración. Vea [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) (Introducción a las migraciones) para obtener más información.

El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/\<time-stamp>_InitialCreate.cs*, que crea la base de datos.

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

En el tutorial siguiente se explican los archivos creados mediante scaffolding.

>[!div class="step-by-step"]
[Anterior: Introducción](xref:tutorials/razor-pages/razor-pages-start)
[Siguiente: Scaffolded Razor Pages (Páginas de Razor creadas mediante scaffolding)](xref:tutorials/razor-pages/page)    
