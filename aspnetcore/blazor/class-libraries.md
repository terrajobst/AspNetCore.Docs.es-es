---
title: Bibliotecas de clases de componentes de ASP.NET Core Razor
author: guardrex
description: Descubra cómo se pueden incluir los componentes en aplicaciones increíbles desde una biblioteca de componentes externos.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/class-libraries
ms.openlocfilehash: 402b7b072554f63f85e7cf5e55336104d235a071
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68948445"
---
# <a name="aspnet-core-razor-components-class-libraries"></a>Bibliotecas de clases de componentes de ASP.NET Core Razor

Por [Simon Timms](https://github.com/stimms)

Los componentes se pueden compartir en una [biblioteca de clases de Razor (RCL)](xref:razor-pages/ui-class) en todos los proyectos. Una *biblioteca de clases de componentes de Razor* se puede incluir desde:

* Otro proyecto de la solución.
* Un paquete de NuGet.
* Biblioteca de .NET A la que se hace referencia.

Del mismo modo que los componentes son tipos regulares de .NET, los componentes proporcionados por un RCL son ensamblados .NET normales.

## <a name="create-an-rcl"></a>Creación de un RCL

Siga las instrucciones <xref:blazor/get-started> del artículo para configurar el entorno para el increíble.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Cree un nuevo proyecto.
1. Seleccione **Aplicación web de ASP.NET Core**. Seleccione **Next** (Siguiente).
1. Proporcione un nombre para el proyecto en el campo **Nombre del proyecto** o acepte el predeterminado. En los ejemplos de este tema se usa el `MyComponentLib1`nombre del proyecto. Seleccione **Crear**.
1. En el cuadro de diálogo **Crear una aplicación web ASP.NET Core**, confirme que las opciones **.NET Core** y **ASP.NET Core 3.0** estén seleccionadas.
1. Seleccione la plantilla **biblioteca de clases de Razor** . Seleccione **Crear**.
1. Agregue RCL a una solución:
   1. Haga clic con el botón secundario en la solución. Seleccione **Agregar** > **proyecto existente**.
   1. Navegue hasta el archivo de proyecto de RCL.
   1. Seleccione el archivo de proyecto de RCL ( *. csproj*).
1. Agregue una referencia a RCL desde la aplicación:
   1. Haga clic con el botón derecho en el proyecto de la aplicación. Seleccione **Agregar** > **referencia**.
   1. Seleccione el proyecto RCL. Seleccione **Aceptar**.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

1. Use la plantilla de biblioteca de clases`razorclasslib`de **Razor** () con el comando [dotnet New](/dotnet/core/tools/dotnet-new) en un shell de comandos. En el ejemplo siguiente, se crea un RCL denominado `MyComponentLib1`. La carpeta que contiene `MyComponentLib1` se crea automáticamente cuando se ejecuta el comando:

   ```console
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. Para agregar la biblioteca a un proyecto existente, use el comando [dotnet Add Reference](/dotnet/core/tools/dotnet-add-reference) en un shell de comandos. En el ejemplo siguiente, el RCL se agrega a la aplicación. Ejecute el siguiente comando desde la carpeta de proyecto de la aplicación con la ruta de acceso a la biblioteca:

   ```console
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="rcls-not-supported-for-client-side-apps"></a>RCLs no se admite para las aplicaciones del lado cliente

En el ASP.NET Core actual de la versión preliminar de 3,0, las bibliotecas de clases de Razor no son compatibles con las aplicaciones de cliente más increíbles. En el caso de las aplicaciones del lado cliente, use una biblioteca de componentes extraordinaria creada `blazorlib` por la plantilla en un shell de comandos:

```console
dotnet new blazorlib -o MyComponentLib1
```

Las bibliotecas de componentes `blazorlib` que usan la plantilla pueden incluir archivos estáticos, como imágenes, JavaScript y hojas de estilos. En tiempo de compilación, los archivos estáticos se incrustan en el archivo de ensamblado compilado ( *. dll*), lo que permite el consumo de los componentes sin tener que preocuparse de cómo incluir sus recursos. Los archivos incluidos en el `content` directorio se marcan como recursos incrustados.

## <a name="consume-a-library-component"></a>Consumir un componente de biblioteca

Para consumir los componentes definidos en una biblioteca de otro proyecto, use cualquiera de los métodos siguientes:

* Utilice el nombre de tipo completo con el espacio de nombres.
* Use la directiva [ \@Using](xref:mvc/views/razor#using) de Razor. Los componentes individuales se pueden agregar por nombre.

En los ejemplos siguientes, `MyComponentLib1` es una biblioteca de componentes que `SalesReport` contiene un componente.

Se `SalesReport` puede hacer referencia al componente usando su nombre de tipo completo con el espacio de nombres:

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

También se puede hacer referencia al componente si la biblioteca se incluye en el ámbito con `@using` una directiva:

```cshtml
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

Incluya la `@using MyComponentLib1` Directiva en el archivo *_Import. Razor* de nivel superior para que los componentes de la biblioteca estén disponibles para un proyecto completo. Agregue la Directiva a un archivo *_Import. Razor* en cualquier nivel para aplicar el espacio de nombres a una sola página o a un conjunto de páginas dentro de una carpeta.

## <a name="build-pack-and-ship-to-nuget"></a>Compilar, empaquetar y enviar a NuGet

Dado que las bibliotecas de componentes son bibliotecas estándar de .NET, empaquetarlas y enviarlas a NuGet no es diferente de empaquetar y enviar cualquier biblioteca a NuGet. El empaquetado se realiza mediante el comando [dotnet Pack](/dotnet/core/tools/dotnet-pack) en un shell de comandos:

```console
dotnet pack
```

Cargue el paquete en NuGet con el comando [dotnet NuGet Publish](/dotnet/core/tools/dotnet-nuget-push) en un shell de comandos:

```console
dotnet nuget publish
```

Al usar la `blazorlib` plantilla, los recursos estáticos se incluyen en el paquete NuGet. Los consumidores de la biblioteca reciben automáticamente scripts y hojas de estilos, por lo que no es necesario que los consumidores instalen manualmente los recursos.

## <a name="create-a-razor-components-class-library-with-static-assets"></a>Crear una biblioteca de clases de componentes de Razor con recursos estáticos

Un RCL puede incluir recursos estáticos. Los recursos estáticos están disponibles para cualquier aplicación que utilice la biblioteca. Para obtener más información, consulta <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:razor-pages/ui-class>
