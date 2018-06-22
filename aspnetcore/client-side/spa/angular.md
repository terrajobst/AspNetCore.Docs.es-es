---
title: Uso de la plantilla de proyecto de Angular con ASP.NET Core
author: SteveSandersonMS
description: Aprenda cómo comenzar a trabajar con la plantilla de proyecto de aplicación de página única (SPA) de ASP.NET Core para Angular y la CLI de Angular.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/angular
ms.openlocfilehash: 811e28af2b67c356ff038d8d673e2164bb56578e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291468"
---
# <a name="use-the-angular-project-template-with-aspnet-core"></a>Uso de la plantilla de proyecto de Angular con ASP.NET Core

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> Esta documentación no trata sobre la plantilla de proyecto de Angular incluida en ASP.NET Core 2.0. Trata sobre la nueva plantilla de Angular que puede actualizar manualmente. La plantilla se incluye de forma predeterminada en ASP.NET Core 2.1.

::: moniker-end

La plantilla de proyecto de Angular actualizada proporciona un práctico punto de partida para las aplicaciones ASP.NET Core que usan Angular y la CLI de Angular para implementar una completa interfaz de usuario (UI) del lado cliente.

La plantilla es equivalente a crear un proyecto de ASP.NET Core que funciona como back-end de API y un proyecto de la CLI de Angular que funciona como interfaz de usuario. La plantilla ofrece la ventaja de hospedar ambos tipos de proyecto en un único proyecto de aplicación. Por lo tanto, el proyecto de aplicación se puede compilar y publicar como una sola unidad.

## <a name="create-a-new-app"></a>Creación de una nueva aplicación

::: moniker range="= aspnetcore-2.0"

Si usa ASP.NET Core 2.0, asegúrese de que ha [instalado la plantilla de proyecto actualizada de React](xref:spa/index#installation).

::: moniker-end
::: moniker range=">= aspnetcore-2.1"

Si tiene ASP.NET Core 2.1 instalado, no hay ninguna necesidad de instalar la plantilla de proyecto Angular.

::: moniker-end

En un símbolo del sistema, cree un nuevo proyecto con el comando `dotnet new angular` en un directorio vacío. Por ejemplo, los siguientes comandos crean la aplicación en un directorio *my-new-app* y cambian a ese directorio:

```console
dotnet new angular -o my-new-app
cd my-new-app
```

Ejecute la aplicación desde Visual Studio o la CLI de .NET Core:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

Abra el archivo *.csproj* generado y, desde ahí, ejecute la aplicación de la manera habitual.

El proceso de compilación restaura las dependencias npm en la primera ejecución, lo que puede tardar varios minutos. Las compilaciones posteriores son mucho más rápidas.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli/)

Asegúrese de tener una variable de entorno denominada `ASPNETCORE_Environment` con un valor de `Development`. En Windows (en los avisos que no son de PowerShell), ejecute `SET ASPNETCORE_Environment=Development`. En Linux o macOS, ejecute `export ASPNETCORE_Environment=Development`.

Ejecute [dotnet build](/dotnet/core/tools/dotnet-build) para comprobar que la aplicación se compila correctamente. En la primera ejecución, el proceso de compilación restaura las dependencias de npm, lo que puede tardar varios minutos. Las compilaciones posteriores son mucho más rápidas.

Ejecute [dotnet run](/dotnet/core/tools/dotnet-run) para iniciar la aplicación. Se registra un mensaje similar al siguiente:

```console
Now listening on: http://localhost:<port>
```

Vaya a esta dirección URL en un explorador.

La aplicación inicia en segundo plano una instancia del servidor de la CLI de Angular. Se registra un mensaje similar al siguiente: *El servidor de desarrollo de NG Live escucha en localhost:&lt;otherport&gt;, abra el explorador en http://localhost:&lt;otherport&gt;/*. Omita este mensaje, no **es** la dirección URL de la aplicación combinada de ASP.NET Core y la CLI de Angular.

---

La plantilla de proyecto crea una aplicación ASP.NET Core y una aplicación de Angular. El uso previsto de la aplicación ASP.NET Core es el acceso a los datos, la autorización y otros problemas relativos al servidor. Por otro lado, la aplicación de Angular, que reside en el subdirectorio *ClientApp* está destinada a todos los problemas relacionados con la interfaz de usuario.

