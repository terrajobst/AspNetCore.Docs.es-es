---
title: Hospedaje e implementación de ASP.NET Core Blazor
author: guardrex
description: Descubra cómo hospedar e implementar aplicaciones de Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 26c8fcf56ab8ca68aeca93560785fc6c1144ab86
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2019
ms.locfileid: "70963686"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a>Hospedaje e implementación de ASP.NET Core Blazor

Por [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) y [Daniel Roth](https://github.com/danroth27)

## <a name="publish-the-app"></a>Publicar la aplicación

Las aplicaciones se publican para implementación en la configuración de versión.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Seleccione **Compilar** > **Publicar {aplicación}** en la barra de navegación.
1. Seleccione el *destino de publicación*. Para publicar localmente, seleccione **Carpeta**.
1. Acepte la ubicación predeterminada del campo **Elegir una carpeta** o especifique una ubicación diferente. Seleccione el botón **Publicar**.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Use el comando [dotnet publish](/dotnet/core/tools/dotnet-publish) para publicar la aplicación con una configuración de versión:

```console
dotnet publish -c Release
```

---

Al publicar la aplicación se desencadena una [restauración](/dotnet/core/tools/dotnet-restore) de las dependencias del proyecto y se [compila](/dotnet/core/tools/dotnet-build) el proyecto antes de crear los recursos para la implementación. Como parte del proceso de compilación, se quitan los ensamblados y métodos que no se usan para reducir los tiempos de carga y el tamaño de descarga de la aplicación.

Una aplicación WebAssembly de Blazor se publica en la carpeta */bin/Release/{RED DE DESTINO}/publish/{NOMBRE DE ENSAMBLADO}/dist*. Una aplicación de servidor Blazor se publica en la carpeta */bin/Release/{TARGET FRAMEWORK}/publish*.

Los recursos de la carpeta se implementan en el servidor web. La implementación puede ser un proceso manual o automatizado, en función de las herramientas de desarrollo que se usen.

## <a name="app-base-path"></a>Ruta de acceso base de la aplicación

La *ruta de acceso base de la aplicación* es la de la dirección URL raíz de la aplicación. Tenga en cuenta la siguiente aplicación principal y la de Blazor:

* La aplicación principal se llama `MyApp`:
  * La aplicación reside físicamente en *d:\\MyApp*.
  * Las solicitudes se reciben en `https://www.contoso.com/{MYAPP RESOURCE}`.
* Una aplicación Blazor llamada `CoolApp` es una subaplicación de `MyApp`:
  * La subaplicación reside físicamente en *d:\\MyApp\\CoolApp*.
  * Las solicitudes se reciben en `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`.

Sin especificar una configuración adicional para `CoolApp`, la subaplicación de este escenario desconoce dónde reside en el servidor. Por ejemplo, la aplicación no puede construir URL relativas correctas para sus recursos sin saber que reside en la ruta de acceso URL relativa `/CoolApp/`.

Para proporcionar la configuración de la ruta de acceso base de la aplicación Blazor de `https://www.contoso.com/CoolApp/`, el atributo `href` de la etiqueta `<base>` se establece en la ruta de acceso raíz relativa en el archivo *wwwroot/index.html*:

```html
<base href="/CoolApp/">
```

Al proporcionar la ruta de acceso URL relativa, un componente que no se encuentre en el directorio raíz puede construir direcciones URL relativas a la ruta de acceso raíz de la aplicación. Los componentes que se encuentran en diferentes niveles de la estructura de directorios pueden compilar vínculos a otros recursos en ubicaciones de toda la aplicación. La ruta de acceso base de la aplicación también se usa para interceptar clics en hipervínculos en los que el destino `href` del vínculo está dentro del espacio de URI de la ruta de acceso base de la aplicación y es el enrutador de Blazor quien controla la navegación interna.

En muchos escenarios de hospedaje, la ruta de acceso URL relativa a la aplicación es la raíz de la aplicación. En estos casos, la ruta de acceso URL relativa de la aplicación es una barra diagonal (`<base href="/" />`), que es la configuración predeterminada para una aplicación Blazor. En otros escenarios de hospedaje, como las subaplicaciones de IIS y GitHub Pages, la ruta de acceso base de la aplicación debe establecerse en la ruta de acceso URL relativa del servidor a la aplicación.

Para establecer la ruta de acceso base de la aplicación, actualice la etiqueta `<base>` dentro de los elementos de etiqueta `<head>` del archivo *wwwroot/index.HTML*. Establezca el valor del atributo `href` en `/{RELATIVE URL PATH}/` (la barra diagonal final es necesaria), donde `{RELATIVE URL PATH}` es la ruta de acceso URL relativa completa de la aplicación.

En el caso de una aplicación con una ruta de acceso URL relativa que no sea raíz (por ejemplo, `<base href="/CoolApp/">`), la aplicación no puede encontrar sus recursos *cuando se ejecuta de forma local*. Para solucionar este problema durante la fase de desarrollo y pruebas local, puede proporcionar un argumento de *ruta de acceso base* que coincida con el valor `href` de la etiqueta `<base>` en tiempo de ejecución. Para pasar el argumento de ruta de acceso base al ejecutar la aplicación de forma local, ejecute el comando `dotnet run` desde el directorio de la aplicación con la opción `--pathbase`:

```console
dotnet run --pathbase=/{RELATIVE URL PATH (no trailing slash)}
```

Para una aplicación con una ruta de acceso URL relativa de `/CoolApp/` (`<base href="/CoolApp/">`), el comando es el siguiente:

```console
dotnet run --pathbase=/CoolApp
```

La aplicación responde de forma local en `http://localhost:port/CoolApp`.

## <a name="deployment"></a>Implementación

Para una guía sobre la implementación, consulte los temas siguientes:

* <xref:host-and-deploy/blazor/webassembly>
* <xref:host-and-deploy/blazor/server>
