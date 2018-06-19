---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Crear y usar una aplicación auxiliar en un ASP.NET Web Pages (Razor) sitio | Documentos de Microsoft
author: tfitzmac
description: En este artículo se describe cómo crear una aplicación auxiliar en un sitio Web de ASP.NET Web Pages (Razor). Una aplicación auxiliar es un componente reutilizable que incluye código y marcado en rendimiento...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 5d0c1ae09d8fbc91ff76cd4045d439abafee7736
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530004"
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a>Crear y usar una aplicación auxiliar en un sitio ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se describe cómo crear una aplicación auxiliar en un sitio Web de ASP.NET Web Pages (Razor). A *auxiliar* es un componente reutilizable que incluye código y marcado para realizar una tarea que podría ser una tarea tediosa o complejas.
> 
> **Lo que aprenderá:** 
> 
> - Cómo crear y usar un complemento sencillo.
> 
> Estas son las características ASP.NET presentadas en el artículo:
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

Si tiene que realizar las mismas tareas en distintas páginas de su sitio, puede usar una aplicación auxiliar. Páginas Web de ASP.NET incluye una serie de aplicaciones auxiliares, y hay muchos más que puede descargar e instalar. (Se muestra una lista de las aplicaciones auxiliares integradas en las páginas Web ASP.NET en el [referencia rápida de la API de ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202907).) Si ninguna de las aplicaciones auxiliares existentes satisface sus necesidades, puede crear su propia aplicación auxiliar.

Una aplicación auxiliar le permite usar un bloque de código común en varias páginas. Suponga que en la página a menudo desea crear un elemento de la nota que se diferencian a párrafos normales. Quizás la nota se crea como un `<div>` elemento que ha con estilo como un cuadro con un borde. En lugar de agregar este mismo marcado a una página cada vez que desea que aparezca una nota, puede empaquetar el marcado como una aplicación auxiliar. A continuación, puede insertar la nota con una sola línea de código en cualquier lugar necesita.

Utilizar una aplicación auxiliar parecido a esto hace que el código en cada una de las páginas más sencillo y más fácil de leer. También resulta más fácil de mantener su sitio Web, porque si necesita cambiar el aspecto de las notas, puede cambiar el marcado en un solo lugar.

## <a name="creating-a-helper"></a>Crear una aplicación auxiliar

Este procedimiento muestra cómo crear la aplicación auxiliar que crea la nota, según se ha descrito. Esto es un ejemplo sencillo, pero la aplicación auxiliar personalizada puede incluir cualquier marcado y código ASP.NET que se necesita.

1. En la carpeta raíz del sitio Web, cree una carpeta denominada *aplicación\_código*. Esto es un nombre de carpeta reservado en ASP.NET donde se puede colocar el código para los componentes como aplicaciones auxiliares.
2. En el *aplicación\_código* crear una nueva carpeta *.cshtml* archivo y asígnele el nombre *MyHelpers.cshtml*.
3. Reemplace el contenido existente con lo siguiente:

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    El código usa el `@helper` sintaxis para declarar una nuevo auxiliar denominada `MakeNote`. Esta aplicación auxiliar determinada permite pasar un parámetro denominado `content` que puede contener una combinación de texto y marcado. La aplicación auxiliar inserta la cadena en el cuerpo de la nota mediante el `@content` variable.

    Observe que el archivo se denomina *MyHelpers.cshtml*, pero la aplicación auxiliar se denomina `MakeNote`. Puede colocar varias aplicaciones auxiliares personalizadas en un único archivo.
4. Guarde y cierre el archivo.

## <a name="using-the-helper-in-a-page"></a>Utilizar la aplicación auxiliar en una página

1. En la carpeta raíz, cree un nuevo archivo en blanco llamado *TestHelper.cshtml*.
2. Agregue el código siguiente al archivo:

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    Para llamar a la aplicación auxiliar que se ha creado, use `@` seguido por el nombre de archivo donde es la aplicación auxiliar, un punto y, a continuación, el nombre de aplicación auxiliar. (Si había varias carpetas el *aplicación\_código* carpeta, puede usar la sintaxis `@FolderName.FileName.HelperName` llamar a esta persona dentro de cualquier anidado el nivel de carpeta). El texto que se agrega comillas dentro de los paréntesis es el texto que se mostrará la aplicación auxiliar como parte de la nota en la página web.
3. Guarde la página y ejecútelo en un explorador. La aplicación auxiliar genera el elemento de la nota derecha donde se llamó a la aplicación auxiliar: entre los dos párrafos.

    ![Captura de pantalla que muestra la página en el explorador y cómo la aplicación auxiliar genera marcado que coloca un cuadro alrededor del texto especificado.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a>Recursos adicionales


[Menú horizontal como una aplicación auxiliar de Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341). Esta entrada de blog de Mike Pope muestra cómo crear un menú horizontal como un Ayudante que usa marcado, CSS y el código.

[Aprovechamiento de HTML5 en ASP.NET Web Pages aplicaciones auxiliares para WebMatrix y ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx). Esta entrada de blog Sam Abraham muestra una aplicación auxiliar que representa un HTML5 `Canvas` elemento.

[La diferencia entre @Helpers y @Functions en WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix). Describe esta entrada de blog de Mike Brind `@helper` sintaxis y `@function` sintaxis y cuándo usar cada una.
