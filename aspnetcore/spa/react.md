---
title: Utilizar la plantilla de proyecto de reaccionar con ASP.NET Core
author: SteveSandersonMS
description: Obtenga información acerca de cómo empezar a trabajar con la plantilla de proyecto de aplicación de página única (SPA) de ASP.NET Core para reaccionar y aplicación reaccionar crear.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react
ms.openlocfilehash: 4dcfef2bbb99873a9d716a4942f39123944f495c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
---
# <a name="use-the-react-project-template-with-aspnet-core"></a>Utilizar la plantilla de proyecto de reaccionar con ASP.NET Core

> [!NOTE]
> Acerca de la plantilla de proyecto de reaccionar en esta documentación no está incluida en ASP.NET 2.0 de núcleo. Se trata de la plantilla de reaccionar más reciente en la que puede actualizar manualmente. La plantilla se incluye en ASP.NET Core 2.1 de forma predeterminada.

La plantilla de proyecto de reaccionar actualizada proporciona un punto de partida cómodo para ASP.NET Core aplicaciones con reaccionar y [aplicación reaccionar crear](https://github.com/facebookincubator/create-react-app) convenciones (CRA) para implementar una interfaz de usuario de cliente enriquecido (UI).

La plantilla es equivalente a crear un proyecto de ASP.NET Core para actuar como un API de back-end y un proyecto de reaccionar CRA estándar para actuar como una interfaz de usuario, pero con la comodidad de hospedarlos en un proyecto de aplicación único que se puedan generar y publicar como una sola unidad.

## <a name="create-a-new-app"></a>Crear una nueva aplicación

Si usa ASP.NET Core 2.0, asegúrese de que se haya [instalar la plantilla de proyecto de reaccionar actualizada](xref:spa/index#installation). Si tiene ASP.NET Core 2.1, no es necesario instalarlo.

Crear un nuevo proyecto desde un símbolo del sistema mediante el comando `dotnet new react` en un directorio vacío. Por ejemplo, los siguientes comandos crean la aplicación en un *mi aplicación nuevo* directorio y cambie a ese directorio:

```console
dotnet new react -o my-new-app
cd my-new-app
```

Ejecute la aplicación desde Visual Studio o en el núcleo de .NET CLI:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Abra el archivo *.csproj* archivo y ejecutar la aplicación como normal desde allí.

El proceso de compilación restaura npm dependencias en la primera ejecución, lo que puede tardar varios minutos. Las compilaciones posteriores son mucho más rápidas.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Asegúrese de que tiene una variable de entorno denominada `ASPNETCORE_Environment` con el valor de `Development`. En Windows (en PowerShell no solicita), ejecute `SET ASPNETCORE_Environment=Development`. En Linux o Mac OS, ejecute `export ASPNETCORE_Environment=Development`.

Ejecutar [dotnet compilación](/dotnet/core/tools/dotnet-build) para comprobar la aplicación se compila correctamente. En la primera ejecución, el proceso de compilación restaura las dependencias de npm, que pueden tardar varios minutos. Las compilaciones posteriores son mucho más rápidas.

Ejecutar [dotnet ejecutar](/dotnet/core/tools/dotnet-run) para iniciar la aplicación.

---

La plantilla de proyecto crea una aplicación de ASP.NET Core y una aplicación de reaccionar. La aplicación de ASP.NET Core está pensada para usarse para el acceso a datos, la autorización y otros problemas relativos a servidor. La aplicación reaccionar, que reside en el *ClientApp* subdirectorio, está diseñada para utilizarse para todos los problemas de la interfaz de usuario.

## <a name="add-pages-images-styles-modules-etc"></a>Agregar páginas, imágenes, estilos, módulos, etcetera.

El *ClientApp* directorio es una aplicación de reaccionar CRA estándar. Vea el oficial [documentación CRA](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) para obtener más información.

Existen pequeñas diferencias entre la aplicación de reaccionar creados por esta plantilla y creado por CRA sí; Sin embargo, no se modifican las capacidades de la aplicación. La aplicación creada por la plantilla contiene un [arranque](https://getbootstrap.com/)-según el diseño y ver un ejemplo de enrutamiento básico.

## <a name="install-npm-packages"></a>Instalar paquetes de npm

Para instalar paquetes de npm de otro fabricante, utilice un símbolo del sistema en el *ClientApp* subdirectorio. Por ejemplo:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>Publicación e implementación

En el desarrollo, la aplicación se ejecuta en modo optimizado para comodidad del desarrollador. Por ejemplo, agrupaciones de JavaScript contienen asignaciones de origen (de modo que durante la depuración, puede ver el código fuente original). La aplicación supervisa JavaScript, HTML y CSS cambios de archivo en el disco y vuelve a compilar automáticamente y vuelve a cargar cuando ve cambiar esos archivos.

En producción, servir una versión de la aplicación que está optimizada para el rendimiento. Esto se configura para que se producen automáticamente. Cuando se publica, la configuración de compilación emite una compilación transpiled reducida, del código de cliente. A diferencia de la compilación de desarrollo, la compilación de producción no requiere Node.js esté instalado en el servidor.

Puede usar estándar [métodos de implementación y hospedaje de ASP.NET Core](xref:host-and-deploy/index).

## <a name="run-the-cra-server-independently"></a>Ejecutar el servidor CRA independientemente

El proyecto está configurado para iniciar su propia instancia del servidor de desarrollo CRA en segundo plano cuando la aplicación ASP.NET Core se inicia en modo de desarrollo. Esto resulta útil porque significa que no tiene que ejecutar manualmente un servidor independiente.

Hay un inconveniente de este programa de instalación de forma predeterminada. Cada vez que modifique el código de C# y su aplicación necesita que se reinicie, de ASP.NET Core se reinicia el servidor CRA. Unos pocos segundos son necesarios para iniciar copia de seguridad. Si está realizando frecuentes modificaciones del código de C# y no desea esperar a que se reinicie el servidor CRA, ejecute el servidor CRA externamente, independientemente del proceso de ASP.NET Core. Para ello:

1. En un símbolo del sistema, cambie a la *ClientApp* subdirectorio e inicie el servidor de desarrollo de CRA:

    ```console
    cd ClientApp
    npm start
    ```

2. Modificar la aplicación de ASP.NET Core para usar la instancia del servidor CRA externa en lugar de iniciar una propia. En su *inicio* clase, reemplace el `spa.UseReactDevelopmentServer` invocación con lo siguiente:

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

Al iniciar la aplicación de ASP.NET Core, no ejecutar un servidor CRA. La instancia que iniciarse de forma manual se usa en su lugar. Esto le permite iniciar y reiniciar con mayor rapidez. Ya no está esperando la aplicación reaccionar a generar cada vez.
