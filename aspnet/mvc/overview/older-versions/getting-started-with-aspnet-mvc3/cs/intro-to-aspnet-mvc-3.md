---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: Introducción a ASP.NET MVC 3 (C#) | Documentos de Microsoft
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 9b965df6175051a084de35627160161c116be42d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="intro-to-aspnet-mvc-3-c"></a>Introducción a ASP.NET MVC 3 (C#)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Hay disponible una versión actualizada de este tutorial [aquí](../../../getting-started/introduction/getting-started.md) que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y se muestra más características.
> 
> 
> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación. Puede instalar todas ellas haciendo clic en el siguiente vínculo: [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar por separado los requisitos previos mediante los siguientes vínculos:
> 
> - [Requisitos previos visuales Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(se admiten en tiempo de ejecución + herramientas)
> 
> Si usa Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos, haga clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un proyecto de Visual Web Developer con el código fuente de C# está disponible como acompañamiento de este tema. [Descargue la versión de C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si prefiere Visual Basic, cambie a la [versión de Visual Basic](../vb/intro-to-aspnet-mvc-3.md) de este tutorial.


## <a name="what-youll-build"></a>Lo que vamos a compilar

Se implementa una aplicación sencilla de mostrar una lista de películas que admite la creación, edición y lista de películas de una base de datos. A continuación se muestran dos capturas de pantalla de la aplicación que compilará. Incluye una página que muestra una lista de películas de una base de datos:

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

La aplicación también permite agregar, editar y eliminar películas, así como ver detalles sobre los individuales. Todos los escenarios de entrada de datos incluyen la validación para asegurarse de que los datos almacenados en la base de datos son correctos.

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a>Obtendrá información de conocimientos

Esto es lo aprenderá:

- Cómo crear un nuevo proyecto de MVC de ASP.NET.
- Cómo crear ASP.NET MVC controladores y vistas.
- Cómo crear una nueva base de datos mediante el paradigma de Entity Framework Code First.
- Cómo recuperar y mostrar los datos.
- Cómo modificar datos y habilitar la validación de datos.

## <a name="getting-started"></a>Introducción

Comience ejecutando Visual Web Developer 2010 Express ("Visual Web Developer" para abreviar) y seleccione **nuevo proyecto** desde el **iniciar** página.

Visual Web Developer es un entorno de desarrollo integrado o IDE. Al igual que usa Microsoft Word para escribir documentos, deberá usar un IDE para crear aplicaciones. En Visual Web Developer, hay una barra de herramientas en la parte superior que muestra distintas opciones disponibles para usted. También hay un menú que proporciona otra manera de realizar las tareas en el IDE. (Por ejemplo, en lugar de seleccionar **nuevo proyecto** desde el **iniciar** página, puede usar el menú y seleccione **archivo** &gt; **denuevoproyecto**.)

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a>Crear su primera aplicación

Puede crear aplicaciones mediante Visual Basic o Visual C# como lenguaje de programación. Seleccione Visual C# a la izquierda y, a continuación, seleccione **aplicación Web de ASP.NET MVC 3**. Denomine el proyecto "MvcMovie" y, a continuación, haga clic en **Aceptar**. (Si lo prefiere, Visual Basic, cambie a la [versión de Visual Basic](../vb/intro-to-aspnet-mvc-3.md) de este tutorial.)

![](intro-to-aspnet-mvc-3/_static/image5.png)

En el **nuevo proyecto de ASP.NET MVC 3** cuadro de diálogo, seleccione **aplicación de Internet**. Comprobar **uso HTML5 marcado** y deje **Razor** como el motor de vista predeterminado.

![](intro-to-aspnet-mvc-3/_static/image6.png)

Haga clic en **Aceptar**. Visual Web Developer utilizará una plantilla predeterminada para el proyecto de MVC de ASP.NET que acaba de crear, por lo que tendrá una aplicación que funciona en este momento sin hacer nada. Se trata de un simple "Hola a todos" proyecto y, de un buen lugar para iniciar la aplicación.

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

En el menú **Depurar**, seleccione **Iniciar depuración**.

![](intro-to-aspnet-mvc-3/_static/image9.png)

Observe que el método abreviado de teclado para iniciar la depuración es F5.

F5 hace que Visual Web Developer iniciar un servidor de desarrollo de web y ejecutar la aplicación web. A continuación, Visual Web Developer inicia un explorador y se abre la página principal de la aplicación. Observe que la barra de direcciones del explorador indica `localhost` y no algo como `example.com`. Esto es así porque `localhost` siempre apunta a su propio equipo local, que en este caso, se ejecuta la aplicación que acaba de crear. Cuando Visual Web Developer ejecuta un proyecto web, se usa un puerto aleatorio para el servidor web. En la imagen siguiente, el número de puerto aleatorio es 43246. Al ejecutar la aplicación, probablemente verá un número de puerto diferente.

![](intro-to-aspnet-mvc-3/_static/image10.png)

Desde el cuadro de esta plantilla predeterminada proporciona dos páginas para visitar y una página de inicio de sesión básica. El paso siguiente es cambiar el funcionamiento de esta aplicación y aprender un poco sobre ASP.NET MVC en el proceso. Cierre el explorador y vamos a cambiar parte del código.

> [!div class="step-by-step"]
> [Siguiente](adding-a-controller.md)
