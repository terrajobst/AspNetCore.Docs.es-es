---
title: Use la plantilla de proyecto Angular
author: SteveSandersonMS
description: "Obtenga información acerca de cómo empezar a trabajar con la plantilla de proyecto de aplicación de página principal de ASP.NET (SPA) versión candidata para Angular y la CLI Angular."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/06/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/angular
ms.openlocfilehash: 4162b1c26e9d278c811f691c4277d4de25adb204
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="use-the-angular-project-template-release-candidate"></a>Utilice la plantilla de proyecto Angular (candidato de versión comercial)

> [!NOTE]
> Esta documentación no consiste en la plantilla de proyecto Angular publicadas. **Esta documentación está sobre el candidato de versión de la plantilla Angular.** Esperamos que enviar la versión de lanzamiento en 2018 temprano.

La plantilla de proyecto Angular actualizado proporciona un punto de partida cómodo para ASP.NET Core aplicaciones con 5 Angular y la CLI Angular para implementar una interfaz de usuario de cliente enriquecido (UI).

La plantilla es equivalente a crear un proyecto de ASP.NET Core para actuar como un API de back-end y un proyecto de CLI Angular para actuar como una interfaz de usuario. La plantilla ofrece la ventaja de alojar ambos tipos de proyecto en un proyecto de aplicación único. Por lo tanto, el proyecto de aplicación puede ser creado y publicado como una sola unidad.

## <a name="create-a-new-app"></a>Crear una nueva aplicación

