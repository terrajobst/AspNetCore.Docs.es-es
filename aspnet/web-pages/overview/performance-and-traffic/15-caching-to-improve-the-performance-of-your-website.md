---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: "Almacenamiento en caché de datos en un ASP.NET Web Pages (Razor) sitio para mejorar el rendimiento | Documentos de Microsoft"
author: tfitzmac
description: "Puede acelerar su sitio Web al tener almacenar: es decir, caché, los resultados de los datos que normalmente podrían tardar un tiempo considerable para recuperar o procesar un..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2014
ms.topic: article
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: 742409219bd3b05f8ddf2c0d5034919fc9bf1d26
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a>Almacenar en caché datos en un sitio de ASP.NET Web Pages (Razor) para mejorar el rendimiento
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo usar una aplicación auxiliar para almacenar en caché información para mejorar el rendimiento en un sitio Web de ASP.NET Web Pages (Razor). Puede acelerar su sitio Web al incluirla almacén &#8212; es decir, caché &#8212; los resultados de los datos que normalmente podría tardar un tiempo considerable para recuperar o procesar y que no cambian con frecuencia.
> 
> **Lo que aprenderá:** 
> 
> - Describe cómo usar el almacenamiento en caché para mejorar la capacidad de respuesta de su sitio Web.
> 
> Estas son las características ASP.NET presentadas en el artículo:
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


Cada vez que un usuario solicita una página del sitio, el servidor web tiene que realizar algún trabajo para satisfacer la solicitud. En algunos de sus páginas, el servidor podría tener que realizar tareas que toman (comparativamente) mucho tiempo, por ejemplo, la recuperación de datos de una base de datos. Aunque estas tareas no toman larga en términos absolutos, si su sitio experimenta una gran cantidad de tráfico, puede sumar una serie completa de las solicitudes individuales que hacer que el servidor web realizar las tareas complicadas o lenta a una gran cantidad de trabajo. En última instancia, esto puede afectar al rendimiento del sitio.

Es una manera de mejorar el rendimiento de su sitio Web en casos como este en caché los datos. Si su sitio obtiene repetir las solicitudes de la misma información y no necesita modificarse por cada persona a la información y no es tiempo confidenciales, en lugar de volver a capturar o actualizar los cálculos, puede capturar los datos una vez y, a continuación, almacenar los resultados. La próxima vez que llega una solicitud para que obtener información, simplemente obtenerlo de la memoria caché.

En general, se almacenan en caché información que no cambia con frecuencia. Al colocar la información en la memoria caché, se almacena en memoria en el servidor web. Puede especificar cuánto tiempo se deben almacenarse en caché, de segundos a días. Cuando expira el período de almacenamiento en caché, se quita automáticamente la información de la memoria caché.

> [!NOTE]
> Las entradas de la memoria caché podrían quitarse por motivos distintos de que han caducado. Por ejemplo, el servidor web podría temporalmente disponga de suficiente memoria y una forma pueda reclamar memoria consiste en producir las entradas de la memoria caché. Como verá, incluso si ha colocado información en la memoria caché, tiene que comprobar para asegurarse de que aún está allí cuando lo necesite.


Imagine que su sitio Web tiene una página que muestra la temperatura actual y el boletín meteorológico. Para obtener este tipo de información, puede enviar una solicitud a un servicio externo. Puesto que esta información no varían mucho (dentro de un período de tiempo de dos horas, por ejemplo) y, como llamadas externas llevar tiempo y el ancho de banda, es un buen candidato para el almacenamiento en caché.

## <a name="adding-caching-to-a-page"></a>Agregar almacenamiento en caché a una página

ASP.NET incluye una `WebCache` auxiliar que resulta muy sencillo agregar almacenamiento en caché a su sitio y agregar datos a la memoria caché. En este procedimiento, creará una página que almacena en caché la hora actual. Esto no es un ejemplo del mundo real, ya que la hora actual es algo que cambian con frecuencia, y que además no resulta difícil de calcular. Sin embargo, resulta una buena forma de ilustrar el almacenamiento en caché en acción.

1. Agregar una nueva página denominada *WebCache.cshtml* al sitio Web.
2. Agregue el código y el marcado siguiente a la página:

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    Cuando se almacena en caché datos, ubicación en la memoria caché con un nombre de esto es único para todo el sitio Web. En este caso, deberá utilizar una entrada de caché con nombre `CachedTime`. Se trata de la `cacheItemKey` se muestra en el ejemplo de código.

    El código lee primero el `CachedTime` entrada de caché. Si se devuelve un valor (es decir, si la entrada de caché no es null), el código simplemente establece el valor de la variable de tiempo en los datos de la memoria caché.

    Sin embargo, si no existe la entrada de caché (es decir, es null), el código establece el valor de tiempo, lo agrega a la memoria caché y establece un valor de expiración en un minuto. Después de un minuto, se descarta la entrada de caché. (El valor de expiración predeterminado para un elemento de la memoria caché es de 20 minutos). El comando `WebCache.Set(cacheItemKey, time, 1, false)` muestra cómo agregar el valor de tiempo actual a la memoria caché y establecer su expiración en 1 minuto. Establecer el *slidingExpiration* parámetro `false` significa que la hora de expiración no se renueva cada vez que se solicita. Expiración exactamente 1 minuto después de que se agregó originalmente a la memoria caché. Si establece este valor en `true` la hora de expiración de 1 minuto se restablece cada vez que se solicita el valor de la memoria caché.

    Este código muestra el patrón que se debe utilizar siempre cuando almacena datos en caché. Antes de que obtener algo de la memoria caché, primero compruebe siempre si la `WebCache.Get` método devolvió un valor nulo. Recuerde que la entrada de caché han expirado o puede que haya quitado por algún otro motivo, por lo que nunca se garantiza que cualquier entrada determinada en la memoria caché.
3. Ejecutar *WebCache.cshtml* en un explorador. (Asegúrese de que la página está seleccionada en el **archivos** área de trabajo antes de ejecutarlo.) La primera vez que solicite la página, los datos de hora no están en la memoria caché y el código tiene que agregar el valor de hora a la memoria caché.

    ![1 de la memoria caché](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. Actualizar *WebCache.cshtml* en el explorador. Esta vez, los datos de tiempo están en la memoria caché. Tenga en cuenta que la hora no ha cambiado desde la última vez que se ha visto la página.

    ![caché-2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. Espere un minuto para vaciar la memoria caché y, a continuación, actualice la página. La página nuevo indica que los datos de tiempo no se encontraron en la memoria caché y la hora actualizada se agrega a la memoria caché.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales


- [Mostrar datos en un gráfico](https://go.microsoft.com/fwlink/?LinkId=202895)
- [Referencia de la API de WebCache](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx) (MSDN)
