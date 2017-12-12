---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
title: "Agregar contenido dinámico a una página almacenada en caché (VB) | Documentos de Microsoft"
author: microsoft
description: "Obtenga información acerca de cómo mezclar contenido dinámico y almacenado en caché en la misma página. Sustitución tras la caché le permite mostrar contenido dinámico, como o de anuncios de pancarta..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 68acd884-fb57-4486-a1be-aaa93e380780
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
msc.type: authoredcontent
ms.openlocfilehash: f07f4ecec36e71679dbc471b65f26d260349a07e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="adding-dynamic-content-to-a-cached-page-vb"></a>Agregar contenido dinámico a una página almacenada en caché (VB)
====================
por [Microsoft](https://github.com/microsoft)

> Obtenga información acerca de cómo mezclar contenido dinámico y almacenado en caché en la misma página. Sustitución tras la caché le permite mostrar contenido dinámico, como anuncios de pancarta o elementos de noticias, dentro de una página que se ha de salida almacenados en memoria caché.


Aprovechando las ventajas del almacenamiento en caché de salida, puede mejorar considerablemente el rendimiento de una aplicación ASP.NET MVC. En lugar de volver a una página cada vez que se solicita la página, la página se pueden generar una vez y almacena en caché en memoria para varios usuarios.

Pero hay un problema. ¿Qué ocurre si necesita mostrar contenido dinámico en la página? Por ejemplo, imagine que desea mostrar un anuncio en la página. No desea que el anuncio de pancarta se almacene en caché para que cada usuario ve el anuncio del mismo. ¡De este modo no realiza dinero!

Afortunadamente, hay una solución sencilla. Puede aprovechar las ventajas de una característica del marco de ASP.NET denominada *sustitución tras la caché*. Sustitución tras la caché permite sustituir contenido dinámico en una página que se ha almacenado en caché en memoria.


Normalmente, cuando los resultados de caché una página mediante el uso de la &lt;OutputCache&gt; atributo, la página se almacena en caché en el servidor y el cliente (Explorador de web). Cuando se usa la sustitución posterior a la caché, se almacena en caché una página en el servidor.


#### <a name="using-post-cache-substitution"></a>Uso de sustitución tras la caché

Uso de sustitución tras la caché, requiere dos pasos. En primer lugar, debe definir un método que devuelve una cadena que representa el contenido dinámico que desea mostrar en la página almacenada en caché. A continuación, llame al método HttpResponse.WriteSubstitution() para insertar el contenido dinámico en la página.

Por ejemplo, imagine que desea mostrar aleatoriamente los elementos de noticias diferente en una página almacenada en caché. La clase en la lista 1 expone un solo método, denominado RenderNews(), que devuelve al azar un elemento de noticias de una lista de tres elementos de noticias.

**Lista 1 – Models\News.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample1.vb)]

Para aprovechar las ventajas de sustitución tras la caché, se llama al método de HttpResponse.WriteSubstitution(). El método WriteSubstitution() establece el código para reemplazar una región de la página en caché con contenido dinámico. El método WriteSubstitution() se usa para mostrar el elemento de noticias aleatorio en la vista en el listado 2.

**La lista 2 – Views\Home\Index.aspx**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample2.aspx)]

El método RenderNews se pasa al método WriteSubstitution(). Tenga en cuenta que no se llama al método de RenderNews. En su lugar, se pasa una referencia al método a WriteSubstitution() con la Ayuda del operador AddressOf.

La vista de índice se almacena en caché. La vista es devuelto por el controlador en el listado 3. Tenga en cuenta que la acción de Index() se decora con un &lt;OutputCache&gt; atributo que hace que la vista de índice en la memoria caché durante 60 segundos.

**El listado 3 – Controllers\HomeController.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample3.vb)]

Aunque la vista de índice se almacena en caché, se muestran elementos de noticias aleatorio diferentes cuando se solicita la página de índice. Cuando se solicita la página de índice, el tiempo que se muestra en la página no cambia durante 60 segundos (consulte la figura 1). El hecho de que no cambia la hora se demuestra que la página se almacena en caché. Sin embargo, el contenido insertado por los cambios de método: el elemento de noticias aleatorio – WriteSubstitution() con cada solicitud.

**Ilustración 1: insertar elementos de noticias dinámica en una página almacenada en caché**

![clip_image002](adding-dynamic-content-to-a-cached-page-vb/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>Uso de sustitución tras la caché en métodos auxiliares

Una manera más fácil de aprovechar las ventajas de sustitución tras la caché consiste en encapsular la llamada al método WriteSubstitution() dentro de un método de aplicación auxiliar personalizada. Este enfoque se muestra en el método auxiliar en el listado 4.

**Listado 4 – Helpers\AdHelper.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample4.vb)]

Listado 4 contiene un módulo de Visual Basic que expone dos métodos: RenderBanner() y RenderBannerInternal(). El método RenderBanner() representa el método auxiliar real. Este método extiende la clase HtmlHelper de MVC de ASP.NET estándar para que se puede llamar a Html.RenderBanner() en una vista al igual que cualquier otro método de aplicación auxiliar.

El método RenderBanner() llama al método de HttpResponse.WriteSubstitution() pasando al método RenderBannerInternal() al método WriteSubsitution().

El método RenderBannerInternal() es un método privado. Este método no se expone como un método auxiliar. El método RenderBannerInternal() devuelve aleatoriamente una imagen de anuncios de pancarta en una lista de tres imágenes de anuncios de pancarta.

La vista de índice modificada en el listado 5 muestra cómo puede usar el método de aplicación auxiliar RenderBanner(). Tenga en cuenta que otro &lt;% @ Import %&gt; directiva se incluye en la parte superior de la vista para importar el espacio de nombres MvcApplication1.Helpers. Si no desea importar este espacio de nombres, el método RenderBanner() no aparecerá como un método en la propiedad de Html.

**Listado 5 – Views\Home\Index.aspx (con el método de RenderBanner())**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample5.aspx)]

Cuando se solicita la página representada por la vista en el listado 5, se muestra un anuncio diferente con cada solicitud (consulte la figura 2). La página se almacena en caché, pero los anuncios de pancarta se inyección dinámicamente por el método de aplicación auxiliar RenderBanner().

**Figura 2: la vista de índice muestra un anuncio aleatorio**

![clip_image004](adding-dynamic-content-to-a-cached-page-vb/_static/image2.jpg)

#### <a name="summary"></a>Resumen

Este tutorial explica cómo puede actualizar dinámicamente el contenido de una página almacenada en caché. Aprendió a utilizar el método HttpResponse.WriteSubstitution() para habilitar el contenido dinámico que puedan insertarse en una página almacenada en caché. También aprendió encapsular la llamada al método WriteSubstitution() dentro de un método de aplicación auxiliar HTML.

Aprovechar las ventajas del almacenamiento en caché siempre que sea posible: puede tener un impacto significativo en el rendimiento de las aplicaciones web. Como se explica en este tutorial, puede aprovechar las ventajas del almacenamiento en caché incluso cuando se necesitan para mostrar contenido dinámico en las páginas.

>[!div class="step-by-step"]
[Anterior](improving-performance-with-output-caching-vb.md)
[Siguiente](creating-a-controller-vb.md)
