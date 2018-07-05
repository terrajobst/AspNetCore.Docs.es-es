---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Información de visitante (análisis) de seguimiento de ASP.NET Web Pages (Razor) sitio | Microsoft Docs
author: tfitzmac
description: Una vez que ha obtenido su sitio Web va, puede analizar el tráfico del sitio Web.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 48782606083b4aa1e32adf6163bcb3f2d9828bc3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387142"
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a>Información de seguimiento visitante (análisis) para un sitio Web de ASP.NET Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se describe cómo usar una aplicación auxiliar para agregar análisis de sitios Web a las páginas de un sitio Web de ASP.NET Web Pages (Razor).
> 
> Lo que aprenderá:
> 
> - Cómo enviar información sobre el tráfico del sitio Web a un proveedor de análisis.
> 
> Estas son las características introducidas en el artículo de programación de ASP.NET:
> 
> - El `Analytics` auxiliar.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - ASP.NET Web Helpers Library (paquete de NuGet)


Analytics es un término general para la tecnología que mide el tráfico en su sitio Web para que pueda comprender cómo se usa el sitio. Muchos de los servicios de análisis están disponibles, incluidos los servicios de Google, Yahoo, StatCounter y otros.

El análisis de manera funciona es que suscribirse a una cuenta con el proveedor de análisis, donde se registra el sitio que desean realizar un seguimiento. El proveedor envía un fragmento de código de JavaScript que incluye un identificador o código de la cuenta de seguimiento. Agregue el fragmento de código de JavaScript a las páginas web en el sitio que desea realizar un seguimiento. (Normalmente, agregar el fragmento de código de análisis para una página de diseño o de pie de página o de otro elemento de marcado HTML que aparece en todas las páginas del sitio.) Cuando los usuarios solicitan una página que contiene uno de estos fragmentos de código de JavaScript, el fragmento de código envía información sobre la página actual para el proveedor de análisis, que registra los diversos detalles acerca de la página.

Cuando desee echar un vistazo a las estadísticas del sitio, inicie sesión en el sitio Web del proveedor de análisis. A continuación, puede ver a todo tipo de informes sobre su sitio, como:

- El número de vistas de página para páginas individuales. Esto indica a (aproximadamente) el número de personas que visitan el sitio, y qué páginas del sitio son los más populares.
- ¿Durante cuánto tiempo trabajadores emplean normalmente en páginas específicas. Esto puede indicar cosas como si la página principal es mantener el interés de las personas.
- Lo que los usuarios sitios estaban en antes de que visiten su sitio. Esto le ayudará a comprender si el tráfico procede de vínculos desde las búsquedas y así sucesivamente.
- Cuando las personas visitan su sitio y cuánto tiempo permanecen.
- ¿Qué países son de los visitantes de.
- ¿Qué sistemas operativos y exploradores utilizan los visitantes.

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a>Uso de una aplicación auxiliar para agregar análisis a una página

Las páginas Web ASP.NET incluye varias aplicaciones auxiliares de análisis (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, y `Analytics.GetStatCounterHtml`) que facilitan administrar los fragmentos de código de JavaScript que se usan para el análisis. En lugar de averiguar cómo y dónde colocar el código de JavaScript, lo único que debe hacer es agregar la aplicación auxiliar a una página. La única información que debe proporcionar es el nombre de la cuenta, identificador o código de seguimiento. (Para StatCounter, también debe proporcionar algunos valores adicionales.)

En este procedimiento, creará una página de diseño que usa el `GetGoogleHtml` auxiliar. Si ya tiene una cuenta con uno de los otros proveedores de análisis, puede utilizar esa cuenta en su lugar y realizar pequeñas modificaciones, según sea necesario.

> [!NOTE]
> Cuando se crea una cuenta de análisis, registrar la dirección URL del sitio que desea realizar el seguimiento. Si va a probar todo en el equipo local, no se seguimiento tráfico real (es el único tráfico), por lo que no será capaz de registrar y ver las estadísticas del sitio. Pero este procedimiento muestra cómo agregar una aplicación auxiliar de analytics a una página. Al publicar su sitio, el sitio activo enviará información a su proveedor de análisis.


1. Agregue la ASP.NET Web Helpers Library a su sitio Web, como se describe en [las aplicaciones auxiliares de la instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si aún no lo ha agregado.
2. Cree una cuenta con Google Analytics y registre el nombre de cuenta.
3. Cree una página de diseño denominada *Analytics.cshtml* y agregue el siguiente marcado:

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > Debe colocar la llamada a la `Analytics` auxiliar en el cuerpo de la página web (antes de la `</body>` etiqueta). En caso contrario, el explorador no ejecutará el script.

    Si usa un proveedor de análisis diferentes, use una de las siguientes aplicaciones auxiliares en su lugar:

    - (Yahoo) `@Analytics.GetYahooHtml("myaccount")`
    - (StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`
4. Reemplace `myaccount` con el nombre de la cuenta, identificador o código de seguimiento que creó en el paso 1.
5. Ejecute la página en el explorador. (Asegúrese de que la página está seleccionada en el **archivos** área de trabajo antes de ejecutarlo.)
6. En el explorador, vea el origen de la página. Podrá ver el código representado analytics:

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. Inicie sesión en el sitio de Google Analytics y examinar las estadísticas para el sitio. Si ejecuta la página en un sitio activo, verá una entrada que registra la visita a la página.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [Sitio de Google Analytics](https://www.google.com/analytics/)
- [Yahoo! Sitio de Web Analytics](http://help.yahoo.com/l/us/yahoo/ywa/)
- [Sitio StatCounter](http://statcounter.com/)
