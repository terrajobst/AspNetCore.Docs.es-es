---
title: Vínculo de explorador en ASP.NET Core
author: ncarandini
description: Explica cómo el vínculo de explorador es una característica de Visual Studio que se vincula el entorno de desarrollo con uno o varios exploradores web.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/22/2017
uid: client-side/using-browserlink
ms.openlocfilehash: 5ab15c841c472e6c9d47bad70fcf5e0c6dc3010f
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894184"
---
# <a name="browser-link-in-aspnet-core"></a>Vínculo de explorador en ASP.NET Core

Por [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), y [Tom Dykstra](https://github.com/tdykstra)

Vínculo de explorador es una característica de Visual Studio que crea un canal de comunicación entre el entorno de desarrollo y uno o varios exploradores web. Puede usar el vínculo de explorador para actualizar la aplicación web en varios exploradores a la vez, que es útil para probar varios exploradores.

## <a name="browser-link-setup"></a>Configuración de vínculo de explorador

::: moniker range=">= aspnetcore-2.1"

Al convertir un proyecto de ASP.NET Core 2.0 a ASP.NET Core 2.1 y realizar la transición a la [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app), instale el [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) del paquete de Funcionalidad de BrowserLink. Usan las plantillas de proyecto de ASP.NET Core 2.1 la `Microsoft.AspNetCore.App` metapaquete de forma predeterminada.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

ASP.NET Core 2.0 **aplicación Web**, **vacía**, y **API Web** uso de plantillas de proyecto la [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage) , que contiene una referencia de paquete para [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/). Por lo tanto, uso el `Microsoft.AspNetCore.All` metapaquete requiere ninguna acción para que el vínculo de explorador esté disponible para su uso.

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

ASP.NET Core 1.x **aplicación Web** plantilla de proyecto tiene una referencia de paquete para el [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) paquete. El **vacía** o **API Web** proyectos de plantilla requieren que agregue una referencia de paquete para `Microsoft.VisualStudio.Web.BrowserLink`.

Puesto que esta es una característica de Visual Studio, la manera más fácil para agregar el paquete a un **vacía** o **API Web** es abrir el proyecto de plantilla el **Package Manager Console** (**Vista** > **Other Windows** > **Package Manager Console**) y ejecute el siguiente comando:

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

Como alternativa, puede usar **Administrador de paquetes de NuGet**. Haga clic en el nombre del proyecto en **el Explorador de soluciones** y elija **administrar paquetes de NuGet**:

![Administrador de paquetes NuGet abierta](using-browserlink/_static/open-nuget-package-manager.png)

Buscar e instalar el paquete:

![Agregue el paquete con el Administrador de paquetes de NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

::: moniker-end

### <a name="configuration"></a>Configuración

En el método `Startup.Configure`:

```csharp
app.UseBrowserLink();
```

Normalmente es el código dentro de un `if` bloque que permite solo el vínculo de explorador en el entorno de desarrollo, como se muestra aquí:

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

Para obtener más información, consulte [Uso de varios entornos](xref:fundamentals/environments).

## <a name="how-to-use-browser-link"></a>Cómo usar el vínculo de explorador

Cuando tiene abierto un proyecto de ASP.NET Core, Visual Studio muestra el control de barra de herramientas de vínculo de explorador junto a la **depurar destino** toolbar (control):

![Menú desplegable de vínculo de explorador](using-browserlink/_static/browserLink-dropdown-menu.png)

Desde el control de barra de herramientas vínculo de explorador, hacer lo siguiente:

* Actualizar la aplicación web a la vez en varios exploradores.
* Abra el **panel vínculo de explorador**.
* Habilitar o deshabilitar **Browser Link**. Nota: El vínculo de explorador está deshabilitada de forma predeterminada en Visual Studio 2017 (15.3).
* Habilitar o deshabilitar [sincronización automática de CSS](#enable-or-disable-css-auto-sync).

> [!NOTE]
> Algunos complementos de Visual Studio, sobre todo *Web extensión Pack 2015* y *Web extensión Pack 2017*, ofrecen funcionalidad ampliada de vínculo de explorador, pero algunas de las características adicionales no funcionan con ASP. Proyectos de .NET Core.

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a>Actualizar la aplicación web en varios exploradores a la vez

Para elegir un explorador web única para iniciar al iniciar el proyecto, use el menú desplegable en el **depurar destino** toolbar (control):

![Menú desplegable de F5](using-browserlink/_static/debug-target-dropdown-menu.png)

Para abrir a la vez varios exploradores, elija **examinar con...**  en la misma lista desplegable. Mantenga presionada la tecla CTRL para seleccionar los exploradores que desee y, a continuación, haga clic en **examinar**:

![Abrir muchos exploradores a la vez](using-browserlink/_static/open-many-browsers-at-once.png)

Este es una captura de pantalla que muestra Visual Studio con la vista de índice abierto y dos exploradores abiertos:

![Sincronizar con el ejemplo de dos exploradores](using-browserlink/_static/sync-with-two-browsers-example.png)

Mantenga el mouse sobre el control de barra de herramientas de vínculo de explorador para ver los exploradores que están conectados al proyecto:

![Sugerencia al mantener el mouse](using-browserlink/_static/hoover-tip.png)

Cambiar la vista de índice y se actualizan todos los exploradores conectados al hacer clic en el botón de actualización de Browser Link:

![sincronización de los exploradores de cambios](using-browserlink/_static/browsers-sync-to-changes.png)

Vínculo de explorador también funciona con los exploradores que inicie desde fuera de Visual Studio y navegue a la dirección URL de la aplicación.

### <a name="the-browser-link-dashboard"></a>El panel de vínculos de explorador

Abra el panel de vínculos de explorador en el menú para administrar la conexión con los exploradores Abrir desplegable Browser Link:

![Abrir Panel de browserslink](using-browserlink/_static/open-browserlink-dashboard.png)

Si no hay ningún explorador está conectado, puede iniciar una sesión de no depuración seleccionando el *ver en el explorador* vínculo:

![conexiones browserlink-dashboard-n](using-browserlink/_static/browserlink-dashboard-no-connections.png)

En caso contrario, se muestran los exploradores conectados con la ruta de acceso a la página que muestra cada explorador:

![browserlink panel dos conexiones](using-browserlink/_static/browserlink-dashboard-two-connections.png)

Si lo desea, puede hacer clic en un nombre de lista del explorador para actualizar ese explorador único.

### <a name="enable-or-disable-browser-link"></a>Habilitar o deshabilitar el vínculo de explorador

Al volver a habilitar vínculo de explorador después de deshabilitarlo, debe actualizar los exploradores para volver a conectarse a ellos.

### <a name="enable-or-disable-css-auto-sync"></a>Habilitar o deshabilitar la sincronización automática de CSS

Cuando se habilita la sincronización automática de CSS, los exploradores conectados se actualizan automáticamente cuando se realiza cualquier cambio a los archivos CSS.

## <a name="how-it-works"></a>Cómo funciona

Vínculo de explorador usa SignalR para crear un canal de comunicación entre Visual Studio y el explorador. Cuando se habilita el vínculo de explorador, Visual Studio actúa como un servidor de SignalR que varios clientes (exploradores) pueden conectarse a. Vínculo de explorador también registra un componente de middleware en la canalización de solicitudes ASP.NET. Este componente inserta especial `<script>` referencias en cada solicitud de página desde el servidor. Puede ver las referencias de script seleccionando **ver código fuente** en el explorador y desplácese hasta el final de la `<body>` etiquetar contenido:

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

No se modifican los archivos de origen. El componente de middleware inserta dinámicamente las referencias de script.

Dado que el código del lado del explorador es JavaScript todas, funciona en todos los exploradores compatibles con SignalR sin necesidad de un complemento de explorador.
