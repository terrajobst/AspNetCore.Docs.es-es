---
title: Interfaz de usuario de Razor reutilizable en bibliotecas de clases con ASP.NET Core
author: Rick-Anderson
description: Explica cómo crear la interfaz de usuario Razor reutilizable con las vistas parciales en una biblioteca de clases en ASP.NET Core.
ms.author: riande
ms.date: 01/25/2020
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 8813244ea6d00b170d9f95d12743e9fee38bf810
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172647"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>Crear una interfaz de usuario reutilizable mediante el proyecto de biblioteca de clases de Razor en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

Las vistas, páginas, controladores, modelos de páginas, [componentes de Razor](xref:blazor/class-libraries), componentes de [vista](xref:mvc/views/view-components)y modelos de datos de Razor se pueden integrar en una biblioteca de clases de Razor (RCL). Las RCL se pueden empaquetar y reutilizar. Las aplicaciones pueden incluir la RCL y reemplazar las vistas y páginas que contienen. Cuando existe una vista, vista parcial o página de Razor tanto en la aplicación web como en la RCL, tendrá prioridad el marcado de Razor (archivo *.cshtml*) de la aplicación web.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>Crear una biblioteca de clases que contenga interfaz de usuario de Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* En Visual Studio, seleccione **crear nuevo proyecto nuevo**.
* Seleccione **biblioteca de clases de Razor** > **siguiente**.
* Asigne a la biblioteca el nombre (por ejemplo, "RazorClassLib"), > **crear**. Para evitar un conflicto de nombres de archivo con la biblioteca de vistas generada, asegúrese de que el nombre de la biblioteca no acaba en `.Views`.
* Seleccione **páginas y vistas de soporte técnico** si necesita admitir vistas. De forma predeterminada, solo se admiten Razor Pages. Seleccione **Crear**.

De forma predeterminada, la plantilla de la biblioteca de clases de Razor (RCL) utiliza el desarrollo de componentes de Razor. La opción **páginas y vistas de soporte técnico** admite páginas y vistas.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Ejecute `dotnet new razorclasslib` desde la línea de comandos. Por ejemplo:

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

De forma predeterminada, la plantilla de la biblioteca de clases de Razor (RCL) utiliza el desarrollo de componentes de Razor. Pase la opción de `--support-pages-and-views` (`dotnet new razorclasslib --support-pages-and-views`) para proporcionar compatibilidad con páginas y vistas.

Para más información, vea [dotnet new](/dotnet/core/tools/dotnet-new). Para evitar un conflicto de nombres de archivo con la biblioteca de vistas generada, asegúrese de que el nombre de la biblioteca no acaba en `.Views`.

---

Agregue archivos de Razor a la RCL.

