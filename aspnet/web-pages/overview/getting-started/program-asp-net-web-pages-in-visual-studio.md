---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Programación de ASP.NET Web Pages (Razor) en Visual Studio | Documentos de Microsoft
author: tfitzmac
description: Este apéndice explica cómo puede usar Visual Studio 2010 o Visual Web Developer 2010 Express al programa ASP.NET Web Pages con la sintaxis de Razor.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2014
ms.topic: article
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: eb17c8cc1fab5b552c8495e74bb86ae9dbc5b972
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896592"
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a>Programar páginas Web ASP.NET (Razor) con Visual Studio
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artículo explica cómo puede usar Visual Studio o Visual Web Developer Express al programa de sitios Web de ASP.NET Web Pages (Razor).
> 
> Lo que aprenderá
> 
> - Lo que necesita instalar (si hay alguna) para que funcione con ASP.NET Web Pages en su versión de Visual Studio.
> - Cómo agregar compatibilidad para ASP.NET Web Pages para Visual Web Developer 2010 Express.
> - Cómo usar características de Visual Studio para trabajar con páginas de ASP.NET Razor, incluyendo IntelliSense y el depurador.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
> - 3 de WebMatrix
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010 y WebMatrix 2.


Puede programar las páginas Web ASP.NET con sintaxis Razor con WebMatrix o muchos otros editores de código. También puede utilizar Microsoft Visual Studio, que es un entorno completo de desarrollo integrado (IDE) que proporciona un conjunto eficaz de herramientas para crear muchos tipos de aplicaciones (no solo sitios Web). Para trabajar con páginas de ASP.NET Razor, puede usar una de las ediciones completas de Visual Studio o gratuitamente [Visual Studio Express para Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) edition.

Dos características especialmente útiles que proporciona Visual Studio para programar con páginas web de ASP.NET Razor son:

- *IntelliSense*. La característica IntelliSense integrada en Visual Studio es una lista más completa de IntelliSense en WebMatrix.
- *Depurador*. El depurador le permite solucionar problemas del código mediante la detención de un programa mientras se está ejecutando, examinar las variables y recorrer el código línea por línea.

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a>Con Visual Studio con distintas versiones de las páginas Web ASP.NET

Visual Studio 2012 y Visual Studio 2013 incluyen compatibilidad para ASP.NET Web Pages. (Los paquetes necesarios para admitir las páginas Web ASP.NET se instalan al instalar Visual Studio).

Visual Studio 2010 no incluye compatibilidad predeterminada para ASP.NET Web Pages. Para usar las páginas Web ASP.NET con Visual Studio 2010, debe instalar el paquete de ASP.NET MVC. Para obtener ASP.NET Web Pages 2, instale ASP.NET MVC 4.

En la tabla siguiente resume la compatibilidad para ASP.NET Web Pages en diferentes versiones de Visual Studio.

|  | Visual Studio 2010 | Visual Studio 2012 | Visual Studio 2013 |
| --- | --- | --- | --- |
| **ASP.NET Web Pages 2** | Instalar ASP.NET MVC 4 | (Incluidos) | (Incluidos) |
| **3 de ASP.NET Web Pages** |  | Actualización a ASP.NET Web Pages 3 NuGet a través | (Incluidos) |

