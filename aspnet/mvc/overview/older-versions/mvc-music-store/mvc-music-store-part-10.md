---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: "Parte 10: Final las actualizaciones de navegación y el diseño del sitio, conclusión | Documentos de Microsoft"
author: jongalloway
description: "Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET. Parte 10 cubre Final actualizaciones para navegación y S..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: 2a65e4b793b615c45cdf31166e0a000ae72ee534
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2018
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a>Parte 10: Final las actualizaciones de navegación y el diseño del sitio, conclusión
====================
por [Jon Galloway](https://github.com/jongalloway)

> La tienda de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> La tienda de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa administración básica del sitio, en el inicio de sesión de usuario y la funcionalidad del carro de la compra.  
>   
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET. Parte 10 cubre las actualizaciones Final de navegación y el diseño del sitio, conclusión.


Hemos completado toda la funcionalidad principal de nuestro sitio, pero todavía tenemos algunas características para agregar a la exploración del sitio, la página principal y la página Examinar de almacén.

## <a name="creating-the-shopping-cart-summary-partial-view"></a>Crea la vista parcial resumen carro de la compra

Debe exponer el número de elementos en el carro de la compra del usuario en todo el sitio.

![](mvc-music-store-part-10/_static/image1.png)

Podemos implementar con facilidad esto mediante la creación de una vista parcial que se agrega a nuestro Site.master.

Como se muestra anteriormente, el controlador ShoppingCart incluye un método de acción de CartSummary que devuelve una vista parcial:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

Para crear la vista parcial CartSummary, haga doble clic en la carpeta vistas/ShoppingCart y seleccione Agregar vista. Nombre de la vista CartSummary y Active la casilla "Crear una vista parcial" tal y como se muestra a continuación.

![](mvc-music-store-part-10/_static/image2.png)

La vista parcial CartSummary es muy sencilla: es solo un vínculo a la vista de índice ShoppingCart que muestra el número de elementos en el carro. El código completo de CartSummary.cshtml es como sigue:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

Podemos incluimos una vista parcial en cualquier página del sitio, incluyendo al patrón de sitio, mediante el método Html.RenderAction. RenderAction requiere que especifique el nombre de acción ("CartSummary") y el nombre del controlador ("ShoppingCart") como a continuación.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

Antes de agregar esto al sitio de diseño, también crearemos el menú de género para que podamos hacer todas nuestras actualizaciones Site.master al mismo tiempo.

## <a name="creating-the-genre-menu-partial-view"></a>Crea la vista parcial del menú de género

Podemos hacer mucho más fácil para los usuarios a navegar por el almacén mediante la adición de un menú de género que enumera todos los géneros disponibles en el almacén.

![](mvc-music-store-part-10/_static/image3.png)

Se sigue los mismos pasos también crea una vista parcial GenreMenu y, a continuación, podemos agregar ambos con el maestro de sitio. En primer lugar, agregue la siguiente acción del controlador GenreMenu a la StoreController:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

Esta acción devuelve una lista de géneros que mostrará la vista parcial, que se creará a continuación.

*Nota: Hemos agregado el atributo [ChildActionOnly] para esta acción de controlador, lo que indica que deseamos solo esta acción puede utilizarse desde una vista parcial. Este atributo evitará que la acción de controlador desde la que se está ejecutando examinando /Store/GenreMenu. Esto no es necesario para las vistas parciales, pero es una buena práctica, puesto que deseamos para asegurarse de que las acciones del controlador se usan como tenemos previsto. También nos estamos devolver PartialView en lugar de la vista, que permite que el motor de vista saber que no debe utilizar el diseño para esta vista, tal y como incluirlo en otras vistas.*

Haga doble clic en la acción del controlador GenreMenu y crear una vista parcial denominada GenreMenu que está fuertemente tipado mediante la clase de datos de vista de género, tal y como se muestra a continuación.

![](mvc-music-store-part-10/_static/image4.png)

Actualice el código de la vista para la vista parcial GenreMenu mostrar los elementos mediante una lista sin ordenar como se indica a continuación.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a>Actualizar el diseño de sitio para mostrar nuestro vistas parciales

Podemos agregar nuestro vistas parciales para el diseño del sitio (ovistas/Shared/\_Layout.cshtml) mediante una llamada a Html.RenderAction(). Vamos a agregar los dos en, así como algunas marcas adicionales para que se muestren, tal y como se muestra a continuación:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

Ahora cuando se ejecuta la aplicación, se verá el género en el área de navegación izquierdo y el resumen de carro en la parte superior.

## <a name="update-to-the-store-browse-page"></a>Actualizar a la página Examinar de almacén

La página Examinar de almacén es funcional, pero no presente un buen aspecto. Podemos actualizar la página para mostrar los álbumes de un mejor diseño actualizando el código de vista (que se encuentra en /Views/Store/Browse.cshtml) como se indica a continuación:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

Aquí estamos realizando empleo de Url.Action en lugar de Html.ActionLink por lo que podemos aplicar un formato especial al vínculo para incluir la ilustración del álbum.

*Nota: Estamos mostrando una portada del álbum genérico para estos álbumes. Esta información se almacena en la base de datos y se puede editar mediante el Administrador de almacén. Pueden agregar su propia ilustración.*

Ahora cuando se vaya a un género, veremos los álbumes que se muestra en una cuadrícula con la ilustración del álbum.

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a>Actualizar la página de inicio para mostrar la parte superior se venden álbumes

Deseamos ofrecer nuestro mayor venta álbumes en la página de inicio para aumentar las ventas. Para nuestro HomeController para ocuparse de esto y agregar en algunos gráficos adicionales también nos aseguraremos de hacer algunas actualizaciones.

En primer lugar, vamos a agregar una propiedad de navegación a nuestra clase álbum para que Entity Framework sepa que está asociados. Las últimas líneas de nuestro **álbum** clase debe tener el siguiente aspecto:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

*Nota: Será necesario agregar un uso de la instrucción que se va a poner en el espacio de nombres System.Collections.Generic.*

En primer lugar, vamos a agregar un campo de storeDB y la MvcMusicStore.Models mediante instrucciones, como en nuestros otros controladores. A continuación, agregaremos el método siguiente a HomeController que consulta nuestra base de datos para buscar los álbumes de ventas superiores según OrderDetails.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

Se trata de un método privado, ya que no queremos que esté disponible como una acción de controlador. Incluiremos en HomeController por motivos de simplicidad, pero se recomienda mover la lógica de negocios en clases de servicio independiente según corresponda.

Teniendo esto en su lugar, podemos actualizar la acción de controlador de índice para consultar las principales 5 venta álbumes y devolverlos a la vista.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

El código completo de HomeController actualizado es tal y como se muestra a continuación.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

Por último, necesitaremos actualizar la vista de índice de inicio para que pueda mostrar una lista de álbumes actualizando el tipo de modelo y agregar la lista de álbumes a la parte inferior. Tomaremos esta oportunidad para agregar un encabezado y una sección de promoción a la página.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

Ahora, cuando se ejecuta la aplicación, veremos nuestra página de principal actualizada con álbumes de ventas superiores y el mensaje de la promoción.

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a>Conclusión

Hemos visto que ASP.NET MVC facilita el proceso para crear un sitio Web sofisticado con acceso de base de datos de pertenencia, AJAX, etcetera. muy rápidamente. Espero que este tutorial le ha concedido las herramientas que necesita para empezar a compilar las aplicaciones de su propia ASP.NET MVC.


>[!div class="step-by-step"]
[Anterior](mvc-music-store-part-9.md)
