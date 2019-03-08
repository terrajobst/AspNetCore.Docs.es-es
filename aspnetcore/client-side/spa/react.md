---
title: Uso de la plantilla de proyecto de React con ASP.NET Core
author: SteveSandersonMS
description: Aprenda cómo comenzar a trabajar con la plantilla de proyecto de aplicación de página única (SPA) de ASP.NET Core para React y create-react-app.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 03/07/2019
uid: spa/react
ms.openlocfilehash: 7b07f696ae071531c88c9f8cf528fc6f0bf97b46
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665566"
---
# <a name="use-the-react-project-template-with-aspnet-core"></a>Uso de la plantilla de proyecto de React con ASP.NET Core

La plantilla de proyecto actualizada de React ofrece un práctico punto de partida para las aplicaciones ASP.NET Core que usan React y las convenciones [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) para implementar una completa interfaz de usuario (UI) en el lado cliente.

La plantilla es equivalente a crear un proyecto de ASP.NET Core para que funcione como un back-end de API y un proyecto de React de CRA estándar para que funcione como interfaz de usuario, pero con la comodidad de hospedar ambos en un único proyecto de aplicación que se puede compilar y publicar como una sola unidad.

## <a name="create-a-new-app"></a>Creación de una nueva aplicación

Si tiene ASP.NET Core 2.1 instalado, no es necesario instalar la plantilla de proyecto de React.

En un símbolo del sistema, cree un nuevo proyecto con el comando `dotnet new react` en un directorio vacío. Por ejemplo, los siguientes comandos crean la aplicación en un directorio *my-new-app* y cambian a ese directorio:

```console
dotnet new react -o my-new-app
cd my-new-app
```

Ejecute la aplicación desde Visual Studio o la CLI de .NET Core:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Abra el archivo *.csproj* generado y, desde ahí, ejecute la aplicación de la manera habitual.

El proceso de compilación restaura las dependencias npm en la primera ejecución, lo que puede tardar varios minutos. Las compilaciones posteriores son mucho más rápidas.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Asegúrese de tener una variable de entorno denominada `ASPNETCORE_Environment` con un valor de `Development`. En Windows (en los avisos que no son de PowerShell), ejecute `SET ASPNETCORE_Environment=Development`. En Linux o macOS, ejecute `export ASPNETCORE_Environment=Development`.

Ejecute [dotnet build](/dotnet/core/tools/dotnet-build) para comprobar que la aplicación se compila correctamente. En la primera ejecución, el proceso de compilación restaura las dependencias de npm, lo que puede tardar varios minutos. Las compilaciones posteriores son mucho más rápidas.

Ejecute [dotnet run](/dotnet/core/tools/dotnet-run) para iniciar la aplicación.

---

La plantilla de proyecto crea una aplicación ASP.NET Core y una aplicación de React. El uso previsto de la aplicación ASP.NET Core es el acceso a los datos, la autorización y otros problemas relativos al servidor. La aplicación de React, que reside en el subdirectorio *ClientApp*, está diseñada para utilizarse para su uso con todos los problemas de la interfaz de usuario.

## <a name="add-pages-images-styles-modules-etc"></a>Adición de páginas, imágenes, estilos, módulos, etc.

El directorio *ClientApp* es una aplicación de React de CRA estándar. Para más información, consulte la [documentación oficial de CRA](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md).

Existe pequeñas diferencias entre la aplicación de React creada mediante este plantilla y la creada mediante CRA propiamente dicho; sin embargo, las funcionalidades de la aplicación permanecen sin cambios. La aplicación creada con la plantilla contiene un diseño basado en [arranque](https://getbootstrap.com/) y un ejemplo de enrutamiento básico.

## <a name="install-npm-packages"></a>Instalar paquetes de npm

Para instalar paquetes de npm de otro fabricante, use un símbolo del sistema en el subdirectorio *ClientApp*. Por ejemplo:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>Publicación e implementación

En el desarrollo, la aplicación se ejecuta en modo optimizado para comodidad del desarrollador. Por ejemplo, agrupaciones de JavaScript contienen asignaciones de origen (de modo que durante la depuración, puede ver el código fuente original). La aplicación inspecciona los cambios en los archivos de JavaScript, HTML y CSS en el disco y, automáticamente, realiza una nueva compilación y recarga cuando observa que esos archivos han cambiado.

En producción, use una versión de la aplicación que esté optimizada para el rendimiento. Esto se configura para que tenga lugar automáticamente. Al publicar, la configuración de compilación emite una compilación transpilada reducida del código de cliente. A diferencia de la compilación de desarrollo, la compilación de producción no requiere la instalación de Node.js en el servidor.

Puede usar [métodos de implementación y hospedaje de ASP.NET Core](xref:host-and-deploy/index) estándar.

## <a name="run-the-cra-server-independently"></a>Ejecución del servidor de CRA de forma independiente

El proyecto está configurado para iniciar su propia instancia del servidor de desarrollo de CRA en segundo plano cuando la aplicación ASP.NET Core se inicia en modo de desarrollo. Esto resulta útil porque significa que no tiene que ejecutar manualmente un servidor independiente.

Sin embargo, esta configuración predeterminada tiene un inconveniente. Cada vez que modifica el código de C# y la aplicación ASP.NET Core debe reiniciarse, el servidor de CRA se reinicia. Se necesitan unos segundos para iniciar la copia de seguridad. Sin realiza frecuentes modificaciones en el código de C# y no quiere esperar a que se reinicie el servidor de CRA, ejecute el servidor de CRA externamente, con independencia del proceso de ASP.NET Core. Para ello:

1. Agregar un *.env* del archivo a la *ClientApp* subdirectorio con la siguiente configuración:

    ```
    BROWSER=none
    ```
    
    Esto impedirá que el explorador web al abrir al iniciar el servidor CRA externamente.

2. En un símbolo del sistema, cambie al subdirectorio *ClientApp* e inicie el servidor de desarrollo de CRA:

    ```console
    cd ClientApp
    npm start
    ```

3. Modifique la aplicación ASP.NET Core para usar la instancia del servidor de CRA externo en lugar de iniciar una de las suyas. En la clase *Startup*, reemplace la invocación de `spa.UseReactDevelopmentServer` por lo siguiente:

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

Cuando inicie la aplicación ASP.NET Core, no se iniciará un servidor de CRA. En su lugar, se usa la instancia que inició manualmente. Esto le permite iniciar y reiniciar con mayor rapidez. Ya no tiene que esperar a que la aplicación de React se recompile de una vez a otra.

> [!IMPORTANT]
> "Representación del lado servidor" no es una característica compatible con esta plantilla. Nuestro objetivo con esta plantilla es satisfacer la paridad con "create de react-app". Por lo tanto, escenarios y características no incluidas en un proyecto de "creación de react-app" (por ejemplo, el SSR) no se admiten y se dejan como un ejercicio para el usuario.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:security/authentication/identity/spa>
