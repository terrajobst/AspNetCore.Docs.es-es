---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: Introducción a ASP.NET MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Una versión actualizada, si este tutorial está disponible aquí con Visual Studio 2013. El nuevo tutorial usa ASP.NET MVC 5, que proporciona muchas mejoras con respecto a t...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: ea3d1517192ded0e5372c49897bb1fec33324b6f
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912402"
---
<a name="intro-to-aspnet-mvc-4"></a>Introducción a ASP.NET MVC 4
====================
por [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Una versión actualizada, si está disponible en este tutorial [aquí](../../getting-started/introduction/getting-started.md) mediante [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). El nuevo tutorial usa ASP.NET MVC 5, que proporciona muchas mejoras con respecto a este tutorial.
>
> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC 4 mediante Microsoft [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) o Visual Web Developer 2010 Express Service Pack 1. Se recomienda Visual Studio 2012, no tendrá que instalar nada para completar el tutorial. Si usa Visual Studio 2010 debe instalar los componentes a continuación. Puede instalar todos ellos haciendo clic en los vínculos siguientes:
>
> - [Requisitos previos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Instalador WPI para ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
>
> Si usa Visual Studio 2010, en lugar de Visual Web Developer 2010, instale el [instalador WPI para ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392) y: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
>
> Un proyecto de Visual Web Developer con código fuente de C# está disponible como acompañamiento de este tema. [Descargue la versión de C#](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip).
>
> En el tutorial para ejecutar la aplicación en Visual Studio. Se puede también que la aplicación esté disponible a través de Internet mediante su implementación en un proveedor de hospedaje. Microsoft ofrece hospedaje web gratuito para hasta 10 sitios web en un [cuenta de evaluación de Windows Azure gratuita](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Para obtener información sobre cómo implementar un proyecto web de Visual Studio a un sitio Web de Windows Azure, consulte [crear e implementar un sitio web de ASP.NET y SQL Database con Visual Studio](https://docs.microsoft.com/dotnet/azure/). Este tutorial también muestra cómo usar migraciones de Entity Framework Code First para implementar la base de datos de SQL Server en Windows Azure SQL Database (anteriormente SQL Azure).
>
> En este tutorial se escribió por Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="what-youll-build"></a>¿Qué va a crear

> [!NOTE]
> Una versión actualizada, si está disponible en este tutorial [aquí](../../getting-started/introduction/getting-started.md) mediante [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). El nuevo tutorial usa ASP.NET MVC 5, que proporciona muchas mejoras con respecto a este tutorial.


Implementará una aplicación simple de la lista de películas que admite la creación, edición, buscar y mostrar películas con una base de datos. A continuación se muestran dos capturas de pantalla de la aplicación que se va a crear. Incluye una página que muestra una lista de películas de una base de datos:

![](intro-to-aspnet-mvc-4/_static/image1.png)

La aplicación también le permite agregar, editar y eliminar las películas, así como ver detalles acerca de los individuales. Todos los escenarios de entrada de datos incluyen la validación para asegurarse de que los datos almacenados en la base de datos están correctos.

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>Introducción

Comience ejecutando Visual Studio Express 2012 o Visual Web Developer 2010 Express. La mayoría de las capturas de pantalla de esta serie se usa Visual Studio Express 2012, pero puede completar este tutorial con Visual Studio 2010 SP1, Visual Studio 2012, Visual Studio Express 2012 o Visual Web Developer 2010 Express. Seleccione **nuevo proyecto** desde el **iniciar** página.

Visual Studio es un entorno de desarrollo integrado o IDE. Al igual que utiliza Microsoft Word para escribir documentos, usará un IDE para crear aplicaciones. En Visual Studio, hay una barra de herramientas en la parte superior que muestra diversas opciones disponibles para usted. También hay un menú que proporciona otra manera de realizar las tareas en el IDE. (Por ejemplo, en lugar de seleccionar **nuevo proyecto** desde el **iniciar** página, puede usar el menú y seleccione **archivo** &gt; **denuevoproyecto**.)

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>Crear su primera aplicación

Puede crear aplicaciones con Visual Basic o Visual C# como lenguaje de programación. Seleccione Visual C# a la izquierda y, a continuación, seleccione **aplicación Web de ASP.NET MVC 4**. Nombre del proyecto &quot;MvcMovie&quot; y, a continuación, haga clic en **Aceptar**.

![](intro-to-aspnet-mvc-4/_static/image4.png)

En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione **aplicación de Internet**. Deje **Razor** como el motor de vistas predeterminado.

![](intro-to-aspnet-mvc-4/_static/image5.png)

Haga clic en **Aceptar**. Visual Studio usa una plantilla predeterminada para el proyecto de ASP.NET MVC que acaba de crear, por lo que tiene ahora una aplicación de trabajo sin hacer nada! Se trata de una sencilla &quot;Hello World!&quot; proyecto y, de un buen lugar para iniciar la aplicación.

![](intro-to-aspnet-mvc-4/_static/image6.png)

En el menú **Depurar**, seleccione **Iniciar depuración**.

![](intro-to-aspnet-mvc-4/_static/image7.png)

Observe que el método abreviado de teclado para iniciar la depuración F5.

F5 hace que Visual Studio iniciar IIS Express y ejecutar la aplicación web. A continuación, Visual Studio inicia un explorador y abre la página principal de la aplicación. Tenga en cuenta que la barra de direcciones del explorador dice `localhost` y no algo como `example.com`. Eso es porque `localhost` siempre apunta a su propio equipo local, que en este caso, se ejecuta la aplicación que acaba de crear. Cuando Visual Studio se ejecuta un proyecto web, se usa un puerto aleatorio para el servidor web. En la imagen siguiente, el número de puerto es 41788. Al ejecutar la aplicación, probablemente verá un número de puerto diferente.

![](intro-to-aspnet-mvc-4/_static/image8.png)

Desde el comienzo esta plantilla predeterminada proporciona páginas principal, contactos y sobre. También proporciona compatibilidad para registrar e inicie sesión y vínculos a Facebook y Twitter. El siguiente paso es cambiar el funcionamiento de esta aplicación y aprender un poco sobre ASP.NET MVC. Cierre el explorador y vamos a cambiar algo de código.

> [!div class="step-by-step"]
> [Siguiente](adding-a-controller.md)
