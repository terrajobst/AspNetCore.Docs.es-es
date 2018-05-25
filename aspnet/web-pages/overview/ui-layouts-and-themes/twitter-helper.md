---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Aplicación auxiliar con ASP.NET Web Pages de Twitter | Documentos de Microsoft
author: tfitzmac
description: Este tema y la aplicación muestran cómo agregar una aplicación auxiliar de Twitter para el proyecto de 3 de WebMatrix. Contiene el código de aplicación auxiliar de Twitter y muestra cómo llamar a la aplicación auxiliar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 07d9c4d485c42b78a42c54c9740af5f67cb44763
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2018
---
<a name="twitter-helper-with-aspnet-web-pages"></a>Aplicación auxiliar de Twitter con páginas Web ASP.NET
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tema y la aplicación muestran cómo agregar una aplicación auxiliar de Twitter para el proyecto de 3 de WebMatrix. Contiene el código de aplicación auxiliar de Twitter y muestra cómo llamar a los métodos auxiliares.
> 
> Este código para el archivo Twitter.cshtml fue desarrollado por **Tian Pan** de Microsoft.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2.


## <a name="introduction"></a>Introducción

Este tema muestra cómo agregar una aplicación auxiliar de Twitter para la aplicación y utilizar la sintaxis de Razor para llamar a los métodos de aplicación auxiliar. La aplicación auxiliar de Twitter facilita la incorporar Twitter botones y los widgets de la aplicación. Para utilizar un widget de Twitter, por ejemplo, la escala de tiempo de un usuario o los resultados de búsqueda de una hashtag, primero debe crear el [widget en Twitter](https://twitter.com/settings/widgets). Después de crear el widget, recibirá un identificador de widget. Pasar este Id. de widget como un parámetro al llamar a los métodos auxiliares que muestran el widget.

En este tema se escribió para la versión 1.1 de la API de Twitter. Agregar directamente el código de aplicación auxiliar de Twitter a su proyecto, puede actualizar el código auxiliar si cambia de la API de Twitter.

Para obtener información acerca de cómo instalar WebMatrix, consulte [Introducción a ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).

## <a name="add-twitter-helper-to-your-project"></a>Agregar aplicación auxiliar de Twitter al proyecto

Para agregar la aplicación auxiliar de Twitter, en primer lugar, agregue una carpeta denominada **aplicación\_código** al proyecto. A continuación, cree un archivo denominado **Twitter.cshtml**.

![Carpeta App_Code](twitter-helper/_static/image1.png)

Reemplace el código predeterminado en Twitter.cshtml con el código siguiente.

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>Llamar a métodos de Twitter desde las páginas web

En el ejemplo siguiente se muestra cómo usar los métodos de aplicación auxiliar de Twitter de una página en el proyecto. En el proyecto, desea reemplazar los valores de parámetro con valores que son relevantes para sus necesidades. Puede usar los identificadores de widget proporcionado para explorar cómo funcionan los métodos, pero desea generar sus propios widgets para el proyecto.

No todos los parámetros que se muestra a continuación son necesarios. Los parámetros opcionales se utilizan para personalizar cómo se muestra el botón o el widget. Por ejemplo, el botón siga solo requiere el nombre de usuario debe seguir, pero en el ejemplo se muestra cómo incluir el número de seguidores de y cómo especificar el tamaño del botón y el idioma.

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>Ver los resultados

El código anterior genera los siguientes botones y widgets. Estos botones y widgets son totalmente funcionales, no capturas de pantalla. Se muestra el botón siga en español porque el parámetro de idioma se estableció en **es**.

### <a name="follow-button"></a>Siga el botón

[Siga @aspnet)](https://twitter.com/aspnet)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>

### <a name="tweet-button"></a>Botón tweet

[Tweet](https://twitter.com/share)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>

### <a name="user-timeline-profile"></a>Escala de tiempo de usuario (perfil)

[TWEETS por @aspnet](https://twitter.com/aspnet)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="favorites"></a>Favoritos

[Favoritos Tweets por @Microsoft](https://twitter.com/Microsoft/favorites)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="list"></a>Lista

[TWEETS de @Microsoft/MS \_consumidor\_bandas](https://twitter.com/microsoft/ms-consumer-brands/)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="search"></a>Buscar

[Tuits acerca de &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>
