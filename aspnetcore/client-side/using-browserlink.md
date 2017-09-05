---
title: "Vínculo de explorador en ASP.NET Core"
author: ncarandini
description: "Una característica de Visual Studio que se vincula el entorno de desarrollo con uno o varios exploradores web"
keywords: "Núcleo de ASP.NET, el vínculo de explorador, la sincronización de CSS"
ms.author: riande
manager: wpickett
ms.date: 12/28/2016
ms.topic: article
ms.assetid: 11813d4c-3f8a-445a-b23b-e4a57d001abc
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-browserlink
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b2ff38288cee3e9ca42a07c219521bb79a00a359
ms.sourcegitcommit: 4e84d8bf5f404bb77f3d41665cf7e7374fc39142
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2017
---
# <a name="introduction-to-browser-link-in-aspnet-core"></a>Introducción al vínculo de explorador en ASP.NET Core 

Por [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), y [Tom Dykstra](https://github.com/tdykstra)

Vínculo de explorador es una característica de Visual Studio que crea un canal de comunicación entre el entorno de desarrollo y uno o varios exploradores web. Puede usar el vínculo de explorador para actualizar la aplicación web en varios exploradores a la vez, lo que resulta útil para las pruebas de varios exploradores.

## <a name="browser-link-setup"></a>Programa de instalación de vínculo de explorador

El núcleo de ASP.NET **aplicación Web** plantillas de proyecto en Visual Studio 2015 y versiones posteriores incluyen todo lo necesario para el vínculo de explorador.

Para agregar el vínculo de explorador a un proyecto que ha creado mediante ASP.NET Core **vacía** o **API Web** plantilla, siga estos pasos:

1. Agregar el *Microsoft.VisualStudio.Web.BrowserLink.Loader* paquete 
2. Agregue el código de configuración en el *Startup.cs* archivo.

### <a name="add-the-package"></a>Agregue el paquete

Puesto que esta es una característica de Visual Studio, la manera más fácil de agregar el paquete es abrir el **Package Manager Console** (**Vista > otras ventanas > Package Manager Console**) y ejecute el siguiente comando:

```console
install-package Microsoft.VisualStudio.Web.BrowserLink.Loader
```

Como alternativa, puede usar **Administrador de paquetes de NuGet**.  Haga clic en el nombre del proyecto en **el Explorador de soluciones**y elija **administrar paquetes de NuGet**. 

![Administrador de paquetes de NuGet abierto](using-browserlink/_static/open-nuget-package-manager.png)

A continuación, buscar e instalar el paquete.

![Agregue el paquete con el Administrador de paquetes de NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

### <a name="add-configuration-code"></a>Agregar código de configuración

Abra la *Startup.cs* archivo y en el `Configure` método agregue el código siguiente:

```csharp
app.UseBrowserLink();
```

Por lo general es que el código dentro de un `if` bloque que permite el vínculo de explorador únicamente en el entorno de desarrollo, como se muestra aquí:

[!code-csharp[Main](./using-browserlink/sample/BrowserLinkSample/src/BrowserLinkSample/Startup.cs?highlight=1,4&range=40-44)]

Para obtener más información, consulte [trabajar con varios entornos](../fundamentals/environments.md).

## <a name="how-to-use-browser-link"></a>Cómo usar el vínculo de explorador

Cuando hay un proyecto de ASP.NET Core abierta, Visual Studio muestra el control de barra de herramientas de vínculo de explorador junto a la **destino de depuración** control de barra de herramientas:

![Menú desplegable de vínculo de explorador](using-browserlink/_static/browserLink-dropdown-menu.png)

Desde el control de barra de herramientas de vínculo de explorador, hacer lo siguiente:

- Actualizar la aplicación web en varios exploradores a la vez
- Abra el **panel de vínculos de explorador**
- Habilitar o deshabilitar **vínculo de explorador**
- Habilitar o deshabilitar la sincronización automática de CSS

> [!NOTE]
> Algunos complementos de Visual Studio, sobre todo *2015 de paquete de extensión de Web* y *2017 de paquete de extensión de Web*, ofrecen una funcionalidad extendida de vínculo de explorador, pero algunas de las características adicionales no funcionan con ASP. Proyectos de red principal.

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a>Actualizar la aplicación web en varios exploradores a la vez

Para elegir un explorador web único para iniciar al iniciar el proyecto, utilice el menú desplegable de la **destino de depuración** control de barra de herramientas:

![Menú desplegable de F5](using-browserlink/_static/debug-target-dropdown-menu.png)

Para abrir varios exploradores a la vez, elija **examinar con...**  en la misma lista desplegable.  Mantenga presionada la tecla CTRL para seleccionar los exploradores que desee y, a continuación, haga clic en **examinar**:

![Abrir a la vez muchos exploradores](using-browserlink/_static/open-many-browsers-at-once.png)

Esta es una captura de pantalla de ejemplo que muestra Visual Studio con la vista de índice abierto y dos exploradores abiertos:

![Sincronizar con el ejemplo de dos exploradores](using-browserlink/_static/sync-with-two-browsers-example.png)

Mantenga el mouse sobre el control de barra de herramientas de vínculo de explorador para ver los exploradores que están conectados al proyecto:

![Sugerencia al mantener el mouse](using-browserlink/_static/hoover-tip.png)

Cambie la vista de índice y se actualizan todos los exploradores conectados al hacer clic en el botón de actualización de vínculo de explorador:

![sincronización de exploradores de cambios](using-browserlink/_static/browsers-sync-to-changes.png)

Vínculo de explorador también funciona con exploradores que inicie desde fuera de Visual Studio y navegue a la dirección URL de la aplicación.

### <a name="the-browser-link-dashboard"></a>El panel de vínculos de explorador

Abrir el panel de vínculo de explorador desde el menú para administrar la conexión con exploradores abiertos desplegable de vínculo de explorador:

![Abrir Panel de browserslink](using-browserlink/_static/open-browserlink-dashboard.png)

Si no hay ningún explorador está conectado, puede iniciar una sesión de depuración no clic en el _ver en el explorador_ vínculo:

![conexiones browserlink-panel-n](using-browserlink/_static/browserlink-dashboard-no-connections.png)

En caso contrario, se muestran los exploradores conectados con la ruta de acceso a la página que se muestra cada explorador:

![browserlink panel dos conexiones](using-browserlink/_static/browserlink-dashboard-two-connections.png)

Si lo desea, puede hacer clic en un nombre de lista del explorador para actualizar esa única del explorador.

### <a name="enable-or-disable-browser-link"></a>Habilitar o deshabilitar el vínculo de explorador

Al volver a habilitar vínculo de explorador después de deshabilitarlo, tendrá que actualizar los exploradores para volver a conectarlos.

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

No se modifican los archivos de origen. El componente de middleware inserta dinámicamente las referencias de script. 

Dado que el código del lado del explorador es JavaScript todos, funciona en todos los exploradores compatibles con SignalR, sin necesidad de cualquier complemento de explorador.
