---
uid: web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
title: Archivo Léame de ASP.NET Web 2 páginas Developer Preview | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 09/14/2011
ms.assetid: 159a92e2-e011-4da7-b61d-2edde2a967da
msc.legacyurl: /web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
msc.type: authoredcontent
ms.openlocfilehash: 0a89216c0f65d49d00c96a27a5ab33e3872f9bc5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818351"
---
<a name="aspnet-web-pages-2-developer-preview-readme"></a>Léame de vista previa de 2 para desarrolladores de ASP.NET Web Pages
====================
por [Microsoft](https://github.com/microsoft)

## <a name="aspnet-web-pages-2-developer-preview-readme"></a>Léame de vista previa de 2 para desarrolladores de ASP.NET Web Pages

14 de septiembre de 2011

### <a name="contents"></a>Contenido

#### <a id="_Toc303701284"></a>  Notas de instalación

Para instalar la versión preliminar para desarrolladores de páginas Web 2, tienes estas opciones:

- WebMatrix 2 Beta se instalan con el [instalador de plataforma Web](https://go.microsoft.com/fwlink/?LinkId=226883). WebMatrix es un conjunto de herramientas de desarrollo web gratuito que incluye páginas Web de ASP.NET. Para obtener más información, consulte la sección de instalación en [principales características de ASP.NET Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

- Instalar Web Pages 2 Developer Preview directamente mediante el [vínculo de descarga](https://go.microsoft.com/fwlink/?LinkID=226335). Utilice este enfoque si desea crear aplicaciones de páginas Web con un texto editor como Bloc de notas. Para ejecutar aplicaciones de Web Pages 2, debe tener IIS Express 7.5. (Esto se incluye automáticamente con WebMatrix). Para obtener sugerencias sobre cómo probar una página de páginas Web mediante IIS Express, consulte la barra lateral "Creación y las pruebas ASP.NET páginas utilizando su propio Editor de texto" en [Introducción a WebMatrix y ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202889).

ASP.NET Web Pages 2 Developer Preview se puede instalarse y se puede ejecutar en paralelo con ASP.NET Web Pages 1. <a id="a"></a>Para obtener más información, vea la sección "Ejecución páginas Web aplicaciones Side-by-Side" en [principales características de Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701285"></a>  Documentación

Tutoriales y otra información sobre ASP.NET Web Pages están disponibles en la página de las páginas Web del sitio Web ASP.NET ([https://www.asp.net/web-pages/](../../index.md)). Para obtener información sobre las nuevas características y mejoras de Web Pages 2, consulte [principales características de Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701286"></a>  Soporte técnico

<a id="_Toc209852135"></a><a id="_Toc255833657"></a> Esto es una versión preliminar y no se admite oficialmente. Si tiene preguntas sobre cómo trabajar con esta versión, publíquelos en el foro de ASP.NET Web Pages ([ https://forums.asp.net/1224.aspx/1?WebMatrix ](https://forums.asp.net/1224.aspx/1?WebMatrix) ), donde los miembros de la Comunidad de ASP.NET son normalmente pueden ofrecer soporte técnico informal.

#### <a id="_Toc303701287"></a>  Requisitos de software

ASP.NET Web Pages 2 requiere .NET Framework 4. También funciona con la versión de vista previa para desarrolladores de .NET Framework 4.5.

<a id="_Toc303701288"></a><a id="_Breaking_Changes"></a>

#### <a name="fixes-known-issues-and-breaking-changes"></a>Correcciones de problemas conocidos y cambios importantes

<a id="_Toc224729061"></a><a id="_Toc238051347"></a>

- **Es\* métodos (por ejemplo, IsDateTime) ahora devuelven valores correctos para todas las referencias culturales.** Algunos métodos como *IsDateTime* previamente devuelta *false* cuando deben haber vuelto *true* dado que anteriormente estaban realizando comprobaciones específicas de la referencia cultural. Estos métodos se han corregido ahora tener referencia cultural en la cuenta. Esto es un cambio brusco; Si la aplicación se basa en el comportamiento anterior, se interrumpirá.
- **Ha cambiado el comportamiento del método Href.** Anteriormente, una llamada a Href("~/SomeFile") devolvería una dirección URL relativa al archivo que se está ejecutando actualmente. Ahora Href("~/SomeFile") siempre devuelve una ruta de acceso absoluta de la raíz de la aplicación. Para la mayoría de los casos, este comportamiento no suponer una diferencia en el valor devuelto. Este cambio se realizó para corregir determinados escenarios Ajax. Por ejemplo, considere el ejemplo de código siguiente: 

    [!code-cshtml[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample1.cshtml)]

    Este código previamente daría como resultado Images/Logo.jpg, que sería correcto para una solicitud Ajax a esa página. Ahora resolverá en la raíz de la (/ MySite/Images/Logo.jpg).
- **Ha cambiado el método HttpContext.RedirectLocal**. Ahora, este método acepta únicamente las direcciones URL que son relativas a la aplicación actual. Nombre completo se rechazan las direcciones URL.
- **El método ModelState.IsValid ahora requiere llamar a Validate primero**. Si va a convertir la aplicación para usar los nuevos métodos de validación de entrada y llama a la *ModelState.IsValid* método, debe llamar ahora *Validation.Validate* con antelación. Por ejemplo, ahora debe seguir este patrón: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample2.cs)]

  Sin embargo, se recomienda que si usa los nuevos métodos de validación de entrada, no use *ModelState.IsValid*. En su lugar, estructurar el código similar al siguiente: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample3.cs)]
- **En Internet Explorer 7 e Internet Explorer 8, la validación del lado cliente no funciona**. La validación del lado cliente no funciona debido a incompatibilidades con jQuery 1.6.2, que se incluye con la plantilla de proyecto predeterminada. (Validación de servidor funciona.).

#### <a id="_Toc303701289"></a>  Declinación de responsabilidades

© 2011 Microsoft Corporation. Todos los derechos reservados. Este documento se proporciona "como-es." Información y las opiniones expresadas en este documento, incluidas las direcciones URL y otras referencias a sitios Web de Internet, pueden cambiar sin previo aviso. Usted asume el riesgo de utilizarla.