Las plantillas ASP.NET Core suponen que el contenido de RCL se encuentra en la carpeta *áreas* . Consulte [diseño de páginas de RCL](#rcl-pages-layout) para crear un RCL que exponga contenido en `~/Pages` en lugar de `~/Areas/Pages`.

## <a name="reference-rcl-content"></a>Contenido de RCL de referencia

Los siguientes elementos pueden hacer referencia a la RCL:

* Paquete NuGet. Vea [Creación de paquetes NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) y [Creación y publicación de un paquete NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* *{NombreDeProyecto}.csproj*. Vea [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).

## <a name="override-views-partial-views-and-pages"></a>Reemplazar vistas, vistas parciales y páginas

Cuando existe una vista, vista parcial o página de Razor tanto en la aplicación web como en la RCL, tendrá prioridad el marcado de Razor (archivo *.cshtml*) de la aplicación web. Por ejemplo, agregue *WebApp1/areas/mi Feature/pages/Page1. cshtml* a WebApp1 y Page1 en WebApp1 tendrá prioridad sobre PÁGINA1 en RCL.

En la descarga de ejemplo, cambie el nombre *WebApp1/Areas/MyFeature2* por *WebApp1/Areas/MyFeature* para comprobar la prioridad.

Copie la vista parcial *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* en *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*. Actualice el marcado para señalar la nueva ubicación. Compile y ejecute la aplicación para comprobar si se está usando la versión de la vista parcial de la aplicación.

### <a name="rcl-pages-layout"></a>Diseño de páginas RCL

Para hacer referencia al contenido de RCL como si formara parte de la carpeta *páginas* de la aplicación Web, cree el proyecto RCL con la siguiente estructura de archivos:

* *RazorUIClassLib/páginas*
* *RazorUIClassLib/páginas/compartido*

Supongamos que *RazorUIClassLib/pages/Shared* contiene dos archivos parciales: *_Header. cshtml* y *_Footer. cshtml*. Las etiquetas de `<partial>` se pueden agregar al archivo *_Layout. cshtml* :

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a>Creación de un RCL con recursos estáticos

Una RCL puede requerir recursos estáticos complementarios a los que puede hacer referencia el RCL o la aplicación de consumo de RCL. ASP.NET Core permite crear RCLs que incluyen recursos estáticos que están disponibles para una aplicación de consumo.

Para incluir los recursos complementarios como parte de un RCL, cree una carpeta *wwwroot* en la biblioteca de clases e incluya los archivos necesarios en esa carpeta.

Al empaquetar un RCL, todos los recursos complementarios de la carpeta *wwwroot* se incluyen automáticamente en el paquete.

### <a name="exclude-static-assets"></a>Excluir recursos estáticos

Para excluir recursos estáticos, agregue la ruta de exclusión deseada al grupo de propiedades `$(DefaultItemExcludes)` en el archivo de proyecto. Separe las entradas con un punto y coma (`;`).

En el ejemplo siguiente, la hoja de estilos *lib. CSS* de la carpeta *wwwroot* no se considera un recurso estático y no se incluye en el RCL publicado:

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a>Integración de Typescript

Para incluir los archivos TypeScript en un RCL:

1. Coloque los archivos TypeScript ( *. ts*) fuera de la carpeta *wwwroot* . Por ejemplo, coloque los archivos en una carpeta de *cliente* .

1. Configure el resultado de la compilación de TypeScript para la carpeta *wwwroot* . Establezca la propiedad `TypescriptOutDir` dentro de un `PropertyGroup` en el archivo de proyecto:

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. Incluya el destino de TypeScript como una dependencia del destino de `ResolveCurrentProjectStaticWebAssets` agregando el siguiente destino dentro de un `PropertyGroup` en el archivo de proyecto:

   ```xml
   <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
     CompileTypeScript;
     $(ResolveCurrentProjectStaticWebAssetsInputs)
   </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a>Consumo de contenido de una RCL a la que se hace referencia

Los archivos incluidos en la carpeta *wwwroot* de RCL se exponen a RCL o a la aplicación de consumo en el prefijo `_content/{LIBRARY NAME}/`. Por ejemplo, una biblioteca denominada *Razor. class. lib* da como resultado una ruta de acceso al contenido estático en `_content/Razor.Class.Lib/`. Al generar un paquete de NuGet y el nombre de ensamblado no es el mismo que el identificador de paquete, use el identificador de paquete para `{LIBRARY NAME}`.

La aplicación de consumo hace referencia a los recursos estáticos proporcionados por la biblioteca con `<script>`, `<style>`, `<img>`y otras etiquetas HTML. La aplicación de consumo debe tener habilitada la [compatibilidad con archivos estáticos](xref:fundamentals/static-files) en `Startup.Configure`:

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

Al ejecutar la aplicación de consumo desde la salida de la compilación (`dotnet run`), los activos web estáticos están habilitados de forma predeterminada en el entorno de desarrollo. Para admitir recursos en otros entornos cuando se ejecutan desde la salida de la compilación, llame a `UseStaticWebAssets` en el generador de hosts en *Program.CS*:

```csharp
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStaticWebAssets();
                webBuilder.UseStartup<Startup>();
            });
}
```

No es necesario llamar a `UseStaticWebAssets` cuando se ejecuta una aplicación desde la salida publicada (`dotnet publish`).

### <a name="multi-project-development-flow"></a>Flujo de desarrollo de varios proyectos

Cuando se ejecuta la aplicación de consumo:

* Los recursos de RCL permanecen en sus carpetas originales. Los recursos no se mueven a la aplicación de consumo.
* Cualquier cambio dentro de la carpeta *wwwroot* de RCL se refleja en la aplicación de consumo después de que se vuelva a generar RCL y sin volver a generar la aplicación de consumo.

Cuando se compila el RCL, se genera un manifiesto que describe las ubicaciones de los recursos web estáticos. La aplicación consumidora lee el manifiesto en tiempo de ejecución para consumir los recursos de los proyectos y paquetes a los que se hace referencia. Cuando se agrega un nuevo recurso a un RCL, se debe volver a generar el RCL para actualizar el manifiesto antes de que una aplicación de consumo pueda acceder al nuevo recurso.

### <a name="publish"></a>Publicar

Cuando se publica la aplicación, los recursos complementarios de todos los proyectos y paquetes a los que se hace referencia se copian en la carpeta *wwwroot* de la aplicación publicada en `_content/{LIBRARY NAME}/`.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Las vistas, páginas, controladores, modelos de páginas, [componentes de Razor](xref:blazor/class-libraries), componentes de [vista](xref:mvc/views/view-components)y modelos de datos de Razor se pueden integrar en una biblioteca de clases de Razor (RCL). Las RCL se pueden empaquetar y reutilizar. Las aplicaciones pueden incluir la RCL y reemplazar las vistas y páginas que contienen. Cuando existe una vista, vista parcial o página de Razor tanto en la aplicación web como en la RCL, tendrá prioridad el marcado de Razor (archivo *.cshtml*) de la aplicación web.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>Crear una biblioteca de clases que contenga interfaz de usuario de Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.
* Seleccione **Aplicación web ASP.NET Core**.
* Asigne un nombre a la biblioteca (por ejemplo, "RazorClassLib") > **Aceptar**. Para evitar un conflicto de nombres de archivo con la biblioteca de vistas generada, asegúrese de que el nombre de la biblioteca no acaba en `.Views`.
* Confirme que **ASP.NET Core 2.1** o una versión posterior está seleccionado.
* Seleccione **biblioteca de clases de Razor** > **Aceptar**.

Un RCL tiene el siguiente archivo de proyecto:

[!code-xml[](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Ejecute `dotnet new razorclasslib` desde la línea de comandos. Por ejemplo:

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

Para más información, vea [dotnet new](/dotnet/core/tools/dotnet-new). Para evitar un conflicto de nombres de archivo con la biblioteca de vistas generada, asegúrese de que el nombre de la biblioteca no acaba en `.Views`.

---

Agregue archivos de Razor a la RCL.

Las plantillas ASP.NET Core suponen que el contenido de RCL se encuentra en la carpeta *áreas* . Consulte [diseño de páginas de RCL](#rcl-pages-layout) para crear un RCL que exponga contenido en `~/Pages` en lugar de `~/Areas/Pages`.

## <a name="reference-rcl-content"></a>Contenido de RCL de referencia

Los siguientes elementos pueden hacer referencia a la RCL:

* Paquete NuGet. Vea [Creación de paquetes NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) y [Creación y publicación de un paquete NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* *{NombreDeProyecto}.csproj*. Vea [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a>Tutorial: creación de un proyecto de RCL y uso de un proyecto de Razor Pages

En lugar de crearlo, puede descargar el [proyecto completo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) y comprobarlo. La descarga de ejemplo contiene más código y vínculos que hacen que el proyecto sea fácil de comprobar. Puede dejar sus comentarios en [este problema de GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6098) sobre las descargas de ejemplo en comparación con las instrucciones paso a paso.

### <a name="test-the-download-app"></a>Comprobar la aplicación de descarga

Si no ha descargado la aplicación final y prefiere crear el proyecto de tutorial, omita este paso y vaya a la [siguiente sección](#create-an-rcl).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Abra el archivo *.sln* en Visual Studio. Ejecute la aplicación.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Desde un símbolo del sistema en el directorio *cli*, cree la RCL y la aplicación web.

```dotnetcli
dotnet build
```

Vaya al directorio *WebApp1* y ejecute la aplicación:

```dotnetcli
dotnet run
```

---

Siga las instrucciones de [Probar WebApp1](#test-webapp1).

## <a name="create-an-rcl"></a>Creación de un RCL

En esta sección, se crea un RCL. y se agregarán a ella archivos de Razor.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Cree el proyecto de RCL:

* En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.
* Seleccione **Aplicación web ASP.NET Core**.
* Asigne a la aplicación el nombre **RazorUIClassLib** > **Aceptar**.
* Confirme que **ASP.NET Core 2.1** o una versión posterior está seleccionado.
* Seleccione **biblioteca de clases de Razor** > **Aceptar**.
* Agregue un archivo de vista parcial de Razor denominado *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Ejecute lo siguiente desde la línea de comandos:

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

Los comandos anteriores:

* Crea el `RazorUIClassLib` RCL.
* Crean una página _Message de Razor y la agrega a la RCL. El parámetro `-np` crea la página sin un `PageModel`.
* Crea un archivo [_ViewStart. cshtml](xref:mvc/views/layout#running-code-before-each-view) y lo agrega a RCL.

El archivo *_ViewStart. cshtml* es necesario para usar el diseño del proyecto Razor Pages (que se agrega en la sección siguiente).

---

### <a name="add-razor-files-and-folders-to-the-project"></a>Agregar archivos de Razor y carpetas al proyecto

* Reemplace el marcado de *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* por el siguiente código:

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* Reemplace el marcado de *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* por el siguiente código:

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

  Se necesita `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` para usar la vista parcial (`<partial name="_Message" />`). En lugar de incluir la directiva `@addTagHelper`, puede agregar un archivo *_ViewImports.cshtml*. Por ejemplo:

  ```dotnetcli
  dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
  ```

  Para obtener más información sobre *_ViewImports. cshtml*, vea [importar directivas compartidas](xref:mvc/views/layout#importing-shared-directives) .

* Compile la biblioteca de clases para confirmar que no hay ningún error de compilador:

  ```dotnetcli
  dotnet build RazorUIClassLib
  ```

El resultado de la compilación contiene *RazorUIClassLib.dll* y *RazorUIClassLib.Views.dll*. *RazorUIClassLib.Views.dll* incluye el contenido de Razor compilado.

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a>Usar la biblioteca de interfaz de usuario de Razor desde un proyecto de páginas de Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Cree la aplicación web de páginas de Razor:

* En **Explorador de soluciones**, haga clic con el botón secundario en la solución > **Agregar** >**nuevo proyecto**.
* Seleccione **Aplicación web ASP.NET Core**.
* Denomine la aplicación **WebApp1**.
* Confirme que **ASP.NET Core 2.1** o una versión posterior está seleccionado.
* Seleccione **aplicación Web** > **Aceptar**.

* En el **Explorador de soluciones**, haga clic con el botón derecho en **WebApp1** y seleccione **Establecer como proyecto de inicio**.
* En **Explorador de soluciones**, haga clic con el botón derecho en **WebApp1** y seleccione **dependencias de compilación** > **dependencias del proyecto**.
* Marque **RazorUIClassLib** como dependencia de **WebApp1**.
* En **Explorador de soluciones**, haga clic con el botón derecho en **WebApp1** y seleccione **Agregar** **referencia**de >.
* En el cuadro de diálogo **Administrador de referencias** , active **RazorUIClassLib** > **Aceptar**.

Ejecute la aplicación.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Cree una aplicación Web de Razor Pages y un archivo de solución que contenga la aplicación Razor Pages y RCL:

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

Compile y ejecute la aplicación web:

```dotnetcli
cd WebApp1
dotnet run
```

---

### <a name="test-webapp1"></a>Probar WebApp1

Vaya a `/MyFeature/Page1` para comprobar que la biblioteca de clases de la interfaz de usuario de Razor está en uso.

## <a name="override-views-partial-views-and-pages"></a>Reemplazar vistas, vistas parciales y páginas

Cuando existe una vista, vista parcial o página de Razor tanto en la aplicación web como en la RCL, tendrá prioridad el marcado de Razor (archivo *.cshtml*) de la aplicación web. Por ejemplo, agregue *WebApp1/areas/mi Feature/pages/Page1. cshtml* a WebApp1 y Page1 en WebApp1 tendrá prioridad sobre PÁGINA1 en RCL.

En la descarga de ejemplo, cambie el nombre *WebApp1/Areas/MyFeature2* por *WebApp1/Areas/MyFeature* para comprobar la prioridad.

Copie la vista parcial *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* en *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*. Actualice el marcado para señalar la nueva ubicación. Compile y ejecute la aplicación para comprobar si se está usando la versión de la vista parcial de la aplicación.

### <a name="rcl-pages-layout"></a>Diseño de páginas RCL

Para hacer referencia al contenido de RCL como si formara parte de la carpeta *páginas* de la aplicación Web, cree el proyecto RCL con la siguiente estructura de archivos:

* *RazorUIClassLib/páginas*
* *RazorUIClassLib/páginas/compartido*

Supongamos que *RazorUIClassLib/pages/Shared* contiene dos archivos parciales: *_Header. cshtml* y *_Footer. cshtml*. Las etiquetas de `<partial>` se pueden agregar al archivo *_Layout. cshtml* :

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker-end

## <a name="additional-resources"></a>Recursos adicionales

* <xref:blazor/class-libraries>
