---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Parte 10: Actualizaciones finales para navegación y el diseño del sitio, conclusión | Microsoft Docs'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC. Parte 10 cubre actualizaciones finales de navegación y S...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: f32509701dd112053aa4f31d6552601f961c7413
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41839068"
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a>Parte 10: Actualizaciones finales para navegación y el diseño del sitio, conclusión
====================
por [Jon Galloway](https://github.com/jongalloway)

> El Store de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> El Store de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa la administración básica del sitio, inicio de sesión de usuario y funcionalidad del carro de la compra.  
>   
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC. Parte 10 cubre actualizaciones finales de navegación y el diseño del sitio, conclusión.


Hemos completado toda la funcionalidad importante de nuestro sitio, pero aún hay algunas características para agregar a la página Examinar Store, la página principal y la navegación del sitio.

## <a name="creating-the-shopping-cart-summary-partial-view"></a>Creación de la vista parcial resumen carro de la compra

Queremos exponer el número de elementos en el carro de la compra del usuario en todo el sitio.

![](mvc-music-store-part-10/_static/image1.png)

Se puede implementar fácilmente esto mediante la creación de una vista parcial que se agrega a nuestro Site.master.

Como se mostró anteriormente, el controlador ShoppingCart incluye un método de acción CartSummary que devuelve una vista parcial:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

Para crear la vista parcial CartSummary, haga doble clic en la carpeta vistas/ShoppingCart y seleccione Agregar vista. Nombre de la vista CartSummary y Active la casilla "Crear una vista parcial" tal como se muestra a continuación.

![](mvc-music-store-part-10/_static/image2.png)

La vista parcial CartSummary es muy sencilla: es solo un vínculo a la vista de índice ShoppingCart que muestra el número de elementos en el carro de compra. El código completo de CartSummary.cshtml es como sigue:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

En cualquier página del sitio, incluido al maestro de sitio, mediante el método Html.RenderAction podemos incluir una vista parcial. RenderAction requiere que especifique el nombre de acción ("CartSummary") y el nombre del controlador ("ShoppingCart") como a continuación.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

Antes de agregar esto al diseño del sitio, también crearemos el género de menú para que podamos tomar todas las actualizaciones de Site.master al mismo tiempo.

## <a name="creating-the-genre-menu-partial-view"></a>Creación de la vista parcial del menú de género

Podemos lo hacemos mucho más fácil para los usuarios a navegar por el almacén mediante la adición de un menú de género que enumera todos los géneros disponibles en nuestra tienda.

![](mvc-music-store-part-10/_static/image3.png)

Se sigue el mismo pasos también crea una vista parcial GenreMenu y, a continuación, podemos agregar ambos al patrón de sitio. En primer lugar, agregue la siguiente acción del controlador GenreMenu a la StoreController:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

Esta acción devuelve una lista de géneros que se muestra la vista parcial, que creará a continuación.

*Nota: Hemos agregado el atributo [ChildActionOnly] a esta acción de controlador, lo que indica que sólo deseamos que esta acción puede utilizarse desde una vista parcial. Este atributo evitará la acción del controlador que se ejecute, vaya a /Store/GenreMenu. Esto no es necesario para las vistas parciales, pero es una buena práctica, puesto que deseamos para asegurarse de que las acciones del controlador se usan como tenemos previsto. También nos estamos devolviendo PartialView en lugar de la vista, que permite que el motor de vistas saber que no debe usar el diseño para esta vista, ya que se incluye en otras vistas.*

Haga doble clic en la acción del controlador GenreMenu y crear una vista parcial denominada GenreMenu que está fuertemente tipada utilizando la clase de datos de vista de género, tal como se muestra a continuación.

![](mvc-music-store-part-10/_static/image4.png)

Actualice el código de vista para la vista parcial GenreMenu mostrar los elementos con una lista desordenada como sigue.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a>Actualizar el diseño de sitio para mostrar nuestras vistas parciales

Podemos agregar nuestro vistas parciales al diseño del sitio (/vistas/Shared/\_Layout.cshtml) mediante una llamada a Html.RenderAction(). Vamos a agregar las dos en, así como algunas marcas adicionales para mostrarlas, como se muestra a continuación:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

Ahora Cuando ejecutemos la aplicación, veremos el género en el área de navegación izquierdo y el resumen del carro de compra en la parte superior.

## <a name="update-to-the-store-browse-page"></a>Actualizar a la página Examinar Store

La página Examinar Store es funcional, pero no parece muy bueno. Podemos actualizar la página para mostrar los álbumes de un mejor diseño actualizando el código de vista (que se encuentra en /Views/Store/Browse.cshtml) como sigue:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

Aquí vamos a hacer que use de Url.Action en lugar de Html.ActionLink para que podemos aplicar un formato especial al vínculo para incluir la ilustración del álbum.

*Nota: Estamos mostrando una portada del álbum genérico para estos álbumes. Esta información se almacena en la base de datos y puede modificarse mediante el Administrador de Store. Pueden agregar su propia ilustración.*

Ahora, cuando se examina un género, veremos los álbumes que se muestra en una cuadrícula con la ilustración del álbum.

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a>Actualización de la página principal para mostrar los álbumes de venta superior

Queremos nuestro álbumes en la página principal para aumentar las ventas más vendidos de características. Haremos algunas actualizaciones para nuestro HomeController controlar eso, y agregar en también algunos gráficos adicionales.

En primer lugar, vamos a agregar una propiedad de navegación a nuestra clase álbum para que sepa de Entity Framework que están asociados. Las últimas líneas de nuestro **álbum** clase ahora debería parecerse a esto:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

*Nota: Esto será necesario agregar el uso de una instrucción para que aparezca en el espacio de nombres System.Collections.Generic.*

En primer lugar, vamos a agregar un campo storeDB y el MvcMusicStore.Models mediante instrucciones, como en nuestro otros controladores. A continuación, agregaremos el método siguiente a HomeController que consulta la base de datos para encontrar los álbumes de ventas superiores según OrderDetails.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

Se trata de un método privado, puesto que no queremos que esté disponible como una acción de controlador. Incluiremos en HomeController por motivos de simplicidad, pero se recomienda para mover la lógica de negocios en clases de servicio independientes según corresponda.

Con esto en su lugar, podemos actualizar la acción de controlador de índice para consultar las principales 5 vender álbumes y devolverlos a la vista.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

El código completo de HomeController actualizada es como se muestra a continuación.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

Por último, se deberá actualizar la vista de nuestro índice de inicio para que puede mostrar una lista de álbumes, actualizar el tipo de modelo y agregue la lista de álbumes a la parte inferior. Tomamos esta oportunidad de agregar también un título y una sección de promoción a la página.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

Ahora Cuando ejecutemos la aplicación, veremos nuestra página de inicio actualizada con nuestro mensajes promocionales y álbumes de ventas superiores.

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a>Conclusión

Hemos visto que MVC de ASP.NET facilita la tarea para crear un sitio Web sofisticado con acceso de base de datos, suscripciones, AJAX, etcetera. muy rápidamente. Espero que este tutorial le haya proporcionado las herramientas que necesita para empezar a crear su propio ASP.NET MVC aplicaciones!


> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-9.md)
