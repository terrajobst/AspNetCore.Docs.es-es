---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Parte 8: Páginas finales, control de excepciones y conclusión | Documentos de Microsoft'
author: JoeStagner
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Tailspin Spyworks. Parte 8 agrega una página de contacto, acerca de la página y la excepción...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: f82294aab0616012393cf3e10f932f6d1ad0cdb6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886276"
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a>Parte 8: Páginas finales, control de excepciones y conclusión
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks muestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET. Muestra cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluidos los de la compra, la desprotección y la administración.
> 
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Tailspin Spyworks. Parte 8 agrega una página de contacto, acerca de la página y el control de excepciones. Ésta es la conclusión de la serie.


## <a id="_Toc260221680"></a>  Póngase en contacto con la página (envío de correo electrónico de ASP.NET)

Crear una nueva página denominada ContactUs.aspx

Mediante el diseñador, cree el siguiente formulario Anotar especial para incluir el ToolkitScriptManager y el control del Editor de la AjaxdControlToolkit. .

![](tailspin-spyworks-part-8/_static/image1.jpg)

Haga doble clic en el botón "Enviar" para generar un controlador de eventos de clic en el archivo de código subyacente e implemente un método para enviar la información de contacto como un correo electrónico.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

Este código requiere que el archivo web.config contiene una entrada en la sección de configuración que especifica el servidor SMTP que se utilizará para enviar correo.

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>  Acerca de la página

Cree una página denominada AboutUs.aspx y agregue cualquier contenido que desee.

## <a id="_Toc260221682"></a>  Controlador de excepciones global

Por último, a lo largo de la aplicación hemos produce excepciones y circunstancias imprevistas inactivos que también hay excepciones no controlada de causa en nuestra aplicación web.

No queremos nunca una excepción no controlada que se mostrará a los visitantes del sitio web.

![](tailspin-spyworks-part-8/_static/image2.jpg)

Aparte de ser una experiencia de usuario terribles las excepciones no controladas también pueden ser un problema de seguridad.

Para solucionar este problema implementamos un controlador de excepciones global.

Para ello, abra el archivo Global.asax y tenga en cuenta el siguiente controlador de eventos generados previamente.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

Agregue código para implementar la aplicación\_controlador de errores como se indica a continuación.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

A continuación, agregue una página denominada Error.aspx a la solución y agregue este fragmento de código de marcado.

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

Ahora, en la página\_extracción de controlador de eventos mensajes de error de carga del objeto Request.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>  Conclusión

Hemos visto que ASP.NET WebForms facilita el proceso para crear un sitio Web sofisticado con acceso de base de datos de pertenencia, AJAX, etcetera. muy rápidamente.

Espero que este tutorial le ha concedido las herramientas que necesita para empezar a compilar sus propios formularios Web Forms de ASP.NET en las aplicaciones.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-7.md)
