---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Parte 8: Páginas finales, control de excepciones y conclusión | Microsoft Docs'
author: JoeStagner
description: Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks. Parte 8 agrega una página de contacto, acerca de la página y la excepción...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 3b49ee53e82933de9b50960779c28ca6ab7441e5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828988"
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a>Parte 8: Páginas finales, control de excepciones y conclusión
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks demuestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET. Resalta cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluida la compra, la desprotección y la administración.
> 
> Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks. Parte 8 agrega una página de contacto, acerca de la página y control de excepciones. Ésta es la conclusión de la serie.


## <a id="_Toc260221680"></a>  Póngase en contacto con la página (envío de correo electrónico desde ASP.NET)

Crear una nueva página denominada ContactUs.aspx

Con el diseñador, cree el siguiente formulario toma nota especial para incluir el ToolkitScriptManager y el control del Editor de la AjaxdControlToolkit. .

![](tailspin-spyworks-part-8/_static/image1.jpg)

Haga doble clic en el botón "Enviar" para generar un controlador de eventos de clic en el archivo de código subyacente e implementar un método para enviar la información de contacto como un correo electrónico.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

Este código requiere que el archivo web.config contiene una entrada en la sección de configuración que especifica el servidor SMTP para enviar correo electrónico.

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>  Acerca de la página

Cree una página denominada AboutUs.aspx y agregue cualquier contenido que desee.

## <a id="_Toc260221682"></a>  Controlador de excepciones global

Por último, en toda la aplicación se ha iniciado la excepción y hay una circunstancia imprevista en frío es también las excepciones no controlada de causa en nuestra aplicación web.

No queremos nunca una excepción no controlada que se mostrará para un visitante del sitio web.

![](tailspin-spyworks-part-8/_static/image2.jpg)

Aparte de ser una experiencia de usuario terrible las excepciones no controladas también pueden ser un problema de seguridad.

Para solucionar este problema implementamos un controlador de excepciones global.

Para ello, abra el archivo Global.asax y tenga en cuenta el siguiente controlador de eventos generados previamente.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

Agregue código para implementar la aplicación\_controlador de errores como se indica a continuación.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

A continuación, agregue una página denominada Error.aspx a la solución y agregue este fragmento de código de marcado.

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

Ahora en la página\_extracción del controlador de eventos los mensajes de error de carga del objeto Request.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>  Conclusión

Hemos visto que ASP.NET WebForms facilita la tarea para crear un sitio Web sofisticado con acceso de base de datos, suscripciones, AJAX, etcetera. muy rápidamente.

Espero que este tutorial le haya proporcionado las herramientas que necesita para empezar a crear su propio ASP.NET WebForms aplicaciones!

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-7.md)
