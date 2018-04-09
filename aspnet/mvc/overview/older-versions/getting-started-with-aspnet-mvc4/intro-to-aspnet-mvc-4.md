---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: Introducción a ASP.NET MVC 4 | Documentos de Microsoft
author: Rick-Anderson
description: Una versión actualizada, si este tutorial está disponible aquí con Visual Studio 2013. El nuevo tutorial usa ASP.NET MVC 5, que proporciona muchas mejoras con respecto a t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 519bac22ba2607931c5f3123b9b567859a2b3d1c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="intro-to-aspnet-mvc-4"></a>Introducción a ASP.NET MVC 4
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Una versión actualizada, si está disponible en este tutorial [aquí](../../getting-started/introduction/getting-started.md) con [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). El nuevo tutorial usa ASP.NET MVC 5, que proporciona muchas mejoras con respecto a este tutorial.
> 
> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC 4 mediante Microsoft [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) o Visual Web Developer 2010 Express Service Pack 1. Se recomienda Visual Studio 2012, no tendrá que instalar nada para completar el tutorial. Si usa Visual Studio 2010 debe instalar los componentes siguientes. Puede instalar todas ellas haciendo clic en los vínculos siguientes:
> 
> - [Requisitos previos visuales Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Instalador WPI para ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
> 
> Si está usando Visual Studio 2010 en lugar de Visual Web Developer 2010, instale el [instalador WPI para ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392) y: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
> 
> Un proyecto de Visual Web Developer con el código fuente de C# está disponible como acompañamiento de este tema. [Descargue la versión de C#](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip).
> 
> En el tutorial se ejecute la aplicación en Visual Studio. También puede hacer la aplicación disponible a través de Internet mediante la implementación en un proveedor de hospedaje. Microsoft ofrece el hospedaje web gratuito para hasta 10 sitios web en un [libre de la cuenta de prueba de Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Para obtener información sobre cómo implementar un proyecto web de Visual Studio en un sitio Web de Windows Azure, consulte [crear e implementar un sitio web ASP.NET y la base de datos de SQL con Visual Studio](https://docs.microsoft.com/dotnet/azure/). Este tutorial también muestra cómo usar migraciones de Entity Framework Code First para implementar la base de datos de SQL Server en Windows Azure SQL Database (anteriormente SQL Azure).
> 
> Este tutorial se escribió Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="what-youll-build"></a>Lo que vamos a compilar

> [!NOTE]
> Una versión actualizada, si está disponible en este tutorial [aquí](../../getting-started/introduction/getting-started.md) con [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). El nuevo tutorial usa ASP.NET MVC 5, que proporciona muchas mejoras con respecto a este tutorial.


Se implementa una aplicación sencilla de mostrar una lista de películas que admite la creación, edición, buscar y enumerar películas de una base de datos. A continuación se muestran dos capturas de pantalla de la aplicación que compilará. Incluye una página que muestra una lista de películas de una base de datos:

![](intro-to-aspnet-mvc-4/_static/image1.png)

La aplicación también permite agregar, editar y eliminar películas, así como ver detalles sobre los individuales. Todos los escenarios de entrada de datos incluyen la validación para asegurarse de que los datos almacenados en la base de datos son correctos.

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>Introducción

Comience ejecutando Visual Studio Express 2012 o Visual Web Developer 2010 Express. La mayoría de las capturas de pantalla en esta serie uso Visual Studio Express 2012, pero puede completar este tutorial con Visual Studio 2010 SP1, Visual Studio 2012, Visual Studio Express 2012 o Visual Web Developer 2010 Express. Seleccione **nuevo proyecto** desde el **iniciar** página.

Visual Studio es un entorno de desarrollo integrado o IDE. Al igual que usa Microsoft Word para escribir documentos, deberá usar un IDE para crear aplicaciones. En Visual Studio, hay una barra de herramientas en la parte superior que muestra distintas opciones disponibles para usted. También hay un menú que proporciona otra manera de realizar las tareas en el IDE. (Por ejemplo, en lugar de seleccionar **nuevo proyecto** desde el **iniciar** página, puede usar el menú y seleccione **archivo** &gt; **denuevoproyecto**.)

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>Crear su primera aplicación

Puede crear aplicaciones mediante Visual Basic o Visual C# como lenguaje de programación. Seleccione Visual C# a la izquierda y, a continuación, seleccione **aplicación Web de ASP.NET MVC 4**. Denomine el proyecto &quot;MvcMovie&quot; y, a continuación, haga clic en **Aceptar**.

![](intro-to-aspnet-mvc-4/_static/image4.png)

En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione **aplicación de Internet**. Deje **Razor** como el motor de vista predeterminado.

![](intro-to-aspnet-mvc-4/_static/image5.png)

Haga clic en **Aceptar**. Visual Studio usa una plantilla predeterminada para el proyecto de MVC de ASP.NET que acaba de crear, por lo que tendrá una aplicación que funciona en este momento sin hacer nada. Se trata de una sencilla &quot;Hello World!&quot; proyecto y un buen lugar para iniciar la aplicación.

![](intro-to-aspnet-mvc-4/_static/image6.png)

En el menú **Depurar**, seleccione **Iniciar depuración**.

![](intro-to-aspnet-mvc-4/_static/image7.png)

Observe que el método abreviado de teclado para iniciar la depuración es F5.

F5 hace que Visual Studio iniciar IIS Express y ejecutar la aplicación web. Visual Studio, a continuación, inicia un explorador y abre la página principal de la aplicación. Observe que la barra de direcciones del explorador indica `localhost` y no algo como `example.com`. Esto es así porque `localhost` siempre apunta a su propio equipo local, que en este caso, se ejecuta la aplicación que acaba de crear. Cuando Visual Studio se ejecuta un proyecto web, se usa un puerto aleatorio para el servidor web. En la imagen siguiente, el número de puerto es 41788. Al ejecutar la aplicación, probablemente verá un número de puerto diferente.

![](intro-to-aspnet-mvc-4/_static/image8.png)

Desde el cuadro de esta plantilla predeterminada proporciona páginas de inicio, póngase en contacto con y sobre. También proporciona soporte técnico para registrar e iniciar sesión y se vincula a Facebook y Twitter. El paso siguiente es cambiar el funcionamiento de esta aplicación y aprender un poco sobre ASP.NET MVC. Cierre el explorador y vamos a cambiar parte del código.

> [!div class="step-by-step"]
> [Siguiente](adding-a-controller.md)
