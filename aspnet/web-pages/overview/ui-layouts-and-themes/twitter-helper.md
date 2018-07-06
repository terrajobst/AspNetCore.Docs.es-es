---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Aplicación auxiliar con ASP.NET Web Pages de Twitter | Microsoft Docs
author: tfitzmac
description: Este tema y la aplicación muestran cómo agregar una aplicación auxiliar de Twitter al proyecto WebMatrix 3. Contiene el código de aplicación auxiliar de Twitter y se muestra cómo llamar a la aplicación auxiliar...
ms.author: aspnetcontent
ms.date: 02/07/2014
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 1824b677a7ba96ea6fc5119610725a30d472764e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817660"
---
<a name="twitter-helper-with-aspnet-web-pages"></a>Aplicación auxiliar de Twitter con ASP.NET Web Pages
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tema y la aplicación muestran cómo agregar una aplicación auxiliar de Twitter al proyecto WebMatrix 3. Contiene el código de aplicación auxiliar de Twitter y se muestra cómo llamar a los métodos auxiliares.
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

En este tema se muestra cómo agregar una aplicación auxiliar de Twitter a la aplicación y usar la sintaxis de Razor para llamar a los métodos auxiliares. La aplicación auxiliar de Twitter facilita incorporar los botones de Twitter y widgets en la aplicación. Para utilizar un widget de Twitter, por ejemplo, la escala de tiempo de un usuario o los resultados de búsqueda para un hashtag, primero debe crear el [widget en Twitter](https://twitter.com/settings/widgets). Después de crear el widget, recibirá un identificador de widget. Pasar el identificador de este widget como un parámetro al llamar a los métodos auxiliares que muestran el widget.

En este tema se escribió para la versión 1.1 de la API de Twitter. Al agregar directamente el código de aplicación auxiliar de Twitter al proyecto, puede actualizar el código auxiliar si cambia de la API de Twitter.

Para obtener información acerca de cómo instalar WebMatrix, consulte [Introducción a ASP.NET Web Pages 2 - Introducción a](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).

## <a name="add-twitter-helper-to-your-project"></a>Agregar la aplicación auxiliar de Twitter al proyecto

Para agregar la aplicación auxiliar de Twitter, en primer lugar, agregue una carpeta denominada **aplicación\_código** al proyecto. A continuación, cree un archivo denominado **Twitter.cshtml**.

![Carpeta App_Code](twitter-helper/_static/image1.png)

Reemplace el código predeterminado en Twitter.cshtml con el código siguiente.

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>Llamar a métodos de Twitter desde las páginas web

El ejemplo siguiente muestra cómo usar los métodos de aplicación auxiliar de Twitter desde una página en el proyecto. En el proyecto, desea reemplazar los valores de parámetro con valores que son relevantes para sus necesidades. Puede usar los identificadores de widget proporcionado para explorar cómo funcionan los métodos, pero desea generar sus propios widgets para el proyecto.

No todos los parámetros mostrados a continuación son necesarios. Los parámetros opcionales se usan para personalizar cómo se muestra el botón o el widget. Por ejemplo, el botón seguir sólo requiere el nombre de usuario para seguir, pero el ejemplo muestra cómo incluir el número de seguidores y cómo especificar el tamaño del botón y el idioma.

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>Ver los resultados

El código anterior genera los siguientes botones y widgets. Estos botones y los widgets son totalmente funcionales, no capturas de pantalla. Se muestra el botón seguir en español porque se estableció el parámetro de idioma en **es**.

### <a name="follow-button"></a>Siga el botón

[Siga @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="tweet-button"></a>Botón de tweet

[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="user-timeline-profile"></a>Escala de tiempo de usuario (perfil)

[TWEETS por @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="favorites"></a>Favoritos

[Tweets favoritas por @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="list"></a>Lista

[TWEETS de @Microsoft/MS \_consumidor\_bandas](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="search"></a>Buscar

[TWEETS sobre &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`
