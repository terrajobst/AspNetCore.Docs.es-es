---
title: Vínculo del explorador en ASP.NET Core
author: ncarandini
description: Explica cómo el vínculo del explorador es una característica de Visual Studio que vincula el entorno de desarrollo con uno o más exploradores Web.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 01/09/2020
no-loc:
- SignalR
uid: client-side/using-browserlink
ms.openlocfilehash: 19cc3c2ed91bd9e05df3c036123c78ecbf81fcc0
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828275"
---
# <a name="browser-link-in-aspnet-core"></a>Vínculo del explorador en ASP.NET Core

Por [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson)y [Tom Dykstra](https://github.com/tdykstra)

El vínculo del explorador es una característica de Visual Studio. Crea un canal de comunicación entre el entorno de desarrollo y uno o varios exploradores Web. Puede usar el vínculo del explorador para actualizar la aplicación web en varios exploradores a la vez, lo que resulta útil para las pruebas entre exploradores.

## <a name="browser-link-setup"></a>Configuración del vínculo del explorador

::: moniker range=">= aspnetcore-3.0"

Agregue el paquete [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) al proyecto. Para los proyectos de ASP.NET Core Razor Pages o MVC, habilite también la compilación en tiempo de ejecución de los archivos Razor ( *. cshtml*) como se describe en <xref:mvc/views/view-compilation>. Sintaxis Razor cambios solo se aplican cuando se ha habilitado la compilación en tiempo de ejecución.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

Al convertir un proyecto ASP.NET Core 2,0 en ASP.NET Core 2,1 y pasar al [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), instale el paquete [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) para la funcionalidad de vínculo de explorador. De forma predeterminada, las plantillas de proyecto de ASP.NET Core 2,1 usan el metapaquete `Microsoft.AspNetCore.App`.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Las plantillas de proyecto **aplicación web**ASP.net Core 2,0, **vacía**y **API Web** usan el [metapaquete Microsoft. AspNetCore. All](xref:fundamentals/metapackage), que contiene una referencia de paquete para [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/). Por lo tanto, el uso del metapaquete `Microsoft.AspNetCore.All` no requiere ninguna acción adicional para que el vínculo del explorador esté disponible para su uso.

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

La plantilla de proyecto de **aplicación web** de ASP.net Core 1. x tiene una referencia de paquete para el paquete [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) . Otros tipos de proyectos requieren que agregue una referencia de paquete a `Microsoft.VisualStudio.Web.BrowserLink`.

::: moniker-end

### <a name="configuration"></a>Configuración de

Llame a `UseBrowserLink` en el método `Startup.Configure`:

```csharp
app.UseBrowserLink();
```

Normalmente, la llamada `UseBrowserLink` se coloca dentro de un bloque `if` que solo habilita el vínculo del explorador en el entorno de desarrollo. Por ejemplo:

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

Para obtener más información, vea <xref:fundamentals/environments>.

## <a name="how-to-use-browser-link"></a>Cómo usar el vínculo del explorador

Cuando tiene un proyecto de ASP.NET Core abierto, Visual Studio muestra el control de la barra de herramientas de vínculos del explorador junto al control de barra de herramientas de **destino de depuración** :

![Menú desplegable de vínculos del explorador](using-browserlink/_static/browserLink-dropdown-menu.png)

En el control de barra de herramientas de vínculo del explorador, puede:

* Actualice la aplicación web en varios exploradores a la vez.
* Abra el **Panel vínculo del explorador**.
* Habilitar o deshabilitar el **vínculo del explorador**. Nota: el vínculo del explorador está deshabilitado de forma predeterminada en Visual Studio.
* Habilitar o deshabilitar [la sincronización automática de CSS](#enable-or-disable-css-auto-sync).

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a>Actualización de la aplicación web en varios exploradores a la vez

Para elegir un solo explorador Web para iniciar al iniciar el proyecto, use el menú desplegable del control de barra de herramientas de **destino de depuración** :

![Menú desplegable F5](using-browserlink/_static/debug-target-dropdown-menu.png)

Para abrir varios exploradores a la vez, elija **examinar con...** en la lista desplegable. Mantenga presionada la tecla <kbd>Ctrl</kbd> para seleccionar los exploradores que desee y, a continuación, haga clic en **examinar**:

![Abrir muchos exploradores a la vez](using-browserlink/_static/open-many-browsers-at-once.png)

En la captura de pantalla siguiente se muestra Visual Studio con la vista de índice abierta y dos exploradores abiertos:

![Ejemplo de sincronización con dos exploradores](using-browserlink/_static/sync-with-two-browsers-example.png)

Mantenga el mouse sobre el control de la barra de herramientas de vínculos del explorador para ver los exploradores que están conectados al proyecto:

![Sugerencia de desplazamiento](using-browserlink/_static/hoover-tip.png)

Cambiar la vista de índice y todos los exploradores conectados se actualizan al hacer clic en el botón actualizar vínculo del explorador:

![exploradores: sincronizar con cambios](using-browserlink/_static/browsers-sync-to-changes.png)

El vínculo del explorador también funciona con los exploradores que se inician desde fuera de Visual Studio y se navega a la dirección URL de la aplicación.

### <a name="the-browser-link-dashboard"></a>Panel de vínculos del explorador

Abra la ventana **Panel de vínculos del explorador** en el menú desplegable del vínculo del explorador para administrar la conexión con exploradores abiertos:

![open-browserslink-dashboard](using-browserlink/_static/open-browserlink-dashboard.png)

Si no hay ningún explorador conectado, puede iniciar una sesión que no sea de depuración seleccionando el vínculo **ver en el explorador** :

![browserlink-dashboard-no-connections](using-browserlink/_static/browserlink-dashboard-no-connections.png)

De lo contrario, los exploradores conectados se muestran con la ruta de acceso a la página que se muestra en cada explorador:

![browserlink-dashboard-two-connections](using-browserlink/_static/browserlink-dashboard-two-connections.png)

También puede hacer clic en un nombre de explorador individual para actualizar solo ese explorador.

### <a name="enable-or-disable-browser-link"></a>Habilitar o deshabilitar el vínculo del explorador

Al volver a habilitar el vínculo del explorador después de deshabilitarlo, debe actualizar los exploradores para volver a conectarlos.

### <a name="enable-or-disable-css-auto-sync"></a>Habilitar o deshabilitar la sincronización automática de CSS

Cuando se habilita la sincronización automática de CSS, los exploradores conectados se actualizan automáticamente cuando se realiza cualquier cambio en los archivos CSS.

## <a name="how-it-works"></a>Cómo funciona

El vínculo del explorador usa [SignalR](xref:signalr/introduction) para crear un canal de comunicación entre Visual Studio y el explorador. Cuando el vínculo del explorador está habilitado, Visual Studio actúa como un servidor de SignalR al que se pueden conectar varios clientes (exploradores). El vínculo del explorador también registra un componente de middleware en la canalización de solicitudes ASP.NET Core. Este componente inserta referencias de `<script>` especiales en cada solicitud de página del servidor. Puede ver las referencias de script seleccionando **Ver código fuente** en el explorador y desplazándose hasta el final del contenido de la etiqueta `<body>`:

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

Los archivos de origen no se modifican. El componente middleware inserta las referencias de script dinámicamente.

Dado que el código del explorador es todo JavaScript, funciona en todos los exploradores que SignalR admite sin necesidad de un complemento de explorador.
