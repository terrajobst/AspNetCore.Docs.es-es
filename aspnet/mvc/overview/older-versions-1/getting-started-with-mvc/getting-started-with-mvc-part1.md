---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Introducción a ASP.NET MVC | Microsoft Docs
author: shanselman
description: Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Cree una aplicación web simple que lee y escribe desde una base de datos.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 3dda4701c9be90bea7c95a330845d601345ac46b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829699"
---
<a name="intro-to-aspnet-mvc"></a>Introducción a ASP.NET MVC
====================
por [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Una versión actualizada, si está disponible en este tutorial [aquí](../../getting-started/introduction/getting-started.md) mediante [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). El nuevo tutorial usa ASP.NET MVC 5, que proporciona muchas mejoras con respecto a este tutorial.
> 
> 
> Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Creará una aplicación web simple que lee y escribe desde una base de datos. Visite el [centro de aprendizaje de ASP.NET MVC](../../../index.md) para buscar otros ASP.NET MVC, tutoriales y ejemplos.


Vamos a crear nuestra primera aplicación Web ASP.NET MVC usando [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/). Nos aseguraremos de hacer una pequeña aplicación de lista de películas que nos gustaría crear y lista de películas.

## <a name="what-youll-build"></a>¿Qué va a crear

Estos son dos capturas de pantalla de la aplicación que se va a crear. Tendrá una sencilla tabla de películas con varias columnas.

[![Lista de películas - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

Y tendrá un formulario de creación, por lo que podemos agregar a la lista de películas.

[![Crear una película - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>Habilidades que aprenderá

Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Visual Studio. Aprenderá lo siguiente:

- Cómo crear un nuevo proyecto de MVC de ASP.NET
- Cómo crear una nueva base de datos con SQL Server
- Cómo crear vistas y controladores de MVC de ASP.NET
- Cómo recuperar y mostrar datos
- Cómo editar los datos y habilitar la validación de datos
- Cómo actualizar el esquema de base de datos

## <a name="get-started"></a>Primeros pasos

Comience ejecutando Visual Web Developer 2010 Express (yo lo llamaré "VWD" de ahora en adelante) y seleccione Nuevo proyecto desde la pantalla de inicio.

Visual Web Developer es un IDE o entorno de desarrollo integrado. Al igual que utiliza Microsoft Word para escribir documentos, usará un IDE para crear aplicaciones. Hay una barra de herramientas en la parte superior que muestra diversas opciones disponibles para, así como el menú también podría haber utilizado para seleccionar archivo | Nuevo proyecto.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>Crear su primera aplicación

Puede crear aplicaciones con Visual Basic o Visual C#. Por ahora, seleccione Visual C# a la izquierda y, a continuación, elija "Aplicación Web de ASP.NET MVC 2." Nombre del proyecto "Movies" y haga clic en Aceptar.

[![Nuevo proyecto](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

En el lado derecho es el Explorador de soluciones que muestra todos los archivos y carpetas en la aplicación. La ventana grande en la parte central es donde se edite el código y pasan la mayor parte del tiempo. Visual Studio usa una plantilla predeterminada para el proyecto de ASP.NET MVC que acaba de crear, por lo que tiene ahora una aplicación de trabajo sin hacer nada! Se trata de un simple "Hola a todos proyecto y es un buen punto de partida para nuestra aplicación.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

Seleccione el botón "reproducir" para la barra de herramientas.

![Iniciar depuración](getting-started-with-mvc-part1/_static/image11.png)

Es una flecha verde hacia la derecha que se compila el programa e iniciar la aplicación en un explorador web.

*Nota: Puede en su lugar, presione F5 en el teclado, o seleccionar depurar -&gt;iniciar la depuración desde el menú "Depurar".*

Esto hará que Visual Web Developer iniciar un servidor web de desarrollo y ejecutar la aplicación web (no hay ninguna configuración o pasos manuales necesarios para habilitar esta opción). A continuación, se ejecutará un explorador y configurarlo para que examine la página principal de la aplicación. Tenga en cuenta que a continuación que la barra de direcciones del explorador dice "localhost" y no algo como ejemplo.com. Eso es porque localhost siempre apunta a su propio equipo local - que en este caso, se ejecuta la aplicación que acaba de crear.

[![Página principal](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

Fuera de la casilla de esta plantilla predeterminada proporciona dos páginas para visitar y una página de inicio de sesión básica. Vamos a cambiar el funcionamiento de esta aplicación y aprender un poco sobre ASP.NET MVC en el proceso. Cierre el explorador y permite cambiar parte del código.

> [!div class="step-by-step"]
> [Siguiente](getting-started-with-mvc-part2.md)
