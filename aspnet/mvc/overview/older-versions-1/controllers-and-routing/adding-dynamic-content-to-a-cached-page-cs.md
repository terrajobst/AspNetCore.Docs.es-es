---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: Agregar contenido dinámico a una página almacenada en caché (C#) | Microsoft Docs
author: microsoft
description: Obtenga información sobre cómo mezclar contenido dinámico y almacenado en caché en la misma página. La substitución posterior a la caché le permite mostrar contenido dinámico, como o los anuncios de pancarta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 08f680e8d057f47a3f2802b1136edfb00634637d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364150"
---
<a name="adding-dynamic-content-to-a-cached-page-c"></a>Agregar contenido dinámico a una página almacenada en caché (C#)
====================
por [Microsoft](https://github.com/microsoft)

> Obtenga información sobre cómo mezclar contenido dinámico y almacenado en caché en la misma página. La substitución posterior a la caché le permite mostrar contenido dinámico, como anuncios de pancarta o elementos de noticias, dentro de una página que ha sido de salida almacenados en caché.


Aprovechando las ventajas de la caché de resultados, puede mejorar considerablemente el rendimiento de una aplicación ASP.NET MVC. En lugar de volver a generar una página cada vez que se solicita la página, la página se puede generar una vez y almacena en caché en memoria para varios usuarios.

Pero hay un problema. ¿Qué ocurre si necesita mostrar contenido dinámico en la página? Por ejemplo, imagine que desea mostrar un anuncio de banner en la página. No desea que el anuncio de banner en la memoria caché para que cada usuario ve el anuncio mismo. No generar dinero de este modo.

Afortunadamente, hay una solución sencilla. Puede aprovechar una característica de ASP.NET framework llama *sustitución tras la caché*. La substitución posterior a la caché permite sustituir el contenido dinámico en una página que se ha almacenado en memoria.


Normalmente, cuando la salida de una página de caché mediante el atributo [OutputCache], la página se almacena en caché en el servidor y el cliente (el explorador web). Cuando se usa la sustitución posterior a la caché, se almacena en caché una página solo en el servidor.


#### <a name="using-post-cache-substitution"></a>Uso de la sustitución posterior a la caché

Uso de la sustitución posterior a la caché, requiere dos pasos. En primer lugar, deberá definir un método que devuelve una cadena que representa el contenido dinámico que desea mostrar en la página en caché. A continuación, llame al método HttpResponse.WriteSubstitution() para insertar el contenido dinámico en la página.

Por ejemplo, imagínese que desea mostrar los elementos de noticias diferente aleatoriamente en una página almacenada en caché. La clase en el listado 1 expone un solo método, denominado RenderNews(), que devuelve aleatoriamente un artículo de noticias de una lista de tres elementos de noticias.

**Listado 1 – Models\News.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

Para aprovechar las ventajas de la sustitución posterior a la caché, se llama al método de HttpResponse.WriteSubstitution(). El método WriteSubstitution() establece el código para reemplazar una región de la página en caché con contenido dinámico. El método WriteSubstitution() se usa para mostrar el elemento de noticias aleatorio en la vista en el listado 2.

**Listado 2 – Views\Home\Index.aspx**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

El método RenderNews se pasa al método WriteSubstitution(). Tenga en cuenta que no se llama al método RenderNews (no hay ningún paréntesis). En su lugar, se pasa una referencia al método a WriteSubstitution().

La vista de índice se almacena en caché. Se devuelve la vista por el controlador en el listado 3. Tenga en cuenta que la acción de Index() está decorada con un atributo [OutputCache] que hace que la vista de índice en la memoria caché durante 60 segundos.

**Listado 3 – controllers\homecontroller. cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

Aunque la vista de índice se almacena en caché, se muestran elementos de noticias aleatorio diferentes cuando se solicita la página de índice. Cuando se solicita la página de índice, la hora mostrada por la página no cambia durante 60 segundos (consulte la figura 1). El hecho de que no cambia la hora demuestra que la página se almacena en caché. Sin embargo, el contenido insertado por los cambios de método: el elemento aleatorio noticias – WriteSubstitution() con cada solicitud.

**Ilustración 1: inserción de elementos de noticias dinámica en una página almacenada en caché**

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>Uso de la sustitución posterior a la caché en métodos auxiliares

Aprovechar las ventajas de la sustitución posterior a la caché de una manera más fácil consiste en encapsular la llamada al método WriteSubstitution() dentro de un método auxiliar personalizada. Este enfoque se ilustra el método auxiliar en el listado 4.

**Listado 4 – AdHelper.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

Listado 4 contiene una clase estática que expone dos métodos: RenderBanner() y RenderBannerInternal(). El método RenderBanner() representa el método auxiliar real. Este método extiende la clase HtmlHelper de MVC de ASP.NET estándar para que pueda llamar Html.RenderBanner() en una vista al igual que cualquier otro método auxiliar.

El método RenderBanner() llama al método de HttpResponse.WriteSubstitution() pasando el método RenderBannerInternal() WriteSubsitution() al método.

El método RenderBannerInternal() es un método privado. Este método no se exponen como un método auxiliar. El método RenderBannerInternal() aleatoriamente devuelve una imagen del anuncio banner de una lista de tres imágenes de anuncios de pancarta.

La vista de índice modificada en el listado 5 muestra cómo puede usar el método auxiliar RenderBanner(). Tenga en cuenta que adicional &lt;% @ Import %&gt; directiva se incluye en la parte superior de la vista para importar el espacio de nombres MvcApplication1.Helpers. Si no importa este espacio de nombres, el método RenderBanner() no aparecerá como un método en la propiedad Html.

**Listado 5 – Views\Home\Index.aspx (con el método RenderBanner())**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

Cuando se solicita la página representada por la vista en el listado 5, se muestra un anuncio diferente de pancarta con cada solicitud (consulte la figura 2). La página se almacena en caché, pero el anuncio se inserta dinámicamente mediante el método auxiliar RenderBanner().

**Ilustración 2: la vista de índice muestra un anuncio aleatorio**

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a>Resumen

En este tutorial se explica cómo actualizar dinámicamente el contenido de una página almacenada en caché. Ha aprendido a usar el método HttpResponse.WriteSubstitution() para habilitar el contenido dinámico para insertarse en una página almacenada en caché. También ha aprendido cómo encapsular la llamada al método WriteSubstitution() dentro de un método de aplicación auxiliar HTML.

Aprovechar las ventajas del almacenamiento en caché siempre que sea posible: puede tener un impacto significativo en el rendimiento de las aplicaciones web. Como se explica en este tutorial, puede beneficiarse del almacenamiento en caché incluso cuando necesite mostrar contenido dinámico en las páginas.

## 

## 

> [!div class="step-by-step"]
> [Anterior](improving-performance-with-output-caching-cs.md)
> [Siguiente](creating-a-controller-cs.md)
