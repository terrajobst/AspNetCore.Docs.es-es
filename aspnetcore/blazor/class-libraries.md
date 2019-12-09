---
title: Bibliotecas de clases de componentes de ASP.NET Core Razor
author: guardrex
description: Descubra cómo se pueden incluir componentes en Blazor aplicaciones desde una biblioteca de componentes externos.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
uid: blazor/class-libraries
ms.openlocfilehash: 6bac007e3e1d046d6b16a3a0be6dc5976b99b766
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2019
ms.locfileid: "74943880"
---
# <a name="aspnet-core-razor-components-class-libraries"></a>Bibliotecas de clases de componentes de ASP.NET Core Razor

Por [Simon Timms](https://github.com/stimms)

Los componentes se pueden compartir en una [biblioteca de clases de Razor (RCL)](xref:razor-pages/ui-class) en todos los proyectos. Una *biblioteca de clases de componentes de Razor* se puede incluir desde:

* Otro proyecto de la solución.
* Un paquete de NuGet.
* Biblioteca de .NET A la que se hace referencia.

Del mismo modo que los componentes son tipos regulares de .NET, los componentes proporcionados por un RCL son ensamblados .NET normales.

## <a name="create-an-rcl"></a>Creación de un RCL

Siga las instrucciones del artículo <xref:blazor/get-started> para configurar el entorno para Blazor.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Cree un nuevo proyecto.
1. Seleccione **biblioteca de clases de Razor**. Seleccione **Siguiente**.
1. En el cuadro de diálogo **crear una nueva biblioteca de clases de Razor** , seleccione **crear**.
1. Proporcione un nombre para el proyecto en el campo **Nombre del proyecto** o acepte el predeterminado. En los ejemplos de este tema se usa el nombre del proyecto `MyComponentLib1`. Seleccione **Crear**.
1. Agregue RCL a una solución:
   1. Haga clic con el botón secundario en la solución. Seleccione **Agregar** > **Proyecto existente**.
   1. Navegue hasta el archivo de proyecto de RCL.
   1. Seleccione el archivo de proyecto de RCL ( *. csproj*).
1. Agregue una referencia a RCL desde la aplicación:
   1. Haga clic con el botón derecho en el proyecto de la aplicación. Seleccione **Agregar** **referencia**de > .
   1. Seleccione el proyecto RCL. Seleccione **Aceptar**.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

1. Use la plantilla de **biblioteca de clases de Razor** (`razorclasslib`) con el comando [dotnet New](/dotnet/core/tools/dotnet-new) en un shell de comandos. En el ejemplo siguiente, se crea un RCL denominado `MyComponentLib1`. La carpeta que contiene `MyComponentLib1` se crea automáticamente cuando se ejecuta el comando:

   ```dotnetcli
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. Para agregar la biblioteca a un proyecto existente, use el comando [dotnet Add Reference](/dotnet/core/tools/dotnet-add-reference) en un shell de comandos. En el ejemplo siguiente, el RCL se agrega a la aplicación. Ejecute el siguiente comando desde la carpeta de proyecto de la aplicación con la ruta de acceso a la biblioteca:

   ```dotnetcli
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="consume-a-library-component"></a>Consumir un componente de biblioteca

Para consumir los componentes definidos en una biblioteca de otro proyecto, use cualquiera de los métodos siguientes:

* Utilice el nombre de tipo completo con el espacio de nombres.
* Use la Directiva de [uso de\@](xref:mvc/views/razor#using) de Razor. Los componentes individuales se pueden agregar por nombre.

En los ejemplos siguientes, `MyComponentLib1` es una biblioteca de componentes que contiene un componente de `SalesReport`.

Se puede hacer referencia al componente de `SalesReport` utilizando su nombre de tipo completo con el espacio de nombres:

```razor
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

También se puede hacer referencia al componente si la biblioteca se incluye en el ámbito con una directiva `@using`:

```razor
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

Incluya la Directiva `@using MyComponentLib1` en el archivo *_Import. Razor* de nivel superior para que los componentes de la biblioteca estén disponibles para un proyecto completo. Agregue la Directiva a un archivo *_Import. Razor* en cualquier nivel para aplicar el espacio de nombres a una sola página o a un conjunto de páginas dentro de una carpeta.

## <a name="build-pack-and-ship-to-nuget"></a>Compilar, empaquetar y enviar a NuGet

Dado que las bibliotecas de componentes son bibliotecas estándar de .NET, empaquetarlas y enviarlas a NuGet no es diferente de empaquetar y enviar cualquier biblioteca a NuGet. El empaquetado se realiza mediante el comando [dotnet Pack](/dotnet/core/tools/dotnet-pack) en un shell de comandos:

```dotnetcli
dotnet pack
```

Cargue el paquete en NuGet con el comando [dotnet NuGet de extracción](/dotnet/core/tools/dotnet-nuget-push) en un shell de comandos.

## <a name="create-a-razor-components-class-library-with-static-assets"></a>Crear una biblioteca de clases de componentes de Razor con recursos estáticos

Un RCL puede incluir recursos estáticos. Los recursos estáticos están disponibles para cualquier aplicación que utilice la biblioteca. Para obtener más información, vea <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:razor-pages/ui-class>
