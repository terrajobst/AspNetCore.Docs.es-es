---
title: Bibliotecas de clases de componentes de Razor
author: guardrex
description: Descubra cómo los componentes se pueden incluir en las aplicaciones de componentes de Razor de una biblioteca de componentes externos.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/14/2019
uid: razor-components/class-libraries
ms.openlocfilehash: 1064ad60d90af15af483ba9bca5ed85fb63c2924
ms.sourcegitcommit: 10e14b85490f064395e9b2f423d21e3c2d39ed8b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "59515591"
---
# <a name="razor-components-class-libraries"></a>Bibliotecas de clases de componentes de Razor

Por [Simon Timms](https://github.com/stimms)

Los componentes se pueden compartir en las bibliotecas de clases de Razor entre proyectos. Los componentes se pueden incluidos desde:

* Otro proyecto en la solución.
* Un paquete de NuGet.
* Biblioteca de .NET que se hace referencia.

Como los componentes son tipos de .NET normales, los componentes proporcionados por las bibliotecas de clases de Razor son ensamblados de .NET normales.

Use la `razorclasslib` plantilla (biblioteca de clases de Razor) con el [dotnet nuevo](/dotnet/core/tools/dotnet-new) comando:

```console
dotnet new razorclasslib -o MyComponentLib1
```

Agregar archivos de componentes de Razor (*.razor*) a la biblioteca de clases de Razor.

Para agregar la biblioteca a un proyecto existente, use el [dotnet sln](/dotnet/core/tools/dotnet-sln) comando:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

```console
dotnet sln add .\MyComponentLib1
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

```console
dotnet add WebApplication1 reference MyComponentLib1
```

---

> [!NOTE]
> Bibliotecas de clases de Razor no son compatibles con las aplicaciones Blazor en ASP.NET Core Preview 3.
>
> Para crear componentes en una biblioteca que se puede compartir con aplicaciones Blazor y componentes de Razor, use una biblioteca de clases Blazor creada por el `blazorlib` plantilla.
>
> Bibliotecas de clases de Razor no admiten recursos estáticos en ASP.NET Core Preview 3. Las bibliotecas de componentes mediante la `blazorlib` plantilla puede incluir los archivos estáticos, como imágenes, JavaScript y hojas de estilos. En tiempo de compilación, los archivos estáticos se incrustan en el archivo de ensamblado compilado (*.dll*), que permite el consumo de los componentes sin tener que preocuparse sobre cómo incluir sus recursos. Los archivos incluidos en el `content` directory se marcan como recurso incrustado.

## <a name="consume-a-library-component"></a>Consumir un componente de la biblioteca

Para consumir los componentes definidos en una biblioteca en otro proyecto, el [ @addTagHelper ](xref:mvc/views/tag-helpers/intro#add-helper-label) se debe usar la directiva. Los componentes individuales se pueden agregar por nombre.

El formato general de la directiva es:

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<Component1 />
```

Por ejemplo, se agrega la siguiente directiva `Component1` de `MyComponentLib1`:

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

Sin embargo, es común para incluir todos los componentes de un ensamblado con un carácter comodín (`*`):

```cshtml
@addTagHelper *, MyComponentLib1
```

El `@addTagHelper` directiva puede incluirse en *_ViewImport.cshtml* para que los componentes disponibles para un proyecto completo o aplicada a una sola página o un conjunto de páginas dentro de una carpeta. Con el `@addTagHelper` la directiva en su lugar, los componentes de la biblioteca de componentes pueden utilizarse como si estuvieran en el mismo ensamblado que la aplicación.

## <a name="build-pack-and-ship-to-nuget"></a>Compilación, pack y envío de NuGet

Dado que las bibliotecas de componentes son bibliotecas de .NET standard, empaquetado y envío a NuGet no es diferente de empaquetado y envío de cualquier biblioteca en NuGet. Empaquetado se realiza mediante el [dotnet pack](/dotnet/core/tools/dotnet-pack) comando:

```console
dotnet pack
```

Cargar el paquete en NuGet mediante la [publicar dotnet nuget](/dotnet/core/tools/dotnet-nuget-push) comando:

```console
dotnet nuget publish
```

Cuando se usa el `blazorlib` , plantilla de recursos estáticos se incluyen en el paquete NuGet. Los consumidores de la biblioteca reciben automáticamente los scripts y hojas de estilos, por lo que no están necesarios instalar manualmente los recursos a los consumidores.
