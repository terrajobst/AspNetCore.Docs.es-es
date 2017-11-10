---
title: Desarrollar aplicaciones de ASP.NET Core con dotnet watch
author: rick-anderson
description: "Este tutorial muestra cómo instalar y usar la herramienta de monitor de archivos (dotnet watch) de la CLI de .NET Core en una aplicación de ASP.NET Core."
keywords: ASP.NET Core, usar dotnet watch
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: article
ms.assetid: 563ffb3f-d369-4aa5-bf0a-7300b4e7832c
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/dotnet-watch
ms.openlocfilehash: 9baf2ce2a1270a728616a8a2ab45deca9a9cde6f
ms.sourcegitcommit: e7f01a649f240b6b57118c53314ab82f7f36f2eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2017
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a>Desarrollar aplicaciones de ASP.NET Core con dotnet watch

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Victor Hurdugaci](https://twitter.com/victorhurdugaci)

`dotnet watch` es una herramienta que ejecuta un comando de la [CLI de .NET Core](/dotnet/core/tools) cuando se modifican los archivos de código fuente. Por ejemplo, un cambio en un archivo puede desencadenar una compilación, una ejecución de prueba o una implementación.

En este tutorial usaremos una aplicación de API web existente con dos puntos de conexión: uno que devuelva una suma y otro que devuelva un producto. El método del producto contiene un error que corregiremos en este mismo tutorial.

Descargue la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample). Contiene dos proyectos: *WebApp* (una API web de ASP.NET Core) y *WebAppTests* (pruebas unitarias para la API web).

En un shell de comandos, vaya a la carpeta *WebApp* y ejecute el siguiente comando:

```console
dotnet run
```

La salida de la consola muestra mensajes similares al siguiente (indicando que la aplicación se ejecuta y espera solicitudes):

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

En un explorador web, vaya a `http://localhost:<port number>/api/math/sum?a=4&b=5`. Debería ver el resultado de `9`.

Navegue a la API del producto (`http://localhost:<port number>/api/math/product?a=4&b=5`). Devuelve `9`, no `20` tal como se esperaría. Lo corregiremos más adelante en el tutorial.

## <a name="add-dotnet-watch-to-a-project"></a>Agregar `dotnet watch` a un proyecto

1. Agregue una referencia de paquete `Microsoft.DotNet.Watcher.Tools` al archivo *.csproj*:

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup> 
    ```

1. Instale el paquete `Microsoft.DotNet.Watcher.Tools` mediante la ejecución del comando siguiente:
    
    ```console
    dotnet restore
    ```

## <a name="running-net-core-cli-commands-using-dotnet-watch"></a>Ejecución de los comandos de la CLI de .NET Core mediante `dotnet watch`

Cualquier [comando de la CLI de .NET Core](/dotnet/core/tools#cli-commands) se puede ejecutar con `dotnet watch`. Por ejemplo:

| Comando | Comando con watch |
| ---- | ----- |
| dotnet run | dotnet watch run |
| dotnet run -f netcoreapp2.0 | dotnet watch run -f netcoreapp2.0 |
| dotnet run -f netcoreapp2.0 -- --arg1 | dotnet watch run -f netcoreapp2.0 -- --arg1 |
| dotnet test | dotnet watch test |

Ejecute `dotnet watch run` en la carpeta *WebApp*. La salida de la consola indica que se ha iniciado `watch`.

## <a name="making-changes-with-dotnet-watch"></a>Efectuar cambios con `dotnet watch`

Asegúrese de que `dotnet watch` se está ejecutando.

Corrija el error en el método `Product` de *MathController.cs* para que devuelva el producto y no la suma:

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

Guarde el archivo. La salida de la consola muestra que `dotnet watch` ha detectado un cambio de archivo y ha reiniciado la aplicación.

Compruebe que `http://localhost:<port number>/api/math/product?a=4&b=5` devuelve el resultado correcto.

## <a name="running-tests-using-dotnet-watch"></a>Ejecutar pruebas con `dotnet watch`

1. Vuelva a cambiar el método `Product` de *MathController.cs* para devolver la suma y guarde el archivo.
1. En un shell de comandos, desplácese hasta la carpeta *WebAppTests*.
1. Ejecute `dotnet restore`.
1. Ejecute `dotnet watch test`. La salida que indica que se ha producido un error en una prueba y que el monitor espera cambios de archivos:

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. Corrija el código del método `Product` para que devuelva el producto. Guarde el archivo.

`dotnet watch` detecta el cambio de archivo y vuelve a ejecutar las pruebas. La salida de la consola indica que se han superado las pruebas.

## <a name="dotnet-watch-in-github"></a>dotnet-watch en GitHub

dotnet-watch forma parte del [repositorio de DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools) de GitHub.

En la [sección MSBuild](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) del [archivo Léame de dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) se describe cómo se puede configurar dotnet-watch desde el archivo de proyecto de MSBuild que se está inspeccionando. El [archivo Léame de dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) contiene información de dotnet-watch que no se trata en este tutorial.
