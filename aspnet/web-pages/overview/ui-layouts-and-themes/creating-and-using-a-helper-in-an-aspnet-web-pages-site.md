---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Crear y usar una aplicación auxiliar en ASP.NET Web Pages (Razor) sitio | Microsoft Docs
author: tfitzmac
description: En este artículo se describe cómo crear una aplicación auxiliar en un sitio Web de ASP.NET Web Pages (Razor). Una aplicación auxiliar es un componente reutilizable que incluye el código y el marcado para rendimiento...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 5e73beabf973a20335112a0d7dc5ecb6b8817744
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394617"
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a>Crear y usar una aplicación auxiliar en un sitio ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se describe cómo crear una aplicación auxiliar en un sitio Web de ASP.NET Web Pages (Razor). Un *auxiliar* es un componente reutilizable que incluye el código y marcado para realizar una tarea que puede ser complejo o una tarea tediosa.
> 
> **Lo que aprenderá:** 
> 
> - Cómo crear y usar un complemento simple.
> 
> Estas son las características ASP.NET incorporadas en el artículo:
> 
> - El `@helper` sintaxis.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2.


## <a name="overview-of-helpers"></a>Información general de las aplicaciones auxiliares

Si tiene que realizar las mismas tareas en distintas páginas del sitio, puede usar una aplicación auxiliar. Las páginas Web ASP.NET incluye una serie de aplicaciones auxiliares, y hay muchas más que puede descargar e instalar. (Se muestra una lista de las aplicaciones auxiliares integradas en ASP.NET Web Pages en el [referencia rápida de la API de ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202907).) Si ninguna de las aplicaciones auxiliares existentes satisfacen sus necesidades, puede crear su propia aplicación auxiliar.

Una aplicación auxiliar le permite usar un bloque de código común en varias páginas. Suponga que en la página a menudo desea crear un elemento de la nota que se distinguen los párrafos normales. Quizás la nota se crea como un `<div>` elemento que haya un estilo como un cuadro con un borde. En lugar de agregar este mismo marcado para una página cada vez que desea mostrar una nota, puede empaquetar el marcado como una aplicación auxiliar. A continuación, puede insertar la nota con una sola línea de código en cualquier lugar necesita.

Usar una aplicación auxiliar así hace que el código en cada una de las páginas más sencillo y más fácil de leer. También resulta más fácil de mantener su sitio, porque si necesita cambiar el aspecto de las notas, puede cambiar el marcado en un solo lugar.

## <a name="creating-a-helper"></a>Creación de una aplicación auxiliar

Este procedimiento muestra cómo crear la aplicación auxiliar que crea la nota, como acabamos de describir. Este es un ejemplo sencillo, pero la aplicación auxiliar personalizada puede incluir cualquier marcado y código ASP.NET que necesita.

1. En la carpeta raíz del sitio Web, cree una carpeta denominada *aplicación\_código*. Esto es un nombre de carpeta reservado en ASP.NET donde puede colocar el código para los componentes como aplicaciones auxiliares.
2. En el *aplicación\_código* crear una nueva carpeta *.cshtml* de archivo y asígnele el nombre *MyHelpers.cshtml*.
3. Reemplace el contenido existente por lo siguiente:

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    El código usa el `@helper` sintaxis para declarar un nueva aplicación auxiliar denominado `MakeNote`. Esta aplicación auxiliar determinada le permite pasar un parámetro denominado `content` que puede contener una combinación de texto y marcado. La aplicación auxiliar inserta la cadena en el cuerpo de la nota mediante el `@content` variable.

    Tenga en cuenta que el archivo se denomina *MyHelpers.cshtml*, pero la aplicación auxiliar se denomina `MakeNote`. Puede colocar varias de las aplicaciones auxiliares personalizadas en un único archivo.
4. Guarde y cierre el archivo.

## <a name="using-the-helper-in-a-page"></a>Uso de la aplicación auxiliar en una página

1. En la carpeta raíz, cree un nuevo archivo en blanco denominado *TestHelper.cshtml*.
2. Agregue el código siguiente al archivo:

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    Para llamar a la aplicación auxiliar que creó, use `@` seguido por el nombre de archivo donde es la aplicación auxiliar, un punto y, a continuación, el nombre de la aplicación auxiliar. (Si tiene varias carpetas en el *aplicación\_código* carpeta, puede usar la sintaxis `@FolderName.FileName.HelperName` para llamar a esta persona en cualquier anidado nivel de carpeta). El texto que agregue comillas entre paréntesis es el texto que se mostrará la aplicación auxiliar como parte de la nota en la página web.
3. Guarde la página y ejecútelo en un explorador. La aplicación auxiliar genera directamente el elemento tenga en cuenta que se llamó a la aplicación auxiliar: entre los dos párrafos.

    ![Captura de pantalla que muestra la página en el explorador y cómo la aplicación auxiliar genera marcado que coloca un cuadro alrededor del texto especificado.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a>Recursos adicionales


[Menú horizontal como una aplicación auxiliar de Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341). Por Mike Pope esta entrada de blog muestra cómo crear un menú horizontal como una aplicación auxiliar con marcado, CSS y código.

[Aprovechar HTML5 en ASP.NET Web Pages aplicaciones auxiliares de WebMatrix y ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx). Esta entrada de blog por Sam Abraham muestra una aplicación auxiliar que representa un HTML5 `Canvas` elemento.

[La diferencia entre @Helpers y @Functions en WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix). Se describe en este artículo blog de Mike Brind `@helper` sintaxis y `@function` sintaxis y cuándo usar cada uno.
