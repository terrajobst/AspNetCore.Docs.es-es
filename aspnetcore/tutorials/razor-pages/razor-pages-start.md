---
title: 'Tutorial: Introducción a Razor Pages en ASP.NET Core'
author: rick-anderson
description: Esta serie de tutoriales muestra cómo usar Razor Pages en ASP.NET Core. Obtenga información sobre cómo crear un modelo, generar código para Razor Pages, usar Entity Framework Core y SQL Server para el acceso a datos, agregar la funcionalidad de búsqueda, agregar validación de entrada y usar migraciones para actualizar el modelo.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1d264ca4a605d8291e273a8f054c92e7eefa5548
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468852"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a>Tutorial: Introducción a Razor Pages en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Este es el primer tutorial de una serie. En la [serie](xref:tutorials/razor-pages/index) se enseñan los conceptos básicos de la compilación de una aplicación web de Razor Pages en ASP.NET Core.

[!INCLUDE[](~/includes/advancedRP.md)]

Al final de la serie, tendrá una aplicación que puede administrar una base de datos de películas.  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

En este tutorial va a:

> [!div class="checklist"]
> * Crear una aplicación web de Razor Pages.
> * Ejecutar la aplicación.
> * Examinar los archivos de proyecto.

Al final de este tutorial, tendrá una aplicación web de Razor Pages que compilará en los tutoriales posteriores.

![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a>Creación de una aplicación web de páginas de Razor

# [<a name="visual-studio"></a>Programa para la mejora](#tab/visual-studio)

* En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.

* Cree una aplicación web de ASP.NET Core. Asigne al proyecto el nombre **RazorPagesMovie**. Es importante asignarle el nombre *RazorPagesMovie* para que los espacios de nombres coincidan al copiar y pegar el código.

  ![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* Seleccione **ASP.NET Core 2.2** en la lista desplegable y, luego, **Aplicación web**.

  ![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  Se crea el proyecto de inicio siguiente:

  ![Explorador de soluciones](razor-pages-start/_static/se2.2.png)

# [<a name="visual-studio-code"></a>Visual Studio Code](#tab/visual-studio-code)

* Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).

* Cambie los directorios (`cd`) a una carpeta que contenga el proyecto.

* Ejecute los comandos siguientes:

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * El comando `dotnet new` crea un proyecto de Razor Pages en la carpeta *RazorPagesMovie*.
  * El comando `code` abre la carpeta *RazorPagesMovie* en una nueva instancia de Visual Studio Code.

  Se muestra un cuadro de diálogo con el texto **Required assets to build and debug are missing from "RazorPagesMovie". Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).

* Seleccione **Sí**.

# [<a name="visual-studio-for-mac"></a>Visual Studio para Mac](#tab/visual-studio-mac)

Desde un terminal, ejecute este comando:

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

Los comandos anteriores utilizan la [CLI de .NET Core](/dotnet/core/tools/dotnet) para crear un proyecto de Razor Pages.

## <a name="open-the-project"></a>Abrir el proyecto

En Visual Studio, seleccione **Archivo > Abrir** y elija el archivo *RazorPagesMovie.csproj*.

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a>Ejecutar la aplicación

# [<a name="visual-studio"></a>Programa para la mejora](#tab/visual-studio)

* Presione Ctrl+F5 para ejecutarla sin el depurador.

  [!INCLUDE[](~/includes/trustCertVS.md)]

  Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación. En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`). Esto es así porque `localhost` es el nombre de host estándar del equipo local. Localhost solo sirve las solicitudes web del equipo local. Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.

* En la página principal de la aplicación, seleccione **Aceptar** para dar su consentimiento al seguimiento.

  Esta aplicación no realiza un seguimiento de la información personal, pero la plantilla del proyecto incluye la función de consentimiento en caso de que sea necesaria para cumplir con el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr) de la Unión Europea.

  ![Página Inicio o Índice](razor-pages-start/_static/homeGDPR2.2.png)

  En la siguiente imagen se muestra la aplicación tras haber dado su consentimiento al seguimiento:

  ![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)
  
# [<a name="visual-studio-code"></a>Visual Studio Code](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* Presione **Ctrl+F5** para ejecutar sin el depurador.

  Visual Studio Code inicia [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplaza hasta `http://localhost:5001`. En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`). Esto es así porque `localhost` es el nombre de host estándar del equipo local. Localhost solo sirve las solicitudes web del equipo local.

* En la página principal de la aplicación, seleccione **Aceptar** para dar su consentimiento al seguimiento.

  Esta aplicación no realiza un seguimiento de la información personal, pero la plantilla del proyecto incluye la función de consentimiento en caso de que sea necesaria para cumplir con el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr) de la Unión Europea.

  ![Página Inicio o Índice](razor-pages-start/_static/homeGDPR2.2.png)

  En la siguiente imagen se muestra la aplicación tras haber dado su consentimiento al seguimiento:

  ![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)
  