## <a name="add-pages-images-styles-modules-etc"></a>Adición de páginas, imágenes, estilos, módulos, etc.

El directorio *ClientApp* contiene una aplicación estándar de la CLI de Angular. Consulte la [documentación de Angular](https://github.com/angular/angular-cli/wiki) para más información.

Existen pequeñas diferencias entre la aplicación de Angular creada con esta plantilla y la creada con la propia CLI de Angular (mediante `ng new`); sin embargo, las funcionalidades de la aplicación permanecen sin cambios. La aplicación creada con la plantilla contiene un diseño basado en [arranque](https://getbootstrap.com/) y un ejemplo de enrutamiento básico.

## <a name="run-ng-commands"></a>Ejecución de comandos ng

En un símbolo del sistema, cambie al subdirectorio *ClientApp*:

```console
cd ClientApp
```

Si tiene instalada la herramienta `ng` globalmente, puede ejecutar cualquiera de sus comandos. Por ejemplo, puede ejecutar `ng lint`, `ng test`, o cualquiera de los otros [comandos de la CLI de Angular](https://github.com/angular/angular-cli/wiki#additional-commands). Si bien, no es necesario ejecutar `ng serve`, dado que la aplicación ASP.NET Core se ocupa de atender las partes del lado cliente y servidor de la aplicación. Internamente, usa `ng serve` en el desarrollo.

Si no tiene instalada la herramienta `ng`, ejecute en su lugar `npm run ng`. Por ejemplo, puede ejecutar `npm run ng lint` o `npm run ng test`.

## <a name="install-npm-packages"></a>Instalar paquetes de npm

Para instalar paquetes de npm de otro fabricante, use un símbolo del sistema en el subdirectorio *ClientApp*. Por ejemplo:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>Publicación e implementación

En el desarrollo, la aplicación se ejecuta en modo optimizado para comodidad del desarrollador. Por ejemplo, las agrupaciones de JavaScript incluyen asignaciones de origen (de modo que, durante la depuración, puede ver el código original de TypeScript). La aplicación inspecciona los cambios en los archivos de TypeScript, HTML y CSS en el disco y, automáticamente, realiza una nueva compilación y recarga cuando observa que esos archivos han cambiado.

En producción, use una versión de la aplicación que esté optimizada para el rendimiento. Esto se configura para que tenga lugar automáticamente. Al publicar, la configuración de compilación emite una compilación Ahead Of Time (AoT) reducida del código del lado cliente. A diferencia de la compilación de desarrollo, la compilación de producción no requiere la instalación de Node.js en el servidor (a no ser que haya habilitado la [representación previa del lado servidor](#server-side-rendering)).

Puede usar [métodos de implementación y hospedaje de ASP.NET Core](xref:host-and-deploy/index) estándar.

## <a name="run-ng-serve-independently"></a>Ejecución de "ng serve" de manera independiente

El proyecto está configurado para iniciar su propia instancia del servidor de CLI de Angular en segundo plano cuando la aplicación ASP.NET Core se inicia en modo de desarrollo. Esto resulta útil porque no tiene que ejecutar manualmente un servidor independiente.

Sin embargo, esta configuración predeterminada tiene un inconveniente. Cada vez que modifica el código de C# y la aplicación ASP.NET Core debe reiniciarse, el servidor de CLI de Angular se reinicia. Se necesitan unos 10 segundos para iniciar la copia de seguridad. Sin realiza frecuentes modificaciones en el código de C# y no quiere esperar a que se reinicie la CLI de Angular, ejecute el servidor de la CLI de Angular externamente, con independencia del proceso de ASP.NET Core. Para ello:

1. En un símbolo del sistema, cambie al subdirectorio *ClientApp* e inicie el servidor de desarrollo de la CLI Angular:

    ```console
    cd ClientApp
    npm start
    ```

    > [!IMPORTANT]
    > Use `npm start` para iniciar el servidor de desarrollo de la CLI de Angular, no `ng serve`, de modo que la configuración de *package.json* se respete. Para pasar parámetros adicionales al servidor de la CLI de Angular, agréguelos a la línea de `scripts` correspondiente de su archivo *package.json*.

2. Modifique la aplicación ASP.NET Core para usar la instancia externa de la CLI de Angular en lugar de iniciar una de las suyas. En la clase *Startup*, reemplace la invocación de `spa.UseAngularCliServer` por lo siguiente:

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:4200");
    ```

Cuando inicie la aplicación ASP.NET Core, no se iniciará un servidor de la CLI de Angular. En su lugar, se usa la instancia que inició manualmente. Esto le permite iniciar y reiniciar con mayor rapidez. Ya no tiene que esperar a que la CLI de Angular vuelva a compilar la aplicación cliente una y otra vez.

## <a name="server-side-rendering"></a>Representación del lado servidor

Como característica de rendimiento, puede elegir representar previamente su aplicación de Angular en el servidor, así como ejecutarla en el cliente. Esto significa que los exploradores reciben marcado HTML que representa la interfaz de usuario inicial de la aplicación, de modo que lo muestran incluso antes de descargar y ejecutar las agrupaciones de JavaScript. La mayor parte de la implementación de esta representación procede de una característica de Angular llamada [Angular Universal](https://universal.angular.io/).

> [!TIP]
> Habilitar la representación del lado servidor (SSR) conlleva una serie de complicaciones adicionales tanto durante el desarrollo como durante la implementación. La las [desventajas de SSR](#drawbacks-of-ssr) para determinar si SSR es una buena opción para sus requisitos.

Para habilitar SSR, deberá realizar varias adiciones al proyecto.

En la clase *Startup*, *después de* la línea que configura `spa.Options.SourcePath`, y *antes de* la llamada a `UseAngularCliServer` o `UseProxyToSpaDevelopmentServer`, agregue el siguiente código:

[!code-csharp[](sample/AngularServerSideRendering/Startup.cs?name=snippet_Call_UseSpa&highlight=5-12)]

En modo de desarrollo, este código intenta compilar la agrupación de SSR mediante la ejecución del script `build:ssr`, que se define en *ClientApp\package.json*. Esta acción compila una aplicación de Angular denominada `ssr`, que aún no se ha definido.

Al final de la matriz `apps` en *ClientApp/.angular-cli.json*, defina una aplicación adicional con el nombre `ssr`. Use las siguientes opciones:

[!code-json[](sample/AngularServerSideRendering/ClientApp/.angular-cli.json?range=24-41)]

Esta nueva configuración de aplicación habilitada para SSR requiere dos archivos más: *tsconfig.server.json* y *main.server.ts*. El archivo *tsconfig.server.json* especifica las opciones de compilación de TypeScript. El archivo *main.server.ts* sirve de punto de entrada de código durante SSR.

Agregue un nuevo archivo llamado *tsconfig.server.json* dentro de *ClientApp/src* (al lado del archivo *tsconfig.app.json* existente), que contenga lo siguiente:

[!code-json[](sample/AngularServerSideRendering/ClientApp/src/tsconfig.server.json)]

Este archivo configura el compilador AoT de Angular para buscar un módulo llamado `app.server.module`. Para agregar el archivo, cree un nuevo archivo en *ClientApp/src/app/app.server.module.ts* (al lado del archivo *app.module.ts* existente), que contenga lo siguiente:

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/app/app.server.module.ts)]

Este módulo hereda del objeto `app.module` del lado cliente y define qué módulos adicionales de Angular están disponibles durante SSR.

Recuerde que la nueva entrada `ssr` en *.angular-cli.json* hacía referencia a un archivo de punto de entrada llamado *main.server.ts*. Ese archivo no se ha agregado aún, y ahora es el momento de hacerlo. Cree un nuevo archivo en *ClientApp/src/main.server.ts* (junto al archivo *main.ts* existente), que contenga lo siguiente:

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/main.server.ts)]

El código de este archivo es lo que ejecuta ASP.NET Core en cada solicitud cuando ejecuta el middleware `UseSpaPrerendering` que agregó a la clase *Startup*. Se encarga de recibir `params` del código de .NET (como la dirección URL que se solicita), y realiza llamadas a las API de SSR de Angular para obtener el HTML resultante.

Estrictamente hablando, esto es suficiente para habilitar SSR en modo de desarrollo. Para que la aplicación funcione correctamente cuando se publica, es fundamental realizar un cambio final. En el archivo *.csproj* principal de la aplicación, establezca el valor de propiedad `BuildServerSideRenderer` en `true`:

[!code-xml[](sample/AngularServerSideRendering/AngularServerSideRendering.csproj?name=snippet_EnableBuildServerSideRenderer)]

Este valor configura el proceso de compilación para ejecutar `build:ssr` durante la publicación e implementar los archivos SSR en el servidor. Si no se habilita, SSR produce un error en producción.

Cuando la aplicación se ejecuta en modo de desarrollo o de producción, el código de Angular se representa previamente como HTML en el servidor. El código del lado cliente se ejecuta con normalidad.

### <a name="pass-data-from-net-code-into-typescript-code"></a>Paso de datos del código de .NET al código de TypeScript

Durante SSR, quizás quiera puede pasar datos por solicitud de la aplicación ASP.NET Core a la aplicación de Angular. Por ejemplo, podría pasar información de cookies o algo que se haya leído de una base de datos. Para ello, edite la clase *Startup*. En la devolución de llamada de `UseSpaPrerendering`, establezca un valor para `options.SupplyData`, como el siguiente:

```csharp
options.SupplyData = (context, data) =>
{
    // Creates a new value called isHttpsRequest that's passed to TypeScript code
    data["isHttpsRequest"] = context.Request.IsHttps;
};
```

La devolución de llamada `SupplyData` permite pasar datos arbitrarios y por solicitud que se pueden serializar con JSON (por ejemplo, cadenas, valores booleanos o números). El código *main.server.ts* la recibe como `params.data`. Por ejemplo, el ejemplo de código anterior pasa un valor booleano como `params.data.isHttpsRequest` a la devolución de llamada `createServerRenderer`. Se puede pasar a otras partes de la aplicación en cualquier modo admitido por Angular. Por ejemplo, vea cómo *main.server.ts* pasa el valor `BASE_URL` a cualquier componente cuyo constructor se declara para recibirlo.

### <a name="drawbacks-of-ssr"></a>Desventajas de SSR

No todas las aplicaciones se benefician de SSR. La principal ventaja es el rendimiento percibido. Los visitantes que llegan a la aplicación a través de una conexión de red lenta o en dispositivos móviles lentos verán la interfaz de usuario inicial rápidamente, incluso si tarda un rato en capturar o analizar las agrupaciones de JavaScript. Sin embargo, muchas SPA se utilizan principalmente a través de redes de empresa internas rápidas en equipos rápidos donde la aplicación aparece casi al instante.

Al mismo tiempo, la habilitación de SSR conlleva importantes desventajas: Agrega complejidad al proceso de desarrollo. El código se debe ejecutar en dos entornos diferentes: cliente y servidor (en un entorno de Node.js se invoca desde ASP.NET Core). Hay algunos aspectos a tener en cuenta:

* SSR requiere la instalación de Node.js en los servidores de producción. En algunos escenarios de implementación esto se produce automáticamente, como Azure App Services, pero no en otros, como Azure Service Fabric.
* Al habilitar la marca de compilación `BuildServerSideRenderer`, el directorio *node_modules* se publica. Esta carpeta contiene más de 20 000 archivos, lo que aumenta el tiempo de implementación.
* Para ejecutar el código en un entorno de Node.js, no puede confiar en la existencia de API de JavaScript específicas del explorador como `window` o `localStorage`. Si el código (o alguna biblioteca de terceros a la que hace referencia) intenta usar estas API, obtendrá un error durante SSR. Por ejemplo, no use jQuery porque hace referencia a API específicas del explorador en muchos lugares. Para evitar errores, no use en la medida de lo posible SSR ni API o bibliotecas específicas del explorador. Puede encapsular las llamadas a estas API en comprobaciones para asegurarse de que no se invoquen durante SSR. Por ejemplo, use una comprobación como la siguiente en código de JavaScript o TypeScript:

    ```javascript
    if (typeof window !== 'undefined') {
        // Call browser-specific APIs here
    }
    ```
