---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Instalar una aplicación auxiliar en un ASP.NET Web Pages (Razor) sitio | Documentos de Microsoft
author: tfitzmac
description: En este artículo se describe cómo instalar una aplicación auxiliar en un sitio Web de ASP.NET Web Pages (Razor). Una aplicación auxiliar es un componente reutilizable que incluye código y marcado en por...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 766fbb87ae8bcb8917eb8fa7f552c00792242cf6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a>Instalar una aplicación auxiliar en un sitio de ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se describe cómo instalar una aplicación auxiliar en un sitio Web de ASP.NET Web Pages (Razor). A *auxiliar* es un componente reutilizable que incluye código y marcado para realizar una tarea que podría ser una tarea tediosa o complejas.
> 
> Lo que aprenderá:
> 
> - Cómo instalar una aplicación auxiliar en un sitio Web creado con 3 de WebMatrix.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - 3 de WebMatrix


## <a name="overview-of-helpers"></a>Información general de las aplicaciones auxiliares

Algunas tareas que personas a menudo es conveniente en las páginas web requieren una gran cantidad de código o requieren conocimientos adicionales. Algunos ejemplos son mostrar un gráfico de datos; colocar un botón de Twitter "Seguir" en una página; al enviar correo electrónico de su sitio Web; recortar o cambiar el tamaño de las imágenes; uso de PayPal para su sitio. Para que sea fácil de hacer este tipo de cosas, ASP.NET Web Pages le permite usar *aplicaciones auxiliares*. Aplicaciones auxiliares son componentes que instalar para un sitio y que permiten realizan tareas habituales con solo una o dos líneas de código Razor.

ASP.NET Web Pages tiene algunas aplicaciones auxiliares integradas. Sin embargo, muchas aplicaciones auxiliares están disponibles en paquetes (complementos) que se proporcionan con el Administrador de paquetes de NuGet. NuGet permite seleccionar un paquete que desea instalar y, a continuación, se encarga de todos los detalles de la instalación.

## <a name="installing-a-helper-in-webmatrix-3"></a>Instalar una aplicación auxiliar de WebMatrix 3

1. En WebMatrix 3, haga clic en el **NuGet** botón.

    ![Cuadro de diálogo de la Galería de NuGet en WebMatrix](installing-helpers/_static/image1.png)
2. Esto inicia el Administrador de paquetes de NuGet y muestra los paquetes disponibles. En el cuadro de búsqueda, escriba una palabra clave para la aplicación auxiliar que se va a instalar.

    ![Cuadro de diálogo de la Galería de NuGet en WebMatrix](installing-helpers/_static/image2.png)
3. Seleccione el paquete y, a continuación, haga clic en **instalar**. Haga clic en **Sí** cuando se le pregunte si desea instalar el paquete e indique que acepta los términos.

     Si se trata de la primera vez que se ha instalado una aplicación auxiliar, NuGet crea carpetas en el sitio Web para el código que constituye la aplicación auxiliar.
4. Para desinstalar una aplicación auxiliar, haga clic en el **galería** botón, haga clic en el **instalado** pestaña y elegir el paquete que desea desinstalar.

## <a name="installing-the-twitter-helper"></a>Instalar la aplicación auxiliar de Twitter

La versión más reciente de la API de Twitter no es compatible con la aplicación auxiliar de Twitter que se instala a través de NuGet. En su lugar, vea el [aplicación auxiliar de Twitter con WebMatrix](twitter-helper.md) tema para obtener información acerca de cómo configurar la aplicación auxiliar de Twitter en su proyecto.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales


[2: conceptos básicos de programación de presentación ASP.NET Web Pages](../getting-started/introducing-razor-syntax-c.md)

[Aplicación auxiliar de Twitter con WebMatrix](twitter-helper.md)
