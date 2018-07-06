---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Instalar una aplicación auxiliar en ASP.NET Web Pages (Razor) sitio | Microsoft Docs
author: tfitzmac
description: En este artículo se describe cómo instalar una aplicación auxiliar en un sitio Web de ASP.NET Web Pages (Razor). Una aplicación auxiliar es un componente reutilizable que incluye código y marcado en por...
ms.author: aspnetcontent
ms.date: 02/18/2014
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: db3dff9f2d70577bb0618335c0100b9899e87727
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838619"
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a>Instalar una aplicación auxiliar en un sitio Web de ASP.NET Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se describe cómo instalar una aplicación auxiliar en un sitio Web de ASP.NET Web Pages (Razor). Un *auxiliar* es un componente reutilizable que incluye el código y marcado para realizar una tarea que puede ser complejo o una tarea tediosa.
> 
> Lo que aprenderá:
> 
> - Cómo instalar una aplicación auxiliar en un sitio Web creado mediante WebMatrix 3.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - WebMatrix 3


## <a name="overview-of-helpers"></a>Información general de las aplicaciones auxiliares

Algunas tareas que las personas desean hacer en las páginas web a menudo requieren una gran cantidad de código o requieren conocimientos adicionales. Algunos ejemplos son mostrar un gráfico para los datos; colocar un botón "Seguir" de Twitter en una página; envío de correo electrónico desde su sitio Web; recortar o cambiar el tamaño de imágenes. uso de PayPal para su sitio. Para que sea fácil de hacer este tipo de cosas, ASP.NET Web Pages le permite usar *aplicaciones auxiliares de*. Las aplicaciones auxiliares son componentes que instale un sitio y que permiten realizan tareas habituales mediante solo una o dos líneas de código Razor.

ASP.NET Web Pages tiene algunas aplicaciones auxiliares integradas. Sin embargo, muchas aplicaciones auxiliares están disponibles en paquetes (complementos) que se proporcionan con el Administrador de paquetes de NuGet. NuGet le permite seleccionar un paquete para instalarlo y, a continuación, se encarga de todos los detalles de la instalación.

## <a name="installing-a-helper-in-webmatrix-3"></a>Instalar una aplicación auxiliar de WebMatrix 3

1. En WebMatrix 3, haga clic en el **NuGet** botón.

    ![Cuadro de diálogo de la Galería de NuGet en WebMatrix](installing-helpers/_static/image1.png)
2. Esto inicia el Administrador de paquetes de NuGet y muestra los paquetes disponibles. En el cuadro de búsqueda, escriba una palabra clave para la aplicación auxiliar que desea instalar.

    ![Cuadro de diálogo de la Galería de NuGet en WebMatrix](installing-helpers/_static/image2.png)
3. Seleccione el paquete y, a continuación, haga clic en **instalar**. Haga clic en **Sí** cuando se le pregunte si desea instalar el paquete e indicar que acepta los términos.

     Si se trata de la primera vez que se ha instalado una aplicación auxiliar, NuGet crea carpetas en su sitio Web para el código que conforma la aplicación auxiliar.
4. Para desinstalar una aplicación auxiliar, haga clic en el **galería** botón, haga clic en el **instalado** pestaña y seleccione el paquete que desea desinstalar.

## <a name="installing-the-twitter-helper"></a>Instalar la aplicación auxiliar de Twitter

La versión más reciente de la API de Twitter no es compatible con la aplicación auxiliar de Twitter que se instala a través de NuGet. En su lugar, consulte el [aplicación auxiliar de Twitter con WebMatrix](twitter-helper.md) tema para obtener información acerca de cómo configurar la aplicación auxiliar de Twitter en el proyecto.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales


[Presentación de ASP.NET Web Pages 2: conceptos básicos de programación](../getting-started/introducing-razor-syntax-c.md)

[Aplicación auxiliar de Twitter con WebMatrix](twitter-helper.md)
