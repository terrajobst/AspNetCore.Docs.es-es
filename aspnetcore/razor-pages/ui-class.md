---
title: Interfaz de usuario de Razor reutilizable en bibliotecas de clases con ASP.NET Core
author: Rick-Anderson
description: Se explica cómo crear una interfaz de usuario de Razor reutilizable en una biblioteca de clases.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/21/2018
uid: razor-pages/ui-class
ms.openlocfilehash: 1f0ef59ce3f3294d6a3bde015ca34800770b1be4
ms.sourcegitcommit: e955a722c05ce2e5e21b4219f7d94fb878e255a6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2018
ms.locfileid: "42909535"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>Cree una interfaz de usuario reutilizable con el proyecto de biblioteca de clases de Razor en ASP.NET Core.

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Las vistas, las páginas, los controladores, los modelos de página, los modelos de datos y los [componentes de vista](xref:mvc/views/view-components) de Razor se pueden integrar en una biblioteca de clases de Razor (RCL). Las RCL se pueden empaquetar y reutilizar. Las aplicaciones pueden incluir la RCL y reemplazar las vistas y páginas que contienen. Cuando existe una vista, vista parcial o página de Razor tanto en la aplicación web como en la RCL, tendrá prioridad el marcado de Razor (archivo *.cshtml*) de la aplicación web.

Esta característica requiere [!INCLUDE[](~/includes/2.1-SDK.md)]

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>Crear una biblioteca de clases que contenga interfaz de usuario de Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.
* Seleccione **Aplicación web de ASP.NET Core**.
* Asigne un nombre a la biblioteca (por ejemplo, "RazorClassLib") > **Aceptar**. Para evitar un conflicto de nombres de archivo con la biblioteca de vistas generada, asegúrese de que el nombre de la biblioteca no acaba en `.Views`.
* Confirme que **ASP.NET Core 2.1** o una versión posterior está seleccionado.
* Seleccione **Razor Class Library** (Biblioteca de clases de Razor) > **Aceptar**.

Una biblioteca de clases de Razor tiene el siguiente archivo del proyecto:

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Ejecute `dotnet new razorclasslib` desde la línea de comandos. Por ejemplo:

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

Para más información, vea [dotnet new](/dotnet/core/tools/dotnet-new). Para evitar un conflicto de nombres de archivo con la biblioteca de vistas generada, asegúrese de que el nombre de la biblioteca no acaba en `.Views`.

------
Agregue archivos de Razor a la RCL.

