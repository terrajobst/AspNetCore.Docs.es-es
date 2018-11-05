---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: Almacenamiento en caché de datos en ASP.NET Web Pages (Razor) sitio para mejorar el rendimiento | Microsoft Docs
author: Rick-Anderson
description: 'Puede acelerar su sitio Web al tener almacenar: es decir, caché, los resultados de datos que normalmente tardará bastante tiempo en recuperar o procesar un...'
ms.author: riande
ms.date: 02/14/2014
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: ede341e02869a9c81cbe2fa7ef97345dc87519a1
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020370"
---
<a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a>Para mejorar el rendimiento de almacenamiento en caché datos en un sitio Web de ASP.NET Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo usar una aplicación auxiliar de la información en caché para mejorar el rendimiento en un sitio Web de ASP.NET Web Pages (Razor). Puede acelerar su sitio Web al tener almacenar &#8212; es decir, almacenar en caché &#8212; los resultados de los datos que normalmente podría tardar bastante tiempo en recuperar ni procesar y que no cambian con frecuencia.
> 
> **Lo que aprenderá:** 
> 
> - Cómo usar el almacenamiento en caché para mejorar la capacidad de respuesta de su sitio Web.
> 
> Estas son las características ASP.NET incorporadas en el artículo:
> 
> - El `WebCache` auxiliar.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2.


Cada vez que un usuario solicita una página del sitio, el servidor web tiene que realizar algún trabajo con el fin de satisfacer la solicitud. Algunos de sus páginas, el servidor podría tener que realizar las tareas que toman (relativamente) mucho tiempo, como la recuperación de datos desde una base de datos. Incluso si estas tareas no tardan tiempo en términos absolutos, si su sitio experimenta una gran cantidad de tráfico, puede agregar una serie completa de las solicitudes individuales que hacer que el servidor web realizar la tarea complicada o lenta hasta una gran cantidad de trabajo. En última instancia, esto puede afectar al rendimiento del sitio.

Es una manera de mejorar el rendimiento de su sitio Web en casos como este en caché los datos. Si su sitio obtiene repetir las solicitudes de la misma información y la información no deben modificarse para cada persona y no es tiempo confidencial, en lugar de volver a capturar o volver a calcularse, puede capturar los datos una vez y, a continuación, almacenar los resultados. La próxima vez que llega una solicitud para que obtener información, simplemente obtenerlo fuera de la memoria caché.

En general, se almacenan en caché información que no cambia con frecuencia. Al colocar información en la memoria caché, se almacena en memoria en el servidor web. Puede especificar cuánto tiempo se deben almacenarse en caché, de segundos a días. Cuando expira el período de almacenamiento en caché, se quita automáticamente la información de la memoria caché.

> [!NOTE]
> Las entradas de la memoria caché podrían quitarse por motivos de distinto que han caducado. Por ejemplo, el servidor web podría temporalmente quede sin memoria y una manera de reclamar la memoria es generando entradas de la memoria caché. Como verá, incluso si ha poner información en la caché, tendrá que comprobar para asegurarse de que sigue ahí es cuando los necesite.


Imagine que su sitio Web tiene una página que muestra la temperatura actual y la previsión meteorológica. Para obtener este tipo de información, puede enviar una solicitud a un servicio externo. Puesto que esta información no cambia mucho (dentro de un período de tiempo de dos horas, por ejemplo) y ya que las llamadas externas requieren tiempo y ancho de banda, es un buen candidato para almacenar en caché.

## <a name="adding-caching-to-a-page"></a>Agregar almacenamiento en caché a una página

ASP.NET incluye un `WebCache` auxiliar que facilita agregar almacenamiento en caché a su sitio y agregar datos a la memoria caché. En este procedimiento, creará una página que almacena en caché la hora actual. Esto no es un ejemplo del mundo real, ya que la hora actual es algo que cambian con frecuencia, y que no es más difícil de calcular. Sin embargo, es una buena manera de ilustrar el almacenamiento en caché en acción.

1. Agregar una nueva página denominada *WebCache.cshtml* al sitio Web.
2. Agregue el código y el marcado siguiente a la página:

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    Al almacenar en caché datos, colocarlo en la caché mediante un nombre de esto es único en el sitio Web. En este caso, usará una entrada de caché denominada `CachedTime`. Se trata de la `cacheItemKey` se muestra en el ejemplo de código.

    El código lee primero el `CachedTime` entrada de caché. Si se devuelve un valor (es decir, si la entrada de caché no es null), el código simplemente establece el valor de la variable de tiempo a los datos de la memoria caché.

    Sin embargo, si no existe la entrada de caché (es decir, es null), el código establece el valor de tiempo, lo agrega a la memoria caché y establece un valor de expiración a un minuto. Después de un minuto, se descarta la entrada de caché. (El valor de expiración predeterminado para un elemento en la memoria caché es 20 minutos). El comando `WebCache.Set(cacheItemKey, time, 1, false)` muestra cómo agregar el valor de tiempo actual a la memoria caché y establece la fecha de expiración en 1 minuto. Establecer el *slidingExpiration* parámetro `false` significa que la hora de expiración no se renueva cada vez que se solicita. Fecha de vencimiento exactamente 1 minuto después de que se agregó originalmente a la memoria caché. Si establece este valor en `true` la hora de expiración de 1 minuto se restablece cada vez que se solicita el valor de la memoria caché.

    Este código muestra el patrón que siempre debe usar cuando almacena datos en caché. Antes de entrar algo fuera de la memoria caché, primero compruebe siempre si la `WebCache.Get` método ha devuelto null. Recuerde que la entrada de caché es posible que han expirado o es posible que se han quitado por algún otro motivo, por lo que nunca se garantiza que cualquier entrada determinada en la memoria caché.
3. Ejecute *WebCache.cshtml* en un explorador. (Asegúrese de que la página está seleccionada en el **archivos** área de trabajo antes de ejecutarlo.) La primera vez que solicite la página, los datos de tiempo no se están en la memoria caché y el código tiene que agregar el valor de hora a la memoria caché.

    ![1 de la memoria caché](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. Actualizar *WebCache.cshtml* en el explorador. Esta vez, los datos de tiempo están en la memoria caché. Tenga en cuenta que el tiempo no ha cambiado desde la última vez que se vio la página.

    ![caché de 2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. Espere un minuto para que se puede vaciar la caché y, a continuación, actualice la página. La página nuevo indica que los datos de tiempo no se encontraron en la memoria caché y la hora de actualización se agrega a la memoria caché.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales


- [Mostrar datos en un gráfico](https://go.microsoft.com/fwlink/?LinkId=202895)
- [Referencia de API WebCache](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx) (MSDN)
