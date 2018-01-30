---
title: "Vínculo de explorador en ASP.NET Core"
author: ncarandini
description: "Explica cómo vínculo de explorador es una característica de Visual Studio que se vincula el entorno de desarrollo con uno o varios exploradores web."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-browserlink
ms.openlocfilehash: 3e62bdd180bb1f5e2ce0645a8cf13c9ffe76197e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="browser-link-in-aspnet-core"></a>Vínculo de explorador en ASP.NET Core 

Por [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), y [Tom Dykstra](https://github.com/tdykstra)

Vínculo de explorador es una característica de Visual Studio que crea un canal de comunicación entre el entorno de desarrollo y uno o varios exploradores web. Puede usar el vínculo de explorador para actualizar la aplicación web en varios exploradores a la vez, lo que resulta útil para las pruebas de varios exploradores.

## <a name="browser-link-setup"></a>Programa de instalación de vínculo de explorador

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

El núcleo de ASP.NET 2.x **aplicación Web**, **vacía**, y **API Web** plantilla proyectos utilizan la [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) paquete de Meta, que contiene una referencia de paquete para [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/). Por tanto, se usan las `Microsoft.AspNetCore.All` meta-package no requiere ninguna acción adicional para que estén disponibles para su uso vínculo de explorador.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

El núcleo de ASP.NET 1.x **aplicación Web** plantilla de proyecto tiene una referencia de paquete para la [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) paquete. El **vacía** o **API Web** proyectos de plantilla requieren que agregue una referencia de paquete a `Microsoft.VisualStudio.Web.BrowserLink`.

Puesto que esta es una característica de Visual Studio, la manera más fácil para agregar el paquete a un **vacía** o **API Web** proyecto de plantilla es abrir el **Package Manager Console** (**Vista** > **otras ventanas** > **Package Manager Console**) y ejecute el siguiente comando:

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

Como alternativa, puede usar **Administrador de paquetes de NuGet**. Haga clic en el nombre del proyecto en **el Explorador de soluciones** y elija **administrar paquetes de NuGet**:

![Administrador de paquetes de NuGet abierto](using-browserlink/_static/open-nuget-package-manager.png)

Buscar e instalar el paquete:

![Agregue el paquete con el Administrador de paquetes de NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

---

### <a name="configuration"></a>Configuración

En el `Configure` método de la *Startup.cs* archivo:

```csharp
app.UseBrowserLink();
```

Normalmente es el código dentro de un `if` bloque que solo habilita el vínculo de explorador en el entorno de desarrollo, como se muestra aquí:

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

Para obtener más información, consulte [Trabajar con varios entornos](xref:fundamentals/environments).

## <a name="how-to-use-browser-link"></a>Cómo usar el vínculo de explorador

Cuando hay un proyecto de ASP.NET Core abierta, Visual Studio muestra el control de barra de herramientas de vínculo de explorador junto a la **destino de depuración** control de barra de herramientas:

![Menú desplegable de vínculo de explorador](using-browserlink/_static/browserLink-dropdown-menu.png)

Desde el control de barra de herramientas de vínculo de explorador, hacer lo siguiente:

* Actualizar la aplicación web en varios exploradores a la vez.
* Abra la **panel de vínculos de explorador**.
* Habilitar o deshabilitar **vínculo de explorador**. Nota: El vínculo de explorador está deshabilitada de forma predeterminada en Visual Studio 2017 (15.3).
* Habilitar o deshabilitar [sincronización automática de CSS](#enable-or-disable-css-auto-sync).

> [!NOTE]
> Algunos complementos de Visual Studio, sobre todo *2015 de paquete de extensión de Web* y *2017 de paquete de extensión de Web*, ofrecen una funcionalidad extendida de vínculo de explorador, pero algunas de las características adicionales no funcionan con ASP. Proyectos de red principal.

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a>Actualizar la aplicación web en varios exploradores a la vez

Para elegir un explorador web único para iniciar al iniciar el proyecto, utilice el menú desplegable de la **destino de depuración** control de barra de herramientas:

![Menú desplegable de F5](using-browserlink/_static/debug-target-dropdown-menu.png)

Para abrir varios exploradores a la vez, elija **examinar con...**  en la misma lista desplegable. Mantenga presionada la tecla CTRL para seleccionar los exploradores que desee y, a continuación, haga clic en **examinar**:

![Abrir a la vez muchos exploradores](using-browserlink/_static/open-many-browsers-at-once.png)

Esta es una captura de pantalla que muestra Visual Studio con la vista de índice abierto y dos exploradores abiertos:

![Sincronizar con el ejemplo de dos exploradores](using-browserlink/_static/sync-with-two-browsers-example.png)

Mantenga el mouse sobre el control de barra de herramientas de vínculo de explorador para ver los exploradores que están conectados al proyecto:

![Sugerencia al mantener el mouse](using-browserlink/_static/hoover-tip.png)

Cambie la vista de índice y se actualizan todos los exploradores conectados al hacer clic en el botón de actualización de vínculo de explorador:

![sincronización de exploradores de cambios](using-browserlink/_static/browsers-sync-to-changes.png)

Vínculo de explorador también funciona con exploradores que inicie desde fuera de Visual Studio y navegue a la dirección URL de la aplicación.

### <a name="the-browser-link-dashboard"></a>El panel de vínculos de explorador

Abrir el panel de vínculo de explorador desde el menú para administrar la conexión con exploradores abiertos desplegable de vínculo de explorador:

![open-browserslink-dashboard](using-browserlink/_static/open-browserlink-dashboard.png)

Si no hay ningún explorador está conectado, puede iniciar una sesión de depuración no seleccionando la *ver en el explorador* vínculo:

![conexiones browserlink-panel-n](using-browserlink/_static/browserlink-dashboard-no-connections.png)

En caso contrario, se muestran los exploradores conectados con la ruta de acceso a la página que se muestra cada explorador:

![browserlink-dashboard-two-connections](using-browserlink/_static/browserlink-dashboard-two-connections.png)

Si lo desea, puede hacer clic en un nombre de lista del explorador para actualizar esa única del explorador.

### <a name="enable-or-disable-browser-link"></a>Habilitar o deshabilitar el vínculo de explorador

Al volver a habilitar vínculo de explorador después de deshabilitarlo, debe actualizar los exploradores para volver a conectarlos.

### <a name="enable-or-disable-css-auto-sync"></a>Habilitar o deshabilitar la sincronización automática de CSS

Cuando se habilita la sincronización automática de CSS, exploradores conectados se actualizan automáticamente cuando se realiza cualquier cambio a los archivos CSS.

## <a name="how-does-it-work"></a>¿Cómo funciona?

Vínculo de explorador usa SignalR para crear un canal de comunicación entre Visual Studio y el explorador. Cuando se habilita el vínculo de explorador, Visual Studio actúa como un servidor de SignalR que varios clientes (exploradores) pueden conectarse a. Vínculo de explorador también registra un componente de middleware en la canalización de solicitudes ASP.NET. Este componente inserta especial `<script>` referencias en cada solicitud de página desde el servidor. Puede ver las referencias de script seleccionando **ver código fuente** en el explorador y el desplazamiento hasta el final de la `<body>` etiquetar contenido:

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

No modifican los archivos de origen. El componente de middleware inserta dinámicamente las referencias de script. 

Dado que el código del lado del explorador es JavaScript todos, funciona en todos los exploradores compatibles con SignalR sin necesidad de un complemento de explorador.