Las plantillas de ASP.NET Core se suponen que el contenido RCL está en el *áreas* carpeta. Consulte [diseño de páginas RCL](#afs) para crear contenido en un RCL que expone `~/Pages` lugar `~/Areas/Pages`.

## <a name="referencing-razor-class-library-content"></a>Hacer referencia a contenido de la biblioteca de clases de Razor

Los siguientes elementos pueden hacer referencia a la RCL:

* Paquete NuGet. Vea [Creación de paquetes NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) y [Creación y publicación de un paquete NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* *{NombreDeProyecto}.csproj*. Vea [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a>Tutorial: Crear un proyecto de biblioteca de clases de Razor y usar desde un proyecto de páginas de Razor

En lugar de crearlo, puede descargar el [proyecto completo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) y comprobarlo. La descarga de ejemplo contiene más código y vínculos que hacen que el proyecto sea fácil de comprobar. Puede dejar sus comentarios en [este problema de GitHub](https://github.com/aspnet/Docs/issues/6098) sobre las descargas de ejemplo en comparación con las instrucciones paso a paso.

### <a name="test-the-download-app"></a>Comprobar la aplicación de descarga

Si no ha descargado la aplicación final y prefiere crear el proyecto de tutorial, omita este paso y vaya a la [siguiente sección](#create-a-razor-class-library).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Abra el archivo *.sln* en Visual Studio. Ejecute la aplicación.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Desde un símbolo del sistema en el directorio *cli*, cree la RCL y la aplicación web.

``` CLI
dotnet build
```

Vaya al directorio *WebApp1* y ejecute la aplicación:

``` CLI
dotnet run
```
------

Siga las instrucciones de [Probar WebApp1](#test).

## <a name="create-a-razor-class-library"></a>Crear una biblioteca de clases de Razor

En esta sección, crearemos una biblioteca de clases de Razor (RCL) y se agregarán a ella archivos de Razor.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Cree el proyecto de RCL:

* En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.
* Seleccione **Aplicación web de ASP.NET Core**.
* Nombre de la aplicación **RazorUIClassLib** > **Aceptar**.
* Confirme que **ASP.NET Core 2.1** o una versión posterior está seleccionado.
* Seleccione **Razor Class Library** (Biblioteca de clases de Razor) > **Aceptar**.
* Agregue un archivo de vista parcial de Razor denominado *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Ejecute lo siguiente desde la línea de comandos:

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

Los comandos anteriores:

* Crean la biblioteca de clases de Razor (RCL) `RazorUIClassLib`.
* Crean una página _Message de Razor y la agrega a la RCL. El parámetro `-np` crea la página sin un `PageModel`.
* Crean un archivo [viewstart](xref:mvc/views/layout#running-code-before-each-view) y lo agregan a la RCL.

El archivo viewstart es necesario para poder usar el diseño del proyecto de páginas de Razor (que agregaremos en la siguiente sección).

------

### <a name="add-razor-files-and-folders-to-the-project"></a>Agregue archivos y carpetas de Razor al proyecto.

* Reemplace el marcado de *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* por el siguiente código:

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* Reemplace el marcado de *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* por el siguiente código:

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

Se necesita `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` para usar la vista parcial (`<partial name="_Message" />`). En lugar de incluir la directiva `@addTagHelper`, puede agregar un archivo *_ViewImports.cshtml*. Por ejemplo:

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

Para más información sobre viewimports, vea [Importar directivas compartidas](xref:mvc/views/layout#importing-shared-directives).

* Compile la biblioteca de clases para confirmar que no hay ningún error de compilador:

``` CLI
dotnet build RazorUIClassLib
```

El resultado de la compilación contiene *RazorUIClassLib.dll* y *RazorUIClassLib.Views.dll*. *RazorUIClassLib.Views.dll* incluye el contenido de Razor compilado.

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a>Usar la biblioteca de interfaz de usuario de Razor desde un proyecto de páginas de Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Cree la aplicación web de páginas de Razor:

* En el **Explorador de soluciones**, haga clic con el botón derecho en la solución > **Agregar** >  **Nuevo proyecto**.
* Seleccione **Aplicación web de ASP.NET Core**.
* Denomine la aplicación **WebApp1**.
* Confirme que **ASP.NET Core 2.1** o una versión posterior está seleccionado.
* Seleccione **Aplicación web** > **Aceptar**.

* En el **Explorador de soluciones**, haga clic con el botón derecho en **WebApp1** y seleccione **Establecer como proyecto de inicio**.
* En el **Explorador de soluciones**, haga clic con el botón derecho en **WebApp1** y seleccione **Dependencias de compilación** > **Dependencias del proyecto**.
* Marque **RazorUIClassLib** como dependencia de **WebApp1**.
* En el **Explorador de soluciones**, haga clic con el botón derecho en **WebApp1** y seleccione **Agregar** > **Referencia**.
* En el cuadro de diálogo **Administrador de referencias**, marque **RazorUIClassLib** > **Aceptar**.

Ejecute la aplicación.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Cree una aplicación web de páginas de Razor y un archivo de solución que contenga dicha aplicación y la biblioteca de clases de Razor:

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

Compile y ejecute la aplicación web:

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a>Probar WebApp1

Compruebe si la biblioteca de clases de interfaz de usuario de Razor se está usando.

* Vaya a `/MyFeature/Page1`.

## <a name="override-views-partial-views-and-pages"></a>Reemplazar vistas, vistas parciales y páginas

Cuando existe una vista, vista parcial o página de Razor tanto en la aplicación web como en la biblioteca de clases de Razor, tendrá prioridad el marcado de Razor (archivo *.cshtml*) de la aplicación web. Por ejemplo, si agrega *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* a WebApp1, Page1 en WebApp1 prevalecerá sobre Page1 en la biblioteca de clases de Razor.

En la descarga de ejemplo, cambie el nombre *WebApp1/Areas/MyFeature2* por *WebApp1/Areas/MyFeature* para comprobar la prioridad.

Copie la vista parcial *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* en *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*. Actualice el marcado para señalar la nueva ubicación. Compile y ejecute la aplicación para comprobar si se está usando la versión de la vista parcial de la aplicación.

<a name="afs"></a>

### <a name="rcl-pages-layout"></a>Diseño de páginas RCL

Para hacer referencia a contenido RCL como si formara parte de la carpeta de la aplicación web, cree el proyecto RCL con la siguiente estructura de archivos:

* *Páginas/RazorUIClassLib*
* *RazorUIClassLib/páginas/Shared*

Supongamos que *RazorUIClassLib, compartidos o páginas* contiene dos archivos parciales, *_Header.cshtml* y *_Footer.cshtml*. El <partial> se puede agregar etiquetas a *_Layout.cshtml* archivo: 
  
```
  <body>
    <partial name="_Header">
    @RenderBody()
    <partial name="_Footer">
  </body>
```
