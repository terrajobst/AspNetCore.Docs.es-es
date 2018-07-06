---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Parte 4: Lista de productos | Microsoft Docs'
author: JoeStagner
description: Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks. Parte 4 cubre la lista de productos con el contr GridView...
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: ec349a2564a63fd950ca5f6a171e6ffd1199c28a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813080"
---
<a name="part-4-listing-products"></a>Parte 4: Lista de productos
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks demuestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET. Resalta cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluida la compra, la desprotección y la administración.
> 
> Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks. Parte 4 trata la lista de productos con el control GridView.


## <a id="_Toc260221670"></a>  Lista de productos con el Control GridView

Vamos a empezar a implementar nuestra página ProductsList.aspx haciendo clic en"secundario" en nuestra solución y seleccione "Agregar" y "Nuevo elemento".

![](tailspin-spyworks-part-4/_static/image1.jpg)

Elija "Uso de maestro de página de formularios Web" y escriba un nombre de la página de ProductsList.aspx".

Haga clic en "Agregar".

![](tailspin-spyworks-part-4/_static/image2.jpg)

A continuación, elija la carpeta "Estilos" donde se coloca la página Site.Master y selecciónelo en la ventana "Contenido de la carpeta".

![](tailspin-spyworks-part-4/_static/image3.jpg)

Haga clic en "Aceptar" para crear la página.

Nuestra base de datos se rellena con datos del producto tal como se muestra a continuación.

![](tailspin-spyworks-part-4/_static/image4.jpg)

Después de crear la página nuevo vamos a usar un origen de datos de entidad para acceder a los datos del producto, pero en esta instancia se debe seleccionar las entidades Product y necesitamos restringir los elementos que se devuelven a aquellos para la categoría seleccionada.

Para realizar esta acción se lo indicaremos EntityDataSource para generar automáticamente la cláusula WHERE y especificamos el WhereParameter.

Recordará que cuando creamos los elementos de menú en nuestra "Menu de categoría de producto" se compila dinámicamente el enlace agregando el CatagoryID a la cadena de consulta para cada vínculo. Se le indicará que el origen de datos de entidad para el parámetro WHERE se derivan de ese parámetro de cadena de consulta.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

A continuación, vamos a configurar el control ListView para mostrar una lista de productos. Para crear una experiencia óptima de la compra que se deberá compactar varias características concisas en cada producto individual que se muestran en nuestra ListVew.

- El nombre del producto será un vínculo a la vista de detalles del producto.
- Se mostrará el precio del producto.
- Se mostrará una imagen del producto y dinámicamente seleccionaremos la imagen desde un directorio de imágenes del catálogo en nuestra aplicación.
- Se incluirá un vínculo para agregar el producto específico al carro de la compra de inmediato.

Aquí está el marcado de nuestra instancia del control ListView.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

Estamos creando varios vínculos para cada producto muestra dinámicamente.

Además, antes de que probamos la propia página nueva debemos crear la estructura de directorios para el producto imágenes del catálogo como sigue.

![](tailspin-spyworks-part-4/_static/image1.png)

Una vez que nuestras imágenes de producto son accesibles podemos probar la página de lista del producto.

![](tailspin-spyworks-part-4/_static/image5.jpg)

En la página del sitio principal, haga clic en uno de los vínculos de categoría de lista.

![](tailspin-spyworks-part-4/_static/image6.jpg)

Ahora debemos implementar la página ProductDetials.apsx y la funcionalidad de AddToCart.

Use archivo -&gt;nuevo para crear un nombre de página ProductDetails.aspx mediante la página principal del sitio como hicimos anteriormente.

Nuevamente, usaremos un control EntityDataSource para acceder al registro de producto específico en la base de datos y se usará un control FormView de ASP.NET para mostrar los datos de productos como sigue.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

No se preocupe si el formato se ve algo divertido para usted. El marcado anterior deja espacio en el diseño de presentación para un par de características que se implementará más adelante.

El carro de la compra representará una lógica más compleja en nuestra aplicación. Para empezar, use archivo -&gt;nuevo para crear una página denominada MyShoppingCart.aspx.

Tenga en cuenta que no nos estamos elección del nombre ShoppingCart.aspx.

Nuestra base de datos contiene una tabla denominada "ShoppingCart". Cuando se genera un Entity Data Model se crea una clase para cada tabla en la base de datos. Por lo tanto, el Entity Data Model genera una clase de entidad denominada "ShoppingCart". Por lo que nos podríamos usar ese nombre para la implementación de carro de la compra o ampliarlo para nuestras necesidades, pero se elegirá en su lugar para simplemente seleccione un nombre que evitará el conflicto, se podríamos modificar el modelo.

También merece la pena tener en cuenta que se va a crear un carro de compra simple e insertar la lógica del carro de la compra con la presentación del carro de la compra. También podríamos optar por implementar nuestro carro de compra en una capa de negocio completamente independiente.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-3.md)
> [Siguiente](tailspin-spyworks-part-5.md)
