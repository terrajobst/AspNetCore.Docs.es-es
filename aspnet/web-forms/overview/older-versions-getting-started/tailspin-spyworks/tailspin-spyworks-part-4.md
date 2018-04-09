---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Parte 4: Lista de productos | Documentos de Microsoft'
author: JoeStagner
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Tailspin Spyworks. Parte 4 portadas de lista de productos con el contrat GridView....
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 69b26344e6dcdbf27e94da90ad5d6cd79f27ccd3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="part-4-listing-products"></a>Parte 4: Lista de productos
====================
por [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks muestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET. Muestra cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluidos los de la compra, la desprotección y la administración.
> 
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Tailspin Spyworks. Parte 4 cubre la lista de productos con el control GridView.


## <a id="_Toc260221670"></a>  Lista de productos con el Control GridView

Vamos a empezar a implementar nuestra página ProductsList.aspx "Haciendo clic" en nuestra solución y seleccione "Agregar" y "Nuevo elemento".

![](tailspin-spyworks-part-4/_static/image1.jpg)

Elija "Usando Master página de formularios Web" y escriba un nombre de página de ProductsList.aspx".

Haga clic en "Agregar".

![](tailspin-spyworks-part-4/_static/image2.jpg)

A continuación, elija la carpeta de "Estilos" donde se coloca la página Site.Master y selecciónelo en la ventana "Contenido de la carpeta".

![](tailspin-spyworks-part-4/_static/image3.jpg)

Haga clic en "Aceptar" para crear la página.

Nuestra base de datos se rellena con datos del producto tal como se muestra a continuación.

![](tailspin-spyworks-part-4/_static/image4.jpg)

Después de crea nuestra página de nuevo, vamos a usar un origen de datos de entidad para tener acceso a esos datos de producto, pero en este caso se debe seleccionar las entidades Product y necesitamos restringir los elementos que se devuelven a los de la categoría seleccionada.

Para lograr esto indicaremos EntityDataSource para generar automáticamente la cláusula WHERE y se especificará el WhereParameter.

Recordará que cuando se crean los elementos de menú en nuestro "menú de categoría de producto" se construidas dinámicamente el enlace agregando el CatagoryID a la cadena de consulta para cada vínculo. Se le indicará el origen de datos de entidad para el parámetro WHERE se deriva ese parámetro de cadena de consulta.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

A continuación, vamos a configurar el control ListView para mostrar una lista de productos. Para crear una experiencia óptima al compra se podrá compactar varias características concisas en cada producto individual que se muestra en nuestro ListVew.

- El nombre de producto será un vínculo a la vista de detalles del producto.
- Se mostrará el precio del producto.
- Se mostrará una imagen del producto y se dinámicamente seleccionará la imagen desde un directorio de imágenes del catálogo en nuestra aplicación.
- Se incluirá un vínculo para agregar el producto específico al carro de la compra de inmediato.

Este es el marcado para la instancia del control ListView.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

Estamos creando dinámicamente varios vínculos de cada producto mostrado.

Además, antes de que probamos propia página nueva es necesario crear la estructura de directorios para el producto imágenes del catálogo como sigue.

![](tailspin-spyworks-part-4/_static/image1.png)

Una vez que nuestras imágenes de producto son accesibles podemos probar nuestra página de lista de productos.

![](tailspin-spyworks-part-4/_static/image5.jpg)

En la página del sitio principal, haga clic en uno de los vínculos de la lista de categorías.

![](tailspin-spyworks-part-4/_static/image6.jpg)

Ahora es necesario implementar la página ProductDetials.apsx y la funcionalidad de AddToCart.

Use archivo -&gt;nuevo para crear un nombre de página ProductDetails.aspx mediante la página principal del sitio tal y como se hacía anteriormente.

Nuevo usaremos un control EntityDataSource para acceder al registro de producto específico en la base de datos y se usará un control FormView de ASP.NET para mostrar los datos de producto como se indica a continuación.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

No se preocupe si el formato tiene un aspecto un poco divertido para usted. El marcado deja espacio en el diseño de presentación para un par de características que implementaremos más adelante.

Shopping Cart representará una lógica más compleja en nuestra aplicación. Para empezar, use el archivo -&gt;nueva para crear una página denominada MyShoppingCart.aspx.

Tenga en cuenta que no elegimos el nombre ShoppingCart.aspx.

Nuestra base de datos contiene una tabla denominada "ShoppingCart". Cuando se genera un Entity Data Model se crea una clase para cada tabla de la base de datos. Por lo tanto, el Entity Data Model genera una clase de entidad denominada "ShoppingCart". Se puede editar el modelo para que se puede utilizar ese nombre para la implementación de carro de la compra o ampliarlo para nuestras necesidades, pero se le participar en su lugar, simplemente seleccione un nombre que evitará el conflicto.

También es necesario tener en cuenta que se va a crear un carro de la compra simple e incrustar la lógica de carro de la compra con la presentación de carro de la compra. También nos tengamos que optar por implementar nuestro carro de compra en una capa de negocio completamente independiente.

> [!div class="step-by-step"]
> [Anterior](tailspin-spyworks-part-3.md)
> [Siguiente](tailspin-spyworks-part-5.md)