# [<a name="visual-studio-for-mac"></a>Visual Studio para Mac](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* Presione **Cmd-Opt-F5** para realizar la ejecución sin el depurador.

  Visual Studio iniciará [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplazará a `http://localhost:5001`.

* En la página principal de la aplicación, seleccione **Aceptar** para dar su consentimiento al seguimiento.

  Esta aplicación no realiza un seguimiento de la información personal, pero la plantilla del proyecto incluye la función de consentimiento en caso de que sea necesaria para cumplir con el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr) de la Unión Europea.

  ![Página Inicio o Índice](razor-pages-start/_static/homeGDPR2.2_safari.png)

  En la siguiente imagen se muestra la aplicación tras haber dado su consentimiento al seguimiento:

  ![Página Inicio o Índice](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a>Examen de los archivo del proyecto

He aquí un resumen de las principales carpetas y archivos del proyecto con los que va a trabajar en los próximos tutoriales.

### <a name="pages-folder"></a>Carpeta Pages

Contiene Razor Pages y los archivos auxiliares. Cada página de Razor se compone de un par de archivos:

* Archivo *.cshtml* que contiene el marcado HTML con código C# que usa la sintaxis Razor.
* Archivo *. cshtml.cs* que contiene C# código que controla los eventos de página.

Los archivos auxiliares tienen nombres que comienzan con un carácter de subrayado. Por ejemplo, el archivo *_Layout.cshtml* configura los elementos de la interfaz de usuario comunes a todas las páginas. Este archivo configura el menú de navegación de la parte superior de la página y el aviso de copyright de la parte inferior de la página. Para obtener más información, vea <xref:mvc/views/layout>.

### <a name="wwwroot-folder"></a>Carpeta wwwroot

Contiene los archivos estáticos, como los archivos HTML, los archivos de JavaScript y los archivos CSS. Para obtener más información, vea <xref:fundamentals/static-files>.

### <a name="appsettingsjson"></a>appSettings.json

Contiene los datos de configuración, como las cadenas de conexión. Para obtener más información, vea <xref:fundamentals/configuration/index>.

### <a name="programcs"></a>Program.cs

Contiene el punto de entrada del programa. Para obtener más información, vea <xref:fundamentals/host/web-host>.

### <a name="startupcs"></a>Startup.cs

Contiene código que configura el comportamiento de la aplicación, como, por ejemplo, si se requiere consentimiento para las cookies. Para obtener más información, vea <xref:fundamentals/startup>.

## <a name="additional-resources"></a>Recursos adicionales

* [Versión en YouTube de este tutorial](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha:

> [!div class="checklist"]
> * Creado una aplicación web de Razor Pages.
> * Ejecutado la aplicación.
> * Examinado los archivo del proyecto.

Pase al siguiente tutorial de la serie:

> [!div class="step-by-step"]
> [Adición de un modelo](xref:tutorials/razor-pages/model)