Para trabajar con Visual Studio 2010, vea [instalar compatibilidad con para ASP.NET Web Pages en Visual Studio 2010](#vs2010support).

## <a name="launching-visual-studio-from-webmatrix"></a>Inicie Visual Studio desde WebMatrix

Si ha iniciado un proyecto en WebMatrix y desea cambiar a Visual Studio, WebMatrix proporciona un botón para abrir fácilmente el proyecto en Visual Studio. Debe tener instalado Visual Studio en el equipo para que este botón esté habilitado. La siguiente imagen muestra el botón de WebMatrix.

![launch Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

Al hacer clic en el botón, se abre el proyecto en Visual Studio. Puede alternar entre WebMatrix y Visual Studio sin problemas. Se le notificará si los archivos han cambiado en otro entorno y necesitan volver a cargarse para obtener los cambios más recientes.

## <a name="creating-aspnet-razor-site-in-visual-studio"></a>Crear el sitio de ASP.NET Razor en Visual Studio

Para crear un sitio Web de ASP.NET Razor en Visual Studio:

1. Inicie Visual Studio o Visual Web Developer.
2. En el **archivo** menú, haga clic en **nuevo sitio Web**.

    ![Crear nuevo sitio web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. En el **nuevo sitio Web** diálogo cuadro, seleccione el idioma que se va a usar (Visual C# o Visual Basic).
4. Seleccione el **sitio Web de ASP.NET (Razor)** plantilla.

    ![sitio de Razor](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. Haga clic en **Aceptar**.

El nuevo proyecto existe y se rellena con algunas páginas web de manera predeterminada que le ayudarán a empezar a trabajar.

### <a name="using-intellisense"></a>Using IntelliSense

Ahora que ha creado un sitio, puede ver cómo funciona IntelliSense en Visual Studio.

1. En el sitio Web que acaba de crear, abrir el *Default.cshtml* página.
2. Después de la `<h3>` etiquetas en la página, escriba `@ServerInfo.` (incluido el punto). Tenga en cuenta cómo IntelliSense muestra los métodos disponibles para la `ServerInfo` auxiliar en una lista desplegable. 

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. Seleccione el `GetHtml` método en la lista y, a continuación, presione ENTRAR. IntelliSense se rellena automáticamente en el método. (Como con cualquier método en C#, debe agregar `()` caracteres después del método.)  
   El código completo para el `GetHtml` método es similar al ejemplo siguiente:  

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. Presione Ctrl + F5 para ejecutar la página. Esto es lo que la página es similar al que se muestran en un explorador: 

    ![página predeterminada en el explorador](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. Cierre el explorador.

### <a name="using-the-debugger"></a>Con el depurador

1. En la parte superior de la *Default.cshtml* página después de la línea que comienza con `Page.Title`, agregue la siguiente línea de código: 

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. En el margen gris del editor a la izquierda del código, haga clic en junto a esta nueva línea con el fin de agregar un *punto de interrupción*. Un punto de interrupción es un marcador que indica al depurador que deje de ejecutarse en ese momento el programa para que pueda ver lo que está sucediendo.

    ![Punto de interrupción establecido](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. Quite la llamada a la `ServerInfo.GetHtml` método y agregue una llamada a la `@myTime` variable en su lugar. Esta llamada muestra el valor de tiempo actual devuelto por la nueva línea de código.
4. Presione F5 para ejecutar la página en el depurador. La página se detiene en el punto de interrupción que estableció. La siguiente imagen muestra la página de aspecto en el editor con el punto de interrupción (amarillo). 

    ![punto de interrupción de depuración](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. En la barra de herramientas de depuración, haga clic en el **paso a paso** botón (o presione F11) para que se ejecute la siguiente línea de código. Cada vez que haga clic en este botón para avanzar la ejecución a la siguiente línea de código.

    ![Paso a paso en el botón](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. Examine el valor de la `myTime` variable, mantenga el puntero del mouse sobre él o mediante la inspección de los valores mostrados en la **locales** y **pila de llamadas** windows. Visual Studio muestra el valor de la variable.

    ![valor de mostrar la hora](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. Cuando haya terminado de examinar la variable y recorrer el código, presione F5 para continuar la ejecución de la página sin detenerse en cada línea. Cuando haya terminado la ejecución paso a paso a través de todo el código, el explorador muestra la página.

Para obtener más información acerca del depurador y cómo depurar código en Visual Studio, vea [Tutorial: depurar páginas Web en Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a>Uso de Razor en proyectos de MVC de ASP.NET con Visual Studio

La sintaxis de Razor también se usa ampliamente en los proyectos de ASP.NET MVC. MVC es una manera eficaz, basada en modelos para crear sitios Web dinámicos. Si el sitio de ASP.NET Web Pages resulta difícil de mantener, puede considerar la conversión a una aplicación de ASP.NET MVC. Para obtener un ejemplo de creación de una aplicación de MVC, vea [Introducción a ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a>Instalar compatibilidad para las páginas Web ASP.NET en Visual Studio 2010

Esta sección muestra cómo instalar Visual Web Developer Express 2010 y las herramientas de páginas Web ASP.NET para Visual Studio.

1. Si ya no tiene el instalador de plataforma Web, descárguelo desde la dirección URL siguiente:

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. Ejecute al instalador de plataforma Web.
3. Haga clic en el **productos** ficha.

    ![Pestaña WebPI productos](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. Busque **ASP.NET MVC 4** (para ASP.NET Web Pages 2) y, a continuación, haga clic en **agregar**. Estos productos incluyen herramientas de Visual Studio para crear sitios Web de ASP.NET Razor.

    ![Opciones de instalación de WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. Haga clic en **instalar** para completar la instalación.
