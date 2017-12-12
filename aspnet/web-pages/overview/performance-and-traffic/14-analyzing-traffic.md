---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: "Seguimiento de la información de visitante (análisis) para ASP.NET Web Pages (Razor) sitio | Documentos de Microsoft"
author: tfitzmac
description: "Después de que ha llegado el sitio Web va, puede analizar el tráfico del sitio Web."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 9a381ebaed30325fdfa5f0f558910d3002c61559
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a>Seguimiento de la información de visitante (análisis) para un sitio de ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artículo describe cómo utilizar una aplicación auxiliar para agregar el análisis de sitio Web a las páginas en un sitio Web de ASP.NET Web Pages (Razor).
> 
> Lo que aprenderá:
> 
> - Cómo enviar información sobre el tráfico del sitio Web a un proveedor de análisis.
> 
> Se trata de la programación de características introducidas en el artículo de ASP.NET:
> 
> - El `Analytics` auxiliar.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - ASP.NET Web Helpers Library (paquete de NuGet)


El análisis es un término general para las tecnologías que mide el tráfico en el sitio Web de manera que pueda comprender cómo se utiliza el sitio. Muchos servicios de análisis están disponibles, incluidos los servicios de Google, Yahoo, StatCounter y otros usuarios.

El análisis de manera funciona es la que se registra para una cuenta con el proveedor de análisis, en el que registra el sitio que desea realizar un seguimiento. El proveedor envía un fragmento de código de JavaScript que incluye un identificador o código de la cuenta de seguimiento. Agregar el fragmento de código de JavaScript a las páginas web en el sitio que desea realizar un seguimiento. (Se suele agrega el fragmento de código de análisis a un diseño o un pie de página u otro formato HTML que aparece en cada página en el sitio.) Cuando los usuarios solicitan una página que contiene uno de estos fragmentos de código de JavaScript, el fragmento de código envía información sobre la página actual para el proveedor de análisis, que registra los detalles acerca de la página.

Cuando desea un vistazo a las estadísticas del sitio, inicia sesión en el sitio Web del proveedor de análisis. A continuación, puede ver a todos los tipos de informes sobre su sitio, como:

- El número de vistas de página para las páginas individuales. Esto indica a (aproximadamente) el número de personas que visitan el sitio y las páginas en el sitio son las más populares.
- ¿Durante cuánto tiempo personas dedican en páginas específicas. Esto puede indicar cosas como si la página principal se mantiene interés popular.
- ¿Qué sitios personas estaban en antes de que visita el sitio. Esto le ayudará a comprender si el tráfico procede de vínculos de búsquedas y así sucesivamente.
- Cuando los usuarios visitan su sitio y cuánto tiempo se mantienen.
- ¿Qué países son gran los visitantes de.
- ¿Qué sistemas operativos y exploradores utilizan los visitantes.

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a>Uso de una aplicación auxiliar para agregar análisis a una página

Las páginas Web ASP.NET incluye varias aplicaciones auxiliares de análisis (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, y `Analytics.GetStatCounterHtml`) que resulte sencillo administrar los fragmentos de código de JavaScript que se usan para el análisis. En lugar de pensar en cómo y dónde colocar el código de JavaScript, lo único que debe hacer es agregar la aplicación auxiliar a una página. La única información que debe proporcionar es el nombre de la cuenta, el identificador o el código de seguimiento. (Para StatCounter, también tendrá que proporcione algunos valores adicionales.)

En este procedimiento, creará una página de diseño que usa el `GetGoogleHtml` auxiliar. Si ya tiene una cuenta con uno de los otros proveedores de análisis, puede utilizar esa cuenta en su lugar y realizar pequeñas modificaciones según sea necesario.

> [!NOTE]
> Al crear una cuenta de análisis, se registra la dirección URL del sitio que desea realizar el seguimiento. Si va a probar todos los elementos en el equipo local, no a seguimiento tráfico real (el único tráfico es usted), por lo que no podrá registrar y ver las estadísticas del sitio. Pero este procedimiento muestra cómo agregar una aplicación auxiliar de análisis a una página. Cuando se publica un sitio, el sitio activo enviará información a su proveedor de análisis.


1. Agregar ASP.NET Web Helpers Library a su sitio Web, como se describe en [aplicaciones auxiliares de instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si aún no se ha agregado.
2. Crear una cuenta con Google Analytics y registre el nombre de cuenta.
3. Crear una página de diseño denominada *Analytics.cshtml* y agregue el siguiente marcado:

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > Debe colocar la llamada a la `Analytics` auxiliar en el cuerpo de la página web (antes de la `</body>` etiqueta). En caso contrario, el explorador no ejecutará la secuencia de comandos.

    Si usa un proveedor de análisis diferentes, utilice en su lugar uno de los elementos auxiliares siguientes:

    - (Yahoo)`@Analytics.GetYahooHtml("myaccount")`
    - (StatCounter)`@Analytics.GetStatCounterHtml("project", "security")`
4. Reemplace `myaccount` con el nombre de la cuenta, el identificador o el código de seguimiento que creó en el paso 1.
5. Ejecute la página en el explorador. (Asegúrese de que la página está seleccionada en el **archivos** área de trabajo antes de ejecutarlo.)
6. En el explorador, vea el origen de la página. Podrá ver el código de análisis representado:

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. Inicie sesión en el sitio de Google Analytics y examine las estadísticas para el sitio. Si estás ejecutando la página en un sitio en vivo, verá una entrada que registra la visita a la página.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [Sitio de Google Analytics](https://www.google.com/analytics/)
- [Yahoo! Sitio Web de análisis](http://help.yahoo.com/l/us/yahoo/ywa/)
- [Sitio de StatCounter](http://statcounter.com/)
