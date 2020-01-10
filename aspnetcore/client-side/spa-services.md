---
title: Usar servicios de JavaScript para crear aplicaciones de una sola página en ASP.NET Core
author: scottaddie
description: Obtenga información sobre las ventajas de usar los servicios de JavaScript para crear una aplicación de una sola página (SPA) respaldada por ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 09/06/2019
uid: client-side/spa-services
ms.openlocfilehash: caff1a735de3274b371f67e6e485dc42e579452c
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828222"
---
# <a name="use-javascript-services-to-create-single-page-applications-in-aspnet-core"></a>Usar servicios de JavaScript para crear aplicaciones de una sola página en ASP.NET Core

Por [Scott Addie](https://github.com/scottaddie) y [Fiyaz Hasan](https://fiyazhasan.me/)

Una aplicación de página única (SPA) es un tipo conocido de aplicación web debido a su experiencia de usuario completa e inherente. La integración de bibliotecas o marcos de SPA del lado cliente, como [angular](https://angular.io/) o [reAct](https://facebook.github.io/react/), con marcos de servidor como ASP.net Core puede ser difícil. Los servicios de JavaScript se desarrollaron para reducir la fricción en el proceso de integración. Permite el funcionamiento sin problemas entre los distintos componentes tecnológicos del lado servidor y cliente.

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> Las características descritas en este artículo están obsoletas a partir de ASP.NET Core 3,0. Hay disponible un mecanismo de integración de marcos de SPA más sencillo en el paquete NuGet [Microsoft. AspNetCore. SpaServices. Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices.Extensions) . Para obtener más información, vea [[anuncio] obsoleto Microsoft. AspNetCore. SpaServices y Microsoft. AspNetCore. NodeServices](https://github.com/dotnet/AspNetCore/issues/12890).

::: moniker-end

## <a name="what-is-javascript-services"></a>Qué son los servicios de JavaScript

Servicios de JavaScript es una colección de tecnologías del lado cliente para ASP.NET Core. Su objetivo es a la posición de ASP.NET Core como plataforma de servidor preferido de los desarrolladores para la creación de spa.

Los servicios de JavaScript se componen de dos paquetes de NuGet distintos:

* [Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)
* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)

Estos paquetes son útiles en los escenarios siguientes:

* Ejecutar JavaScript en el servidor
* Usa un marco o biblioteca de SPA
* Compile los recursos del lado cliente con Webpack

Gran parte el foco en este artículo se coloca sobre el uso del paquete SpaServices.

## <a name="what-is-spaservices"></a>¿Qué es SpaServices

SpaServices se creó para colocar ASP.NET Core como la plataforma del lado servidor preferida de los desarrolladores para compilar las SPA. SpaServices no es necesario para desarrollar Spa con ASP.NET Core y no bloquea a los desarrolladores en un marco de cliente determinado.

SpaServices proporciona infraestructura útil, como:

* [Procesamiento previo del lado servidor](#server-side-prerendering)
* [Middleware de desarrollo de Webpack](#webpack-dev-middleware)
* [Sustitución del módulo de acceso frecuente](#hot-module-replacement)
* [Aplicaciones auxiliares de enrutamientos](#routing-helpers)

De forma colectiva, estos componentes de infraestructura mejoran el flujo de trabajo de desarrollo y la experiencia en tiempo de ejecución. Los componentes pueden adoptar individualmente.

## <a name="prerequisites-for-using-spaservices"></a>Requisitos previos para usar SpaServices

Para trabajar con SpaServices, instale lo siguiente:

* [Node.js](https://nodejs.org/) (versión 6 o posterior) con npm

  * Para comprobar estos componentes se instalan y se pueden encontrar, ejecute lo siguiente desde la línea de comandos:

    ```console
    node -v && npm -v
    ```

  * Si se implementa en un sitio web de Azure, no se requiere ninguna acción&mdash;node. js está instalado y disponible en los entornos de servidor.

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * En Windows con Visual Studio 2017, el SDK se instala seleccionando la carga de trabajo de **desarrollo multiplataforma de .net Core** .

* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) paquete NuGet

## <a name="server-side-prerendering"></a>Procesamiento previo del lado servidor

Una aplicación universal (también conocida como isomorfa) es una aplicación de JavaScript capaz de ejecutarse tanto en el servidor como en el cliente. Angular, React y otros marcos populares proporcionan una plataforma universal para el desarrollo de este estilo de aplicaciones. La idea es representar primero los componentes del marco en el servidor a través de Node.js y, después, delegar el resto de la ejecución al cliente.

ASP.NET Core [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro) proporcionada por SpaServices simplificar la implementación de procesamiento previo de lado servidor mediante la invocación de las funciones de JavaScript en el servidor.

### <a name="server-side-prerendering-prerequisites"></a>Requisitos previos de la representación en el servidor

Instale el paquete [de NPM de preprocesamiento de ASPNET](https://www.npmjs.com/package/aspnet-prerendering) :

```console
npm i -S aspnet-prerendering
```

### <a name="server-side-prerendering-configuration"></a>Configuración de la representación previa en el lado servidor

Las aplicaciones auxiliares de etiquetas se hacen reconocibles a través del registro del espacio de nombres en el proyecto *_ViewImports.cshtml* archivo:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

Estas aplicaciones auxiliares de etiquetas abstraer las complejidades de la comunicación directa con la API de bajo nivel mediante el aprovechamiento de una sintaxis similar a HTML dentro de la vista de Razor:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="asp-prerender-module-tag-helper"></a>ASP-PreRender-aplicación auxiliar de etiquetas

El `asp-prerender-module` ejecuta la aplicación auxiliar de etiquetas, utilizados en el ejemplo de código anterior, *ClientApp/dist/main-server.js* en el servidor a través de Node.js. Para no complicarlo, *main server.js* archivo es un artefacto de la tarea de transpilación de TypeScript y JavaScript en el [Webpack](https://webpack.github.io/) proceso de compilación. Webpack define un alias de punto de entrada de `main-server`; y comienza el cruce seguro de que el gráfico de dependencias para este alias en el *ClientApp, arranque-server.ts* archivo:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

En el siguiente ejemplo Angular, el *ClientApp, arranque-server.ts* archivo utiliza el `createServerRenderer` función y `RenderResult` el tipo de la `aspnet-prerendering` paquete de npm para configurar la representación del servidor a través de Node.js. El marcado HTML destinado para la representación del lado servidor se pasa a una llamada de función de resolución, que se ajusta en JavaScript fuertemente tipado `Promise` objeto. El `Promise` es de importancia del objeto que proporciona asincrónicamente el marcado HTML para la página para la inserción en el elemento de DOM marcador de posición.

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="asp-prerender-data-tag-helper"></a>ASP-PreRender-aplicación auxiliar de etiquetas de datos

Cuando se combina con la `asp-prerender-module` aplicación auxiliar de etiquetas, la `asp-prerender-data` aplicación auxiliar de etiquetas se puede usar para pasar información contextual de la vista de Razor a JavaScript del lado servidor. Por ejemplo, el marcado siguiente pasa los datos de usuario en el `main-server` módulo:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

Los datos recibidos `UserName` argumento se serializa utilizando el serializador JSON integrado y se almacena en la `params.data` objeto. En el siguiente ejemplo Angular, los datos se utilizan para construir un saludo personalizado dentro de un `h1` elemento:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

Los nombres de propiedad pasados en las aplicaciones auxiliares de etiquetas se representan con la notación **PascalCase** . Compare esto con JavaScript, donde se representan los mismos nombres de propiedad con **camelCase**. La configuración predeterminada de serialización de JSON es responsable de esta diferencia.

Para ampliar el ejemplo de código anterior, datos pueden pasarse desde el servidor a la vista por hidratación el `globals` propiedad proporcionada a la `resolve` función:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

El `postList` matriz definida dentro de la `globals` objeto está adjunto al explorador global `window` objeto. Esta elevación de variables en ámbito global elimina la duplicación de esfuerzos, sobre todo lo que respecta a la carga de los mismos datos una vez en el servidor y de nuevo en el cliente.

![variable global de postList adjuntado al objeto de ventana](spa-services/_static/global_variable.png)

## <a name="webpack-dev-middleware"></a>Middleware de desarrollo de Webpack

[Middleware de desarrollo de Webpack](https://webpack.js.org/guides/development/#using-webpack-dev-middleware) incorpora un flujo de trabajo de desarrollo optimizado por el cual Webpack compila los recursos a petición. El middleware automáticamente compila y sirve de recursos del cliente cuando se vuelve a cargar una página en el explorador. El enfoque alternativo es invocar manualmente la Webpack a través de script de compilación del proyecto npm cuando cambia una dependencia de terceros o código personalizado. Script de compilación de un npm el *package.json* archivo se muestra en el ejemplo siguiente:

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="webpack-dev-middleware-prerequisites"></a>Requisitos previos del middleware de desarrollo de WebPack

Instale el paquete [de NPM de ASPNET-WebPack](https://www.npmjs.com/package/aspnet-webpack) :

```console
npm i -D aspnet-webpack
```

### <a name="webpack-dev-middleware-configuration"></a>Configuración de middleware de dev de WebPack

Middleware de desarrollo de Webpack está registrado en la canalización de solicitudes HTTP mediante el siguiente código en el *Startup.cs* del archivo `Configure` método:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_WebpackMiddlewareRegistration&highlight=4)]

El `UseWebpackDevMiddleware` método de extensión debe llamarse antes [registrar archivos estáticos de hospedaje](xref:fundamentals/static-files) a través de la `UseStaticFiles` método de extensión. Por motivos de seguridad, registre el middleware solo cuando la aplicación se ejecuta en modo de desarrollo.

El *webpack.config.js* del archivo `output.publicPath` propiedad indica el middleware para inspeccionar el `dist` carpeta para los cambios:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

## <a name="hot-module-replacement"></a>Sustitución del módulo de acceso frecuente

Piense de Webpack [reemplazo en caliente módulo](https://webpack.js.org/concepts/hot-module-replacement/) característica (HMR) como una evolución de [Webpack Dev Middleware](#webpack-dev-middleware). HMR presenta las mismas ventajas, pero simplifica aún más el flujo de trabajo de desarrollo actualizando automáticamente el contenido de la página después de compilar los cambios. No lo confunda con una actualización del explorador, lo que interferiría con el estado actual de en memoria y la sesión de depuración de la SPA. Hay un vínculo directo entre el servicio de Webpack Dev Middleware y el explorador, lo que significa que los cambios se insertan en el explorador.

### <a name="hot-module-replacement-prerequisites"></a>Requisitos previos de sustitución de módulos activos

Instale el paquete [WebPack-Hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) NPM:

```console
npm i -D webpack-hot-middleware
```

### <a name="hot-module-replacement-configuration"></a>Configuración de reemplazo del módulo activo

El componente HMR debe estar registrado en la canalización de solicitudes HTTP de MVC en el `Configure` método:

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

Igual que ocurría con [Webpack Dev Middleware](#webpack-dev-middleware), `UseWebpackDevMiddleware` método de extensión debe llamarse antes el `UseStaticFiles` método de extensión. Por motivos de seguridad, registre el middleware solo cuando la aplicación se ejecuta en modo de desarrollo.

El *webpack.config.js* archivo debe definir un `plugins` matriz, incluso si se deja vacío:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

Después de cargar la aplicación en el explorador, pestaña de la consola de las herramientas de desarrollo proporciona la confirmación de la activación de HMR:

![Mensaje de conectado hot sustitución del módulo](spa-services/_static/hmr_connected.png)

## <a name="routing-helpers"></a>Aplicaciones auxiliares de enrutamientos

En la mayoría de los Spa basados en ASP.NET Core, a menudo se desea el enrutamiento del lado cliente además del enrutamiento del lado servidor. Los sistemas de enrutamiento SPA y MVC pueden trabajar de forma independiente sin interferencias. Hay, sin embargo, un borde case plantean desafíos: identificación de respuestas HTTP 404.

Considere el escenario en el que una ruta sin extensión de `/some/page` se utiliza. Suponga que la solicitud no hacer coincidir el patrón una ruta de servidor, pero su patrón coincide con una ruta del lado cliente. Ahora considere una solicitud entrante para `/images/user-512.png`, por lo general que espera encontrar un archivo de imagen en el servidor. Si la ruta de acceso a recursos solicitada no coincide con ninguna ruta del lado servidor o archivo estático, es poco probable que la aplicación del lado cliente la administrara&mdash;, por lo general, se desea devolver un código de Estado HTTP 404.

### <a name="routing-helpers-prerequisites"></a>Requisitos previos para el enrutamiento

Instale el paquete NPM de enrutamiento del lado cliente. Uso de Angular como ejemplo:

```console
npm i -S @angular/router
```

### <a name="routing-helpers-configuration"></a>Configuración de aplicaciones auxiliares de enrutamiento

Un método de extensión denominado `MapSpaFallbackRoute` se utiliza en el `Configure` método:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_MvcRoutingTable&highlight=7-9)]

Las rutas se evalúan en el orden en que están configuradas. Por lo tanto, el `default` ruta en el ejemplo de código anterior se usa primero para la coincidencia de patrones.

## <a name="create-a-new-project"></a>Crear un proyecto nuevo

Los servicios de JavaScript proporcionan plantillas de aplicación preconfiguradas. SpaServices se usa en estas plantillas junto con diferentes marcos y bibliotecas, como angular, reAct y Redux.

Estas plantillas se pueden instalar a través de la CLI de .NET Core, ejecute el comando siguiente:

```dotnetcli
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

Se muestra una lista de plantillas SPA disponibles:

| Plantillas                                 | Nombre corto | Lenguaje | Etiquetas        |
| ------------------------------------------| :--------: | :------: | :---------: |
| MVC de ASP.NET Core con Angular             | angular    | [C#]     | Web/MVC/SPA |
| MVC de ASP.NET Core con React.js            | react      | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core con React.js y Redux  | reactredux | [C#]     | Web/MVC/SPA |

Para crear un nuevo proyecto con una de las plantillas SPA, incluya el **nombre corto** de la plantilla en el [dotnet nuevo](/dotnet/core/tools/dotnet-new) comando. El siguiente comando crea una aplicación Angular con ASP.NET Core MVC configurada para el lado del servidor:

```dotnetcli
dotnet new angular
```

### <a name="set-the-runtime-configuration-mode"></a>Establecer el modo de configuración en tiempo de ejecución

Existen dos modos de configuración en tiempo de ejecución principal:

* **Desarrollo**:
  * Incluye mapas de código fuente para facilitar la depuración.
  * No optimice el código del lado cliente para el rendimiento.
* **Producción**:
  * Excluye los mapas de código fuente.
  * Optimiza el código del lado cliente a través de la agrupación y minificación.

ASP.NET Core usa una variable de entorno denominada `ASPNETCORE_ENVIRONMENT` para almacenar el modo de configuración. Para obtener más información, consulte [establecimiento del entorno](xref:fundamentals/environments#set-the-environment).

### <a name="run-with-net-core-cli"></a>Ejecutar con CLI de .NET Core

Restaurar los paquetes de npm y NuGet necesario ejecutando el siguiente comando en la raíz del proyecto:

```dotnetcli
dotnet restore && npm i
```

Compilar y ejecutar la aplicación:

```dotnetcli
dotnet run
```

Se inicia la aplicación en el host local según el [modo de configuración en tiempo de ejecución](#set-the-runtime-configuration-mode). Vaya a `http://localhost:5000` en el explorador muestra la página de inicio.

### <a name="run-with-visual-studio-2017"></a>Ejecutar con Visual Studio 2017

Abra el *.csproj* archivo generado por el [dotnet nuevo](/dotnet/core/tools/dotnet-new) comando. Los paquetes de NuGet y npm necesarios se restauran automáticamente al proyecto abierto. Este proceso de restauración puede tardar varios minutos, y la aplicación está lista para ejecutarse cuando se complete. Haga clic en el botón verde de ejecución o presione `Ctrl + F5`, y el explorador se abre en la página de aterrizaje de la aplicación. La aplicación se ejecuta en localhost según la [modo de configuración en tiempo de ejecución](#set-the-runtime-configuration-mode).

## <a name="test-the-app"></a>Probar la aplicación

Las plantillas de SpaServices están preconfiguradas para ejecutar pruebas del lado cliente mediante [Karma](https://karma-runner.github.io/1.0/index.html) y [Jasmine](https://jasmine.github.io/). Jasmine es un marco de pruebas unitarias popular para JavaScript, mientras que Karma es un ejecutor de esas pruebas. Karma está configurado para funcionar con [Webpack Dev Middleware](#webpack-dev-middleware), de forma que el desarrollador no tenga que parar y ejecutar la prueba cada vez que se realicen cambios. Tanto si se ejecuta el código en el caso de prueba como si se trata del propio caso de prueba, la prueba se ejecuta automáticamente.

Con la aplicación Angular como ejemplo, dos casos de prueba Jasmine ya se proporcionan para el `CounterComponent` en el *counter.component.spec.ts* archivo:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

Abra el símbolo del sistema en el *ClientApp* directory. Ejecute el siguiente comando:

```console
npm test
```

El script inicia el ejecutor de pruebas Karma, que lee la configuración definida en el *karma.conf.js* archivo. Entre otras opciones, el *karma.conf.js* identifica los archivos de prueba que se ejecuta a través de su `files` matriz:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

## <a name="publish-the-app"></a>Publicar la aplicación

Consulte [este problema de github](https://github.com/aspnet/AspNetCore.Docs/issues/12474) para obtener más información sobre la publicación en Azure.

Combinación de los recursos del lado cliente generados y los artefactos de ASP.NET Core publicados en un paquete listo para implementar puede resultar tedioso. Afortunadamente, SpaServices organiza ese proceso de publicación completa con un destino de MSBuild personalizado denominado `RunWebpack`:

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

El destino de MSBuild tiene las siguientes responsabilidades:

1. Restaure los paquetes de NPM.
1. Cree una compilación de nivel de producción de los recursos del lado cliente de terceros.
1. Cree una compilación de nivel de producción de los recursos personalizados del lado cliente.
1. Copie los recursos generados por WebPack en la carpeta de publicación.

El destino de MSBuild se invoca cuando se ejecuta:

```dotnetcli
dotnet publish -c Release
```

## <a name="additional-resources"></a>Recursos adicionales

* [Docs angulares](https://angular.io/docs)
