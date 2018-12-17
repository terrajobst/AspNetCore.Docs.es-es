---
title: Introducción a las páginas de Razor en ASP.NET Core
author: rick-anderson
monikerRange: '>= aspnetcore-2.2'
description: Esta serie de tutoriales muestra cómo usar Razor Pages en ASP.NET Core. Obtenga información sobre cómo crear un modelo, generar código para Razor Pages, usar Entity Framework Core y SQL Server para el acceso a datos, agregar la funcionalidad de búsqueda, agregar validación de entrada y usar migraciones para actualizar el modelo.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1152ebfcee48a46ecd28c941fce32d3fc1e05c41
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861633"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a>Tutorial: Introducción a Razor Pages en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En este tutorial se enseñan los conceptos básicos de la compilación de una aplicación web de páginas de Razor de ASP.NET Core.

La aplicación administra una base de datos de títulos de películas. Aprenderá a:

> [!div class="checklist"]
> * Crear una aplicación web de Razor Pages.
> * Agregar un modelo y aplicarle scaffolding.
> * Trabajar con una base de datos.
> * Agregar búsqueda y validación.

Al final, tendrá una aplicación que le permitirá administrar y mostrar los elementos de los títulos de películas.

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-web-app"></a>Creación de una aplicación web de Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.
* Cree una aplicación web de ASP.NET Core. Asigne al proyecto el nombre **RazorPagesMovie**. Es importante asignarle el nombre *RazorPagesMovie* para que los espacios de nombres coincidan al copiar y pegar el código.
 ![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* Seleccione **ASP.NET Core 2.2** en la lista desplegable y, luego, **Aplicación web**.

  ![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  Se crea el proyecto de inicio siguiente:

  ![Explorador de soluciones](razor-pages-start/_static/se2.2.png)

* Presione **Ctrl+F5** para ejecutar sin el depurador.

  Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación. En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`). Esto es así porque `localhost` es el nombre de host estándar del equipo local. Localhost solo sirve las solicitudes web del equipo local. Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web. En la imagen anterior, el número de puerto es 5001. Al ejecutar la aplicación verá otro puerto distinto.

  Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código. Muchos desarrolladores prefieren usar el modo de no depuración para actualizar la página y ver los cambios.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Cambie los directorios (`cd`) a una carpeta que contenga el proyecto.
* Ejecute el siguiente comando:

   ```console
   dotnet new webapp -o RazorPagesMovie
   code -r RazorPagesMovie
   ```

  * Se muestra un cuadro de diálogo con el texto **Required assets to build and debug are missing from "RazorPagesMovie". Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).  Seleccione **Sí**.

  * `dotnet new webapp -o RazorPagesMovie`: crea un nuevo proyecto de Razor Pages en la carpeta *RazorPagesMovie*.
  * `code -r RazorPagesMovie`: carga el archivo del proyecto *RazorPagesMovie.csproj* en Visual Studio Code.

### <a name="launch-the-app"></a>Iniciar la aplicación

* Presione **Ctrl+F5** para ejecutar sin el depurador.

  Visual Studio Code inicia [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplaza hasta `http://localhost:5001`. En la barra de direcciones aparece `localhost:port:5001` (y no algo como `example.com`). Esto es así porque `localhost` es el nombre de host estándar del equipo local. Localhost solo sirve las solicitudes web del equipo local.

  Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código. Muchos desarrolladores prefieren usar el modo de no depuración para actualizar la página y ver los cambios.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

Desde un terminal, ejecute estos comandos:

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

Los comandos anteriores usan el [CLI de .NET Core](/dotnet/core/tools/dotnet) para crear y ejecutar un proyecto de páginas de Razor. Abra http://localhost:5000 en un explorador para ver la aplicación.

## <a name="open-the-project"></a>Abrir el proyecto

Presione CTRL+C para cerrar la aplicación.

En Visual Studio, seleccione **Archivo > Abrir** y elija el archivo *RazorPagesMovie.csproj*.

### <a name="launch-the-app"></a>Iniciar la aplicación

Seleccione **Ejecutar > Iniciar sin depurar** para iniciar la aplicación. Visual Studio iniciará [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplazará a `http://localhost:5001`.

<!-- End of VS tabs -->

---

* Seleccione **Aceptar** para dar su consentimiento al seguimiento. Esta aplicación no lleva un seguimiento de la información personal. El código generado con plantilla incluye activos que sirven para cumplir el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr).

  ![Página Inicio o Índice](razor-pages-start/_static/homeGDPR2.2.png)

  En la siguiente imagen se muestra la aplicación tras haber aceptado el seguimiento:

  ![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)

## <a name="project-files-and-folders"></a>Archivos y carpetas del proyecto

En la tabla siguiente se enumeran los archivos y las carpetas del proyecto. En este punto del tutorial, el archivo *Startup.cs* es el más importante. No es necesario revisar todos los vínculos siguientes. Los vínculos se proporcionan como referencia para cuando necesite más información sobre un archivo o una carpeta del proyecto.

| Archivo o carpeta              | Propósito |
| ----------------- | ------------ |
| *wwwroot* | Contiene archivos estáticos. Vea [Archivos estáticos](xref:fundamentals/static-files). |
| *Páginas* | Carpeta para [páginas de Razor](xref:razor-pages/index). |
| *appsettings.json* | [Configuración](xref:fundamentals/configuration/index) |
| *Program.cs* | [Aloja](xref:fundamentals/host/index) la aplicación de ASP.NET Core.|
| *Startup.cs* | Configura los servicios y la canalización de solicitudes. Consulte [Inicio](xref:fundamentals/startup).|

### <a name="the-pages-folder"></a>Carpeta Páginas

El archivo *_Layout.cshtml* contiene elementos HTML comunes (scripts y hojas de estilos) y establece el diseño de la aplicación. Por ejemplo, al hacer clic en **RazorPagesMovie**, **Inicio** o **Privacidad**, verá los mismos elementos. Los elementos comunes incluyen el menú de navegación de la parte superior y el encabezado de la parte inferior de la ventana. Vea [Layout](xref:mvc/views/layout) (Diseño) para más información.

El archivo *_ViewImports.cshtml* contiene directivas de Razor que se importan en cada página de Razor. Consulte [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives) (Importar directivas compartidas) para obtener más información.

El archivo *_ViewStart.cshtml* establece la propiedad `Layout` de las páginas de Razor que para usar el archivo *_Layout.cshtml*. Vea [Layout](xref:mvc/views/layout) (Diseño) para más información.

El archivo *_ValidationScriptsPartial.cshtml* proporciona una referencia de los scripts de validación de [jQuery](https://jquery.com/). Cuando las páginas `Create` y `Edit` se agreguen más adelante en el tutorial, se usará el archivo *_ValidationScriptsPartial.cshtml*.

Las páginas `Index`, `Error` y `Privacy` sirven para:

* `Index`: iniciar una aplicación.
* `Error`: mostrar información sobre errores.
* `Privacy`: especificar los detalles de la directiva de privacidad del sitio web.

En este tutorial, las páginas anteriores no se utilizan.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

<a name="f7"></a>
### <a name="use-f7-to-toggle-between-a-razor-page-and-the-pagemodel"></a>Use F7 para alternar entre una página de Razor y la clase PageModel

F7 alterna entre una página de Razor (archivo *\*.cshtml*) y el archivo de C# (*\*.cshtml.cs*).

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!-- TODO review  Need something in these tabs -->

Por convención, la página de Razor (archivo *\*.cshtml*) y el elemento `PageModel` asociado tienen el mismo nombre de archivo raíz.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

Por convención, la página de Razor (archivo *\*.cshtml*) y el elemento `PageModel` asociado tienen el mismo nombre de archivo raíz.

---

> [!div class="step-by-step"]
> [Siguiente: Adición de un modelo](xref:tutorials/razor-pages/model)