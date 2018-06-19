---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Introducción a ASP.NET MVC | Documentos de Microsoft
author: shanselman
description: Se trata de un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Crear una aplicación web simple que lee y escribe desde una base de datos.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 476d832e389b9b5a26fe2d552ca648c79b100056
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2018
ms.locfileid: "30868495"
---
<a name="intro-to-aspnet-mvc"></a>Introducción a ASP.NET MVC
====================
by [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Una versión actualizada, si está disponible en este tutorial [aquí](../../getting-started/introduction/getting-started.md) con [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). El nuevo tutorial usa ASP.NET MVC 5, que proporciona muchas mejoras con respecto a este tutorial.
> 
> 
> Se trata de un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Se creará una aplicación web simple que lee y escribe desde una base de datos. Visite la [centro de aprendizaje de ASP.NET MVC](../../../index.md) para buscar otros ASP.NET MVC, tutoriales y ejemplos.


Vamos a hacer nuestra primera aplicación Web ASP.NET MVC usando [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/). Nos aseguraremos de hacer una pequeña aplicación de lista de películas que creará Permítanos y lista de películas.

## <a name="what-youll-build"></a>Lo que vamos a compilar

Presentamos dos capturas de pantalla de la aplicación que compilará. Tendrá una tabla sencilla de películas con varias columnas.

[![Lista de películas - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

Y tendrá una forma de crear por lo que podemos agregar a la lista de películas.

[![Crear una película - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>Obtendrá información de conocimientos

Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Visual Studio. Aprenderá a:

- Cómo crear un nuevo proyecto de MVC de ASP.NET
- Cómo crear una nueva base de datos con SQL Server
- Cómo crear controladores de MVC de ASP.NET y vistas
- Cómo recuperar y mostrar datos
- Cómo modificar datos y habilitar la validación de datos
- Cómo actualizar el esquema de base de datos

## <a name="get-started"></a>Primeros pasos

Comience ejecutando Visual Web Developer 2010 Express (llamaré "VWD" de ahora en adelante) y seleccione Nuevo proyecto de la pantalla de inicio.

Visual Web Developer es un entorno de desarrollador integrado o IDE. Al igual que usa Microsoft Word para escribir documentos, deberá usar un IDE para crear aplicaciones. Hay una barra de herramientas en la parte superior que muestra distintas opciones disponibles para, así como el menú también podría haber utilizado para seleccionar archivo | Nuevo proyecto.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>Crear su primera aplicación

Puede crear aplicaciones mediante Visual Basic o Visual C#. Por ahora, seleccione Visual C# a la izquierda, a continuación, seleccione "Aplicación Web de ASP.NET MVC 2." Denomine el proyecto "Películas" y haga clic en Aceptar.

[![Nuevo proyecto](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

En el lado derecho es el Explorador de soluciones que muestra todos los archivos y carpetas en la aplicación. La ventana grande en la parte central es donde se edite el código y pasan la mayor parte del tiempo. Visual Studio usa una plantilla predeterminada para el proyecto de MVC de ASP.NET que acaba de crear, por lo que tendrá una aplicación que funciona en este momento sin hacer nada. Se trata de un simple "Hola a todos proyecto y es un buen punto de partida para nuestra aplicación.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

Seleccione el botón "reproducir" en la barra de herramientas.

![Iniciar depuración](getting-started-with-mvc-part1/_static/image11.png)

Es una flecha verde hacia la derecha que se compila el programa e iniciar la aplicación en un explorador web.

*Nota: Puede en su lugar, presione F5 en el teclado, o seleccione depuración -&gt;iniciar la depuración desde el menú "Debug".*

Esto hará que Visual Web Developer iniciar un servidor web de desarrollo y ejecutar la aplicación web (no hay ninguna configuración ni pasos manuales necesarios para habilitar esta opción). A continuación, se inicie un explorador y configurarlo para examinar la página principal de la aplicación. Tenga en cuenta que a continuación que la barra de direcciones del explorador dice "localhost" y no algo parecido a ejemplo.com. Eso es porque localhost siempre apunta a su propio equipo local - que en este caso, se ejecuta la aplicación que se acaba de compilar.

[![Página principal](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

De fábrica esta plantilla predeterminada proporciona dos páginas para visitar y una página de inicio de sesión básica. Vamos a cambiar el funcionamiento de esta aplicación y aprender un poco sobre ASP.NET MVC en el proceso. Cierre el explorador y permite cambiar el código.

> [!div class="step-by-step"]
> [Siguiente](getting-started-with-mvc-part2.md)
