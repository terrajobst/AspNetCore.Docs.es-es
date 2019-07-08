---
title: Comando dotnet aspnet-codegenerator
author: rick-anderson
ms.author: riande
description: El comando dotnet aspnet-codegenerator aplica scaffolding a los proyectos de ASP.NET Core
ms.date: 07/04/2019
uid: fundamentals/tools/dotnet-aspnet-codegenerator
ms.openlocfilehash: 55b592d9d203970777c84438e210519957abb35d
ms.sourcegitcommit: f6e6730872a7d6f039f97d1df762f0d0bd5e34cf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561688"
---
# <a name="dotnet-aspnet-codegenerator"></a>dotnet aspnet-codegenerator

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

`dotnet aspnet-codegenerator`: ejecuta el motor de scaffolding de ASP.NET Core. `dotnet aspnet-codegenerator` solo es necesario para aplicar scaffolding desde la línea de comandos, no es necesario para usar la técnica de scaffolding con Visual Studio.

Este artículo se aplica al [SDK de .NET Core 2.2](https://dotnet.microsoft.com/download/dotnet-core/2.2) y versiones posteriores.

## <a name="installing-aspnet-codegenerator"></a>Instalación de aspnet-codegenerator

`aspnet-codegenerator` es una [herramienta global](/dotnet/core/tools/global-tools) que debe estar instalada. El comando siguiente instala la versión estable más reciente de la herramienta `aspnet-codegenerator`:

```console
dotnet tool install -g aspnet-codegenerator
```

El comando siguiente actualiza `aspnet-codegenerator` a la versión estable más reciente disponible desde los SDK instalados de .NET Core:

```console
dotnet tool update -g aspnet-codegenerator
```

## <a name="synopsis"></a>Sinopsis

```
dotnet aspnet-codegenerator [arguments] [-p|--project] [-n|--nuget-package-dir] [-c|--configuration] [-tfm|--target-framework] [-b|--build-base-path] [--no-build] 
dotnet aspnet-codegenerator [-h|--help]
```

## <a name="description"></a>DESCRIPCIÓN

El comando `dotnet aspnet-codegenerator ` global ejecuta el generador de código de ASP.NET Core y el motor de scaffolding.

## <a name="arguments"></a>Argumentos

`generator`

El generador de código que se va a ejecutar. Estos generadores están disponibles:

| Generator | Operación |
| ----------------- | ------------ | 
| área      | [Aplica scaffolding a un área](/aspnet/core/mvc/controllers/areas) |
  controlador| [Aplica scaffolding a un controlador](/aspnet/core/tutorials/first-mvc-app/adding-model) |
  identidad  | [Aplica scaffolding a una identidad](/aspnet/core/security/authentication/scaffold-identity) |
  razorpage | [Aplica scaffolding a Razor Pages](/aspnet/core/tutorials/razor-pages/model) |
  view      | [Aplica scaffolding a una vista](/aspnet/core/mvc/views/overview) |

## <a name="options"></a>Opciones

`-n|--nuget-package-dir`

Especifica el directorio del paquete de NuGet.

`-c|--configuration {Debug|Release}`

Define la configuración de compilación. El valor predeterminado es `Debug`.

`-tfm|--target-framework`

Establece como destino el [marco](/dotnet/standard/frameworks) que se va a usar. Por ejemplo: `net46`.

`-b|--build-base-path`

La ruta de acceso de la base de compilación.

`-h|--help`

Imprime una corta ayuda para el comando.

`--no-build`

No compila el proyecto antes de ejecutarlo. También establece la marca `--no-restore` de forma implícita.

`-p|--project <PATH>`

Especifica la ruta de acceso del archivo del proyecto que se va a ejecutar (nombre de la carpeta o ruta de acceso completa). Si no se especifica, se toma como predeterminado el directorio actual.

## <a name="generator-options"></a>Opciones del generador

En las secciones siguientes se detallan las opciones disponibles para los generadores compatibles:

* Área
* Controlador
* identidad  
* Razorpage
* Ver

<a name="area"></a>

### <a name="area-options"></a>Opciones del área

Esta herramienta está diseñada para los proyectos web de ASP.NET Core con controladores y vistas. No está pensada para las aplicaciones de Razor Pages.

Uso: `dotnet aspnet-codegenerator area AreaNameToGenerate`

El comando anterior genera estas carpetas:

* *Áreas*
  * *AreaNameToGenerate*
    * *Controladores*
    * *Data*
    * *Models*
    * *Vistas*

<a name="ctl"></a>

### <a name="controller-options"></a>Opciones del controlador

En la tabla siguiente se muestran las opciones para `controller` y `razorpage` de `aspnet-codegenerator`:

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

En la tabla siguiente se muestran las opciones únicas para `aspnet-codegenerator controller`:

| Opción               | DESCRIPCIÓN|
| ----------------- | ------------ |
| --controllerName o -name | El nombre del controlador. |
| --useAsyncActions o -async | Genera acciones asincrónicas del controlador. |
| --noViews o -nv | **No** genera vistas. |
| --restWithNoViews o -api  | Genera un controlador con una API de estilo REST. Se supone `noViews` y todas las opciones relacionadas con la vista se omiten. |
| --readWriteActions o -actions | Genera un controlador con acciones de lectura/escritura sin un modelo. |

Use el modificador `-h` para obtener ayuda sobre el comando `aspnet-codegenerator controller`:

```console
dotnet aspnet-codegenerator controller -h
```

Consulte [Aplicar scaffolding al modelo de película](/aspnet/core/tutorials/razor-pages/model) para ver un ejemplo de `dotnet aspnet-codegenerator controller`.

### <a name="razorpage"></a>Razorpage

<a name="rp"></a>

Es posible aplicar scaffolding a Razor Pages de manera individual si se especifica el nombre de la página nueva y la plantilla a usar. Las plantillas admitidas son:

* `Empty`
* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

Por ejemplo, el comando siguiente usa la plantilla de edición para generar *MyEdit.cshtml* y *MyEdit.cshtml.cs*:

```console
dotnet aspnet-codegenerator razorpage MyEdit Edit -m Movie -dc RazorPagesMovieContext -outDir Pages/Movies
```

Por lo general, no se especifica la plantilla ni el nombre del archivo generado y se crean estas plantillas:

* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

En la tabla siguiente se muestran las opciones para `razorpage` y `controller` de `aspnet-codegenerator`:

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

En la tabla siguiente se muestran las opciones únicas para `aspnet-codegenerator razorpage`:

| Opción               | DESCRIPCIÓN|
| ----------------- | ------------ |
|   --namespaceName o -namespace | El nombre del espacio de nombres que se usará para la clase PageModel generada. |
| --partialView o -partial | Genera una vista parcial. Si se especifica, las opciones de diseño -l y -udl se ignoran. |
| --noPageModel o -npm | Cambia a no generar una clase PageModel para una plantilla vacía. |

Use el modificador `-h` para obtener ayuda sobre el comando `aspnet-codegenerator razorpage`:

```console
dotnet aspnet-codegenerator razorpage -h
```

Consulte [Aplicar scaffolding al modelo de película](/aspnet/core/tutorials/razor-pages/model) para ver un ejemplo de `dotnet aspnet-codegenerator razorpage`.

### <a name="identity"></a>identidad

Consulte [Identidad de scaffolding](/aspnet/core/security/authentication/scaffold-identity)