Para empezar, asegúrese de que haya [instalar la plantilla de proyecto Angular actualizado](xref:spa/index#installation). Estas instrucciones no son aplicables a la plantilla de proyecto Angular anterior incluida en el núcleo de .NET SDK 2.0. x.

Crear un nuevo proyecto desde un símbolo del sistema mediante el comando `dotnet new angular` en un directorio vacío. Por ejemplo, los siguientes comandos crean la aplicación en un *mi aplicación nuevo* directorio y cambie a ese directorio:

```console
dotnet new angular -o my-new-app
cd my-new-app
```

Ejecute la aplicación desde Visual Studio o en el núcleo de .NET CLI:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Abra el archivo *.csproj* archivo y ejecutar la aplicación como normal desde allí.

El proceso de compilación restaura npm dependencias en la primera ejecución, lo que puede tardar varios minutos. Las compilaciones posteriores son mucho más rápidas.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Asegúrese de que tiene una variable de entorno denominada `ASPNETCORE_Environment` con un valor de `Development`. En Windows (en PowerShell no solicita), ejecute `SET ASPNETCORE_Environment=Development`. En Linux o Mac OS, ejecute `export ASPNETCORE_Environment=Development`.

Ejecutar `dotnet build` para comprobar la aplicación se compila correctamente. En la primera ejecución, el proceso de compilación restaura las dependencias de npm, que pueden tardar varios minutos. Las compilaciones posteriores son mucho más rápidas.

Ejecute `dotnet run` para iniciar la aplicación. Se registra un mensaje similar al siguiente:

```console
Now listening on: http://localhost:<port>
```

Vaya a esta dirección URL en un explorador.

La aplicación se inicia una instancia del servidor de CLI Angular en segundo plano. Se registra un mensaje similar al siguiente: *NG Live desarrollo Server está escuchando en localhost:&lt;otherport&gt;, abra el explorador en http://localhost:&lt;otherport&gt; /* . Pasar por alto este mensaje&mdash;tiene **no** la dirección URL de la aplicación de ASP.NET Core y la CLI Angular combinada.

---

La plantilla de proyecto crea una aplicación de ASP.NET Core y una aplicación Angular. La aplicación de ASP.NET Core está pensada para usarse para el acceso a datos, la autorización y otros problemas relativos a servidor. La aplicación Angular, que reside en el *ClientApp* subdirectorio, está diseñada para utilizarse para todos los problemas de la interfaz de usuario.

## <a name="add-pages-images-styles-modules-etc"></a>Agregar páginas, imágenes, estilos, módulos, etcetera.

El *ClientApp* directorio contiene una aplicación de CLI Angular estándar. Vea el oficial [documentación Angular](https://github.com/angular/angular-cli/wiki) para obtener más información.

Existen pequeñas diferencias entre la aplicación Angular creadas con esta plantilla y creado CLI Angular (a través de `ng new`); sin embargo, las capacidades de la aplicación no se modifican. La aplicación creada por la plantilla contiene un [arranque](https://getbootstrap.com/)-según el diseño y ver un ejemplo de enrutamiento básico.

## <a name="run-ng-commands"></a>Ejecutar comandos ng

En un símbolo del sistema, cambie a la *ClientApp* subdirectorio:

```console
cd ClientApp
```

Si tiene la `ng` herramienta instalada global, puede ejecutar cualquiera de sus comandos. Por ejemplo, puede ejecutar `ng lint`, `ng test`, o cualquiera de los demás [comandos de CLI Angular](https://github.com/angular/angular-cli/wiki#additional-commands). No es necesario para ejecutar `ng serve` sin embargo, dado que la aplicación de ASP.NET Core ocupa que sirve al servidor y de cliente partes de la aplicación. Internamente, utiliza `ng serve` en desarrollo.

Si no tiene la `ng` herramienta instalada, ejecute `npm run ng` en su lugar. Por ejemplo, puede ejecutar `npm run ng lint` o `npm run ng test`.

## <a name="install-npm-packages"></a>Instalar paquetes de npm

Para instalar paquetes de npm de otro fabricante, utilice un símbolo del sistema en el *ClientApp* subdirectorio. Por ejemplo:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>Publicación e implementación

En el desarrollo, la aplicación se ejecuta en modo optimizado para comodidad del desarrollador. Por ejemplo, agrupaciones de JavaScript contienen asignaciones de origen (de modo que durante la depuración, puede ver el código original de TypeScript). La aplicación inspecciona los cambios de archivo de TypeScript, HTML y CSS en el disco y vuelve a compilar automáticamente y vuelve a cargar cuando ve cambiar esos archivos.

En producción, servir una versión de la aplicación que está optimizada para el rendimiento. Esto se configura para que se producen automáticamente. Cuando se publica, la configuración de compilación emite una reducida, de anticipado (AoT) compila compilación del código de cliente. A diferencia de la compilación de desarrollo, la compilación de producción no requiere Node.js esté instalado en el servidor (salvo que haya habilitado [procesamiento previo de servidor](#server-side-rendering)).

Puede usar estándar [métodos de implementación y hospedaje de ASP.NET Core](xref:host-and-deploy/index).

## <a name="run-ng-serve-independently"></a>Ejecutar de forma independiente "ng Server"

El proyecto está configurado para iniciar su propia instancia del servidor de CLI Angular en segundo plano cuando la aplicación ASP.NET Core se inicia en modo de desarrollo. Esto resulta útil porque no tiene que ejecutar manualmente un servidor independiente.

Hay un inconveniente de este programa de instalación de forma predeterminada. Cada vez que modifique el código de C# y su aplicación necesita que se reinicie, de ASP.NET Core se reinicia el servidor de CLI Angular. Se requiere alrededor de 10 segundos para iniciar copia de seguridad. Si está realizando frecuentes modificaciones del código de C# y no desea esperar a CLI Angular reiniciarlo, ejecute la CLI Angular servidor externamente, independientemente del proceso de ASP.NET Core. Para ello:

1. En un símbolo del sistema, cambie a la *ClientApp* subdirectorio e inicie el servidor de desarrollo de CLI Angular:

    ```console
    cd ClientApp
    npm start
    ```

    > [!IMPORTANT]
    > Use `npm start` para iniciar el servidor de desarrollo de CLI Angular, no `ng serve`, de modo que la configuración de *package.json* se respeta. Para pasar parámetros adicionales al servidor CLI Angular, agréguelos a la correspondiente `scripts` de línea en su *package.json* archivo.

2. Modificar la aplicación de ASP.NET Core para usar la instancia de CLI Angular externo en lugar de iniciar una de sus propias. En su *inicio* clase, reemplace el `spa.UseAngularCliServer` invocación con lo siguiente:

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:4200");
    ```

Al iniciar la aplicación de ASP.NET Core, no ejecutar un servidor de CLI Angular. La instancia que iniciarse de forma manual se usa en su lugar. Esto le permite iniciar y reiniciar con mayor rapidez. Ya no está esperando Angular CLI para volver a generar la aplicación de cliente cada vez.

## <a name="server-side-rendering"></a>Representación del lado servidor

Como una característica de rendimiento, puede elegir para un procesamiento previo de la aplicación Angular en el servidor, así como la ejecución en el cliente. Esto significa que los exploradores reciban marcado HTML que representa la interfaz de usuario inicial de la aplicación, por lo que muestra incluso antes de descargar y ejecutar los paquetes de JavaScript. La mayoría de la implementación de este provienen de una característica Angular denominada [Angular Universal](https://universal.angular.io/).

> [!TIP]
> Habilitar la representación del lado servidor (SSR) presenta a una serie de complicaciones adicionales tanto durante el desarrollo e implementación. Lectura [desventajas del SSR](#drawbacks-of-ssr) para determinar si SSR es una buena opción para sus requisitos.

Para habilitar SSR, debe realizar varias adiciones al proyecto.

En el *inicio* (clase), *después* la línea que configura `spa.Options.SourcePath`, y *antes de* la llamada a `UseAngularCliServer` o `UseProxyToSpaDevelopmentServer`, agregue lo siguiente:

[!code-csharp[](sample/AngularServerSideRendering/Startup.cs?name=snippet_Call_UseSpa&highlight=5-12)]

En el modo de desarrollo, este código intentará crear el paquete SSR mediante la ejecución de la secuencia de comandos `build:ssr`, que se define en *ClientApp\package.json*. Esto genera una aplicación Angular denominada `ssr`, que aún no se ha definido. 

Al final de la `apps` la matriz en *ClientApp/.angular-cli.json*, definir una aplicación adicional con el nombre `ssr`. Utilice las siguientes opciones:

[!code-json[](sample/AngularServerSideRendering/ClientApp/.angular-cli.json?range=24-41)]

Esta nueva configuración de la aplicación habilitada para SSR requiere dos archivos más: *tsconfig.server.json* y *main.server.ts*. El *tsconfig.server.json* archivo especifica las opciones de compilación de TypeScript. El *main.server.ts* archivo actúa como el punto de entrada de código durante SSR.

Agregar un nuevo archivo denominado *tsconfig.server.json* en *ClientApp/src* (junto con las existentes *tsconfig.app.json*), que contiene lo siguiente:

[!code-json[](sample/AngularServerSideRendering/ClientApp/src/tsconfig.server.json)]

Este archivo configura el compilador de AoT del Angular para buscar un módulo denominado `app.server.module`. Agregar esto mediante la creación de un nuevo archivo en *ClientApp/src/app/app.server.module.ts* (junto con las existentes *app.module.ts*) que contiene lo siguiente: 

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/app/app.server.module.ts)]

Este módulo se hereda de su cliente `app.module` y define qué módulos angulares adicionales están disponibles durante SSR.

Recuerde que el nuevo `ssr` entrada en *.angular cli.json* hace referencia a un archivo de punto de entrada denominado *main.server.ts*. Aún no ha agregado ese archivo y ahora es el momento de hacerlo. Crear un nuevo archivo en *ClientApp/src/main.server.ts* (junto con las existentes *main.ts*), que contiene lo siguiente:

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/main.server.ts)]

Código de este archivo es lo que ejecuta ASP.NET Core para cada solicitud, cuando se ejecuta el `UseSpaPrerendering` middleware que ha agregado a la *inicio* clase. Abordan la recepción `params` desde el código de .NET (por ejemplo, la dirección URL que se solicita) y realizar llamadas a las API de SSR angulares para obtener el HTML resultante. 

Estrictamente-general, esto es suficiente para habilitar SSR en modo de desarrollo. Es fundamental realizar un cambio final para que la aplicación funciona correctamente cuando se publica. En main de la aplicación *.csproj* de archivos, establezca la `BuildServerSideRenderer` valor de propiedad `true`:

[!code-xml[](sample/AngularServerSideRendering/AngularServerSideRendering.csproj?name=snippet_EnableBuildServerSideRenderer)]

Esto configura el proceso de compilación para ejecutar `build:ssr` durante la publicación e implementar los archivos SSR en el servidor. Si no habilita esta, SSR produce un error en producción.

Cuando la aplicación se ejecuta en modo de desarrollo o de producción, el código Angular previamente representa como HTML en el servidor. El código de cliente se ejecuta con normalidad.

### <a name="pass-data-from-net-code-into-typescript-code"></a>Pasar datos desde el código de .NET en código TypeScript

Durante la SSR, puede pasar datos de cada solicitud de la aplicación de ASP.NET Core en su aplicación Angular. Por ejemplo, podría pasar información de la cookie o algo lee de una base de datos. Para ello, edite la *inicio* clase. En la devolución de llamada para `UseSpaPrerendering`, establezca un valor para `options.SupplyData` como el siguiente:

```csharp
options.SupplyData = (context, data) =>
{
    // Creates a new value called isHttpsRequest that's passed to TypeScript code
    data["isHttpsRequest"] = context.Request.IsHttps;
};
```

El `SupplyData` permite de devolución de llamada se pasan arbitrarios, cada solicitud, datos JSON serializable (por ejemplo, cadenas, valores booleanos o números). Su *main.server.ts* código recibe como `params.data`. Por ejemplo, el ejemplo de código anterior pasa un valor booleano como `params.data.isHttpsRequest` en el `createServerRenderer` devolución de llamada. Se puede pasar a otras partes de la aplicación en cualquier modo admitido por Angular. Por ejemplo, vea cómo *main.server.ts* pasa el `BASE_URL` valor a cualquier componente cuyo constructor se declara para recibirlo.

### <a name="drawbacks-of-ssr"></a>Desventajas de SSR

No todas las aplicaciones se benefician SSR. La principal ventaja es rendimiento percibida. Los visitantes que llegan a la aplicación mediante una conexión de red lenta o en los dispositivos móviles lenta verán la interfaz de usuario inicial rápidamente, incluso si se tarda un tiempo para capturar o analizar las agrupaciones de JavaScript. Sin embargo, muchos SPAs se utilizan principalmente a través de redes de empresa rápida, interno en equipos rápidos donde la aplicación aparece casi al instante.

Al mismo tiempo, hay unas desventajas importantes para habilitar SSR. Agrega complejidad al proceso de desarrollo. El código debe ejecutarse en dos entornos diferentes: de cliente y servidor (en un entorno de Node.js se invoca desde ASP.NET Core). Hay algunas cuestiones a tener en cuenta:

* SSR requiere una instalación de Node.js en los servidores de producción. Esto sucede automáticamente para algunos escenarios de implementación, como servicios de aplicaciones de Azure, pero no para otros, como Azure Service Fabric.
* Habilitar la `BuildServerSideRenderer` crear causas de marca su *node_modules* directory para publicar. Esta carpeta contiene archivos de 20.000, lo que aumenta el tiempo de implementación.
* Para ejecutar el código en un entorno de Node.js, no puede confiar en la existencia de explorador JavaScript APIs específicas como `window` o `localStorage`. Si el código (o alguna biblioteca de terceros que hacen referencia a ella) intenta utilizar estas API, obtendrá un error durante la SSR. Por ejemplo, no use jQuery porque hace referencia a las API específicas del explorador en muchos lugares. Para evitar errores, debe evitar SSR o evitar API o bibliotecas de explorador específico. Puede ajustar las llamadas a estas API en comprobaciones para asegurarse de que no se invoquen durante SSR. Por ejemplo, use una comprobación como la siguiente en el código de JavaScript o TypeScript:

    ```javascript
    if (typeof window !== 'undefined') {
        // Call browser-specific APIs here
    }
    ```
