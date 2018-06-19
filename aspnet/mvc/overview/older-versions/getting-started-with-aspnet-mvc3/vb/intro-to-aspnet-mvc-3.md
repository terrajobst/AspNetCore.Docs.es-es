---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
title: Introducción a ASP.NET MVC 3 (VB) | Documentos de Microsoft
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: a1b3d789-93b4-487f-b90d-80c9c9b4f8fa
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 3be0de6ea6d49f9c0de659398190b71c36ba222a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870380"
---
<a name="intro-to-aspnet-mvc-3-vb"></a>Introducción a ASP.NET MVC 3 (VB)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación. Puede instalar todas ellas haciendo clic en el siguiente vínculo: [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar por separado los requisitos previos mediante los siguientes vínculos:
> 
> - [Requisitos previos visuales Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(se admiten en tiempo de ejecución + herramientas)
> 
> Si usa Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos, haga clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un proyecto de Visual Web Developer con el código fuente VB.NET está disponible como acompañamiento de este tema. [Descargue la versión VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si lo prefiere C#, cambie a la [versión de C#](../cs/intro-to-aspnet-mvc-3.md) de este tutorial.


Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación. Puede instalar todas ellas haciendo clic en el siguiente vínculo: [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar por separado los requisitos previos mediante los siguientes vínculos:

- [Requisitos previos visuales Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(se admiten en tiempo de ejecución + herramientas)

Si usa Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos, haga clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Un proyecto de Visual Web Developer con el código fuente VB está disponible como acompañamiento de este tema. [Descargue la versión VB aquí](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824). Si lo prefiere, CSharp, cambie a la [versión CSharp](../cs/intro-to-aspnet-mvc-3.md) de este tutorial.

## <a name="what-youll-build"></a>Lo que vamos a compilar

Se implementa una aplicación sencilla de mostrar una lista de películas que admite la creación, edición y lista de películas de una base de datos. A continuación se muestran dos capturas de pantalla de la aplicación que compilará. Incluye una página que muestra una lista de películas de una base de datos:

[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)

La aplicación también permite agregar, editar y eliminar películas, así como ver detalles sobre los individuales. Todos los escenarios de entrada de datos incluyen la validación para asegurarse de que los datos almacenados en la base de datos son correctos.

[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="skills-youll-learn"></a>Obtendrá información de conocimientos

Esto es lo aprenderá:

- Cómo crear un nuevo proyecto de MVC de ASP.NET
- Cómo crear una nueva base de datos mediante código basado en Entity Framework
- Cómo crear ASP.NET MVC controladores y vistas
- Cómo recuperar y mostrar datos
- Cómo modificar datos y habilitar la validación de datos

## <a name="getting-started"></a>Introducción

Comience ejecutando Visual Web Developer 2010 Express ("VWD" para abreviar) y seleccione **nuevo proyecto** desde el **iniciar** página.

Visual Web Developer es un entorno de desarrollo integrado o IDE. Al igual que usa Microsoft Word para escribir documentos, deberá usar un IDE para crear aplicaciones. En Visual Web Developer, hay una barra de herramientas en la parte superior que muestra distintas opciones disponibles para usted. También hay un menú que proporciona otra manera de realizar las tareas en el IDE. (Por ejemplo, en lugar de seleccionar **nuevo proyecto** desde el **iniciar** página, puede usar el menú y seleccione **archivo** &gt; **denuevoproyecto**.)

[![](intro-to-aspnet-mvc-3/_static/image6.png)](intro-to-aspnet-mvc-3/_static/image5.png)

## <a name="creating-your-first-application"></a>Crear su primera aplicación

Puede crear aplicaciones mediante su elección de Visual Basic o Visual C# como lenguaje de programación. Para este tutorial, seleccione Visual Basic a la izquierda y luego seleccione **aplicación Web de ASP.NET MVC 3**. Denomine el proyecto "MvcMovie" y, a continuación, haga clic en **Aceptar**.

![1NewMVCproj_sm](intro-to-aspnet-mvc-3/_static/image7.png)

En el **nuevo proyecto de ASP.NET MVC 3** cuadro de diálogo, seleccione **aplicación de Internet**. Deje **Razor** como el motor de vista predeterminado.

![1InternetAppRazor_SM](intro-to-aspnet-mvc-3/_static/image8.png)

Haga clic en **Aceptar**. Visual Web Developer utilizará una plantilla predeterminada para el proyecto de MVC de ASP.NET que acaba de crear, por lo que tendrá una aplicación que funciona en este momento sin hacer nada. Se trata de un simple "Hola a todos" proyecto y, de un buen lugar para iniciar la aplicación.

[![](intro-to-aspnet-mvc-3/_static/image10.png)](intro-to-aspnet-mvc-3/_static/image9.png)

En el menú **Depurar**, seleccione **Iniciar depuración**.

![](intro-to-aspnet-mvc-3/_static/image11.png)

Observe que el método abreviado de teclado para iniciar la depuración es F5.

F5 hace que Visual Web Developer iniciar un servidor de desarrollo de web y ejecutar la aplicación web. VWD, a continuación, inicia un explorador y abre la página principal de la aplicación. Observe que la barra de direcciones del explorador indica `localhost` y no algo como `example.com`. Esto es así porque `localhost` siempre apunta a su propio equipo local, que en este caso, se ejecuta la aplicación que acaba de crear. Cuando VWD ejecuta un proyecto web, se usa un puerto aleatorio para el proyecto. En la imagen siguiente, el número de puerto aleatorio es 43246. Su proyecto probablemente utilizará un número de puerto diferente.

![](intro-to-aspnet-mvc-3/_static/image12.png)

De fábrica esta plantilla predeterminada proporciona dos páginas para visitar y una página de inicio de sesión básica. Vamos a cambiar el funcionamiento de esta aplicación y aprender un poco sobre ASP.NET MVC en el proceso. Cierre el explorador y vamos a cambiar parte del código.

> [!div class="step-by-step"]
> [Siguiente](adding-a-controller.md)
