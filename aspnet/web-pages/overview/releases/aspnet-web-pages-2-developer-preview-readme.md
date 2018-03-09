---
uid: web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
title: "Archivo Léame de ASP.NET Web 2 páginas Developer Preview | Documentos de Microsoft"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/14/2011
ms.topic: article
ms.assetid: 159a92e2-e011-4da7-b61d-2edde2a967da
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
msc.type: authoredcontent
ms.openlocfilehash: 119265c62abb3f3110cdc7f0b94a7c9b16b4251c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-pages-2-developer-preview-readme"></a>Léame de vista previa de desarrollador 2 de ASP.NET Web Pages
====================
por [Microsoft](https://github.com/microsoft)

## <a name="aspnet-web-pages-2-developer-preview-readme"></a>Léame de vista previa de desarrollador 2 de ASP.NET Web Pages

14 de septiembre de 2011

### <a name="contents"></a>Contenido

#### <a id="_Toc303701284"></a>Notas sobre la instalación

Para instalar Web Pages 2 Developer Preview, tienes estas opciones:

- Versión Beta 2 de WebMatrix se instalan con el [instalador de plataforma Web](https://go.microsoft.com/fwlink/?LinkId=226883). WebMatrix es un conjunto de herramientas de desarrollo web gratuita que incluye ASP.NET Web Pages. Para obtener más información, consulte la sección de instalación en [características de la parte superior en ASP.NET Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

- Instalar Web Pages 2 Developer Preview directamente mediante el [vínculo de descarga](https://go.microsoft.com/fwlink/?LinkID=226335). Utilice este método si desea crear aplicaciones de páginas Web con un texto editor como el Bloc de notas. Para ejecutar aplicaciones de Web Pages 2, debe tener IIS 7.5 Express. (Esto se incluye automáticamente con WebMatrix). Para obtener sugerencias sobre cómo probar una página de páginas Web mediante IIS Express, consulte la barra lateral "Crear y probar ASP.NET páginas usando su propio Editor de texto" en [Introducción a WebMatrix y ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202889).

ASP.NET Web Pages 2 Developer Preview puede instalarse y se puede ejecutar en paralelo con ASP.NET Web Pages 1. <a id="a"></a>Para obtener más información, vea la sección "Ejecutar páginas Web aplicaciones Side-by-Side" en [características de la parte superior en páginas Web 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701285"></a>Documentación

Tutoriales y otra información sobre ASP.NET Web Pages están disponibles en la página de las páginas Web del sitio Web ASP.NET ([https://www.asp.net/web-pages/](../../index.md)). Para obtener información acerca de nuevas características y mejoras realizadas en páginas Web 2, vea [características de la parte superior en páginas Web 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701286"></a>Soporte técnico

<a id="_Toc209852135"></a><a id="_Toc255833657"></a>Esto es una versión preliminar y no se admite oficialmente. Si tiene alguna pregunta sobre cómo trabajar con esta versión, registrarlas en el foro de ASP.NET Web Pages ([https://forums.asp.net/1224.aspx/1?WebMatrix](https://forums.asp.net/1224.aspx/1?WebMatrix) ), donde los miembros de la Comunidad de ASP.NET son normalmente pueden ofrecer soporte técnico informal.

#### <a id="_Toc303701287"></a>Requisitos de software

ASP.NET Web Pages 2 requiere .NET Framework 4. También funciona con la versión de vista previa de desarrollador de .NET Framework 4.5.

<a id="_Toc303701288"></a><a id="_Breaking_Changes"></a>

#### <a name="fixes-known-issues-and-breaking-changes"></a>Correcciones, problemas conocidos y los cambios recientes

<a id="_Toc224729061"></a><a id="_Toc238051347"></a>

- **Es\* métodos (por ejemplo, IsDateTime) ahora devuelven valores correctos para todas las referencias culturales.** Algunos métodos como *IsDateTime* devuelto anteriormente *false* cuando se deben devolver *true* dado que anteriormente estaban realizando comprobaciones específicas de la referencia cultural. Estos métodos se han corregido para aprovechar ahora la referencia cultural en cuenta. Se trata de un cambio brusco; Si la aplicación se basa en el comportamiento anterior, se interrumpirá.
- **El comportamiento del método Href ha cambiado.** Anteriormente, una llamada a Href("~/SomeFile") devolvería una dirección URL relativa al archivo que se está ejecutando actualmente. Ahora Href("~/SomeFile") siempre devuelve una ruta de acceso absoluta de la raíz de la aplicación. Para la mayoría de los casos, este comportamiento no suponer una diferencia en el valor devuelto. Este cambio se realizó corregir ciertos escenarios Ajax. Por ejemplo, considere el ejemplo de código siguiente: 

    [!code-cshtml[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample1.cshtml)]

    Este código previamente se resolverá en Images/Logo.jpg, lo que sería correcto para una solicitud de Ajax a esa página. Ahora se resolverá en la raíz de la (/ MySite/Images/Logo.jpg).
- **Ha cambiado el método HttpContext.RedirectLocal**. Este método ahora acepta solo las direcciones URL que son relativas a la aplicación actual. Nombre completo se rechazan las direcciones URL.
- **El método ModelState.IsValid ahora requiere llamar primero a validar**. Si va a convertir la aplicación para usar los nuevos métodos de validación de entrada y va a llamar a la *ModelState.IsValid* método, debe llamar ahora *Validation.Validate* con antelación. Por ejemplo, ahora debe seguir este patrón: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample2.cs)]

 Sin embargo, se recomienda que si usa los nuevos métodos de validación de entrada, no use *ModelState.IsValid*. En su lugar, estructurar el código similar al siguiente: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample3.cs)]
- **En Internet Explorer 7 y Internet Explorer 8, no funciona la validación del lado cliente**. Validación del lado cliente no funciona debido a incompatibilidades con jQuery 1.6.2, que se incluye con la plantilla de proyecto predeterminada. (Validación del lado servidor funciona.).

#### <a id="_Toc303701289"></a>Declinación de responsabilidades

© 2011 Microsoft Corporation. Todos los derechos reservados. Este documento se proporciona "como-es." Información y opiniones expresadas en este documento, incluidas direcciones URL y otras referencias a sitios Web de Internet, pueden cambiar sin previo aviso. Usted asume el riesgo de utilizarla.
